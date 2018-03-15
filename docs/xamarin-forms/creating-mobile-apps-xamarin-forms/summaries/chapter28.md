---
title: "28 章の概要です。 場所とマップ"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d7a75ce0303030d53315b5e698fc604a910c969c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>28 章の概要です。 場所とマップ

Xamarin.Forms をサポートしている、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)から派生した要素`View`です。 マップの使用に関係する特別なプラットフォームの要件のため、実装、独立したアセンブリで**Xamarin.Forms.Maps**、別の名前空間に含まれる:`Xamarin.Forms.Maps`です。

## <a name="the-geographic-coordinate-system"></a>地理座標系

地理座標系では、地球のような球面 (またはほぼ球) オブジェクトの位置を識別します。 座標は、両方の*緯度*と*経度*角度で表されます。

優れた円と呼ばれる、`equator`地球の軸が概念的に拡張し、2 つの極地の間の中間に位置します。

### <a name="parallels-and-latitude"></a>Parallels と緯度

測定した角度北または南記号の地球の中心から赤道行と呼ばれる等しい緯度の*parallels*です。 これらの範囲 0 ° 赤道付近で、北と南南北極に 90 度です。 慣例により、赤道の北緯度正値であり、赤道の南は負の値。

### <a name="longitude-and-meridians"></a>経度と経線

通り北極北極から優れた円の半分と等しい経度、線とも呼ばれます*経線*です。 これらはイギリスのグリニッジをで子午線に対して相対的です。 慣例により、子午線の経度は、0 ° 180 °、正の値、経度子午線から負の値を 0 ° &ndash;180 度。

### <a name="the-equirectangular-projection"></a>距円筒図法

地球の任意のフラット マップにゆがみが導入されています。 緯度と経度のすべての行であり緯度と経度の角度に等しい違いは、マップに等距離に対応して、場合、結果は場合、*距円筒図法*です。 このマップ水平方向に拡張されていてため南北極に近い場所に領域を変形します。

### <a name="the-mercator-projection"></a>Mercator 投影法

人気の高い*メルカトル図法*もこれらの領域を垂直方向に拡大して水平方向の伸縮を補正しようとしています。 これにより、マップ、極地の近くの領域が実際が、実際の領域を持つ任意のローカル領域が非常に密接に準拠してよりはるかに大きく表示されます。

### <a name="map-services-and-tiles"></a>マップのサービスとタイル

マップのサービスと呼ばれる、メルカトル図法のバリエーションを使用して`Web Mercator`です。 マップのサービスは、場所とズーム レベルに基づいてクライアントにビットマップのタイルを提供します。

## <a name="getting-the-users-location"></a>ユーザーの場所の取得

Xamarin.Forms`Map`クラスに、ユーザーの地理的な場所を取得する機能が含まれていませんが、これは多くの場合、望ましい場合に、マップのための依存関係サービス操作とそれが処理する必要があります。

### <a name="the-location-tracker-api"></a>API の場所のトラッカー

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューションには、API の場所のトラッカーのコードが含まれています。 [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs)構造体は、緯度と経度をカプセル化します。 [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs)インターフェイスを起動し、場所のトラッカーとは、新しい場所が使用可能なときにイベントを一時停止に 2 つのメソッドを定義します。

#### <a name="the-ios-location-manager"></a>IOS location manager

IOS 実装`ILocationTracker`は、 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) iOS の使用できるクラスを使用して[ `CLLocationManager`](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/)です。

#### <a name="the-android-location-manager"></a>Android location manager

Android 実装`ILocationTracker`は、 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs)が Android を使用するクラス[ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)クラスです。

#### <a name="the-windows-runtime-geo-locator"></a>Windows ランタイム geo ロケーター

Windows ランタイムの実装`ILocationTracker`は、 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs)クラスを使用している UWP [ `Geolocator`](https://msdn.microsoft.com/library/windows/apps/br225534)です。

### <a name="display-the-phones-location"></a>スマート フォンの場所を表示します。

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI)サンプルでは、場所の追跡ツールを使用して、テキストと図法マップ上の両方に電話の場所を表示します。

### <a name="the-required-overhead"></a>オーバーヘッドが必要

一部のオーバーヘッドが必要**WhereAmI**場所追跡ツールを使用します。 最初に、すべてのプロジェクトで、 **WhereAmI**ソリューション内の対応するプロジェクトへの参照を持つ必要があります**Xamarin.FormsBook.Platform**、および各**WhereAmI**プロジェクト呼び出す必要があります、`Toolkit.Init`メソッドです。

場所のアクセス許可の形式で、追加のプラットフォームに固有のオーバーヘッドが必要です。

