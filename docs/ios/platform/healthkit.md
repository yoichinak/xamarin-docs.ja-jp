---
title: Xamarin. iOS の HealthKit
description: このドキュメントでは、HealthKit について説明します。これは、iOS 8 で導入されたフレームワークであり、正常性に関連したデータストアを一元化、連携、セキュリティで保護します。 HealthKit アプリをプロビジョニングする方法と、HealthKit フレームワークを使用するコードを記述する方法について説明します。
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 6446bd7ef196fadae25c0e4dc18542d269424d6d
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200275"
---
# <a name="healthkit-in-xamarinios"></a>Xamarin. iOS の HealthKit

Health Kit は、ユーザーの正常性に関連する情報のセキュリティで保護されたデータストアを提供します。 正常性キットアプリは、ユーザーの明示的なアクセス許可、このデータストアへの読み取りと書き込み、関連データが追加されたときに通知を受け取ることができます。 アプリはデータを提示することができます。また、ユーザーは Apple が提供する正常性アプリを使用して、すべてのデータのダッシュボードを表示できます。

正常性に関連するデータは非常に重要で重要なので、正常性キットは厳密に型指定されており、測定単位と記録されている情報の種類 (たとえば、血液 glucose レベルまたはハートレート) との明示的な関連付けがあります。 また、正常性キットアプリでは、明示的な権利を使用する必要があり、特定の種類の情報へのアクセスを要求する必要があります。また、ユーザーは、これらの種類のデータへのアクセスをアプリに明示的に付与する必要があります。

この記事では、次のことについて説明します。

- 正常性キットのセキュリティ要件 (アプリケーションのプロビジョニングや、正常性キットデータベースにアクセスするためのユーザーアクセス許可の要求を含む)
- 正常性キットの型システム。これにより、データを誤って適用したり、誤って解釈したりする可能性が最小限に抑えられます。
- システム全体に共通する正常性キットデータストアへの書き込み。

この記事では、データベースのクエリ、測定単位間の変換、新しいデータの通知の受信など、より高度なトピックについては説明しません。

この記事では、ユーザーのハートレートを記録するサンプルアプリケーションを作成します。

