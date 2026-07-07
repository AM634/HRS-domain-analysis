# 演習6 レビュー結果

## レビュー対象

- docs/exercise6_maintenance.md
- diagrams/exercise6_usecase_delta.puml
- diagrams/exercise6_activity_cancel.puml
- diagrams/exercise6_collaboration_cancel.puml
- implementation/HRS_hotel_homepage_ui/hotel_homepage/js/application.js
- implementation/HRS_hotel_homepage_ui/hotel_homepage/js/presentation.js
- implementation/HRS_hotel_homepage_ui/hotel_homepage/index.html
- implementation/HRS_hotel_homepage_ui/hotel_homepage/css/style.css

## レビュー項目

| 項目 | 結果 |
|---|---|
| 新要求「予約キャンセル」が追加されている | OK |
| 予約済（RESERVED）の予約のみキャンセルできる | OK |
| チェックイン済みの予約はキャンセル不可 | OK |
| キャンセル後、空室検索で再予約可能になる | OK |
| キャンセル済み予約はチェックイン不可 | OK |
| CancelReservationControl が独立している | OK |
| 3層アーキテクチャが維持されている | OK |
| 差分ドキュメント・UML図が作成されている | OK |
| 工学的観点（ロバストネス、SOLID、3層）の考察がある | OK |

## 確認内容

### 1. 正常系

予約番号と正しい氏名を入力すると、予約状態が「キャンセル済」に更新され、キャンセル日時が表示されることを確認した。

### 2. 例外系

- 存在しない予約番号 → エラー表示
- 氏名不一致 → エラー表示
- チェックイン済み予約 → キャンセル不可エラー
- キャンセル済み予約の再キャンセル → エラー表示
- キャンセル済み予約のチェックイン → エラー表示

### 3. 空室への影響

キャンセル前に同一日程で空室がなかった場合でも、キャンセル後は同条件で予約可能になることを確認した。

## 修正点

特になし。
