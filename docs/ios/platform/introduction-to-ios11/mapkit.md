---
title: IOS 11 で MapKit の新機能
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 3b1ac8015a86292f00f26133277ead2f211dca33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="new-features-in-mapkit-on-ios-11"></a>IOS 11 で MapKit の新機能

iOS 11 では、MapKit に次の新機能を追加します。

- [クラスタ リング注釈](#clustering)
- [コンパス ボタン](#compass)
- [スケールの表示](#scale)
- [ユーザーの追跡 ボタン](#user-tracking)

![クラスター化されたマーカーの表示をマップし、コンパス ボタン](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>拡大表示中に自動的にグループ化マーカー

サンプル[MapKit サンプル"Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)新しい iOS 11 注釈がクラスタ リング機能を実装する方法を示します。

### <a name="1-create-an-mkpointannotation-subclass"></a>1.作成、`MKPointAnnotation`サブクラス

ポイント注釈クラスでは、マップ上の各マーカーを表します。 使用して個別に追加できる`MapView.AddAnnotation()`または使用して、配列から`MapView.AddAnnotations()`です。

ポイント注釈クラスには、視覚的に表現はありません、マーカーに関連付けられているデータを表すためだけに必要な (最も重要な`Coordinate`プロパティは、緯度と経度、マップに)、および任意のカスタム プロパティ。

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2.作成、 `MKMarkerAnnotationView` 1 つのマーカーに対してサブクラス

マーカー注釈ビューをそれぞれの注釈のビジュアル表現してなどのプロパティを使用してのスタイルします。

- **MarkerTintColor** – マーカーの色。
- **GlyphText** – マーカーにテキストが表示されます。
- **GlyphImage** – マーカーに表示されるイメージを設定します。
- **DisplayPriority** – z オーダー (スタックの動作) を決定が、マップのマーカーが多くなりすぎる場合です。 いずれかを使用して`Required`、 `DefaultHigh`、または`DefaultLow`です。

自動のクラスタ リングをサポートするには、必要がありますも設定します。

- **ClusteringIdentifier** – これは、どのマーカー取得一緒にクラスター化を制御します。 同じ識別子を使用して、すべてのマーカーのまたは異なる識別子を使用してグループ化されている方法を制御できます。

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

マーカーのクラスターを表す注釈の表示中に_でした_単純なイメージをするには、ユーザーが期待をアプリにどのくらいマーカーが一緒にグループ化に関する視覚的な手掛かりを提供します。

[のサンプル コード](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)CoreGraphics を各マーカーの種類の割合の円グラフ形式と同様に、クラスター内のマーカーの数を表示するために使用します。

設定もする必要があります。

- **DisplayPriority** – z オーダー (スタックの動作) を決定が、マップのマーカーが多くなりすぎる場合です。 いずれかを使用して`Required`、 `DefaultHigh`、または`DefaultLow`です。
- **CollisionMode** –`Circle`または`Rectangle`です。

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

マップ ビュー コントロールが作成されると、マップは、サインアウト拡大されると、自動のクラスタ リングの動作を有効にする注釈の表示の種類を登録ビューに追加する場合。

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5.マップを表示します。

マップがレンダリングされると、コメント マーカーがクラスター化またはズーム レベルによってレンダリングされます。 ズーム レベル変更では、クラスターとの間にマーカーがアニメーション化します。

![シミュレーターがマップ上のクラスター化されたマーカーの表示](mapkit-images/cyclemap-sml.png)

参照してください、[セクションをマップ](~/ios/user-interface/controls/ios-maps/index.md)MapKit を使用してデータを表示する方法の詳細。

<a name="compass" />

## <a name="compass-button"></a>コンパス ボタン

iOS 11 では、コンパスをマップからのポップして別の場所へのレンダリング、表示する機能を追加します。 参照してください、 [Tandm サンプル アプリ](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)例についてはします。

(を含むライブ アニメーション マップの向きを変更すると)、コンパスのようなボタンを作成し、別のコントロールに表示します。

![ナビゲーション バーでコンパス ボタン](mapkit-images/compass-sml.png)

次のコードでは、コンパス ボタンを作成し、ナビゲーション バーで表示します。

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass`既定コンパス マップ ビュー内の可視性を制御するプロパティを使用できます。

<a name="scale" />

## <a name="scale-view"></a>スケールの表示

ビューを使用して、他の場所で、スケールを追加、`MKScaleView.FromMapView()`ビュー階層内で他の場所を追加するスケール ビューのインスタンスを取得します。

![マップにオーバーレイされるスケール ビュー](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale`既定コンパス マップ ビュー内の可視性を制御するプロパティを使用できます。

<a name="user-tracking" />

## <a name="user-tracking-button"></a>ユーザーの追跡 ボタン

ユーザー追跡ボタン センターは、ユーザーの現在の場所にマップします。 使用して、`MKUserTrackingButton.FromMapView()`メソッド ボタンのインスタンスを取得し、書式設定の変更を適用し、ビュー階層内で他の場所を追加します。

![マップにオーバーレイされるユーザーの場所 ボタン](mapkit-images/user-location-sml.png)

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
- [新で MapKit (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/237/)
