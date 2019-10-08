---
title: Xamarin.Forms での単純なアニメーション
description: ViewExtensions クラスは、単純なアニメーションを作成するために使用できる拡張メソッドを提供します。 この記事では、作成および ViewExtensions クラスを使用してアニメーションのキャンセルを示します。
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 26068973fd91d5229b7e2108f5df46ae4476ef74
ms.sourcegitcommit: 4cf434b126eb7df6b2fd9bb1d71613bf2b6aac0e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997200"
---
# <a name="simple-animations-in-xamarinforms"></a>Xamarin.Forms での単純なアニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_ViewExtensions クラスは、単純なアニメーションを作成するために使用できる拡張メソッドを提供します。この記事では、作成および ViewExtensions クラスを使用してアニメーションのキャンセルを示します。_

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスは、単純なアニメーションを作成するために使用できる次の拡張メソッドを提供します。

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化、 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)と[ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) アニメーション化、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化された増分増加または減少に適用されます、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化された増分増加または減少に適用されます、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化、 [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化、 [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーション化、 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity)のプロパティを[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。

既定では、それぞれのアニメーションを 250 ミリ秒となります。 ただし、アニメーションを作成するときに、それぞれのアニメーションの有効期間を指定できます。

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスも含まれています、 [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))メソッド、アニメーションをキャンセルするために使用できます。

> [!NOTE]
> [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスには、 [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))拡張メソッド。 ただし、このメソッドは、サイズを格納し、変更を配置するレイアウト状態間の遷移をアニメーション化するレイアウトで使用されるものです。 したがって、この属性にのみ使用する必要があります[ `Layout` ](xref:Xamarin.Forms.Layout)サブクラスです。

アニメーションの拡張メソッドにおいて、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスには、すべて非同期に戻り、`Task<bool>`オブジェクト。 戻り値は`false`アニメーションが完了すると、および`true`アニメーションが取り消された場合。 そのため、アニメーションのメソッドで使用が通常、`await`演算子は、簡単にアニメーションが完了したタイミングを決定することができます。 さらにが可能になり、前のメソッドが完了した後に実行する後続のアニメーションの方法で順次アニメーションを作成します。 詳細については、次を参照してください。[複合アニメーション](#compound)します。

アニメーションをできるようにする必要がある場合、バック グラウンドで完了、`await`演算子を省略できます。 このシナリオで、迅速に、バック グラウンドで発生しているアニメーションを使用して、アニメーションを開始した後に、アニメーションの拡張機能メソッドが返されます。 複合のアニメーションを作成するときにより、この操作に利用を実行できます。 詳細については、次を参照してください。[複合アニメーション](#composite)します。

詳細については、`await`演算子を参照してください[非同期サポートの概要](~/cross-platform/platform/async.md)します。

## <a name="single-animations"></a>1 つのアニメーション

内の各拡張メソッド、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)期間にわたっては 1 つの値から別の値のプロパティを変更段階的にする 1 つのアニメーションの操作を実装します。 このセクションでは、アニメーションの各操作について説明します。

### <a name="rotation"></a>[回転]

次のコード例に示しますを使用して、 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))をアニメーション化するメソッド、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

このコードをアニメーション化、 [ `Image` ](xref:Xamarin.Forms.Image) 2 秒 (2000 ミリ秒) を超えた最大 360 度の回転を指定してインスタンス。 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは現在、取得[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)プロパティは、アニメーションの開始値し、その値から最初の引数 (360) に、回転します。 アニメーションが完了、イメージの[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)プロパティが 0 にリセットされます。 これにより、`Rotation`アニメーションが完了した、追加の回転をできなくなるプロパティの 360 まましません。

次のスクリーン ショットでは、各プラットフォームで実行中の回転角度を表示します。

![](simple-images/rotateto.png "回転アニメーション")

### <a name="relative-rotation"></a>相対の回転

次のコード例に示しますを使用して、 [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))増分を拡大または縮小する方法、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

