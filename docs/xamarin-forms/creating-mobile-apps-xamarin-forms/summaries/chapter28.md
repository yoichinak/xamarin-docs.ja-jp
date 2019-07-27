---
title: 第 28 章の概要です。 場所とマップ
description: Xamarin を使用した Mobile Apps の作成:第 28 章の概要です。 場所とマップ
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 846b7fa3c905b208771a110a013283bd77214b72
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511697"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>第 28 章の概要です。 場所とマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

Xamarin.Forms のサポート、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)から派生した要素`View`します。 マップの使用に関連する特殊なプラットフォームの要件のために別のアセンブリに実装されます**Xamarin.Forms.Maps**を別の名前空間が含まれる:`Xamarin.Forms.Maps`します。

## <a name="the-geographic-coordinate-system"></a>地理の座標系

地理の座標系は、地球のような球 (またはほぼ球) オブジェクトの位置を識別します。 座標は、両方の*緯度*と*経度*角度で表されます。

大圏と呼ばれる、`equator`地球の軸が概念的に拡張し、2 つの極地の間の中間にあります。

### <a name="parallels-and-latitude"></a>Parallels と緯度

測定した角度北または南記号の地球の中心から赤道行と呼ばれる等しい緯度の*parallels*します。 これら範囲は 0 度は、赤道から南北、北米にある 90 度です。 規則により、緯度、赤道より北は正の値と、赤道南は負の値。

### <a name="longitude-and-meridians"></a>経度と経線

南極北極から優れた円の上半分と等しい経度の行とも呼ばれます*経線*します。 これらで、英国のグリニッジ子午線を基準としました。 慣例により、子午線の経度は、180 °、0 度から正の値、本初子午線経度負の値を 0 度&ndash;180 度。

### <a name="the-equirectangular-projection"></a>正距円筒図法

地球の任意のフラット マップには、ゆがみが導入されています。 緯度と経度のすべての行では、直線、および、緯度と経度の角度と等しい違いは、マップ上の同じ距離に対応して場合、結果は場合、*正距円筒図法*します。 水平方向に広がりますが、ために、このマップは南北極に近い場所に領域を変形します。

### <a name="the-mercator-projection"></a>メルカトル投影法

広く普及している*メルカトル投影法*もこれらの領域を垂直方向に拡大して水平方向の伸縮を補正しようとしています。 これにより、マップ、極付近の領域が実際が、実際の領域を持つ非常に密接に準拠している任意のローカル領域よりもずっと大きく表示されます。

### <a name="map-services-and-tiles"></a>マップ サービスとタイル

マップ サービスを使用して、メルカトル投影法と呼ばれるのバリエーション`Web Mercator`します。 マップ サービスは、場所、およびズーム レベルに基づいてクライアントにビットマップのタイルを提供します。

## <a name="getting-the-users-location"></a>ユーザーの場所の取得

Xamarin.Forms`Map`クラスには、ユーザーの地理的な場所を取得する機能が含まれていませんが、これは多くの場合、望ましい場合に、マップのための依存関係サービス操作とそれが処理する必要があります。

> [!NOTE]
> Xamarin.Forms アプリケーションは代わりに使用できます、 [ `Geolocation` ](~/essentials/geolocation.md) Xamarin.Essentials に含まれるクラス。

### <a name="the-location-tracker-api"></a>API の場所のトラッカー

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューションには、API の場所のトラッカーのコードが含まれています。 [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs)構造体には、緯度と経度がカプセル化します。 [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs)インターフェイスを起動し、場所のトラッカーと新しい場所が使用可能なときにイベントを一時停止の 2 つのメソッドを定義します。

#### <a name="the-ios-location-manager"></a>IOS のロケーション マネージャー

IOS のカスタム実装の`ILocationTracker`は、 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs)により、クラスを使用して、iOS の[ `CLLocationManager`](xref:CoreLocation.CLLocationManager)します。

#### <a name="the-android-location-manager"></a>Android のロケーション マネージャー

Android の実装の`ILocationTracker`は、 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) 、Android の使用により、クラス[ `LocationManager` ](xref:Android.Locations.LocationManager)クラス。

#### <a name="the-uwp-geo-locator"></a>UWP の geo ロケーター

