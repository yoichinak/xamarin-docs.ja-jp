---
title: IOS 11 での MapKit の新機能
description: このドキュメントは、iOS 11 での MapKit の新機能を説明します。 マーカー、コンパスのボタン、スケール ビュー、およびユーザーの追跡 ボタンをグループ化します。
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: c060a7bbc8d5968aeaca5f84743cdf22513dfbec
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350587"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>IOS 11 での MapKit の新機能

iOS 11 では、MapKit に次の新機能が追加されます。

- [注釈のクラスタ リング](#clustering)
- [コンパス ボタン](#compass)
- [スケール ビュー](#scale)
- [ユーザーの追跡 ボタン](#user-tracking)

![クラスター化されたマーカーを示すマップし、コンパス ボタン](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>ズーム中に自動的にグループ化マーカー

サンプル[MapKit サンプル"Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)新しい iOS 11 注釈がクラスタ リング機能を実装する方法を示しています。

### <a name="1-create-an-mkpointannotation-subclass"></a>1.作成、`MKPointAnnotation`サブクラス

Point 注釈クラスは、マップ上の各マーカーを表します。 使用して個別に追加できる`MapView.AddAnnotation()`またはを使用して、配列から`MapView.AddAnnotations()`します。

ポイント注釈クラスには、視覚的に表現はありません、マーカーに関連付けられているデータを表すためだけに必要な (最も重要なこととして、`Coordinate`緯度と経度、マップ上であるプロパティ)、およびカスタム プロパティ。

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2.作成、`MKMarkerAnnotationView`サブクラスの 1 つのマーカー

マーカー注釈ビューをそれぞれの注釈の視覚的表現してなどのプロパティを使用してスタイルします。

- **MarkerTintColor** : マーカーの色。
- **GlyphText** – テキスト マーカーに表示されます。
- **GlyphImage** – マーカーに表示されるイメージを設定します。
- **DisplayPriority** – z オーダー (スタックの動作) を決定しますが、マップのマーカーが多くなりすぎる場合。 いずれかを使用して、 `Required`、 `DefaultHigh`、または`DefaultLow`します。

自動のクラスタ リングをサポートするには、必要がありますも設定します。

- **ClusteringIdentifier** – これは、マーカーまとめてクラスター化を制御します。 同じ識別子を使用して、すべてのマーカーのまたは異なる識別子を使用して、グループ化する方法を制御できます。

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3.作成、`MKAnnotationView`を表すマーカーのクラスター

マーカーのクラスターを表す注釈ビューの中に_でした_ユーザーが期待する単純なイメージには、マーカーの数がまとめてグループ化に関する視覚的な手掛かりを提供するアプリです。

[サンプル コード](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)CoreGraphics を各マーカーの種類の割合の円グラフ表現と同様に、クラスター内のマーカーの数を表示するために使用します。

設定もする必要があります。

- **DisplayPriority** – z オーダー (スタックの動作) を決定しますが、マップのマーカーが多くなりすぎる場合。 いずれかを使用して、 `Required`、 `DefaultHigh`、または`DefaultLow`します。
- **CollisionMode** –`Circle`または`Rectangle`します。

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4.ビュー クラスを登録します。

マップのビュー コントロールが作成されると、自動のクラスタ リングの動作を有効にするように、マップを縮小拡大表示する注釈ビューの種類を登録するビューに追加する。

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5.マップを表示します。

マップがレンダリングされると、注釈マーカーをクラスター化か、ズーム レベルによってレンダリングされます。 ズーム レベルを変更すると、クラスターとの間のマーカーがアニメーション化します。

![シミュレーターがマップ上のクラスター化されたマーカーの表示](mapkit-images/cyclemap-sml.png)

参照してください、[マップ セクション](~/ios/user-interface/controls/ios-maps/index.md)MapKit でデータの表示の詳細についてはします。

<a name="compass" />

## <a name="compass-button"></a>コンパス ボタン

iOS 11 では、マップから compass をポップし、別の場所を表示にする機能を追加します。 参照してください、 [Tandm サンプル アプリ](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)例についてはします。

(を含むライブ アニメーション マップの向きが変更されたとき)、コンパスのようなボタンを作成し、それを別のコントロールをレンダリングします。

![ナビゲーション バーのコンパスのボタン](mapkit-images/compass-sml.png)

次のコードでは、コンパスのボタンを作成し、ナビゲーション バーで表示します。

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass`マップ ビュー内で既定のコンパスの可視性を制御するプロパティを使用できます。

<a name="scale" />

## <a name="scale-view"></a>スケール ビュー

ビューを使用して、他の場所で、スケールを追加、`MKScaleView.FromMapView()`ビュー階層に別の場所を追加するスケール ビューのインスタンスを取得します。

![スケール ビューのマップにオーバーレイ](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale`マップ ビュー内で既定のコンパスの可視性を制御するプロパティを使用できます。

<a name="user-tracking" />

## <a name="user-tracking-button"></a>ユーザーの追跡 ボタン

ユーザーの追跡 ボタンは、ユーザーの現在の場所にマップを中央揃えします。 使用して、`MKUserTrackingButton.FromMapView()`メソッドは、ボタンのインスタンスを取得し、書式設定の変更を適用し、ビュー階層に別の場所を追加します。

![ユーザーの場所 ボタンをマップにオーバーレイ](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>関連リンク

- [MapKit サンプル 'Tandm'](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [新機能で MapKit (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/237/)
