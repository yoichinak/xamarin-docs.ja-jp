---
title: " Xamarin.Forms マップの初期化と構成" の説明: "the Xamarin.Forms .Maps NuGet パッケージは、アプリケーションで maps 機能を使用するために必要です。 さらに、ユーザーの場所にアクセスするには、アプリケーションに対して場所のアクセス許可が付与されている必要があります。 "
ms. 製品: xamarin ms. assetid: 59CD1344-8248-406C9144-0c8a67141e: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 02/07/2020 no loc: [ Xamarin.Forms ,、 Xamarin.Essentials ]
---

# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin.Formsマップの初期化と構成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

コントロールは、 [`Map`](xref:Xamarin.Forms.Maps.Map) 各プラットフォームでネイティブマップコントロールを使用します。 これにより、ユーザーのための高速で使い慣れたマップエクスペリエンスが提供されますが、各プラットフォーム API の要件に準拠するためにはいくつかの構成手順が必要になります。

## <a name="map-initialization"></a>マップの初期化

[`Map`](xref:Xamarin.Forms.Maps.Map)コントロールは、によって提供され[ Xamarin.Forms ます。](https://www.nuget.org/packages/Xamarin.Forms.Maps/)ソリューション内のすべてのプロジェクトに追加する必要がある、NuGet パッケージをマップします。

をインストールした後[ Xamarin.Forms 。マップ](https://www.nuget.org/packages/Xamarin.Forms.Maps/)NuGet パッケージは、各プラットフォームプロジェクトで初期化する必要があります。

IOS では、メソッドの後にメソッドを呼び出すことによって、 **AppDelegate.cs**でこれを行う必要があり `Xamarin.FormsMaps.Init` *after* `Xamarin.Forms.Forms.Init` ます。

```csharp
Xamarin.FormsMaps.Init();
```

Android では、メソッドの後にメソッドを呼び出すことによって、 **MainActivity.cs**でこれを行う必要があり `Xamarin.FormsMaps.Init` *after* `Xamarin.Forms.Forms.Init` ます。

```csharp
Xamarin.FormsMaps.Init(this, savedInstanceState);
```

ユニバーサル Windows プラットフォーム (UWP) では、コンストラクターからメソッドを呼び出すことによって**MainPage.xaml.cs**でこれを行う必要があり `Xamarin.FormsMaps.Init` `MainPage` ます。

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

UWP に必要な認証トークンの詳細については、「[ユニバーサル Windows プラットフォーム](#universal-windows-platform)」を参照してください。

NuGet パッケージが追加され、各アプリケーション内部で初期化メソッドが呼び出されると、 `Xamarin.Forms.Maps` 共有コードプロジェクトで api を使用できるようになります。

## <a name="platform-configuration"></a>プラットフォームの構成

Android では、マップを表示する前に、追加の構成が必要になります。これは、ユニバーサル Windows プラットフォーム (UWP) です。 さらに、iOS、Android、UWP では、ユーザーの場所にアクセスするには、アプリケーションに対する場所のアクセス許可が付与されている必要があります。

### <a name="ios"></a>iOS

IOS でマップを表示して操作する場合、追加の構成は必要ありません。 ただし、ロケーションサービスにアクセスするには、次のキーを**情報 plist**で設定する必要があります。

- iOS 11 以降
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)–アプリケーションの使用時にロケーションサービスを使用する場合
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationalwaysandwheninuseusagedescription)–常にロケーションサービスを使用する場合
- iOS 10 以前
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)–アプリケーションの使用時にロケーションサービスを使用する場合
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18)–常にロケーションサービスを使用する場合    

IOS 11 以前をサポートするには、、、およびの3つのキーすべてを含めることができ `NSLocationWhenInUseUsageDescription` `NSLocationAlwaysAndWhenInUseUsageDescription` `NSLocationAlwaysUsageDescription` ます。

次に、これらのキーの XML 表現を**情報 plist**で示します。 `string`アプリケーションが場所情報をどのように使用しているかを反映するように値を更新する必要があります。

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
1. optionalマニフェストで WRITE_EXTERNAL_STORAGE アクセス許可を指定します。
1. optionalマニフェストでの場所のアクセス許可を指定します。
1. optionalクラスのランタイムの場所のアクセス許可を要求 `MainActivity` します。

