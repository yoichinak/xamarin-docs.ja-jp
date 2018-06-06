---
title: 注釈と Xamarin.iOS のオーバーレイ
description: この記事では、マップのキットの注釈とオーバーレイ機能を使用する方法を示す詳細なチュートリアルを表示します。 Xamarin の進化 2013 カンファレンスの場所にある注釈とオーバーレイを表示するアプリケーションにマップを追加する方法を示します。
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7d224f034afc9b841bbf82b2b15b92db2d7820c7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789581"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>注釈と Xamarin.iOS のオーバーレイ

このチュートリアルで構築するアプリケーションは、次に示します。

 [![](ios-maps-walkthrough-images/00-map-overlay.png "MapKit のサンプル アプリ")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
完全なコードを見つけることができます、 **MapsWalkthroughComplete**内のフォルダー、[マップ デモ サンプル](https://developer.xamarin.com/samples/monotouch/MapDemo/)です。

新しいを作成してみましょう**iOS 空のプロジェクト**、関連する名前を付けます。 MapView を表示する、ビューのコント ローラーにコードを追加することで開始し、MapDelegate とカスタムの注釈の新しいクラスを作成し、します。 以下の手順でビルドします。

## <a name="viewcontroller"></a>ViewController


1. 次の名前空間を追加、 `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. 追加、`MKMapView`と共に、クラスに変数をインスタンス化、`MapDelegate`インスタンス。 ここで作成、`MapDelegate`間もなく。

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. コント ローラーで`LoadView`メソッドを追加、`MKMapView`に設定し、`View`コント ローラーのプロパティ。

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    次に、追加のマップを初期化するコードを 'ViewDidLoad' メソッドです。

1. `ViewDidLoad`コードを追加して、マップの種類を設定し、ユーザーの場所を表示して、ズームとパンを許可します。

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. 次に、マップの中心および it の領域を設定するコードを追加します。

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. 新しいインスタンスを作成する`MapDelegate`に割り当てると、`Delegate`の`MKMapView`です。 Implcodeent しましょう、もう一度、`MapDelegate`間もなく。

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. 8、iOS の時点では、場所を使用して、ではこれをサンプルを追加するユーザーから承認が要求する必要があります。 最初に、定義、`CLLocationManager`クラス レベルの変数。

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. `ViewDidLoad`かどうかアプリケーションを実行するデバイスで iOS 8 を使用し、アプリを使用するときに、承認を要求するおである場合を確認したいメソッド。

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. 最後に、編集する必要があります、 **Info.plist**ファイルの場所を要求するためのユーザーに通知します。 **ソース**のメニューを開き、 **Info.plist**、次のキーを追加します。
    
    `NSLocationWhenInUseUsageDescription` 
    
    文字列: 

    `Maps Demo`。


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – カスタム注釈のクラス


1. 注釈と呼ばれるカスタム クラスを使用しようとして`ConferenceAnnotation`です。 次のクラスをプロジェクトに追加します。

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - 注釈とオーバーレイを追加します。

1. `ConferenceAnnotation`インプレースおことができますが、マップに追加します。 戻り、`ViewDidLoad`のメソッド、`ViewController`マップの中心の座標で注釈を追加します。

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. ホテルのオーバーレイが必要なもします。 次のコード作成を追加、 `MKPolygon` 、ホテルの座標を使用して、呼び出しによって、マップに追加`AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
これで完了、コードで`ViewDidLoad`です。 実装する必要がありますので、`MapDelegate`注釈の作成を処理し、それぞれのビューをオーバーレイするクラス。


## <a name="mapdelegate"></a>MapDelegate

1. 呼ばれるクラスを作成する`MapDelegate`から継承する`MKMapViewDelegate`を含めると、`annotationId`注釈の再利用する識別子として使用する変数。

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    おのみがある 1 つの注釈は、ここため再利用するコードが必ずしも必要はありませんを含めることをお勧めします。

1. 実装、`GetViewForAnnotation`のビューを返すメソッドを`ConferenceAnnotation`を使用して、 **conference.png**このチュートリアルに含まれているイメージ。

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. ユーザーはタップのコメント、したときにオースティンの市区町村が表示された画像を表示します。 次の変数を追加して、`MapDelegate`イメージと、ビューを表示します。

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. 次に、注釈がタップされたときに、画像を表示、実装、`DidSelectAnnotation`メソッドを次のようにします。

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. ユーザー マップ上の別の場所をタップしてコメントを解除するときに、イメージを非表示には、実装、`DidSelectAnnotationView`次のようにメソッド。

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    場所にコメントのコードがあるようになりました。 コードを追加するがあと、`MapDelegate`ホテル オーバーレイのビューを作成します。

1. 次の実装を追加`GetViewForOverlay`を`MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

アプリケーションを実行します。 カスタム注釈とオーバーレイ対話型のマップがあるようになりました。 注釈をタップし、次のように、オースティンのイメージが表示されます。

 [![](ios-maps-walkthrough-images/01-map-image.png "注釈をタップし、オースティンのイメージが表示されます。")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、マップに注釈を追加する方法と、指定した多角形のオーバーレイを追加する方法を説明しました。 マップ上の画像をアニメーション化する注釈をタッチのサポートを追加する方法も示しました。


## <a name="related-links"></a>関連リンク

- [マップのデモ (サンプル)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS のマップ](~/ios/user-interface/controls/ios-maps/index.md)
