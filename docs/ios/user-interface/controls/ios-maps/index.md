---
title: Xamarin.iOS でのマップ
description: このドキュメントでは、iOS の MapKit フレームワークと Xamarin.iOS での使用方法について説明します。 これには、スタイル、パンしズーム、ユーザーの場所を使用、注釈を追加、コールアウト、およびオーバーレイ、操作およびローカル検索を実行、マップを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: c61b5a8bd99afda5e8fbeea44e3362574fa7feea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61226731"
---
# <a name="maps-in-xamarinios"></a>Xamarin.iOS でのマップ

マップは、すべての最新のモバイル オペレーティング システムで一般的な機能です。 iOS では、Map Kit フレームワークを通じてネイティブ マッピング サポートを提供します。 マップのキット、アプリケーションは、リッチで対話型のマップを簡単に追加できます。 これらのマップは、さまざまな、マップ上の場所をマークする注釈を追加して、任意図形のグラフィックスのオーバーレイなどの方法でカスタマイズできます。 Map Kit もデバイスの現在の場所を表示するための組み込みのサポートしています。

## <a name="adding-a-map"></a>マップの追加

マップをアプリケーションに追加するために追加することでは、`MKMapView`次に示すように、ビュー階層にインスタンスします。

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` `UIView`マップを表示するサブクラスです。 上記のコードを使用してマップを追加するだけには、対話型のマップが生成されます。

 ![](images/00-map.png "サンプル マップ")

## <a name="map-style"></a>マップ スタイル

 `MKMapView` マップの 3 つの異なるスタイルをサポートしています。 マップ スタイルを適用する設定、`MapType`プロパティから値を`MKMapType`列挙体。
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  次のスクリーン ショットは、使用可能な別のマップのスタイルを表示します。

 ![](images/01-mapstyles.png "このスクリーン ショットは、利用できる別のマップのスタイルを表示します。")

## <a name="panning-and-zooming"></a>パンとズーム

 `MKMapView` マップのインタラクティビティ機能のサポートにはなどが含まれています。

-  ピンチ ジェスチャを使用してズーム
-  パン ジェスチャを使用してパン


これらの機能を有効になっているか、設定するだけで無効になっている、`ZoomEnabled`と`ScrollEnabled`のプロパティ、`MKMapView`インスタンス、既定値が両方に当てはまります。 たとえば、静的なマップを表示する単に、適切なプロパティ false に設定します。

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>ユーザーの場所

ユーザーの操作だけでなく`MKMapView`デバイスの場所を表示するための組み込みサポートがあります。 これを使用して、 *Core 場所*フレームワーク。 ユーザーの場所にアクセスする前に、ユーザーにダイアログを表示する必要があります。 これを行うには、インスタンスを作成`CLLocationManager`を呼び出すと`RequestWhenInUseAuthorization`します。

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

IOS 8.0 では、呼び出しを試みるより前のバージョンで`RequestWhenInUseAuthorization`エラーが発生します。 IOS のバージョンを 8 より前のバージョンをサポートする場合は、その呼び出しを行う前に確認してください。

変更が必要です、ユーザーの場所へのアクセスも**Info.plist**します。 位置データに関連する次のキーを設定する必要があります。

- **NSLocationWhenInUseUsageDescription**: アプリでやり取りしている間にユーザーの場所にアクセスする場合。
- **NSLocationAlwaysUsageDescription**: アプリがバック グラウンドでユーザーの場所にアクセスする場合。

これらのキーを追加するには、開く**Info.plist**を選択して*ソース*エディターの下部にあります。

更新すると**Info.plist**とその場所へのアクセス許可をユーザーに求めるメッセージが表示、マップでユーザーの場所を表示するには設定して、`ShowsUserLocation`プロパティを true にします。

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "許可する場所のアクセスのアラート")
 
## <a name="annotations"></a>コメント

 `MKMapView` イメージを表示する、マップ上の注釈と呼ばれるをサポートしています。 カスタム イメージまたはさまざまな色のシステム定義のピンのいずれかを指定できます。 たとえば、次のスクリーン ショットが両方 pin を使用してマップを表示し、カスタム イメージ。

 ![](images/03-annotations.png "このスクリーン ショットは両方 pin を使用してマップし、カスタム イメージ")

### <a name="adding-an-annotation"></a>注釈の追加

注釈自体には、2 つの部分があります。

-  `MKAnnotation`オブジェクトで、注釈、タイトルや注釈の場所などに関するデータ モデルにはが含まれています。
-  `MKAnnotationView` 、イメージを表示して、必要に応じてユーザーが、注釈をタップしたときに表示されているコールアウトを含むです。


Map Kit は iOS の委任パターンを使用して、マップに注釈を追加する場所、`Delegate`のプロパティ、`MKMapView`のインスタンスに設定されている、`MKMapViewDelegate`します。 このデリゲートの実装が返されますが、`MKAnnotationView`注釈の。

最初に注釈を追加するには、注釈は呼び出すことによって追加`AddAnnotations`上、`MKMapView`インスタンス。

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

注釈の位置が、マップに表示されるとき、`MKMapView`が呼び出す、デリゲートの`GetViewForAnnotation`を取得するメソッド、`MKAnnotationView`を表示します。

次のコードでのシステム指定の返しますなど`MKPinAnnotationView`:

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

メモリを節約するために`MKMapView`プール テーブルのセルが再利用するように再利用するための注釈の表示を許可します。 呼び出しでは、注釈の表示をプールから取得`DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>吹き出しを表示

