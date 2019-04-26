---
title: 追加の watchOS 3 フレームワークの変更
description: このドキュメントでは、watchOS 3、および Xamarin で操作する方法で導入されたさまざまなフレームワークの変更について説明します。 Core Data、Core モーション、Foundation、HealthKit、HomeKit、PassKit、および UIKit がについて説明します。
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: e3eb4e3454aeab08d1333c5dbc3d4808fa4d676c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224514"
---
# <a name="additional-watchos-3-frameworks-changes"></a>追加の watchOS 3 フレームワークの変更

_この記事では、追加、マイナー変更や watchOS 3 用の既存のフレームワークの機能強化について説明します。_

IOS の大幅な変更を加え Apple は、watchOS 3 での変更と既存のフレームワークをいくつかの機能強化を行ったが。


## <a name="core-data"></a>Core Data

ウォッチ 3 の OS の Core Data framework に、次のように強化します。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトは、同時実行のエラーとシリアル化せずフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラスには、SQLite のデータ ストアのプールが管理されます。
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL ジャーナルのモードのサポート、新しいクエリの生成にデータ ストアを SQLite オブジェクト機能の管理オブジェクトのコンテキスト (MOC) を将来をフェッチするための特定のデータベース バージョンに固定でき、トランザクションのエラーが発生します。
- 高レベルを使用して`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)およびその他のコア データの構成リソース。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)します。


## <a name="core-motion"></a>コア アニメーション

Core モーション framework ウォッチ 3 の OS の次のように強化します。

- 新しいデバイス モーション イベントは、加速度計、ジャイロスコープなどがありますを使用して、モーション センサーと印刷の向きの更新プログラムを提供します。 この更新プログラム (最大 100 Hz の料金) でのアプリを登録できます。
- 新しい歩数計イベントは高速なユーザーを置いたときにリアルタイムで通知と実行を再開します。 使用して、 [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer)フォア グラウンドまたはバック グラウンドの歩数計イベントを登録します。


## <a name="foundation"></a>Foundation

Foundation framework ウォッチ OS 3 の次のように強化します。

- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)間隔を比較して、間隔の交差部分のテストのための期間などの日付と時刻の間隔の計算を行うクラス。
- いくつかの新しいプロパティが追加されて、 [NSLocal](https://developer.apple.com/reference/foundation/nslocale)ローカル情報と使用可能な表示形式を取得するクラス。
- 使用して、新しい[NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)間さまざまなユニットの測定 (UOM) を変換または異なる UOMs 内の値に対して計算を実行するクラス。
- 使用して、新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスは、エンドユーザーに表示するためのローカライズされた測定値の書式を設定します。
- 使用して、新しい[NSUnit](https://developer.apple.com/reference/foundation/nsunit)と[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)特定 UOMs を表すためのクラス。


## <a name="healthkit"></a>HealthKit

HealthKit framework ウォッチ OS 3 の次のように強化します。

- 使用して、新しい[HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration)クラスを指定する、`ActivityType`と`LocationType`トレーニングの。
- 新しい[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject)と`WheelchairUse`のメソッド、 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)手押し車を操作するためのクラスが追加されました正常性データに関連します。
- 天気の種類の新しいメタデータのキーが追加されています (など`HKWeatherConditionClear`と`HKWeatherConditionCloudy`) とトレーニングの種類 (など`HKWorkoutActivityTypeFlexibility`と`HKWorkoutActivityTypeWheelchairRunPace`) が追加されています。


## <a name="homekit"></a>HomeKit

Watch OS 3 の HomeKit フレームワークに、次のように強化します。

- 接続の IP を HomeKit の表示および操作する機能を追加するカメラ。
- いくつかの新しいサービスや特性を追加します。
- コンテキストと主要なサービスとリンク サービスのアクセサリの構成の詳細を追加します。


## <a name="passkit"></a>PassKit

PassKit framework ウォッチ OS 3 の次のように強化します。

- 物理的な商品およびサービスの両方の Apple Watch でセキュリティで保護されたアプリでの支払いをサポートするフレームワークを展開します。
- 次のクラスを使用できますようになりました。[PKPayment](https://developer.apple.com/reference/passkit/pkpayment)、 [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod)、 [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest)と[PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Watch OS 3 の UIKit フレームワークに、次のように強化します。

- ラベルで動的な型をサポートするためにテキスト フィールドとテキスト ボックスを使用して、新しい`PreferredFontForTextStyle`のメソッド、`UIFont`クラス。
- `ColorWithDisplayP3`メソッドは、色をサポートするために追加されました。


## <a name="related-links"></a>関連リンク

- [作業の開始 (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [WatchOS 3 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