ユニバーサル Windows プラットフォーム実装`ILocationTracker`は、 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) 、UWP は、クラス[ `Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator)します。

### <a name="display-the-phones-location"></a>スマート フォンの場所を表示します。

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI)サンプルでは、場所の追跡ツールを使用して、テキストと正距円筒図法マップの両方に、スマート フォンの場所を表示します。

### <a name="the-required-overhead"></a>オーバーヘッドが必要

いくつかのオーバーヘッドが必要です。 **WhereAmI**場所の追跡ツールを使用します。 最初に、すべてのプロジェクトで、 **WhereAmI**ソリューション内の対応するプロジェクトへの参照をいる必要があります**Xamarin.FormsBook.Platform**、および各**WhereAmI**プロジェクト呼び出す必要があります、`Toolkit.Init`メソッド。

場所のアクセス許可のフォームにいくつか追加のプラットフォーム固有オーバーヘッドが必要です。

#### <a name="location-permission-for-ios"></a>IOS 用の場所のアクセス許可

Ios の場合、 **info.plist**ファイルは、そのユーザーの場所の取得を許可するユーザーの質問のテキストを含む項目を含める必要があります。

#### <a name="location-permissions-for-android"></a>Android 用の場所のアクセス許可

ユーザーの場所を取得する android アプリケーションは、ACCESS_FILE_LOCATION のアクセス許可を AndroidManifest.xml ファイルが必要です。

#### <a name="location-permissions-for-the-uwp"></a>UWP の場所のアクセス許可

ユニバーサル Windows プラットフォーム アプリケーションする必要がありますが、 `location` Package.appxmanifest ファイルでマークされているデバイスの機能です。

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps の操作

使用していくつかの要件が関係する、`Map`クラス。

### <a name="the-nuget-package"></a>NuGet パッケージ

**Xamarin.Forms.Maps** NuGet ライブラリは、アプリケーションのソリューションに追加する必要があります。 バージョン番号が同じにする必要があります、 **Xamarin.Forms**パッケージが現在インストールされています。

### <a name="initializing-the-maps-package"></a>マップのパッケージを初期化しています

アプリケーション プロジェクトを呼び出す必要があります、`Xamarin.FormsMaps.Init`メソッドへの呼び出しを行った後`Xamarin.Forms.Forms.Init`します。

### <a name="enabling-map-services"></a>マップ サービスを有効にします。

`Map`ユーザーの場所を取得できる、アプリケーションは、この章で既に説明した方法でユーザーのアクセス許可を取得する必要があります。

#### <a name="enabling-ios-maps"></a>マップの iOS の有効化

使用して iOS アプリケーション`Map`info.plist ファイルの 2 行必要があります。

#### <a name="enabling-android-maps"></a>マップの Android を有効にします。

承認キーは、Google マップ サービスを使用する必要があります。 このキーが挿入、 **AndroidManifest.xml**ファイル。 さらに、 **AndroidManifest.xml**ファイルが必要な`manifest`ユーザーの場所の取得中に関連するタグ。

#### <a name="enabling-uwp-maps"></a>マップ UWP を有効にします。

ユニバーサル Windows プラットフォーム アプリケーションには、Bing Maps を使用するための承認キーが必要です。 このキーが引数として渡される、`Xamarin.FormsMaps.Init`メソッド。 アプリケーションは、位置情報サービスにも有効にする必要があります。

### <a name="the-unadorned-map"></a>非マップ

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos)サンプルから成る、 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml)ファイルと[MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs)分離コード ファイルさまざまなデモンストレーション プログラムに移動できます。

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml)ファイルを表示する方法を示しています、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)ビュー。 既定では、ローマ都市が表示されますが、ユーザーが、マップを操作できます。

水平方向と垂直方向のスクロールを無効にするには設定、 [ `HasScrollEnabled` ](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)プロパティを`false`します。 ズームを無効にするには設定[ `HasZoomEnabled` ](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)に`false`します。 これらのプロパティは、すべてのプラットフォームで動作しない可能性があります。

### <a name="streets-and-terrain"></a>通りや地形

設定して、さまざまな種類のマップを表示することができます、`Map`プロパティ[ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType)型の[ `MapType` ](xref:Xamarin.Forms.Maps.MapType)、3 つのメンバーを列挙します。

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street)、既定値
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml)ファイルは、マップの種類を選択するラジオ ボタンを使用する方法を示します。 これを利用、 [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリとクラスに基づいて、 [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml)ファイル。

### <a name="map-coordinates"></a>マップ座標

プログラムを取得できます、現在の領域を`Map`を通じて表示されて、 [ `VisibleRegion` ](xref:Xamarin.Forms.Maps.Map.VisibleRegion)プロパティ。 このプロパティは*いない*バインド可能なプロパティでサポートされるため、目的のために、タイマーを使ってくださいプロパティを監視することを希望するプログラムが変更されたときを示す通知メカニズムはありません。

`VisibleRegion` 種類は[ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan)、4 つの読み取り専用プロパティを持つクラス。

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) 型の [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) 型の`double`マップの表示領域の高さを示す
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) 型の`double`マップの表示領域の幅を示す
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) 型の[ `Distance`](xref:Xamarin.Forms.Maps.Distance)マップに表示される円形領域の最大のサイズを示す

`Position` `Distance`は両方の構造体。 `Position` 設定を使用して、2 つの読み取り専用プロパティを定義、 [ `Position`コンス トラクター](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)):

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` メトリックとヤード ポンド単位間で変換することで、単位に依存しない距離を提供するものです。 A`Distance`値は、いくつかの方法で作成できます。

- [`Distance` コンストラクター](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double))メートル単位の距離
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) 静的メソッド
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) 静的メソッド
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) 静的メソッド

