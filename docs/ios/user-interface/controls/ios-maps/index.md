---
title: "マップ"
description: "iOS には、フレームワークで、簡単に追加するアプリケーションにマップされて MapKit が含まれています。 IOS アプリケーションは、マップ キットを使用して、パン、ズーム、地図上のユーザーの場所と重ね順の豊富なグラフィックスの表示などの機能をサポートする対話型のマップを追加できます。 いくつかのアプリケーションに地理的な機能を構築するように利用する方法を示す、マップ キットの機能について詳しく説明この記事の内容"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3fecf17a4f70e44ca169c825bf0dd34a5127cec8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="maps"></a>マップ

_iOS には、フレームワークで、簡単に追加するアプリケーションにマップされて MapKit が含まれています。IOS アプリケーションは、マップ キットを使用して、パン、ズーム、地図上のユーザーの場所と重ね順の豊富なグラフィックスの表示などの機能をサポートする対話型のマップを追加できます。いくつかのアプリケーションに地理的な機能を構築するように利用する方法を示す、マップ キットの機能について詳しく説明この記事の内容_

マップは、すべての最新のモバイル オペレーティング システムで一般的な機能です。 iOS では、マップ キット フレームワークを通じてネイティブ サポートがマッピングされています。 マップ キットを使用して、アプリケーションは、豊富な対話型のマップを簡単に追加できます。 これらのマップは、さまざまな、地図上に場所を示すために注釈を追加して、任意図形のグラフィックスのオーバーレイなどの方法でカスタマイズできます。 マップ キットでは、デバイスの現在の場所を表示するための組み込みサポートもあります。

## <a name="adding-a-map"></a>マップの追加

マップをアプリケーションに追加するために追加することでは、`MKMapView`次に示すように階層の表示するインスタンスします。

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` `UIView`マップを表示するサブクラスです。 単に上記のコードを使用してマップを追加するには、対話型のマップが生成されます。

 ![](images/00-map.png "サンプル マップ")

## <a name="map-style"></a>マップのスタイル

 `MKMapView` マップの 3 種類のスタイルをサポートしています。 マップのスタイルを適用する設定、`MapType`プロパティから値を`MKMapType`列挙。
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  次のスクリーン ショットは、使用可能な別のマップのスタイルを表示します。

 ![](images/01-mapstyles.png "このスクリーン ショットは、使用可能な別のマップのスタイルを表示します。")

## <a name="panning-and-zooming"></a>パンとズーム

 `MKMapView` マップの対話機能のサポートにはなどが含まれています。

-  ピンチ ジェスチャを使用してズーム
-  パン ジェスチャを使用してパン


これらの機能を有効になっているか、設定するだけで無効になっている、`ZoomEnabled`と`ScrollEnabled`のプロパティ、`MKMapView`インスタンス、既定値が両方に当てはまります。 たとえば、静的なマップを表示するだけで、適切なプロパティ false に設定します。

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>ユーザーの場所

ユーザーの操作だけでなく`MKMapView`デバイスの場所を表示するための組み込みサポートがあります。 これを使用して、*コア場所*フレームワークです。 ユーザーの所在地にアクセスする前にユーザーを求める必要があります。 これを行うには、インスタンスを作成`CLLocationManager`を呼び出すと`RequestWhenInUseAuthorization`です。

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

IOS 8.0 では、呼び出そうとするとより前のバージョンで`RequestWhenInUseAuthorization`エラーが発生します。 8 より前のバージョンをサポートする場合は、その呼び出しを行う前に、iOS のバージョンを確認してください。

変更が必要です、ユーザーの場所へのアクセスも**Info.plist**です。 位置データに関連する次のキーを設定する必要があります。

- **NSLocationWhenInUseUsageDescription**: アプリでやり取りしている間にユーザーの場所にアクセスする場合。
- **NSLocationAlwaysUsageDescription**: アプリがバック グラウンドでユーザーの場所にアクセスする場合。

これらのキーを追加するには、開く**Info.plist**を選択して*ソース*エディターの下部にあります。

更新すると**Info.plist**とその場所へのアクセス許可をユーザーに求めるメッセージが表示、マップにユーザーの所在地を表示するには設定して、`ShowsUserLocation`プロパティを true にします。

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "許可する場所のアクセスのアラート")
 
## <a name="annotations"></a>コメント

 `MKMapView` イメージを表示する、マップ上のコメントと呼ばれるもサポートしています。 これらは、カスタム イメージまたはさまざまな色のピンがシステム定義のいずれかであることができます。 たとえば、次のスクリーン ショットが両方 pin とマップを表示し、カスタム イメージ。

 ![](images/03-annotations.png "このスクリーン ショットは、両方 pin とマップを表示し、カスタム イメージ")

### <a name="adding-an-annotation"></a>注釈の追加

注釈自体には、2 つの部分があります。

-  `MKAnnotation`オブジェクトで、タイトルと注釈の場所など、注釈に関するモデルのデータが含まれています。
-  `MKAnnotationView` 、および必要に応じて、ユーザーが注釈をタップしたときに表示されているコールアウト表示するイメージが含まれています。


マップ キット iOS 委任パターンを使用して、マップに注釈を追加する場所、`Delegate`のプロパティ、`MKMapView`のインスタンスに設定されている、`MKMapViewDelegate`です。 このデリゲートの実装に返すことを担当する、`MKAnnotationView`の注釈。

最初に注釈を追加するには、注釈は呼び出すことによって追加`AddAnnotations`上、`MKMapView`インスタンス。

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

注釈の位置が、マップに表示されるときに、`MKMapView`が呼び出す、デリゲートの`GetViewForAnnotation`取得するメソッド、`MKAnnotationView`を表示します。

たとえば、次のコード返しますシステム提供`MKPinAnnotationView`:

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

### <a name="reusing-annotations"></a>注釈を再利用

メモリを節約するために`MKMapView`注釈ビューが再利用できるように、テーブルのセルが再利用する場合と同様にプールできるを許可します。 呼び出しには、プールから注釈ビューを取得する`DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>コールアウト付きの表示

