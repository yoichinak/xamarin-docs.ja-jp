---
title: 単純なアニメーション Xamarin.Forms
description: ViewExtensions クラスは、単純なアニメーションを構築するために使用できる拡張メソッドを提供します。 この記事では、ViewExtensions クラスを使用してアニメーションを作成およびキャンセルする方法について説明します。
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4078fa50e2e86d80e1e5b35321223deea5adeab7
ms.sourcegitcommit: 424eaef56fd2933c98e72f1d3e7ac71730fe4835
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98758059"
---
# <a name="simple-animations-in-no-locxamarinforms"></a>単純なアニメーション Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_ViewExtensions クラスは、単純なアニメーションを構築するために使用できる拡張メソッドを提供します。この記事では、ViewExtensions クラスを使用してアニメーションを作成およびキャンセルする方法について説明します。_

クラスには、 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 単純なアニメーションを作成するために使用できる次の拡張メソッドが用意されています。

- [ `CancelAnimations` ] (xref: Xamarin.Forms 。ViewExtensions。 CancelAnimations ( Xamarin.Forms .VisualElement)) アニメーションをキャンセルします。
- [ `FadeTo` ] (xref: Xamarin.Forms 。ViewExtensions. FadeTo ( Xamarin.Forms .VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- [ `RelScaleTo` ] (xref: Xamarin.Forms 。ViewExtensions. Rel拡張性 ( Xamarin.Forms .VisualElement、system.string、system.string、 Xamarin.Forms 。イージング) を適用すると、アニメーションのインクリメントがのプロパティに適用さ [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`VisualElement`](xref:Xamarin.Forms.VisualElement) れます。
- [ `RotateTo` ] (xref: Xamarin.Forms 。ViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- [ `RelRotateTo` ] (xref: Xamarin.Forms 。ViewExtensions Xamarin.Forms . RelRotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング) を適用すると、アニメーションのインクリメントがのプロパティに適用さ [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`VisualElement`](xref:Xamarin.Forms.VisualElement) れます。
- [ `RotateXTo` ] (xref: Xamarin.Forms 。ViewExtensions Xamarin.Forms . RotateXTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- [ `RotateYTo` ] (xref: Xamarin.Forms 。ViewExtensions Xamarin.Forms . RotateYTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- `ScaleXTo`[`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- `ScaleYTo`[`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)のプロパティをアニメーション化 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。
- [ `TranslateTo` ] (xref: Xamarin.Forms 。ViewExtensions Xamarin.Forms . TranslateTo ()VisualElement, system.string, system.string, system.string, Xamarin.Forms です。イージング)) [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) のプロパティとプロパティをアニメーション化し [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

既定では、各アニメーションは250ミリ秒かかります。 ただし、アニメーションの作成時には、各アニメーションの継続時間を指定できます。

> [!NOTE]
> クラスには [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 、[ `LayoutTo` ] (xref: が用意されて Xamarin.Forms います。ViewExtensions。 LayoutTo ( Xamarin.Forms .VisualElement、 Xamarin.Forms 。四角形、system.string、 Xamarin.Forms 。イージング)) 拡張メソッド。 ただし、このメソッドは、サイズと位置の変更を含むレイアウト状態間の遷移をアニメーション化するためにレイアウトで使用されることを意図しています。 したがって、サブクラスでのみ使用する必要があり [`Layout`](xref:Xamarin.Forms.Layout) ます。

クラスのアニメーション拡張メソッド [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) はすべて非同期であり、オブジェクトを返し `Task<bool>` ます。 `false`アニメーションが完了した場合、およびアニメーションがキャンセルされた場合、戻り値はになり `true` ます。 そのため、アニメーションメソッドは通常、演算子と共に使用する必要があり `await` ます。これにより、アニメーションがいつ完了したかを簡単に判断できるようになります。 さらに、前のメソッドの完了後に実行される後続のアニメーションメソッドを使用してシーケンシャルアニメーションを作成できるようになります。 詳細については、「 [複合アニメーション](#compound-animations)」を参照してください。

バックグラウンドでアニメーションを完了させる必要がある場合は、 `await` 演算子を省略できます。 このシナリオでは、アニメーションを開始した後、アニメーションの拡張メソッドを使用してアニメーションがバックグラウンドで発生するようになります。 この操作は、複合アニメーションを作成するときにを利用できます。 詳細については、「 [複合アニメーション](#composite-animations)」を参照してください。

オペレーターの詳細については `await` 、「 [Async Support の概要](~/cross-platform/platform/async.md)」を参照してください。

## <a name="single-animations"></a>1つのアニメーション

の各拡張メソッドは、 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 一定の期間にわたってプロパティをある値から別の値に徐々に変更する1つのアニメーション操作を実装します。 このセクションでは、各アニメーション操作について説明します。

### <a name="rotation"></a>回転

次のコード例は、[ `RotateTo` ] (xref: を使用する方法を示して Xamarin.Forms います。ViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドのプロパティをアニメーション化するメソッド [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Image`](xref:Xamarin.Forms.Image) 。

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image) 2 秒 (2000 ミリ秒) を超えて最大360度回転してインスタンスをアニメーション化します。 [ `RotateTo` ] (Xref: Xamarin.FormsViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドは、アニメーションの開始時に現在のプロパティ値を取得し、 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) その値から最初の引数 (360) に回転します。 アニメーションが完了すると、イメージの [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) プロパティは0にリセットされます。 これにより、 `Rotation` アニメーションが終了した後にプロパティが360に残されないようにすることで、追加の回転を防ぐことができます。

次のスクリーンショットは、各プラットフォームでの進行中の回転を示しています。

![回転アニメーション](simple-images/rotateto.png)

> [!NOTE]
> [ `RotateTo` ] (Xref: Xamarin.Forms .ViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドにも [ `RotateXTo` ] (xref: があり Xamarin.Forms ます。ViewExtensions Xamarin.Forms . RotateXTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング) と [ `RotateYTo` ] (xref: Xamarin.Forms .ViewExtensions Xamarin.Forms . RotateYTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング) [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) 。プロパティとプロパティをそれぞれアニメーション化するメソッド。

### <a name="relative-rotation"></a>相対回転

次のコード例は、[ `RelRotateTo` ] (xref: を使用する方法を示して Xamarin.Forms います。ViewExtensions Xamarin.Forms . RelRotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドのプロパティを増分または縮小するメソッド [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Image`](xref:Xamarin.Forms.Image) 。

```csharp
await image.RelRotateTo (360, 2000);
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image) 2 秒 (2000 ミリ秒) を越えて開始位置から360°を回転して、インスタンスをアニメーション化します。 [ `RelRotateTo` ] (Xref: Xamarin.FormsViewExtensions Xamarin.Forms . RelRotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドは、アニメーションの開始時に現在のプロパティ値を取得し、 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) その値から1番目の引数 (360) に回転します。 これにより、各アニメーションは常に開始位置から360°回転します。 したがって、アニメーションが既に進行中のときに新しいアニメーションが呼び出されると、現在の位置から開始され、360度のインクリメントではない位置で終了することがあります。

次のスクリーンショットは、各プラットフォームで実行されている相対的な回転を示しています。

![相対回転アニメーション](simple-images/relrotateto.png)

### <a name="scaling"></a>Scaling

次のコード例は、メソッドを使用して [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) のプロパティをアニメーション化する方法を示してい [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) ます。

```csharp
await image.ScaleTo (2, 2000);
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image) 2 秒 (2000 ミリ秒) を超えてサイズを2倍にスケールアップして、インスタンスをアニメーション化します。 メソッドは、 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) アニメーションの開始時に現在のプロパティ値 (既定値は 1) を取得し、その値から最初の引数 (2) にスケーリングします。 これにより、イメージのサイズがサイズの2倍になるようになります。

次のスクリーンショットは、各プラットフォームでのスケーリングの進行状況を示しています。

![スケーリング (アニメーションを)](simple-images/scaleto.png)

> [!NOTE]
> メソッドに加えて、 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) `ScaleXTo` プロパティと `ScaleYTo` [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) プロパティをそれぞれアニメーション化するメソッドとメソッドもあり [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) ます。

### <a name="relative-scaling"></a>相対スケーリング

次のコード例は、[ `RelScaleTo` ] (xref: を使用する方法を示して Xamarin.Forms います。ViewExtensions. Rel拡張性 ( Xamarin.Forms .VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドのプロパティをアニメーション化するメソッド [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) 。

```csharp
await image.RelScaleTo (2, 2000);
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image) 2 秒 (2000 ミリ秒) を超えてサイズを2倍にスケールアップして、インスタンスをアニメーション化します。 [ `RelScaleTo` ] (Xref: Xamarin.FormsViewExtensions. Rel拡張性 ( Xamarin.Forms .VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドは、 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) アニメーションの開始時に現在のプロパティ値を取得し、その値から1番目の引数 (2) にスケーリングします。 これにより、各アニメーションは常に開始位置から2のスケーリングになります。

### <a name="scaling-and-rotation-with-anchors"></a>アンカーを使用したスケーリングと回転

プロパティとプロパティは、 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) プロパティとプロパティのスケールまたは回転の中心を設定し [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) ます。 そのため、値は [ `RotateTo` ] (xref: にも影響します Xamarin.Forms 。ViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) と [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) メソッド。

[`Image`](xref:Xamarin.Forms.Image)レイアウトの中央に配置されたを指定した場合、次のコード例では、プロパティを設定することによって、レイアウトの中心を中心に画像を回転してい [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) ます。

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

レイアウトの中心を中心にインスタンスを回転させるには、 [`Image`](xref:Xamarin.Forms.Image) [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) プロパティとプロパティをの [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 幅と高さに対して相対的な値に設定する必要があり `Image` ます。 この例では、の中心 `Image` がレイアウトの中心になるように定義されているため、 `AnchorX` 0.5 の既定値は変更する必要がありません。 ただし、 `AnchorY` プロパティは、の上部からレイアウトの中心点までの値に再定義され `Image` ます。 これにより、 `Image` 次のスクリーンショットに示すように、によってレイアウトの中心点を中心に360度の完全な回転が行われます。

![アンカーを使用した回転アニメーション](simple-images/rotate-anchors.png)

### <a name="translation"></a>翻訳

次のコード例は、[ `TranslateTo` ] (xref: を使用する方法を示して Xamarin.Forms います。ViewExtensions Xamarin.Forms . TranslateTo ()VisualElement, system.string, system.string, system.string, Xamarin.Forms です。イージング)) メソッド [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) のプロパティとプロパティをアニメーション化するメソッド [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) [`Image`](xref:Xamarin.Forms.Image) 。

```csharp
await image.TranslateTo (-100, -100, 1000);
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image) 1 秒 (1000 ミリ秒) で水平方向および垂直方向に変換することにより、インスタンスをアニメーション化します。 [ `TranslateTo` ] (Xref: Xamarin.FormsViewExtensions Xamarin.Forms . TranslateTo ()VisualElement, system.string, system.string, system.string, Xamarin.Forms です。イージング)) メソッドを実行すると、イメージは100ピクセルの左および100ピクセルの高さに同時に変換されます。 これは、1番目と2番目の引数が両方とも負の値であるためです。 正の数値を指定すると、イメージが右、下に変換されます。

次のスクリーンショットは、各プラットフォームで実行されている変換を示しています。

![翻訳アニメーション](simple-images/translateto.png)

> [!NOTE]
> 要素が最初に画面にレイアウトされてから画面に変換された場合、変換後、要素の入力レイアウトは画面に表示されず、ユーザーは操作できません。 そのため、ビューを最終的な位置に配置し、必要な変換を実行することをお勧めします。

### <a name="fading"></a>フェード

次のコード例は、[ `FadeTo` ] (xref: を使用する方法を示して Xamarin.Forms います。ViewExtensions. FadeTo ( Xamarin.Forms .VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドのプロパティをアニメーション化するメソッド [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) [`Image`](xref:Xamarin.Forms.Image) 。

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image) 4 秒 (4000 ミリ秒) でフェードして、インスタンスをアニメーション化します。 [ `FadeTo` ] (Xref: Xamarin.FormsViewExtensions. FadeTo ( Xamarin.Forms .VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドは、アニメーションの開始時に現在のプロパティ値を取得し、 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) その値から最初の引数 (1) にフェードインします。

次のスクリーンショットは、各プラットフォームでのフェードインを示しています。

![フェードアニメーション](simple-images/fadeto.png)

## <a name="compound-animations"></a>複合アニメーション

複合アニメーションは、アニメーションの連続した組み合わせであり、 `await` 次のコード例に示すように、演算子を使用して作成できます。

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

この例では、は [`Image`](xref:Xamarin.Forms.Image) 6 秒 (6000 ミリ秒) に変換されます。 の変換では `Image` 、 `await` 各アニメーションが連続して実行されることを示す演算子と共に、5つのアニメーションが使用されます。 そのため、前のメソッドが完了した後で、後続のアニメーションメソッドが実行されます。

## <a name="composite-animations"></a>複合アニメーション

複合アニメーションは、2つ以上のアニメーションが同時に実行されるアニメーションの組み合わせです。 複合アニメーションを作成するには、次のコード例に示すように、待機中のアニメーションと待機しないアニメーションを混在させます。

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

この例では、 [`Image`](xref:Xamarin.Forms.Image) がスケールされ、同時に4秒 (4000 ミリ秒) に回転します。 のスケーリングでは、 `Image` 回転と同時に発生する2つの連続するアニメーションが使用されます。 [ `RotateTo` ] (Xref: Xamarin.FormsViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドは、演算子なしで実行され、 `await` 最初のアニメーションを開始してすぐに制御を戻し [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) ます。 最初のメソッド呼び出しの演算子は、 `await` `ScaleTo` `ScaleTo` 最初のメソッドの呼び出しが完了するまで、2番目のメソッド呼び出しを遅らせ `ScaleTo` ます。 この時点で、 `RotateTo` アニメーションは半分の完了で、は `Image` 180 °回転します。 最後の2秒 (2000 ミリ秒) 中に、2番目の `ScaleTo` アニメーションと `RotateTo` アニメーションの両方が完了します。

### <a name="running-multiple-asynchronous-methods-concurrently"></a>複数の非同期メソッドの同時実行

`static` `Task.WhenAny` メソッドと `Task.WhenAll` メソッドは、複数の非同期メソッドを同時に実行するために使用されるため、複合アニメーションの作成に使用できます。 どちらのメソッドもオブジェクトを返し、 `Task` それぞれがオブジェクトを返すメソッドのコレクションを受け取り `Task` ます。 メソッドは、 `Task.WhenAny` 次のコード例に示すように、コレクション内のいずれかのメソッドの実行が完了すると完了します。

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

この例では、 `Task.WhenAny` メソッド呼び出しに2つのタスクが含まれています。 最初のタスクは、イメージを4秒 (4000 ミリ秒) 上に回転させ、2番目のタスクはイメージを2秒 (2000 ミリ秒) 上に拡大します。 2番目のタスクが完了すると、 `Task.WhenAny` メソッドの呼び出しが完了します。 ただし、[ `RotateTo` ] (xref: Xamarin.FormsViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) メソッドはまだ実行中で、2番目の [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) メソッドは開始できます。

メソッドは、 `Task.WhenAll` 次のコード例に示すように、コレクション内のすべてのメソッドが完了すると完了します。

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

この例では、 `Task.WhenAll` メソッド呼び出しに3つのタスクが含まれており、それぞれが10分を超えて実行されます。 各 `Task` では、[ `RotateTo` ] (xref:. に対して、307ローテーションの360度の回転数が異なり Xamarin.Forms ます。ViewExtensions Xamarin.Forms . RotateTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング))、251 `RotateXTo` (xref: Xamarin.Forms .ViewExtensions Xamarin.Forms . RotateXTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング)) と199の回転を [ `RotateYTo` ] (xref: Xamarin.Forms .ViewExtensions Xamarin.Forms . RotateYTo ()VisualElement、system.string、system.string、 Xamarin.Forms 。イージング))。 これらの値は素数であるため、回転が同期されていないことが保証されるため、繰り返しパターンになることはありません。

次のスクリーンショットは、各プラットフォームで進行中の複数の回転を示しています。

![複合アニメーション](simple-images/multiple-rotations.png)

## <a name="canceling-animations"></a>アニメーションの取り消し

アプリケーションでは、[ `CancelAnimations` ] (xref: を呼び出して1つ以上のアニメーションをキャンセルできます Xamarin.Forms 。ViewExtensions。 CancelAnimations ( Xamarin.Forms .VisualElement)): 次のコード例に示すように、拡張メソッドを使用します。

```csharp
image.CancelAnimations();
```

これにより、インスタンスで現在実行中のすべてのアニメーションが直ちにキャンセルされ [`Image`](xref:Xamarin.Forms.Image) ます。

## <a name="summary"></a>まとめ

この記事では、クラスを使用してアニメーションを作成およびキャンセルする方法について説明し [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) ます。 このクラスには、インスタンスを回転、拡大縮小、変換、およびフェードする単純なアニメーションを作成するために使用できる拡張メソッドが用意されて [`VisualElement`](xref:Xamarin.Forms.VisualElement) います。

## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [基本的なアニメーション (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