このコードをアニメーション化、 [ `Image` ](xref:Xamarin.Forms.Image) 2 秒 (2000 ミリ秒) を開始位置からの 360 度の回転を指定してインスタンス。 [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは現在、取得[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)プロパティが、アニメーションの開始値し、その値から最初の引数 (360) を加えた値に、回転します。 これにより、それぞれのアニメーションは開始位置からの 360 度回転で常にはなります。 そのため、新しいアニメーションはアニメーションが既に進行中のときに呼び出される場合、現在の位置から開始され、360 度の増分ではない位置になる可能性があります。

次のスクリーン ショットは、各プラットフォームでの処理中の相対的な回転を表示します。

![](simple-images/relrotateto.png "相対回転アニメーション")

### <a name="scaling"></a>スケーリング

次のコード例に示しますを使用して、 [ `ScaleTo` ](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)をアニメーション化するメソッド、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

このコードをアニメーション化、 [ `Image` ](xref:Xamarin.Forms.Image)インスタンスのスケール アップで 2 倍のサイズを超える 2 秒 (2000 ミリ秒)。 [ `ScaleTo` ](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッドは現在、取得[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)アニメーション、および最初の引数 (2) にその値から、スケールの開始のプロパティの値 (既定値の 1)。 イメージのサイズを 2 倍のサイズに拡大の効果があります。

次のスクリーン ショットは、各プラットフォームでの進行状況でのスケーリングを示しています。

![](simple-images/scaleto.png "アニメーションのスケーリング")

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスも定義[ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX)と[ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY)プロパティ、拡張することが、`VisualElement`異なる方法で、水平および垂直方向。 これらのプロパティをアニメーション化できる、 [ `Animation` ](xref:Xamarin.Forms.Animation)クラス。 詳細については、次を参照してください。 [Xamarin.Forms でのカスタム アニメーション](custom.md)します。

### <a name="relative-scaling"></a>相対スケール

次のコード例に示しますを使用して、 [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))をアニメーション化するメソッド、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

このコードをアニメーション化、 [ `Image` ](xref:Xamarin.Forms.Image)インスタンスのスケール アップで 2 倍のサイズを超える 2 秒 (2000 ミリ秒)。 [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは現在、取得[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)アニメーション、および最初の引数 (2) を加えた値をその値から、スケールの開始のプロパティの値。 これにより、それぞれのアニメーションは常に 2 を開始位置からのスケーリングします。

### <a name="scaling-and-rotation-with-anchors"></a>拡大縮小および回転のアンカー

[ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX)と[ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティの拡大縮小や回転の中心を設定する、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)と[ `Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティ。 そのため、その値にも影響、 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))と[ `ScaleTo` ](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッド。

指定された、 [ `Image` ](xref:Xamarin.Forms.Image)ですが、レイアウトの中央に配置されている、次のコード例に示しますを設定して、イメージ、レイアウトの中心を回転、 [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティ:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

回転する、 [ `Image` ](xref:Xamarin.Forms.Image) 、レイアウトの中心付近のインスタンス、 [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX)と[ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY)した値にプロパティを設定する必要があります幅と高さの基準とした、`Image`します。 この例では、中心で、 `Image` 、レイアウトの中心に定義されているため、既定値`AnchorX`値 0.5 は変更する必要ありません。 ただし、`AnchorY`プロパティが再定義する値の先頭から、`Image`レイアウトの中心点にします。 これにより、`Image`レイアウトの中心点を中心に 360 度の完全な回転を次のスクリーン ショットに示すようになります。

![](simple-images/rotate-anchors.png "アンカーの回転アニメーション")

### <a name="translation"></a>変換

次のコード例に示しますを使用して、 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))をアニメーション化するメソッド、 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)と[ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) のプロパティ[`Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

このコードをアニメーション化、 [ `Image` ](xref:Xamarin.Forms.Image)変換することによって、水平および垂直方向に 1 秒 (1000 ミリ秒) のインスタンス。 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、同時に、左にイメージ 100 ピクセル、100 ピクセル上方向に変換します。 これは、最初と 2 番目の引数が両方の負の数値であるためです。 正の数値を提供する、右、下の画像に変換します。

次のスクリーン ショットは、各プラットフォームでの処理中の翻訳を表示します。

![](simple-images/translateto.png "翻訳のアニメーション")

> [!NOTE]
> 要素が画面外最初に配置され、画面上に変換し場合、は、変換後、要素の入力のレイアウトが画面外は、ユーザー操作ことはできません。 そのため、その最後の位置でビューをレイアウトして、実行される変換し、必要なことをお勧めします。

### <a name="fading"></a>フェード

次のコード例に示しますを使用して、 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))をアニメーション化するメソッド、 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

このコードをアニメーション化、 [ `Image` ](xref:Xamarin.Forms.Image)で、フェードインを 4 秒 (4000 ミリ秒) 間でのインスタンス。 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは現在、取得[ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity)アニメーション、および最初の引数 (1) にその値からで、フェードの開始のプロパティの値。

次のスクリーン ショットは、各プラットフォームでの処理中のフェードを表示します。

![](simple-images/fadeto.png "フェード アニメーション")

<a name="compound" />

## <a name="compound-animations"></a>複合のアニメーション

複合アニメーション アニメーション、シーケンシャルを組み合わせたものですし、使用して作成できます、`await`オペレーターは、次のコード例で示した。

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

この例で、 [ `Image` ](xref:Xamarin.Forms.Image) 6 秒 (6000 のミリ秒単位) を超えるに変換されます。 変換、`Image`で 5 つのアニメーションを使用して、`await`それぞれのアニメーションを順番に実行を示す演算子。 そのため、次のアニメーションのメソッドは、前のメソッドが完了した後を実行します。

<a name="composite" />

## <a name="composite-animations"></a>複合のアニメーション

複合のアニメーションは、2 つまたは複数のアニメーションを同時に実行しているアニメーションの組み合わせです。 次のコード例で示した、待機中と非待機のアニメーションを組み合わせることで合成アニメーションを作成できます。

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

この例で、 [ `Image` ](xref:Xamarin.Forms.Image)スケーリングおよび 4 秒 (4000 ミリ秒単位) を同時に回転します。 スケーリング、`Image`回転と同時に発生する 2 つの連続アニメーションを使用します。 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))せずにメソッドを実行、`await`演算子最初がすぐに返されます[ `ScaleTo` ](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)アニメーションを開始します。 `await`最初の演算子`ScaleTo`メソッドの呼び出し、2 つ目の遅延`ScaleTo`最初までメソッドの呼び出し`ScaleTo`メソッドの呼び出しが完了します。 この時点で、`RotateTo`アニメーションが完了したことの半分と`Image`180 度回転されます。 最後の 2 秒 (2000 ミリ秒) 間、2 つ目`ScaleTo`アニメーションと`RotateTo`アニメーション両方を完了します。

