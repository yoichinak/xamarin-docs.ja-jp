---
title: Xamarin.iOS で HealthKit
description: このドキュメントでは、HealthKit、正常性関連の情報を一元的な調整、およびセキュリティで保護されたデータ ストアを提供する iOS 8 で導入されたフレームワークについて説明します。 これは、HealthKit framework を使用するコードを記述する方法と HealthKit アプリをプロビジョニングする方法について説明します。
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 8dcb478b303c4c73f7e73dc018ad56b1301389c8
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830042"
---
# <a name="healthkit-in-xamarinios"></a>Xamarin.iOS で HealthKit

正常性のキットでは、正常性関連情報については、ユーザーのセキュリティで保護されたデータ ストアを提供します。 キット アプリの正常性は、ユーザーの明示的なアクセス許可を持つ読み取りおよび書き込みこのデータストアにおよび関連データが追加されたときに通知を受信します。 アプリがデータを表示またはユーザーは、Apple の指定された正常性のアプリを使用して、すべてのデータのダッシュ ボードを表示します。

正常性関連データがあるためので機密性の高いや、重要な正常性のキットが厳密に型指定されたメジャーと明示的な関連付け (たとえば、血中血糖または心拍数など) に記録される情報の種類の単位で。 さらに、正常性のキット アプリは明示的な権利を使用する必要があります、する必要がありますとユーザーの特定の種類の情報にアクセスを要求する必要があります明示的に、アプリへのアクセス許可データの種類。

この記事を紹介します。

- 正常性キットのセキュリティ要件、アプリケーションのプロビジョニングと正常性のキット データベースにアクセスする権限をユーザーの要求を含む
- 正常性キットの型システムは、不適切に適用するか、データを解釈する可能性を最小限に抑える
- 共有のシステム全体の正常性のキット データストアへの書き込み。

この記事では、データベース クエリを実行、測定単位の間で変換する、新しいデータの通知の受信などのより高度なトピックは説明しません。

この記事でから作成していきますユーザーの心拍数を記録するサンプル アプリケーション。

[![](healthkit-images/image01.png "ユーザーの心拍数を記録するサンプル アプリケーション")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>必要条件

次がこの記事で紹介する手順を完了する必要です。

- **Xcode 7 および iOS 8 (またはそれ以上)** – Apple の最新の Xcode と iOS Api がインストールされ、開発者のコンピューターに構成する必要があります。
- **Visual Studio for Mac または Visual Studio** – Visual Studio for Mac の最新バージョンをインストールして、開発者のコンピューターで構成されている必要があります。
- **iOS 8 (またはそれ以上) のデバイス**– 8 以上をテストするために、iOS の最新バージョンを実行している iOS デバイス。

> [!IMPORTANT]
> 正常性のキットは、iOS 8 で導入されました。 現在、正常性のキットは、iOS シミュレーターで使用できません、デバッグには、物理 iOS デバイスへの接続が必要です。




## <a name="creating-and-provisioning-a-health-kit-app"></a>作成して正常性のキット アプリのプロビジョニング
Xamarin iOS 8 のアプリケーションでは、HealthKit API を使用できます、前に、正しく構成およびプロビジョニングする必要があります。 このセクションでは、Xamarin アプリケーションを設定するために必要な手順を説明します。

正常性のキット アプリが必要です。

- 明示的な**アプリ ID**します。
- A**プロビジョニング プロファイル**明示的なと関連付けられた**アプリ ID**と**ヘルス キット**アクセス許可。
- `Entitlements.plist`で、`com.apple.developer.healthkit`型のプロパティ`Boolean`設定`Yes`します。
- `Info.plist`が`UIRequiredDeviceCapabilities`キーには持つエントリが含まれています、`String`値`healthkit`します。
- `Info.plist`適切なプライバシーに関する説明のエントリも必要があります。`String`キーの説明`NSHealthUpdateUsageDescription`、アプリがデータを記述する場合、`String`キーの説明`NSHealthShareUsageDescription`アプリが正常性のキットを読み取る場合データ。

IOS アプリのプロビジョニングについての詳細を確認する、 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)で Xamarin の記事**Getting Started**シリーズは、開発者の証明書、アプリの Id の間のリレーションシップを説明します。プロファイル、およびアプリの権利をプロビジョニングします。

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>明示的なアプリ ID とプロビジョニング プロファイル

