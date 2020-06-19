---
title: 'Xamarin.Forms図形: ジオメトリ'
description: Xamarin.Formsgeometry クラスを使用すると、2D 図形のジオメトリを記述できます。
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c816da59dce291e6fbd354e2c6079fe24044e978
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990780"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.Forms図形: ジオメトリ

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Geometry`クラスとそれから派生するクラスを使用すると、2d 図形のジオメトリを記述できます。 `Geometry` オブジェクトは、四角形や円などの単純なものにすることも、2 つ以上のジオメトリ オブジェクトから作成された複合的なものにすることもできます。 クラスを使用すると、より複雑なジオメトリを作成でき `PathGeometry` ます。これにより、円弧と曲線を記述できます。

`Geometry`クラスと `Shape` クラスは、どちらも2d 図形を記述しますが、重要な違いがあると思われます。 クラスはクラスから派生し、クラスはクラスから派生し `Geometry` `BindableObject` `Shape` `View` ます。 そのため、 `Shape` オブジェクトは、自身をレンダリングしてレイアウトシステムに参加させることができますが、 `Geometry` オブジェクトは使用できません。 オブジェクト `Shape` はオブジェクトよりも簡単に使用 `Geometry` `Geometry` できますが、オブジェクトの方が汎用性があります。 オブジェクトを使用して2D グラフィックスをレンダリングしている間に、 `Shape` オブジェクトを使用して `Geometry` 2d グラフィックスのジオメトリック領域を定義し、領域をクリッピング用に定義できます。

すべてのジオメトリの基底クラスは、抽象クラスの `Geometry` です。 このクラスから派生するクラスは、単純なジオメトリ、パスジオメトリ、および複合ジオメトリの3つのカテゴリにグループ化できます。

## <a name="simple-geometries"></a>単純なジオメトリ

単純な geometry クラスは `EllipseGeometry` 、、、 `LineGeometry` および `RectangleGeometry` です。 これらは、円、線、四角形などの基本的なジオメトリック形状を作成するために使用されます。 `PathGeometry` を使用するか、ジオメトリ オブジェクトを結合する方法でも、これらと同じ図形や、さらに複雑な図形を作成できますが、これらのクラスを使用すると、このような基本的な幾何学図形をより簡単に生成できます。

### <a name="ellipsegeometry"></a>System.windows.media.ellipsegeometry>

楕円のジオメトリは、ジオメトリまたは楕円または円を表し、中心点、x 半径、および y 半径によって定義されます。

`EllipseGeometry`クラスは、次のプロパティを定義します。

- `Center``Point`geometry の中心点を表す型の。
- `RadiusX``double`geometry の x 半径の値を表す型の。 このプロパティの既定値は0.0 です。
- `RadiusY``double`geometry の y 半径の値を表す型の。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

次の例は、を作成して表示する方法を示してい `EllipseGeometry` ます。

```xaml
<Path Fill="Blue"
      Stroke="Red"
      StrokeThickness="1">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

この例では、の中央は `EllipseGeometry` (50, 50) に設定され、x 半径と y 半径は両方とも50に設定されています。 これにより、直径が100である円が作成されます。この円の内部は青で描画されます。

### <a name="linegeometry"></a>LineGeometry

線のジオメトリは線のジオメトリを表し、直線の始点と終点を指定することによって定義されます。

`LimeGeometry`クラスは、次のプロパティを定義します。

- `StartPoint``Point`行の開始点を表す型の。
- `EndPoint``Point`線の終点を表す、型の。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

次の例は、を作成して表示する方法を示してい `LineGeometry` ます。

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

この例では、 `LineGeometry` (10, 20) から (100130) にを描画します。

### <a name="rectanglegeometry"></a>RectangleGeometry

四角形のジオメトリは四角形を表し、 `Rect` その相対位置と高さおよび幅を指定する構造体で定義されます。

`RectangleGeometry`クラスは、次のプロパティを定義します。

