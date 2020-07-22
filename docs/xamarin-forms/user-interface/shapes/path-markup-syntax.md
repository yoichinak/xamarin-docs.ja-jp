---
title: 'Xamarin.Forms図形: パスマークアップ構文'
description: Xamarin.Formsパスマークアップ構文を使用すると、XAML でパスジオメトリをコンパクトに指定できます。
ms.prod: xamarin
ms.assetid: A2C1BD59-1A16-4E26-A825-0338E2AF9E65
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 124c739f68ce8a3fcbc359a07513a2bcb178578f
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853122"
---
# <a name="xamarinforms-shapes-path-markup-syntax"></a>Xamarin.Forms図形: パスマークアップ構文

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Xamarin.Formsパスマークアップ構文を使用すると、XAML でパスジオメトリをコンパクトに指定できます。 構文は、プロパティに文字列値として指定され `Path.Data` ます。

```xaml
<Path Stroke="Black"
      Data="M13.908992,16.207977 L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983Z" />
```

パスマークアップ構文は、省略可能な値と1つ以上の図の説明で構成され `FillRule` ています。 この構文は、 `<Path Data="` *[fillrule]* *figureDescription* *[figureDescription]* として表すことができます。 * `" ... />`

この構文では、次のようになります。

- *Fillrule*は、 `Xamarin.Forms.Shapes.FillRule` ジオメトリでまたはを使用する必要があるかどうかを指定する省略可能なです `EvenOdd` `Nonzero` `FillRule` 。 `F0`は、塗りつぶしルールを指定するために使用され `EvenOdd` `F1` ます。を使用して、 `Nonzero` 塗りつぶしルールを指定します。 塗りつぶしルールの詳細については、「 [ Xamarin.Forms 図形: 塗りつぶしルール](fillrules.md)」を参照してください。
- *figureDescription*は、移動コマンド、描画コマンド、および省略可能な close コマンドで構成される図形を表します。 Move コマンドは、図形の始点を指定します。 描画コマンドは、図の内容を記述し、省略可能な [閉じる] コマンドを実行して図を閉じます。

上記の例では、パスのマークアップ構文で、move コマンド () を使用して開始点を指定し、 `M` line コマンド () を使用して一連の直線を指定 `L` し、close コマンド () を使用してパスを閉じ `Z` ます。

パスマークアップ構文では、コマンドの前または後にスペースは必要ありません。 また、2つの数値はコンマまたは空白で区切る必要はありませんが、文字列が明確である場合にのみ実現できます。

> [!TIP]
> パスマークアップ言語では、スケーラブルベクターグラフィックス (SVG) イメージパス定義と互換性のある構文が使用されるため、SVG 形式からグラフィックスを移植する場合に便利です。

## <a name="move-command"></a>移動コマンド

Move コマンドは、新しい図形の始点を指定します。 このコマンドの構文は、 `M` *startPoint*または `m` *startPoint*です。

この構文では、 *startPoint*は [`Point`](xref:Xamarin.Forms.Point) 新しい図形の始点を指定する構造体です。 Move コマンドの後に複数のポイントを指定すると、それらのポイントに線が描画されます。

`M 10,10`は、有効な move コマンドの例です。

## <a name="draw-commands"></a>描画コマンド

描画コマンドは、さまざまな図形コマンドで構成できます。 次の描画コマンドを使用できます。

- Line ( `L` または `l` )。
- 水平線 ( `H` または `h` )。
- 垂直線 ( `V` または `v` )。
- 楕円の円弧 ( `A` または `a` )。
- 3次ベジエ曲線 ( `C` または `c` )。
- 2次ベジエ曲線 ( `Q` または `q` )。
- 平滑3次ベジエ曲線 ( `S` または `s` )。
- Smooth 2 次ベジエ曲線 ( `T` または `t` )。

