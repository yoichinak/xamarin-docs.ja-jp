---
title: SkiaSharp 変換
description: SkiaSharp グラフィックを表示するための変換についてください。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: a94e1011557a5c7487315681e6e7c4d106ae4ba1
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/05/2018
---
# <a name="skiasharp-transforms"></a>SkiaSharp 変換

_SkiaSharp グラフィックを表示するための変換についてください。_

SkiaSharp のメソッドとして実装されている従来のグラフィックス変換をサポートしている、 [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/)オブジェクト。 変換が座標とで指定したサイズを変更する数学的に、`SKCanvas`グラフィカル オブジェクトがレンダリングされるように関数を描画します。 変換では、多くの場合、便利なは、反復的なグラフィックスを描画するため、またはアニメーションを実行します。 一部のテクニック&mdash;ビットマップまたはテキストの回転など&mdash;変換を使用せずにできない場合は、します。

SkiaSharp 変換には、次の操作がサポートします。

- *翻訳*を 1 つの場所から別の shift キーを押し座標
- *スケール*座標とサイズを増減する
- *回転*座標点を中心に回転するには
- *傾斜*座標水平方向または垂直方向にシフトする平行四辺形が四角形になるように

これらと呼ばれます*アフィン*を変換します。 アフィン変換では、平行線を常に保持し、無限にするには、座標またはサイズが発生することはありません。 平行四辺形、以外のものに正方形が変換されることはありませんし、円が楕円以外のものに変換されることはありません。

SkiaSharp には、非アフィン変換もサポートしています (とも呼ばれる*射影*または*パースペクティブ*変換) 標準的な 3-3 での変換行列に基づきます。 非アフィン変換では、すべて凸状の四角形 (4 方向図 180 度未満のすべての内部の角度で) に変換する四角形を使用します。 座標やサイズは、無限になる非アフィン変換が発生することができますが、3 D 効果に不可欠です。

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp と Xamarin.Forms の変換の違い

Xamarin.Forms では、SkiaSharp と同様の変換もサポートします。 Xamarin.Forms [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)クラスは、次の変換のプロパティを定義します。

- `TranslationX` および `TranslationY`
- `Scale`
- `Rotation`、`RotationX`、および `RotationY`

`RotationX`と`RotationY`プロパティは、パースペクティブの変換準 3D 効果を作成します。

SkiaSharp 変換や Xamarin.Forms 変換の間でのいくつかの重要な違いもあります。

最初の違いは、全体に SkiaSharp 変換が適用される`SKCanvas`Xamarin.Forms トランス フォームのユーザーに適用中にオブジェクト`VisualElement`派生します。 (Xamarin.Forms 変換を適用することができます、`SKCanvasView`オブジェクト自体、`SKCanvasView`から派生した`VisualElement`がその`SKCanvasView`、SkiaSkarp 変換を適用します)。

SkiaSharp 変換は、の左上隅を基準とした、 `SKCanvas` Xamarin.Forms 変換は、の左上隅を基準としたときに、`VisualElement`に適用されます。 スケーリングを適用するときに、この違いは重要と回転変換の変換は、常に特定の時点の基準としたためです。

非常に大きな違いは、ある SKiaSharp 変換*メソッド*Xamarin.Forms 変換されているときに*プロパティ*です。 これは、構文の違いを超えるセマンティックの相違: SkiaSharp 変換は、Xamarin.Forms 変換状態を設定中に、操作を実行します。 SkiaSharp 変換は、変換が適用される前に描画されるグラフィックス オブジェクトではなく、後で描画されたグラフィックス オブジェクトに適用されます。 これに対し、プロパティを設定するとすぐには、以前にレンダリングされる要素に、Xamarin.Forms 変換が適用されます。 SkiaSharp 変換、累積的なメソッドが呼び出されます。Xamarin.Forms の変換には、別の値を持つプロパティが設定されている場合は置き換えられます。

という見出しの下に表示されるこのセクションのすべてのサンプル プログラム**変換**のホーム ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)プログラム、および、 [**変換**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms)ソリューションのフォルダーです。

## <a name="the-translate-transformtranslatemd"></a>[平行移動変換](translate.md)

平行移動の変換を使用して、SkiaSharp グラフィックスをシフトする方法を説明します。

## <a name="the-scale-transformscalemd"></a>[スケール変換](scale.md)

オブジェクトをさまざまなサイズに合わせたスケーリングの SkiaSharp スケール変換を検出します。

## <a name="the-rotate-transformrotatemd"></a>[回転変換](rotate.md)

SkiaSharp 回転変換で実行できるアニメーションと効果について説明します。

## <a name="the-skew-transformskewmd"></a>[傾斜変換](skew.md)

傾斜変換が SkiaSharp で割引グラフィカル オブジェクトを作成する方法を参照してください。

## <a name="matrix-transformsmatrixmd"></a>[行列変換](matrix.md)

さらに深く SkiaSharp 変換に、汎用性の変換行列。

## <a name="touch-manipulationstouchmd"></a>[タッチ操作](touch.md)

使用して行列による変換をドラッグして、スケーリング、および回転のタッチ操作を実装します。

## <a name="non-affine-transformsnon-affinemd"></a>[非アフィン変換](non-affine.md)

非アフィン変換効果 oridinary を超えます。

## <a name="3d-rotation3d-rotationmd"></a>[3 次元回転](3d-rotation.md)

非アフィン変換を使用して、3 D 空間に 2 D のオブジェクトを回転します。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
