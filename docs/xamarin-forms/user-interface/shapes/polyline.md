---
title: 'Xamarin.Forms 図形: ポリライン'
description: Xamarin.Formsポリラインクラスを使用すると、接続された一連の直線を描画できます。
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2c80a82c50c34d45184e8f6359e8940b697e9823
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375058"
---
# <a name="no-locxamarinforms-shapes-polyline"></a>Xamarin.Forms 図形: ポリライン

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polyline`クラスはクラスから派生し、接続された `Shape` 一連の直線を描画するために使用できます。 ポリラインは多角形に似ていますが、ポリラインの最後の点は最初の点に接続されていません。 クラスから継承されるプロパティの詳細につい `Polyline` `Shape` ては、「 [ Xamarin.Forms 図形](index.md)」を参照してください。

`Polyline` は次の特性を定義します。

- `Points`型の。 `PointCollection` これは、 `Point` ポリラインの頂点を記述する構造体のコレクションです。
- `FillRule`型の。 `FillRule` これは、ポリライン内の交差する領域の結合方法を指定します。 このプロパティの既定値は `FillRule.EvenOdd` です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

`PointsCollection`型は `ObservableCollection` オブジェクトのです [`Point`](xref:Xamarin.Forms.Point) 。 `Point`構造体は `X` 、 `Y` `double` 2d 空間の x 座標と y 座標のペアを表す、型のプロパティとプロパティを定義します。 したがって、 `Points` プロパティは、ポリラインの頂点を記述する x 座標と y 座標のペアのリストに設定する必要があります。これは、1つのコンマや1つ以上の空白で区切られます。 たとえば、"40, 10 70, 80" と "40 10, 70 80" はどちらも有効です。

列挙体の詳細については `FillRule` 、「 [ Xamarin.Forms Shapes: Fill rules](fillrules.md)」を参照してください。

## <a name="create-a-polyline"></a>ポリラインを作成する

ポリラインを描画するには、 `Polyline` オブジェクトを作成し、その `Points` プロパティに図形の頂点を設定します。 ポリラインに輪郭を付けるには、その `Stroke` プロパティをに設定 [`Color`](xref:Xamarin.Forms.Color) します。 プロパティは、 `StrokeThickness` ポリラインの輪郭の太さを指定します。

> [!IMPORTANT]
> のプロパティをに設定すると、 `Fill` `Polyline` [`Color`](xref:Xamarin.Forms.Color) 始点と終点が交差しない場合でも、ポリラインの内部空間が描画されます。

次の XAML の例は、ポリラインを描画する方法を示しています。

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          StrokeThickness="1" />
```

この例では、赤いポリラインが描画されます。

![ポリライン](polyline-images/stroke.png "ポリライン")

次の XAML の例は、破線のポリラインを描画する方法を示しています。

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          StrokeThickness="2"
          StrokeDashArray="1,1"
          StrokeDashOffset="6" />
```

この例では、ポリラインは破線です。

![破線](polyline-images/dashed.png "破線")

破線のポリラインの描画の詳細については、「 [破線の図形を描画](index.md#draw-dashed-shapes)する」を参照してください。

次の XAML の例は、既定の fill ルールを使用するポリラインを示しています。

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Blue"
          Stroke="Red"
          StrokeThickness="3" />
```

この例では、ポリラインの塗りつぶしの動作が、fill ルールを使用して決定され `EvenOdd` ます。

![EvenOdd ポリライン](polyline-images/evenodd.png "EvenOdd polyine")

次の XAML の例は、fill ルールを使用するポリラインを示してい `Nonzero` ます。

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Black"
          FillRule="Nonzero"
          Stroke="Yellow"
          StrokeThickness="3" />
```

![0以外のポリライン](polyline-images/nonzero.png "0以外のポリライン")

この例では、ポリラインの塗りつぶしの動作が、fill ルールを使用して決定され `Nonzero` ます。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms 図形](index.md)
- [Xamarin.Forms 図形: 塗りつぶしルール](fillrules.md)