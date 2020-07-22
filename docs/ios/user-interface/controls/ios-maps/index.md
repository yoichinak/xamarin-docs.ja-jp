---
title: Xamarin. iOS のマップ
description: このドキュメントでは、iOS MapKit フレームワークと、それが Xamarin. iOS でどのように使用されるかについて説明します。 ここでは、マップの追加、スタイルの指定、パンとズーム、ユーザーの位置を操作する方法、注釈を追加する方法、コールアウトとオーバーレイを操作する方法、およびローカル検索を実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: cdb8505bd8c25b6e591dd86daf6a4d1e174012f6
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569037"
---
# <a name="maps-in-xamarinios"></a>Xamarin. iOS のマップ

Maps は、すべての最新のモバイルオペレーティングシステムで共通の機能です。 iOS では、マップキットフレームワークによってネイティブにマッピングがサポートされています。 マップキットを使用すると、アプリケーションは、リッチでインタラクティブなマップを簡単に追加できます。 これらのマップは、マップ上の場所をマークする注釈を追加したり、任意の図形のグラフィックをオーバーレイしたりするなど、さまざまな方法でカスタマイズできます。 マップキットには、デバイスの現在の場所を表示するためのサポートが組み込まれています。

## <a name="adding-a-map"></a>マップの追加

アプリケーションにマップを追加するには `MKMapView` 、次に示すように、インスタンスをビュー階層に追加します。

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

`MKMapView`は、 `UIView` マップを表示するサブクラスです。 上記のコードを使用してマップを追加するだけで、対話型のマップが生成されます。

![](images/00-map.png "A sample map")

## <a name="map-style"></a>マップスタイル

`MKMapView`では、3種類のマップのスタイルがサポートされています。 マップスタイルを適用するには、単に `MapType` プロパティを列挙型の値に設定し `MKMapType` ます。

```csharp
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
```

次のスクリーンショットは、使用可能なさまざまなマップスタイルを示しています。

![](images/01-mapstyles.png "This screenshot show the different map styles that are available")

## <a name="panning-and-zooming"></a>パンとズーム

`MKMapView`には、次のようなマップインタラクティビティ機能のサポートが含まれています。

- ピンチジェスチャを使用したズーム
- パンジェスチャによるパン

これらの機能は、インスタンスのプロパティとプロパティを設定するだけで有効または無効にすることができます。この場合、 `ZoomEnabled` `ScrollEnabled` `MKMapView` 既定値は両方とも true になります。 たとえば、静的マップを表示するには、適切なプロパティを false に設定するだけです。

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>ユーザーの位置

ユーザーの操作に加えて、に `MKMapView` は、デバイスの場所を表示するためのサポートも組み込まれています。 これは、*コアロケーション*フレームワークを使用して行われます。 ユーザーの場所にアクセスするには、ユーザーにメッセージを表示する必要があります。 これを行うには、のインスタンスを作成し、を `CLLocationManager` 呼び出し `RequestWhenInUseAuthorization` ます。

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

8.0 より前のバージョンの iOS では、を呼び出そうとするとエラーが発生することに注意 `RequestWhenInUseAuthorization` してください。 8より前のバージョンをサポートする場合は、その呼び出しを行う前に iOS のバージョンを確認してください。

ユーザーの場所にアクセスするには、**情報 plist**を変更する必要もあります。 位置データに関連する次のキーを設定する必要があります。

- **NSLocationWhenInUseUsageDescription**: アプリでやり取りしている間にユーザーの場所にアクセスする場合。
- **NSLocationAlwaysUsageDescription**: アプリがバック グラウンドでユーザーの場所にアクセスする場合。

これらのキーを追加するには、[ **plist** ] を開き、エディターの下部にある [*ソース*] を選択します。

**情報**を更新し、ユーザーにその場所へのアクセス許可を求めるメッセージが表示されたら、プロパティを true に設定してマップ上のユーザーの場所を表示できます `ShowsUserLocation` 。

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "The allow location access alert")

## <a name="annotations"></a>注釈

 `MKMapView`では、マップ上の画像 (注釈) の表示もサポートされています。 これには、カスタムイメージ、またはさまざまな色のシステム定義の pin を使用できます。 たとえば、次のスクリーンショットは、ピンとカスタムイメージの両方を含むマップを示しています。

 ![](images/03-annotations.png "This screenshot shows a map with a both a pin and a custom image")

### <a name="adding-an-annotation"></a>注釈の追加

