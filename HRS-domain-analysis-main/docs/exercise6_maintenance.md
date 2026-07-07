# 演習6：保守（予約キャンセル機能の追加）

## 1. 目的

HRSに新しい要求「顧客が予約をキャンセルする」を追加し、要求分析・設計・実装を拡張する。
演習1〜5の成果物との差分（デルタ）として整理し、拡張が既存設計に与える影響を考察する。

## 2. 追加した要求

### ユースケース：予約番号と氏名で予約をキャンセルする

- **アクタ**: 利用者（顧客）
- **事前条件**: 予約が存在し、状態が「予約済（RESERVED）」である
- **事後条件**: 予約の状態が「キャンセル済（CANCELLED）」に更新され、該当日程の空室枠が解放される
- **基本系列**:
  1. 利用者は予約番号と予約者氏名（姓・名）を入力する
  2. HRSは予約を検索し、氏名が一致するか確認する
  3. HRSは予約状態をキャンセル済に更新する
  4. HRSはキャンセル完了を利用者に通知する
- **代替・例外系列**:
  - 予約番号が存在しない → エラー表示
  - 氏名が一致しない → エラー表示
  - すでにキャンセル済み → エラー表示
  - チェックイン済み・チェックアウト済み → キャンセル不可

## 3. ドメイン分析への影響（差分）

演習1のクラス図において、**予約（Reservation）** の状態に `CANCELLED` を追加した。
予約は作成後、以下のいずれかの状態を持つ。

| 状態 | 意味 |
|---|---|
| RESERVED | 予約済（キャンセル可能） |
| CHECKED_IN | チェックイン済（キャンセル不可） |
| CHECKED_OUT | チェックアウト済 |
| CANCELLED | キャンセル済 |

キャンセル日時 `cancelledAt` を予約に追加し、いつキャンセルされたかを記録する。

## 4. 設計モデルへの影響（差分）

### 4.1 ユースケース図

[演習6 ユースケース図（差分）](../diagrams/exercise6_usecase_delta.puml)

利用者アクタから新ユースケース「予約番号と氏名で予約をキャンセルする」を追加した。

### 4.2 アクティビティ図

[予約キャンセル アクティビティ図](../diagrams/exercise6_activity_cancel.puml)

### 4.3 コラボレーション図

[予約キャンセル コラボレーション図](../diagrams/exercise6_collaboration_cancel.puml)

BCEモデルに従い、以下を追加した。

| 役割 | 追加内容 |
|---|---|
| Boundary | キャンセル画面（cancel ページ） |
| Control | CancelReservationControl |
| Entity | Reservation（状態 CANCELLED） |

### 4.4 既存クラスへの変更

| クラス | 変更内容 |
|---|---|
| ReservationManager | `cancel()` メソッド追加、空室判定で CANCELLED を除外 |
| CheckInRoomControl | CANCELLED 状態の予約をチェックイン不可に |
| ReservationManager.availableRooms | CANCELLED 予約を空室枠から除外 |

## 5. 実装（差分）

実装プログラム: [HRS Grand Hotel Web UI](../implementation/HRS_hotel_homepage_ui)

| ファイル | 変更内容 |
|---|---|
| js/application.js | CancelReservationControl、ReservationManager.cancel() |
| js/presentation.js | キャンセル画面・イベント処理、一覧からのキャンセル導線 |
| index.html | キャンセルページ、ナビゲーション追加 |
| css/style.css | CANCELLED バッジスタイル |

### 操作の流れ

1. 「キャンセル」タブ、または予約照会画面からキャンセルページへ移動
2. 予約番号・姓・名を入力してキャンセル実行
3. 予約一覧で状態が「キャンセル済」に変わる
4. キャンセルされた日程は再予約可能になる

## 6. 工学的観点からの影響考察

### 6.1 ロバストネス分析

| 境界オブジェクト | 追加 | 影響 |
|---|---|---|
| キャンセル画面 | 新規 | 利用者入力（予約番号・氏名）を受け取る |
| 予約照会画面 | 変更 | キャンセル可能な予約にキャンセル導線を追加 |

| コントロール | 追加 | 影響 |
|---|---|---|
| CancelReservationControl | 新規 | キャンセル処理の責任を独立させた |
| CheckInRoomControl | 変更 | キャンセル済予約のチェックインを拒否 |
| ReservationManager | 変更 | 空室判定・キャンセル処理を担当 |

| エンティティ | 変更 | 影響 |
|---|---|---|
| Reservation | 状態追加 | CANCELLED 状態と cancelledAt を追加 |

新機能追加時も、入力（Boundary）→ 処理（Control）→ データ（Entity）の流れを維持できた。

### 6.2 設計原則（SOLID）

- **単一責任の原則（SRP）**: キャンセル処理を `CancelReservationControl` として独立させ、予約・チェックイン・チェックアウト各処理と責任を分離した
- **開放閉鎖の原則（OCP）**: 既存の `ReserveRoomControl` 等を変更せず、新しい Control クラスを追加することで機能拡張した
- **依存性逆転の原則（DIP）**: presentation 層は `CancelReservationControl` 経由で処理を呼び出し、domain/datasource の詳細に直接依存しない

### 6.3 アーキテクチャパターン（3層アーキテクチャ）

演習4で採用した3層構造を維持したまま拡張できた。

```
presentation.js  →  CancelReservationControl  →  ReservationManager  →  domain.js / datasource.js
```

- **UI層の変更**: キャンセルフォーム追加（他画面への影響は最小）
- **アプリケーション層の変更**: 新 Control + 既存 Manager の拡張
- **ドメイン層の変更**: 状態値の追加のみ（新クラス不要）

この結果、**保守性**（変更箇所の局所化）と **変更容易性**（新 Control の追加）が確保された。

### 6.4 保守作業の教訓

1. 状態遷移を持つドメイン（予約）では、新状態追加時に**すべての状態判定箇所**（空室検索、チェックイン等）への影響確認が必要
2. BCE + 3層アーキテクチャにより、新ユースケース追加が既存コードへの波及を抑えられた
3. 差分のみのドキュメント化により、演習1〜5の成果物を維持しつつ拡張履歴を追跡できる
