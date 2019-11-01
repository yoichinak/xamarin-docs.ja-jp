---
title: 第28章の概要。 場所とマップ
description: Xamarin を使用した Mobile Apps の作成:第28章の概要。 場所とマップ
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5dcd84536cc6d80deb753fc6fe57f9090f6b2dad
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697072"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>第28章の概要。 場所とマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> このページのメモでは、分岐が書籍に記載されている素材から見た領域を示しています。

Xamarin. Forms は、`View`から派生する[`Map`](xref:Xamarin.Forms.Maps.Map)要素をサポートしています。 Maps の使用には特別なプラットフォーム要件があるため、これらは別のアセンブリである**Xamarin. Forms. map**に実装され、`Xamarin.Forms.Maps`とは異なる名前空間が関係します。

## <a name="the-geographic-coordinate-system"></a>地理的な座標系

地理的座標系は、地球のような球体 (またはほぼ球体) のオブジェクト上の位置を識別します。 座標は、角度で表される*緯度*と*経度*の両方で構成されます。

`equator` と呼ばれる優れた円は、地球の軸が概念的に拡張する2つの電柱の中間にあります。

### <a name="parallels-and-latitude"></a>平行になると緯度

地球の中心から赤道の北または南に測定された角度は、*平行*一致と呼ばれる同等の緯度の線をマークします。 これらの範囲は、赤道の0°から北と南部の90度までです。 慣例により、赤道の北緯度は正の値であり、赤道の南部は負の値です。

### <a name="longitude-and-meridians"></a>経度と経線

北の極から南の極までの優れた円の半分は、*経線*とも呼ばれる等しい経度の線です。 これらは、英国の英国のプライム経線を基準としています。 慣例として、素数経線の経度東部は 0 ~ 180 度の正の値であり、素数経線の経度西部は負の値であり、0度から180度 &ndash;になります。

### <a name="the-equirectangular-projection"></a>予想される四角形の投影

地球のフラットマップでは、ゆがみが発生します。 緯度と経度のすべての線が直線で、緯度と経度の角度が等しい場合、マップの距離が均等になると、結果は*直交する四角形の投影*になります。 このマップは、水平方向に拡張されるため、電柱に近い領域を歪めます。

### <a name="the-mercator-projection"></a>商品の投影

*人気の*あるのは、これらの領域を垂直方向に拡大することで、水平方向の伸縮を補正することです。 これにより、電話の近くにある領域が実際の値よりもかなり大きいと思われるマップが生成されますが、ローカル領域は実際の領域と密接に一致します。

### <a name="map-services-and-tiles"></a>サービスとタイルのマップ

マップサービスでは、`Web Mercator`と呼ばれる商品のバリエーションを使用します。 マップサービスは、位置とズームレベルに基づいてクライアントにビットマップタイルを配信します。

## <a name="getting-the-users-location"></a>ユーザーの所在地を取得する

Xamarin `Map` クラスには、ユーザーの地理的な場所を取得する機能が含まれていませんが、多くの場合、マップを操作するときは、依存関係サービスがそれを処理する必要があります。

> [!NOTE]
> 代わりに、xamarin アプリケーションでは、Xamarin. Essentials に含まれている[`Geolocation`](~/essentials/geolocation.md)クラスを使用できます。

### <a name="the-location-tracker-api"></a>ロケーショントラッカー API