値は、3 つのプロパティから入手できます。

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) 型の `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) 型の `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) 型の `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml)ファイルでは、いくつか含まれています`Label`要素を表示するため、`MapSpan`情報。 [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs)分離コード ファイルでは、タイマーを使用して、ユーザーは、マップを操作するため、更新情報を保持します。

### <a name="position-extensions"></a>位置の拡張機能

この本をという名前の新しいライブラリ[ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps)マップ固有ではプラットフォームに依存しない型が含まれます。 [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)クラスには、`ToString`メソッドの`Position`、2 つの間の距離を計算するメソッドをおよび`Position`値。

### <a name="setting-an-initial-location"></a>初期の場所の設定

呼び出すことができます、 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan))メソッドの`Map`をプログラムで、マップ上の場所とズーム レベルを設定します。 型の引数は、`MapSpan`します。 作成することができます、`MapSpan`オブジェクトを使用して、次のいずれか。

- [`MapSpan` コンストラクター](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double))で、`Position`と緯度と経度のスパン
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) `Position`と半径

新たに作成することも`MapSpan`メソッドを使用して、既存のものから[ `ClampLatitude` ](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double))または[ `WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double))します。

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml)ファイルと[WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs)分離コード ファイルが使用する方法を示します、`MoveToRegion`ワイオミングの状態を表示するメソッド。

また使用することができます、 [ `Map`コンストラクター](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan))で、`MapSpan`マップの場所を初期化するオブジェクト。 [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml)ファイルが完全にサンフランシスコの Xamarin の本社の表示を XAML でこれを実行する方法を示します。

### <a name="dynamic-zooming"></a>動的ズーム

使用することができます、`Slider`を動的にマップを拡大します。 [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml)ファイルと[RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs)分離コード ファイルに基づくマップの半径を変更する方法を紹介する、`Slider`値。

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml)ファイルと[LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs)分離コード ファイルは、android より適切に動作する別の方法を説明しますが、どちらのアプローチが、Windows で適切に機能プラットフォーム。

### <a name="the-phones-location"></a>スマート フォンの場所

[ `IsShowingUser` ](xref:Xamarin.Forms.Maps.Map.IsShowingUser)プロパティの`Map`動作としてのプラットフォームごとに異なる方法で少し、 [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml)ファイルを示します。

- Ios では、青い点が、電話の場所を示しますが、ある手動で移動する必要があります。
- Android では、アイコンが表示されるときにプッシュ移動、電話の場所へのマップ
- UWP は、iOS に似ていますが、自動的に場合がありますが、場所に移動します。

**MapDemos**プロジェクトが最初に基づくアイコン ベースのボタンを定義することで、Android のアプローチを模倣するために試行、 [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml)ファイルと[MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs)分離コード ファイル。

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml)ファイルと[GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs)分離コード ファイルがこのボタンを使用する電話の場所に移動します。

### <a name="pins-and-science-museums"></a>Pin と科学美術館

最後に、`Map`クラスを定義、 [ `Pins` ](xref:Xamarin.Forms.Maps.Map.Pins)型のプロパティ`IList<Pin>`します。 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin)クラスは、4 つのプロパティを定義します。

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) 型の`string`、必須プロパティ
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) 型の`string`人間が判読できる省略可能なアドレス
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) 型の`Position`pin がマップに表示される場所を示す
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) 型の[ `PinType` ](xref:Xamarin.Forms.Maps.PinType)、列挙体は、使用されていません

**MapDemos**プロジェクトには、ファイルが含まれています[ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml)、米国での科学美術館を一覧表示すると[ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs)と[。`Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs)クラスのこのデータを逆シリアル化します。

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml)ファイルと[ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs)マップ内のこれらの科学美術館の分離コード ファイルの表示 pin。 ユーザーが pin をタップしたときに、アドレスと博物館の web サイトを表示します。

### <a name="the-distance-between-two-points"></a>2 つのポイント間の距離

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)クラスが含まれています、 [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88)メソッドが 2 つの地理的場所間の距離の簡略化された計算を使用します。

これで使用、 [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml)ファイルと[LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs)分離コード ファイルも、博物館にユーザーの場所からの距離を表示します。

[![ローカルの美術館ページのスクリーン ショットをトリプル](images/ch28fg28-small.png "場所までの距離")](images/ch28fg28-large.png#lightbox "場所までの距離")

プログラムでは、動的にマップの場所に基づいてピンの数を制限する方法も示します。

## <a name="geocoding-and-back-again"></a>ジオコーディングとの間

[ **Xamarin.Forms.Maps** ](xref:Xamarin.Forms.Maps)アセンブリも含まれています、 [ `Geocoder` ](xref:Xamarin.Forms.Maps.Geocoder)クラス、 [ `GetPositionsForAddressAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String))に変換するメソッド0 またはより多くの地理的位置や別のメソッドにテキスト アドレス[ `GetAddressesForPositionAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position))他の方向に変換します。

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml)ファイルと[GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs)分離コード ファイルは、この機能を示します。



## <a name="related-links"></a>関連リンク

- [第 28 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [第 28 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.Forms のマップ](~/xamarin-forms/user-interface/map.md)
