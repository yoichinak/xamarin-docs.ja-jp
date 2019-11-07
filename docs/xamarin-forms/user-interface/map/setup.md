---
title: Xamarin. フォームマップの初期化と構成
description: アプリケーションで maps 機能を使用するには、Xamarin. Forms. Map NuGet パッケージが必要です。 さらに、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が必要です。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/06/2019
ms.openlocfilehash: 038ff27907573c1fe15516f6f4caf26d0892ab9f
ms.sourcegitcommit: 283810340de5310f63ef7c3e4b266fe9dc2ffcaf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73662342"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin. フォームマップの初期化と構成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、各プラットフォームでネイティブマップコントロールを使用します。 これにより、ユーザーのための高速で使い慣れたマップエクスペリエンスが提供されますが、各プラットフォーム API の要件に準拠するためにはいくつかの構成手順が必要になります。

## <a name="map-initialization"></a>マップの初期化

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、ソリューション内のすべてのプロジェクトに追加する必要がある、 [Xamarin. Forms. map](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet パッケージによって提供されます。

[Xamarin.Forms.Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet パッケージをインストールした後は、各プラットフォームプロジェクトで初期化する必要があります。

IOS では、これは、`Xamarin.Forms.Forms.Init` メソッドの*後*に `Xamarin.FormsMaps.Init` メソッドを呼び出すことによって**AppDelegate.cs**で発生する必要があります。

```csharp
Xamarin.FormsMaps.Init();
```

Android では、これは、`Xamarin.Forms.Forms.Init` メソッドの*後*に `Xamarin.FormsMaps.Init` メソッドを呼び出すことによって**MainActivity.cs**で発生する必要があります。

```csharp
Xamarin.FormsMaps.Init(this, savedInstanceState);
```

ユニバーサル Windows プラットフォーム (UWP) では、`MainPage` コンストラクターから `Xamarin.FormsMaps.Init` メソッドを呼び出すことによって、 **MainPage.xaml.cs**でこれを行う必要があります。

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

UWP に必要な認証トークンの詳細については、「[ユニバーサル Windows プラットフォーム](#universal-windows-platform)」を参照してください。

NuGet パッケージが追加され、各アプリケーション内部で初期化メソッドが呼び出されると、共有コードプロジェクトで `Xamarin.Forms.Maps` API を使用できるようになります。

## <a name="platform-configuration"></a>プラットフォームの構成

Android では、マップを表示する前に、追加の構成が必要になります。これは、ユニバーサル Windows プラットフォーム (UWP) です。 さらに、iOS、Android、UWP では、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が付与されている必要があります。

### <a name="ios"></a>iOS

iOS でマップを表示して操作する場合、追加の構成は必要ありません。 ただし、ロケーションサービスにアクセスするには、次のキーを **info.plist** で設定する必要があります。

- iOS 11 以降
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) –アプリケーションが使用されているときにロケーションサービスを使用する場合
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) –位置情報サービスを常に使用する場合
- iOS 10 以前
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) –アプリケーションが使用されているときにロケーションサービスを使用する場合
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) –位置情報サービスを常に使用する場合    

iOS 11 以前をサポートするには、`NSLocationWhenInUseUsageDescription`、`NSLocationAlwaysAndWhenInUseUsageDescription`、`NSLocationAlwaysUsageDescription` の 3 つのキーすべてを含めることができます。

次に、これらのキーの XML 表現を **info.plist** で示します。 アプリケーションが場所情報をどのように使用しているかを反映するように、`string` の値を更新する必要があります。

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your application is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

また **、情報の plist ファイルを**編集しているときに、**ソース**ビューで**情報**を追加することもできます。

![IOS 8 用情報 plist](setup-images/ios8-map-permissions.png "iOS 8 必須情報. plist エントリ")

アプリケーションがユーザーの場所にアクセスしようとしてアクセスを要求したときに、プロンプトが表示されます。

