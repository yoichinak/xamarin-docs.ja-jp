---
title: アプリケーションで Google マップ API を使用します。
description: Xamarin.Android アプリケーションに Google マップ API v2 の機能を実装する方法。
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: 1889154a12a701fb4ce57ef8644699dd978f768e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61186295"
---
# <a name="using-the-google-maps-api-in-your-application"></a>アプリケーションで Google マップ API の使用

マップ アプリケーションを使用するには優れていますが、アプリケーションに直接マップを追加することがあります。 だけでなく、組み込みマップ アプリケーション、Google も提供しています、 [for Android のネイティブ マッピング API](https://developers.google.com/maps/documentation/android-sdk/intro)します。
マップの API は、マッピングのエクスペリエンスをさらに制御を維持する必要がある場合に適しています。 マップの API で使用できるものは次のとおりです。

-  マップのビュー ポイントをプログラム的に変更します。
-  追加して、マーカーをカスタマイズします。
-  オーバーレイを使用してマップに注釈を付けます。

ここで非推奨と Google Maps Android API v1 とは異なりの一部である Google Maps Android API v2 [Google play 開発者サービス](https://developers.google.com/android/guides/overview)します。
Xamarin.Android アプリは Google Maps Android API を使用することは前に、いくつかの必須の前提条件を満たす必要があります。


## <a name="google-maps-api-prerequisites"></a>Google マップ API の前提条件

マップの API を使用する前に実行する必要があるいくつかの手順を含みます。

-  [マップ API キーを取得します。](#obtain-maps-key)
-  [Google Play Services SDK をインストールします。](#install-gps-sdk)
-  [NuGet から Xamarin.GooglePlayServices.Maps パッケージをインストールします。](#install-gpsmaps-nuget)
-  [必要なアクセス許可を指定します。](#declare-permissions)
-  [必要に応じて、Google Api を使用した、エミュレーターを作成します。](#create-emulator-with-google-api)


### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Google マップ API キーを入手します。

最初の手順では、Google マップ API キー (従来の Google Maps v1 API から API キーを再利用できないことに注意してください) を取得します。 取得して、Xamarin.Android で API キーを使用する方法については、次を参照してください。 [A Google マップ API のキーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)します。


### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Google Play Services SDK をインストールします。

Google play 開発者サービスは、Google +、アプリ内課金、およびマップなどのさまざまな Google 機能を活用するために Android アプリケーションを使用する Google からのテクノロジです。 含まれているバック グラウンド サービスとして、これらの機能は Android デバイスでアクセス可能な[Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)します。

Android アプリケーションは、Google play 開発者サービスのクライアント ライブラリを介した Google play 開発者サービスと対話します。 このライブラリには、インターフェイスと、個々 のサービス マップなどのクラスが含まれています。 次の図は、Android アプリケーションと Google play 開発者サービスの間のリレーションシップを示しています。

![Google Play Services APK の更新、Google Play ストアを示す図](maps-api-images/play-services-diagram.png)

マップの Android API は、Google play 開発者サービスの一部として提供されます。
使用して Google Play Services SDK をインストールする必要があります、Xamarin.Android アプリケーションには、Maps API を使用できます、前に、 [Android SDK Manager](~/android/get-started/installation/android-sdk.md)します。 次のスクリーン ショットは、Google Play サービス クライアントを検出できる Android SDK Manager で場所を示します。

![Google play 開発者サービスが Extras Android SDK Manager で下に表示されます。](maps-api-images/image01.png)

> [!NOTE]
> Google Play services APK は、すべてのデバイス上に存在することができない可能性があるライセンスされた製品です。 インストールされていないデバイスで Google Maps は機能しません。

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> NuGet から Xamarin.GooglePlayServices.Maps パッケージをインストールします。

[Xamarin.GooglePlayServices.Maps パッケージ](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps)Google Play Services マップ API の Xamarin.Android バインドが含まれています。
Google Play Services のマップ パッケージを追加するを右クリックし、**参照**ソリューション エクスプ ローラーで、プロジェクトのフォルダー **NuGet パッケージの管理.**:

![ソリューション エクスプ ローラーが表示された 参照の NuGet パッケージの管理コンテキスト メニュー項目](maps-api-images/image02.png)

開き、 **NuGet パッケージ マネージャー**します。 をクリックして**参照**入力**Xamarin Google Play Services のマップ**検索フィールドにします。 選択**Xamarin.GooglePlayServices.Maps**クリック**インストール**します。 (このパッケージが既にインストールされていた場合はクリックして**Update**。)。

[![選択した Xamarin.GooglePlayServices.Maps パッケージで NuGet パッケージ マネージャー](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

次の依存関係パッケージもインストールされていることに注意してください。

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" /> 必要なアクセス許可を指定します。

アプリでは、Google マップ API を使用するには、ハードウェアとアクセス許可の要件を特定する必要があります。  一部のアクセス許可が Google Play Services SDK によって自動的に付与して、開発者に明示的に追加する必要はありません**AndroidManfest.XML**:

-  **ネットワークの状態へのアクセス**&ndash;マップ API は、マップのタイルをダウンロードできるかどうかを確認できる必要があります。

-  **インターネットにアクセスできる**&ndash;インターネットにアクセスできるマップ タイルをダウンロードして、API へのアクセス、Google Play サーバーと通信する必要があります。

次のアクセス許可と機能を指定する必要があります、 **AndroidManifest.XML** Google Maps Android api:

-  **OpenGL ES v2** &ndash;アプリケーションは、OpenGL ES v2 の要件を宣言する必要があります。

-  **Google マップ API キー** &ndash; API キーを使用して、アプリケーションが登録され、Google play 開発者サービスの使用が許可されていることを確認します。 参照してください[Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)詳細については、このキー。

- **従来の Apache HTTP クライアントに要求** &ndash; Android 9.0 (API レベル 28) を対象とするアプリまたはレガシ Apache HTTP クライアントは、オプションのライブラリを使用すること以上する必要があります指定します。

-  **Google Web ベースのサービスへのアクセス**&ndash;アプリケーションには、マップの Android API をバックアップする Google の web サービスへのアクセス許可が必要があります。

-  **Google Play Services の通知のアクセス許可**&ndash;アプリケーションに Google play 開発者サービスからリモート通知を受信する権限を許可する必要があります。

-  **場所プロバイダーへのアクセス**&ndash;これらは省略可能なアクセス許可。
   `GoogleMap`クラスをマップに、デバイスの場所を表示します。

さらに、Android 9 では、Apache HTTP クライアント ライブラリは、bootclasspath から削除し、ないので以上 API 28 を対象とするアプリケーションを使用できます。 次の行を追加する必要があります、`application`のノード、 **AndroidManifest.xml**は引き続き 28 またはそれ以降の API を対象とするアプリケーションでの Apache HTTP クライアントを使用するファイル。

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Google Play SDK の非常に古いバージョンを要求するアプリに必要な`WRITE_EXTERNAL_STORAGE`権限。 この要件は、Google play 開発者サービス用の最新の Xamarin バインドで必要はなくなりました。

次のスニペットに追加する必要があります設定の例は、 **AndroidManifest.XML**:

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

アクセス許可を要求するだけでなく**AndroidManifest.XML**、アプリのランタイム アクセス許可のチェックを実行する必要があります、`ACCESS_COARSE_LOCATION`と`ACCESS_FINE_LOCATION`アクセス許可。 参照してください、 [Xamarin.Android 権限](~/android/app-fundamentals/permissions.md)実行時のアクセス許可のチェックを実行する方法についてのガイド。


### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Google Api を使用した、エミュレーターを作成します。

物理 Android デバイスで Google play 開発者サービスがインストールされていないこと、開発用のエミュレーター イメージを作成することができます。 詳細については、次を参照してください。、[デバイス マネージャー](~/android/get-started/installation/android-emulator/device-manager.md)します。


## <a name="the-googlemap-class"></a>GoogleMap クラス

前提条件が満たされていると、アプリケーションの開発を開始し、Android のマップ API を使用する時間になります。 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap)クラスは、Xamarin.Android アプリケーションが表示され、Android 用 Google Maps との対話に使用する主要な API。 このクラスは、次の責任には。

-  Google web サービスとアプリケーションを承認するために Google Play サービスと対話します。

-  ダウンロード、キャッシュ、およびマップのタイルを表示します。

-  などの UI コントロールを表示して、パン、ユーザーにズームします。

-  マップのマーカーと幾何学的図形を描画します。

`GoogleMap` 2 つの方法の 1 つのアクティビティに追加されます。

-  **MapFragment** - [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment)のホストとして機能する特殊なフラグメントは、`GoogleMap`オブジェクト。 `MapFragment` Android API レベル 12 以上が必要です。
   以前のバージョンの Android を使用できる、 [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)します。  このガイドは使用に重点を`MapFragment`クラス。

-  **MapView** - [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView)のホストとして動作できる特殊なビュー サブクラスは、`GoogleMap`オブジェクト。 このクラスのユーザーには、すべてのアクティビティのライフ サイクル メソッドを転送する必要があります、`MapView`クラス。

これらの各コンテナーを公開、`Map`のインスタンスを返すプロパティ`GoogleMap`します。 優先する必要があります、 [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment)クラスには、開発者が実装する必要があります手動で実行する量の定型コードを削減するより単純な API。

### <a name="adding-a-mapfragment-to-an-activity"></a>アクティビティへ、MapFragment の追加

次のスクリーン ショットのような簡単な`MapFragment`:

[![Google マップ フラグメントを表示するデバイスのスクリーン ショット](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

フラグメントの他のクラスと同様に、ある 2 つの方法を追加する、`MapFragment`アクティビティ。

-   **宣言によって**- `MapFragment` XML のレイアウト ファイルを使用して、アクティビティを追加できます。 次の XML スニペットを使用する方法の例を示しています、`fragment`要素。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **プログラムで**-`MapFragment`を使用してプログラムでインスタンス化できる、 [ `MapFragment.NewInstance` ](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance())メソッド アクティビティに追加します。 このスニペットは、インスタンス化する最も簡単な方法を示しています、`MapFragment`オブジェクトし、アクティビティを追加します。

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    構成することは、`MapFragment`オブジェクトを渡すことによって、 [ `GoogleMapOptions` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions)オブジェクトを`NewInstance`します。 これは、セクションで説明[GoogleMap プロパティ](#googlemap_object)このガイドで後で表示されます。

`MapFragment.GetMapAsync`メソッドが初期化に使用される、 [ `GoogleMap` ](#googlemap_object)フラグメントによってホストされ、によってホストされているマップ オブジェクトへの参照を取得する、`MapFragment`します。 このメソッドを実装するオブジェクトには、`IOnMapReadyCallback`インターフェイス。

このインターフェイスは、1 つのメソッドを持って`IMapReadyCallback.OnMapReady(MapFragment map)`することは、アプリと対話するときに呼び出される、`GoogleMap`オブジェクト。 次のコード スニペットは、Android の Activity を初期化できる方法を示しています、`MapFragment`を実装し、`IOnMapReadyCallback`インターフェイス。
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

Google マップ API から使用可能な 5 つの異なる種類のマップは。

-  **標準**-これは、既定のマップの種類。 道路と (建物ブリッジなど) 関心のある人為的なポイントとの重要な自然な機能が表示されます。

-  **サテライト**-このマップには、衛星写真が表示されます。

-  **ハイブリッド**- このマップには、衛星写真が表示されます。 および、道路マップします。

-  **地形**-一部の道路で地形の特徴が主に表示されます。

-  **None** -このマップでは、タイルが読み込まれません、これは空のグリッドとして表示されます。


次の図は、左から右 (通常、ハイブリッド、地形) から、マップのさまざまな種類のうち 3 つを示しています。

[![3 つのマップのスクリーン ショットの例。通常、ハイブリッド、および地形](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType`プロパティを使用して設定または変更するマップの種類が表示されます。 次のコード スニペットでは、サテライト マップを表示する方法を示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```


### <a name="a-namegooglemapobject-googlemap-properties"></a><a name="googlemap_object" />GoogleMap プロパティ

`GoogleMap` 機能と、マップの外観を制御するいくつかのプロパティを定義します。 初期状態を構成する方法の 1 つ、`GoogleMap`を渡すには、 [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions)オブジェクトの作成時に、`MapFragment`します。 次のコード スニペットは、1 つの例を使用する、`GoogleMapOptions`オブジェクトの作成時に、 `MapFragment`:

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

もう 1 つの構成、`GoogleMap`でプロパティを操作することでは、 [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings)マップ オブジェクトの。 次のコード サンプルを構成する方法を示しています、`GoogleMap`ズーム コントロールと、コンパスを表示します。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>GoogleMap と対話します。

マップの Android API には、視点を変更する、マーカーの追加、カスタムのオーバーレイを配置または幾何学的図形を描画するためのアクティビティを許可する Api が提供されます。 このセクションでは、Xamarin.Android でこれらのタスクの一部を実行する方法について説明します。

### <a name="changing-the-viewpoint"></a>視点を変更します。

マップはメルカトル投影法に基づく画面で、平面としてモデル化します。 マップ ビューは、*カメラ*この平面の見栄えをまっすぐ下。 カメラの位置は、場所、ズームの傾きを変更して、方位で制御できます。 [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate)クラスを使用すると、カメラの位置を移動します。 `CameraUpdate` オブジェクトは直接インスタンス化されない、マップの API が提供する代わりに、 [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory)クラス。

1 回、`CameraUpdate`オブジェクトが作成されたら、いずれかをパラメーターとして渡される、 [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate))または[GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate))メソッド。 `MoveCamera`メソッドは更新中にすぐに、マップ、`AnimateCamera`メソッドは、smooth、アニメーション遷移を提供します。

このコード スニペットは、使用する方法の簡単な例を示します、`CameraUpdateFactory`を作成する、 `CameraUpdate` 1 つのズーム レベルによって、マップのズーム レベルを 1 ずつ増分されます。

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

マップの API を提供、 [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html)するすべてのカメラの位置で使用できる値が集計されます。 このクラスのインスタンスに提供できる、 [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29)メソッドが返されます、`CameraUpdate`オブジェクト。 マップの API も含まれています、 [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html)クラスを作成するために、fluent API を提供する`CameraPosition`オブジェクト。
次のコード スニペットを作成する例を示しています、`CameraUpdate`から、`CameraPosition`でカメラの位置を変更するを使用して、 `GoogleMap`:

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

前のコード スニペットでは、マップ上の特定の場所で表される、 [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng)クラス。 ズーム レベルは、Google マップで使用されるズームの任意のメジャーは、18 に設定されます。 方位とは、北から時計回りコンパスの測定値です。 傾きプロパティは、表示角度を制御し、垂直方向から 25 度の角度を指定します。 次のスクリーン ショット、`GoogleMap`上記のコードを実行した後。

[![Google マップの例に、傾斜するいると指定した場所を示す角度を表示します。](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>マップの描画

Android のマップ API では、マップに次の項目を描画するための API を提供します。

-  **マーカー** -これらは、マップ上の 1 つの場所を識別するために使用される特別なアイコン。

-  **オーバーレイ**-これは、イメージの場所、または、マップ上の領域のコレクションを識別するために使用できます。

-  **線、多角形、および円**-これらは、Api をマップに図形を追加するアクティビティを使用します。


#### <a name="markers"></a>Markers

マップの API を提供します、[マーカー](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker)クラスは、マップ上の 1 つの場所に関するデータがすべてカプセル化します。 既定では、マーカーは、クラスは、Google マップによって提供される標準的なアイコンを使用します。 マーカーの外観をカスタマイズして、ユーザーのクリックに応答することができます。


##### <a name="adding-a-marker"></a>マーカーの追加

マップにマーカーを追加する必要がありますを新規作成[MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions)オブジェクトを呼び出して、 [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29)メソッドを`GoogleMap`インスタンス。 このメソッドは、[マーカー](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker)オブジェクト。

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

マーカーのタイトルが表示されます、*情報ウィンドウ*マーカーをタップするとします。 このマーカーの次のスクリーン ショットに示します。

[![マーカーとヴィミーねじ山の情報 ウィンドウが、Google マップの例](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>マーカーのカスタマイズ

呼び出すことによって、マーカーによって使用されるアイコンをカスタマイズすることができます、`MarkerOptions.InvokeIcon`メソッドをマップにマーカーを追加するときにします。
このメソッドは、 [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor)アイコンを表示するために必要なデータを格納しているオブジェクト。 [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory)クラスの作成を簡略化には、いくつかヘルパー メソッドを提供する、`BitmapDescriptor`します。 次の一覧では、これらのメソッドのいくつか導入されています。

-   `DefaultMarker(float colour)` &ndash; 既定の Google Maps マーカーを使用してがの色に変更します。

-   `FromAsset(string assetName)` &ndash; Assets フォルダーに指定したファイルからカスタム アイコンを使用します。

-   `FromBitmap(Bitmap image)` &ndash; 指定したビットマップのアイコンとして使用します。

-   `FromFile(string fileName)` &ndash; 指定したパスにファイルからカスタム アイコンを作成します。

-   `FromResource(int resourceId)` &ndash; 指定されたリソースからのカスタム アイコンを作成します。

次のコード スニペットは、シアン色つきの既定のマーカーを作成する例を示しています。

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

#### <a name="info-windows"></a>すべての情報ウィンドウ

*情報 windows*特殊な windows の特定のマーカーをタップしたときに、ユーザーに情報を表示するには、そのポップアップが。 情報ウィンドウでは既定では、マーカーのタイトルの内容が表示されます。 タイトルが割り当てられていない場合の情報 ウィンドウは表示されません。 一度に 1 つだけの情報ウィンドウを表示可能性があります。

実装することによって、情報ウィンドウをカスタマイズすることができます、 [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter)インターフェイス。 このインターフェイスでは、2 つの重要なメソッドがあります。

-  `public View GetInfoWindow(Marker marker)` &ndash; このメソッドが呼び出されて、マーカーのカスタム情報ウィンドウを取得します。 返された場合`null`既定のウィンドウのレンダリングが使用されます。 このメソッドがビューに戻る場合、情報のウィンドウ フレーム内でそのビューが配置されます。

-  `public View GetInfoContents(Marker marker)` &ndash; GetInfoWindow を返す場合、このメソッドは呼び出さのみ`null`します。 このメソッドが返すことができます、`null`以外の場合、情報ウィンドウの内容の既定のレンダリングが使用される値します。 それ以外の場合、このメソッドは、情報ウィンドウの内容を含むビューを返す必要があります。

情報ウィンドウがライブ ビューではありません - Android は、ビューを静的なビットマップに変換し、イメージ上に表示する代わりにします。 つまり、情報ウィンドウがジェスチャ、またはタッチ イベントに応答できないことが自動的に自動的に更新されます。 呼び出す必要は、情報ウィンドウを更新する、 [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow())メソッド。

次の図は、一部のカスタマイズされた情報ウィンドウの例をいくつかを示します。 左側のイメージでは、その内容をカスタマイズすると、右側のイメージは、ウィンドウや角が丸いでカスタマイズ内容があります。

![メルボルン、アイコン、カタログの作成などの例のマーカー windows。 右側のウィンドウには、角が丸められます。](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

マーカーで、マップ上の特定の場所を特定するとは異なり、 [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay)が場所や、マップ上の領域のコレクションを識別するために使用されるイメージです。

##### <a name="adding-a-groundoverlay"></a>GroundOverlay を追加します。

マップへの地上オーバーレイの追加は、マップにマーカーを追加することに似ています。 まず、 [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions)オブジェクトが作成されます。 このオブジェクトがパラメーターとして渡されますし、 [ `GoogleMap.AddGroundOverlay` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions))を返すメソッドを`GroundOverlay`オブジェクト。 このコード スニペットでは、地上オーバーレイをマップに追加する例を示します。

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

次のスクリーン ショットは、マップ上のこのオーバーレイを示しています。

[![極座標グラフのぬいぐるみの店舗のイメージを使ってマップの例](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>線、円、および多角形

マップに追加できる幾何学的図形の 3 つの単純な種類があります。

-  **ポリライン**-これは接続された線分のシリーズです。 幾何学図形を作成、マップ上のパスをマークすることもできます。

-  **円**-このマップに円を描くには。

-  **多角形**-マップ上の領域をマークするため、閉じた形状になります。


##### <a name="polylines"></a>ポリライン

A[ポリライン](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline)は一連の連続する`LatLng`各直線セグメントの頂点を指定するオブジェクト。 ポリラインが最初の作成によって作成された、`PolylineOptions`オブジェクトとをポイントを追加します。 `PolylineOption`オブジェクトに渡されます、`GoogleMap`オブジェクトを呼び出すことによって、`AddPolyline`メソッド。

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

円は、最初のインスタンス化によって作成された、 [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions)センターでは、円の半径をメートルで指定するオブジェクト。 円は、呼び出すことによって、マップに描画される[GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions))します。
次のコード スニペットでは、円を描画する方法を示します。

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```


##### <a name="polygons"></a>多角形

`Polygon`s と似ています`Polyline`s、開いていないが終了しました。 `Polygon`s の閉じたループで、入力、内部です。
`Polygon`まったく同じ方法で作成、 `Polyline`、を除き、 [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions))メソッドが呼び出されます。

異なり、 `Polyline`、`Polygon`は自己終了します。 多角形はによってオフ閉じられます、`AddPolygon`最初と最後の点を結ぶ線を描画することによってメソッド。 次のコード スニペットは前のコード スニペットと同じ領域に実線の四角形を作成、`Polyline`例。

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```


## <a name="responding-to-user-events"></a>ユーザー イベントに応答します。

マップされているユーザーの相互作用の 3 つの種類があります。

-  **マーカーをクリックして**-ユーザーが、マーカーをクリックします。

-  **マーカーのドラッグ**-ユーザーが時間の長い-クリックしてで、mparger

-  **情報ウィンドウをクリックして**-ユーザーが、情報ウィンドウでをクリックします。

これらの各イベントについては、以下で詳しく説明します。


### <a name="marker-click-events"></a>マーカーのクリックしてイベント

`MarkerClicked`マーカーで、ユーザーがタップしたときにイベントが発生します。 このイベントを受け入れる、`GoogleMap.MarkerClickEventArgs`オブジェクトをパラメーターとして。 このクラスには、2 つのプロパティが含まれています。

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; このプロパティに設定する必要があります`true`をイベント ハンドラーがイベントを消費されることを示します。 設定されている場合`false`し、カスタム イベント ハンドラーの動作だけでなく、既定の動作が発生します。

-  `Marker` &ndash; このプロパティは、マーカーを発生させたへの参照、`MarkerClick`イベント。


このコード スニペットの例を示します、`MarkerClick`マップ上の新しい場所に、カメラの位置を変更するされます。

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


### <a name="marker-drag-events"></a>マーカーのドラッグ イベント

マーカーをドラッグするときに、このイベントが発生します。 既定では、マーカーはドラッグできません。 マーカーを設定できる、ドラッグ可能な設定、`Marker.Draggable`プロパティを`true`を呼び出すことにより、`MarkerOptions.Draggable`メソッド`true`をパラメーターとして。

マーカーをドラッグするユーザー必要があります最初時間の長い マーカーのし、マップ上の指がしおく必要があります。 ユーザーの指を画面上をドラッグすると、マーカーが移動します。 ユーザーの指が画面の外を離したときに、マーカーは場所に残ります。

次の一覧には、マーカーをドラッグ可能なは、発生するさまざまなイベントについて説明します。

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; ユーザーが最初にマーカーをドラッグすると、このイベントが発生します。

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; マーカーがドラッグされていると、このイベントが発生します。

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; ユーザーが終了すると、このイベントが発生しますマーカーをドラッグします。

各、`EventArgs`という単一のプロパティを含む`P0`への参照は、`Marker`ドラッグされているオブジェクトします。


### <a name="info-window-click-events"></a>情報ウィンドウのクリックしてイベント

一度に 1 つだけの情報ウィンドウを表示できます。 ユーザーは、マップで、[情報] ウィンドウでクリックすると、map オブジェクトが発生する`InfoWindowClick`イベント。 次のコード スニペットでは、イベントにハンドラーを接続する方法を示します。

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

情報ウィンドウが静的であることを思い出してください`View`これは、マップ上の画像としてレンダリングされます。 ボタン、チェック ボックス、または情報のウィンドウ内に配置されているテキスト ビューなどの任意のウィジェットを使用して、模擬なります、その整数ユーザー イベントのいずれかに応答できません。


## <a name="related-links"></a>関連リンク

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google play 開発者サービスします。](https://developers.google.com/android/guides/overview)
- [Google Android API v2 をマップします。](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google マップ API キーを取得します。](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [使用して機能](https://developer.android.com/guide/topics/manifest/uses-feature-element)
