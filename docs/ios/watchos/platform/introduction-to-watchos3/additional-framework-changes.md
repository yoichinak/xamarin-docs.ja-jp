---
title: 追加 watchOS 3 フレームワークの変更
description: この記事では、追加、マイナーの変更または watchOS 3 の既存のフレームワークの機能強化について説明します。
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 572aaff9d010ec1ec1f48db2878859e445e2fb20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="additional-watchos-3-frameworks-changes"></a>追加 watchOS 3 フレームワークの変更

_この記事では、追加、マイナーの変更または watchOS 3 の既存のフレームワークの機能強化について説明します。_

IOS に主要な変更だけでなく Apple が行われた変更を行うと既存のフレームワークをいくつかの機能強化 watchOS 3 でします。


## <a name="core-data"></a>コア データ

ウォッチ OS 3 のコア データ フレームワークには、次の機能強化が加えします。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトには、同時実行エラーが発生とシリアル化せずフェッチがサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラス SQLite データ ストアのプールが保持されます。
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL ジャーナル モードのサポート、新しいクエリの生成に SQLite データ ストア オブジェクトの機能で管理されているオブジェクトのコンテキスト (MOC) ピン留めできます将来をフェッチするための特定のデータベース バージョンにし、。トランザクション エラーが発生します。
- 使用して、高度な`NSPersistenceContainer`参照に、 `NSPersistentStoreCoordinator`、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)およびその他のコア データの構成リソース。
- いくつかの新しい便利なメソッドに追加された`NSManagedObject`フェッチを実行し、サブクラスを作成しやすきます。

詳細については、Apple を参照してください[コア データ フレームワーク参照](https://developer.apple.com/reference/coredata)です。


## <a name="core-motion"></a>コア モーション

次の機能強化は、ウォッチ OS 3 のコア モーション フレームワークにいます。

- 新しい/device Motion イベントは、加速度計、ジャイロスコープを使用して、運動と印刷の向きの更新プログラムを提供します。 更新 (最大 100 Hz の速度) このアプリのアプリを登録できます。
- 新しい Pedometer イベントによって、高速、リアルタイムの通知ユーザーを置いたときに実行を再開します。 使用して、 [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer)フォア グラウンドまたはバック グラウンドの pedometer イベントを登録します。


## <a name="foundation"></a>Foundation

次の機能強化は、ウォッチ OS 3 の Foundation フレームワークにいます。

- 使用して、新しい[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)の間隔を比較して、間隔の交差部分のテストの期間などの日付と時刻の間隔の計算を作成するクラス。
- いくつかの新しいプロパティが追加されて、 [NSLocal](https://developer.apple.com/reference/foundation/nslocale)クラスをローカルの情報と使用可能な表示形式を取得します。
- 使用して、新しい[NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement)間でさまざまなユニットのメジャー (出荷単位) を変換するか、異なる UOMs 内の値に対して計算を実行するクラス。
- 使用して、新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスをエンドユーザーに表示するローカライズされた測定値の書式を設定します。
- 使用して、新しい[NSUnit](https://developer.apple.com/reference/foundation/nsunit)と[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)特定 UOMs を表すクラス。


## <a name="healthkit"></a>HealthKit

次の機能強化は、ウォッチ OS 3 の HealthKit フレームワークにいます。

- 使用して、新しい[HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration)クラスを指定する、`ActivityType`と`LocationType`トレーニングのです。
- 新しい[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject)と`WheelchairUse`のメソッド、 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)手押し車を操作するためのクラスが追加されましたに関連したヘルス データ。
- 天気の種類の新しいメタデータ キーが追加されました (など`HKWeatherConditionClear`と`HKWeatherConditionCloudy`) とトレーニング型 (など`HKWorkoutActivityTypeFlexibility`と`HKWorkoutActivityTypeWheelchairRunPace`) が追加されました。


## <a name="homekit"></a>HomeKit

次の機能強化は、ウォッチ OS 3 の HomeKit フレームワークにいます。

- IP を接続する HomeKit の表示および操作する機能を追加カメラ。
- いくつかの新しいサービスや特性を追加します。
- 複数のコンテキストと主要なサービスとサービスのリンクのアクセサリの構成を追加します。


## <a name="passkit"></a>PassKit

次の機能強化は、ウォッチ OS 3 の PassKit フレームワークにいます。

- 物理的な商品およびサービスの両方の Apple Watch でセキュリティで保護された、アプリ内の支払いをサポートするようフレームワークを拡張します。
- 次のクラスが使用できるようになりました: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment)、 [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod)、 [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest)と[PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

UIKit framework ウォッチ OS 3 の次のように強化します。

- ラベルの動的な型をサポートするためにテキスト フィールドとテキスト ボックスを使用して、新しい`PreferredFontForTextStyle`のメソッド、`UIFont`クラスです。
- `ColorWithDisplayP3`広色をサポートするためにメソッドが追加されました。


## <a name="related-links"></a>関連リンク

- [作業の開始 (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [WatchOS 3 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
