---
title: Xamarin.iOS で HealthKit
description: このドキュメントでは、HealthKit、正常性関連の情報を一元的な調整、およびセキュリティで保護されたデータ ストアを提供する iOS 8 で導入されたフレームワークについて説明します。 これは、HealthKit framework を使用するコードを記述する方法と HealthKit アプリをプロビジョニングする方法について説明します。
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 06c0231bbb9aa7b82b92e0a8c2157b8be9c8b05b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787534"
---
# <a name="healthkit-in-xamarinios"></a>Xamarin.iOS で HealthKit

正常性キットは、ユーザーの正常性関連の情報をセキュリティで保護されたデータ ストアを提供します。 ヘルス キット アプリは、ユーザーの明示的な権限を持つ読み取りおよび書き込みこのデータストアにおよび関連データが追加されたときに通知を受信します。 アプリがデータを表示またはユーザーは、すべてのデータのダッシュ ボードを表示する Apple の指定されたヘルス アプリを使用できます。

正常性に関連するデータがあるためと小文字が区別され、重要なためヘルス キットが厳密に型指定されたメジャーと明示的な関連付け (たとえば、血液血糖または心拍数) に記録される情報の種類の単位にします。 さらに、ヘルス キット アプリは、明示的な権限を使用する必要があります、与える必要がありますとユーザーの特定の種類の情報にアクセス権を要求する必要があります明示的にこれらの種類のデータへのアプリ アクセスします。

この記事では説明します。

- アプリケーションのプロビジョニングおよびヘルス キット データベースにアクセスする権限をユーザーの要求を含む正常性キットのセキュリティ要件
- 正常性キットの型システムは、ミスを適用またはデータの変換の可能性を最小限に抑えられます
- 共有、システム全体のヘルス キット データストアを記述します。

この記事より高度なトピックについては、データベースの照会、測定単位の間で変換する、新しいデータの通知を受け取るなどは説明しません。

この記事では今後を作成するユーザーの心拍数を記録するサンプル アプリケーション。

[![](healthkit-images/image01.png "ユーザーの心拍数を記録するサンプル アプリケーション")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>必要条件

次がこの記事の手順を実行する必要です。

- **Xcode 7 および iOS 8 (またはそれ以上)** – Apple の最新の Xcode と iOS Api をインストールして、開発者のコンピューター上に構成する必要があります。
- **Mac または Visual Studio 用の visual Studio** – Visual Studio for Mac の最新バージョンをインストールし、開発者のコンピューター上に構成します。
- **iOS 8 (またはそれ以上) のデバイス**– 最新バージョンの iOS 8 以上のテストを実行している iOS デバイス。

> [!IMPORTANT]
> 正常性キットは、iOS 8 で導入されました。 現在、iOS シミュレーターでヘルス キットはできません、デバッグには、物理的な iOS デバイスへの接続が必要です。




## <a name="creating-and-provisioning-a-health-kit-app"></a>作成して、ヘルス キット アプリのプロビジョニング
Xamarin iOS 8 アプリケーションには、HealthKit API を使用ことができます、前に、正しく構成されているをプロビジョニングする必要があります。 このセクションでは、Xamarin アプリケーションを正しくセットアップに必要な手順を説明します。

正常性キット アプリが必要です。

- 明示的な**アプリ ID**です。
- A**プロビジョニング プロファイル**に関連付けられている明示的な**アプリ ID**と**ヘルス キット**アクセス許可。
- `Entitlements.plist`で、`com.apple.developer.healthkit`型のプロパティ`Boolean`'éý'`Yes`です。
- `Info.plist`が`UIRequiredDeviceCapabilities`キーには持つエントリが含まれています、`String`値`healthkit`です。
- `Info.plist`適切なプライバシーに関する説明のエントリも必要があります。`String`キーの説明`NSHealthUpdateUsageDescription`アプリ データを記述する場合は、`String`キーの説明`NSHealthShareUsageDescription`ヘルス キットを読み取るしようとして、アプリの場合データ。

IOS アプリのプロビジョニングに関する詳細を確認する、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)Xamarin の内のアーティクル**作業の開始**系列には、開発者の証明書、アプリ Id の間のリレーションシップがについて説明しますプロファイル、およびアプリの権利をプロビジョニングします。

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>明示的なアプリ ID とプロビジョニング プロファイル

