---
title: Xamarin. iOS の注釈とオーバーレイ
description: この記事では、マップキットの注釈機能とオーバーレイ機能を使用する方法を説明するステップバイステップのチュートリアルを示します。 ここでは、Xamarin 進化2013カンファレンスの場所に注釈とオーバーレイを表示するマップをアプリケーションに追加する方法を示します。
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 404483bb0c2c405fb810ebcd3a8007692219f522
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021999"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Xamarin. iOS の注釈とオーバーレイ

このチュートリアルでビルドするアプリケーションは次のようになります。

 [![](ios-maps-walkthrough-images/00-map-overlay.png "An example MapKit app")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)

完成したコードは、マップの[チュートリアルのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)で確認できます。

まず、新しい IOS の空の**プロジェクト**を作成し、関連する名前を付けます。 まず、ビューコントローラーに MapView を表示するコードを追加し、Mapview の新しいクラスとカスタム注釈を作成します。 以下の手順でビルドします。

## <a name="viewcontroller"></a>ViewController

1. `ViewController`に次の名前空間を追加します。

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. `MapDelegate` インスタンスと共に、`MKMapView` インスタンス変数をクラスに追加します。 `MapDelegate` は、すぐに作成します。

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. コントローラーの `LoadView` メソッドで、`MKMapView` を追加し、コントローラーの `View` プロパティに設定します。

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    次に、' ViewDidLoad ' ' メソッドでマップを初期化するコードを追加します。

1. `ViewDidLoad` マップの種類を設定するコードを追加して、ユーザーの場所を表示し、ズームとパンを許可します。

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;

    ```

1. 次に、マップを中央に配置し、その領域を設定するためのコードを追加します。

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;

    ```

1. `MapDelegate` の新しいインスタンスを作成し、`MKMapView`の `Delegate` に割り当てます。 ここでも、`MapDelegate` をすぐに implcodeent します。

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;
    ```

1. IOS 8 では、ユーザーが自分の場所を使用するために承認を要求する必要があるので、このサンプルに追加してみましょう。 まず、`CLLocationManager` クラスレベルの変数を定義します。

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. `ViewDidLoad` 方法では、アプリケーションを実行しているデバイスが iOS 8 を使用しているかどうかを確認し、アプリが使用されている場合に承認を要求します。

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
        locationManager.RequestWhenInUseAuthorization ();
    }
    ```

1. 最後に、ユーザーが場所を要求する理由をユーザーに通知するために、**情報 plist**ファイルを編集する必要があります。 **情報 plist**の **[ソース]** メニューで、次のキーを追加します。

    `NSLocationWhenInUseUsageDescription`

    および文字列:

    `Maps Walkthrough Docs Sample`.

## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs –カスタム注釈のクラス

1. ここでは、`ConferenceAnnotation`という注釈にカスタムクラスを使用します。 次のクラスをプロジェクトに追加します。

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;

    namespace MapsWalkthrough
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

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController-注釈とオーバーレイの追加

1. `ConferenceAnnotation` が配置されているので、マップに追加できます。 `ViewController`の `ViewDidLoad` メソッドに戻り、マップの中心座標に注釈を追加します。

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter));
    ```

1. また、ホテルを重ねる必要もあります。 次のコードを追加して、指定されたホテルの座標を使用して `MKPolygon` を作成し、`AddOverlay`を呼び出してマップに追加します。

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

これで `ViewDidLoad`のコードが完成します。 ここで、注釈とオーバーレイビューの作成をそれぞれ処理するために、`MapDelegate` クラスを実装する必要があります。

## <a name="mapdelegate"></a>MapDelegate

1. `MKMapViewDelegate` から継承し、注釈の再利用識別子として使用する `annotationId` 変数を含む `MapDelegate` というクラスを作成します。

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```

    ここには1つの注釈しかないので、再利用コードは厳密には必要ありませんが、これを含めることをお勧めします。

1. このチュートリアルに含まれている**カンファレンスの .png**イメージを使用して `ConferenceAnnotation` のビューを返すには、`GetViewForAnnotation` メソッドを実装します。

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

1. ユーザーが注釈をタップすると、オースティンの市を示す画像が表示されます。 イメージの `MapDelegate` に次の変数を追加し、ビューを表示します。

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. 次に、注釈がタップされたときにイメージを表示するには、次のように `DidSelectAnnotation` メソッドを実装します。

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

1. ユーザーがマップ上の任意の場所をタップして注釈を選択解除したときにイメージを非表示にするには、次のように `DidSelectAnnotationView` メソッドを実装します。

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

    これで、注釈のコードが配置されました。 残りのことは、ホテルオーバーレイのビューを作成するために、`MapDelegate` にコードを追加することだけです。

1. 次の `GetViewForOverlay` の実装を `MapDelegate`に追加します。

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

アプリケーションを実行します。 カスタム注釈とオーバーレイを含む対話型マップが作成されました。 次に示すように、注釈をタップすると、オースティンのイメージが表示されます。

 [![](ios-maps-walkthrough-images/01-map-image.png "Tap on the annotation and the image of Austin is displayed")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、マップに注釈を追加する方法と、指定した多角形のオーバーレイを追加する方法について説明しました。 また、注釈にタッチサポートを追加して、マップ上でイメージをアニメーション化する方法についても説明します。

## <a name="related-links"></a>関連リンク

- [Maps チュートリアルのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)
- [マップのデモのサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
- [iOS のマップ](~/ios/user-interface/controls/ios-maps/index.md)