[![](healthkit-images/image01.png "ユーザーのハートレートを記録するサンプルアプリケーション")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>必要条件

この記事に記載されている手順を完了するには、次のものが必要です。

- **Xcode 7 と ios 8 (またはそれ以降)** – Apple の最新の Xcode と ios api は、開発者のコンピューターにインストールして構成する必要があります。
- **Visual Studio for Mac または Visual Studio** –最新バージョンの Visual Studio for Mac は、開発者のコンピューターにインストールして構成する必要があります。
- **ios 8 (またはそれ以降) のデバイス**–テスト用に ios 8 以降の最新バージョンを実行している ios デバイスです。

> [!IMPORTANT]
> Health Kit は、iOS 8 で導入されました。 現在、iOS シミュレーターでは正常性キットを使用できません。また、デバッグを行うには、物理的な iOS デバイスに接続する必要があります。




## <a name="creating-and-provisioning-a-health-kit-app"></a>正常性キットアプリの作成とプロビジョニング
Xamarin iOS 8 アプリケーションでは、HealthKit API を使用する前に、適切に構成してプロビジョニングする必要があります。 このセクションでは、Xamarin アプリケーションを適切に設定するために必要な手順について説明します。

ヘルスキットアプリには以下が必要です。

- 明示的な**アプリ ID**。
- その明示的な**アプリ ID**と**正常性キット**のアクセス許可に関連付けられている**プロビジョニングプロファイル**。
- 型`Entitlements.plist` `com.apple.developer.healthkit` のプロパティ`Yes`がに設定された。 `Boolean`
- `String`値`UIRequiredDeviceCapabilities` `Info.plist` を持つ`healthkit`エントリを格納しているキーを持つ。
- また`Info.plist` 、には、適切なプライバシーに関する説明`String`のエントリが必要`NSHealthUpdateUsageDescription`です。 `String`アプリが正常性キットを読み取っている場合に`NSHealthShareUsageDescription` 、アプリがデータを書き込む場合のキーの説明とキーの説明データ.

IOS アプリのプロビジョニングの詳細については、Xamarin の**はじめに**シリーズの[デバイスプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)に関する記事で、開発者の証明書、アプリ id、プロビジョニングプロファイル、アプリの権利の関係について説明しています。

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>明示的なアプリ ID とプロビジョニングプロファイル

明示的な**アプリ ID**と適切な**プロビジョニングプロファイル**の作成は、Apple の[iOS デベロッパーセンター](https://developer.apple.com/devcenter/ios/index.action)内で行われます。 

現在の**アプリ id**は、デベロッパーセンターの [ [Certificates, identifier & Profiles](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) ] セクション内に一覧表示されます。 多くの場合、この一覧にはの`*`**id** 値が表示されます。これは、**アプリ id** - **名**を任意の数のサフィックスと共に使用できることを示します。 このような*ワイルドカードアプリ id*を正常性キットと一緒に使用することはできません。
 
明示的な**アプリ id**を作成するに **+** は、右上のボタンをクリックして、 **[iOS アプリ id の登録]** ページに移動します。


[![](healthkit-images/image02.png "Apple Developer ポータルでのアプリの登録")](healthkit-images/image02.png#lightbox)

上の図に示されているように、アプリの説明を作成した後、 **[明示的なアプリ id]** セクションを使用してアプリケーションの id を作成します。 **[App Services]** セクションで、 **[サービスの有効化]** セクションの **[Health Kit]** をオンにします。

完了したら、 **[続行]** ボタンをクリックして、アカウントに**アプリ ID**を登録します。 **[証明書、識別子、およびプロファイル]** ページに戻ります。 **[プロビジョニングプロファイル]** をクリックして現在のプロビジョニングプロファイルの一覧に移動し、 **+** 右上隅にあるボタンをクリックして **[iOS プロビジョニングプロファイルの追加]** ページに移動します。 **[IOS アプリ開発]** オプションを選択し、 **[続行]** をクリックして **[アプリ ID の選択]** ページに移動します。 ここでは、前に指定した明示的な**アプリ ID**を選択します。


[![](healthkit-images/image03.png "明示的なアプリ ID を選択します")](healthkit-images/image03.png#lightbox)

**[続行]** をクリックして残りの画面を表示します。ここで、**開発者の証明書**、**デバイス**、およびこの**プロビジョニングプロファイル**の**名前**を指定します。

[![](healthkit-images/image04.png "プロビジョニングプロファイルを生成しています")](healthkit-images/image04.png#lightbox)

**[生成]** をクリックして、プロファイルの作成を待機します。 ファイルをダウンロードし、ダブルクリックして Xcode にインストールします。 インストールされていることを確認するには、 **Xcode > の [基本設定] > アカウント > [詳細の表示...] を使用します。** インストール済みのプロビジョニングプロファイルが表示され、**権利** 行に Health Kit とその他の特別なサービスのアイコンが表示されます。

[![](healthkit-images/image05.png "Xcode でのプロファイルの表示")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>アプリ ID とプロビジョニングプロファイルを Xamarin iOS アプリに関連付ける

説明に従って適切な**プロビジョニングプロファイル**を作成してインストールしたら、通常は Visual Studio for Mac または Visual Studio でソリューションを作成します。 ヘルスキットアクセスは、iOS C#またはF#プロジェクトで利用できます。

Xamarin iOS 8 プロジェクトを手動で作成するプロセスを実行するのではなく、この記事に添付されているサンプルアプリ (ストーリーボードとコードを含む) を開きます。 サンプルアプリを正常性キットが有効になっている**プロビジョニングプロファイル**に関連付けるには、 **Solution Pad**でプロジェクトを右クリックし、 **[オプション]** ダイアログを表示します。 **IOS アプリケーション**パネルに切り替え、以前にアプリの**バンドル識別子**として作成した明示的な**アプリ ID**を入力します。

[![](healthkit-images/image06.png "明示的なアプリ ID を入力してください")](healthkit-images/image06.png#lightbox)

今すぐに切り替え、 **iOS バンドル署名** パネルです。 最近インストールされた**プロビジョニングプロファイル**は、明示的な**アプリ ID**に関連付けられており、**プロビジョニングプロファイル**として使用できるようになりました。

[![](healthkit-images/image07.png "プロビジョニングプロファイルの選択")](healthkit-images/image07.png#lightbox)

**プロビジョニングプロファイル**を使用できない場合は、Ios**デベロッパーセンター**で指定されているものと、**プロビジョニングプロファイル**がインストールされている**ios アプリケーション**パネルで**バンドル id**を再確認します (**Xcode> の設定 > アカウント > 詳細の表示...** )。

正常性キット対応の **[プロビジョニングプロファイル]** を選択したら、 **[OK]** をクリックして [プロジェクトオプション] ダイアログボックスを閉じます。

### <a name="entitlementsplist-and-infoplist-values"></a>権利の plist と情報 plist の値

このサンプルアプリには`Entitlements.plist` 、(正常性キットが有効になっているアプリに必要な) ファイルが含まれており、すべてのプロジェクトテンプレートには含まれていません。 プロジェクトに権利が含まれていない場合は、プロジェクトを右クリックし、[ファイル]、[新しいファイルの >] の順に選択します。 **iOS > を >** して手動で追加します。

最終的に、 `Entitlements.plist`には次のキーと値のペアが必要です。

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

同様に、アプリ`UIRequiredDeviceCapabilities` `healthkit` `Info.plist`のの値はキーに関連付けられている必要があります。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

この記事で提供するサンプルアプリケーションには、 `Entitlements.plist`必要なすべてのキーを含む事前構成済みのが含まれています。

<a name="programming" />

## <a name="programming-health-kit"></a>正常性キットのプログラミング

正常性キットのデータストアは、アプリ間で共有されるプライベートなユーザー固有のデータストアです。 ヘルス情報が非常に重要であるため、ユーザーはデータアクセスを許可するために正の手順を実行する必要があります。 このアクセスは部分的なものである場合があります (読み取りはできず、一部の種類のデータにアクセスできますが、その他はアクセスできません)。また、いつでも取り消すことができます。 正常性キットアプリケーションは防御に記述する必要があります。多くのユーザーは、正常性に関連する情報の保存にためらいされることを理解しています。

ヘルスキットデータは、Apple が指定した種類に限定されます。 これらの型は厳密に定義されています。血液型などの一部は、Apple が提供する列挙体の特定の値に制限されますが、他の型は測定単位 (グラム、calories、リットルなど) を持つ大きさを組み合わせたものです。 互換性のある測定単位を共有するデータであっても`HKObjectType`、によって識別されます。たとえば、型システム`HKQuantityTypeIdentifier.NumberOfTimesFallen` `HKQuantityTypeIdentifier.FlightsClimbed`は、を使用`HKUnit.Count`する場合でも、測定単位。

正常性キットデータストアの格納できる型は、のすべて`HKObjectType`のサブクラスです。 `HKCharacteristicType`オブジェクトには、生物性別、血液の種類、生年月日が格納されます。 ただし、一般的には`HKSampleType` 、特定の時間または一定期間にサンプリングされるデータを表すオブジェクトです。 

[![](healthkit-images/image08.png "HKSampleType オブジェクトグラフ")](healthkit-images/image08.png#lightbox)

`HKSampleType`は abstract であり、4つの具象サブクラスを持ちます。 現在、スリープ分析であるデータ`HKCategoryType`の種類は1つだけです。 Health Kit のデータの大部分は型`HKQuantityType`で、オブジェクトに`HKQuantitySample`データを格納します。オブジェクトは、使い慣れたファクトリデザインパターンを使用して作成されます。

[![](healthkit-images/image09.png "Health Kit のデータの大部分は HKQuantityType 型で、HKQuantitySample オブジェクトにデータを格納します。")](healthkit-images/image09.png#lightbox)

`HKQuantityType`型の範囲`HKQuantityTypeIdentifier.ActiveEnergyBurned`は`HKQuantityTypeIdentifier.StepCount`からです。 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>ユーザーにアクセス許可を要求しています

エンドユーザーは、アプリが正常性キットデータの読み取りまたは書き込みを行えるようにするために、正の手順を実行する必要があります。 これは、iOS 8 デバイスにプレインストールされている正常性アプリを使用して行います。 正常性キットアプリを初めて実行すると、システムによって管理される**正常性のアクセス**ダイアログが表示されます。

[![](healthkit-images/image10.png "ユーザーには、システムによって管理されているヘルスアクセスダイアログが表示されます。")](healthkit-images/image10.png#lightbox)

その後、ユーザーは、ヘルスアプリの **[ソース]** ダイアログボックスを使用してアクセス許可を変更できます。

[![](healthkit-images/image11.png "ユーザーは、[ヘルスアプリソース] ダイアログボックスを使用してアクセス許可を変更できます。")](healthkit-images/image11.png#lightbox)

正常性の情報は非常に機密性が高いため、アプリの開発者は、アプリの実行中にアクセス許可が拒否され、変更されることを見込んで、プログラムを防御に記述する必要があります。 最も一般的な表現は、 `UIApplicationDelegate.OnActivated`メソッドでアクセス許可を要求し、必要に応じてユーザーインターフェイスを変更することです。

### <a name="permissions-walkthrough"></a>アクセス許可のチュートリアル

正常性キットによってプロビジョニングされた`AppDelegate.cs`プロジェクトで、ファイルを開きます。 ファイルの先頭に`HealthKit`ある; を使用して、ステートメントに注意してください。


次のコードは、正常性キットのアクセス許可に関連しています。

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

これらのメソッドのすべてのコードは、でインラインで`OnActivated`実行できますが、サンプルアプリでは個別のメソッドを使用`ValidateAuthorization()`して目的を明確にしています。書き込み中の特定の型へのアクセスを要求するための手順があります (アプリが必要な場合は読み取り)。と`ReactToHealthCarePermissions()`は、ユーザーが正常性アプリの [アクセス許可] ダイアログを使用した後にアクティブ化されるコールバックです。

の`ValidateAuthorization()`ジョブでは、アプリが書き込む`HKObjectTypes`セットを作成し、そのデータを更新するための承認を要求します。 サンプルアプリ`HKObjectType`では、はキー `KHQuantityTypeIdentifierKey.HeartRate`を対象としています。 この型はセット`typesToWrite`に追加され、セット`typesToRead`は空のままになります。 これらのセット、および`ReactToHealthCarePermissions()`コールバックへの参照は、に`HKHealthStore.RequestAuthorizationToShare()`渡されます。

コールバックは、ユーザーがアクセス許可ダイアログを`bool`操作した後に呼び出され、2つの情報が渡されます。 `true`これは、ユーザーがアクセス許可ダイアログを操作した場合の値です。 `ReactToHealthCarePermissions()` `NSError`null 以外の場合は、アクセス許可ダイアログの表示に関連するエラーの種類を示します。

> [!IMPORTANT]
> この関数の引数を明確にするために、 _success_および_error_パラメーターは、ユーザーが正常性キットデータへのアクセス許可を付与したかどうかを示しません。 ユーザーにデータへのアクセスを許可する機会が与えられていることだけを示します。

アプリがデータにアクセスできるかどうかを確認する`HKHealthStore.GetAuthorizationStatus()`には、を使用`HKQuantityTypeIdentifierKey.HeartRate`してを渡します。 返された状態に基づいて、アプリはデータを入力する機能を有効または無効にします。 アクセス拒否に対処するための標準的なユーザーエクスペリエンスはありません。また、多くのオプションがあります。 この例のアプリでは、 `HeartRateModel`シングルトンオブジェクトに状態が設定され、さらに関連するイベントが発生します。

## <a name="model-view-and-controller"></a>モデル、ビュー、およびコントローラー

`HeartRateModel`シングルトンオブジェクトを確認するには、 `HeartRateModel.cs`次のようにファイルを開きます。

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

最初のセクションは、汎用イベントとハンドラーを作成するための定型コードです。 `HeartRateModel`クラスの最初の部分は、スレッドセーフなシングルトンオブジェクトを作成するための定型的な部分でもあります。

次に`HeartRateModel` 、は3つのイベントを公開します。 

- `EnabledChanged`-ハートレートストレージが有効になっているか無効になっていることを示します (ストレージが最初に無効になっていることに注意してください)。 
- `ErrorMessageChanged`-このサンプルアプリでは、非常に単純なエラー処理モデル (最後のエラーを含む文字列) が用意されています。 
- `HeartRateStored`-ハートレートが正常性キットデータベースに格納されている場合に発生します。

これらのイベントが発生するたびに、によっ`NSObject.InvokeOnMainThread()`て実行されるので、サブスクライバーは UI を更新できます。 または、イベントがバックグラウンドスレッドで発生しているとして文書化されていて、互換性を確認する必要があるかどうかを、ハンドラーに委ねておく必要があります。 権限要求などの多くの関数は非同期であり、メイン以外のスレッドでコールバックを実行するため、正常性キットアプリケーションではスレッドに関する考慮事項が重要です。

の`HeartRateModel`ヘルスキット固有のコードは、と`StoreHeartRate()`の 2 `HeartRateInBeatsPerMinute()`つの関数に含まれています。 

`HeartRateInBeatsPerMinute()`引数を厳密に型指定された正常`HKQuantity`性キットに変換します。 に`HKQuantityTypeIdentifierKey.HeartRate`よって指定された数量の種類と数量の単位がによっ`HKUnit.Count`て`HKUnit.Minute`分割されます (つまり、単位は 1*分あたり拍*です)。 

関数`StoreHeartRate()`は (サンプル`HKQuantity`アプリでは、によって`HeartRateInBeatsPerMinute()`作成された) を受け取ります。 データを検証するには、 `HKQuantity.IsCompatible()`メソッドを使用します。これは、オブジェクトの単位を引数の単位に変換できる場合にを返し`true`ます。 こので`HeartRateInBeatsPerMinute()`数量が作成された場合は`true`、が返されます`true`が、たとえば、 *1 時間あたりの拍*数として数量が作成された場合にもが返されます。 より一般的に`HKQuantity.IsCompatible()`は、ユーザーまたはデバイスが1つの測定システム (ヤード単位など) で入力または表示しても、別のシステム (メトリックユニットなど) に格納されている可能性がある、質量、距離、およびエネルギーを検証するために使用できます。 

数量の互換性が検証されると、 `HKQuantitySample.FromType()`ファクトリメソッドを使用して、厳密に型指定`heartRateSample`されたオブジェクトが作成されます。 `HKSample`オブジェクトには開始日と終了日があります。瞬間的に読み取れるように、これらの値は、例に示すように同じである必要があります。 また、このサンプルでは、 `HKMetadata`引数にキー値データを設定しませんが、次のコードのようなコードを使用してセンサーの場所を指定することもできます。

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

`heartRateSample`が作成されると、コードは using ブロックを使用してデータベースへの新しい接続を作成します。 このブロック内で、 `HKHealthStore.SaveObject()`メソッドはデータベースへの非同期書き込みを試行します。 結果として得られるラムダ式への呼び出し`HeartRateStored`で`ErrorMessageChanged`は、またはのいずれかの関連イベントがトリガーされます。

モデルがプログラミングされたので、次は、コントローラーがモデルの状態を反映していることを確認します。 `HKWorkViewController.cs`ファイルを開きます。 コンストラクターは単純に`HeartRateModel`シングルトンをイベント処理メソッドに配線します (この場合も、ラムダ式を使用してインラインで実行できますが、個別のメソッドにより明確になります)。

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

関連するハンドラーは次のとおりです。

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

当然ながら、1つのコントローラーを持つアプリケーションでは、個別のモデルオブジェクトの作成と制御フローのイベントの使用を避けることができますが、モデルオブジェクトの使用は実際のアプリに適しています。

## <a name="running-the-sample-app"></a>サンプルアプリの実行

IOS シミュレーターでは、Health Kit はサポートされていません。 デバッグは、iOS 8 を実行している物理デバイスで実行する必要があります。

適切にプロビジョニングされた iOS 8 開発デバイスをシステムに接続します。 Visual Studio for Mac でデプロイターゲットとして選択し、メニューから [**実行] [> デバッグ**] の順に選択します。

> [!IMPORTANT]
> プロビジョニングに関連する間違いはこの時点で表示されます。 エラーのトラブルシューティングを行うには、前の「正常性キットアプリの作成とプロビジョニング」セクションを参照してください。 コンポーネントは次のとおりです。 
>
> - **IOS デベロッパーセンター** -EXPLICIT アプリ ID & Health Kit が有効になっているプロビジョニングプロファイル。 
> - **プロジェクトオプション**-バンドル識別子 (明示的なアプリ ID) & プロビジョニングプロファイル。
> - **ソースコード**-権利 & 情報 plist

プロビジョニングが適切に設定されていると仮定すると、アプリケーションが起動します。 その`OnActivated`メソッドに到達すると、正常性キットの承認を要求します。 オペレーティングシステムによって初めて検出された場合、ユーザーには次のダイアログが表示されます。


[![](healthkit-images/image12.png "ユーザーにこのダイアログが表示されます")](healthkit-images/image12.png#lightbox)

アプリでハートレートデータを更新できるようにします。アプリは再び表示されます。 `ReactToHealthCarePermissions`コールバックは非同期にアクティブ化されます。 これにより、 `HeartRateModel’s`プロパティが`Enabled`変更され、イベントが`EnabledChanged` `HKPermissionsViewController.OnEnabledChanged()`発生します。これにより、イベントハンドラーが実行され`StoreData` 、ボタンが有効になります。 次の図は、シーケンスを示しています。


[![](healthkit-images/image13.png "次の図は、イベントのシーケンスを示しています。")](healthkit-images/image13.png#lightbox)

**[レコード]** ボタンを押します。 これにより、 `StoreData_TouchUpInside()`ハンドラーが実行されます。これにより、 `heartRate`テキスト`HKQuantity`フィールドの値を解析し、前に説明`HeartRateModel.HeartRateInBeatsPerMinute()`した関数を使用して`HeartRateModel.StoreHeartRate()`に変換し、その数量をに渡します。 前に説明したように、これによりデータの格納が試行`HeartRateStored`さ`ErrorMessageChanged`れ、イベントまたはイベントが発生します。

デバイスの **ホーム** ボタンをダブルクリックし、Health app を開きます。 **[ソース]** タブをクリックすると、サンプルアプリが一覧表示されます。 これを選択し、ハートレートデータを更新する権限を許可しません。 **[ホーム]** ボタンをダブルクリックして、アプリに戻ります。 ここでも`ReactToHealthCarePermissions()` 、が呼び出されますが、今回はアクセスが拒否されたため、 **[storedata]** ボタンが無効になります (これは非同期に発生し、ユーザーインターフェイスの変更はエンドユーザーに表示される可能性があります)。

## <a name="advanced-topics"></a>高度なトピック

正常性キットデータベースからのデータの読み取りは、データの書き込みと非常によく似ています。1つは、アクセスしようとしているデータの種類を指定し、承認を要求します。承認が許可された場合は、互換性のある単位に自動的に変換されたデータを使用できます。措置.

述語ベースのクエリと、関連するデータが更新されたときに更新を実行するクエリを可能にする、より高度なクエリ関数がいくつかあります。 

正常性キットアプリケーションの開発者は、Apple の[アプリレビューガイドライン](https://developer.apple.com/app-store/review/guidelines/#healthkit)の「正常性キット」セクションを確認する必要があります。

セキュリティとタイプシステムモデルが認識されると、共有正常性キットデータベースにデータを格納して読み取ることが非常に簡単になります。 正常性キット内の関数の多くは非同期に動作し、アプリケーション開発者はプログラムを適切に記述する必要があります。

この記事の執筆時点では、現在、Android または Windows Phone の正常性キットに相当するものはありません。

## <a name="summary"></a>まとめ

この記事では、Health Kit を使用して、アプリケーションで正常性に関する情報を格納、取得、および共有できるようにすると同時に、ユーザーがこのデータにアクセスして制御できる標準の正常性アプリも提供する方法について説明しました。 

また、プライバシー、セキュリティ、およびデータの整合性が、正常性に関連する情報と、正常性キットを使用したアプリケーションでは、アプリケーション管理の側面 (プロビジョニング)、コーディング (正常性キットの種類) の複雑さの増加に対処する必要があることも確認しました。システム) とユーザーエクスペリエンス (システムダイアログとヘルスアプリを使用したアクセス許可のユーザー制御)。 

最後に、ハートビートデータを Health Kit ストアに書き込み、非同期対応のデザインを持つ、付属のサンプルアプリを使用して、正常性キットの単純な実装を見てきました。

## <a name="related-links"></a>関連リンク

- [HKWork (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-introtohealthkit)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
