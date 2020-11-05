---
title: SkiaSharp の変換
description: この記事では、アプリケーションで SkiaSharp グラフィックスを表示するための変換につい Xamarin.Forms て説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6c59a2c669a6a60049b6bb6383faea35a7de3631
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371197"
---
# <a name="skiasharp-transforms"></a>SkiaSharp の変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp グラフィックスを表示するための変換について説明します_

SkiaSharp は、オブジェクトのメソッドとして実装されている従来のグラフィックス変換をサポートしてい [`SKCanvas`](xref:SkiaSharp.SKCanvas) ます。 数学的には、 `SKCanvas` グラフィックオブジェクトがレンダリングされるときに、描画関数で指定した座標とサイズを変更します。 変換は、多くの場合、繰り返しのグラフィックスやアニメーションの描画に便利です。 変換を &mdash; 使用しなくても、ビットマップやテキストの回転などのいくつか &mdash; の手法は実行できません。

SkiaSharp の変換は、次の操作をサポートします。

- ある場所から別の場所へのシフト座標への *変換*
- 座標とサイズを増減するための *スケール*
- ポイントの周りを回転するための *回転*
- 四角形が平行四辺形になるように、水平方向または垂直方向にシフト座標に *傾斜* します

これらは、 *アフィン* 変換と呼ばれます。 アフィン変換は、常に平行線を保持し、座標やサイズが無限になることはありません。 正方形は、平行四辺形以外には変換されず、円は楕円以外のものに変換されることはありません。

SkiaSharp は、標準の 3-3 変換行列に基づいて、非アフィン変換 ( *射影* または *パースペクティブ* 変換とも呼ばれます) もサポートしています。 非アフィン変換を使用すると、四角形を任意の凸形に変換できます。これは、すべての内側の角度が180°未満の4つの辺を持つ図です。 非アフィン変換は、座標またはサイズが無限になる可能性がありますが、3D 効果にとっては重要です。

## <a name="differences-between-skiasharp-and-no-locxamarinforms-transforms"></a>SkiaSharp と変換の違い Xamarin.Forms

Xamarin.Forms では、SkiaSharp の変換に似た変換もサポートされています。 クラスは、 Xamarin.Forms [`VisualElement`](xref:Xamarin.Forms.VisualElement) 次の変換プロパティを定義します。

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) および [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 、 [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 、および [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

`RotationX`プロパティと `RotationY` プロパティは、準3d 効果を作成するパースペクティブ変換です。

SkiaSharp の変換と変換には、いくつかの重要な違いがあり Xamarin.Forms ます。

最初の違いは、 `SKCanvas` Xamarin.Forms 変換が個別の派生物に適用されている間に、SkiaSharp 変換がオブジェクト全体に適用されることです `VisualElement` 。 (は Xamarin.Forms `SKCanvasView` `SKCanvasView` から派生し `VisualElement` ますが、その中で SkiaSkarp 変換が適用されるため、オブジェクト自体に変換を適用でき `SKCanvasView` ます)。

SkiaSharp の変換は、の左上隅を基準としていますが、 `SKCanvas` Xamarin.Forms 変換は適用されるの左上隅を基準としてい `VisualElement` ます。 この違いは、スケーリングと回転の変換を適用するときに重要です。これらの変換は常に特定のポイントに対して相対的であるためです。

非常に大きな違いは、変換がプロパティであるのに対して、SKiaSharp の変換が *メソッド* であることです Xamarin.Forms 。 *properties* これは、構文の相違点を超えるセマンティックな違いです。 SkiaSharp は、変換時に操作を実行 Xamarin.Forms し、変換は状態を設定します。 SkiaSharp の変換は、その後の描画グラフィックスオブジェクトに適用されますが、変換が適用される前に描画されるグラフィックスオブジェクトには適用されません。 これに対して、変換は、 Xamarin.Forms プロパティが設定されるとすぐに、以前にレンダリングされた要素に適用されます。 SkiaSharp の変換は、メソッドが呼び出されると累積されます。 Xamarin.Forms プロパティが別の値に設定されている場合、変換は置き換えられます。

このセクションのすべてのサンプルプログラムは、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの **SkiaSharp の変換** セクションに表示されます。 ソースコードは、ソリューションの [ [**変換**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) ] フォルダーにあります。

## <a name="the-translate-transform"></a>[平行移動変換](translate.md)

変換変換を使用して SkiaSharp グラフィックスをシフトする方法について説明します。

## <a name="the-scale-transform"></a>[スケール変換](scale.md)

オブジェクトをさまざまなサイズにスケーリングするための SkiaSharp scale 変換について説明します。

## <a name="the-rotate-transform"></a>[回転変換](rotate.md)

SkiaSharp rotate 変換で可能な効果とアニメーションについて説明します。

## <a name="the-skew-transform"></a>[傾斜変換](skew.md)

傾斜変換によって、傾いたグラフィックオブジェクトを作成する方法を確認します。

## <a name="matrix-transforms"></a>[行列変換](matrix.md)

汎用性のある変換マトリックスを使用した SkiaSharp 変換について詳しく説明します。

## <a name="touch-manipulations"></a>[タッチ操作](touch.md)

マトリックス変換を使用して、ドラッグ、拡大縮小、および回転のタッチ操作を実装します。

## <a name="non-affine-transforms"></a>[非アフィン変換](non-affine.md)

非アフィン変換効果を使用して oridinary を超えます。

## <a name="3d-rotation"></a>[3 次元回転](3d-rotation.md)

非アフィン変換を使用して、3D 空間内の2D オブジェクトを回転します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)