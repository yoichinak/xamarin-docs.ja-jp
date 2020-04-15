---
title: '第 28 章の概要: 位置情報と地図'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 28 章の概要: 位置情報と地図'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5dcd84536cc6d80deb753fc6fe57f9090f6b2dad
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "72697072"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>第 28 章の概要: 位置情報と地図

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> このページのメモでは、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

Xamarin.Forms では、`View` から派生した [`Map`](xref:Xamarin.Forms.Maps.Map) 要素がサポートされています。 地図の使用に関する特別なプラットフォーム要件のため、これらは別のアセンブリ (**Xamarin.Forms.Maps**) に実装され、別の名前空間 (`Xamarin.Forms.Maps`) が使用されています。

## <a name="the-geographic-coordinate-system"></a>地理座標系

地理座標系では、地球のような球体 (またはほぼ球体) の物体上の位置が特定されます。 座標は、角度で表された "*緯度*" と "*経度*" の両方によって構成されます。

`equator` と呼ばれる大きな円は、地球の軸が概念的に伸びていく 2 つの極の中間にあります。

### <a name="parallels-and-latitude"></a>緯線と緯度

地球の中心点から赤道の北または南に向かってある角度を測ると、"*緯線*" と呼ばれる等しい緯度の線ができます。 これらの範囲は、赤道の 0 度から、北極と南極の 90 度までです。 慣例により、赤道より北の緯度は正の値、赤道より南は負の値になります。

### <a name="longitude-and-meridians"></a>経度と経線

北極から南極までを結ぶ大きな半円は、等しい経度の線です。"*経線*" とも呼ばれます。 これらは、英国のグリニッジ子午線を基準としています。 慣例により、グリニッジ子午線より東の経度は 0 度から 180 度までの正の値、グリニッジ子午線より西の経度は 0 度から &ndash;180 度までの負の値になります。

### <a name="the-equirectangular-projection"></a>正距円筒図法

地球を平らな地図にすると、必ずゆがみが発生します。 緯度と経度の線がすべて直線であり、緯度および経度の角度差が同じなら対応する地図上の距離も同じになる場合、その結果は "*正距円筒図法*" になります。 この地図では、極に近い領域でゆがみが発生します。水平方向に拡張されるためです。

### <a name="the-mercator-projection"></a>メルカトル図法

よく使われる "*メルカトル図法*" では、これらの領域を垂直方向にも拡張することによって、水平方向の拡張を補おうとします。 これにより、極に近い領域が実際よりもかなり大きく見える地図が生成されますが、局所的な領域は実際の領域と極めて正確に一致します。

### <a name="map-services-and-tiles"></a>地図サービスとタイル

地図サービスでは、`Web Mercator` と呼ばれるメルカトル図法のバリエーションの 1 つが使用されます。 地図サービスは、位置とズーム レベルに基づいて、クライアントにビットマップ タイルを提供します。

## <a name="getting-the-users-location"></a>ユーザーの位置情報を取得する

Xamarin.Forms の `Map` クラスには、ユーザーの地理的な位置情報を取得する機能は含まれていません。しかし、これは地図を操作するときによく必要になるので、依存関係サービスによって処理される必要があります。

> [!NOTE]
> Xamarin.Forms アプリケーションでは、代わりに、Xamarin.Essentials に含まれる [`Geolocation`](~/essentials/geolocation.md) クラスを使用できます。

### <a name="the-location-tracker-api"></a>位置情報トラッカー API

[**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) ソリューションには、位置情報トラッカー API のコードが含まれています。 [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) 構造体により、緯度と経度がカプセル化されます。 [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) インターフェイスでは、位置情報トラッカーを開始および一時停止する 2 つのメソッドと、新しい位置情報が利用可能になったときのイベントが定義されています。

#### <a name="the-ios-location-manager"></a>iOS 位置情報マネージャー

