---
title: アプリケーションでの Google Maps API の使用
description: Xamarin.Android アプリケーションに Google Maps API v2 の機能を実装する方法。
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027148"
---
# <a name="using-the-google-maps-api-in-your-application"></a>アプリケーションでの Google Maps API の使用

Maps アプリケーションを使用すると便利ですが、マップをアプリケーションに直接含めることが必要になる場合があります。 Google では、組み込みの Maps アプリケーションに加えて、[Android 用のネイティブ マッピング API](https://developers.google.com/maps/documentation/android-sdk/intro) も提供されています。
Maps API は、マッピング エクスペリエンスをより細かく制御する必要がある場合に適しています。 Maps API でできることは次のとおりです。

- プログラムによるマップの視点の変更。
- マーカーの追加とカスタマイズ。
- オーバーレイによるマップへの注釈付け。

現在では非推奨になっている Google Maps Android API v1 とは異なり、Google Maps Android API v2 は [Google Play 開発者サービス](https://developers.google.com/android/guides/overview)に含まれています。
Google Maps Android API を使用できるようにするには、Xamarin.Android アプリが必須の前提条件を満たしている必要があります。

## <a name="google-maps-api-prerequisites"></a>Google Maps API の前提条件

Maps API を使用する前に、次のようないくつかのステップを実行する必要があります。

- [Maps API キーを取得する](#obtain-maps-key)
- [Google Play 開発者サービス SDK をインストールする](#install-gps-sdk)
- [NuGet から Xamarin.GooglePlayServices.Maps パッケージをインストールする](#install-gpsmaps-nuget)
- [必要なアクセス許可を指定する](#declare-permissions)
- [必要に応じて、Google API を使用してエミュレーターを作成する](#create-emulator-with-google-api)

### <a name="obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Google Maps API キーを取得する

最初のステップでは、Google Maps API キーを取得します (従来の Google Maps v1 API から API キーを再利用することはできないことに注意してください)。 Xamarin.Android で API キーを取得して使用する方法の詳細については、「[Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)」を参照してください。

### <a name="install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Google Play 開発者サービス SDK をインストールする

Google Play 開発者サービスは Google のテクノロジであり、Android アプリケーションで Google+、アプリ内課金、Maps などのさまざまな Google 機能を利用できます。 これらの機能には、[Google Play 開発者サービス APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en) に含まれバックグラウンド サービスとして、Android デバイスでアクセスできます。

Android アプリケーションは、Google Play 開発者サービス クライアント ライブラリを通して Google Play 開発者サービスと対話します。 このライブラリには、Maps などの個々のサービス用のインターフェイスとクラスが含まれています。 次の図では、Android アプリケーションと Google Play 開発者サービスの関係を示します。

![Google Play 開発者サービス APK を更新する Google Play ストアを示す図](maps-api-images/play-services-diagram.png)

Android Maps API は Google Play 開発者サービスの一部として提供されています。
Xamarin.Android アプリケーションで Maps API を使用できるようにするには、その前に、[Android SDK マネージャー](~/android/get-started/installation/android-sdk.md)を使用して Google Play 開発者サービス SDK をインストールする必要があります。 次のスクリーンショットでは、Android SDK マネージャーで Google Play 開発者サービス クライアントがどこにあるかを示します。

![Google Play 開発者サービスは Android SDK マネージャーの [追加] の下に表示される](maps-api-images/image01.png)

> [!NOTE]
> Google Play 開発者サービス APK はライセンス製品であり、デバイスによっては存在しない場合があります。 インストールされていないデバイスでは、Google Maps は機能しません。

### <a name="install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> NuGet から Xamarin.GooglePlayServices.Maps パッケージをインストールする

[Xamarin.GooglePlayServices.Maps パッケージ](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps)には、Google Play 開発者サービス Maps API 用の Xamarin.Android バインドが含まれています。
Google Play 開発者サービス Maps パッケージを追加するには、ソリューション エクスプローラーでプロジェクトの **[参照]** フォルダーを右クリックし、 **[NuGet パッケージの管理]** をクリックします。

![[参照] の [NuGet パッケージの管理] コンテキスト メニュー項目が表示されているソリューション エクスプローラー](maps-api-images/image02.png)

**NuGet パッケージ マネージャー**が開きます。 **[参照]** をクリックし、検索フィールドに「**Xamarin Google Play Services Maps**」と入力します。 **Xamarin.GooglePlayServices.Maps** を選択し、 **[インストール]** をクリックします。 (このパッケージが既にインストールされている場合は、 **[更新]** をクリックします)。

[![Xamarin.GooglePlayServices.Maps パッケージが選択された NuGet パッケージ マネージャー](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

次の依存関係パッケージもインストールされていることに注意してください。

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="specify-the-required-permissions"></a><a name="declare-permissions" /> 必要なアクセス許可を指定する

アプリでは、Google Maps API を使用するためのハードウェアとアクセス許可の要件を明らかにする必要があります。  一部のアクセス許可は、Google Play 開発者サービス SDK によって自動的に付与されるため、開発者が明示的に **AndroidManfest.XML** に追加する必要はありません。

- **ネットワーク状態へのアクセス** &ndash; Map API では、マップ タイルをダウンロードできるかどうかを確認できる必要があります。

- **インターネット アクセス** &ndash; マップ タイルをダウンロードし、API アクセス用に Google Play サーバーと通信するには、インターネット アクセスが必要です。

Google Maps Android API 用の **AndroidManifest.XML** では、次のアクセス許可と機能を指定する必要があります。

- **OpenGL ES v2** &ndash; アプリケーションでは、OpenGL ES v2 の要件を宣言する必要があります。

- **Google Maps API キー** &ndash; API キーは、アプリケーションが登録され、Google Play 開発者サービスの使用を承認されていることを確認するために使用します。 このキーの詳細については、「[Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)」を参照してください。

- **レガシ Apache HTTP クライアント を要求する** &ndash; Android 9.0 (API レベル 28) 以降を対象とするアプリでは、レガシ Apache HTTP クライアントがオプションで使用するライブラリであることを指定する必要があります。

- **Google Web ベースのサービスにアクセスする** &ndash; アプリケーションには、Android Maps API を支援する Google の Web サービスにアクセスするためのアクセス許可が必要です。

- **Google Play 開発者サービス通知のアクセス許可** &ndash; アプリケーションには、Google Play 開発者サービスからリモート通知を受け取るためのアクセス許可が付与されている必要があります。

- **位置情報プロバイダーへのアクセス** &ndash; これらはオプションのアクセス許可です。
   これにより、`GoogleMap` クラスでは、マップ上のデバイスの位置を表示できます。

さらに、Android 9 では bootclasspath から Apache HTTP クライアント ライブラリが削除されているため、API 28 以降を対象とするアプリケーションでは使用できません。 API 28 以降を対象とするアプリケーションで Apache HTTP クライアントを引き続き使用するには、**AndroidManifest.xml** ファイルの `application` ノードに次の行を追加する必要があります。

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> 非常に古いバージョンの Google Play SDK では、アプリで `WRITE_EXTERNAL_STORAGE` アクセス許可を要求する必要がありました。 最近の Google Play 開発者サービスに対する Xamarin バインドでは、この要件は不要になっています。

次のスニペットは、**AndroidManifest.XML** に追加する必要がある設定の例です。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
        <!-- Necessary for apps that target Android 9.0 or higher -->
        <uses-library android:name="org.apache.http.legacy" android:required="false" />
    </application>
</manifest>
```

アプリでは、アクセス許可の **AndroidManifest.XML** を要求するだけでなく、`ACCESS_COARSE_LOCATION` および `ACCESS_FINE_LOCATION` アクセス許可に対する実行時アクセス許可チェックも実行する必要があります。 実行時アクセス許可チェックの実行の詳細については、「[Xamarin.Android のアクセス許可](~/android/app-fundamentals/permissions.md)」ガイドを参照してください。

### <a name="create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Google API を使用してエミュレーターを作成する

物理 Android デバイスに Google Play 開発者サービスがインストールされていない場合は、開発用のエミュレーター イメージを作成することができます。 詳細については、[デバイス マネージャー](~/android/get-started/installation/android-emulator/device-manager.md)に関する記事を参照してください。

## <a name="the-googlemap-class"></a>GoogleMap クラス

前提条件が満たされたら、アプリケーションの開発を開始し、Android Maps API を使用できます。 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) クラスは、Android 用の Google Maps を表示して操作するために Xamarin.Android アプリケーションで使用するメイン API です。 このクラスには次のような役割があります。

- Google Play 開発者サービスと対話し、Google Web サービスでアプリケーションを承認する。

- マップ タイルをダウンロード、キャッシュ、表示する。

- パンやズームなどの UI コントロールをユーザーに表示する。

- マップにマーカーと幾何学図形を描画する。

`GoogleMap` は、次の 2 つの方法のいずれかでアクティビティに追加されます。

- **MapFragment** - [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) は、`GoogleMap` オブジェクトのホストとして機能する特殊なフラグメントです。 `MapFragment` には、Android API レベル 12 以降が必要です。
   古いバージョンの Android では、[SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment) を使用できます。  このガイドでは、`MapFragment` クラスの使用について重点的に説明します。

- **MapView** - [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) は特殊な View サブクラスであり、`GoogleMap` オブジェクトのホストとして機能できます。 このクラスのユーザーは、すべてのアクティビティ ライフサイクル メソッドを `MapView` クラスに転送する必要があります。

これらの各コンテナーでは、`GoogleMap` のインスタンスを返す `Map` プロパティが公開されます。 [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) クラスは開発者が手動で実装する必要のある定型コードの量が減る、より簡単な API なので、こちらのクラスを優先的に使用する必要があります。

### <a name="adding-a-mapfragment-to-an-activity"></a>アクティビティへの MapFragment の追加

次のスクリーンショットは、簡単な `MapFragment` の例です。

[![Google Map フラグメントが表示されているデバイスのスクリーンショット](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

他の Fragment クラスと同様に、アクティビティに `MapFragment` を追加するには 2 つの方法があります。

- **宣言** - `MapFragment` は、アクティビティの XML レイアウト ファイルを使用して追加できます。 次の XML スニペットでは、`fragment` 要素を使用する方法の例を示します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **プログラム** - [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) メソッドを使用してプログラムで `MapFragment` をインスタンス化してから、アクティビティに追加できます。 次のスニペットでは、`MapFragment` オブジェクトをインスタンス化してアクティビティに追加する最も簡単な方法を示します。

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) オブジェクトを `NewInstance` に渡すことによって、`MapFragment` オブジェクトを構成することができます。 これについては、後の「[GoogleMap のプロパティ](#googlemap_object)」セクションで説明します。

フラグメントによってホストされる [`GoogleMap`](#googlemap_object) を初期化し、`MapFragment` によってホストされるマップ オブジェクトへの参照を取得するには、`MapFragment.GetMapAsync` メソッドを使用します。 このメソッドは、`IOnMapReadyCallback` インターフェイスを実装するオブジェクトを受け取ります。

このインターフェイスに含まれる 1 つのメソッド `IMapReadyCallback.OnMapReady(MapFragment map)` は、アプリが `GoogleMap` オブジェクトと対話できるようになると呼び出されます。 次のコード スニペットでは、Android アクティビティで `MapFragment` を初期化し、`IOnMapReadyCallback` インターフェイスを実装する方法を示します。

```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);

        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);

        // remainder of code omitted
    }

    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>マップの種類

Google Maps API からは、次の 5 種類のマップを使用できます。

- **通常** - これは既定のマップの種類です。 道路および重要な自然的特徴と、いくつかの人工的な目的のポイント (建物や橋など) が表示されます。

- **衛星** - このマップには、衛星写真が表示されます。

- **ハイブリッド** - このマップには、衛星写真と道路地図が表示されます。

- **地形** - このマップには主に、いくつかの道路の地形学的特徴が表示されます。

- **なし** - このマップにはタイルが読み込まれず、空のグリッドとしてレンダリングされます。

次の図には、3 種類のマップ (左から右に、通常、ハイブリッド、地形) が示されています。

[![3 つのマップの例のスクリーンショット: 標準、ハイブリッド、地形](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

表示されるマップの種類を設定または変更するには、`GoogleMap.MapType` プロパティを使用します。 次のコード スニペットでは、衛星マップを表示する方法を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="googlemap-properties"></a><a name="googlemap_object" />GoogleMap のプロパティ

`GoogleMap` では、マップの機能と外観を制御できるいくつかのプロパティが定義されています。 `GoogleMap` の初期状態を構成する方法の 1 つは、`MapFragment` の作成時に [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) オブジェクトを渡すことです。 次のコード スニペットは、`MapFragment` を作成するときに `GoogleMapOptions` オブジェクトを使用する例の 1 つです。

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

`GoogleMap` を構成するもう 1 つの方法としては、マップ オブジェクトの [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) でプロパティを操作します。 次のコード サンプルでは、ズーム コントロールとコンパスを表示するように `GoogleMap` を構成する方法を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>GoogleMap との対話

Android Maps API では、アクティビティで視点を変更したり、マーカーを追加したり、カスタム オーバーレイを配置したり、幾何学図形を描画したりできる API が提供されています。 このセクションでは、Xamarin.Android でこれらのタスクのいくつかを実行する方法について説明します。

### <a name="changing-the-viewpoint"></a>視点の変更

マップは、メルカトル図法に基づいて、画面上に平面としてモデル化されます。 マップ ビューは、この平面を真上から見下ろす "*カメラ*" のものです。 カメラの位置は、場所、ズーム、傾き、方位を変更することによって制御できます。 カメラの場所を移動するには、[CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) クラスを使用します。 `CameraUpdate` オブジェクトは直接インスタンス化されず、代わりに Maps API によって [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) クラスが提供されます。

`CameraUpdate` オブジェクトを作成した後、[GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) メソッドまたは [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) メソッドにそれをパラメーターとして渡します。 `MoveCamera` メソッドではマップがすぐに更新されますが、`AnimateCamera` メソッドでは滑らかなアニメーション化された遷移が提供されます。

次のコード スニペットは、`CameraUpdateFactory` を使用して、マップのズーム レベルを 1 ズーム レベルずつインクリメントする `CameraUpdate` を作成する方法の簡単な例です。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API の [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) 使うと、カメラの位置に対して使用できるすべての値が集約されます。 `CameraUpdate` オブジェクトを返す [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) メソッドに、このクラスのインスタンスを提供できます。 Maps API には、`CameraPosition` オブジェクトを作成するための fluent API を提供する [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) クラスも含まれています。
次のコード スニペットでは、`CameraPosition` から `CameraUpdate` を作成し、それを使用して `GoogleMap` 上のカメラの位置を変更する例を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);

    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);

    CameraPosition cameraPosition = builder.Build();

    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

前のコード スニペットでは、マップ上の特定の位置が [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) クラスによって表されています。 ズーム レベルは 18 に設定されており、これは Google Maps によって使用されるズームの任意の測定単位です。 方位は、北から時計回りのコンパスの測定値です。 Tilt プロパティでは、表示角度が制御され、垂直方向から 25 度の角度が指定されています。 次のスクリーンショットは、前のコードを実行した後の `GoogleMap` を示したものです。

[![表示角度が傾いている指定位置を示す Google Maps の例](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>マップへの描画

Android Maps API では、マップ上に次の項目を描画するための API が提供されています。

- **マーカー** - マップ上の 1 つの場所を示すために使用される特殊なアイコンです。

- **オーバーレイ** - マップ上の場所または領域のコレクションを示すために使用できる画像です。

- **線、多角形、円** - アクティビティでマップに図形を追加できる API です。

#### <a name="markers"></a>Markers

Maps API では、マップ上の 1 つの場所に関するすべてのデータをカプセル化する [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) クラスが提供されています。 既定の Marker クラスでは、Google Maps によって提供される標準アイコンが使用されます。 マーカーの外観をカスタマイズしたり、ユーザーのクリックに応答したりすることができます。

##### <a name="adding-a-marker"></a>マーカーの追加

マップにマーカーを追加するには、新しい [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) オブジェクトを作成し、`GoogleMap` インスタンスで [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) メソッドを呼び出す必要があります。 このメソッドからは、[Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) オブジェクトが返されます。

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

ユーザーがマーカーをタップすると、"*情報ウィンドウ*" にマーカーのタイトルが表示されます。 次のスクリーンショットでは、このマーカーの外観を示します。

[![Vimy Ridge のマーカーと情報ウィンドウが表示された Google マップの例](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>マーカーのカスタマイズ

マーカーをマップに追加するときに、`MarkerOptions.InvokeIcon` メソッドを呼び出すことで、マーカーによって使用されるアイコンをカスタマイズできます。
このメソッドは、アイコンをレンダリングするために必要なデータが含まれる [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) オブジェクトを受け取ります。 [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) クラスでは、`BitmapDescriptor` の作成を簡略化するためのヘルパー メソッドがいくつか提供されています。 これらのメソッドの一部を次に示します。

- `DefaultMarker(float colour)` &ndash; 既定の Google Maps マーカーを使用しますが、色を変更します。

- `FromAsset(string assetName)` &ndash; Assets フォルダーにある指定したファイルのカスタム アイコンを使用します。

- `FromBitmap(Bitmap image)` &ndash; 指定したビットマップをアイコンとして使用します。

- `FromFile(string fileName)` &ndash; 指定したパスにあるファイルからカスタム アイコンを作成します。

- `FromResource(int resourceId)` &ndash; 指定したリソースからカスタム アイコンを作成します。

次のコード スニペットでは、シアン色の既定のマーカーを作成する例を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);

    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>情報ウィンドウ

"*情報ウィンドウ*" は、ユーザーが特定のマーカーをタップしたときにユーザーに情報を表示するためにポップアップする特別なウィンドウです。 既定では、情報ウィンドウにはマーカーのタイトルの内容が表示されます。 タイトルが割り当てられていない場合、情報ウィンドウは表示されません。 一度に表示できる情報ウィンドウは 1 つだけです。

[GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) インターフェイスを実装することにより、情報ウィンドウをカスタマイズできます。 このインターフェイスには、次の 2 つの重要なメソッドがあります。

- `public View GetInfoWindow(Marker marker)` &ndash; このメソッドは、マーカーのカスタム情報ウィンドウを取得するために呼び出されます。 `null` が返された場合、既定のウィンドウのレンダリングが使用されます。 このメソッドから View が返された場合は、その View が情報ウィンドウのフレーム内に配置されます。

- `public View GetInfoContents(Marker marker)` &ndash; このメソッドは、GetInfoWindow から `null` が返された場合にのみ呼び出されます。 情報ウィンドウの内容の既定のレンダリングを使用する場合、このメソッドは `null` 値を返すことができます。 それ以外の場合、このメソッドは情報ウィンドウの内容を含む View を返す必要があります。

情報ウィンドウはライブ ビューではありません。代わりに、Android によってビューが静的ビットマップに変換されて、画像上に表示されます。 これは、情報ウィンドウがタッチ イベントやジェスチャに応答できず、自動的には更新されないことを意味します。 情報ウィンドウを更新するには、[GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) メソッドを呼び出す必要があります。

次の図では、カスタマイズされた情報ウィンドウの例をいくつか示します。 左側の画像では内容がカスタマイズされていますが、右側の画像ではウィンドウと内容が丸い角にカスタマイズされています。

![アイコンと人口が含まれる、メルボルンのマーカー ウィンドウの例。 右側のウィンドウでは角が丸くなっている。](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlay

マップ上の特定の位置を示すマーカーとは異なり、[GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) は、マップ上の位置のコレクションまたは領域を示すために使用される画像です。

##### <a name="adding-a-groundoverlay"></a>GroundOverlay の追加

マップへのグラウンド オーバーレイの追加は、マップへのマーカーの追加と似ています。 まず、[GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) オブジェクトを作成します。 次に、このオブジェクトをパラメーターとして [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) メソッドに渡すと、`GroundOverlay` オブジェクトが返されます。 次のコード スニペットは、マップにグラウンド オーバーレイを追加する例です。

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

次のスクリーンショットは、マップ上にこのオーバーレイを表示したものです。

[![マップにシロクマの画像をオーバーレイした例](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>線、円、多角形

マップに追加できる 3 つの簡単な幾何学図形があります。

- **ポリライン** - 接続された一連の線分です。 マップ上のパスにマークを付けたり、幾何学図形を作成したりできます。

- **円** - マップ上に円が描画されます。

- **多角形** - これは、マップ上の領域をマークするための閉じた図形です。

##### <a name="polylines"></a>ポリライン

[ポリライン](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline)は、各線分の頂点を指定する連続した `LatLng` オブジェクトのリストです。 ポリラインを作成するには、まず `PolylineOptions` オブジェクトを作成し、それに点を追加します。 その後、`AddPolyline` メソッドを呼び出すことによって、`PolylineOption` オブジェクトを `GoogleMap` オブジェクトに渡します。

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>円

円を作成するには、最初に、円の中心と半径 (メートル単位) を指定する [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) オブジェクトをインスタンス化します。 [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)) を呼び出すことによって、円をマップ上に描画します。
次のコード スニペットでは、円を描画する方法を示します。

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>多角形

`Polygon` は `Polyline` に似ていますが、必ず閉じています。 `Polygon` は閉じたループであり、その内側が塗りつぶされます。
`Polygon` は、`Polyline` とまったく同じ方法で作成しますが、代わりに [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) メソッドを呼び出します。

`Polyline` とは異なり、`Polygon` はそれ自体が閉じています。 多角形は、`AddPolygon` メソッドによって最初と最後のポイントを結ぶ線を描画することで閉じられます。 次のコード スニペットでは、前の `Polyline` の例のコード スニペットと同じ領域に、単色の四角形が作成されます。

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>ユーザー イベントへの応答

ユーザーとマップの間には 3 種類の対話があります。

- **マーカー クリック** - ユーザーがマーカーをクリックします。

- **マーカー ドラッグ** - ユーザーがマーカーを長押しします

- **情報ウィンドウ クリック** - ユーザーが情報ウィンドウをクリックします。

それぞれのイベントについて、以下で詳しく説明します。

### <a name="marker-click-events"></a>マーカー クリック イベント

`MarkerClicked` イベントは、ユーザーがマーカーをタップすると発生します。 このイベントは、パラメーターとして `GoogleMap.MarkerClickEventArgs` オブジェクトを受け取ります。 このクラスには、2 つのプロパティが含まれています。

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; このプロパティは、イベント ハンドラーがイベントを使用したことを示す `true` に設定されている必要があります。 これが `false` に設定されている場合は、イベント ハンドラーのカスタム動作に加えて、既定の動作が発生します。

- `Marker` &ndash; このプロパティは、`MarkerClick` イベントを発生させたマーカーへの参照です。

次のコード スニペットでは、カメラの位置をマップ上の新しい位置に変更する `MarkerClick` の例を示します。

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);

        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```

### <a name="marker-drag-events"></a>マーカー ドラッグ イベント

このイベントは、ユーザーがマーカーをドラッグしようとしたときに発生します。 既定では、マーカーはドラッグできません。 マーカーは、`Marker.Draggable` プロパティを `true` に設定するか、`true` をパラメーターとして `MarkerOptions.Draggable` メソッドを呼び出すことにより、ドラッグ可能として設定できます。

マーカーをドラッグするには、ユーザーは最初にマーカーを長押しし、指をマップ上に留めておく必要があります。 ユーザーが指を画面上でドラッグすると、マーカーが移動します。 ユーザーが指を画面から離すと、マーカーはその位置に残ります。

次の一覧では、ドラッグ可能なマーカーに対して発生するさまざまなイベントについて説明します。

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; このイベントは、ユーザーが最初にマーカーをドラッグすると発生します。

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; このイベントは、マーカーがドラッグされている間に発生します。

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; このイベントは、ユーザーがマーカーのドラッグを終了すると発生します。

各 `EventArgs` には、ドラッグされている `Marker` オブジェクトへの参照である `P0` という名前の 1 つのプロパティが含まれます。

### <a name="info-window-click-events"></a>情報ウィンドウ クリック イベント

一度に表示できる情報ウィンドウは 1 つだけです。 ユーザーがマップで情報ウィンドウをクリックすると、マップ オブジェクトで `InfoWindowClick` イベントが発生します。 次のコード スニペットでは、ハンドラーをイベントに結び付ける方法を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

情報ウィンドウは、マップ上に画像としてレンダリングされる静的な `View` であることを思い出してください。 情報ウィンドウ内に配置されているボタン、チェック ボックス、テキスト ビューなどのウィジェットは不活性であり、どのようなユーザー イベントにも応答できません。

## <a name="related-links"></a>関連リンク

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play 開発者サービス](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play 開発者サービス APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
