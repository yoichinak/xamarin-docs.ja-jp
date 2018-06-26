---
title: アプリケーションでは、Google マップ API を使用します。
description: Xamarin.Android アプリケーションで Google Maps API v2 の機能を実装する方法。
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: a0e010a8300eb4b4452737e34d2f55a35ab95428
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935140"
---
# <a name="using-the-google-maps-api-in-your-application"></a>API を使用して、Google マップ、アプリケーションで

マップのアプリケーションを使用して、あまり良い外観は、アプリケーションに直接マップを追加することがあります。 だけでなく、組み込みマップ アプリケーションでは、Google も用意されています、 [for Android のネイティブ マッピング API](https://developers.google.com/maps/documentation/android/)です。
マップの API は、マッピング エクスペリエンスより詳細に制御を維持する必要がある場合に適しています。 マップの API で使用できるものは次のとおりです。

-  マップの視点をプログラムで変更します。
-  追加して、マーカーをカスタマイズします。
-  オーバーレイをマップに注釈を付けます。

ここで推奨されなくなりました Google Maps Android API v1 とは異なりの一部である Google Maps Android API v2 [Google Play サービス](http://developer.android.com/google/play-services/index.html)です。
したがってを Xamarin.Android アプリケーションで、Google マップ Android API を使用することは前に、いくつかの必須の前提条件を満たすために必要なは。


## <a name="google-maps-api-prerequisites"></a>Google は、API の前提条件をマップします。

マップの API を使用する前に構成する必要があるいくつかの項目を含みます。

-  Google Play サービス SDK をインストールします。
-  Google Api を使用したエミュレーターを作成します。
-  マップの API キーを取得します。
-  必要なアクセス許可を指定します。



### <a name="install-the-google-play-services-sdk"></a>Google Play サービス SDK をインストールします。

Google Play サービスは、Google +、アプリでは、次の請求、およびマップなどのさまざまな Google 機能を利用するために Android アプリケーションを使用する Google からテクノロジです。 含まれているバック グラウンド サービスとしてこれらの機能は Android デバイスでアクセス可能な[Google プレイ サービス APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)です。

Android アプリケーションは、Google Play サービス クライアント ライブラリによって Google Play サービスと対話します。 このライブラリには、インターフェイスと、マップなどのサービスの各クラスが含まれています。 次の図は、Android のアプリケーションと Google Play サービス間のリレーションシップを示しています。

![Google プレイ サービス APK の更新、Google Play ストアを示すダイアグラム](maps-api-images/play-services-diagram.png)

Android のマップの API は、Google Play サービスの一部として提供されます。
Xamarin.Android アプリケーションは、マップの API を使用できます、前に Google プレイ サービス SDK をインストールしてバインドする必要があります。 Google プレイ サービス SDK には、Android SDK Manager はインストールされます。 次のスクリーン ショットは、Google Play サービス クライアントはあります Android SDK Manager で場所を示しています。

![Google Play サービスが、Android SDK Manager その他の機能の下に表示されます。](maps-api-images/image01.png)

> [!NOTE]
> Google Play サービス APK は、すべてのデバイス上に存在できない可能性があるライセンスされた製品です。 インストールされていないデバイスで Google Maps は機能しません。


#### <a name="binding-google-play-services"></a>バインディングの Google Play サービスします。

Google Play サービス クライアント ライブラリがインストールされると、Xamarin.Android Java バインディング ライブラリによってバインドしなければなりません。 これにはこれを実現する 2 つの方法があります。

-  **Google プレイ サービス マップの NuGet パッケージを使用して**-これは、最も簡単なアプローチ (Xamarin.Android 4.8 でのみ使用可能なまたはそれ以降)。
   インストール、 [Xamarin Google サービス マップ NuGet の再生](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); Google Play サービス依存関係パッケージもインストールされます。
   このガイドの残りの部分は、この方法について説明します。

-  **Google Play サービス クライアント ライブラリを手動でバインド**-これはより複雑な方法は、Google プレイ サービス SDK をバインドするには、Xamarin.Android 4.4 または Xamarin.Android 4.6 の唯一の方法です。
   このドキュメントの範囲を超えては Google Play サービス クライアント ライブラリを手動でバインドしますが、これを行う方法の例は、「、[マップや場所デモ v3 サンプル](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3)Github でします。


#### <a name="adding-the-google-play-services-map-package"></a>Google Play サービス マップのパッケージを追加します。

パッケージを追加する、Google プレイ サービス マップを右クリックして、**参照**フォルダーをクリックして、ソリューション エクスプ ローラーでプロジェクトの**NuGet パッケージを管理しています.**:

![参照 ソリューション エクスプ ローラーの表示 NuGet パッケージの管理コンテキスト メニュー項目](maps-api-images/image02.png)

開き、 **NuGet Package Manager**です。 をクリックして**参照**入力と**Xamarin Google プレイ サービス マップ**検索フィールドにします。 選択**Xamarin.GooglePlayServices.Maps**  をクリック**インストール**です。 (このパッケージが既にインストールされていた場合はクリックして**更新**。)。

[![選択した Xamarin.GooglePlayServices.Maps パッケージで NuGet パッケージ マネージャー](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

次の依存関係パッケージもインストールされていることに注意してください。

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Google Api を使用したエミュレーターを作成します。

これはお勧めできません、Android のマップの API をサポートするために、エミュレーターをセットアップすることです。 エミュレーターは、Google API レベル 17 (Android 4.2.2) を対象とするように構成する必要がありますまたはそれ以降。 次のスクリーン ショットでは、API レベル 19 のエミュレーター イメージを構成します。 

![API レベル 19 用に構成された、AVD で android エミュレーター マネージャー](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Google Maps API キーを取得します。

最後の手順では、Google マップの API キー (従来の Google Maps v1 から API キーを再利用できないことに注意してください) を取得します。 取得および Xamarin.Android と API キーを使用する方法については、次を参照してください。 [A Google Maps API のキーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)です。
 


### <a name="specify-the-required-permissions"></a>必要なアクセス許可を指定します。

次のアクセス許可を指定する必要があります、 **AndroidManifest.XML** Google Maps Android api:

-  **ネットワークの状態へのアクセス**&ndash;マップ API は、マップのタイルをダウンロードできるかどうかを確認できる必要があります。

-  **インターネットにアクセスできる**&ndash;インターネット アクセスがマップのタイルをダウンロードして、API アクセス用 Google プレイ サーバーと通信するために必要です。

-  **OpenGL ES v2** &ndash;アプリケーションは、OpenGL ES v2 の要件を宣言する必要があります。

-  **Google Maps API キー** &ndash; API は、キーを使用して、アプリケーションが登録され、Google Play サービスの使用が許可されていることを確認します。 参照してください[Google Maps API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)詳細については、このキーはします。

-  **外部ストレージに書き込む** &ndash; Google マップ Android API は、外部ストレージにダウンロードしたタイルをキャッシュします。

-  **Google の Web ベースのサービスへのアクセス**&ndash;アプリケーション マップ API は、Android のバックアップを Google の web サービスにアクセスする権限が必要です。

-  **Google プレイ サービスの通知のアクセス許可**&ndash;アプリケーションが Google Play サービスからリモートの通知を受信する権限を与える必要があります。

-  **プロバイダーの場所へのアクセス**&ndash;これらは省略可能なアクセス許可。
   `GoogleMap`マップに、デバイスの場所を表示するクラス。


次のスニペットに追加する必要がある設定の例は、 **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

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
    </application>
</manifest>
```


## <a name="the-googlemap-class"></a>GoogleMap クラス

後、前提条件が満たさ、アプリケーションの開発を開始し、Android の Maps API を使用します。 [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap)クラスは、メインの API Xamarin.Android アプリケーションが表示され、Android 用 Google Maps との対話に使用します。 このクラスには、次の責任があります。

-  Google web サービスとアプリケーションを承認するために、Google Play サービスと対話します。

-  ダウンロードする、キャッシュ、およびマップのタイルを表示します。

-  UI コントロールを表示するなど、パンし、ユーザーにズームします。

-  マップのマーカーと幾何学図形を描画します。

`GoogleMap`が 2 つの方法の 1 つのアクティビティに追加します。

-  **MapFragment** - [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html)特殊化されたフラグメントのホストとして機能するは、`GoogleMap`オブジェクト。 `MapFragment` Android API レベル 12 以上が必要です。
   Android の古いバージョンを使用できる、 [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html)です。

-  **MapView** - [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/)のホストとして動作できる特殊なビュー サブクラス、`GoogleMap`オブジェクト。 このクラスのユーザーを転送する必要がありますすべてをアクティビティのライフ サイクル メソッドの`MapView`クラスです。

これらの各コンテナーを公開、`Map`のインスタンスを返すプロパティ`GoogleMap`です。 優先する必要があります、 [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html)クラスには、開発者を手動で実装する必要がある量定型コードを削減する単純な API。


### <a name="adding-a-mapfragment-to-an-activity"></a>アクティビティに、MapFragment を追加します。

次のスクリーン ショットは、非常に単純な例を示します`MapFragment`:

[![マップのフラグメントを表示するデバイスのスクリーン ショット](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

他のフラグメント クラスと同様に、ある 2 つの方法でこれを追加`MapFragment`アクティビティ。

-   **宣言的**- `MapFragment` XML レイアウト ファイルを使用して、アクティビティを追加できます。 次の XML スニペットを使用する方法の例を示しています、`fragment`要素。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **プログラムで**-`MapFragment`次のようにプログラミングで追加することができます。

プログラムで追加する、 `MapFragment`、アクティビティを実装する必要があります、`IOnMapReadyCallback`インターフェイスです。 の初期化、`GoogleMap`オブジェクトには、Google Play API と通信する際の完了に時間がかかり、アプリに通知されるコールバックを指定する必要があるときに、`GoogleMap`準備ができました。

最初に、追加`IOnMapReadyCallback`を`Activity`クラス宣言します。
例えば:

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

次に、`OnCreate`メソッドを追加、`MapFragment`次のコード例のように (、`GoogleMapOptions`クラスがこのガイドの後半で説明されている)。

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

A`GoogleMap`を使用して取得する必要があります`GetMapAsync`前のコード例の最後に示すように、&ndash;これは、マップのシステムと、ビューに自動的に初期化します。 (このメソッドを使用しない注`await` / `async`セマンティクス&ndash;、 `Async` Android によって動作を実装します)。ときに、 `GoogleMap` Android アプリの呼び出しは、オブジェクトが準備完了`OnMapReady`メソッド (の一部として実装する必要があります`IOnMapReadyCallback`インターフェイス)。 例えば:

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

上記のコード例では、`OnMapReady`コールバックを初期化、 `_map` 、作成した変数`GoogleMap`オブジェクト。

この結果を使用する方法の例としてとき`OnResume`が呼び出されると、かどうかを確認できます`_map`以外の場合します。 場合`_map`に設定されている、`GoogleMap`オブジェクト、`OnResume`マーカーを追加して、指定した経度と緯度にそのカメラを移動する場合がメソッドを呼び出すことができます。 完全なコード例は、次を参照してください。 [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)です。



### <a name="map-types"></a>マップの種類

5 つの異なる種類のマップは、Google マップ API から入手できます。

-  **標準**-これは、既定のマップの種類。 道路とと共に一部人為的ポイント (建物ブリッジなど) 関心のある重要な自然な機能が表示されます。

-  **サテライト**-このマップは、衛星写真を示しています。

-  **ハイブリッド**- このマップは、衛星写真を表示し、道路マップします。

-  **地形**-一部道路と機能を地形、主に表示します。

-  **None** -このマップは、タイルを読み込みません、空のグリッドとしてレンダリングされます。


次の図は、さまざまな種類のマップの左から右 (通常、ハイブリッド、地形) から 3 つを示しています。

[![3 つのマップの例のスクリーン ショット: 通常、ハイブリッド、および地形](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType`を設定または表示するマップの種類を変更するプロパティを使用します。 次のコード スニペットは、サテライト マップを表示する方法を示します。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>GoogleMap プロパティ

`GoogleMap` 機能およびマップの外観を制御するいくつかのプロパティを定義します。 初期状態を構成する方法の 1 つ、`GoogleMap`を渡すには、 [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html)オブジェクトを作成するとき、`MapFragment`です。 次のコード スニペットは、1 つの例を使用する、`GoogleMapOptions`オブジェクトを作成するとき、 `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

構成するのには別の方法、`GoogleMap`値を設定して、オブジェクトが、 [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html)のマップ オブジェクトのプロパティです。 次のコード サンプルを構成する方法を示しています、`GoogleMap`ズーム コントロールとは、コンパスを表示します。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>マップとの対話

マップの Android API には、ビュー ポイントを変更する、マーカーを追加、カスタムのオーバーレイを配置または幾何学図形を描画するためのアクティビティを許可する API が用意されています。 このセクションでは、いくつかの Xamarin.Android でこれらのタスクを実行する方法を説明します。

### <a name="changing-the-viewpoint"></a>視点を変更します。

マップは、は Mercator 投影法に基づく画面で、フラットな面として modelled はします。 マップ ビューは、*カメラ*この平面のまっすぐ探し求めています。 カメラの位置は、場所、ズームの傾きを変更して影響を制御できます。 [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate)カメラの位置を移動するクラスを使用します。 `CameraUpdate` オブジェクトは直接インスタンス化されません、マップの API は、代わりに、 [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html)クラスです。

1 回、`CameraUpdate`オブジェクトが作成された、いずれかをパラメーターとして渡される、 [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera%28com.google.maps.CameraUpdate%29)または[GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera%28com.google.maps.CameraUpdate%29)メソッドです。 `MoveCamera`メソッドを更新中にすぐに、マップ、`AnimateCamera`メソッドは、smooth、動画の遷移を提供します。

このコード スニペットは、使用する方法の簡単な例を示します、`CameraUpdateFactory`を作成する、`CameraUpdate`いずれかによって、マップのズーム レベルを 1 ずつ増分されます。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

マップの API を提供、 [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html)カメラの位置の有効な値のすべてを集計するされます。 このクラスのインスタンスに提供できる、 [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29)を返すメソッド、`CameraUpdate`オブジェクト。 マップの API も含まれています、 [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html)を作成するため、fluent API を提供するクラス`CameraPosition`オブジェクト。
次のコード スニペットを作成する例を示しています、`CameraUpdate`から、`CameraPosition`でカメラの位置を変更するを使用して、 `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

前のコード スニペットでは、マップ上の特定の場所で表される、 [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng)クラスです。 ズーム レベルを 18 に設定するとします。 影響は、北部から時計回りにコンパスの測定値です。 プロパティは、表示角度を制御しは傾き、垂直方向から 25 度の角度を指定します。 次のスクリーン ショットでは、`GoogleMap`上記のコードを実行した後。

[![表示する角度を傾斜するいると、指定した場所を示す例 Google マップ](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>マップの描画

Android のマップ API は、マップで、次の項目を描画するための API を提供します。

-  **マーカー** -これらは、マップ上の 1 つの場所を識別するために使用される特別なアイコンです。

-  **オーバーレイ**-これは、イメージの場所、または、マップ上の領域のコレクションを識別するために使用できます。

-  **線、多角形、および円**-これらは、マップに図形を追加するアクティビティを許可する Api。


#### <a name="markers"></a>Markers

マップの API を提供、[マーカー](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker)のすべてのマップ上の 1 つの場所に関するデータをカプセル化するクラス。 既定では、Google マップによって提供される標準的なアイコンを使用します。 ユーザーのクリックに応答するマーカーの外観をカスタマイズする可能性があります。


##### <a name="adding-a-marker"></a>マーカーを追加します。

マップにマーカーを追加する必要があるを新規作成[MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions)オブジェクトを呼び出す、 [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29)メソッドを`GoogleMap`インスタンス。 このメソッドは、[マーカー](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker)オブジェクト。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(markerOpt1);
}
```

マーカーのタイトルが表示されます、*情報ウィンドウ*マーカーをタップするとします。 次のスクリーン ショットは、このマーカーの外観を示します。

[![マーカーとヴィミーねじ山の情報 ウィンドウが、Google マップの例](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>マーカーのカスタマイズ

呼び出すことによって、マーカーで使用されるアイコンのカスタマイズ可能であれば、`MarkerOptions.InvokeIcon`メソッドをマップにマーカーを追加するときにします。
このメソッドは、 [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html)アイコンを表示するために必要なデータを含むオブジェクト。 [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html)クラスの作成を簡略化のヘルパー メソッドを提供する、`BitmapDescriptor`です。 次の一覧では、これらのメソッドのいくつか導入されています。

-   `DefaultMarker(float colour)` &ndash; 既定の Google Maps マーカーを使用してがの色に変更します。

-   `FromAsset(string assetName)` &ndash; Assets フォルダー内の指定したファイルからカスタム アイコンを使用します。

-   `FromBitmap(Bitmap image)` &ndash; 指定したビットマップをアイコンとして使用します。

-   `FromFile(string fileName)` &ndash; 指定したパスにあるファイルからカスタム アイコンを作成します。

-   `FromResource(int resourceId)` &ndash; 指定されたリソースからカスタム アイコンを作成します。

次のコード スニペットは、シアンな既定のマーカーを作成する例を示しています。

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(markerOpt1);
}
```


#### <a name="info-windows"></a>すべての情報ウィンドウ

*すべての情報ウィンドウ*特定マーカーをタップしたときに、ユーザーに情報を表示するには、そのポップアップの特殊なウィンドウは、します。 既定では、情報ウィンドウに、マーカーのタイトルの内容が表示されます。 タイトルが割り当てられていない場合の情報 ウィンドウは表示されません。 1 つだけの情報ウィンドウが同時に表示する可能性があります。

実装することで、情報ウィンドウをカスタマイズ可能であれば、 [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter)インターフェイスです。 このインターフェイスでは、次の 2 つの重要なメソッドがあります。

-  `public View GetInfoWindow(Marker marker)` &ndash; このメソッドが呼び出されて、マーカーのカスタムの情報ウィンドウを取得します。 返された場合`null`、その既定のウィンドウのレンダリングが使われます。 このメソッドを返す場合、ビュー、そのビューは、情報ウィンドウ フレーム内配置されます。

-  `public View GetInfoContents(Marker marker)` &ndash; このメソッドは、GetInfoWindow を返す場合にのみ呼び出す`null`です。 このメソッドが返すことができます、`null`以外の場合、情報ウィンドウの内容の既定のレンダリングが使用される値します。 それ以外の場合、このメソッドは、情報ウィンドウの内容を含むビューを返す必要があります。

情報ウィンドウがライブ ビューではありません - Android は、ビューを静止ビットマップに変換し、イメージ上に表示される代わりにします。 つまり、情報ウィンドウは、タッチ イベントまたはジェスチャに応答できないことも、自動的に自動的に更新されます。 情報ウィンドウを更新する必要があるを呼び出して、 [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow())メソッドです。

次の図は、いくつかのカスタマイズされた情報ウィンドウの例を示します。 左側のイメージに存在のコンテンツをカスタマイズすると、右上のイメージがのウィンドウ、およびカスタマイズの内容。

![アイコンをクリックしてカタログの作成を含む、メルボルンの例のマーカー windows です。 右側のウィンドウには、角が丸いです。](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>地上オーバーレイ

マーカーは、マップ上の特定の場所を特定するとは異なり、 [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html)イメージの場所または、マップ上の領域のコレクションを識別するために使用します。


##### <a name="adding-a-groundoverlay"></a>GroundOverlay を追加します。

マップに地上オーバーレイを追加することは、マーカーをマップに追加する場合によく似ています。 最初に、 [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html)オブジェクトを作成します。 このオブジェクトはパラメーターとして渡され、`GoogleMap.AddGroundOverlay`を返すメソッド、`GroundOverlay`オブジェクト。 このコード スニペットは、マップに地上オーバーレイを追加する例を示します。

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

次のスクリーン ショットは、マップ上のこのオーバーレイを示しています。

[![極座標グラフ下げのオーバーレイをイメージ マップの例](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>線、円、および多角形

マップに追加できる幾何学模様の図の 3 つの単純な種類があります。

-  **ポリライン**-これは、一連の接続されている直線セグメント。 マップ上のパスをマークすることも必要な任意の図形をフォームします。

-  **多角形**-これは、閉じた図形マップ上の領域をマークします。

-  **円**-このマップに円を描画するは。



##### <a name="polylines"></a>多角形

A[ポリライン](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline)連続の一覧を示します`LatLng`各直線セグメントの頂点を指定するオブジェクト。 ポリラインが最初に作成することで作成された、`PolylineOptions`オブジェクトをポイントを追加するとします。 `PolylineOption`にオブジェクトが渡され、`GoogleMap`オブジェクトを呼び出して、`AddPolyline`メソッドです。

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>多角形

`Polygon`よく似ている`Polyline`s、ただしは開いていないが終了しました。 `Polygon`s は閉じたループであり、入力された状態で、内部を持っています。
`Polygon`まったく同じ方法で作成される、 `Polyline`、を除き、 [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions))メソッドが呼び出されました。

異なり、 `Polyline`、`Polygon`自己終了します。 ときに`AddPolygon`が呼び出されると、メソッドは自動的に閉じて最初と最後の点を結ぶ線を描画して、多角形オフします。 次のコード スニペットはで、前のコード スニペットと同じ領域を純色の四角形を作成、`Polyline`例です。

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>円

円が最初のインスタンス化して作成された、 [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions)メートルでセンターでは、円の半径を指定するオブジェクト。 円が呼び出すことによって、マップに描画される[GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions))です。
次のコード スニペットは、円を描画する方法を示しています。

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (circleOptions);
```


## <a name="responding-to-events"></a>イベントに応答します。

マップされているユーザーの相互作用の 3 つの種類があります。

-  **マーカーをクリックして**-ユーザーが、マーカーをクリックします。

-  **マーカー ドラッグ**-ユーザーが時間の長い-クリックして、mparger 上

-  **情報ウィンドウをクリックして**-情報ウィンドウで、ユーザーがクリックしました。

これらのイベントについてで詳しく説明します。


### <a name="marker-click-events"></a>マーカーのクリックしてイベント

マーカーで、ユーザーがクリックしたとき、`MarkerClick`イベントを発生させると`GoogleMap.MarkerClickEventArgs`で渡されます。 このクラスには、2 つのプロパティが含まれています。

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; このプロパティに設定する必要があります`true`をイベント ハンドラーがイベントを消費することを示します。 設定されている場合`false`イベント ハンドラーのカスタム動作に加えて、既定の動作が発生し、します。

-  `P0` &ndash; このが不十分な name パラメーターは、マーカーを発生させたへの参照、`MarkerClick`イベント。


このコード スニペットの例を示しています、`MarkerClick`そのカメラの位置をマップ上の新しい場所に変更されます。

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>マーカー ドラッグ イベント

このイベントは、マーカーをドラッグするときに発生します。 既定では、マーカーはドラッグできません。 マーカーを設定できるようドラッグ可能な設定、`Marker.Draggable`プロパティを`true`を呼び出すことにより、`MarkerOptions.Draggable`メソッドを`true`をパラメーターとして。

最初にマーカーをドラッグして、ユーザー必要があります時間の長い にし、マップ上に指を保持します。 画面の周りには、その指をドラッグするときにマーカーが移動します。 ユーザーの本の指を画面の外離した、マーカーは場所に残ります。

次に、ドラッグ可能な指標は、発生するさまざまなイベントを説明します。

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; このイベントは、ユーザーが最初にマーカーをドラッグしたときに発生します。

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; このイベントは、マーカーがドラッグされているとします。

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; このイベントは、ときに when、ユーザーが終了マーカーをドラッグします。

各、`EventArgs`と呼ばれる 1 つのプロパティを含む`P0`への参照は、`Marker`ドラッグされているオブジェクトします。


### <a name="info-window-click-events"></a>情報ウィンドウのクリックしてイベント

一度に 1 つだけの情報ウィンドウを表示できます。 マップ内の情報ウィンドウで、ユーザーがクリックすると、マップのオブジェクトが生成されます、`InfoWindowClick`イベント。 次のコード スニペットは、イベントにハンドラーを接続するための方法を示しています。

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

情報ウィンドウは、静的な`View`これは、マップ上の画像としてレンダリングされます。 ボタン、チェック ボックス、または情報のウィンドウ内に配置されているテキスト ビューなどのすべてのウィジェットはのみになり、整数のユーザー イベントのいずれかに応答できません。



## <a name="related-links"></a>関連リンク

- [Google Play サービス](http://developer.android.com/google/play-services/index.html)
- [Google Android API v2 のマップ](https://developers.google.com/maps/documentation/android/)
- [Google Play サービス APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google Maps API キーを取得します。](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