`ILocationTracker` の iOS での実装は、iOS の [`CLLocationManager`](xref:CoreLocation.CLLocationManager) を使用する [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) クラスです。

#### <a name="the-android-location-manager"></a>Android 位置情報マネージャー

`ILocationTracker` の Android での実装は、Android の [`LocationManager`](xref:Android.Locations.LocationManager) クラスを使用する [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) クラスです。

#### <a name="the-uwp-geo-locator"></a>UWP geo ロケーター

`ILocationTracker` のユニバーサル Windows プラットフォームでの実装は、UWP [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator) を使用する [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) クラスです。

### <a name="display-the-phones-location"></a>スマートフォンの位置情報を表示する

[**WhereAmI**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) サンプルでは、位置情報トラッカーを使用して、スマートフォンの位置情報が、テキストと正距円筒図法の地図の両方に表示されます。

### <a name="the-required-overhead"></a>必要なオーバーヘッド

**WhereAmI** で位置情報トラッカーを使用するためには、いくらかのオーバーヘッドが必要です。 まず、**WhereAmI** ソリューション内のすべてのプロジェクトが、**Xamarin.FormsBook.Platform** 内の対応するプロジェクトに対する参照を持っている必要があります。また、各 **WhereAmI** プロジェクトで `Toolkit.Init` メソッドを呼び出す必要があります。

位置情報のアクセス許可という形で、さらにいくらかのプラットフォーム固有のオーバーヘッドが必要です。

#### <a name="location-permission-for-ios"></a>iOS の位置情報のアクセス許可

iOS の場合、**info.plist** ファイルには、ユーザーにそのユーザーの位置情報の取得を許可するように求める、質問のテキストを含む項目が含まれている必要があります。

#### <a name="location-permissions-for-android"></a>Android 位置情報のアクセス許可

ユーザーの位置情報を取得する Android アプリケーションは、AndroidManifest.xml ファイルの ACCESS_FILE_LOCATION アクセス許可を持っている必要があります。

#### <a name="location-permissions-for-the-uwp"></a>UWP 位置情報のアクセス許可

ユニバーサル Windows プラットフォーム アプリケーションの場合は、Package.appxmanifest ファイルで `location` デバイス機能がマークされている必要があります。

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps の操作

`Map` クラスの使用には、いくつかの要件が関係しています。

### <a name="the-nuget-package"></a>NuGet パッケージ

**Xamarin.Forms.Maps** NuGet ライブラリをアプリケーション ソリューションに追加する必要があります。 バージョン番号は、現在インストールされている **Xamarin.Forms** パッケージと同じである必要があります。

### <a name="initializing-the-maps-package"></a>Maps パッケージの初期化

アプリケーション プロジェクトでは、`Xamarin.Forms.Forms.Init` の呼び出しを行った後、`Xamarin.FormsMaps.Init` メソッドを呼び出す必要があります。

### <a name="enabling-map-services"></a>地図サービスの有効化

`Map` ではユーザーの位置情報を取得できるため、アプリケーションでは、この章で前に説明した方法で、ユーザーのアクセス許可を取得する必要があります。

#### <a name="enabling-ios-maps"></a>iOS の地図の有効化

`Map` を使用する iOS アプリケーションでは、info.plist ファイルに 2 行が必要です。

#### <a name="enabling-android-maps"></a>Android の地図の有効化

Google Map サービスを使用するには、承認キーが必要です。 このキーは、**AndroidManifest.xml** ファイルに挿入されます。 さらに、**AndroidManifest.xml** ファイルには、ユーザーの位置情報の取得に関連する `manifest` タグが必要です。

#### <a name="enabling-uwp-maps"></a>UWP の地図の有効化

ユニバーサル Windows プラットフォーム アプリケーションでは、Bing Maps を使用するための承認キーが必要です。 このキーは、引数として `Xamarin.FormsMaps.Init` メソッドに渡されます。 また、位置情報サービスに対してもアプリケーションを有効にする必要があります。