注釈自体には、次の2つの部分があります。

- 注釈 `MKAnnotation` のタイトルや場所など、注釈に関するモデルデータを含むオブジェクト。
- `MKAnnotationView`表示するイメージと、ユーザーが注釈をタップしたときに表示されるコールアウトを格納する。

マップキットでは、iOS の委任パターンを使用して、の `Delegate` プロパティ `MKMapView` がのインスタンスに設定されているマップに注釈を追加し `MKMapViewDelegate` ます。 これは、このデリゲートの実装であり、 `MKAnnotationView` 注釈のを返します。

注釈を追加するには、まず、インスタンスでを呼び出して注釈を追加し `AddAnnotations` `MKMapView` ます。

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

注釈の位置がマップに表示されると、はその `MKMapView` デリゲートのメソッドを呼び出して `GetViewForAnnotation` 、 `MKAnnotationView` 表示するを取得します。

たとえば、次のコードはシステムが提供するを返し `MKPinAnnotationView` ます。

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>注釈の再利用

メモリを節約するため、では、 `MKMapView` テーブルのセルを再利用する場合と同様に、注釈ビューを再利用できるようにプールできます。 プールから注釈ビューを取得するには、次の呼び出しを行い `DequeueReusableAnnotation` ます。

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>コールアウトを表示する

前述のように、注釈は必要に応じてコールアウトを表示できます。 コールアウトを表示するに `CanShowCallout` は、で true に設定されているだけ `MKAnnotationView` です。 次のように、注釈がタップされると、注釈のタイトルが表示されます。

 ![](images/04-callout.png "The annotations title being displayed")

### <a name="customizing-the-callout"></a>コールアウトのカスタマイズ

次に示すように、吹き出しをカスタマイズして、左および右のアクセサリビューを表示することもできます。

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

このコードを実行すると、次のように吹き出しが表示されます。

 ![](images/05-callout-accessories.png "An example callout")

ユーザーが適切なアクセサリをタップする処理を行うには、でメソッドを実装するだけです `CalloutAccessoryControlTapped` `MKMapViewDelegate` 。

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>オーバーレイ

マップ上でグラフィックスをレイヤー化するもう1つの方法は、オーバーレイを使用することです。 オーバーレイでは、マップが拡大縮小されたときに、マップと一緒に拡大縮小されるグラフィカル コンテンツの描画がサポートされます。 iOS では、次のようないくつかの種類のオーバーレイがサポートされています。

- 多角形-マップ上の一部の領域を強調表示するためによく使用されます。
- ポリライン-多くの場合、ルートを表示するときに見られます。
- 円-マップの円形の領域を強調表示するために使用されます。

さらに、カスタムオーバーレイを作成して、カスタマイズされた詳細な描画コードを持つ任意のジオメトリを表示することもできます。 たとえば、気象レーダーはカスタムオーバーレイの候補として適しています。

#### <a name="adding-an-overlay"></a>オーバーレイの追加

注釈と同様に、オーバーレイの追加には次の2つの部分が含まれます。

- オーバーレイのモデルオブジェクトを作成し、に追加し `MKMapView` ます。
- でオーバーレイのビューを作成 `MKMapViewDelegate` します。

オーバーレイのモデルには、任意の `MKShape` サブクラスを指定できます。 Xamarin. iOS には、、、およびの各クラスを使用して、 `MKShape` 多角形、ポリライン、および円のサブクラスが含まれてい `MKPolygon` `MKPolyline` `MKCircle` ます。

たとえば、次のコードを使用してを追加し `MKCircle` ます。

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

オーバーレイのビューは、のに `MKOverlayView` よって返されるインスタンスです `GetViewForOverlay` `MKMapViewDelegate` 。 各には `MKShape` 、 `MKOverlayView` 指定された図形を表示する方法を認識する対応するがあります。 に `MKPolygon` はがあり `MKPolygonView` ます。 同様に、 `MKPolyline` はに対応し、に `MKPolylineView` `MKCircle` はがあり `MKCircleView` ます。

たとえば、次のコードは、 `MKCircleView` のを返し `MKCircle` ます。

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

次のように、マップ上に円が表示されます。

 ![](images/06-circle-overlay.png "A circle displayed on the map")

## <a name="local-search"></a>ローカル検索

iOS には、マップキットを使用したローカル検索 API が含まれています。これにより、指定された地理的領域内の関心のあるポイントを非同期検索できます。

