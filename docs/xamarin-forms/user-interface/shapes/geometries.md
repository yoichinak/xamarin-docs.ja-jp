---
title: 'Xamarin.Forms図形: ジオメトリ'
description: Xamarin.Formsgeometry クラスを使用すると、2D 図形のジオメトリを記述できます。
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: be0c12231ff6106e07c935a111195df779698172
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934278"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.Forms図形: ジオメトリ

![プレリリース API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Geometry`クラスとそれから派生するクラスを使用すると、2d 図形のジオメトリを記述できます。 `Geometry` オブジェクトは、四角形や円などの単純なものにすることも、2 つ以上のジオメトリ オブジェクトから作成された複合的なものにすることもできます。 さらに、円弧や曲線を含むより複雑なジオメトリを作成することもできます。

クラスは、 `Geometry` さまざまなカテゴリのジオメトリを定義するいくつかのクラスの親クラスです。

- `EllipseGeometry`。楕円または円のジオメトリを表します。
- `GeometryGroup`。複数の geometry オブジェクトを1つのオブジェクトに結合できるコンテナーを表します。
- `LineGeometry`。線のジオメトリを表します。
- `PathGeometry`。円弧、曲線、楕円、線、および四角形で構成できる複雑な図形のジオメトリを表します。
- `RectangleGeometry`。四角形または正方形のジオメトリを表します。

`Geometry`クラスと `Shape` クラスは、どちらも2d 図形を記述しますが、重要な違いがあると思われます。 クラスはクラスから派生し、クラスはクラスから派生し `Geometry` [`BindableObject`](xref:Xamarin.Forms.BindableObject) `Shape` [`View`](xref:Xamarin.Forms.View) ます。 そのため、 `Shape` オブジェクトは、自身をレンダリングしてレイアウトシステムに参加させることができますが、 `Geometry` オブジェクトは使用できません。 オブジェクト `Shape` はオブジェクトよりも簡単に使用 `Geometry` `Geometry` できますが、オブジェクトの方が汎用性があります。 オブジェクトを使用して2D グラフィックスをレンダリングしている間に、 `Shape` オブジェクトを使用して `Geometry` 2d グラフィックスのジオメトリック領域を定義し、領域をクリッピング用に定義できます。

次のクラスには、オブジェクトに設定できるプロパティがあり `Geometry` ます。

- `Path`クラスは、を使用して `Geometry` その内容を記述します。 を表示するには、 `Geometry` `Path.Data` プロパティをオブジェクトに設定し、 `Geometry` `Path` オブジェクトの `Fill` プロパティとプロパティを設定し `Stroke` ます。
- [`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスには、 `Clip` `Geometry` 要素の内容のアウトラインを定義する型のプロパティがあります。 `Clip`プロパティがオブジェクトに設定されている場合は、 `Geometry` の領域内にある領域だけが `Geometry` 表示されます。 詳細については、「 [Geometry を使用したクリップ](#clip-with-a-geometry)」を参照してください。

クラスから派生するクラスは、 `Geometry` 単純なジオメトリ、パスジオメトリ、および複合ジオメトリという3つのカテゴリに分類できます。

## <a name="simple-geometries"></a>単純なジオメトリ

単純な geometry クラスは `EllipseGeometry` 、、、 `LineGeometry` および `RectangleGeometry` です。 これらは、円、線、四角形などの基本的なジオメトリック形状を作成するために使用されます。 これらの同じ図形とより複雑な図形は、を使用して、 `PathGeometry` または geometry オブジェクトを組み合わせることによって作成できますが、これらのクラスは、これらの基本的なジオメトリック形状を生成するためのより簡単な方法を提供します。

### <a name="ellipsegeometry"></a>System.windows.media.ellipsegeometry>

楕円のジオメトリは、ジオメトリまたは楕円または円を表し、中心点、x 半径、および y 半径によって定義されます。

`EllipseGeometry` クラスでは、次のプロパティが定義されます。

- `Center`[`Point`](xref:Xamarin.Forms.Point)geometry の中心点を表す型の。
- `RadiusX``double`geometry の x 半径の値を表す型の。 このプロパティの既定値は0.0 です。
- `RadiusY``double`geometry の y 半径の値を表す型の。 このプロパティの既定値は0.0 です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

次の例は、オブジェクトでを作成して表示する方法を示してい `EllipseGeometry` `Path` ます。

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

この例では、の中央は `EllipseGeometry` (50, 50) に設定され、x 半径と y 半径は両方とも50に設定されています。 これにより、100のデバイスに依存しない単位の赤い円が作成され、内部が青で描画されます。

![System.windows.media.ellipsegeometry>](geometry-images/ellipse.png "System.windows.media.ellipsegeometry>")

### <a name="linegeometry"></a>LineGeometry

線のジオメトリは線のジオメトリを表し、直線の始点と終点を指定することによって定義されます。

`LineGeometry` クラスでは、次のプロパティが定義されます。

- `StartPoint`[`Point`](xref:Xamarin.Forms.Point)行の開始点を表す型の。
- `EndPoint`[`Point`](xref:Xamarin.Forms.Point)線の終点を表す、型の。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

次の例は、オブジェクトでを作成して表示する方法を示してい `LineGeometry` `Path` ます。

```xaml
<Path Stroke="Black">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

この例では、を `LineGeometry` (10, 20) から (100130) に描画します。

![LineGeometry](geometry-images/line.png "LineGeometry")

> [!NOTE]
> を `Fill` 表示するのプロパティを設定し `Path` ても `LineGeometry` 効果はありません。これは、行に内部が存在しないためです。

### <a name="rectanglegeometry"></a>RectangleGeometry

四角形のジオメトリは、四角形または正方形のジオメトリを表し、 `Rect` 相対位置と高さおよび幅を指定する構造体で定義されます。

クラスは、 `RectangleGeometry` `Rect` 四角形の大きさを表す型のプロパティを定義し [`Rectangle`](xref:Xamarin.Forms.Rectangle) ます。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

次の例は、オブジェクトでを作成して表示する方法を示してい `RectangleGeometry` `Path` ます。

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <RectangleGeometry Rect="10,10,150,100" />
  </Path.Data>
</Path>
```

四角形の位置と大きさは、構造体によって定義され `Rect` ます。 この例では、位置は (10, 10)、幅は150、高さは100デバイスに依存しない単位です。

![RectangleGeometry](geometry-images/rectangle.png "RectangleGeometry")

## <a name="path-geometries"></a>パスジオメトリ

パスジオメトリは、円弧、曲線、楕円、線、および四角形で構成できる複雑な図形を表します。

`PathGeometry` クラスでは、次のプロパティが定義されます。

- `Figures`型の `PathFigureCollection` `PathFigure` 。パスの内容を記述するオブジェクトのコレクションを表します。
- `FillRule`型の。 `FillRule` これは、ジオメトリに含まれる交差する領域を結合する方法を決定します。 このプロパティの既定値は `FillRule.EvenOdd` です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

列挙体の詳細については `FillRule` 、「 [ Xamarin.Forms Shapes: Fill rules](fillrules.md)」を参照してください。

> [!NOTE]
> `Figures`プロパティは `ContentProperty` クラスのである `PathGeometry` ため、XAML から明示的に設定する必要はありません。

は、 `PathGeometry` オブジェクトのコレクションで構成され `PathFigure` ます。各オブジェクトは、 `PathFigure` ジオメトリ内の図形を記述します。 それぞれ `PathFigure` が1つ以上のオブジェクトで構成され `PathSegment` ており、それぞれが図形のセグメントを表します。 セグメントには、次のようなさまざまな種類があります。

- `ArcSegment`。2つの点の間に楕円の円弧を作成します。
- `BezierSegment`。2つの点の間に3次ベジエ曲線を作成します。
- `LineSegment`。2つの点の間に線を作成します。
- `PolyBezierSegment`。一連の3次ベジエ曲線を作成します。
- `PolyLineSegment`。一連の線を作成します。
- `PolyQuadraticBezierSegment`。一連の2次ベジエ曲線を作成します。
- `QuadraticBezierSegment`。2次ベジエ曲線を作成します。

上記のすべてのクラスは、抽象 `PathSegment` クラスから派生します。

`PathFigure` 内のセグメントは 1 つの幾何学図形に結合されて、各セグメントの終点が次のセグメントの始点になります。 `PathFigure` の `StartPoint` プロパティでは、最初のセグメントが描画される開始点を指定します。 後続の各セグメントは、前のセグメントの終点から始まります。 たとえば、からへの垂直線を定義するには、 `10,50` `10,150` プロパティを `StartPoint` に設定し、プロパティ設定を使用して `10,50` を作成し `LineSegment` `Point` `10,150` ます。

```xaml
<Path Stroke="Black">
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

### <a name="create-an-arcsegment"></a>ArcSegment を作成する

は、 `ArcSegment` 2 つの点の間に楕円の円弧を作成します。 楕円の円弧は、始点と終点、x と y 半径、x 軸の回転率、円弧が180度より大きいかどうかを示す値、および円弧が描画される方向を示す値によって定義されます。

`ArcSegment` クラスでは、次のプロパティが定義されます。

- `Point`[`Point`](xref:Xamarin.Forms.Point)楕円の円弧のエンドポイントを表す型の。このプロパティの既定値は (0, 0) です。
- `Size`[`Size`](xref:Xamarin.Forms.Size)円弧の x 半径と y 半径を表す型の。このプロパティの既定値は (0, 0) です。
- `RotationAngle`型の。 `double` これは、楕円が x 軸を中心とする角度 (度単位) を表します。 このプロパティの既定値は 0 です。
- `SweepDirection``SweepDirection`円弧が描画される方向を指定する型の。 このプロパティの既定値は `SweepDirection.CounterClockwise` です。
- `IsLargeArc`型の `bool` 。これは、弧が180°を超えるかどうかを示します。 このプロパティの既定値は `false` です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスに、 `ArcSegment` 円弧の開始点のプロパティが含まれていません。これは、それが表す円弧の終点だけを定義します。 円弧の始点は、の追加先となるの現在の点です `PathFigure` `ArcSegment` 。

`SweepDirection` 列挙体を使って、次のメンバーを定義できます。

- `CounterClockwise`。円弧が時計回りの方向に描画されることを指定します。
- `Clockwise`。円弧が反時計回りの方向に描画されることを指定します。

次の例は、オブジェクトでを作成して表示する方法を示してい `ArcSegment` `Path` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <ArcSegment Size="100,50"
                                            RotationAngle="45"
                                            IsLargeArc="True"
                                            SweepDirection="CounterClockwise"
                                            Point="200,100" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、楕円弧は (10100) から (200100) に描画されます。

### <a name="create-a-beziersegment"></a>System.windows.media.beziersegment> を作成する

は、 `BezierSegment` 2 つの点の間に3次ベジエ曲線を作成します。 3次ベジエ曲線は、始点、終点、および2つの制御点によって定義されます。

`BezierSegment` クラスでは、次のプロパティが定義されます。

- `Point1`[`Point`](xref:Xamarin.Forms.Point)曲線の最初の制御点を表す型の。 このプロパティの既定値は (0, 0) です。
- `Point2`[`Point`](xref:Xamarin.Forms.Point)曲線の2番目の制御点を表す型の。 このプロパティの既定値は (0, 0) です。
- `Point3`[`Point`](xref:Xamarin.Forms.Point)曲線の終点を表す型の。 このプロパティの既定値は (0, 0) です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスに、 `BezierSegment` 曲線の開始点のプロパティが含まれていません。 曲線の始点は、の追加先となるの現在の点です `PathFigure` `BezierSegment` 。

3次ベジエ曲線の2つの制御点は、マグネットのように動作し、それ以外の場合は、それ自体が直線になるような部分を獲得し、曲線を生成します。 最初の制御点は曲線の開始部分に影響します。 2つ目の制御点は曲線の終了部分に影響します。 曲線は、必ずしも制御点のいずれかを通過しません。 代わりに、各コントロールポイントは、行のその部分をそれ自体に移動しません。

次の例は、オブジェクトでを作成して表示する方法を示してい `BezierSegment` `Path` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <BezierSegment Point1="100,0"
                                               Point2="200,200"
                                               Point3="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、3次ベジエ曲線を (10, 10) から (300, 10) に描画します。 曲線には、(100, 0) と (200200) の2つの制御点があります。

![System.windows.media.beziersegment>](geometry-images/beziersegment.png "System.windows.media.beziersegment>")

### <a name="create-a-linesegment"></a>LineSegment を作成する

は、 `LineSegment` 2 つの点の間に線を作成します。

クラスは、 `LineSegment` `Point` 直線セグメントの終点を表す型のプロパティを定義し [`Point`](xref:Xamarin.Forms.Point) ます。 このプロパティの既定値は (0, 0) で、オブジェクトによってサポートされます。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスには、 `LineSegment` 行の開始点のプロパティが含まれていません。 エンドポイントのみを定義します。 線の始点は、の追加先となるの現在の点です `PathFigure` `LineSegment` 。

次の例では、オブジェクト内のオブジェクトを作成および表示する方法を示し `LineSegment` `Path` ます。

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure IsClosed="True"
                                StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="100,100" />
                                <LineSegment Point="100,50" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、(10100) から (100100)、および (100100) から (100, 50) までの線分が描画されます。 さらに、 `PathFigure` `IsClosed` プロパティがに設定されているため、は閉じられ `true` ます。 この結果、三角形が描画されます。

![LineSegments](geometry-images/linesegments.png "LineSegments")

### <a name="create-a-polybeziersegment"></a>PolyBezierSegment を作成する

は、 `PolyBezierSegment` 1 つまたは複数の3次ベジエ曲線を作成します。

クラスは、を `PolyBezierSegment` `Points` 定義する点を表す型のプロパティを定義し `PointCollection` `PolyBezierSegment` ます。 `PointCollection`は、 `ObservableCollection` オブジェクトのです [`Point`](xref:Xamarin.Forms.Point) 。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスに、 `PolyBezierSegment` 曲線の開始点のプロパティが含まれていません。 曲線の始点は、の追加先となるの現在の点です `PathFigure` `PolyBezierSegment` 。

次の例は、オブジェクトでを作成して表示する方法を示してい `PolyBezierSegment` `Path` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyBezierSegment Points="0,0 100,0 150,100 150,0 200,0 300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、によって `PolyBezierSegment` 2 つの3次ベジエ曲線が指定されています。 最初の曲線は、(10, 10) から (150100) までの制御ポイント (0, 0) と、(100, 0) の別の制御ポイントです。 2番目の曲線は、(150100) から (300, 10) の制御ポイント (150, 0) と、(200, 0) の別の制御ポイントです。

![PolyBezierSegment](geometry-images/polybeziersegment.png "PolyBezierSegment")

### <a name="create-a-polylinesegment"></a>System.windows.media.polylinesegment> を作成する

は `PolyLineSegment` 1 つ以上の線分を作成します。

クラスは、を `PolyLineSegment` `Points` 定義する点を表す型のプロパティを定義し `PointCollection` `PolyLineSegment` ます。 `PointCollection`は、 `ObservableCollection` オブジェクトのです [`Point`](xref:Xamarin.Forms.Point) 。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスには、 `PolyLineSegment` 行の開始点のプロパティが含まれていません。 線の始点は、の追加先となるの現在の点です `PathFigure` `PolyLineSegment` 。

次の例は、オブジェクトでを作成して表示する方法を示してい `PolyLineSegment` `Path` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,10">
                    <PathFigure.Segments>
                        <PolyLineSegment Points="50,10 50,50" />
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、は `PolyLineSegment` 2 行を指定しています。 最初の行は (10, 10) ~ (50, 10) で、2行目は (50, 10) から (50, 50) までです。

![System.windows.media.polylinesegment>](geometry-images/polylinesegment.png "System.windows.media.polylinesegment>")

### <a name="create-a-polyquadraticbeziersegment"></a>PolyQuadraticBezierSegment を作成する

は、 `PolyQuadraticBezierSegment` 1 つまたは複数の2次ベジエ曲線を作成します。

クラスは、を `PolyQuadraticBezierSegment` `Points` 定義する点を表す型のプロパティを定義し `PointCollection` `PolyQuadraticBezierSegment` ます。 `PointCollection`は、 `ObservableCollection` オブジェクトのです [`Point`](xref:Xamarin.Forms.Point) 。 このプロパティは、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスに、 `PolyQuadraticBezierSegment` 曲線の開始点のプロパティが含まれていません。 曲線の始点は、の追加先となるの現在の点です `PathFigure` `PolyQuadraticBezierSegment` 。

次の例では、オブジェクトでを作成およびレンダリングする方法を示し `PolyQuadraticBezierSegment` `Path` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyQuadraticBezierSegment Points="100,100 150,50 0,100 15,200" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、は `PolyQuadraticBezierSegment` 2 つのベジエ曲線を指定しています。 最初の曲線は (10, 10) から (150, 50) で、制御ポイントは (100100) です。 2番目の曲線は (100100) から (15200) で、制御ポイントは (0100) です。

![PolyQuadraticBezierSegment](geometry-images/polyquadraticbeziersegment.png "PolyQuadraticBezierSegment")

### <a name="create-a-quadraticbeziersegment"></a>System.windows.media.quadraticbeziersegment> を作成する

は、 `QuadraticBezierSegment` 2 つの点の間に2次ベジエ曲線を作成します。

`QuadraticBezierSegment` クラスでは、次のプロパティが定義されます。

- `Point1`[`Point`](xref:Xamarin.Forms.Point)曲線の制御点を表す型の。 このプロパティの既定値は (0, 0) です。
- `Point2`[`Point`](xref:Xamarin.Forms.Point)曲線の終点を表す型の。 このプロパティの既定値は (0, 0) です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> クラスに、 `QuadraticBezierSegment` 曲線の開始点のプロパティが含まれていません。 曲線の始点は、の追加先となるの現在の点です `PathFigure` `QuadraticBezierSegment` 。

次の例は、オブジェクトでを作成して表示する方法を示してい `QuadraticBezierSegment` `Path` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <QuadraticBezierSegment Point1="200,200"
                                                        Point2="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、2次ベジエ曲線を (10, 10) から (300, 10) に描画します。 曲線の制御点は (200200) です。

![System.windows.media.quadraticbeziersegment>](geometry-images/quadraticbeziersegment.png "System.windows.media.quadraticbeziersegment>")

### <a name="create-complex-geometries"></a>複雑なジオメトリを作成する

より複雑なジオメトリは、 `PathSegment` オブジェクトの組み合わせを使用して作成できます。 次の例では、、、およびを使用して図形を作成し `BezierSegment` `LineSegment` `ArcSegment` ます。

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,50">
                    <PathFigure.Segments>
                        <BezierSegment Point1="100,0"
                                       Point2="200,200"
                                       Point3="300,100"/>
                        <LineSegment Point="400,100" />
                        <ArcSegment Size="50,50"
                                    RotationAngle="45"
                                    IsLargeArc="True"
                                    SweepDirection="Clockwise"
                                    Point="200,100"/>
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、 `BezierSegment` が最初に4つのポイントを使用して定義されています。 次に、の `LineSegment` 終点からに `BezierSegment` よって指定された点までのを描画するを追加し `LineSegment` ます。 最後に、の `ArcSegment` 終点からに `LineSegment` よって指定された点まで、が描画され `ArcSegment` ます。

さらに複雑なジオメトリは、`PathGeometry` 内で複数の `PathFigure` オブジェクトを使用して作成できます。 次の例では、 `PathGeometry` 7 つのオブジェクトからを作成し `PathFigure` ます。その中には複数のオブジェクトが含まれ `PathSegment` ます。

```xaml
<Path Stroke="Red"
      StrokeThickness="12"
      StrokeLineJoin="Round">
    <Path.Data>
        <PathGeometry>
            <!-- H -->
            <PathFigure StartPoint="0,0">
                <LineSegment Point="0,100" />
            </PathFigure>
            <PathFigure StartPoint="0,50">
                <LineSegment Point="50,50" />
            </PathFigure>
            <PathFigure StartPoint="50,0">
                <LineSegment Point="50,100" />
            </PathFigure>

            <!-- E -->
            <PathFigure StartPoint="125, 0">
                <BezierSegment Point1="60, -10"
                               Point2="60, 60"
                               Point3="125, 50" />
                <BezierSegment Point1="60, 40"
                               Point2="60, 110"
                               Point3="125, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="150, 0">
                <LineSegment Point="150, 100" />
                <LineSegment Point="200, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="225, 0">
                <LineSegment Point="225, 100" />
                <LineSegment Point="275, 100" />
            </PathFigure>

            <!-- O -->
            <PathFigure StartPoint="300, 50">
                <ArcSegment Size="25, 50"
                            Point="300, 49.9"
                            IsLargeArc="True" />
            </PathFigure>
        </PathGeometry>
    </Path.Data>
</Path>
```

この例では、オブジェクトとオブジェクトの組み合わせを使用して、"Hello" という単語を1つのオブジェクトと共に描画し `LineSegment` `BezierSegment` `ArcSegment` ます。

![複数の PathFigure オブジェクト](geometry-images/multiple-pathfigures.png "複数の PathFigure オブジェクト")

## <a name="composite-geometries"></a>複合ジオメトリ

複合 geometry オブジェクトは、を使用して作成でき `GeometryGroup` ます。 クラスは、 `GeometryGroup` 1 つまたは複数のオブジェクトから複合ジオメトリを作成し `Geometry` ます。 任意の数の `Geometry` オブジェクトを `GeometryGroup` に追加できます。

`GeometryGroup` クラスでは、次のプロパティが定義されます。

- `Children`型の `GeometryCollection` 。これは、を定義するオブジェクトを設定し `GeomtryGroup` ます。 `GeometryCollection`は、 `ObservableCollection` オブジェクトのです `Geometry` 。
- `FillRule``FillRule`内の交差する領域を結合する方法を指定する型の。 `GeometryGroup` このプロパティの既定値は `FillRule.EvenOdd` です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> `Children`プロパティは `ContentProperty` クラスのである `GeometryGroup` ため、XAML から明示的に設定する必要はありません。

列挙体の詳細については `FillRule` 、「 [ Xamarin.Forms Shapes: Fill rules](fillrules.md)」を参照してください。

複合ジオメトリを描画するには、必要なオブジェクトをの `Geometry` 子として設定 `GeometryGroup` し、オブジェクトと共に表示し `Path` ます。 次の XAML は、この例を示しています。

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

この例では、 `EllipseGeometry` x 半径が同じで、中心座標が異なる4つのオブジェクトが結合されています。 これにより、既定の塗りつぶしルールにより、内側がオレンジ色になる4つの重なり合う円が作成され `EvenOdd` ます。

![System.windows.media.geometrygroup>](geometry-images/geometrygroup.png "System.windows.media.geometrygroup>")

## <a name="clip-with-a-geometry"></a>Geometry を使用してクリップする

[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスには、 `Clip` `Geometry` 要素の内容のアウトラインを定義する型のプロパティがあります。 `Clip`プロパティがオブジェクトに設定されている場合は、 `Geometry` の領域内にある領域だけが `Geometry` 表示されます。

次の例は、 `Geometry` のクリップ領域としてオブジェクトを使用する方法を示してい [`Image`](xref:Xamarin.Forms.Image) ます。

```xaml
<Image Source="monkeyface.png">
    <Image.Clip>
        <EllipseGeometry RadiusX="100"
                         RadiusY="100"
                         Center="180,180" />
    </Image.Clip>
</Image>
```

この例では、 `EllipseGeometry` との値が100で、 `RadiusX` `RadiusY` `Center` 値 (180180) がのプロパティに設定されてい `Clip` [`Image`](xref:Xamarin.Forms.Image) ます。 楕円の領域内にあるイメージの部分のみが表示されます。

![System.windows.media.ellipsegeometry> を使用してイメージをクリップする](geometries-images/clip-ellipsegeometry.png "System.windows.media.ellipsegeometry> を使用してイメージをクリップする")

> [!NOTE]
> オブジェクトのクリップには、単純なジオメトリ、パスジオメトリ、および複合ジオメトリを使用でき [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

## <a name="other-features"></a>その他の機能

クラスには `GeometryHelper` 、次のヘルパーメソッドが用意されています。

- `FlattenGeometry`。をに平坦化し `Geometry` `PathGeometry` ます。
- `FlattenCubicBezier`。3次ベジエ曲線を1つのコレクションに平坦化 `List<Point>` します。
- `FlattenQuadraticBezier`。2次ベジエ曲線を1つのコレクションに平坦化 `List<Point>` します。
- `FlattenArc`。楕円の円弧をコレクションに平坦化 `List<Point>` します。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
- [Xamarin.Forms図形: 塗りつぶしルール](fillrules.md)
