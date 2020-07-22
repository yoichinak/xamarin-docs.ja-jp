---
title: Xamarin. iOS の HealthKit
description: このドキュメントでは、HealthKit について説明します。これは、iOS 8 で導入されたフレームワークであり、正常性に関連したデータストアを一元化、連携、セキュリティで保護します。 HealthKit アプリをプロビジョニングする方法と、HealthKit フレームワークを使用するコードを記述する方法について説明します。
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 8090cb3c694083be4ef12294799d6aadf26b6038
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569128"
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

[![](healthkit-images/image01.png "A sample application to record the users heart rate")](healthkit-images/image01.png#lightbox)

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
- `Entitlements.plist` `com.apple.developer.healthkit` 型のプロパティが `Boolean` に設定さ `Yes` れた。
- `Info.plist` `UIRequiredDeviceCapabilities` 値を持つエントリを格納しているキーを持つ `String` `healthkit` 。
- また、には、 `Info.plist` 適切なプライバシーに関する説明のエントリが必要です。 `String` `NSHealthUpdateUsageDescription` アプリがデータを書き込む場合のキーの説明と、 `String` `NSHealthShareUsageDescription` アプリが正常性キットデータを読み取る場合のキーの説明です。

IOS アプリのプロビジョニングの詳細については、Xamarin の**はじめに**シリーズの[デバイスプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)に関する記事で、開発者の証明書、アプリ id、プロビジョニングプロファイル、アプリの権利の関係について説明しています。

<a name="explicit-appid"></a>

### <a name="explicit-app-id-and-provisioning-profile"></a>明示的なアプリ ID とプロビジョニングプロファイル

明示的な**アプリ ID**と適切な**プロビジョニングプロファイル**の作成は、Apple の[iOS デベロッパーセンター](https://developer.apple.com/devcenter/ios/index.action)内で行われます。 

現在の**アプリ id**は、デベロッパーセンターの [ [Certificates, identifier & Profiles](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) ] セクション内に一覧表示されます。 多くの場合、この一覧にはの**id**値が表示されます。これは、 `*` **アプリ id**  -  **名**を任意の数のサフィックスと共に使用できることを示します。 このような*ワイルドカードアプリ id*を正常性キットと一緒に使用することはできません。

明示的な**アプリ id**を作成するには、右上のボタンをクリックして **+** 、[ **IOS アプリ id の登録**] ページに移動します。

[![](healthkit-images/image02.png "Registering an app on the Apple Developer Portal")](healthkit-images/image02.png#lightbox)

上の図に示されているように、アプリの説明を作成した後、[**明示的なアプリ id** ] セクションを使用してアプリケーションの id を作成します。 [ **App Services** ] セクションで、[**サービスの有効化**] セクションの [ **Health Kit** ] をオンにします。

完了したら、[**続行**] ボタンをクリックして、アカウントに**アプリ ID**を登録します。 [**証明書、識別子、およびプロファイル**] ページに戻ります。 [**プロビジョニングプロファイル**] をクリックして現在のプロビジョニングプロファイルの一覧に移動し、右上隅にあるボタンをクリックして [ **+** **IOS プロビジョニングプロファイルの追加**] ページに移動します。 [ **IOS アプリ開発**] オプションを選択し、[**続行**] をクリックして [**アプリ ID の選択**] ページに移動します。 ここでは、前に指定した明示的な**アプリ ID**を選択します。

[![](healthkit-images/image03.png "Select the explicit App ID")](healthkit-images/image03.png#lightbox)

[**続行**] をクリックして残りの画面を表示します。ここで、**開発者の証明書**、**デバイス**、およびこの**プロビジョニングプロファイル**の**名前**を指定します。

[![](healthkit-images/image04.png "Generating the Provisioning Profile")](healthkit-images/image04.png#lightbox)

[**生成**] をクリックして、プロファイルの作成を待機します。 ファイルをダウンロードし、ダブルクリックして Xcode にインストールします。 インストールされていることを確認するには、 **Xcode > の [基本設定] > アカウント > [詳細の表示...] を使用します。** インストール済みのプロビジョニングプロファイルが表示され、[**権利**] 行に [Health Kit] とその他の特別なサービスのアイコンが表示されます。

[![](healthkit-images/image05.png "Viewing the profile in Xcode")](healthkit-images/image05.png#lightbox)

<a name="associating-appid"></a>

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>アプリ ID とプロビジョニングプロファイルを Xamarin iOS アプリに関連付ける

説明に従って適切な**プロビジョニングプロファイル**を作成してインストールしたら、通常は Visual Studio for Mac または Visual Studio でソリューションを作成します。 ヘルスキットのアクセスは、すべての iOS C# または F # プロジェクトで利用できます。

Xamarin iOS 8 プロジェクトを手動で作成するプロセスを実行するのではなく、この記事に添付されているサンプルアプリ (ストーリーボードとコードを含む) を開きます。 サンプルアプリを正常性キットが有効になっている**プロビジョニングプロファイル**に関連付けるには、 **Solution Pad**でプロジェクトを右クリックし、[**オプション**] ダイアログを表示します。 **IOS アプリケーション**パネルに切り替え、以前にアプリの**バンドル識別子**として作成した明示的な**アプリ ID**を入力します。

[![](healthkit-images/image06.png "Enter the explicit App ID")](healthkit-images/image06.png#lightbox)

次に、[ **IOS バンドル署名**] パネルに切り替えます。 最近インストールされた**プロビジョニングプロファイル**は、明示的な**アプリ ID**に関連付けられており、**プロビジョニングプロファイル**として使用できるようになりました。

[![](healthkit-images/image07.png "Select the Provisioning Profile")](healthkit-images/image07.png#lightbox)

**プロビジョニングプロファイル**を使用できない場合は、Ios**デベロッパーセンター**で指定されているものとは、Ios**アプリケーション**パネルで**バンドル Id**を再確認し、**プロビジョニングプロファイル**がインストールされていることを確認します (**Xcode > 設定 > アカウント > 詳細の表示...**)。

[正常性キット対応の**プロビジョニングプロファイル**] を選択したら、[ **OK** ] をクリックして [プロジェクトオプション] ダイアログボックスを閉じます。

### <a name="entitlementsplist-and-infoplist-values"></a>権利の plist と情報 plist の値

このサンプルアプリには、 `Entitlements.plist` (正常性キットが有効になっているアプリに必要な) ファイルが含まれており、すべてのプロジェクトテンプレートには含まれていません。 プロジェクトに権利が含まれていない場合は、プロジェクトを右クリックし、[**ファイル > 新しいファイル]、[> iOS >** ]、[plist] の順に選択して手動で追加します。

最終的に、には `Entitlements.plist` 次のキーと値のペアが必要です。

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

同様に、 `Info.plist` アプリのの値は `healthkit` キーに関連付けられている必要があり `UIRequiredDeviceCapabilities` ます。

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

この記事で提供するサンプルアプリケーションには、 `Entitlements.plist` 必要なすべてのキーを含む事前構成済みのが含まれています。

<a name="programming"></a>

## <a name="programming-health-kit"></a>正常性キットのプログラミング

正常性キットのデータストアは、アプリ間で共有されるプライベートなユーザー固有のデータストアです。 ヘルス情報が非常に重要であるため、ユーザーはデータアクセスを許可するために正の手順を実行する必要があります。 このアクセスは部分的なものである場合があります (読み取りはできず、一部の種類のデータにアクセスできますが、その他はアクセスできません)。また、いつでも取り消すことができます。 正常性キットアプリケーションは防御に記述する必要があります。多くのユーザーは、正常性に関連する情報の保存にためらいされることを理解しています。

ヘルスキットデータは、Apple が指定した種類に限定されます。 これらの型は厳密に定義されています。血液型などの一部は、Apple が提供する列挙体の特定の値に制限されますが、他の型は測定単位 (グラム、calories、リットルなど) を持つ大きさを組み合わせたものです。 互換性のある測定単位を共有するデータであっても、によって識別されます `HKObjectType` 。たとえば、型システムでは、 `HKQuantityTypeIdentifier.NumberOfTimesFallen` 両方と `HKQuantityTypeIdentifier.FlightsClimbed` も測定単位が使用されていても、を想定しているフィールドに値を格納しようとする誤った試行をキャッチします。 `HKUnit.Count`

正常性キットデータストアの格納できる型は、のすべてのサブクラスです `HKObjectType` 。 `HKCharacteristicType`オブジェクトには、生物性別、血液の種類、生年月日が格納されます。 ただし、一般的には、 `HKSampleType` 特定の時間または一定期間にサンプリングされるデータを表すオブジェクトです。 

[![](healthkit-images/image08.png "HKSampleType objects chart")](healthkit-images/image08.png#lightbox)

`HKSampleType`は abstract であり、4つの具象サブクラスを持ちます。 現在、スリープ分析であるデータの種類は1つだけです `HKCategoryType` 。 Health Kit のデータの大部分は型で、 `HKQuantityType` オブジェクトにデータを格納し `HKQuantitySample` ます。オブジェクトは、使い慣れたファクトリデザインパターンを使用して作成されます。

[![](healthkit-images/image09.png "The large majority of data in Health Kit are of type HKQuantityType and store their data in HKQuantitySample objects")](healthkit-images/image09.png#lightbox)

`HKQuantityType`型の範囲 `HKQuantityTypeIdentifier.ActiveEnergyBurned` はから `HKQuantityTypeIdentifier.StepCount` です。 

<a name="requesting-permission"></a>

### <a name="requesting-permission-from-the-user"></a>ユーザーにアクセス許可を要求しています

エンドユーザーは、アプリが正常性キットデータの読み取りまたは書き込みを行えるようにするために、正の手順を実行する必要があります。 これは、iOS 8 デバイスにプレインストールされている正常性アプリを使用して行います。 正常性キットアプリを初めて実行すると、システムによって管理される**正常性のアクセス**ダイアログが表示されます。

[![](healthkit-images/image10.png "The user is presented with a system-controlled Health Access dialog")](healthkit-images/image10.png#lightbox)

その後、ユーザーは、ヘルスアプリの [**ソース**] ダイアログボックスを使用してアクセス許可を変更できます。

[![](healthkit-images/image11.png "The user can change permissions using Health apps Sources dialog")](healthkit-images/image11.png#lightbox)

正常性の情報は非常に機密性が高いため、アプリの開発者は、アプリの実行中にアクセス許可が拒否され、変更されることを見込んで、プログラムを防御に記述する必要があります。 最も一般的な表現は、メソッドでアクセス許可を要求 `UIApplicationDelegate.OnActivated` し、必要に応じてユーザーインターフェイスを変更することです。

### <a name="permissions-walkthrough"></a>アクセス許可のチュートリアル

正常性キットによってプロビジョニングされたプロジェクトで、ファイルを開き `AppDelegate.cs` ます。 ファイルの先頭にある; を使用して、ステートメントに注意して `HealthKit` ください。

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

これらのメソッドのコードはすべてインラインで実行でき `OnActivated` ますが、サンプルアプリでは、意図した方法を明確にするために個別のメソッドを使用しています。は、 `ValidateAuthorization()` 記述されている特定の型へのアクセスを要求し (アプリが必要な場合は読み取り)、 `ReactToHealthCarePermissions()` ユーザーが正常性アプリの [アクセス許可] ダイアログを操作した

のジョブで `ValidateAuthorization()` は、アプリが書き込むセットを作成 `HKObjectTypes` し、そのデータを更新するための承認を要求します。 サンプルアプリでは、は `HKObjectType` キーを対象としてい `KHQuantityTypeIdentifierKey.HeartRate` ます。 この型はセットに追加され、 `typesToWrite` セットは `typesToRead` 空のままになります。 これらのセット、およびコールバックへの参照 `ReactToHealthCarePermissions()` は、に渡され `HKHealthStore.RequestAuthorizationToShare()` ます。

`ReactToHealthCarePermissions()`コールバックは、ユーザーがアクセス許可ダイアログを操作した後に呼び出され、2つの情報が渡されます。これは、ユーザーがアクセス許可ダイアログを操作した場合の値です。 `bool` また、 `true` `NSError` null 以外の場合は、アクセス許可ダイアログの表示に関連する何らかのエラーを示します。

> [!IMPORTANT]
> この関数の引数を明確にするために、 _success_および_error_パラメーターは、ユーザーが正常性キットデータへのアクセス許可を付与したかどうかを示しません。 ユーザーにデータへのアクセスを許可する機会が与えられていることだけを示します。

アプリがデータにアクセスできるかどうかを確認するには、を使用してを `HKHealthStore.GetAuthorizationStatus()` 渡し `HKQuantityTypeIdentifierKey.HeartRate` ます。 返された状態に基づいて、アプリはデータを入力する機能を有効または無効にします。 アクセス拒否に対処するための標準的なユーザーエクスペリエンスはありません。また、多くのオプションがあります。 この例のアプリでは、シングルトンオブジェクトに状態が設定され、 `HeartRateModel` さらに関連するイベントが発生します。

## <a name="model-view-and-controller"></a>モデル、ビュー、およびコントローラー

シングルトンオブジェクトを確認するには、次のように `HeartRateModel` ファイルを開き `HeartRateModel.cs` ます。

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

最初のセクションは、汎用イベントとハンドラーを作成するための定型コードです。 クラスの最初の部分は、 `HeartRateModel` スレッドセーフなシングルトンオブジェクトを作成するための定型的な部分でもあります。

次に、は `HeartRateModel` 3 つのイベントを公開します。 

- `EnabledChanged`-ハートレートストレージが有効になっているか無効になっていることを示します (ストレージが最初に無効になっていることに注意してください)。 
- `ErrorMessageChanged`-このサンプルアプリでは、非常に単純なエラー処理モデル (最後のエラーを含む文字列) が用意されています。 
- `HeartRateStored`-ハートレートが正常性キットデータベースに格納されている場合に発生します。

これらのイベントが発生するたびに、によって実行されるので、 `NSObject.InvokeOnMainThread()` サブスクライバーは UI を更新できます。 または、イベントがバックグラウンドスレッドで発生しているとして文書化されていて、互換性を確認する必要があるかどうかを、ハンドラーに委ねておく必要があります。 権限要求などの多くの関数は非同期であり、メイン以外のスレッドでコールバックを実行するため、正常性キットアプリケーションではスレッドに関する考慮事項が重要です。

のヘルスキット固有のコード `HeartRateModel` は、との2つの関数に含まれて `HeartRateInBeatsPerMinute()` `StoreHeartRate()` います。 

`HeartRateInBeatsPerMinute()`引数を厳密に型指定された正常性キットに変換 `HKQuantity` します。 によって指定された数量の種類 `HKQuantityTypeIdentifierKey.HeartRate` と数量の単位が `HKUnit.Count` によって分割され `HKUnit.Minute` ます (つまり、単位は 1*分あたり拍*です)。 

`StoreHeartRate()`関数は `HKQuantity` (サンプルアプリでは、によって作成された) を受け取り `HeartRateInBeatsPerMinute()` ます。 データを検証するには、メソッドを使用します。これは、 `HKQuantity.IsCompatible()` `true` オブジェクトの単位を引数の単位に変換できる場合にを返します。 こので数量が作成された場合 `HeartRateInBeatsPerMinute()` は、が返されますが、たとえば、 `true` `true` *1 時間あたりの拍*数として数量が作成された場合にもが返されます。 より一般的には、 `HKQuantity.IsCompatible()` ユーザーまたはデバイスが1つの測定システム (ヤード単位など) で入力または表示しても、別のシステム (メトリックユニットなど) に格納されている可能性がある、質量、距離、およびエネルギーを検証するために使用できます。 

数量の互換性が検証されると、ファクトリメソッドを使用して、 `HKQuantitySample.FromType()` 厳密に型指定されたオブジェクトが作成され `heartRateSample` ます。 `HKSample`オブジェクトには開始日と終了日があります。瞬間的に読み取れるように、これらの値は、例に示すように同じである必要があります。 また、このサンプルでは、引数にキー値データを設定しません `HKMetadata` が、次のコードのようなコードを使用してセンサーの場所を指定することもできます。

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

`heartRateSample`が作成されると、コードは using ブロックを使用してデータベースへの新しい接続を作成します。 このブロック内で、 `HKHealthStore.SaveObject()` メソッドはデータベースへの非同期書き込みを試行します。 結果として得られるラムダ式への呼び出しでは、またはのいずれかの関連イベントがトリガーされ `HeartRateStored` `ErrorMessageChanged` ます。

モデルがプログラミングされたので、次は、コントローラーがモデルの状態を反映していることを確認します。 ファイルを開き `HKWorkViewController.cs` ます。 コンストラクターは単純 `HeartRateModel` にシングルトンをイベント処理メソッドに配線します (この場合も、ラムダ式を使用してインラインで実行できますが、個別のメソッドにより明確になります)。

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

プロビジョニングが適切に設定されていると仮定すると、アプリケーションが起動します。 そのメソッドに到達すると `OnActivated` 、正常性キットの承認を要求します。 オペレーティングシステムによって初めて検出された場合、ユーザーには次のダイアログが表示されます。

[![](healthkit-images/image12.png "The user will be presented with this dialog")](healthkit-images/image12.png#lightbox)

アプリでハートレートデータを更新できるようにします。アプリは再び表示されます。 `ReactToHealthCarePermissions`コールバックは非同期にアクティブ化されます。 これにより、プロパティが変更され、イベントが発生します。これにより、イベントハンドラーが実行され、 `HeartRateModel’s` `Enabled` `EnabledChanged` `HKPermissionsViewController.OnEnabledChanged()` ボタンが有効になり `StoreData` ます。 次の図は、シーケンスを示しています。

[![](healthkit-images/image13.png "This diagram shows the sequence of events")](healthkit-images/image13.png#lightbox)

[**レコード**] ボタンを押します。 これにより、ハンドラーが実行されます。これにより、 `StoreData_TouchUpInside()` テキストフィールドの値を解析し、 `heartRate` 前に説明した関数を使用してに変換し、 `HKQuantity` `HeartRateModel.HeartRateInBeatsPerMinute()` その数量をに渡し `HeartRateModel.StoreHeartRate()` ます。 前に説明したように、これによりデータの格納が試行され、 `HeartRateStored` イベントまたはイベントが発生し `ErrorMessageChanged` ます。

デバイスの [**ホーム**] ボタンをダブルクリックし、[Health app] を開きます。 [**ソース**] タブをクリックすると、サンプルアプリが一覧表示されます。 これを選択し、ハートレートデータを更新する権限を許可しません。 [**ホーム**] ボタンをダブルクリックして、アプリに戻ります。 ここでも、が呼び出されますが、今回はアクセスが拒否されたため、[ `ReactToHealthCarePermissions()` **storedata** ] ボタンが無効になります (これは非同期に発生し、ユーザーインターフェイスの変更はエンドユーザーに表示される可能性があります)。

## <a name="advanced-topics"></a>高度なトピック

正常性キットデータベースからのデータの読み取りは、データの書き込みと非常によく似ています。1つは、アクセスしようとしているデータの種類を指定し、承認を要求します。承認が許可された場合は、互換性のある測定単位への自動変換を使用してデータを使用できます。

述語ベースのクエリと、関連するデータが更新されたときに更新を実行するクエリを可能にする、より高度なクエリ関数がいくつかあります。 

正常性キットアプリケーションの開発者は、Apple の[アプリレビューガイドライン](https://developer.apple.com/app-store/review/guidelines/#healthkit)の「正常性キット」セクションを確認する必要があります。

セキュリティとタイプシステムモデルが認識されると、共有正常性キットデータベースにデータを格納して読み取ることが非常に簡単になります。 正常性キット内の関数の多くは非同期に動作し、アプリケーション開発者はプログラムを適切に記述する必要があります。

この記事の執筆時点では、現在、Android または Windows Phone の正常性キットに相当するものはありません。

## <a name="summary"></a>まとめ

この記事では、Health Kit を使用して、アプリケーションで正常性に関する情報を格納、取得、および共有できるようにすると同時に、ユーザーがこのデータにアクセスして制御できる標準の正常性アプリも提供する方法について説明しました。 

また、プライバシー、セキュリティ、およびデータの整合性が、正常性に関連する情報を上書きしていることと、正常性キットを使用するアプリケーションでは、アプリケーション管理の側面 (プロビジョニング)、コーディング (正常性キットの型システム)、およびユーザーエクスペリエンス (システムダイアログとヘルスアプリを使用したアクセス許可のユーザー制御) の複雑さの増加に対処する 

最後に、ハートビートデータを Health Kit ストアに書き込み、非同期対応のデザインを持つ、付属のサンプルアプリを使用して、正常性キットの単純な実装を見てきました。

## <a name="related-links"></a>関連リンク

- [HKWork (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-introtohealthkit)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