明示的な作成**アプリ ID**と適切な**プロビジョニング プロファイル**Apple ので行われる[iOS デベロッパー センター](https://developer.apple.com/devcenter/ios/index.action)します。 

現在**アプリ Id**に記載されて、[証明書, Identifiers & Profiles](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action)デベロッパー センターの「します。 多くの場合、この一覧が表示されます**ID**の値`*`かを示すを**アプリ ID** - **名前**サフィックスの任意の数で使用できます。 このような*ワイルドカード アプリ Id*正常性のキットでは使用できません。
 
明示的な作成**アプリ ID**、 をクリックして、 **+** する右のボタン、 **iOS アプリ ID を登録**ページ。


[![](healthkit-images/image02.png "Apple の開発者ポータルでアプリの登録")](healthkit-images/image02.png#lightbox)

上の図に示すように、アプリの説明を作成した後を使用して、**明示的なアプリ ID**セクションに、アプリケーションの ID を作成します。 **App Services**  セクションで、チェック**ヘルス キット**で、**サービスの有効化**セクション。

完了したら、キーを押して、**続行**を登録する ボタン、**アプリ ID**アカウント。 戻さするれましたが、**証明書、識別子、およびプロファイル**ページ。 をクリックして**Provisioning Profiles**をすると、現在のプロビジョニング プロファイルの一覧に移動し、をクリックして、 **+** する右上隅のボタン、 **Add iOSプロビジョニング プロファイル**ページ。 選択、 **iOS App Development**オプションを選択し、をクリックして**続行**を取得する、**アプリ ID の選択**ページ。 ここでは、明示的な選択**アプリ ID**以前に指定します。


[![](healthkit-images/image03.png "明示的なアプリ ID を選択します。")](healthkit-images/image03.png#lightbox)

をクリックして**続行**と作業時間を指定する場所、残りの画面を**開発者の証明書**、**デバイス**、および**名前**この**プロビジョニング プロファイル**:

[![](healthkit-images/image04.png "プロビジョニング プロファイルを生成します。")](healthkit-images/image04.png#lightbox)

クリックして**生成**し、プロファイルの作成を待機します。 ファイルをダウンロードし、ダブルクリックして Xcode でインストールします。 インストールのことを確認できます**Xcode > 設定 > アカウント > の詳細を表示しています.** だけがインストールされているプロビジョニング プロファイルが表示され、正常性キットおよびその他の特別なサービスでのアイコンがあります、**権利**行。

[![](healthkit-images/image05.png "Xcode でのプロファイルの表示")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>アプリ ID を関連付けると、プロビジョニングで Xamarin.iOS アプリのプロファイル

作成し、適切なインストール後**プロビジョニング プロファイル**ようが、通常は Visual studio for Mac または Visual Studio ソリューションを作成する時間。 正常性のキット アクセスのすべての Io にはC#またはF#プロジェクト。

Xamarin iOS 8 プロジェクトを手動で作成するプロセスを説明するのではなく、この記事 (を事前構築済みのストーリー ボードとコードを含む) に接続されているサンプル アプリを開きます。 サンプル アプリを有効になっている、正常性のキットに関連付ける**プロビジョニング プロファイル**の**Solution Pad**プロジェクトを右クリックし、起動、その**オプション**ダイアログ。 切り替えて、 **iOS アプリケーション**パネルし、明示的な入力**アプリ ID** 、アプリの前に作成した**バンドル識別子**:

[![](healthkit-images/image06.png "明示的なアプリ ID を入力します。")](healthkit-images/image06.png#lightbox)

今すぐに切り替え、 **iOS バンドル署名** パネルです。 最近インストールされた**プロビジョニング プロファイル**、明示的に**アプリ ID**、として使用可能、**プロビジョニング プロファイル**:

[![](healthkit-images/image07.png "プロビジョニング プロファイルを選択します。")](healthkit-images/image07.png#lightbox)

場合、**プロビジョニング プロファイル**が使用できない再確認、**バンドル識別子**で、 **iOS アプリケーション**パネルで指定されていると、 **iOSデベロッパー センター**ことと、**プロビジョニング プロファイル**がインストールされている (**Xcode > 設定 > アカウント >... 詳細を表示します。** ).

ときに正常性のキットが有効な**プロビジョニング プロファイル**が選択されていること、 **ok**プロジェクト オプション ダイアログを閉じます。

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist と Info.plist 値

サンプル アプリでは、`Entitlements.plist`ファイル (必要な正常性のキットには、アプリが有効になっている)、すべてのプロジェクト テンプレートに含まれていません。 プロジェクトに権利が含まれていない場合、プロジェクトを右クリックし、選択**ファイル > 新しいファイル... > iOS > Entitlements.plist**を手動で追加します。

最終的には、`Entitlements.plist`次のキーと値のペアが必要があります。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

同様に、`Info.plist`はの値がある必要とアプリ`healthkit`に関連付けられている、`UIRequiredDeviceCapabilities`キー。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

この記事に付属するサンプル アプリケーションが含まれていますが、構成済み`Entitlements.plist`を含むすべての必要なキー。

<a name="programming" />

## <a name="programming-health-kit"></a>プログラミングの正常性のキット

正常性のキット データ ストアとは、アプリ間で共有される、ユーザー固有のプライベート データストアです。 正常性に関する情報は機密性の高いためであるため、ユーザーは、データ アクセスを許可する正の手順を実行する必要があります。 このアクセスは、部分的な可能性があります (読み取り、一部の種類のデータが、アクセスではありませんが、書き込みなど) と、いつでも取り消すことがあります。 正常性キットのアプリケーションは、多くのユーザーが、正常性に関する情報を格納するためらわれます理解したうえで防御のため、作成する必要があります。

キット データの正常性に限定されます Apple の種類を指定します。 これらの型が厳密に定義されている: など、一部は、(グラム、カロリー、リットルなど) の測定単位の絶対値を組み合わせる他のユーザーの Apple が提供される列挙体は、特定の値に制限されます。 互換性のある測定単位を共有するデータも含め、によって区別、 `HKObjectType`; たとえば、型システムを格納する、誤った試行をキャッチする、`HKQuantityTypeIdentifier.NumberOfTimesFallen`フィールド予測するように値を`HKQuantityTypeIdentifier.FlightsClimbed`場合でも、両方を使用して、 `HKUnit.Count`測定単位。

正常性のキット データ ストアに保存の種類は、すべてのサブクラスの`HKObjectType`します。 `HKCharacteristicType` オブジェクトは、生物学上の性別、血中の種類、および生年月日を格納します。 一般的なただしは`HKSampleType`は特定の時刻または期間にわたって、サンプリング データを表すオブジェクト。 

[![](healthkit-images/image08.png "HKSampleType オブジェクト グラフ")](healthkit-images/image08.png#lightbox)

`HKSampleType` 抽象であり、4 つの具象サブクラスです。 現在の 1 つだけの型がある`HKCategoryType`分析のスリープ状態であるデータ。 正常性のキット内のデータの大多数は型`HKQuantityType`でデータを保存および`HKQuantitySample`オブジェクトで、使い慣れたファクトリ デザイン パターンを使用して作成されます。

[![](healthkit-images/image09.png "正常性のキット内のデータの大多数は型 HKQuantityType と HKQuantitySample オブジェクトにデータを格納")](healthkit-images/image09.png#lightbox)

`HKQuantityType` 型の範囲から`HKQuantityTypeIdentifier.ActiveEnergyBurned`に`HKQuantityTypeIdentifier.StepCount`します。 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>ユーザーからのアクセス許可を要求します。

エンドユーザーは、キットの正常性データを読み書きするアプリを許可する正の手順を実行する必要があります。 これは、iOS 8 デバイスに事前にインストールされる正常性のアプリを使用して実行されます。 正常性のキット アプリを実行すると、最初に、ユーザーはシステム管理と表示されます。**ヘルス アクセス**ダイアログ。

[![](healthkit-images/image10.png "システム管理の対象の正常性のアクセス ダイアログで、ユーザーが表示されます。")](healthkit-images/image10.png#lightbox)

後で、ユーザーがアプリの正常性を使用してアクセス許可を変更できる**ソース**ダイアログ。

[![](healthkit-images/image11.png "ユーザーがアプリのソース ダイアログの正常性を使用してアクセス許可を変更できます。")](healthkit-images/image11.png#lightbox)

正常性に関する情報がきわめて機密性の高いため、アプリ開発者書き込む必要があります、プログラム防御のため、アクセス許可を拒否して、アプリの実行中に変更ことを見込んでします。 最も一般的な表現形式でのアクセス許可を要求する、`UIApplicationDelegate.OnActivated`メソッドし、適切なユーザー インターフェイスを変更します。

### <a name="permissions-walkthrough"></a>アクセス許可のチュートリアル

正常性のキット プロビジョニング プロジェクトで、開く、`AppDelegate.cs`ファイル。 使用して、ステートメントに注目してください。 `HealthKit`; ファイルの上部にあります。


次のコードに関連する正常性のキットのアクセス許可。

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

これらのメソッドでコードをすべてがインラインで行う`OnActivated`が、サンプル アプリの個別のメソッドを使用して意思を明確にする:`ValidateAuthorization()`は特定の書き込まれている型 (および読み取りをアプリに必要な場合) にアクセスを要求するために必要な手順があります`ReactToHealthCarePermissions()` Health.app、アクセス許可ダイアログ ボックスで、ユーザーが操作した後にアクティブ化されるコールバックです。

ジョブの`ValidateAuthorization()`のセットを作成するのには、`HKObjectTypes`アプリは記述し、そのデータを更新するための承認を要求します。 サンプル アプリで、`HKObjectType`キーは`KHQuantityTypeIdentifierKey.HeartRate`します。 この型は、セットに追加`typesToWrite`、セットの中に`typesToRead`空のままにします。 これらのセットとへの参照、`ReactToHealthCarePermissions()`にコールバックが渡される`HKHealthStore.RequestAuthorizationToShare()`します。

`ReactToHealthCarePermissions()`ユーザーのアクセス許可ダイアログ ボックスの操作をすると 2 つの情報が渡されるコールバックが呼び出されます、`bool`値となる`true`場合、ユーザーの操作のアクセス許可ダイアログ ボックスと、を`NSError`。、null 以外の場合を示しますある種のエラーに関連付けられたアクセス許可 ダイアログを表示します。

> [!IMPORTANT]
> この関数の引数を明確にする:_成功_と_エラー_パラメーターは、ユーザーが正常性のキット データにアクセスするためのアクセス許可を与えるかどうかを示していません。 これらは、のみ、データへのアクセスを許可するように営業案件が、ユーザーに付与されていることを示します。

アプリが、データへのアクセスを持つかどうかを確認する、`HKHealthStore.GetAuthorizationStatus()`を渡して使用`HKQuantityTypeIdentifierKey.HeartRate`します。 返される状態に基づき、アプリを有効またはデータを入力する機能を無効にします。 多くの使用可能なオプションがあるし、アクセスの拒否攻撃に対処するための標準的なユーザー エクスペリエンスはありません。 例のアプリで、状態が に設定されている、`HeartRateModel`シングルトン オブジェクトをさらに、関連するイベントを発生させます。

## <a name="model-view-and-controller"></a>モデル、ビュー、およびコント ローラー

確認する、`HeartRateModel`シングルトン オブジェクトを開いて、`HeartRateModel.cs`ファイル。

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

最初のセクションでは、一般的なイベントとハンドラーを作成するための定型コードです。 最初の部分、`HeartRateModel`クラスはスレッド セーフである単一オブジェクトを作成するための定型コードもです。

次に、 `HeartRateModel` 3 つのイベントを公開します。 

- `EnabledChanged` -心拍数のストレージが有効または無効にされていることを示します (記憶域が最初に無効になっていることに注意してください)。 
- `ErrorMessageChanged` このサンプル アプリの非常に単純なエラー処理モデルがある: 最後のエラーを含む文字列。 
- `HeartRateStored` -心拍数が、正常性のキット データベースに格納されているときに発生します。

なお、これらのイベントが発生したときに経由では、その`NSObject.InvokeOnMainThread()`サブスクライバーが UI を更新することができます。 または、イベントは、バック グラウンド スレッドで発生しているとして文書化する可能性があり、互換性を確保する責任は、それぞれのハンドラーに残る可能性があります。 スレッドに関する考慮事項は、アクセス許可の要求などの関数の多くは非同期であり main 以外のスレッドでコールバックを実行するためにアプリケーションの正常性のキットで重要です。

Heath キット固有のコード`HeartRateModel`2 つの関数では、`HeartRateInBeatsPerMinute()`と`StoreHeartRate()`します。 

`HeartRateInBeatsPerMinute()` 厳密に型指定された正常性キットへの引数に変換します`HKQuantity`します。 数量の型がで指定されている、`HKQuantityTypeIdentifierKey.HeartRate`と数量の単位は`HKUnit.Count`で割った値`HKUnit.Minute`(単位は、つまり、*分あたりのビート*)。 

`StoreHeartRate()`関数は、 `HKQuantity` (、サンプル アプリでは、1 つ作成`HeartRateInBeatsPerMinute()`)。 使用してそのデータを検証する、`HKQuantity.IsCompatible()`を返すメソッド`true`オブジェクトのユニットは、引数の単位に変換できる場合。 数量が作成された場合`HeartRateInBeatsPerMinute()`が明らかに返されます`true`もが返されますが、`true`として、たとえば、数量が作成された場合*時間あたりのビート*。 一般的には、`HKQuantity.IsCompatible()`質量などを検証するために使用できる、距離とエネルギーが、ユーザーまたはデバイスが入力または (インペリアル法) などの測定値の 1 つのシステムで表示 (メートル単位) などの別のシステムに格納される可能性があります。 

数量の互換性が検証されると、`HKQuantitySample.FromType()`厳密に型を作成するファクトリ メソッドが使用される`heartRateSample`オブジェクト。 `HKSample` オブジェクトは開始と終了日が設定されます。瞬時の測定値のこれらの値と同じであるの例では。 サンプルでもが設定されていない任意のキー/値データその`HKMetadata`引数が 1 つは、センサーの場所を指定する次のコードなどのコードを使用できます。

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

1 回、`heartRateSample`が作成されると、コードを作成、データベースへの新しい接続を使用してブロックします。 そのブロック内で、`HKHealthStore.SaveObject()`メソッドは、データベースへの非同期書き込みを試みます。 ラムダ式の結果として得られる呼び出しに関連するイベントは、トリガーか`HeartRateStored`または`ErrorMessageChanged`します。

これで、モデルをプログラミングすると、コント ローラーで、モデルの状態を反映する方法を確認する時間を勧めします。 開き、`HKWorkViewController.cs`ファイル。 コンス トラクターは単につながり、`HeartRateModel`単一イベント処理メソッドを (インライン ラムダ式で実行できるが、個別のメソッドを指定する、目的は、もう少し明確)。

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

関連するハンドラーを次に示します。

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

当然ながら、アプリケーションが単一のコント ローラーでは、個別のモデル オブジェクトの作成と、制御フローのイベントの使用を回避することは不可能がモデル オブジェクトの使用は実際のアプリケーションに適しています。

## <a name="running-the-sample-app"></a>サンプル アプリを実行

IOS シミュレーターは、キットの正常性をサポートしていません。 IOS 8 を実行している物理デバイスでは、デバッグを行う必要があります。

システムに適切にプロビジョニング iOS 8 開発用のデバイスをアタッチします。 Mac 用の Visual Studio でのデプロイ ターゲットとして選択し、メニューを選択**実行 > デバッグ**します。

> [!IMPORTANT]
> プロビジョニングに関連のミスは、この時点で提示します。 エラーをトラブルシューティングするには、作成とプロビジョニング前キット アプリの正常性のセクションを確認します。 コンポーネントは次のとおりです。 
>
> - **iOS デベロッパー センター** -明示的なアプリ ID と正常性のキットは、プロビジョニング プロファイルを有効にします。 
> - **プロジェクト オプション**-バンドル Id (明示的なアプリ ID) とプロビジョニング プロファイル。
> - **ソース コード**-Entitlements.plist & Info.plist

プロビジョニングが正しく設定されていることと仮定すると、アプリケーションが開始されます。 達したとき、`OnActivated`メソッド、正常性のキットの承認に要求します。 最初に、オペレーティング システムによってこれが発生した次のダイアログ ボックスで、ユーザーが表示されます。


[![](healthkit-images/image12.png "このダイアログ ボックスで、ユーザーに表示されます。")](healthkit-images/image12.png#lightbox)

心拍数のデータを更新するアプリを有効にして、アプリが表示されます。 `ReactToHealthCarePermissions`コールバックは非同期的にアクティブになります。 これにより、 `HeartRateModel’s` `Enabled`が発生するプロパティを変更する、`EnabledChanged`イベントは、これにより、`HKPermissionsViewController.OnEnabledChanged()`イベント ハンドラーを実行するを有効に、`StoreData`ボタンをクリックします。 次の図は、シーケンスを示しています。


[![](healthkit-images/image13.png "この図では、イベントのシーケンスを示しています。")](healthkit-images/image13.png#lightbox)

キーを押して、**レコード**ボタンをクリックします。 これにより、`StoreData_TouchUpInside()`の値を解析するハンドラーを実行するには、`heartRate`テキスト フィールドに、変換に、`HKQuantity`経由で既に説明した`HeartRateModel.HeartRateInBeatsPerMinute()`関数を渡すには、その数量`HeartRateModel.StoreHeartRate()`します。 前述のように、これは、データを格納していずれかが発生する`HeartRateStored`または`ErrorMessageChanged`イベント。

ダブルクリックして、**ホーム**デバイスでボタンをクリックし、正常性のアプリを開きます。 をクリックして、**ソース**タブが表示されているサンプル アプリに表示されます。 選択して、心拍数のデータを更新する権限を許可しないようにします。 ダブルクリックして、**ホーム**ボタンとスイッチがアプリに戻します。 もう一度、`ReactToHealthCarePermissions()`呼び出される、ただし、アクセスが拒否されたため、 **StoreData**ボタンが無効になります (これが非同期に行われますことと、ユーザー インターフェイスの変更がエンドユーザーに表示される可能性がありますに注意してください)。

## <a name="advanced-topics"></a>高度なトピック

正常性キットからデータを読み取るデータベースはデータの書き込みによく似ていますデータは、要求の承認にアクセスしようとする 1 つの種類を 1 つを指定とその承認が与えられている場合、データは、の互換性のある単位に自動的に変換。測定します。

述語ベースのクエリと関連するデータが更新されたときに、更新プログラムを実行するクエリを許可するより高度なクエリ機能を数多くあります。 

アプリケーションの正常性のキットの開発者は、Apple の正常性のキット セクションを確認する必要があります[App レビューに関するガイドライン](https://developer.apple.com/app-store/review/guidelines/#healthkit)します。

セキュリティと型システムのモデルを理解すると、格納および共有の正常性のキット データベース内のデータの読み取りはとても簡単です。 正常性のキット内の関数の多くが非同期的に動作し、アプリケーション開発者が自分のプログラムを適切に記述する必要があります。

この記事の執筆時点ではありません現在 Android または Windows Phone での正常性のキットと等価です。

## <a name="summary"></a>まとめ

この記事では、正常性のキットを保存するアプリケーションを使用する方法を説明しました、取得、および共有の正常性にながら、ユーザーのアクセスとこのデータの制御を可能にする標準的な正常性のアプリについては、関連します。 

プライバシー、セキュリティ、およびデータの整合性が正常性に関連する情報の問題をオーバーライドする方法をも行ってきたし、アプリケーション管理の側面 (プロビジョニング)、(正常性キットの型のコーディングの複雑さの増加していることの正常性のキットを使用してアプリが処理する必要があります。システム)、およびユーザー エクスペリエンス (システム ダイアログと正常性のアプリを介したアクセス許可のユーザー コントロール)。 

最後に、ハートビート データをキットの正常性ストアに書き込み、非同期対応の設計が含まれているサンプル アプリを使用して正常性のキットの単純な実装について見ています。

## <a name="related-links"></a>関連リンク

- [HKWork (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