### <a name="the-unadorned-map"></a>簡素なマップ

[**MapDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) サンプルは、[MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) と、さまざまなデモンストレーション プログラムへの移動を可能にする [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) 分離コード ファイルで構成されています。

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) ファイルでは、[`Map`](xref:Xamarin.Forms.Maps.Map) ビューを表示する方法が示されています。 既定ではローマ市が表示されるようになっていますが、ユーザーが地図を操作することが可能です。

水平方向および垂直方向のスクロール機能を無効にするには、[`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) プロパティを `false` に設定します。 ズーム機能を無効にするには、[`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) を `false` に設定します。 これらのプロパティは、すべてのプラットフォームでは動作しない可能性があります。

### <a name="streets-and-terrain"></a>ストリートと地形

3 つのメンバーを含む列挙型 [`MapType`](xref:Xamarin.Forms.Maps.MapType) 型である、`Map` のプロパティ [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) を設定することで、さまざまな種類の地図を表示することができます。

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street) (既定値)
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) ファイルでは、ラジオ ボタンを使用してマップの種類を選択する方法が示されています。 ここでは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) クラスと、[MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) ファイルに基づくクラスが使用されています。

### <a name="map-coordinates"></a>地図の座標

プログラムでは、[`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) プロパティを通じて `Map` が表示している現在の領域を取得することができます。 このプロパティは、バインド可能なプロパティによってサポートされて "*いません*"。これが変更されたときに通知するメカニズムは存在しないため、このプロパティを監視する必要があるプログラムでは、おそらく、その目的のためにタイマーを使用する必要があります。

`VisibleRegion` は [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 型です。これは、4 つの読み取り専用プロパティを持つクラスです。

- [`Position`](xref:Xamarin.Forms.Maps.Position) 型の [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)
- `double` 型の [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)。地図の表示領域の高さを示します
- `double` 型の [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)。地図の表示領域の幅を示します
- [`Distance`](xref:Xamarin.Forms.Maps.Distance) 型の [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)。地図上に表示される最大の円形領域のサイズを示します

`Position` と `Distance` は両方とも構造体です。 `Position` では、[`Position` コンストラクター](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double))によって設定される 2 つの読み取り専用プロパティが定義されます。

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` の目的は、メートル法とヤード ポンド法の間の変換を行うことで、単位に依存しない距離を提供することです。 `Distance` 値は、いくつかの方法で作成できます。

- [`Distance` コンストラクター](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) (メートル法の距離を指定)
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) 静的メソッド
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) 静的メソッド
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) 静的メソッド

その値は、次の 3 つのプロパティから利用できます。

- `double` 型の [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)
- `double` 型の [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)
- `double` 型の [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) ファイルには、`MapSpan` 情報を表示するための `Label` 要素がいくつか含まれています。 [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) 分離コード ファイルでは、ユーザーが地図を操作したときに情報を最新の状態に保つために、タイマーが使用されています。

### <a name="position-extensions"></a>Position の拡張機能

[**Xamarin.FormsBook.Toolkit.Maps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) という名前の、この書籍用の新しいライブラリには、地図に固有で、プラットフォームに依存しない型が含まれています。 [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) クラスには、`Position` 用の `ToString` メソッドと、2 つの `Position` 値の間の距離を計算するメソッドが含まれています。

### <a name="setting-an-initial-location"></a>初期位置情報を設定する

`Map` の [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) メソッドを呼び出して、地図上の位置情報とズーム レベルをプログラムで設定することができます。 この引数は `MapSpan` 型です。 `MapSpan` オブジェクトは、次のいずれかを使用して作成できます。

- [`MapSpan` コンストラクター](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) (`Position`、緯度と経度のスパンを指定)
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) (`Position` と半径を指定)

