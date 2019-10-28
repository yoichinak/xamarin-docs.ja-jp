---
title: Xamarin の単純なアニメーション
description: ViewExtensions クラスは、単純なアニメーションを構築するために使用できる拡張メソッドを提供します。 この記事では、ViewExtensions クラスを使用してアニメーションを作成およびキャンセルする方法について説明します。
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2019
ms.openlocfilehash: 116911787db128b103fb555554076704a0549db5
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959155"
---
# <a name="simple-animations-in-xamarinforms"></a>Xamarin の単純なアニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_ViewExtensions クラスは、単純なアニメーションを構築するために使用できる拡張メソッドを提供します。この記事では、ViewExtensions クラスを使用してアニメーションを作成およびキャンセルする方法について説明します。_

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスには、単純なアニメーションを作成するために使用できる次の拡張メソッドが用意されています。

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)と[`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)プロパティをアニメーション化します。
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティをアニメーション化します。
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))は、アニメーション化された増分値を[`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティに適用します。
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティをアニメーション化します。
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))は、アニメーション化された増分値を[`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティに適用します。
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)プロパティをアニメーション化します。
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)プロパティをアニメーション化します。
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)の[`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)プロパティをアニメーション化します。

既定では、各アニメーションは250ミリ秒かかります。 ただし、アニメーションの作成時には、各アニメーションの継続時間を指定できます。

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスには、アニメーションをキャンセルするために使用できる[`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))メソッドも含まれています。

> [!NOTE]
> [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスは、 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))の拡張メソッドを提供します。 ただし、このメソッドは、サイズと位置の変更を含むレイアウト状態間の遷移をアニメーション化するためにレイアウトで使用されることを意図しています。 したがって、 [`Layout`](xref:Xamarin.Forms.Layout)サブクラスでのみ使用する必要があります。

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスのアニメーション拡張メソッドはすべて非同期であり、`Task<bool>` オブジェクトを返します。 アニメーションが完了した場合、戻り値は `false`、アニメーションがキャンセルされた場合は `true` ます。 そのため、アニメーションメソッドは通常、`await` 演算子と共に使用する必要があります。これにより、アニメーションがいつ完了したかを簡単に判断できるようになります。 さらに、前のメソッドの完了後に実行される後続のアニメーションメソッドを使用してシーケンシャルアニメーションを作成できるようになります。 詳細については、「[複合アニメーション](#compound)」を参照してください。

バックグラウンドでアニメーションを完了させる必要がある場合は、`await` 演算子を省略できます。 このシナリオでは、アニメーションを開始した後、アニメーションの拡張メソッドを使用してアニメーションがバックグラウンドで発生するようになります。 この操作は、複合アニメーションを作成するときにを利用できます。 詳細については、「[複合アニメーション](#composite)」を参照してください。

`await` 演算子の詳細については、「 [Async Support の概要](~/cross-platform/platform/async.md)」を参照してください。

## <a name="single-animations"></a>1つのアニメーション

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)の各拡張メソッドは、一定の期間にわたってプロパティをある値から別の値に徐々に変更する1つのアニメーション操作を実装します。 このセクションでは、各アニメーション操作について説明します。

### <a name="rotation"></a>[回転]

次のコード例は、 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドを使用して[`Image`](xref:Xamarin.Forms.Image)の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティをアニメーション化する方法を示しています。

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

このコードは、2秒 (2000 ミリ秒) を超えて最大360度回転して、 [`Image`](xref:Xamarin.Forms.Image)インスタンスをアニメーション化します。 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、アニメーションの開始時に現在の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティ値を取得し、その値から最初の引数 (360) に回転します。 アニメーションが完了すると、画像の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティが0にリセットされます。 これにより、アニメーションが終了した後も `Rotation` プロパティが360のままにならないため、追加の回転ができなくなります。

次のスクリーンショットは、各プラットフォームでの進行中の回転を示しています。

![](simple-images/rotateto.png "Rotation Animation")

### <a name="relative-rotation"></a>相対回転

次のコード例では、 [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドを使用して、 [`Image`](xref:Xamarin.Forms.Image)の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティを増分または縮小する方法を示します。

```csharp
await image.RelRotateTo (360, 2000);
```

このコードは、2秒 (2000 ミリ秒) を越えて開始位置から360°を回転して、 [`Image`](xref:Xamarin.Forms.Image)インスタンスをアニメーション化します。 [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、アニメーションの開始時に現在の[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティ値を取得し、その値から1番目の引数 (360) に回転します。 これにより、各アニメーションは常に開始位置から360°回転します。 したがって、アニメーションが既に進行中のときに新しいアニメーションが呼び出されると、現在の位置から開始され、360度のインクリメントではない位置で終了することがあります。

次のスクリーンショットは、各プラットフォームで実行されている相対的な回転を示しています。

![](simple-images/relrotateto.png "Relative Rotation Animation")

### <a name="scaling"></a>スケーリング

次のコード例は、 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッドを使用して[`Image`](xref:Xamarin.Forms.Image)の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティをアニメーション化する方法を示しています。

```csharp
await image.ScaleTo (2, 2000);
```

このコードでは、2秒 (2000 ミリ秒) でサイズを2倍にスケールアップして、 [`Image`](xref:Xamarin.Forms.Image)インスタンスをアニメーション化します。 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッドは、アニメーションの開始時に現在の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティ値 (既定値は 1) を取得し、その値から最初の引数 (2) にスケーリングします。 これにより、イメージのサイズがサイズの2倍になるようになります。

次のスクリーンショットは、各プラットフォームでのスケーリングの進行状況を示しています。

![](simple-images/scaleto.png "Scaling Animation")

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement) クラスでも [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) プロパティと [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) プロパティが定義されます。これにより、`VisualElement` を水平方向と垂直方向に別々に拡大縮小することができます。 これらのプロパティは、 [`Animation`](xref:Xamarin.Forms.Animation)クラスを使用してアニメーション化できます。 詳細については、「 [Xamarin. Forms のカスタムアニメーション](custom.md)」を参照してください。

### <a name="relative-scaling"></a>相対スケーリング

次のコード例は、 [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドを使用して[`Image`](xref:Xamarin.Forms.Image)の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティをアニメーション化する方法を示しています。

```csharp
await image.RelScaleTo (2, 2000);
```

このコードでは、2秒 (2000 ミリ秒) でサイズを2倍にスケールアップして、 [`Image`](xref:Xamarin.Forms.Image)インスタンスをアニメーション化します。 [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、アニメーションの開始時に現在の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティ値を取得し、その値から1番目の引数 (2) にスケールします。 これにより、各アニメーションは常に開始位置から2のスケーリングになります。

### <a name="scaling-and-rotation-with-anchors"></a>アンカーを使用したスケーリングと回転

[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)プロパティと[`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティでは、 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)と[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティのスケールまたは回転の中心を設定します。 そのため、これらの値は、 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))および[`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッドにも影響します。

レイアウトの中央に配置されている[`Image`](xref:Xamarin.Forms.Image)の場合、次のコード例では、 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティを設定することによって、レイアウトの中心を中心にしたイメージの回転を示しています。

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

レイアウトの中心を中心に[`Image`](xref:Xamarin.Forms.Image)インスタンスを回転させるには、 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)プロパティと[`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティを `Image`の幅と高さに対して相対的な値に設定する必要があります。 この例では、`Image` の中心がレイアウトの中心になるように定義されているため、0.5 の既定の `AnchorX` 値は変更する必要がありません。 ただし、`AnchorY` プロパティは、`Image` の上部からレイアウトの中心点までの値に再定義されます。 これにより、次のスクリーンショットに示すように、`Image` によって、レイアウトの中心点を中心に360度の完全な回転が行われます。

![](simple-images/rotate-anchors.png "Rotation Animation with Anchors")

### <a name="translation"></a>変換

次のコード例は、 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドを使用して、 [`Image`](xref:Xamarin.Forms.Image)の[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)と[`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)プロパティをアニメーション化する方法を示しています。

```csharp
await image.TranslateTo (-100, -100, 1000);
```

このコードは、1秒 (1000 ミリ秒) で水平方向および垂直方向に変換することによって[`Image`](xref:Xamarin.Forms.Image)インスタンスをアニメーション化します。 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、イメージを100ピクセルずつ左に、100ピクセルを上に平行変換します。 これは、1番目と2番目の引数が両方とも負の値であるためです。 正の数値を指定すると、イメージが右、下に変換されます。

次のスクリーンショットは、各プラットフォームで実行されている変換を示しています。

![](simple-images/translateto.png "Translation Animation")

> [!NOTE]
> 要素が最初に画面にレイアウトされてから画面に変換された場合、変換後、要素の入力レイアウトは画面に表示されず、ユーザーは操作できません。 そのため、ビューを最終的な位置に配置し、必要な変換を実行することをお勧めします。

### <a name="fading"></a>フェード

次のコード例は、 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドを使用して[`Image`](xref:Xamarin.Forms.Image)の[`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)プロパティをアニメーション化する方法を示しています。

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

このコードでは、4秒 (4000 ミリ秒) でフェードして、 [`Image`](xref:Xamarin.Forms.Image)インスタンスをアニメーション化します。 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、アニメーションの開始時に現在の[`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)プロパティ値を取得し、その値から最初の引数 (1) にフェードインします。

次のスクリーンショットは、各プラットフォームでのフェードインを示しています。

![](simple-images/fadeto.png "Fading Animation")

<a name="compound" />

## <a name="compound-animations"></a>複合アニメーション

複合アニメーションは、アニメーションの連続した組み合わせであり、次のコード例に示すように、`await` 演算子を使用して作成できます。

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

この例では、 [`Image`](xref:Xamarin.Forms.Image)は6秒 (6000 ミリ秒) で変換されます。 `Image` の変換では、各アニメーションが連続して実行されることを示す `await` 演算子を使用して、5つのアニメーションを使用します。 そのため、前のメソッドが完了した後で、後続のアニメーションメソッドが実行されます。

<a name="composite" />

## <a name="composite-animations"></a>複合アニメーション

複合アニメーションは、2つ以上のアニメーションが同時に実行されるアニメーションの組み合わせです。 複合アニメーションを作成するには、次のコード例に示すように、待機中のアニメーションと待機しないアニメーションを混在させます。

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

この例では、 [`Image`](xref:Xamarin.Forms.Image)がスケールされ、同時に4秒 (4000 ミリ秒) に回転します。 `Image` のスケーリングでは、回転と同時に発生する2つの連続するアニメーションが使用されます。 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドは、`await` 演算子を使用せずに実行され、直ちに戻り、最初の[`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)アニメーションが開始されます。 最初の `ScaleTo` メソッド呼び出しの `await` 演算子は、最初の `ScaleTo` メソッドの呼び出しが完了するまで、2番目の `ScaleTo` メソッド呼び出しを遅らせます。 この時点で、`RotateTo` のアニメーションは半分の完了で、`Image` は180度回転されます。 最後の2秒 (2000 ミリ秒) 中に、2番目の `ScaleTo` アニメーションと `RotateTo` アニメーションの両方が完了します。

### <a name="running-multiple-asynchronous-methods-concurrently"></a>複数の非同期メソッドの同時実行

`static` の `Task.WhenAny` メソッドと `Task.WhenAll` メソッドは、複数の非同期メソッドを同時に実行するために使用されます。したがって、複合アニメーションを作成するために使用できます。 どちらのメソッドも `Task` オブジェクトを返し、それぞれが `Task` オブジェクトを返すメソッドのコレクションを受け入れます。 `Task.WhenAny` メソッドは、次のコード例に示すように、コレクション内のいずれかのメソッドの実行が完了すると完了します。

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

この例では、`Task.WhenAny` メソッドの呼び出しに2つのタスクが含まれています。 最初のタスクは、イメージを4秒 (4000 ミリ秒) 上に回転させ、2番目のタスクはイメージを2秒 (2000 ミリ秒) 上に拡大します。 2番目のタスクが完了すると、`Task.WhenAny` メソッドの呼び出しが完了します。 ただし、 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドがまだ実行されている場合でも、2番目の[`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)メソッドを開始できます。

`Task.WhenAll` メソッドは、次のコード例に示すように、コレクション内のすべてのメソッドが完了すると完了します。

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

この例では、`Task.WhenAll` メソッド呼び出しに3つのタスクが含まれており、それぞれが10分を超えて実行されます。 各 `Task` は、 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))の307ローテーション、 [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))の251回転、および[`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))の199ローテーション360の数が異なります。 これらの値は素数であるため、回転が同期されていないことが保証されるため、繰り返しパターンになることはありません。

次のスクリーンショットは、各プラットフォームで進行中の複数の回転を示しています。

![](simple-images/multiple-rotations.png "Composite Animation")

## <a name="canceling-animations"></a>アニメーションの取り消し

アプリケーションでは、次のコード例に示すように、`static` [`ViewExtensions.CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))メソッドの呼び出しを使用して1つ以上のアニメーションをキャンセルできます。

```csharp
ViewExtensions.CancelAnimations (image);
```

これにより、 [`Image`](xref:Xamarin.Forms.Image)インスタンスで現在実行されているすべてのアニメーションが直ちに取り消されます。

## <a name="summary"></a>まとめ

この記事では、 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスを使用してアニメーションを作成およびキャンセルする方法について説明します。 このクラスには、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)インスタンスを回転、拡大縮小、変換、およびフェードする単純なアニメーションを作成するために使用できる拡張メソッドが用意されています。

## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [基本的なアニメーション (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