ローカル検索を実行するには、アプリケーションで次の手順を実行する必要があります。

1. `MKLocalSearchRequest`オブジェクトを作成します。
1. `MKLocalSearch`からオブジェクトを作成 `MKLocalSearchRequest` します。
1. `Start`オブジェクトに対してメソッドを呼び出し `MKLocalSearch` ます。
1. `MKLocalSearchResponse`コールバック内のオブジェクトを取得します。

ローカル検索 API 自体は、ユーザーインターフェイスを提供しません。 マップを使用する必要もありません。 ただし、ローカル検索を実際に使用するには、アプリケーションで検索クエリを指定して結果を表示する方法が用意されている必要があります。 また、結果には位置データが含まれるため、多くの場合、マップに表示するのが理にかなっています。

<a name="Adding_a_Local_Search_UI"></a>

### <a name="adding-a-local-search-ui"></a>ローカル検索 UI の追加

検索入力を受け入れる方法の1つは、に `UISearchBar` よって提供されるを使用して、 `UISearchController` 結果をテーブルに表示することです。

次のコードでは、 `UISearchController` のメソッドに (検索バープロパティを持つ) を追加してい `ViewDidLoad` `MapViewController` ます。

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;

//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;
```

ユーザーインターフェイスには検索バーオブジェクトを組み込む必要があることに注意してください。 この例では、ナビゲーションバーの TitleView に割り当てましたが、アプリケーションでナビゲーションコントローラーを使用しない場合は、別の場所を検索して表示する必要があります。

このコードスニペットでは、別のカスタムビューコントローラー () を作成しまし `searchResultsController` た。これによって検索結果が表示され、このオブジェクトを使用して検索コントローラーオブジェクトが作成されました。 また、新しい検索アップデーターも作成しました。これは、ユーザーが検索バーと対話するときにアクティブになります。 各キーストロークで検索に関する通知を受け取り、UI の更新を担当します。
との両方を実装する方法については、 `searchResultsController` `searchResultsUpdater` このガイドの後半で説明します。

この結果、次に示すように、マップ上に検索バーが表示されます。

 ![](images/07-searchbar.png "A search bar displayed over the map")

### <a name="displaying-the-search-results"></a>検索結果の表示

検索結果を表示するには、カスタムビューコントローラーを作成する必要があります。通常は `UITableViewController` です。 上に示すように、を `searchResultsController` 作成するときに、のコンストラクターにが渡され `searchController` ます。
このカスタムビューコントローラーを作成する方法の例を次のコードに示します。

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });

    }
}
```

### <a name="updating-the-search-results"></a>検索結果の更新

は、 `SearchResultsUpdater` `searchController` の検索バーと検索結果の間の媒介として機能します。

この例では、最初にで検索メソッドを作成する必要があり `SearchResultsViewController` ます。 これを行うには、オブジェクトを作成し、それを使用しての検索を発行する必要があり `MKLocalSearch` `MKLocalSearchRequest` ます。結果は、オブジェクトのメソッドに渡されたコールバックで取得され `Start` `MKLocalSearch` ます。 結果は、 `MKLocalSearchResponse` オブジェクトの配列を含むオブジェクトで返され `MKMapItem` ます。

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });

}
```

次に、「 `MapViewController` `UISearchResultsUpdating` `SearchResultsUpdater` `searchController` [ローカル検索 UI を追加](#Adding_a_Local_Search_UI)する」セクションでのプロパティに割り当てられたのカスタム実装を作成します。

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

上の実装では、次のように、結果から項目が選択されたときにマップに注釈を追加します。

 ![](images/08-search-results.png "An annotation added to the map when an item is selected from the results")

> [!IMPORTANT]
> `UISearchController`は、iOS 8 で実装されました。 これより前のデバイスをサポートする場合は、を使用する必要があり `UISearchDisplayController` ます。

## <a name="summary"></a>まとめ

この記事では、iOS 用*Map* *Kit*フレームワークについて説明します。 まず、クラスを使用して、 `MKMapView` 対話的なマップをアプリケーションに含める方法を見てきました。 さらに、注釈とオーバーレイを使用してマップをカスタマイズする方法についても説明します。 最後に、iOS 6.1 を使用してマップキットに追加されたローカル検索機能を確認しました。ここでは、関心のあるポイントに対して場所ベースのクエリを実行し、マップに追加する方法を示します。

## <a name="related-links"></a>関連リンク

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
