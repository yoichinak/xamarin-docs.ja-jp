---
title: 'Xamarin.Forms 図形: パス'
description: Xamarin.FormsPath クラスを使用して、曲線や複雑な形状を描画できます。
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1ba44903f20d0431c27f2d49429b2feb9c9a51e0
ms.sourcegitcommit: f920ac0724f09e5c9b4f36be1995a5a17a6d9f95
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94591061"
---
# <a name="no-locxamarinforms-shapes-path"></a>Xamarin.Forms 図形: パス

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Path`クラスはクラスから派生 `Shape` し、曲線や複雑な図形を描画するために使用できます。 これらの曲線と図形は、多くの場合、オブジェクトを使用して記述され `Geometry` ます。 クラスから継承されるプロパティの詳細につい `Path` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Path` は次の特性を定義します。

- `Data``Geometry`描画する図形を指定する型の。
- `RenderTransform`型の `Transform` 。これは、描画される前のパスのジオメトリに適用される変換を表します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

変換の詳細については、「 [ Xamarin.Forms パス変換](path-transforms.md)」を参照してください。

## <a name="create-a-path"></a>パスを作成する

パスを描画するには、 `Path` オブジェクトを作成し、そのプロパティを設定し `Data` ます。 プロパティを設定するには、次の2つの方法があり `Data` ます。

- `Data`パスマークアップ構文を使用して、XAML での文字列値を設定できます。 この方法では、 `Path.Data` 値はグラフィックスのシリアル化形式を使用します。 通常、この文字列値は、作成後に手動で編集しません。 代わりに、デザインツールを使用してデータを操作し、プロパティによって使用できる文字列フラグメントとしてエクスポートし `Data` ます。
- プロパティは、 `Data` オブジェクトに設定でき `Geometry` ます。 これには、特定の `Geometry` オブジェクト、または `GeometryGroup` 複数の geometry オブジェクトを1つのオブジェクトに結合できるコンテナーとして機能するがあります。

### <a name="create-a-path-with-path-markup-syntax"></a>パスマークアップ構文を使用してパスを作成する

次の XAML の例は、パスマークアップ構文を使用して三角形を描画する方法を示しています。

```xaml
<Path Data="M 10,100 L 100,100 100,50Z"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start" />
```

`Data`文字列は `M` 、パスの絶対開始点を確立するによって示される move コマンドで始まります。 `L` は line コマンドです。このコマンドは、始点から指定された終点までの直線を作成します。 `Z` close コマンドです。このコマンドは、現在の点を開始点につなげる線を作成します。 結果は三角形です。

![パスの三角形](path-images/triangle.png "パスの三角形")

パスマークアップ構文の詳細については、「 [ Xamarin.Forms パスマークアップ構文](path-markup-syntax.md)」を参照してください。

### <a name="create-a-path-with-geometry-objects"></a>Geometry オブジェクトを使用してパスを作成する

曲線と図形は `Geometry` 、オブジェクトのプロパティを設定するために使用されるオブジェクトを使用して記述できます `Path` `Data` 。 さまざまな `Geometry` オブジェクトから選択できます。 `EllipseGeometry`、`LineGeometry`、`RectangleGeometry` の各クラスは、比較的単純な図形を記述します。 より複雑な図形を作成したり、曲線を作成したりするには、`PathGeometry` を使用します。

`PathGeometry` オブジェクトは1つ以上のオブジェクトで構成され `PathFigure` ます。 各 `PathFigure` オブジェクトは、異なる形状を表します。 各 `PathFigure` オブジェクトは `PathSegment` 、それぞれが図形の接続部分を表す1つ以上のオブジェクトで構成されています。 セグメントの種類には、、、およびの各クラスが含まれ `LineSegment` `BezierSegment` `ArcSegment` ます。

次の XAML の例は、オブジェクトを使用して三角形を描画する方法を示してい `PathGeometry` ます。

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
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

この例では、三角形の始点は (10100) です。 線分は、(10100) から (100100)、および (100100) から (100, 50) までの間で描画されます。 次 `PathFigure.IsClosed` に、プロパティがに設定されているため、図の最初と最後のセグメントが接続され `true` ます。 結果は三角形です。

![パスの三角形](path-images/triangle.png "パスの三角形")

ジオメトリの詳細については、「 [ Xamarin.Forms ジオメトリ](geometries.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms 図形](index.md)
- [Xamarin.Forms ジオメトリ](geometries.md)
- [Xamarin.Forms パスマークアップ構文](path-markup-syntax.md)
- [Xamarin.Forms パスの変換](path-transforms.md)