[**Xamarin. のプラットフォーム**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューションには、LOCATION tracker API のコードが含まれています。 [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs)構造体は、緯度と経度をカプセル化します。 [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs)インターフェイスは、位置トラッカーを開始および一時停止する2つのメソッドと、新しい場所が使用可能になったときのイベントを定義します。

#### <a name="the-ios-location-manager"></a>IOS ロケーションマネージャー

`ILocationTracker` の iOS 実装は、iOS [`CLLocationManager`](xref:CoreLocation.CLLocationManager)を利用する[`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs)クラスです。

#### <a name="the-android-location-manager"></a>Android ロケーションマネージャー

Android の `ILocationTracker` の実装は、Android [`LocationManager`](xref:Android.Locations.LocationManager)クラスを使用する[`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs)クラスです。

#### <a name="the-uwp-geo-locator"></a>UWP geo ロケーター

`ILocationTracker` のユニバーサル Windows プラットフォームの実装は、UWP [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator)を利用する[`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs)クラスです。

### <a name="display-the-phones-location"></a>電話の場所を表示する

ここで[**は、場所**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI)トラッカーを使用して、電話の場所 (テキストと、両方の文字が入力された四角形の両方) を表示します。

### <a name="the-required-overhead"></a>必要なオーバーヘッド

場所の追跡ツールを使用するには、いくつ**かのオーバーヘッド**が必要です。 1つ目の方法として**は、** すべてのプロジェクトが、そのソリューション内のすべてのプロジェクトが、対応するプロジェクトを参照している必要があり**ます。また** **、各プロジェクト**は、`Toolkit.Init` メソッドを呼び出す必要があります。

場所のアクセス許可の形式で、プラットフォーム固有の追加のオーバーヘッドが必要です。

#### <a name="location-permission-for-ios"></a>IOS の場所のアクセス許可

IOS の場合、**情報の plist**ファイルには、ユーザーの場所を取得することをユーザーに求める質問のテキストを含む項目が含まれている必要があります。

#### <a name="location-permissions-for-android"></a>Android の場所のアクセス許可

ユーザーの場所を取得する Android アプリケーションには、AndroidManifest .xml ファイルの ACCESS_FILE_LOCATION アクセス許可が必要です。

#### <a name="location-permissions-for-the-uwp"></a>UWP の場所のアクセス許可

ユニバーサル Windows プラットフォームアプリケーションには、package.appxmanifest ファイルでマークされた `location` デバイス機能が必要です。

## <a name="working-with-xamarinformsmaps"></a>Xamarin. Forms. マップの操作

`Map` クラスの使用には、いくつかの要件が関係しています。

### <a name="the-nuget-package"></a>NuGet パッケージ

アプリケーションソリューションには、 **Xamarin. Forms. Maps** NuGet ライブラリを追加する必要があります。 バージョン番号は、現在インストールされている**Xamarin. Forms**パッケージと同じである必要があります。

### <a name="initializing-the-maps-package"></a>Maps パッケージを初期化しています

アプリケーションプロジェクトは、`Xamarin.Forms.Forms.Init`の呼び出しを行った後、`Xamarin.FormsMaps.Init` メソッドを呼び出す必要があります。

### <a name="enabling-map-services"></a>マップサービスの有効化

`Map` はユーザーの場所を取得できるため、アプリケーションは、この章で前に説明した方法で、ユーザーのアクセス許可を取得する必要があります。

#### <a name="enabling-ios-maps"></a>IOS マップの有効化

`Map` を使用する iOS アプリケーションでは、情報 plist ファイルに2行が必要です。

#### <a name="enabling-android-maps"></a>Android maps を有効にする

Google マップサービスを使用するには、承認キーが必要です。 このキーは、 **Androidmanifest .xml**ファイルに挿入されます。 さらに、 **Androidmanifest .xml**ファイルには、ユーザーの場所の取得に関連する `manifest` タグが必要です。

#### <a name="enabling-uwp-maps"></a>UWP マップの有効化

ユニバーサル Windows プラットフォームアプリケーションでは、Bing Maps を使用するための承認キーが必要です。 このキーは、`Xamarin.FormsMaps.Init` メソッドに引数として渡されます。 また、ロケーションサービスに対してもアプリケーションを有効にする必要があります。

### <a name="the-unadorned-map"></a>非修飾マップ

[**Mapdemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos)のサンプルは、さまざまなデモンストレーションプログラムへの移動を可能にする、[MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml)ファイルと、分離コードファイルで構成されています。 [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs)

[Basicmappage .xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml)ファイルは、 [`Map`](xref:Xamarin.Forms.Maps.Map)ビューを表示する方法を示しています。 既定では、ローマ市が表示されますが、マップはユーザーが操作できます。

水平方向および垂直方向のスクロールを無効にするには、 [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)プロパティを `false`に設定します。 ズームを無効にするには、 [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)を `false`に設定します。 これらのプロパティは、すべてのプラットフォームで動作しない可能性があります。

### <a name="streets-and-terrain"></a>通りと地形

次の3つのメンバーを持つ列挙体[`MapType`](xref:Xamarin.Forms.Maps.MapType)型の `Map` プロパティ[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)を設定することにより、さまざまな種類のマップを表示できます。

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street)、既定値
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[Mapタイプのページ .xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml)ファイルは、オプションボタンを使用してマップの種類を選択する方法を示しています。 このクラスは、 [MapTypeRadioButton](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml)ファイルに基づいて、 [**Xamarin.** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)形式の Toolkit ライブラリとクラスの[`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs)クラスを使用します。

### <a name="map-coordinates"></a>マップの座標

プログラムは、 [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)プロパティを通じて `Map` が表示している現在の領域を取得できます。 このプロパティは、バインド可能なプロパティではサポートされて*おらず*、変更された日時を示す通知メカニズムは存在しないため、プロパティを監視するプログラムでは、その目的のためにタイマーが使用される可能性があります。

`VisibleRegion` は、次の4つの読み取り専用プロパティを持つクラス[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)型です。

- [`Position`](xref:Xamarin.Forms.Maps.Position) 型の [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)
- マップに表示されている領域の高さを示す `double`型の[`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)
- マップに表示されている領域の幅を示す `double`型の[`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)
- マップに表示される最大の円形領域のサイズを示す[`Distance`](xref:Xamarin.Forms.Maps.Distance)型の[`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)

`Position` と `Distance` は両方とも構造です。 `Position` は、 [`Position` コンストラクター](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double))を使用して設定される2つの読み取り専用プロパティを定義します。

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` は、メトリックと英語の単位を変換することによって、単位に依存しない距離を提供することを目的としています。 `Distance` 値は、いくつかの方法で作成できます。

- 距離がメートル単位の[`Distance` コンストラクター](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double))
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double))の静的メソッド
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double))の静的メソッド
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double))の静的メソッド

