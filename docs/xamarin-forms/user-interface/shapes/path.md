---
title: 'Xamarin.Forms図形: パス'
description: Xamarin.FormsPath クラスを使用して、曲線や複雑な形状を描画できます。
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e558c510fa71f4d8f33b70b7dcb7e9a9334c6a84
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947330"
---
# <a name="xamarinforms-shapes-path"></a>Xamarin.Forms図形: パス

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Path`クラスはクラスから派生 `Shape` し、曲線や複雑な図形を描画するために使用できます。 これらの曲線と図形は、多くの場合、オブジェクトを使用して記述され `Geometry` ます。 クラスから継承されるプロパティの詳細につい `Path` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Path` は次の特性を定義します。

- `Data``Geometry`描画する図形を指定する型の。
- `RenderTransform`型の `Transform` 。これは、描画される前のパスのジオメトリに適用される変換を表します。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

変換の詳細については、「 [ Xamarin.Forms パス変換](path-transforms.md)」を参照してください。

## <a name="create-a-path"></a>パスを作成する

次の XAML の例は、特別な省略構文を使用して多角形を描画する方法を示しています。

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

`Data`文字列は、"moveto" コマンドで始まります。このコマンドは `M` 、パスの始点を確立するによって指定されています。 パスデータパラメーターでは、大文字と小文字が区別されます。 大文字は、 `M` 始点の絶対位置を示します。 小文字は `m` 相対座標を示します。 `L`は line コマンドです。このコマンドは、始点から指定された終点までの直線を作成します。

## <a name="path-geometry"></a>パスジオメトリ

曲線と図形は `Geometry` 、オブジェクトのプロパティを設定するために使用されるオブジェクトを使用して記述できます `Path` `Data` 。

さまざまな `Geometry` オブジェクトを選択できます。 、、およびの各クラスでは、 `EllipseGeometry` `LineGeometry` `RectangleGeometry` 比較的単純なシェイプが記述されています。 より複雑な図形を作成したり、曲線を作成したりするには、を使用 `PathGeometry` します。

`PathGeometry`オブジェクトは1つ以上のオブジェクトで構成され `PathFigure` ます。 各 `PathFigure` オブジェクトは、異なる形状を表します。 各 `PathFigure` オブジェクトは `PathSegment` 、それぞれが図形の接続部分を表す1つ以上のオブジェクトで構成されています。 セグメントの種類には、 `LineSegment` 、 `BezierSegment` 、およびがあり `ArcSegment` ます。

次の例では、を使用して `Path` 三角形を描画しています。

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
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

ジオメトリの詳細については、「 [ Xamarin.Forms ジオメトリ](geometries.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms図形](index.md)
- [Xamarin.Formsシェイプジオメトリ](geometries.md)
- [Xamarin.Formsパスの変換](path-transforms.md)
