---
title: IOS 11 の MapKit の新機能
description: このドキュメントでは、iOS 11 の新しい MapKit 機能について説明します。グループ化マーカー、コンパスボタン、スケールビュー、およびユーザー追跡ボタンです。
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 02bd25c4b4e251536dfdabdef109eb659fe3be37
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032153"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>IOS 11 の MapKit の新機能

iOS 11 では、MapKit に次の新機能が追加されています。

- [注釈のクラスタリング](#clustering)
- [コンパスボタン](#compass)
- [スケールビュー](#scale)
- [[ユーザーの追跡] ボタン](#user-tracking)

![クラスター化マーカーとコンパスボタンを示すマップ](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>ズーム中にマーカーを自動的にグループ化する

サンプル[Mapkit サンプル "Tandm"](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)は、新しい iOS 11 注釈クラスタリング機能を実装する方法を示しています。

### <a name="1-create-an-mkpointannotation-subclass"></a>1. `MKPointAnnotation` サブクラスを作成する

Point 注釈クラスは、マップ上の各マーカーを表します。 これらは、`MapView.AddAnnotation()` を使用して個別に追加することも、`MapView.AddAnnotations()`を使用して配列から追加することもできます。

ポイント注釈クラスには視覚的な表現がありません。マーカーに関連付けられているデータ (最も重要なもの、マップの緯度と経度である `Coordinate` プロパティ)、およびカスタムプロパティを表すためにのみ必要です。

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. 1 つのマーカーに対して `MKMarkerAnnotationView` サブクラスを作成する

マーカー注釈ビューは、各注釈を視覚的に表現したものであり、次のようなプロパティを使用してスタイルが付けられています。

- **MarkerTintColor** –マーカーの色です。
- **GlyphText** –マーカーに表示されるテキストです。
- **GlyphImage** –マーカーに表示されるイメージを設定します。
- **DisplayPriority** –マップにマーカーがある場合の z オーダー (スタック動作) を決定します。 `Required`、`DefaultHigh`、`DefaultLow`のいずれかを使用します。

自動クラスタリングをサポートするには、次の設定も行う必要があります。

- **Clusteringidentifier** –クラスター化するマーカーを制御します。 すべてのマーカーに同じ識別子を使用することも、異なる識別子を使用してグループ化する方法を制御することもできます。

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

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. マーカーのクラスターを表す `MKAnnotationView` を作成する

マーカーのクラスターを表す注釈ビューは単純なイメージ_である場合_がありますが、ユーザーは、グループ化されたマーカーの数について視覚的な手掛かりを提供するようアプリに要求します。

この[サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)では、coregraphics を使用して、クラスター内のマーカーの数と、各マーカーの種類の比率の円グラフ表現を表示します。

次の設定も行う必要があります。

- **DisplayPriority** –マップにマーカーがある場合の z オーダー (スタック動作) を決定します。 `Required`、`DefaultHigh`、`DefaultLow`のいずれかを使用します。
- **CollisionMode** – `Circle` または `Rectangle`します。

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

### <a name="4-register-the-view-classes"></a>4. ビュークラスを登録する

マップビューコントロールを作成してビューに追加するときに、マップが拡大または縮小されたときの自動クラスタリング動作を有効にするために、注釈ビューの種類を登録します。

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. マップを表示します。

マップがレンダリングされると、注釈マーカーは、ズームレベルに応じてクラスター化または表示されます。 ズームレベルが変化すると、マーカーはクラスターとの間でアニメーション化されます。

![マップ上のクラスター化マーカーを示すシミュレーター](mapkit-images/cyclemap-sml.png)

MapKit を使用してデータを表示する方法の詳細については、 [「マップ」セクション](~/ios/user-interface/controls/ios-maps/index.md)を参照してください。

<a name="compass" />

## <a name="compass-button"></a>コンパスボタン

iOS 11 では、マップからコンパスをポップして、ビュー内の他の場所にレンダリングする機能が追加されています。 例については、 [Tandm サンプルアプリ](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)を参照してください。

コンパス (マップの向きが変更されたときのライブアニメーションを含む) のようなボタンを作成し、別のコントロールでレンダリングします。

![ナビゲーションバーのコンパスボタン](mapkit-images/compass-sml.png)

次のコードでは、コンパスボタンを作成し、ナビゲーションバーにレンダリングします。

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` プロパティは、マップビュー内の既定のコンパスの表示を制御するために使用できます。

<a name="scale" />

## <a name="scale-view"></a>スケールビュー

`MKScaleView.FromMapView()` メソッドを使用してビュー内の他の場所にスケールを追加し、ビュー階層内の他の場所にスケールビューのインスタンスを追加します。

![マップに重ねて表示されるスケールビュー](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` プロパティは、マップビュー内の既定のコンパスの表示を制御するために使用できます。

<a name="user-tracking" />

## <a name="user-tracking-button"></a>[ユーザーの追跡] ボタン

[ユーザーの追跡] ボタンをクリックすると、ユーザーの現在の場所にマップが配置されます。 `MKUserTrackingButton.FromMapView()` メソッドを使用して、ボタンのインスタンスを取得し、書式設定の変更を適用して、ビュー階層内の他の場所に追加します。

![マップに重ねて配置される [ユーザーの場所] ボタン](mapkit-images/user-location-sml.png)

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

- [MapKit サンプル ' Tandm '](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [MapKit (WWDC) の新機能 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/237/)
