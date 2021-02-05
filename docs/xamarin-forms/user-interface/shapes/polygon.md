---
title: 'Xamarin.Forms 図形: 多角形'
description: Xamarin.FormsPolygon クラスを使用すると、閉じた図形を形成する一連の線が接続されたポリゴンを描画できます。
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/05/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2928e3bd32d7dd2722f01ad5a51dbaab127afd77
ms.sourcegitcommit: 06701714021545eb5e932847829b876082194ffc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585885"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms 図形: 多角形

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`クラスはクラスから派生 `Shape` し、閉じた図形を形成する一連の線が接続されている多角形を描画するために使用できます。 クラスから継承されるプロパティの詳細につい `Polygon` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Polygon` は次の特性を定義します。

- `Points`型の。 `PointCollection` これは、 [`Point`](xref:Xamarin.Forms.Point) 多角形の頂点を記述する構造体のコレクションです。
- `FillRule``FillRule`形状の内部の塗りつぶしを決定する方法を指定する型の。 このプロパティの既定値は `FillRule.EvenOdd` です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

`PointsCollection`型は `ObservableCollection` オブジェクトのです [`Point`](xref:Xamarin.Forms.Point) 。 `Point`構造体は `X` 、 `Y` `double` 2d 空間の x 座標と y 座標のペアを表す、型のプロパティとプロパティを定義します。 したがって、 `Points` プロパティは、多角形の頂点を表す x 座標と y 座標のペアのリストに設定する必要があります。これは、1つのコンマや1つ以上の空白で区切られます。 たとえば、"40, 10 70, 80" と "40 10, 70 80" はどちらも有効です。

列挙体の詳細については `FillRule` 、「 [ Xamarin.Forms Shapes: Fill rules](fillrules.md)」を参照してください。

## <a name="create-a-polygon"></a>多角形を作成する

多角形を描画するには、 `Polygon` オブジェクトを作成し、その `Points` プロパティに図形の頂点を設定します。 最初と最後の点をつなぐ線が自動的に描画されます。 多角形の内部を描画するには、その `Fill` プロパティをから派生したオブジェクトに設定し [`Brush`](xref:Xamarin.Forms.Brush) ます。 多角形に輪郭を付けるには、その `Stroke` プロパティをから派生したオブジェクトに設定し [`Brush`](xref:Xamarin.Forms.Brush) ます。 プロパティは、 `StrokeThickness` 多角形の輪郭の太さを指定します。 オブジェクトの詳細について `Brush` は、「 [ Xamarin.Forms ブラシ](~/xamarin-forms/user-interface/brushes/index.md)」を参照してください。

次の XAML の例は、塗りつぶされた多角形を描画する方法を示しています。

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5" />
```

この例では、三角形を表す塗りつぶされた多角形が描画されます。

![塗りつぶされた多角形](polygon-images/filled.png "塗りつぶされた多角形")

次の XAML の例は、破線の多角形を描画する方法を示しています。

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         StrokeDashArray="1,1"
         StrokeDashOffset="6" />
```

この例では、多角形の輪郭は破線です。

![破線の多角形](polygon-images/dashed.png "破線の多角形")

破線の多角形の描画の詳細については、「 [破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

次の XAML の例は、既定の fill ルールを使用する polygon を示しています。

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Blue"
         Stroke="Red"
         StrokeThickness="3" />
```

この例では、各多角形の塗りつぶしの動作が、fill ルールを使用して決定され `EvenOdd` ます。

![EvenOdd polygon](polygon-images/evenodd.png "EvenOdd polygon")

次の XAML の例は、fill ルールを使用する polygon を示してい `Nonzero` ます。

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Black"
         FillRule="Nonzero"
         Stroke="Yellow"
         StrokeThickness="3" />
```

![0以外の多角形](polygon-images/nonzero.png "0以外の多角形")

この例では、各多角形の塗りつぶしの動作が、fill ルールを使用して決定され `Nonzero` ます。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms 図形](index.md)
- [Xamarin.Forms 図形: 塗りつぶしルール](fillrules.md)
- [Xamarin.Forms ブラシ](~/xamarin-forms/user-interface/brushes/index.md)
