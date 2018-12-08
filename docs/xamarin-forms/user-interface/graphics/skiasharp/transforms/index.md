---
title: SkiaSharp の変換
description: この記事では、Xamarin.Forms アプリケーションで SkiaSharp グラフィックスを表示するための変換について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: a57e50f098c92dbfcdcaa3139565d2ba0e291e3d
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057536"
---
# <a name="skiasharp-transforms"></a>SkiaSharp の変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp のグラフィックスを表示するための変換について説明します_

SkiaSharp のメソッドとして実装されている従来のグラフィックス変換をサポートしている、 [ `SKCanvas` ](xref:SkiaSharp.SKCanvas)オブジェクト。 変換が座標とで指定したサイズを変更する数学的、`SKCanvas`グラフィカル オブジェクトが表示された関数を描画します。 変換では、多くの場合、またはアニメーションの反復的なグラフィックスを描画するために便利です。 いくつかのテクニック&mdash;ビットマップまたはテキストの回転など&mdash;変換を使用しないことはできません。

SkiaSharp の変換では、次の操作をサポートします。

- *変換*1 つの場所から別のシフト座標に
- *スケール*座標とサイズを増減する
- *回転*座標点を中心に回転するには
- *傾斜*シフトする水平方向または垂直方向を調整できるように、四角形を平行四辺形になります

これらと呼ばれます。*アフィン*を変換します。 アフィン変換では、常に平行な線を保持しに無限になるには、座標やサイズは発生しません。 正方形が、平行四辺形以外のものに変換しないと、円が楕円以外のものに変換されることはありません。

SkiaSharp が非アフィン変換もサポートしています (とも呼ばれる*射影*または*パースペクティブ*変換) 標準 3-3 での変換行列に基づきます。 非アフィン変換では、凸四辺形、180 度より小さい 4 辺すべて内部の角度の図に変換する四角形、します。 座標やサイズは、無限になる非アフィン変換が発生することができますが、3 D 効果に不可欠です。

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp と Xamarin.Forms の変換の違い

Xamarin.Forms で SkiaSharp と同様の変換もサポートします。 Xamarin.Forms [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスは、次の変換プロパティを定義します。

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) および [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)、 [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX)、と [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

`RotationX`と`RotationY`プロパティは、パースペクティブの変換準 3D 効果を作成します。

SkiaSharp の変換と Xamarin.Forms の変換の間のいくつかの重要な違いがあります。

SkiaSharp の変換が全体に適用されている最初の違いは、`SKCanvas`個人に Xamarin.Forms 変換の適用中にオブジェクト`VisualElement`派生クラス。 (する Xamarin.Forms の変換を適用することができます、`SKCanvasView`オブジェクト自体のため、`SKCanvasView`から派生した`VisualElement`が内で`SKCanvasView`、SkiaSkarp 変換が適用されます)。

SkiaSharp の変換の左上隅を基準と、 `SKCanvas` Xamarin.Forms 変換は、の左上隅を基準としたときに、`VisualElement`に適用されます。 スケーリングを適用するときに、この違いは重要ですし、回転変換のため、これらの変換は、常に特定の時点を基準とします。

SKiaSharp の変換が非常に大きな違いは、*メソッド*Xamarin.Forms 変換は*プロパティ*します。 これは、構文の違いを超えるセマンティックな相違点: SkiaSharp の変換は、Xamarin.Forms の変換状態を設定中に操作を実行します。 SkiaSharp の変換は、変換が適用される前に、描画する graphics オブジェクトではなく、グラフィックスの描画後のオブジェクトに適用されます。 これに対し、プロパティを設定するとすぐには、以前にレンダリングされる要素に Xamarin.Forms 変換が適用されます。 SkiaSharp の変換、累積的なメソッドが呼び出されます。Xamarin.Forms の変換には、別の値を持つプロパティが設定されている場合は置き換えられます。

このセクションでは、すべてのサンプル プログラムに表示、 **SkiaSharp の変換**のセクション、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラム。 ソース コードが記載されて、 [**変換**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms)ソリューションのフォルダー。

## <a name="the-translate-transformtranslatemd"></a>[平行移動変換](translate.md)

SkiaSharp のグラフィックスをシフトする平行移動変換を使用する方法について説明します。

## <a name="the-scale-transformscalemd"></a>[スケール変換](scale.md)

さまざまなサイズにオブジェクトを拡張するための SkiaSharp スケール変換を検出します。

## <a name="the-rotate-transformrotatemd"></a>[回転変換](rotate.md)

SkiaSharp の回転変換で可能なアニメーションと効果について説明します。

## <a name="the-skew-transformskewmd"></a>[傾斜変換](skew.md)

傾斜変換が傾いているグラフィカル オブジェクトを作成する方法を参照してください。

## <a name="matrix-transformsmatrixmd"></a>[行列変換](matrix.md)

さらに深く SkiaSharp の変換に、汎用性の高い変換行列。

## <a name="touch-manipulationstouchmd"></a>[タッチ操作](touch.md)

使用してマトリックスは、ドラッグ、スケーリング、および回転のタッチ操作を実装するために変換します。

## <a name="non-affine-transformsnon-affinemd"></a>[非アフィン変換](non-affine.md)

非アフィン変換の効果を使用した oridinary をとどまりません。

## <a name="3d-rotation3d-rotationmd"></a>[3 次元回転](3d-rotation.md)

非アフィン変換を使用して、3 D 空間で 2D オブジェクトを回転させます。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