[![IOS での場所のアクセス許可要求のスクリーンショット](setup-images/permission-ios.png "iOS アクセス許可要求")](setup-images/permission-ios-large.png#lightbox "iOS アクセス許可要求")

### <a name="android"></a>Android

Android でマップを表示して操作するための構成プロセスは次のとおりです。

1. Google Maps API キーを取得し、マニフェストに追加します。
1. マニフェストで Google Play services のバージョン番号を指定します。
1. マニフェストで Apache HTTP レガシライブラリの要件を指定します。
1. [任意] マニフェストで WRITE_EXTERNAL_STORAGE アクセス許可を指定します。
1. [任意] マニフェストでの場所のアクセス許可を指定します。
1. [任意] `MainActivity` クラスでランタイムの場所のアクセス許可を要求します。

正しく構成されたマニフェストファイルの例については、サンプルアプリケーションの「 [AndroidManifest.xml](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/WorkingWithMaps.Android/Properties/AndroidManifest.xml) 」を参照してください。

#### <a name="get-a-google-maps-api-key"></a>Google Maps API キーを取得する

Android で[Google Maps API](https://developers.google.com/maps/documentation/android/) を使用するには、API キーを生成する必要があります。 これを行うには、「 [Google Maps API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)」の手順に従います。

API キーを取得したら、 **Properties/AndroidManifest.xml** ファイルの `<application>` 要素内に追加する必要があります。

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="PASTE-YOUR-API-KEY-HERE" />
</application>
```

これにより、API キーがマニフェストに埋め込まれます。 有効な API キーがない場合、 [`Map`](xref:Xamarin.Forms.Maps.Map)コントロールには空のグリッドが表示されます。

> [!NOTE]
> APK が Google Maps にアクセスできるようにするには、APK に署名するために使用するすべてのキーストア (デバッグとリリース) に SHA-1 指紋とパッケージ名を含める必要があることに注意してください。 たとえば、デバッグに1台のコンピューターを使用し、リリース APK を生成する別のコンピューターを使用する場合は、最初のコンピューターのデバッグキーストアから SHA-1 証明書のフィンガープリントを指定し、次のリリースキーストアから SHA-1 証明書のフィンガープリントを含める必要があります。2番目のコンピューター。 また、アプリの**パッケージ名**が変更された場合は、キーの資格情報を編集することも忘れないでください。 「 [Google MAPS API キーを取得する」を](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)参照してください。

#### <a name="specify-the-google-play-services-version-number"></a>Google Play services のバージョン番号を指定する

**AndroidManifest.xml** の `<application>` 要素内に次の宣言を追加します。

```xml
<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
```

これにより、アプリケーションがコンパイルされた Google Play サービスのバージョンがマニフェストに埋め込まれます。

#### <a name="specify-the-requirement-for-the-apache-http-legacy-library"></a>Apache HTTP レガシライブラリの要件を指定する

Xamarin アプリケーションが API 28 以上を対象としている場合は、 **AndroidManifest.xml** の `<application>` 要素内に次の宣言を追加する必要があります。

```xml
<uses-library android:name="org.apache.http.legacy" android:required="false" />    
```

これにより、Android 9 の `bootclasspath` から削除された Apache Http クライアントライブラリを使用するようにアプリケーションに指示します。

#### <a name="specify-the-write_external_storage-permission"></a>WRITE_EXTERNAL_STORAGE アクセス許可を指定する

アプリケーションが API 22 以下を対象としている場合は、`<manifest>` 要素の子として、マニフェストに `WRITE_EXTERNAL_STORAGE` のアクセス許可を追加する必要がある場合があります。

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

これは、アプリケーションが API 23 以上を対象としている場合は必要ありません。

#### <a name="specify-location-permissions"></a>場所のアクセス許可を指定する

アプリケーションがユーザーの場所にアクセスする必要がある場合は、`<manifest>` 要素の子として、マニフェスト (またはその両方) に `ACCESS_COARSE_LOCATION` または `ACCESS_FINE_LOCATION` のアクセス許可を追加することにより、アクセス許可を要求する必要があります。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.companyname.myapp">
  ...
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>
```

`ACCESS_COARSE_LOCATION` のアクセス許可により、API は、デバイスの場所を特定するために、WiFi またはモバイルデータ、またはその両方を使用できます。 `ACCESS_FINE_LOCATION` のアクセス許可により、API は、Global Positioning System (GPS)、WiFi、またはモバイルデータを使用して、可能な限り正確な場所を判断できます。

または、マニフェストエディターを使用して次のアクセス許可を追加することによって、これらのアクセス許可を有効にすることもできます。

- `AccessCoarseLocation`
- `AccessFineLocation`

これらは次のスクリーンショットに示されています。

![Android に必要なアクセス許可](setup-images/android-map-permissions.png "Android に必要なアクセス許可")

#### <a name="request-runtime-location-permissions"></a>ランタイムの場所のアクセス許可を要求する

アプリケーションが API 23 以降を対象としていて、ユーザーの場所にアクセスする必要がある場合は、実行時に必要なアクセス許可があるかどうかを確認し、存在しない場合は要求する必要があります。 これは次のようにして実装します。

1. `MainActivity` クラスに、次のフィールドを追加します。

    ```csharp
    const int RequestLocationId = 0;

    readonly string[] LocationPermissions =
    {
        Manifest.Permission.AccessCoarseLocation,
        Manifest.Permission.AccessFineLocation
    };
    ```

1. `MainActivity` クラスで、次の `OnStart` オーバーライドを追加します。

    ```csharp
    protected override void OnStart()
    {
        base.OnStart();

        if ((int)Build.VERSION.SdkInt >= 23)
        {
            if (CheckSelfPermission(Manifest.Permission.AccessFineLocation) != Permission.Granted)
            {
                RequestPermissions(LocationPermissions, RequestLocationId);
            }
            else
            {
                // Permissions already granted - display a message.
            }
        }
    }
    ```

    アプリケーションが API 23 以上を対象としている場合、このコードは、`AccessFineLocation` のアクセス許可に対して実行時のアクセス許可チェックを実行します。 アクセス許可が付与されていない場合は、`RequestPermissions` メソッドを呼び出すことによってアクセス許可要求が行われます。

1. `MainActivity` クラスで、次の `OnRequestPermissionsResult` オーバーライドを追加します。

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Permission[] grantResults)
    {
        if (requestCode == RequestLocationId)
        {
            if ((grantResults.Length == 1) && (grantResults[0] == (int)Permission.Granted))
                // Permissions granted - display a message.
            else
                // Permissions denied - display a message.
        }
        else
        {
            base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
    ```

    このオーバーライドは、アクセス許可要求の結果を処理します。

このコードの全体的な影響として、アプリケーションがユーザーの場所を要求すると、次のダイアログが表示され、アクセス許可を要求します。

[![Android での location アクセス許可要求のスクリーンショット](setup-images/permission-android.png "Android アクセス許可要求")](setup-images/permission-android-large.png#lightbox "Android アクセス許可要求")

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

UWP では、マップを表示してマップサービスを使用する前に、アプリケーションを認証する必要があります。 アプリケーションを認証するには、maps 認証キーを指定する必要があります。 詳細については、「 [Request a maps authentication key](/windows/uwp/maps-and-location/authentication-key)」を参照してください。 その後、`FormsMaps.Init("AUTHORIZATION_TOKEN")` メソッドの呼び出しで認証トークンを指定し、Bing Maps でアプリケーションを認証する必要があります。

また、アプリケーションがユーザーの場所にアクセスする必要がある場合は、パッケージマニフェストで場所の機能を有効にする必要があります。 これは次のようにして実装します。

1. **ソリューションエクスプローラー**で、 **package.appxmanifest**をダブルクリックし、 **[機能]** タブを選択します。
1. **[機能]** の一覧で、 **[場所]** のチェックボックスをオンにします。 これにより、`location` デバイス機能がパッケージマニフェストファイルに追加されます。

    ```xml
    <Capabilities>
      <!-- DeviceCapability elements must follow Capability elements (if present) -->
      <DeviceCapability Name="location"/>
    </Capabilities>
    ```

#### <a name="release-builds"></a>リリースビルド

UWP リリースビルドでは、.NET ネイティブコンパイルを使用して、アプリケーションを直接ネイティブコードにコンパイルします。 ただし、このような結果として、UWP の[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールのレンダラーが実行可能ファイルからリンクされている可能性があります。 これは、 **App.xaml.cs**の `Forms.Init` メソッドの UWP 固有のオーバーロードを使用して修正できます。

```csharp
var assembliesToInclude = new [] { typeof(Xamarin.Forms.Maps.UWP.MapRenderer).GetTypeInfo().Assembly };
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
```

このコードは、`Xamarin.Forms.Maps.UWP.MapRenderer` クラスが存在するアセンブリを `Forms.Init` メソッドに渡します。 これにより、.NET ネイティブコンパイルプロセスによってアセンブリが実行可能ファイルからリンクされなくなります。

> [!IMPORTANT]
> この操作を行わないと、リリースビルドの実行時に[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールが表示されなくなります。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Forms.Maps のピン](~/xamarin-forms/user-interface/map/pins.md)
- [Maps API](xref:Xamarin.Forms.Maps)
- [マップカスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