前述のように、注釈は必要に応じて、引き出し線を表示できます。 引き出し線を単純に表示するには設定`CanShowCallout`場合は true を`MKAnnotationView`です。 これは、結果に示すように、注釈をタップすると、ときに表示されている注釈のタイトル。

 ![](images/04-callout.png "表示されている注釈のタイトル")

### <a name="customizing-the-callout"></a>引き出し線のカスタマイズ

引き出し線は次のように、左と右付属品のビューを表示するカスタマイズすることも。

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

このコードは、次のコールアウトが得られます。

 ![](images/05-callout-accessories.png "例の引き出し線")

右のアクセサリをタップすると、ユーザーを処理するための実装だけで、`CalloutAccessoryControlTapped`メソッドで、 `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>オーバーレイ

マップにレイヤー グラフィックを別の方法は、オーバーレイを使用しています。 オーバーレイに拡大されると、マップされた拡大縮小描画のグラフィカルなコンテンツをサポートします。 iOS では、オーバーレイなどのいくつかの型のサポートを提供します。

-  多角形のマップ上のいくつかの領域を強調表示に一般的に使用します。
-  多角形、ルートを表示するときによく見られます。
-  円の円形のマップの領域を強調表示するために使用します。


さらに、任意のジオメトリを細かく制御できる、カスタマイズされた描画コードを表示するカスタムのオーバーレイを作成できます。 たとえば、気象レーダーはカスタム オーバーレイの候補になります。

#### <a name="adding-an-overlay"></a>オーバーレイを追加します。

注釈と同様に、オーバーレイを追加する 2 つの部分には。

-  オーバーレイのモデル オブジェクトを作成しに追加すること、`MKMapView`です。
-  オーバーレイのビューを作成する、`MKMapViewDelegate`です。


オーバーレイのモデルには、いずれかを指定できる`MKShape`サブクラスです。 Xamarin.iOS が含まれています`MKShape`多角形、多角形、および円のサブクラスを使用して、 `MKPolygon`、`MKPolyline`と`MKCircle`それぞれクラスです。

追加する次のコードを使用するなど、 `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