前述のように、注釈は必要に応じて、吹き出しを表示できます。 単に吹き出しを表示するには設定`CanShowCallout`を true に、`MKAnnotationView`します。 これは、結果、注釈のタイトルが示すように、注釈がタップされたときに表示されています。

 ![](images/04-callout.png "表示されている注釈のタイトル")

### <a name="customizing-the-callout"></a>引き出し線のカスタマイズ

次に示すよう、左と右アクセサリのビューを表示する引き出し線をカスタマイズもできます。

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

このコードは、次のコールアウトが得られます。

 ![](images/05-callout-accessories.png "例の引き出し線")

右側のアクセサリをタップしてユーザーを処理するために単純に実装、`CalloutAccessoryControlTapped`メソッドで、 `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>オーバーレイ

マップ レイヤーのグラフィックスを別の方法では、オーバーレイを使用しています。 オーバーレイでは、マップが拡大縮小されたときに、マップと一緒に拡大縮小されるグラフィカル コンテンツの描画がサポートされます。 iOS では、いくつかの種類など、オーバーレイのサポートを提供します。

-  多角形のマップ上のいくつかの領域を強調表示によく使用します。
-  ポリライン - ルートを表示するときによく見られます。
-  円 - マップの円形の領域を強調表示するために使用します。


さらに、詳細なカスタマイズされた描画コードの任意のジオメトリを表示するカスタムのオーバーレイを作成できます。 たとえば、気象レーダーはカスタムのオーバーレイの有力候補になります。

#### <a name="adding-an-overlay"></a>オーバーレイの追加

注釈と同様に、オーバーレイを追加する 2 つの部分には。

-  オーバーレイのモデル オブジェクトを作成し、追加すること、`MKMapView`します。
-  オーバーレイのビューを作成する、`MKMapViewDelegate`します。


オーバーレイのモデルには、いずれかを指定できる`MKShape`サブクラスです。 Xamarin.iOS が含まれています`MKShape`多角形、多角形や円のサブクラスを使用して、 `MKPolygon`、`MKPolyline`と`MKCircle`それぞれクラスします。

追加する次のコードを使用するなど、 `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

オーバーレイのビューは、`MKOverlayView`インスタンスによって返される、`GetViewForOverlay`で、`MKMapViewDelegate`します。 各`MKShape`対応しています`MKOverlayView`特定の形状を表示する方法を把握しています。 `MKPolygon`が`MKPolygonView`します。 同様に、`MKPolyline`に対応する`MKPolylineView`、および`MKCircle`が`MKCircleView`します。

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

マップに円を表示このように。

 ![](images/06-circle-overlay.png "マップに表示される円")

## <a name="local-search"></a>ローカルの検索

iOS には、ローカル検索マップ キットは、指定した地理的リージョン観光名所の検索を非同期 API が含まれています。