明示的な作成**アプリ ID**と適切な**プロビジョニング プロファイル**Apple の操作は実行[iOS Dev Center](https://developer.apple.com/devcenter/ios/index.action)です。 

現在**のアプリ Id**に記載されて、[証明書、識別子、およびプロファイル](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action)デベロッパー センターの「します。 多くの場合、このリストに表示されます**ID**値`*`を示すを**アプリ ID** - **名前**任意の数のサフィックスで使用できます。 このような*ワイルドカードのアプリ Id*ヘルス キットでは使用できません。
 
明示的な作成を**アプリ ID**をクリックして、 **+** する右のボタン、 **iOS アプリ ID を登録**ページ。


[![](healthkit-images/image02.png "Apple 開発者ポータルでアプリを登録します。")](healthkit-images/image02.png#lightbox)

アプリの説明を作成した後、上の図に示すようにを使用して、**アプリ ID の明示的な**セクションで、アプリケーションの ID を作成します。 **App Services**  セクションで、チェック**ヘルス キット**で、**サービスの有効化**セクションです。

完了したら、キーを押して、**続行**を登録する ボタン、**アプリ ID**お客様のアカウントです。 戻る、**証明書、識別子、およびプロファイル**ページ。 をクリックして**プロビジョニング プロファイル**を現在のプロビジョニング プロファイルの一覧に移動し、をクリックして、 **+** する右上隅のボタン、 **iOS の追加プロビジョニング プロファイル**ページ。 選択、 **iOS アプリの開発**オプションをクリックして**続行**を取得する、**アプリ ID の選択**ページ。 ここでは、明示的な選択**アプリ ID**以前に指定します。


[![](healthkit-images/image03.png "明示的なアプリ ID を選択します")](healthkit-images/image03.png#lightbox)

をクリックして**続行**と指定する、残りの画面を仕事、**開発者の証明書**、**デバイス**、および**名前**この**プロビジョニング プロファイル**:

[![](healthkit-images/image04.png "プロビジョニング プロファイルを生成します。")](healthkit-images/image04.png#lightbox)

をクリックして**生成**し、プロファイルの作成を待機します。 ファイルをダウンロードし、ダブルクリックして、Xcode でインストールします。 下で、インストールを確認する**Xcode > 設定 > アカウント > の詳細を表示しています.** だけがインストールされているプロビジョニング プロファイルが表示され、正常性キットとでの他の特別なサービス用のアイコンが必要、**権利**行。

[![](healthkit-images/image05.png "Xcode でのプロファイルの表示")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>アプリ ID を関連付けると、プロビジョニング Xamarin.iOS アプリとプロファイル

作成され、適切なをインストールした後**プロビジョニング プロファイル**ように、通常なります Mac または Visual Studio の Visual Studio でソリューションを作成する時間。 正常性キット アクセスは、iOS (C#) または f# プロジェクトを使用です。

Xamarin iOS 8 のプロジェクトを手動で作成するプロセスを案内するのではなく、この記事の内容 (をあらかじめ作成されているストーリー ボードとコードを含む) に接続されているサンプル アプリを開きます。 サンプル アプリを有効になっている、正常性キットに関連付ける**プロビジョニング プロファイル**で、**ソリューション パッド**プロジェクトを右クリックし、立ち上げると、その**オプション**ダイアログ。 切り替えて、 **iOS アプリケーション**パネルし、明示的な入力**アプリ ID**アプリのとして以前に作成した**バンドル Id**:

[![](healthkit-images/image06.png "明示的なアプリ ID を入力してください。")](healthkit-images/image06.png#lightbox)

今すぐに切り替え、 **iOS バンドル署名** パネルです。 最近インストールされた**プロビジョニング プロファイル**、明示的なへの関連付けを**アプリ ID**、として使用できるようになりました、**プロビジョニング プロファイル**:

[![](healthkit-images/image07.png "プロビジョニング プロファイルを選択します。")](healthkit-images/image07.png#lightbox)

場合、**プロビジョニング プロファイル**が使用できないを再確認してください、**バンドル Id**で、 **iOS アプリケーション**パネルで指定されていると、 **iOSデベロッパー センター**ことと、**プロビジョニング プロファイル**がインストールされている (**Xcode > 設定 > アカウント >... の詳細の表示**).

ときにヘルス キットが有効な**プロビジョニング プロファイル**は選択すると、をクリックして**OK**プロジェクトのオプション ダイアログを閉じます。

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist と Info.plist 値

サンプル アプリを含む、`Entitlements.plist`ファイル (必要な正常性キットには、アプリが有効になっている)、すべてのプロジェクト テンプレートに含まれていないとします。 プロジェクトに権利が含まれていない場合、プロジェクトを右クリックし、選択**ファイル > 新しい File… > iOS > Entitlements.plist**を手動で追加します。

最終的には、`Entitlements.plist`次のキーと値のペアを持つ必要があります。

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

同様に、`Info.plist`の値がある、アプリの`healthkit`に関連付けられている、`UIRequiredDeviceCapabilities`キー。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

この記事で提供されるサンプル アプリケーションが含まれていますが、構成済み`Entitlements.plist`を含むすべての必須のキー。

<a name="programming" />

## <a name="programming-health-kit"></a>プログラミング ヘルス キット

ヘルス キット データ ストアは、アプリ間で共有される、ユーザー固有のプライベート データ ストアです。 正常性の情報は機密性の高いためであるため、ユーザーは、データ アクセスを許可するための正の手順を実行する必要があります。 このアクセスは、部分的な可能性があります (読み取り、一部の種類のデータだけが、アクセスではありませんが、書き込みなど) と、いつでも取り消すことがあります。 正常性キット アプリケーションは、多くのユーザーが、正常性関連の情報を格納する方法についてためらうなる理解したうえで防御的、作成する必要があります。

正常性キット データに限定されます Apple の種類を指定します。 これらの型が厳密に定義されている: 血液型など、一部は、他のユーザーを組み合わせて、絶対値 (グラム、カロリー、リットルなど) の測定単位中に、Apple の指定された列挙型の特定の値に制限されます。 互換性のある測定単位を共有するデータがによって識別されます、`HKObjectType`たとえば、型システムは間違ってしようとしたストアをキャッチする、`HKQuantityTypeIdentifier.NumberOfTimesFallen`フィールド予測する値、`HKQuantityTypeIdentifier.FlightsClimbed`どちらも使用する場合でも、` HKUnit.Count` 。測定単位。

ヘルス キット データ ストアの保存の種類は、のすべてのサブクラス`HKObjectType`です。 `HKCharacteristicType` 生物の性別、血液型、および生年月日をオブジェクトが格納されます。 ただしより一般的なは、 `HKSampleType` 、特定の時刻または期間にわたって、サンプリングされるデータを表すオブジェクト。 

[![](healthkit-images/image08.png "HKSampleType オブジェクト グラフ")](healthkit-images/image08.png#lightbox)

`HKSampleType` 抽象であり具体的なサブクラスが 4 つです。 現在の 1 つだけの型がある`HKCategoryType`分析のスリープ状態であるデータ。 ヘルス キット内のデータの大多数は、型の`HKQuantityType`にデータを保存および`HKQuantitySample`使い慣れたファクトリ デザイン パターンを使用して作成されるオブジェクト。

[![](healthkit-images/image09.png "ヘルス キット内のデータの大多数の種類 HKQuantityType と HKQuantitySample オブジェクトにデータを格納")](healthkit-images/image09.png#lightbox)

`HKQuantityType` 型の範囲`HKQuantityTypeIdentifier.ActiveEnergyBurned`に`HKQuantityTypeIdentifier.StepCount`です。 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>ユーザーからのアクセス許可を要求します。

エンドユーザーは、ヘルス キット データを読み書きするアプリを許可する正の手順を実行する必要があります。 これは iOS 8 デバイスに事前にインストールされる正常性アプリ経由で行われます。 ヘルス キット アプリを実行すると、最初に、ユーザーは、システム管理の表示は**ヘルス アクセス** ダイアログ。

[![](healthkit-images/image10.png "システム制御ヘルス アクセス ダイアログ ボックスで、ユーザーが表示されます。")](healthkit-images/image10.png#lightbox)

後で、ユーザーがアプリの正常性を使用してアクセス許可を変更できる**ソース** ダイアログ。

[![](healthkit-images/image11.png "ユーザーは、正常性のアプリのソース ダイアログを使用してアクセス許可を変更できます。")](healthkit-images/image11.png#lightbox)

正常性の情報は非常に機密性が高いためアプリ記述べき、プログラム防御的、権限の拒否され、アプリが実行されているときに変更行われることを見込んでです。 最も一般的な表現形式でのアクセス許可を要求する、`UIApplicationDelegate.OnActivated`メソッドおよび適切なユーザー インターフェイスを変更します。

### <a name="permissions-walkthrough"></a>アクセス許可のチュートリアル

正常性のキットがプロビジョニングしたプロジェクトで、開く、`AppDelegate.cs`ファイル。 使用して、ステートメントに注意してください。`HealthKit`以外の場合は、ファイルの上部にあります。


次のコードが正常性のキットのアクセス許可に関連します。

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

すべてのこれらのメソッドでコードがインラインで行う`OnActivated`、サンプル アプリがそれらの意図を明確に異なるメソッドを使用:`ValidateAuthorization()`は、特定の型が書き込まれている (および読み取り、アプリが必要な場合) にアクセスを要求するために必要な手順がありますおよび`ReactToHealthCarePermissions()`Health.app [アクセス許可] ダイアログで、ユーザーが操作した後にアクティブ化されるコールバックです。

ジョブ`ValidateAuthorization()`のセットを作成するのには、`HKObjectTypes`こと、アプリは書き込みし、そのデータを更新するための承認を要求します。 サンプル アプリで、`HKObjectType`はキー用`KHQuantityTypeIdentifierKey.HeartRate`です。 この種類は、セットに追加`typesToWrite`、セットの中に`typesToRead`空のままにします。 これらのセットとへの参照、`ReactToHealthCarePermissions()`にコールバックが渡される`HKHealthStore.RequestAuthorizationToShare()`です。

`ReactToHealthCarePermissions()`ユーザーの操作のアクセス許可 ダイアログと 2 つの情報が渡されたコールバックが呼び出されます、`bool`値となる`true`アクセス許可ダイアログと、 、ユーザーが操作した場合は`NSError`。、null 以外の場合、いくつかの種類を示しますにアクセス許可 ダイアログを表示する関連付けられたエラー。

> [!IMPORTANT]
> この関数の引数を明示する:_成功_と_エラー_パラメーターは、ユーザーが正常性キット データにアクセスする権限を与えるかどうかを指定しません。 これらは、のみ、データへのアクセスを許可するように営業案件が、ユーザーに付与されていることを示します。

アプリが、データへのアクセスを持つかどうかを確認する、`HKHealthStore.GetAuthorizationStatus()`を渡して、使用`HKQuantityTypeIdentifierKey.HeartRate`です。 返される状態に基づいて、アプリを有効またはデータを入力する機能を無効にします。 アクセスの拒否攻撃に対処するための標準的なユーザー エクスペリエンスがないと、多くの可能なオプションがあります。 状態の設定の例のアプリで、`HeartRateModel`シングルトン オブジェクトをさらに、関連するイベントを発生させます。

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

最初のセクションでは、一般的なイベントおよびハンドラーを作成するための定型コードです。 最初の部分、`HeartRateModel`クラスはスレッド セーフである単一オブジェクトを作成するための定型句もです。

その後、 `HeartRateModel` 3 のイベントを公開します。 

- `EnabledChanged` -心拍数の記憶域が有効または無効にされていることを示します (記憶域が最初に無効になっていることに注意してください)。 
- `ErrorMessageChanged` -非常に単純なエラー処理モデルがあるこのサンプル アプリの: 最後のエラーを含む文字列。 
- `HeartRateStored` -心拍数が、正常性キット データベースに格納されているときに発生します。

なおを使用して行うことは、これらのイベントが起動されるたびに`NSObject.InvokeOnMainThread()`、これにより、UI を更新するサブスクライバー。 代わりに、バック グラウンド スレッドで発生すると、イベントを文書化する可能性がありの互換性を保証する責任は、そのハンドラーに残しておく可能性があります。 スレッドの考慮事項は、多くのアクセス許可の要求など、関数は非同期であり、コールバックをメイン以外のスレッドで実行するためにヘルス キット アプリケーションで重要です。

Heath キットの特定のコードで`HeartRateModel`2 つの関数では、`HeartRateInBeatsPerMinute()`と`StoreHeartRate()`です。 

`HeartRateInBeatsPerMinute()` 厳密に型指定された状態セットへの引数の変換`HKQuantity`です。 数量の型がで指定されている、`HKQuantityTypeIdentifierKey.HeartRate`数量の単位は`HKUnit.Count`で割った値`HKUnit.Minute`(単位は、つまり、*分あたりのビート*)。 

`StoreHeartRate()`関数は、 `HKQuantity` (サンプル アプリでは 1 つ作成`HeartRateInBeatsPerMinute()`)。 使用してそのデータを検証する、`HKQuantity.IsCompatible()`を返すメソッド`true`オブジェクトの単位は、引数の単位に変換できる場合です。 数量の作成時の`HeartRateInBeatsPerMinute()`が明らかに返されます`true`、返すことも、`true`数量として作成された、インスタンスの場合*時間あたりのビート*です。 一般的には、`HKQuantity.IsCompatible()`大容量の検証に使用できる距離、およびエネルギーが、これは、ユーザーまたはデバイスの入力 (帝国ユニット数) などの測定値の 1 つのシステムで表示 (メートル単位) などの別のシステムに格納される可能性があります。 

数量との互換性が検証された後、 `HKQuantitySample.FromType()` 、厳密に型指定を作成するファクトリ メソッドが使用される`heartRateSample`オブジェクト。 `HKSample` オブジェクトがある、開始日と終了日です。瞬時の測定値のこれらの値と同じであるの例ではします。 サンプルでもは設定されていない任意のキー値のデータその`HKMetadata`は 1 つの引数、コードを使用して、次のコードなどセンサーの場所を指定します。

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

1 回、`heartRateSample`が作成されると、コードを作成、データベースへの新しい接続を使用してブロックします。 そのブロック内で、`HKHealthStore.SaveObject()`メソッドは、データベースへの非同期書き込みを試行します。 ラムダ式の結果として得られる呼び出しをするか、関連するイベントは、トリガー`HeartRateStored`または`ErrorMessageChanged`です。

これで、モデルがプログラムされてが、コント ローラーが、モデルの状態を反映する方法を参照してください。 開き、`HKWorkViewController.cs`ファイル。 コンス トラクターは、単に呼び出され、`HeartRateModel`単一イベント処理メソッドに (こうことがインライン ラムダ式でも異なるメソッドを指定する、目的は、もう少し明確)。

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

当然ながら、アプリケーションでは単一のコント ローラーで、別のモデル オブジェクトの作成と制御フロー、イベントの使用を回避する可能性がありますが、モデル オブジェクトの使用は実際のアプリケーションに適しています。

## <a name="running-the-sample-app"></a>サンプル アプリの実行

IOS シミュレーターでは、正常性のキットがサポートされていません。 デバッグは、iOS 8 を実行している物理デバイスで行う必要があります。

システムに適切にプロビジョニングされて iOS 8 開発用のデバイスをアタッチします。 Mac 用の Visual Studio での配置ターゲットとして選択し、メニュー**実行 > デバッグ**です。

> [!IMPORTANT]
> この時点でのプロビジョニングに関連する間違いが現れます。 エラーをトラブルシューティングするには、作成と前の正常性のキットのアプリ セクションのプロビジョニングを確認します。 コンポーネントは次のとおりです。 
>
> - **iOS Dev Center** -明示的なアプリ ID とヘルス キットは、プロビジョニング プロファイルを有効にします。 
> - **プロジェクト オプション**-バンドル Id (明示的なアプリ ID) とプロビジョニング プロファイル。
> - **ソース コード**-Entitlements.plist & Info.plist

プロビジョニングを正しく設定されていると想定されるので、アプリケーションが開始されます。 達したとき、`OnActivated`メソッド、正常性のキットの承認を要求します。 これが、オペレーティング システムによって検出された最初に次のダイアログ ボックスで、ユーザーが表示されます。


[![](healthkit-images/image12.png "このダイアログ ボックスで、ユーザーが表示されます。")](healthkit-images/image12.png#lightbox)

心拍数のデータを更新するアプリを有効にし、アプリは再表示されます。 `ReactToHealthCarePermissions`コールバックを非同期的にアクティブ化されます。 これにより、 `HeartRateModel’s` `Enabled`が発生するプロパティを変更する、`EnabledChanged`原因となるイベント、`HKPermissionsViewController.OnEnabledChanged()`されるようにすることを実行するイベント ハンドラー、`StoreData`ボタンをクリックします。 次の図は、シーケンスを示しています。


[![](healthkit-images/image13.png "この図は、イベントのシーケンスを示しています。")](healthkit-images/image13.png#lightbox)

キーを押して、**レコード**ボタンをクリックします。 これにより、`StoreData_TouchUpInside()`の値を解析するハンドラーを実行するには、`heartRate`テキスト フィールドに変換、`HKQuantity`経由で既に説明したよう`HeartRateModel.HeartRateInBeatsPerMinute()`関数およびその数量を渡す`HeartRateModel.StoreHeartRate()`です。 前述のように、これは、データを格納してが発生するか、`HeartRateStored`または`ErrorMessageChanged`イベント。

ダブルクリックして、**ホーム**デバイスでボタンをクリックし、正常性のアプリを開きます。 クリックして、**ソース**タブとするサンプル アプリを一覧表示が表示されます。 それを選択し、心拍数のデータを更新する権限を許可しないようにします。 ダブルクリックして、**ホーム**ボタンとスイッチがアプリにバックアップします。 もう一度`ReactToHealthCarePermissions()`呼び出される、この時間が、アクセスが拒否されたため、 **StoreData**ボタンが無効になります (非同期的にこれが発生し、ユーザー インターフェイスの変更がエンドユーザーに表示される可能性がありますを注意してください)。

## <a name="advanced-topics"></a>高度なトピック

正常性キットからデータを読み取るデータベースは、データの書き込みによく似ています、要求の承認にアクセスしようとする 1 つのデータの種類を指定し、その承認が与えられている場合、データは、互換性のある単位へ自動変換を持つ。測定します。

述語ベースのクエリと関連データが更新されたときに、更新プログラムを実行するクエリを許可するより高度なクエリ関数の数があります。 

ヘルス キット アプリケーションの開発者は、Apple のヘルス キット セクションを確認する必要があります[App レビューに関するガイドライン](https://developer.apple.com/app-store/review/guidelines/#healthkit)です。

セキュリティとモデルの型システムが認識されたらを格納して、共有ヘルス キット データベース内のデータの読み取りは非常に簡単です。 非同期動作の正常性キット内で関数の多くと、アプリケーション開発者は、プログラムを適切に記述する必要があります。

この記事の執筆時点ではありません現在 Android や Windows Phone での正常性キットに相当します。

## <a name="summary"></a>まとめ

正常性のキットを保存するアプリケーションを使用するどのこれまで見てきたこの記事で取得、および共有の正常性関連の情報、ながら、ユーザーのアクセスやこのデータを制御する標準的なヘルス アプリ。 

プライバシー、セキュリティ、およびデータの整合性が正常性関連の情報の問題をオーバーライドする方法も検出されて、アプリケーション管理の各側面 (プロビジョニング)、(ヘルス キットの型のコーディングの複雑さで増加ヘルス キットを使用してアプリを処理する必要があります。システム)、およびユーザー (システム ダイアログと正常性のアプリを使用してアクセス許可のユーザーによる制御) が発生します。 

最後に、ハートビート データ ヘルス キット ストアを書き込みますを非同期に対応するデザインを持つサンプル アプリを使用して正常性のキットの簡単な実装を見る示しました。

## <a name="related-links"></a>関連リンク

- [HKWork (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
