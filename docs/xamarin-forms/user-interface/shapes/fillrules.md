---
title: 'Xamarin.Forms図形: 塗りつぶしルール'
description: Xamarin.Forms図形塗りつぶしルールは、ポイントが図形の塗りつぶし領域内にあるかどうかを決定します。
ms.prod: xamarin
ms.assetid: 5CABB22B-C6BE-43D1-91D9-6E90A4BD5622
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 86bbad476f206c13e6437f867c8e85e6bea5063a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937294"
---
# <a name="xamarinforms-shapes-fill-rules"></a>Xamarin.Forms図形: 塗りつぶしルール

![プレリリース API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

いくつか Xamarin.Forms の図形クラス `FillRule` には、型のプロパティがあり `FillRule` ます。 これらには、 `Polygon` 、 `Polyline` 、および `GeometryGroup` が含まれます。

`FillRule`列挙体は、 `EvenOdd` メンバーとメンバーを定義し `Nonzero` ます。 各メンバーは、あるポイントが図形の塗りつぶし領域内にあるかどうかを判断するための別のルールを表します。

> [!IMPORTANT]
> すべての図形は、塗りつぶしルールのために閉じていると見なされます。

## <a name="evenodd"></a>EvenOdd

この `EvenOdd` fill ルールでは、地点から任意の方向に無限に伸びる射線を描画し、その射線が交差する図形内のセグメントの数をカウントします。 この数値が奇数の場合、ポイントは内側にあります。 この数値が偶数の場合、ポイントは外側にあります。

次の XAML の例では、複合図形を作成してレンダリングし `FillRule` ます。既定では次のように `EvenOdd` なります。

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <!-- FillRule doesn't need to be set, because EvenOdd is the default. -->
        <GeometryGroup>
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

この例では、一連の同心円リングで構成される複合図形が表示されます。

![EvenOdd fill ルールが設定される複合図形](fillrule-images/evenodd.png "EvenOdd fill ルールが設定される複合図形")

複合図形では、中央と3番目のリングは塗りつぶされていないことに注意してください。 これは、これらの2つのリング内のいずれかの地点から描画された射線が、偶数のセグメントを通過するためです。

![EvenOdd fill ルールが設定される注釈付き複合図形](fillrule-images/evenodd-annotated.png "EvenOdd fill ルールが設定される注釈付き複合図形")

上の図では、赤い円はポイントを表し、線は任意の光線を表しています。 上の点では、2つの任意の光線が偶数の線セグメントを通過します。 そのため、ポイントのあるリングは塗りつぶされていません。 下限点として、それぞれが奇数の線分を通過する2つの任意の光線があります。 したがって、ポイントがあるリングは塗りつぶされます。

## <a name="nonzero"></a>なら

この `Nonzero` 塗りつぶしルールでは、ある地点から任意の方向に無限に伸びる射線を描画し、図形のセグメントが射線と交差する場所を調べます。 カウントは0から始まり、セグメントが光線を左から右に交差するたびに増加し、セグメントが右から左に射線と交差するたびにデクリメントされます。 跨りをカウントした後、結果がゼロの場合、ポイントは多角形の外側にあります。 それ以外の場合は、内にあります。

次の XAML の例では、をに設定して、複合図形を作成し、レンダリングし `FillRule` `Nonzero` ます。

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <GeometryGroup FillRule="Nonzero">
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

この例では、一連の同心円リングで構成される複合図形が表示されます。

![0以外の塗りつぶしルールのある複合図形](fillrule-images/nonzero.png "0以外の塗りつぶしルールのある複合図形")

複合図形で、すべてのリングがいっぱいになっていることを確認します。 これは、すべてのセグメントが同じ方向で実行されているために、任意の点から描画された射線が1つ以上のセグメントをまたぐため、跨りの合計がゼロに等しくないためです。

![0以外の塗りつぶしルールを持つ注釈付き複合図形](fillrule-images/nonzero-annotated.png "0以外の塗りつぶしルールを持つ注釈付き複合図形")

上の図では、赤の矢印はセグメントが描画される方向を表し、黒い矢印は最も内側のリング内のポイントから実行されている任意の射線を表しています。 値 0 から開始し、セグメントは左から右に射線と交わるため、射線が交わるセグメントごとに値 1 が加算されます。

異なる方向で実行されるセグメントを含むより複雑な図形は、fill ルールの動作をより正確に示すために必要です `Nonzero` 。 次の XAML の例では、前の例と同様の図形を作成しますが、ではなくを使用して作成されている点が異なり `PathGeometry` `EllipseGeometry` ます。

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF">
     <Path.Data>
         <GeometryGroup FillRule="Nonzero">
             <PathGeometry>
                 <PathGeometry.Figures>
                     <!-- Inner ring -->
                     <PathFigure StartPoint="120,120">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="50,50"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,120" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Second ring -->
                     <PathFigure StartPoint="120,100">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="70,70"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,100" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Third ring  -->
                         <PathFigure StartPoint="120,70">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="100,100"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,70" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Outer ring -->
                     <PathFigure StartPoint="120,300">
                         <PathFigure.Segments>
                             <ArcSegment Size="130,130"
                                         IsLargeArc="True"
                                         SweepDirection="Clockwise"
                                         Point="140,300" />
                         </PathFigure.Segments>
                     </PathFigure>
                 </PathGeometry.Figures>
             </PathGeometry>
         </GeometryGroup>
     </Path.Data>
 </Path>
```

この例では、閉じていない一連の円弧セグメントが描画されます。

![0以外の塗りつぶしルールのある複合図形](fillrule-images/nonzero-gaps.png "0以外の塗りつぶしルールのある複合図形")

上の図では、中央の3番目の弧は塗りつぶされていません。 これは、指定された射線からパスのセグメントを越える値の合計が0であるためです。

![0以外の塗りつぶしルールを持つ注釈付き複合図形](fillrule-images/nonzero-gaps-annotated.png "0以外の塗りつぶしルールを持つ注釈付き複合図形")

上の図では、赤い円はポイントを表し、黒い線は塗りつぶされていない領域の点から移動する任意の光線を表し、赤い矢印はセグメントが描画される方向を表します。 ご覧のように、セグメントを越える線の値の合計はゼロになります。

- 右斜めに移動する任意の射線は、異なる方向に実行される2つのセグメントを交差します。 したがって、セグメントは、値0を指定して互いにキャンセルします。
- 斜めに左に移動する任意の射線が、合計6つのセグメントを超えています。 ただし、0が最終的な合計であるように、クロス集計は互いにキャンセルされます。

0を合計すると、リングがいっぱいになります。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形](index.md)
