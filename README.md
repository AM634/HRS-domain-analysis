# HRS-domain-analysis

## 概要

このリポジトリは、ソフトウェア工学Aのチーム開発課題における  
演習1「HRSのドメイン分析」の成果物を管理するためのものである。

対象システムはホテル予約システム（HRS）である。

## 演習1：ドメイン分析

HRSの問題領域を分析し、主要な概念と概念間の関係をUMLクラス図として表現した。
また、具体的な状況をUMLオブジェクト図として表現し、クラス図の妥当性を確認した。

## 成果物

- [ドメイン分析説明](docs/exercise1_domain_analysis.md)
- [UMLクラス図 PlantUML](diagrams/class_diagram.puml)
- [UMLオブジェクト図 PlantUML](diagrams/object_diagram.puml)
- [レビュー結果](reviews/exercise1_review.md)

## 作成した主な図

### UMLクラス図

HRSにおける利用者、予約、部屋、ホテル受付係、宿泊、宿泊料、支払いなどの主要概念と、それらの関係を表した。

### UMLオブジェクト図

具体例として、山田太郎が予約番号012345で101号室を予約し、宿泊につながる状況を表した。

## レビュー

レビューでは、主要な概念が網羅されているか、関係が自然に読めるか、実装寄りの内容になっていないかを確認した。
その結果、予約と部屋の多重度、宿泊料と支払いの関係を明確にした。
---

## 演習2：要求分析

HRSに期待される重要な動作として、以下の3つのユースケースを特定した。

1. 空室を確認して部屋を予約する
2. 予約番号でチェックインする
3. 宿泊料を精算してチェックアウトする

これらについて、UMLユースケース図、ユースケース記述、アクティビティ図を作成した。

## 演習2の成果物

- [ユースケース図 PlantUML](diagrams/usecase_diagram.puml)
- [ユースケース記述](docs/exercise2_usecase_descriptions.md)
- [予約のアクティビティ図 PlantUML](diagrams/activity_reservation.puml)
- [チェックインのアクティビティ図 PlantUML](diagrams/activity_checkin.puml)
- [チェックアウトのアクティビティ図 PlantUML](diagrams/activity_checkout.puml)
- [演習2レビュー結果](reviews/exercise2_review.md)

## 演習2の工夫点

ユースケース名を「予約」「チェックイン」「チェックアウト」のような単語だけにせず、
「空室を確認して部屋を予約する」「予約番号でチェックインする」「宿泊料を精算してチェックアウトする」のように、利用者にとって意味のある動詞句にした。

また、基本系列だけでなく、空室がない場合、予約番号が存在しない場合、支払いが完了しない場合などの代替・例外系列も記述した。

---

## 演習3：システム分析

演習2で作成したユースケースをもとに、HRSのシステム分析を行った。

本演習では、以下の3つのユースケースを対象に、UMLコラボレーション図とシステム分析クラス図を作成した。

1. 空室を確認して部屋を予約する
2. 予約番号でチェックインする
3. 宿泊料を精算してチェックアウトする

## 演習3の成果物

- [システム分析説明](docs/exercise3_system_analysis.md)
- [予約コラボレーション図 PlantUML](diagrams/collaboration_reservation.puml)
- [チェックインコラボレーション図 PlantUML](diagrams/collaboration_checkin.puml)
- [チェックアウトコラボレーション図 PlantUML](diagrams/collaboration_checkout.puml)
- [システム分析クラス図 PlantUML](diagrams/analysis_class_diagram.puml)
- [演習3レビュー結果](reviews/exercise3_review.md)

## 演習3の工夫点

バウンダリ、コントロール、エンティティを区別して分析した。

予約画面、チェックイン画面、チェックアウト画面をバウンダリクラスとし、利用者やホテル受付係とHRSのやり取りを表した。

また、予約処理、チェックイン処理、チェックアウト処理をコントロールクラスとして分けることで、各ユースケースの処理の責任を明確にした。