- `Rect``FormsRect`四角形の大きさを表す型の。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

次の例は、を作成して表示する方法を示してい `RectangleGeometry` ます。

```xaml
<Path Fill="Blue"
      Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <RectangleGeometry Rect="50,50,25,25" />
  </Path.Data>
</Path>
```

四角形の位置と大きさは、構造体によって定義され `Rect` ます。 この例では、位置は (50, 50) で、高さと幅は両方とも25で、正方形が作成されます。

## <a name="path-geometries"></a>パスジオメトリ

パスジオメトリは、円弧、曲線、楕円、線、および四角形で構成できる複雑な図形を表します。

`PathGeometry`クラスは、次のプロパティを定義します。

- `Figures`型の `PathFigureCollection` `PathFigure` 。パスの内容を記述するオブジェクトのコレクションを表します。
- `FillRule`型の。 `FillRule` これは、ジオメトリに含まれる交差する領域を結合する方法を決定します。 このプロパティの既定値は `FillRule.EvenOdd` です。

> [!NOTE]
> `Figures`プロパティは `ContentProperty` クラスのである `PathGeometry` ため、XAML から明示的に設定する必要はありません。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

は、 `PathGeometry` オブジェクトのコレクションで構成され `PathFigure` ます。各オブジェクトは、 `PathFigure` ジオメトリ内の図形を記述します。 それぞれ `PathFigure` が1つ以上のオブジェクトで構成され `PathSegment` ており、それぞれが図形のセグメントを表します。 セグメントには、次のようなさまざまな種類があります。

- `ArcSegment`。2つの点の間に楕円の円弧を作成します。
- `BezierSegment`。2つの点の間に3次ベジエ曲線を作成します。
- `LineSegment`。2つの点の間に線を作成します。
- `PolyBezierSegment`。一連の3次ベジエ曲線を作成します。
- `PolyLineSegment`。一連の線を作成します。
- `PolyQuadraticBezierSegment`。一連の2次ベジエ曲線を作成します。
- `QuadraticBezierSegment`。2次ベジエ曲線を作成します。

`PathFigure` 内のセグメントは 1 つの幾何学図形に結合されて、各セグメントの終点が次のセグメントの始点になります。 `PathFigure` の `StartPoint` プロパティでは、最初のセグメントが描画される開始点を指定します。 後続の各セグメントは、前のセグメントの終点から始まります。 たとえば、からへの垂直線を定義するには、 `10,50` `10,150` プロパティを `StartPoint` に設定し、プロパティ設定を使用して `10,50` を作成し `LineSegment` `Point` `10,150` ます。

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,50">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="10,150" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

オブジェクトの組み合わせを使用して、 `PathSegment` または内の複数のオブジェクトを使用することにより、より複雑なジオメトリを作成でき `PathFigure` `PathGeometry` ます。

## <a name="composite-geometries"></a>複合ジオメトリ

複合 geometry オブジェクトは、を使用して作成でき `GeometryGroup` ます。 クラスは、 `GeometryGroup` 1 つまたは複数のオブジェクトから複合ジオメトリを作成し `Geometry` ます。 任意の数の `Geometry` オブジェクトを `GeometryGroup` に追加できます。

次の例は、のジオメトリを結合する方法を示してい `GeometryGroup` ます。

```xaml
<Path Stroke="Green"
      StrokeThickness="2"
      Fill="Orange">
    <Path.Data>
        <GeometryGroup>
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,250" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,250" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

この例では、 `EllipseGeometry` x 半径が同じで、中心座標が異なる4つのオブジェクトが結合されています。 これにより、内側に塗りつぶしルールがオレンジ色で塗りつぶされた4つの重なり合う円が作成され `EvenOdd` ます。

## <a name="other-features"></a>その他の機能

クラスには `GeometryHelper` 、次のヘルパーメソッドが用意されています。

- `FlattenGeometry`
- `FlattenCubicBezier`
- `FlattenQuadraticBezier`
- `FlattenArc`

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