ローカルの検索を実行するには、アプリケーションは以下の手順を実行する必要があります。

1.  作成`MKLocalSearchRequest`オブジェクト。
1.  作成、`MKLocalSearch`オブジェクトから、`MKLocalSearchRequest`します。
1.  呼び出す、`Start`メソッドを`MKLocalSearch`オブジェクト。
1.  取得、`MKLocalSearchResponse`コールバック オブジェクト。


ローカルの検索 API 自体では、ユーザー インターフェイスはありません。 使用するマップも必要ありません。 ただし、ローカルの検索を実際に使用するために、アプリケーションが検索クエリを指定し、結果を表示する手段を提供する必要があります。 さらに、結果には、場所データが含まれているが、のでさせることが多くの場合、マップで表示することです。

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>ローカル検索 UI の追加

検索の入力をそのまま使用する 1 つの方法は、 `UISearchBar`、によって提供される、`UISearchController`をテーブルの結果が表示されます。

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
```

検索バーのオブジェクトをユーザー インターフェイスに組み込む担当していることに注意してください。 この例でのナビゲーション バーの TitleView を割り当てましたが、アプリケーションでナビゲーション コント ローラーを使用しないことを表示する別の場所を検索する必要があります。

このコード スニペットで – 別のカスタム ビュー コント ローラーを作成しました`searchResultsController`– 検索結果を表示して、検索のコント ローラー オブジェクトを作成するこのオブジェクトを使用します。 検索バーで、ユーザーが操作したときにアクティブになった新しい検索 updater も作成しました。 キーストロークごとの検索に関する通知を受信し、UI の更新を担当します。
両方を実装する方法を見てかかります、`searchResultsController`と`searchResultsUpdater`このガイドで後述します。

これは、次に示すように、マップ上に表示される検索バーが得られます。

 ![](images/07-searchbar.png "マップ上に表示される検索バー")
 


### <a name="displaying-the-search-results"></a>検索結果を表示します。

カスタム ビュー コント ローラー; の作成に必要な検索結果を表示するには通常、`UITableViewController`します。 上記のよう、`searchResultsController`のコンス トラクターに渡される、`searchController`作成されるときにします。
次のコードでは、このカスタム ビュー コント ローラーを作成する方法の例を示します。

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

`SearchResultsUpdater`間の媒介として機能、`searchController`の検索バーと検索結果。 

最初に検索メソッドを作成するがこの例では、`SearchResultsViewController`します。 作成する必要がありますこれを行う、`MKLocalSearch`オブジェクトし、それを使用して、検索を発行する、`MKLocalSearchRequest`に渡されたコールバックで、結果が取得されます、`Start`のメソッド、`MKLocalSearch`オブジェクト。 後、結果が返されます、`MKLocalSearchResponse`オブジェクトの配列を含む`MKMapItem`オブジェクト。

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

次に、`MapViewController`のカスタム実装を作成します`UISearchResultsUpdating`に割り当てられる、`SearchResultsUpdater`のプロパティ、`searchController`で、[ローカル検索 UI を追加する](#Adding_a_Local_Search_UI)セクション。

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

上記の実装注釈を追加、マップ項目を結果から、選択すると次に示すよう。

 ![](images/08-search-results.png "結果から項目が選択されているときに、マップに追加注釈")
 
> [!IMPORTANT]
> `UISearchController` iOS 8 で実装されていました。 これより前にデバイスをサポートするかどうかは、使用する必要があります`UISearchDisplayController`します。



## <a name="summary"></a>まとめ

この記事では調査、*マップ* *キット* iOS 用のフレームワークです。 方法を説明しましたが、まず、`MKMapView`クラスは、アプリケーションに含まれる対話型のマップを使用できます。 注釈とオーバーレイを使用してマップをさらにカスタマイズする方法を説明します。 最後に、ios 6.1、Map Kit に追加されたローカルの検索機能を検証を使用する方法を示す観光名所の場所ベースのクエリを実行し、マップに追加します。

## <a name="related-links"></a>関連リンク

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (サンプル)](https://developer.xamarin.com/samples/monotouch/MapDemo)