メソッド [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) または [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)) を使用して、既存のものから新しい `MapSpan` を作成することもできます。

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) ファイルと [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) 分離コード ファイルでは、`MoveToRegion` メソッドを使用してワイオミング州を表示する方法が示されています。

または、`MapSpan` オブジェクトを指定する [`Map` コンストラクター](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan))を使用して、地図の位置情報を初期化することもできます。 [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) ファイルでは、これを完全に XAML で実行して、サンフランシスコにある Xamarin 本社を表示する方法が示されています。

### <a name="dynamic-zooming"></a>動的ズーム

`Slider` を使用すると、地図を動的にズームできます。 [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) ファイルと [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) 分離コード ファイルでは、`Slider` 値に基づいて地図の半径を変更する方法が示されています。

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) ファイルと [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) 分離コード ファイルでは、Android 上でよりうまく機能する別の方法が示されています。ただし、どちらの方法も Windows プラットフォーム上では適切に機能しません。

### <a name="the-phones-location"></a>スマートフォンの位置情報

[ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) ファイルで示されているように、`Map` の [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) プロパティは、各プラットフォーム上で動作が少し異なります。

- iOS では、青い点がスマートフォンの位置情報を示しますが、そこまで手動で移動する必要があります
- Android では、押すと地図をスマートフォンの位置情報に移動させるアイコンが表示されます
- UWP は iOS に似ていますが、その位置情報に自動的に移動する場合があります

**MapDemos** プロジェクトでは、Android のアプローチを模倣しようとしています。そのために、[MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) ファイルと [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) 分離コード ファイルに基づいて、最初にアイコン ベースのボタンを定義します。

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) ファイルと [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) 分離コード ファイルでは、このボタンを使用してスマートフォンの位置情報に移動します。

### <a name="pins-and-science-museums"></a>Pins と科学博物館

最後に、`Map` クラスでは `IList<Pin>` 型の [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) プロパティが定義されます。 [`Pin`](xref:Xamarin.Forms.Maps.Pin) クラスでは、4 つのプロパティが定義されます。

- `string` 型の [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) (必須プロパティです)
- `string` 型の [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) (省略可能な人間が判読できる住所です)
- `Position` 型の [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) (地図上にピンが表示される場所を示します)
- [`PinType`](xref:Xamarin.Forms.Maps.PinType) 型 (列挙型) の [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) (使用されません)

**MapDemos** プロジェクトには、米国にある科学博物館を一覧表示した [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml) ファイルと、このデータを逆シリアル化するための [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) クラスと [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) クラスが含まれています。

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) ファイルと [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) 分離コード ファイルでは、地図内のこれらの科学博物館に対してピンが表示されます。 ユーザーがピンをタップすると、その博物館の住所と Web サイトが表示されます。

### <a name="the-distance-between-two-points"></a>2 点間の距離

[`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) クラスには、2 つの地理的な場所の距離を簡単に計算する [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) メソッドが含まれています。

これは、[LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) ファイルと [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) 分離コード ファイルで、ユーザーの位置情報から博物館までの距離も表示するために使用されています。

[![地域の博物館ページのトリプル スクリーンショット](images/ch28fg28-small.png "位置情報までの距離")](images/ch28fg28-large.png#lightbox "位置情報までの距離")

また、このプログラムでは、地図の位置情報に基づいてピンの数を動的に制限する方法も示されています。

## <a name="geocoding-and-back-again"></a>ジオコーディングと逆ジオコーディング

[**Xamarin.Forms.Maps**](xref:Xamarin.Forms.Maps) アセンブリには、[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) クラスも含まれています。これには、テキストの住所を可能な 0 個以上の地理的位置に変換する [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) メソッドと、逆方向に変換する別の [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) メソッドが含まれています。

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) ファイルと [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) 分離コード ファイルでは、この機能が示されています。

## <a name="related-links"></a>関連リンク

- [第 28 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [第 28 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.Forms Map](~/xamarin-forms/user-interface/map/index.md)