各 draw コマンドには、大文字と小文字を区別しない文字が指定されています。 同じ種類のコマンドを複数回続けて入力するときは、重複するコマンドの入力を省略できます。 たとえば、 `L 100,200 300,400` はと同じです `L 100,200 L 300,400` 。

### <a name="line-command"></a>直線コマンド

Line コマンドは、現在の点と指定された終点の間に直線を作成します。 このコマンドの構文は、 `L` *エンドポイント*または `l` *エンドポイント*です。

この構文では、*エンド*ポイントは、 [`Point`](xref:Xamarin.Forms.Point) 直線の終点を表すです。

たとえば、`L 20,30` や `L 20 30` は有効な直線コマンドです。

直線をオブジェクトとして作成する方法の詳細につい `PathGeometry` ては、「 [Linesegment を作成する](geometries.md#create-a-linesegment)」を参照してください。

### <a name="horizontal-line-command"></a>水平線コマンド

水平線コマンドは、現在の点と指定した x 座標の間に水平線を作成します。 このコマンドの構文は `H` *x*または `h` *x*です。

この構文では、 *x*は、 `double` 直線の終点の x 座標を表すです。

`H 90` は、有効な水平線コマンドの例です。

### <a name="vertical-line-command"></a>垂直線コマンド

垂直線コマンドは、現在の点と指定された y 座標の間に垂直線を作成します。 このコマンドの構文は、 `V` *y*または `v` *y*です。

この構文で*y*は、 `double` 直線の終点の y 座標を表すです。

`V 90` は、有効な垂直線コマンドの例です。

### <a name="elliptical-arc-command"></a>楕円円弧コマンド

楕円の弧コマンドは、現在の点と指定された終点の間に楕円の円弧を作成します。 このコマンドの構文は、 `A` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endpoint*または `a` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endpoint*です。

この構文では、次のようになります。

- `size`は、 [`Size`](xref:Xamarin.Forms.Size) 円弧の x 半径と y 半径を表すです。
- `rotationAngle``double`楕円の回転を表すです (度単位)。
- `isLargeArcFlag`弧の角度が180°以上でなければならない場合は、1に設定します。それ以外の場合は0に設定します。
- `sweepDirectionFlag`円弧が正の方向に描画される場合は、1に設定されます。それ以外の場合は0に設定されます。
- `endPoint`[`Point`](xref:Xamarin.Forms.Point)円弧が描画されるです。

`A 150,150 0 1,0 150,-150`は、有効な楕円弧コマンドの例です。

楕円の円弧をオブジェクトとして作成する方法については `PathGeometry` 、「 [Arcsegment を作成](geometries.md#create-an-arcsegment)する」を参照してください。

### <a name="cubic-bezier-curve-command"></a>3次ベジエ曲線コマンド

3次ベジエ曲線コマンドは、指定された2つの制御点を使用して、現在の点と指定された終点の間に3次ベジエ曲線を作成します。 このコマンドの構文は、 `C` *controlPoint1* *controlPoint2* *endpoint*または `c` *controlPoint1* *controlPoint2* *endpoint*です。

この構文では、次のようになります。

- *controlPoint1*は、曲線の [`Point`](xref:Xamarin.Forms.Point) 最初の制御点を表すです。これは曲線の開始接線を決定します。
- *controlPoint2*は、曲線の [`Point`](xref:Xamarin.Forms.Point) 2 番目の制御点を表すです。これは曲線の終了タンジェントを決定します。
- *エンドポイント*は、 [`Point`](xref:Xamarin.Forms.Point) 曲線が描画される点を表すです。

`C 100,200 200,400 300,200`は、有効な3次ベジエ曲線コマンドの例です。

3次ベジエ曲線をオブジェクトとして作成する方法の詳細につい `PathGeometry` ては、「 [Create a system.windows.media.beziersegment>](geometries.md#create-a-beziersegment)」を参照してください。

### <a name="quadratic-bezier-curve-command"></a>2次ベジエ曲線コマンド

2次ベジエ曲線コマンドは、指定された制御点を使用して、現在の点と指定された終点の間に2次ベジエ曲線を作成します。 このコマンドの構文は、 `Q` *controlpoint* *エンド*ポイントまたは `q` *controlpoint* *エンド*ポイントです。

この構文では、次のようになります。

- *Controlpoint*は、曲線の [`Point`](xref:Xamarin.Forms.Point) 制御点を表すです。これは曲線の始点と終点を決定します。
- *エンドポイント*は、 [`Point`](xref:Xamarin.Forms.Point) 曲線が描画される点を表すです。

`Q 100,200 300,200` は、有効な 2 次ベジエ曲線コマンドの例です。

2次ベジエ曲線をオブジェクトとして作成する方法の詳細につい `PathGeometry` ては、「 [Create a system.windows.media.quadraticbeziersegment>](geometries.md#create-a-quadraticbeziersegment)」を参照してください。

### <a name="smooth-cubic-bezier-curve-command"></a>Smooth 三次ベジエ曲線コマンド

Smooth 三次ベジエ曲線コマンドは、指定された制御点を使用して、現在の点と指定された終点の間に3次ベジエ曲線を作成します。 このコマンドの構文は、 `S` *controlPoint2* *endpoint*または `s` *controlPoint2* *endpoint*です。  

この構文では、次のようになります。

- *controlPoint2*は、曲線の [`Point`](xref:Xamarin.Forms.Point) 2 番目の制御点を表すです。これは曲線の終了タンジェントを決定します。
- *エンドポイント*は、 [`Point`](xref:Xamarin.Forms.Point) 曲線が描画される点を表すです。

最初の制御点は、現在の点を基準として、前のコマンドの2つ目の制御点のリフレクションと見なされます。 前のコマンドがない場合、または前のコマンドが3次ベジエ曲線コマンドまたは smooth 三次ベジエ曲線コマンドでなかった場合、最初の制御点は、現在のポイントと一致していると見なされます。

`S 100,200 200,300`は、有効な smooth 三次ベジエ曲線コマンドの例です。

### <a name="smooth-quadratic-bezier-curve-command"></a>Smooth 2 次ベジエ曲線コマンド

Smooth 2 次ベジエ曲線コマンドは、コントロールポイントを使用して、現在の点と指定された終点の間に2次ベジエ曲線を作成します。 このコマンドの構文は、 `T` *エンドポイント*または `t` *エンドポイント*です。

この構文では、*エンドポイント*は、 [`Point`](xref:Xamarin.Forms.Point) 曲線が描画される点を表すです。

制御点は、現在の点に対する前のコマンドの制御点のリフレクションと見なされます。 前のコマンドが存在しない場合、または前のコマンドが2次ベジエ曲線または smooth 2 次ベジエ曲線コマンドでなかった場合、制御点は現在の点と一致していると見なされます。

`T 100,30`は、有効な smooth 2 次ベジエ曲線コマンドの例です。

## <a name="close-command"></a>終了コマンド

Close コマンドは、現在の図形を終了し、現在の点を図形の開始点につなげる線を作成します。 したがって、このコマンドは、最後のセグメントと図形の最初のセグメントの間に、直線結合を作成します。

Close コマンドの構文は、 `Z` または `z` です。

## <a name="additional-values"></a>追加の値

標準の数値の代わりに、次のような大文字と小文字を区別する特殊な値を使用することもできます。

- `Infinity`を表し `double.PositiveInfinity` ます。
- `-Infinity`を表し `double.NegativeInfinity` ます。
- `NaN`を表し `double.NaN` ます。

また、大文字と小文字を区別しない科学的表記法を使用することもできます。 したがって、 `+1.e17` は有効な値です。

## <a name="related-links"></a>関連リンク

- [図形のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms図形: ジオメトリ](geometries.md)
- [Xamarin.Forms図形: 塗りつぶしルール](fillrules.md)
