---
title: アプリケーションでの Google Maps API の使用
description: Xamarin Android アプリケーションに Google Maps API v2 の機能を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027148"
---
# <a name="using-the-google-maps-api-in-your-application"></a>アプリケーションでの Google Maps API の使用

Maps アプリケーションの使用は優れていますが、マップをアプリケーションに直接含めることが必要になる場合があります。 Google は、組み込みの maps アプリケーションに加えて、 [Android 用のネイティブマッピング API](https://developers.google.com/maps/documentation/android-sdk/intro)も提供しています。
Maps API は、マッピングエクスペリエンスをより細かく制御する必要がある場合に適しています。 Maps API でできることは次のとおりです。

- プログラムによるマップの視点の変更。
- マーカーの追加とカスタマイズ
- オーバーレイを使用してマップに注釈を付ける。

現在、非推奨の Google Maps Android API v1 とは異なり、Google Maps Android API v2 は[Google Play 開発者サービス](https://developers.google.com/android/guides/overview)に含まれています。
Google Maps Android API を使用できるようにするには、Xamarin Android アプリが必須の前提条件を満たしている必要があります。

## <a name="google-maps-api-prerequisites"></a>Google Maps API の前提条件

Maps API を使用する前に、次のようないくつかの手順を実行する必要があります。

- [Maps API キーを取得する](#obtain-maps-key)
- [Google Play 開発者サービス SDK のインストール](#install-gps-sdk)
- [NuGet から GooglePlayServices パッケージをインストールする](#install-gpsmaps-nuget)
- [必要なアクセス許可を指定します](#declare-permissions)
- [必要に応じて、Google Api を使用してエミュレーターを作成します。](#create-emulator-with-google-api)

### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a>Google Maps API キーを取得 <a name="obtain-maps-key" />には

最初の手順では、Google Maps API キーを取得します (従来の Google Maps v1 API から API キーを再利用することはできないことに注意してください)。 Xamarin Android で API キーを取得して使用する方法の詳細については、「 [Google MAPS Api キーを取得](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)する」を参照してください。

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a>Google Play 開発者サービス SDK をインストール <a name="install-gps-sdk" /> には

Google Play 開発者サービスは Google のテクノロジであり、Android アプリケーションは google +、アプリ内課金、Maps などのさまざまな Google 機能を利用できます。 これらの機能には、Android デバイスの[Google Play 開発者サービス APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)に含まれているバックグラウンドサービスとしてアクセスできます。

Android アプリケーションは、Google Play 開発者サービスクライアントライブラリを使用して Google Play 開発者サービスと対話します。 このライブラリには、Maps などの個々のサービスのインターフェイスとクラスが含まれています。 次の図は、Android アプリケーションと Google Play 開発者サービスの関係を示しています。

![Google Play 開発者サービス APK を更新 Google Play ストアを示す図](maps-api-images/play-services-diagram.png)

Android Maps API は Google Play 開発者サービスの一部として提供されています。
Xamarin Android アプリケーションで Maps API を使用するには、 [Android SDK マネージャー](~/android/get-started/installation/android-sdk.md)を使用して Google Play 開発者サービス SDK をインストールする必要があります。 次のスクリーンショットは、Android SDK Manager の Google Play services クライアントがどこにあるかを示しています。

![Google Play 開発者サービスは Android SDK マネージャーの [エクストラ] の下に表示されます。](maps-api-images/image01.png)

> [!NOTE]
> Google Play services APK は、すべてのデバイスに存在しない可能性があるライセンスされた製品です。 インストールされていない場合、Google Maps はデバイスで機能しません。

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a>NuGet から GooglePlayServices パッケージをインストール <a name="install-gpsmaps-nuget" /> には

[GooglePlayServices パッケージ](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps)には、GOOGLE PLAY 開発者サービス maps API 用の Xamarin. Android バインドが含まれています。
Google Play 開発者サービスマップパッケージを追加するには、ソリューションエクスプローラーでプロジェクトの **[参照]** フォルダーを右クリックし、 **[NuGet パッケージの管理...]** をクリックします。

![[参照] の下の [NuGet パッケージの管理] コンテキストメニュー項目を表示するソリューションエクスプローラー](maps-api-images/image02.png)

**NuGet パッケージマネージャー**が開きます。 **[参照]** をクリックし、検索フィールドに「 **Xamarin Google Play 開発者サービス Maps** 」と入力します。 **GooglePlayServices**を選択し、 **[インストール]** をクリックします。 (このパッケージが既にインストールされている場合は、 **[更新]** をクリックします)。

[GooglePlayServices パッケージが選択された状態で![NuGet パッケージマネージャー](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

次の依存関係パッケージもインストールされていることに注意してください。

- **GooglePlayServices**
- **GooglePlayServices. Basement**
- **GooglePlayServices**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a>必要なアクセス許可を指定 <a name="declare-permissions" /> には

アプリは、Google Maps API を使用するために、ハードウェアとアクセス許可の要件を識別する必要があります。  一部のアクセス許可は Google Play 開発者サービス SDK によって自動的に付与されるため、開発者が明示的に**Androidmanfest .xml**に追加する必要はありません。

- Maps API がマップタイルをダウンロードできるかどうかを確認できるようにするには、**ネットワークの状態にアクセス**&ndash; 必要があります。

- マップタイルをダウンロードし、API アクセス用に Google Play サーバーと通信するには、**インターネットアクセス**&ndash; インターネットアクセスが必要です。

Google Maps Android API の**Androidmanifest .xml**では、次のアクセス許可と機能を指定する必要があります。

- **OPENGL es v2** &ndash; アプリケーションは、opengl es v2 の要件を宣言する必要があります。

- **Google MAPS Api キー** &ndash; api キーを使用して、アプリケーションが登録され、Google Play 開発者サービスを使用する権限があることを確認します。 このキーの詳細について[は、「Google MAPS API キーの取得](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)」を参照してください。

- 従来の**APACHE http クライアント**&ndash; Android 9.0 (API レベル 28) 以降を対象とするアプリを要求するレガシ apache http クライアントが、使用するオプションのライブラリであることを指定する必要があります。

- Android Maps API をバックする Google の web サービスにアクセスするためのアクセス許可がアプリケーションに必要な &ndash;、 **Google web ベースのサービスにアクセス**します。

- アプリケーション &ndash; **Google Play 開発者サービス通知に対するアクセス**許可は、Google Play 開発者サービスからリモート通知を受信するためのアクセス許可を付与する必要があります。

- **場所プロバイダーへのアクセス**&ndash; これらはオプションのアクセス許可です。
   これにより、`GoogleMap` クラスは、マップ上のデバイスの場所を表示できます。

さらに、Android 9 は bootclasspath から Apache HTTP クライアントライブラリを削除したため、API 28 以上を対象とするアプリケーションでは使用できません。 API 28 以降を対象とするアプリケーションで Apache HTTP クライアントを引き続き使用するには、次の行を**Androidmanifest .xml**ファイルの `application` ノードに追加する必要があります。

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Google Play SDK の以前のバージョンでは、`WRITE_EXTERNAL_STORAGE` アクセス許可を要求するアプリが必要でした。 Google Play 開発者サービス用の最新の Xamarin バインドでは、この要件は不要になりました。

次のスニペットは、 **Androidmanifest .xml**に追加する必要がある設定の例です。

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

アプリでは、アクセス許可**Androidmanifest .xml**を要求するだけでなく、`ACCESS_COARSE_LOCATION` および `ACCESS_FINE_LOCATION` のアクセス許可に対する実行時アクセス許可チェックも実行する必要があります。 実行時のアクセス許可チェックの実行の詳細については、「 [Xamarin Android のアクセス許可](~/android/app-fundamentals/permissions.md)ガイド」を参照してください。

### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a>Google Api を使用してエミュレーターを作成 <a name="create-emulator-with-google-api" />には

Google Play services を使用する物理的な Android デバイスがインストールされていない場合は、開発用のエミュレーターイメージを作成することができます。 詳細については、[デバイスマネージャー](~/android/get-started/installation/android-emulator/device-manager.md)を参照してください。

## <a name="the-googlemap-class"></a>GoogleMap クラス

前提条件が満たされたら、アプリケーションの開発を開始し、Android Maps API を使用します。 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap)クラスは、android 用の Google Maps を表示して対話するために Xamarin アプリケーションで使用されるメイン API です。 このクラスには、次の役割があります。

- Google Play サービスと対話して、Google web サービスでアプリケーションを承認します。

- マップタイルのダウンロード、キャッシュ、および表示。

- パンやズームなどの UI コントロールをユーザーに表示します。

- マップにマーカーと幾何学図形を描画します。

`GoogleMap` は、次の2つの方法のいずれかでアクティビティに追加されます。

- **Mapfragment** - [mapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment)は、`GoogleMap` オブジェクトのホストとして機能する特殊なフラグメントです。 `MapFragment` には、Android API レベル12以上が必要です。
   以前のバージョンの Android では、 [Supportmapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)を使用できます。  このガイドでは、`MapFragment` クラスの使用について重点的に説明します。

- **Mapview** - [mapview](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView)は特殊化されたビューサブクラスであり、`GoogleMap` オブジェクトのホストとして機能することができます。 このクラスのユーザーは、すべてのアクティビティライフサイクルメソッドを `MapView` クラスに転送する必要があります。

これらの各コンテナーは、`GoogleMap`のインスタンスを返す `Map` プロパティを公開します。 開発者が手動で実装する必要がある定型コードの量を減らすことができるため、 [Mapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment)クラスに設定する必要があります。

### <a name="adding-a-mapfragment-to-an-activity"></a>アクティビティへの MapFragment の追加

単純な `MapFragment`の例を次のスクリーンショットに示します。

[Google マップフラグメントを表示しているデバイスのスクリーンショットを![します](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

他のフラグメントクラスと同様に、アクティビティに `MapFragment` を追加するには、次の2つの方法があります。

- **宣言**によっては、アクティビティの XML レイアウトファイルを使用して `MapFragment` を追加できます。 次の XML スニペットは、`fragment` 要素の使用方法の例を示しています。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **プログラム**によって、`MapFragment` を[`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance())メソッドを使用してプログラムでインスタンス化してから、アクティビティに追加することができます。 このスニペットは、`MapFragment` オブジェクトをインスタンス化してアクティビティに追加する最も簡単な方法を示しています。

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    `NewInstance`に[`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions)オブジェクトを渡すことによって、`MapFragment` オブジェクトを構成することができます。 この点については、このガイドで後述する「 [GoogleMap properties](#googlemap_object) 」セクションを参照してください。

`MapFragment.GetMapAsync` メソッドは、フラグメントによってホストされる[`GoogleMap`](#googlemap_object)を初期化し、`MapFragment`によってホストされる map オブジェクトへの参照を取得するために使用されます。 このメソッドは、`IOnMapReadyCallback` インターフェイスを実装するオブジェクトを受け取ります。

このインターフェイスには、アプリが `GoogleMap` オブジェクトと対話できるようになったときに呼び出される `IMapReadyCallback.OnMapReady(MapFragment map)` 1 つのメソッドがあります。 次のコードスニペットは、Android アクティビティが `MapFragment` を初期化し、`IOnMapReadyCallback` インターフェイスを実装する方法を示しています。

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

Google Maps API からは、次の5種類のマップを使用できます。

- **Normal** -これは既定のマップの種類です。 これは、道路と重要な自然特徴と、いくつかの人工ポイント (建物やブリッジなど) を示しています。

- **サテライト**-このマップは、衛星写真を示しています。

- **ハイブリッド**-このマップには、衛星写真と道路地図が表示されます。

- **地形**-これは主に、いくつかの道路の topographical 特徴を示しています。

- **None** -このマップはタイルを読み込みません。空のグリッドとしてレンダリングされます。

次の図は、左から右 (通常、ハイブリッド、地形) の3種類のマップを示しています。

[![3 つのマップの例 (通常、ハイブリッド、地形)](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` プロパティは、表示されるマップの種類を設定または変更するために使用されます。 次のコードスニペットは、サテライトマップを表示する方法を示しています。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="a-namegooglemap_object-googlemap-properties"></a><a name="googlemap_object" />GoogleMap のプロパティ

`GoogleMap` は、マップの機能と外観を制御できるいくつかのプロパティを定義します。 `GoogleMap` の初期状態を構成する方法の1つとして、`MapFragment`を作成するときに[GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions)オブジェクトを渡す方法があります。 次のコードスニペットは、`MapFragment`を作成するときに `GoogleMapOptions` オブジェクトを使用する例の1つです。

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

`GoogleMap` を構成するもう1つの方法は、map オブジェクトの[Uisettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings)のプロパティを操作することです。 次のコードサンプルでは、ズームコントロールとコンパスを表示するように `GoogleMap` を構成する方法を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>GoogleMap との対話

Android Maps API は、アクティビティが視点を変更したり、マーカーを追加したり、カスタムオーバーレイを配置したり、幾何学図形を描画したりできるようにする Api を提供します。 このセクションでは、Xamarin Android でこれらのタスクのいくつかを実行する方法について説明します。

### <a name="changing-the-viewpoint"></a>視点の変更

マップは、商品投影に基づいて、画面上の平面としてモデル化です。 [マップ] ビューは、この平面上の*カメラ*の表面を左右に移動します。 カメラの位置は、場所、ズーム、傾き、およびベアリングを変更することによって制御できます。 [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate)クラスは、カメラの位置を移動するために使用されます。 `CameraUpdate` オブジェクトは直接インスタンス化されません。代わりに、Maps API は[CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory)クラスを提供します。

`CameraUpdate` オブジェクトが作成されると、 [MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate))または[GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate))のいずれかのメソッドにパラメーターとして渡されます。 `MoveCamera` メソッドは、`AnimateCamera` メソッドが滑らかでアニメーション化された遷移を提供している間に、すぐにマップを更新します。

このコードスニペットは、`CameraUpdateFactory` を使用して、マップのズームレベルを1ズームレベルずつインクリメントする `CameraUpdate` を作成する方法の簡単な例です。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API は、カメラの位置に使用可能なすべての値を集計する[CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html)を提供します。 このクラスのインスタンスは、`CameraUpdate` オブジェクトを返す[NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29)メソッドに提供できます。 Maps API には、`CameraPosition` オブジェクトを作成するための fluent API を提供する[CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html)クラスも含まれています。
次のコードスニペットは、`CameraPosition` から `CameraUpdate` を作成し、それを使用して `GoogleMap`上のカメラの位置を変更する例を示しています。

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

前のコードスニペットでは、マップ上の特定の位置は[LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng)クラスによって表されています。 ズームレベルは18に設定されています。これは、Google Maps によって使用されるズームの任意の測定単位です。 方位は、北から時計回りのコンパス測定値です。 傾きプロパティは、表示角度を制御し、垂直方向の25°の角度を指定します。 次のスクリーンショットは、前のコードを実行した後の `GoogleMap` を示しています。

[![の例では、指定された位置を傾いた表示角度で示しています](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>マップ上の描画

Android Maps API は、マップ上に次の項目を描画するための API を提供します。

- **マーカー** -マップ上の1つの場所を識別するために使用される特殊なアイコンです。

- **[オーバーレイ]** -マップ上の場所または領域のコレクションを識別するために使用できるイメージです。

- **線、多角形、および円**-これらは、アクティビティがマップに図形を追加できるようにする api です。

#### <a name="markers"></a>Markers

Maps API には、マップ上の1つの場所に関するすべてのデータをカプセル化する[マーカー](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker)クラスが用意されています。 既定では、マーカークラスは Google Maps によって提供される標準アイコンを使用します。 マーカーの外観をカスタマイズしたり、ユーザーのクリックに応答したりすることができます。

##### <a name="adding-a-marker"></a>マーカーの追加

マップにマーカーを追加するには、新しい[MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions)オブジェクトを作成し、`GoogleMap` インスタンスで[addmarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29)メソッドを呼び出す必要があります。 このメソッドは、[マーカー](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker)オブジェクトを返します。

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

マーカーのタイトルは、ユーザーがマーカーをタップしたときに*情報ウィンドウ*に表示されます。 次のスクリーンショットは、このマーカーの外観を示しています。

[![例 Google Map とヴィミーねじ山の情報ウィンドウ](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>マーカーのカスタマイズ

マーカーをマップに追加するときに、`MarkerOptions.InvokeIcon` メソッドを呼び出すことで、マーカーによって使用されるアイコンをカスタマイズできます。
このメソッドは、アイコンを表示するために必要なデータを含む[Bitmapdescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor)オブジェクトを受け取ります。 [Bitmap記述子ファクトリ](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory)クラスには、`BitmapDescriptor`の作成を簡略化するためのヘルパーメソッドが用意されています。 次の一覧では、これらのメソッドのいくつかについて説明します。

- `DefaultMarker(float colour)` &ndash; 既定の Google Maps マーカーを使用しますが、色を変更します。

- `FromAsset(string assetName)` &ndash; Assets フォルダー内の指定されたファイルのカスタムアイコンを使用します。

- `FromBitmap(Bitmap image)` は、指定されたビットマップをアイコンとして使用 &ndash; ます。

- `FromFile(string fileName)`、指定したパスにあるファイルからカスタムアイコンを作成 &ndash; ます。

- `FromResource(int resourceId)`、指定したリソースからカスタムアイコンを作成 &ndash; ます。

次のコードスニペットは、シアンの色付きの既定のマーカーを作成する例を示しています。

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

*情報ウィンドウ*は、特定のマーカーをタップしたときにユーザーに情報を表示するための特別なウィンドウです。 既定では、[情報] ウィンドウにマーカーのタイトルの内容が表示されます。 タイトルが割り当てられていない場合は、情報ウィンドウは表示されません。 一度に表示できる情報ウィンドウは1つだけです。

[GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter)インターフェイスを実装することによって、情報ウィンドウをカスタマイズすることができます。 このインターフェイスには、次の2つの重要なメソッドがあります。

- `public View GetInfoWindow(Marker marker)` &ndash;、このメソッドを呼び出して、マーカーのカスタム情報ウィンドウを取得します。 `null` が返される場合は、既定のウィンドウレンダリングが使用されます。 このメソッドがビューを返す場合、そのビューは情報ウィンドウフレーム内に配置されます。

- `public View GetInfoContents(Marker marker)` &ndash; このメソッドは、GetInfoWindow が `null` を返す場合にのみ呼び出されます。 情報ウィンドウの内容の既定の表示を使用する場合、このメソッドは `null` 値を返すことができます。 それ以外の場合、このメソッドは、情報ウィンドウの内容を含むビューを返します。

情報ウィンドウはライブビューではありません。代わりに、Android はビューを静的ビットマップに変換し、画像上に表示します。 これは、情報ウィンドウがタッチイベントやジェスチャに応答できないことと、自動的には更新されないことを意味します。 情報ウィンドウを更新するには、 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow())メソッドを呼び出す必要があります。

次の図は、カスタマイズされた情報ウィンドウのいくつかの例を示しています。 左側の画像の内容はカスタマイズされていますが、右側のイメージのウィンドウと内容は丸い角でカスタマイズされています。

![アイコンや人口など、メルボルンのマーカーウィンドウの例を示します。 右側のウィンドウの角が丸くなっています。](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

マップ上の特定の位置を示すマーカーとは異なり、 [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay)は、場所のコレクションまたはマップ上の領域を識別するために使用されるイメージです。

##### <a name="adding-a-groundoverlay"></a>GroundOverlay の追加

マップにグラウンドオーバーレイを追加することは、マップにマーカーを追加することと似ています。 まず、 [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions)オブジェクトを作成します。 このオブジェクトは、 [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions))メソッドにパラメーターとして渡され、`GroundOverlay` オブジェクトを返します。 次のコードスニペットは、マップにグラウンドオーバーレイを追加する例です。

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

次のスクリーンショットは、マップ上のこのオーバーレイを示しています。

[店舗の図を含むマップの例![](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>線、円、および多角形

マップに追加できるジオメトリック図形には、次の3つの単純な種類があります。

- **ポリライン**-接続された一連の線分です。 マップ上のパスにマークを付けたり、幾何学図形を作成したりできます。

- **Circle** -マップ上に円を描画します。

- **Polygon** -マップ上の領域をマークするための閉じた図形です。

##### <a name="polylines"></a>多角形

[ポリライン](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline)は、各線分の頂点を指定する連続した `LatLng` オブジェクトの一覧です。 ポリラインを作成するには、まず `PolylineOptions` オブジェクトを作成し、そのオブジェクトに点を追加します。 その後、`PolylineOption` オブジェクトは、`AddPolyline` メソッドを呼び出すことによって `GoogleMap` オブジェクトに渡されます。

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>周囲

円は、最初に[CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions)オブジェクトをインスタンス化することによって作成されます。このオブジェクトは、metres の中心と円の半径を指定します。 円は、 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions))を呼び出すことによって、マップ上に描画されます。
次のコードスニペットは、円を描画する方法を示しています。

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>多角形

`Polygon`は `Polyline`に似ていますが、開いていません。 `Polygon`s は閉じられたループであり、内部が入力されています。
`Polygon`は、呼び出された[GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions))メソッドを除き、`Polyline`とまったく同じ方法で作成されます。

`Polyline`とは異なり、`Polygon` は自己終了です。 最初と最後の点を結ぶ線を描画することによって、多角形は `AddPolygon` メソッドによって閉じられます。 次のコードスニペットでは、`Polyline` の例の前のコードスニペットと同じ領域に単色の四角形を作成します。

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>ユーザーイベントへの応答

マップには、次の3種類の相互作用があります。

- **マーカーのクリック**-ユーザーがマーカーをクリックします。

- **マーカーのドラッグ**-ユーザーが長いクリックを行っている

- **情報ウィンドウのクリック**-ユーザーが情報ウィンドウをクリックしました。

これらの各イベントについては、以下で詳しく説明します。

### <a name="marker-click-events"></a>マーカークリックイベント

`MarkerClicked` イベントは、ユーザーがマーカーをタップしたときに発生します。 このイベントは、パラメーターとして `GoogleMap.MarkerClickEventArgs` オブジェクトを受け取ります。 このクラスには、次の2つのプロパティが含まれます。

- イベントハンドラーがイベントを使用したことを示すには、このプロパティ &ndash; `GoogleMap.MarkerClickEventArgs.Handled` を `true` に設定する必要があります。 これが `false` に設定されている場合は、イベントハンドラーのカスタム動作に加えて、既定の動作が発生します。

- `Marker` &ndash; このプロパティは、`MarkerClick` イベントを発生させたマーカーへの参照です。

次のコードスニペットは、カメラの位置をマップ上の新しい場所に変更する `MarkerClick` の例を示しています。

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

### <a name="marker-drag-events"></a>マーカーのドラッグイベント

このイベントは、ユーザーがマーカーをドラッグしようとしたときに発生します。 既定では、マーカーはドラッグできません。 マーカーは、`Marker.Draggable` プロパティを `true` に設定するか、`true` をパラメーターとして `MarkerOptions.Draggable` メソッドを呼び出すことによって、ドラッグ可能として設定できます。

マーカーをドラッグするには、ユーザーがマーカーを長い順にクリックする必要があります。その後、指をマップ上に残しておく必要があります。 ユーザーの指を画面の周囲にドラッグすると、マーカーが移動します。 ユーザーの指が画面から離したときに、マーカーはそのまま残ります。

次の一覧では、ドラッグ可能なマーカーに対して発生するさまざまなイベントについて説明します。

- このイベントは、ユーザーが最初にマーカーをドラッグしたときに発生する &ndash; `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` ます。

- マーカーをドラッグしているときにこのイベントが発生する &ndash; `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` ます。

- このイベントは、ユーザーがマーカーのドラッグを終了したときに発生し &ndash; `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` ます。

各 `EventArgs` には、ドラッグされる `Marker` オブジェクトへの参照である `P0` と呼ばれる1つのプロパティが含まれています。

### <a name="info-window-click-events"></a>情報ウィンドウのクリックイベント

一度に表示できる情報ウィンドウは1つだけです。 ユーザーがマップ内の情報ウィンドウをクリックすると、map オブジェクトによって `InfoWindowClick` イベントが発生します。 次のコードスニペットは、ハンドラーをイベントに接続する方法を示しています。

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

情報ウィンドウは静的な `View` であり、マップ上にイメージとしてレンダリングされることを思い出してください。 情報ウィンドウ内に配置されているボタン、チェックボックス、テキストビューなどのウィジェットはのみされ、整数のユーザーイベントには応答できません。

## <a name="related-links"></a>関連リンク

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play 開発者サービス](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play 開発者サービス APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google Maps API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [使用-ライブラリ](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [使用-機能](https://developer.android.com/guide/topics/manifest/uses-feature-element)