### <a name="running-multiple-asynchronous-methods-concurrently"></a>複数の非同期メソッドを同時に実行

`static` `Task.WhenAny`と`Task.WhenAll`メソッドが同時に複数の非同期メソッドの実行に使用および合成アニメーションを作成するために使用できます。 両方のメソッドが返す、`Task`オブジェクトし、メソッドのコレクションを受け取る各戻り、`Task`オブジェクト。 `Task.WhenAny`メソッドの完了がコレクション内の任意のメソッドの実行が完了すると、次のコード例で示した。

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

この例で、`Task.WhenAny`メソッドの呼び出しには、2 つのタスクが含まれています。 最初のタスクは 4 秒 (4000 のミリ秒単位)、画像を回転し、2 番目のタスクは 2 秒 (2000 ミリ秒) を超えるイメージを縮小します。 2 番目のタスクが完了したら、`Task.WhenAny`メソッドの呼び出しが完了するとします。 ただし、場合でも、 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドがまだ実行されている 2 つ目[ `ScaleTo` ](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッドを開始できます。

`Task.WhenAll`メソッドの完了がコレクション内のすべてのメソッドが完了したら、次のコード例で示した。

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

この例で、`Task.WhenAll`メソッドの呼び出しに 10 分以上実行されるそれぞれの 3 つのタスクが含まれています。 各`Task`360 度の回転 – の 307 の回転のさまざまな数は、 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、251 の回転の[ `RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))との 199 回転[ `RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). これらの値は、素数、そのため、回転が同期されていないし、繰り返しのパターンが発生しないためのことを確認します。

次のスクリーン ショットは、各プラットフォームでの処理中の複数の回転を表示します。

![](simple-images/multiple-rotations.png "複合のアニメーション")

## <a name="canceling-animations"></a>アニメーションのキャンセル

アプリケーションへの呼び出しに 1 つまたは複数のアニメーションを取り消すことができます、 `static` [ `ViewExtensions.CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))メソッドを次のコード例で示されています。

```csharp
ViewExtensions.CancelAnimations (image);
```

これで現在実行されているすべてのアニメーションがすぐに取り消されます、 [ `Image` ](xref:Xamarin.Forms.Image)インスタンス。

## <a name="summary"></a>まとめ

この記事で説明を作成してを使用してアニメーションをキャンセル、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラス。 このクラスは、回転、拡大縮小、平行移動、およびフェードする単純なアニメーションの作成に使用できる拡張メソッドを提供します。 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)インスタンス。

## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [基本的なアニメーション (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