この値は、次の3つのプロパティから取得できます。

- `double` 型の [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)
- `double` 型の [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)
- `double` 型の [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)

[MapCoordinatesPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml)ファイルには、`MapSpan` 情報を表示するための `Label` 要素がいくつか含まれています。 [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs)分離コードファイルは、ユーザーがマップを操作するときに、タイマーを使用して情報を更新したままにします。

### <a name="position-extensions"></a>位置拡張

このブックの新しいライブラリ ( [**Xamarin.** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) .....) には、マップ固有のプラットフォームに依存しない型が含まれています。 [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)クラスには `Position`の `ToString` メソッドと、2つの `Position` 値間の距離を計算するメソッドがあります。

### <a name="setting-an-initial-location"></a>初期の場所の設定

`Map` の[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan))メソッドを呼び出すと、マップの位置とズームレベルをプログラムによって設定できます。 引数の型は `MapSpan`です。 次のいずれかを使用して、`MapSpan` オブジェクトを作成できます。

- `Position`、緯度と経度のスパンを持つ[`MapSpan` コンストラクター](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double))
- `Position` と radius を使用した[`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance))

また、 [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double))または[`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double))メソッドを使用して、既存のものから新しい `MapSpan` を作成することもできます。

[WyomingPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml)ファイルと[WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs)分離コードファイルは、`MoveToRegion` メソッドを使用してワイオミング州の状態を表示する方法を示しています。

または、 [`Map` コンストラクター](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan))と `MapSpan` オブジェクトを使用して、マップの場所を初期化することもできます。 [XamarinHQPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml)ファイルは、この操作を xaml で完全に実行して、サンフランシスコに Xamarin の本社を表示する方法を示しています。