#### <a name="location-permission-for-ios"></a>IOS 用の場所のアクセス許可

Ios の場合、 **info.plist**ファイルは、そのユーザーの場所の取得を許可するように求める質問のテキストが含まれる項目を含める必要があります。

#### <a name="location-permissions-for-android"></a>Android 用の場所のアクセス許可

ユーザーの場所を取得する android アプリケーションは、AndroidManifest.xml ファイル ACCESS_FILE_LOCATION 権限が必要です。

#### <a name="location-permissions-for-the-windows-runtime"></a>Windows ランタイムの場所のアクセス許可

Windows または Windows Phone のアプリケーションが必要な`location`Package.appxmanifest ファイルにデバイスの機能が設定されています。

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps の操作

使用して関連するいくつかの要件、`Map`クラスです。

### <a name="the-nuget-package"></a>NuGet パッケージ

**Xamarin.Forms.Maps** NuGet ライブラリは、アプリケーションのソリューションに追加する必要があります。 バージョン番号が同じにする必要があります、 **Xamarin.Forms**現在インストールされているパッケージ。

### <a name="initializing-the-maps-package"></a>マップのパッケージの初期化

アプリケーション プロジェクトを呼び出す必要があります、`Xamarin.FormsMaps.Init`メソッドへの呼び出しを行った後`Xamarin.Forms.Forms.Init`です。

### <a name="enabling-map-services"></a>マップのサービスを有効にします。

`Map`できるのユーザーの所在地、アプリケーションは、この章で説明した方法でそのユーザーのアクセス許可を取得する必要があります。

#### <a name="enabling-ios-maps"></a>マップ iOS を有効にします。

IOS アプリケーションを使用して、 `Map` info.plist ファイル内の 2 つの行を必要があります。

#### <a name="enabling-android-maps"></a>マップ Android を有効にします。

承認キーは、Google マップのサービスを使用する必要があります。 このキーが挿入、 **AndroidManifest.xml**ファイル。 さらに、 **AndroidManifest.xml**ファイルが必要です`manifest`タグ、ユーザーの場所を取得するに必要です。

#### <a name="enabling-windows-runtime-maps"></a>マップの Windows ランタイムを有効にします。

Windows ランタイム アプリケーションでは、Bing Maps の使用の認証キーが必要です。 このキーが引数として渡される、`Xamarin.FormsMaps.Init`メソッドです。 アプリケーションは、ロケーション サービスのも有効にする必要があります。

### <a name="the-unadorned-map"></a>非装飾のマップ

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos)サンプルを構成、 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml)ファイルと[MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs)分離コード ファイルデモのさまざまなプログラムへの移動を使用できます。

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml)ファイルを表示する方法を示しています、 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)ビュー。 既定では、ローマ市区町村が表示されますが、ユーザーが、マップを操作できます。

水平および垂直方向のスクロールを無効にするには設定、 [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/)プロパティを`false`です。 ズームを無効にするには設定[ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/)に`false`です。 すべてのプラットフォームでこれらのプロパティは使用できません。

### <a name="streets-and-terrain"></a>通りや地形

設定して、さまざまな種類のマップを表示することができます、`Map`プロパティ[ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/)型の[ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/)、3 つのメンバーを持つ列挙します。

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/)、既定値
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml)ファイル、ラジオ ボタンを使用して、マップの種類を選択する方法を示しています。 これでは、使用、 [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリとクラスに基づいて、 [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml)ファイル。

### <a name="map-coordinates"></a>マップ座標

プログラムを取得できます、現在の領域を`Map`を表示する、 [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/)プロパティです。 このプロパティは、*いない*支えられて、バインド可能なプロパティが変更されたときを示すため、プロパティを監視することを希望するプログラムを目的とするタイマーを使ってくださいに通知メカニズムがないとします。

`VisibleRegion` 型は[ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/)、4 つの読み取り専用プロパティを持つクラス。

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) 型の [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) 型の`double`マップの表示領域の高さを示す
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) 型の`double`マップの表示領域の幅を示す
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) 型の[ `Distance`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)マップに表示されている最大の円形領域のサイズを示す

`Position` および`Distance`は両方の構造体。 `Position` 設定を使用して、2 つの読み取り専用プロパティを定義、 [ `Position`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` メトリック、および英語のユニット間で変換することで単位に依存しない距離を記載します。 A`Distance`値がいくつかの方法で作成することができます。

- [`Distance` コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/)メートル単位で距離で
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) 静的メソッド
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) 静的メソッド
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) 静的メソッド

値は、3 つのプロパティから入手できます。

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) 型の `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) 型の `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) 型の `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml)ファイルでは、いくつか含まれています`Label`要素を表示するため、`MapSpan`情報。 [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs)分離コード ファイルでは、タイマーを使用して、ユーザーが、マップを操作すると更新の情報を保持します。