オーバーレイの表示は、`MKOverlayView`によって返されるインスタンス、`GetViewForOverlay`で、`MKMapViewDelegate`です。 各`MKShape`に、対応する`MKOverlayView`を特定の形状を表示する方法を認識します。 `MKPolygon`が`MKPolygonView`です。 同様に、`MKPolyline`に対応する`MKPolylineView`、および`MKCircle`が`MKCircleView`です。

たとえば、次のコードを返します、`MKCircleView`の`MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

これは、表示します。 円、マップに。

 ![](images/06-circle-overlay.png "マップに表示される円")

## <a name="local-search"></a>ローカルの検索

iOS には、ローカル検索マップ キットは、指定した地理的領域内の追跡ポイントの非同期検索 API が含まれています。

ローカルの検索を実行するには、アプリケーションは以下の手順を実行する必要があります。

1.  作成`MKLocalSearchRequest`オブジェクト。
1.  作成、`MKLocalSearch`オブジェクトから、`MKLocalSearchRequest`です。
1.  呼び出す、`Start`メソッドを`MKLocalSearch`オブジェクト。
1.  取得、`MKLocalSearchResponse`コールバック オブジェクト。


ローカルの検索 API 自体は、ユーザー インターフェイスを提供されません。 使用するマップには必要ありません。 ただし、実用的なを使用するローカルの検索、アプリケーションが、検索クエリを指定し、結果を表示する手段を提供する必要があります。 さらに、結果には、場所データが含まれているが、のでさせることが多くの場合、マップで表示することです。

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>ローカル検索 UI を追加します。

検索入力をそのまま使用する 1 つの方法は、 `UISearchBar`、によって提供される、`UISearchController`され、テーブル内の結果が表示されます。

次のコードを追加、 `UISearchController` (検索バーのプロパティに) で、`ViewDidLoad`メソッドの`MapViewController`:

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

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

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

`SearchResultsUpdater`間の媒介として機能し、`searchController`の検索バー、検索結果。 

最初に検索メソッドを作成するがこの例では、`SearchResultsViewController`です。 こうすることを作成する必要があります、`MKLocalSearch`オブジェクトし、発行の検索に使用する、`MKLocalSearchRequest`に渡されたコールバックでは、結果が取得された、`Start`のメソッド、`MKLocalSearch`オブジェクト。 結果は次に返されます、`MKLocalSearchResponse`オブジェクトの配列を含む`MKMapItem`オブジェクト。

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

次に、`MapViewController`のカスタム実装を作成`UISearchResultsUpdating`に割り当てられる、`SearchResultsUpdater`のプロパティ、`searchController`で、[ローカル検索 UI を追加する](#Adding_a_Local_Search_UI)セクション。

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

上記の実装では、注釈、マップに追加項目が、結果から、選択されたときに次のように。

 ![](images/08-search-results.png "結果から項目を選択すると、マップに追加注釈")
 
> [!IMPORTANT]
> `UISearchController` iOS 8 に実装されました。 これより前にデバイスをサポートするかどうかは、使用する必要があります`UISearchDisplayController`です。



## <a name="summary"></a>まとめ

この記事の検査、*マップ**キット*ios フレームワークです。 最初に、方法で検索する、`MKMapView`クラスは、アプリケーションに含まれる対話型のマップを使用できます。 注釈、オーバーレイを使用して、マップをさらにカスタマイズする方法を示しました。 最後に、ios 6.1、マップ キットに追加されたローカルの検索機能を調べるを使用する方法を示す追跡ポイントの場所ベースのクエリを実行し、マップに追加します。

## <a name="related-links"></a>関連リンク

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (sample)](https://developer.xamarin.com/samples/monotouch/MapDemo)