正しく構成されたマニフェストファイルの例については、サンプルアプリケーションの「 [AndroidManifest.xml](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/WorkingWithMaps.Android/Properties/AndroidManifest.xml) 」を参照してください。

#### <a name="get-a-google-maps-api-key"></a>Google Maps API キーを取得する

Android で[Google MAPS api](https://developers.google.com/maps/documentation/android/)を使用するには、API キーを生成する必要があります。 これを行うには、「 [Google MAPS API キーを取得](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)する」の手順に従います。

API キーを取得したら、 `<application>` **Properties/AndroidManifest.xml**ファイルの要素内に追加する必要があります。

```xml
<application ...>
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="PASTE-YOUR-API-KEY-HERE" />
</application>
```

これにより、API キーがマニフェストに埋め込まれます。 有効な API キーがない場合、コントロールには [`Map`](xref:Xamarin.Forms.Maps.Map) 空のグリッドが表示されます。

> [!NOTE]
> `com.google.android.geo.API_KEY`は、API キーの推奨されるメタデータ名です。 旧バージョンとの互換性のために、 `com.google.android.maps.v2.API_KEY` メタデータ名を使用できますが、Android MAPS API v2 での認証のみが許可されます。

APK が Google Maps にアクセスするには、APK に署名するために使用するすべてのキーストア (デバッグとリリース) に SHA-1 の指紋名とパッケージ名を含める必要があります。 たとえば、デバッグに1台のコンピューターを使用し、リリース APK を生成する別のコンピューターを使用する場合は、最初のコンピューターのデバッグキーストアから SHA-1 証明書のフィンガープリントを、2番目のコンピューターのリリースキーストアから SHA-1 証明書のフィンガープリントを含める必要があります。 また、アプリの**パッケージ名**が変更された場合は、キーの資格情報を編集することも忘れないでください。 「 [Google MAPS API キーを取得する」を](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)参照してください。

#### <a name="specify-the-google-play-services-version-number"></a>Google Play services のバージョン番号を指定してください

AndroidManifest.xmlの要素内に次の宣言を追加し `<application>` ます。 ** **

```xml
<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
```

これにより、アプリケーションがコンパイルされた Google Play サービスのバージョンがマニフェストに埋め込まれます。

#### <a name="specify-the-requirement-for-the-apache-http-legacy-library"></a>Apache HTTP レガシライブラリの要件を指定します

アプリケーションが Xamarin.Forms API 28 以上を対象としている場合は、AndroidManifest.xmlの要素内に次の宣言を追加する必要があり `<application>` ます。 ** **

```xml
<uses-library android:name="org.apache.http.legacy" android:required="false" />    
```

これにより、Android 9 でから削除された Apache Http クライアントライブラリを使用するようにアプリケーションに指示し `bootclasspath` ます。

#### <a name="specify-the-write_external_storage-permission"></a>WRITE_EXTERNAL_STORAGE のアクセス許可を指定する

アプリケーションが API 22 以下を対象としている場合は、 `WRITE_EXTERNAL_STORAGE` 要素の子としてマニフェストにアクセス許可を追加する必要があり `<manifest>` ます。

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

これは、アプリケーションが API 23 以上を対象としている場合は必要ありません。

#### <a name="specify-location-permissions"></a>場所のアクセス許可の指定

アプリケーションでユーザーの場所にアクセスする必要がある場合 `ACCESS_COARSE_LOCATION` は、 `ACCESS_FINE_LOCATION` 要素の子として、またはのアクセス許可をマニフェスト (またはその両方) に追加することによって、アクセス許可を要求する必要があり `<manifest>` ます。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.companyname.myapp">
  ...
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>
```

この `ACCESS_COARSE_LOCATION` アクセス許可によって、API は、デバイスの場所を特定するために、WiFi またはモバイルデータ (またはその両方) を使用できます。 アクセス許可によって、 `ACCESS_FINE_LOCATION` API は、Global Positioning System (GPS)、WiFi、またはモバイルデータを使用して、可能な限り正確な場所を判断できます。

または、マニフェストエディターを使用して次のアクセス許可を追加することによって、これらのアクセス許可を有効にすることもできます。

- `AccessCoarseLocation`
- `AccessFineLocation`

これらは次のスクリーンショットに示されています。

![Android に必要なアクセス許可](setup-images/android-map-permissions.png "Android に必要なアクセス許可")

#### <a name="request-runtime-location-permissions"></a>ランタイムの場所のアクセス許可を要求する

アプリケーションが API 23 以降を対象としていて、ユーザーの場所にアクセスする必要がある場合は、実行時に必要なアクセス許可があるかどうかを確認し、存在しない場合は要求する必要があります。 これは次のようにして実装します。

1. クラスに、 `MainActivity` 次のフィールドを追加します。

    ```csharp
    const int RequestLocationId = 0;

    readonly string[] LocationPermissions =
    {
        Manifest.Permission.AccessCoarseLocation,
        Manifest.Permission.AccessFineLocation
    };
    ```

1. クラスで `MainActivity` 、次のオーバーライドを追加し `OnStart` ます。

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

    アプリケーションが API 23 以上を対象としている場合、このコードは、アクセス許可に対するランタイムアクセス許可チェックを実行し `AccessFineLocation` ます。 アクセス許可が付与されていない場合は、メソッドを呼び出すことによってアクセス許可要求が行われ `RequestPermissions` ます。

1. クラスで `MainActivity` 、次のオーバーライドを追加し `OnRequestPermissionsResult` ます。

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

UWP では、マップを表示してマップサービスを使用する前に、アプリケーションを認証する必要があります。 アプリケーションを認証するには、maps 認証キーを指定する必要があります。 詳細については、「 [Request a maps authentication key](/windows/uwp/maps-and-location/authentication-key)」を参照してください。 その後、メソッドの呼び出しで認証トークンを指定し、 `FormsMaps.Init("AUTHORIZATION_TOKEN")` Bing Maps でアプリケーションを認証する必要があります。

> [!NOTE]
> UWP では、ジオコーディングなどのマップサービスを使用するには、 `MapService.ServiceToken` プロパティを認証キーの値に設定する必要もあります。 これは、次のコード行を使用して実現できます。 `Windows.Services.Maps.MapService.ServiceToken = "INSERT_AUTH_TOKEN_HERE";`

また、アプリケーションがユーザーの場所にアクセスする必要がある場合は、パッケージマニフェストで場所の機能を有効にする必要があります。 これは次のようにして実装します。

1. **ソリューション エクスプローラー**で、**package.appxmanifest** をダブルクリックし、**[機能]** タブを選びます。
1. **[機能]** ボックスの一覧で、**[位置情報]** チェックボックスをオンにします。 これにより、 `location` デバイスの機能がパッケージマニフェストファイルに追加されます。

    ```xml
    <Capabilities>
      <!-- DeviceCapability elements must follow Capability elements (if present) -->
      <DeviceCapability Name="location"/>
    </Capabilities>
    ```

#### <a name="release-builds"></a>リリースビルド

UWP リリースビルドでは、.NET ネイティブコンパイルを使用して、アプリケーションを直接ネイティブコードにコンパイルします。 ただし、このような結果として、UWP のコントロールのレンダラーが [`Map`](xref:Xamarin.Forms.Maps.Map) 実行可能ファイルからリンクされている可能性があります。 これは、App.xaml.cs のメソッドの UWP 固有のオーバーロードを使用して修正でき `Forms.Init` ます。 **App.xaml.cs**

```csharp
var assembliesToInclude = new [] { typeof(Xamarin.Forms.Maps.UWP.MapRenderer).GetTypeInfo().Assembly };
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
```

このコードは、クラスが存在するアセンブリを `Xamarin.Forms.Maps.UWP.MapRenderer` メソッドに渡し `Forms.Init` ます。 これにより、.NET ネイティブコンパイルプロセスによってアセンブリが実行可能ファイルからリンクされなくなります。

> [!IMPORTANT]
> この操作を行わない [`Map`](xref:Xamarin.Forms.Maps.Map) と、リリースビルドの実行時にコントロールが表示されなくなります。

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Forms.ピンをマップ](~/xamarin-forms/user-interface/map/pins.md)します。
- [Maps API](xref:Xamarin.Forms.Maps)
- [カスタムレンダラーのマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