### <a name="position-extensions"></a>位置の拡張機能

この書籍という名前の新しいライブラリ[ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps)マップ固有ではプラットフォームに依存しない型が含まれています。 [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)クラスには、`ToString`メソッドを`Position`、2 つの間の距離を計算する方法と`Position`値。

### <a name="setting-an-initial-location"></a>初期の場所の設定

呼び出すことができます、 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/)メソッドの`Map`プログラムで、マップ上の場所とズーム レベルを設定します。 引数の型が`MapSpan`です。 作成することができます、`MapSpan`オブジェクトを使用して、次のいずれか。

- [`MapSpan` コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/)で、 `Position`、および緯度と経度の範囲
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) `Position`および radius

新規作成することも`MapSpan`メソッドを使用して既存のものから[ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/)または[ `WithZoom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/)です。

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml)ファイルと[WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs)分離コード ファイルを使用する方法を示しています、`MoveToRegion`ワイオミングの状態を表示するメソッド。

代わりに使用することができます、 [ `Map`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/)で、`MapSpan`マップの場所を初期化するオブジェクト。 [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml)ファイル サンフランシスコに Xamarin の本社を表示する XAML でこれを行う方法を示しています。

### <a name="dynamic-zooming"></a>動的なズーム

使用することができます、`Slider`マップを動的に拡大します。 [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml)ファイルと[RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs)分離コード ファイルに基づくマップの半径を変更する方法を表示する、`Slider`値。

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml)ファイルと[LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs)分離コード ファイルが android でより適切に動作する他の方法を示しますが、どちらアプローチは、Windows でうまく動作プラットフォームです。

### <a name="the-phones-location"></a>スマート フォンの場所

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/)プロパティ`Map`動作は少し異なるとして 3 つのプラットフォームで、 [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml)ファイルを示します。

- Ios の場合は、青いドットは、スマート フォンの場所を示しますが手動で移動する必要があります。
- Android でアイコンが表示される時にプッシュ移動スマート フォンの場所へのマップ
- UWP は iOS に似ていますが、自動的にことがありますが、場所に移動します。

**MapDemos**プロジェクトが最初に基づくアイコン ベースのボタンを定義することで Android のアプローチを模倣するために試行、 [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml)ファイルと[MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs)分離コード ファイル。

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml)ファイルと[GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs)分離コード ファイルでは、このボタンを使用して電話の場所に移動します。

### <a name="pins-and-science-museums"></a>Pin とサイエンス美術館

最後に、`Map`クラスを定義、 [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/)型のプロパティ`IList<Pin>`です。 [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/)クラスは、4 つのプロパティを定義します。

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) 型の`string`、必須プロパティ
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) 型の`string`人間が判読できる省略可能なアドレス
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) 型の`Position`マップ上、暗証番号 (pin) を表示する場所を示す
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) 型の[ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/)、列挙体で使用されていません

**MapDemos**プロジェクトには、ファイルが含まれています[ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml)、米国の州、科学美術館を一覧表示すると[ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs)と[。`Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs)クラスのこのデータを逆シリアル化します。

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml)ファイルと[ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs)これらサイエンス美術館、マップ内の分離コード ファイルの表示 pin です。 ユーザーが pin をタップしたときに、アドレスと博物館の web サイトが表示されます。

### <a name="the-distance-between-two-points"></a>2 つのポイント間の距離

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)クラスに含まれる、 [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) 2 つの地理的な場所の間の距離の簡略化された計算を持つメソッドです。

これで使用される、 [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml)ファイルと[LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs)も博物館にユーザーの所在地からの距離を表示する、分離コード ファイル。

[![ローカルの美術館ページのスクリーン ショットをトリプル](images/ch28fg28-small.png "場所までの距離")](images/ch28fg28-large.png#lightbox "場所までの距離")

プログラムでは、動的にマップの場所に基づく pin の数を制限する方法も示します。

## <a name="geocoding-and-back-again"></a>ジオコード化との間

[ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)アセンブリも含まれています、 [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/)クラス、 [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/)に変換するメソッド0 またはより多くの地理的な位置や別のメソッドにテキスト アドレス[ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/)逆方向に変換します。

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml)ファイルと[GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs)分離コード ファイルは、この機能をデモンストレーションします。



## <a name="related-links"></a>関連リンク

- [28 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [28 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [マップ コントロール](~/xamarin-forms/user-interface/map.md)