### <a name="dynamic-zooming"></a>動的ズーム

`Slider` を使用すると、マップを動的にズームできます。 [RadiusZoomPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml)ファイルと[RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs)分離コードファイルは、`Slider` 値に基づいてマップの半径を変更する方法を示しています。

[LongitudeZoomPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml)ファイルと[LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs)分離コードファイルには、Android では適切に機能する別の方法が示されていますが、どちらの方法も Windows プラットフォームでは適切に機能しません。

### <a name="the-phones-location"></a>電話の場所

`Map` の [ [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) ] プロパティは、次のように、各プラットフォームで、 [showlocationpage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml)のように動作が異なります。

- IOS では、青い点は電話の場所を示しますが、手動で移動する必要があります。
- Android では、プッシュ時にマップが電話の場所に移動するとアイコンが表示されます。
- UWP は iOS に似ていますが、場所に自動的に移動する場合があります。

**Mapdemos**プロジェクトでは、最初に、 [mylocationbutton .xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml)ファイルと[MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs)分離コードファイルに基づいてアイコンベースのボタンを定義することで、Android のアプローチを模倣しようとしています。

[GoToLocationPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml)ファイルと[GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs)分離コードファイルでは、このボタンを使用して電話の場所に移動します。

### <a name="pins-and-science-museums"></a>Pin とサイエンス美術館

最後に、`Map` クラスは `IList<Pin>`型の[`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)プロパティを定義します。 [`Pin`](xref:Xamarin.Forms.Maps.Pin)クラスは、次の4つのプロパティを定義します。

- 必須プロパティである `string`型の[`Label`](xref:Xamarin.Forms.Maps.Pin.Label)
- `string`型の[`Address`](xref:Xamarin.Forms.Maps.Pin.Address) (任意のユーザーが判読できるアドレス)
- マップにピンが表示される場所を示す `Position`型の[`Position`](xref:Xamarin.Forms.Maps.Pin.Position)
- [`PinType`](xref:Xamarin.Forms.Maps.PinType)型の[`Type`](xref:Xamarin.Forms.Maps.Pin.Type) 。列挙型。これは使用されません。

**Mapdemos**プロジェクトには、ScienceMuseums ファイルが含まれてい[ます。](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml)このファイルには、米国内のサイエンス美術館[`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs)と、このデータを逆シリアル化するためのクラスと[`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs)クラスが示されています。

[ScienceMuseumsPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml)ファイルと[ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs)分離コードファイルには、マップ内のこれらのサイエンス美術館のピンが表示されます。 ユーザーが pin をタップすると、その博物館の住所と web サイトが表示されます。

### <a name="the-distance-between-two-points"></a>2つの点の間の距離

[`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)クラスには、2つの地理的な場所の距離を簡略化した計算を行う[`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88)メソッドが含まれています。

これは、 [LocalMuseumsPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml)ファイルと[LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs)分離コードファイルで、ユーザーの位置から博物館までの距離も表示するために使用されます。

[![ローカル美術館ページのトリプルスクリーンショット](images/ch28fg28-small.png "場所への距離")](images/ch28fg28-large.png#lightbox "場所への距離")

また、このプログラムでは、マップの位置に基づいて pin の数を動的に制限する方法も示しています。

## <a name="geocoding-and-back-again"></a>ジオコーディングと戻る

また、 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String))メソッドを含む[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)クラスも含まれています。このメソッドは、テキストアドレスを0個以上の地理的位置に変換し、もう一方のメソッドを変換する[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position))別のメソッドを使用し[**ます**](xref:Xamarin.Forms.Maps)。横書き.

[GeocoderRoundTrip](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml)ファイルと[GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs)分離コードファイルは、この機能を示しています。

## <a name="related-links"></a>関連リンク

- [第28章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [第 28 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin. Forms マップ](~/xamarin-forms/user-interface/map/index.md)
