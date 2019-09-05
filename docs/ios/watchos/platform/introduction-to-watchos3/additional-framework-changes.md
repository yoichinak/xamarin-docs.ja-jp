---
title: 追加の watchOS 3 フレームワークの変更
description: このドキュメントでは、watchOS 3 で導入されたさまざまなフレームワークの変更と、Xamarin でそれらを使用する方法について説明します。 コアデータ、コアモーション、Foundation、HealthKit、ホームキット、Pass Kit、および UIKit について説明します。
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 34f192938ac583e39232312377142015aa6d3811
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287564"
---
# <a name="additional-watchos-3-frameworks-changes"></a>追加の watchOS 3 フレームワークの変更

_この記事では、watchOS 3 の既存のフレームワークに対する追加、軽微な変更、または強化された機能について説明します。_

Apple は、iOS に加えられた主な変更に加えて、watchOS 3 の既存のいくつかのフレームワークに対して変更や機能強化を加えました。


## <a name="core-data"></a>コアデータ

Watch OS 3 のコアデータフレームワークには、次の機能強化が行われています。

- ルート[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトは、シリアル化を使用しない同時エラーおよびフェッチをサポートしています。
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)クラスは、SQLite データストアのプールを保持します。
- WAL ジャーナルモードで SQLite データストアを使用する[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)オブジェクトでは、新しいクエリ生成機能がサポートされています。この機能を使用すると、マネージオブジェクトコンテキスト (MOC) を特定のデータベースバージョンに固定して、将来のフェッチやエラー処理を行うことができます。
- 高レベル`NSPersistenceContainer`を使用して`NSPersistentStoreCoordinator`、、 [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 、その他のコアデータ構成リソースを参照します。
- フェッチの実行とサブクラスの作成`NSManagedObject`を容易にするために、いくつかの新しい便利なメソッドが追加されました。

詳細については、Apple の[Core Data Framework リファレンス](https://developer.apple.com/reference/coredata)を参照してください。


## <a name="core-motion"></a>コアモーション

Watch OS 3 の主要なモーションフレームワークには、次の機能強化が行われています。

- 新しいデバイスのモーションイベントは、加速度計とジャイロスコープを使用して、動きと向きの更新を提供します。 アプリはこの更新プログラムに登録できます (最大 100Hz)。
- 新しい Pedometer イベントを使用すると、ユーザーが一時停止して実行を再開したときに、迅速なリアルタイムの通知を実行できます。 [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer)を使用して、フォアグラウンドまたはバックグラウンドの pedometer イベントに登録します。


## <a name="foundation"></a>Team

Watch OS 3 の Foundation framework には、次の機能強化が加えられています。

- 新しい[Nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval)クラスを使用して、間隔の比較と間隔の交差部分のテストを行うために、期間などの日付と時間の間隔を計算します。
- ローカル情報と使用可能な表示形式を取得するために、いくつかの新しいプロパティが[nslocal](https://developer.apple.com/reference/foundation/nslocale)クラスに追加されました。
- 新しい [Nsmeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) クラスを使用して異なる測定単位 (UOM) 間で変換するか、異なる uoms の値に対して計算を実行します。
- 新しい[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)クラスを使用して、ローカライズされた測定形式をエンドユーザーに表示するように書式設定します。
- 特定の UOMs を表すために、新しい[Nsunit](https://developer.apple.com/reference/foundation/nsunit)クラスと[nsunit](https://developer.apple.com/reference/foundation/nsdimension)クラスを使用します。


## <a name="healthkit"></a>HealthKit

Watch OS 3 の HealthKit framework には、次の機能強化が行われています。

- 新しい[hkworkspace outconfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration)クラスを使用して、 `ActivityType`トレーニング`LocationType`のおよびを指定します。
- 車椅子に関連する正常`WheelchairUse`性データを操作するために、新しい[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject)と[HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)クラスのメソッドが追加されました。
- 気象の種類`HKWeatherConditionClear` (や`HKWeatherConditionCloudy`など) に新しいメタデータキーが追加され、 `HKWorkoutActivityTypeFlexibility`トレーニングの種類 ( `HKWorkoutActivityTypeWheelchairRunPace`やなど) が追加されました。


## <a name="homekit"></a>HomeKit

Watch OS 3 のホームキットフレームワークには、次の機能強化が行われています。

- ホームキットの接続された IP カメラを表示および操作する機能が追加されました。
- 新しいサービスと特性がいくつか追加されました。
- 主要なサービスとリンクサービスのアクセサリのコンテキストと構成が追加されました。


## <a name="passkit"></a>PassKit

Watch OS 3 の Pass Kit フレームワークには、次の機能強化が行われています。

- フレームワークを拡張して、物理的な商品とサービスの両方の Apple Watch における、セキュリティで保護されたアプリ内支払いをサポートします。
- 次のクラスが使用できるようになりました。[Pkpayment](https://developer.apple.com/reference/passkit/pkpayment)、 [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod)、 [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) 、 [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Watch OS 3 の UIKit フレームワークには、次の機能強化が行われています。

- ラベルの動的な型をサポートするために、テキストフィールドとテキスト`PreferredFontForTextStyle`ボックスは`UIFont`クラスの新しいメソッドを使用します。
- この`ColorWithDisplayP3`メソッドは、ワイド色をサポートするために追加されました。


## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS%20watchos)
- [WatchOS 3 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
