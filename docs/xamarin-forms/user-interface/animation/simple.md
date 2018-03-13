---
title: "単純なアニメーション"
description: "ViewExtensions クラスでは、単純なアニメーションを構築するために使用できる拡張メソッドを提供します。 この記事は、作成、ViewExtensions クラスを使用してアニメーションを取り消す方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: fb7ca216978e4c890349a44b07d5a383e9ca2384
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="simple-animations"></a>単純なアニメーション

_ViewExtensions クラスでは、単純なアニメーションを構築するために使用できる拡張メソッドを提供します。この記事は、作成、ViewExtensions クラスを使用してアニメーションを取り消す方法について説明します。_


[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスは、単純なアニメーションを作成するために使用する次の拡張メソッドを提供します。

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化、 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)と[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) アニメーション化、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化された増分を上げたり下げたりするには適用されます、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化された増分を上げたり下げたりするには適用されます、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化、 [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化、 [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) アニメーション化、 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/)のプロパティ、 [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。

既定では、それぞれのアニメーションを 250 ミリ秒となります。 ただし、アニメーションを作成するときに、それぞれのアニメーションの期間を指定できます。

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスも含まれます、 [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/)アニメーションのキャンセルに使用できるメソッドです。

> [!NOTE]
> [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスを提供する[ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/)拡張メソッド。 ただし、このメソッドは、サイズが含まれており、変更を配置するレイアウト状態間の遷移をアニメーション化するレイアウトで使用されるものです。 したがって、この属性にのみ使用する必要があります[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)サブクラスです。

アニメーションの拡張メソッドにおいて、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスは、非同期および戻り値のすべて、`Task<bool>`オブジェクト。 戻り値は`false`アニメーションが完了すると、および`true`アニメーションが取り消された場合。 したがって、アニメーション メソッドで使用される通常の`await`演算子、アニメーションが完了したタイミングを容易に判別できるようにします。 さらに、しが可能になり、前のメソッドが完了した後に実行する後続のアニメーション メソッドで順次アニメーションを作成します。 詳細については、次を参照してください。[複合アニメーション](#compound)です。

アニメーションをできるようにする必要がある場合、バック グラウンドで完了、`await`演算子は省略できます。 このシナリオでは、アニメーションの拡張メソッドはバック グラウンドで発生する、アニメーションでアニメーションの開始後すぐに戻しです。 複合のアニメーションを作成する場合により、この操作に利用を実行できます。 詳細については、次を参照してください。[複合アニメーション](#composite)です。

詳細については、`await`演算子を参照してください[非同期サポートの概要](~/cross-platform/platform/async.md)です。

## <a name="single-animations"></a>1 つのアニメーション

各拡張メソッドで、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)段階的に変更するプロパティ 1 つの値から別の値、期間にわたって 1 つのアニメーションの操作を実装します。 このセクションでは、アニメーションの各操作について説明します。

### <a name="rotation"></a>[回転]

次のコード例では、使用方法を示します、 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)アニメーション化するメソッド、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

このコードをアニメーション化、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 秒 (2000 ミリ秒) を超えた 360 度までの回転を指定してインスタンス。 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドは現在、取得[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)プロパティは、アニメーションの開始値し、最初の引数 (360) にその値から、回転します。 アニメーションが後で、イメージの完了の[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)プロパティは 0 にリセットします。 これにより、`Rotation`プロパティしないまま 360 にアニメーションが終了した後、追加の回転することを防止します。

次のスクリーン ショットは、各プラットフォームで実行中に回転を表示します。

![](simple-images/rotateto.png "回転アニメーション")

### <a name="relative-rotation"></a>相対回転

次のコード例では、使用方法を示します、 [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)増分を拡大または縮小する方法、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

このコードをアニメーション化、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)インスタンスの開始位置から 360 ° の 2 秒 (2000 ミリ秒) を超えるを回転させるとします。 [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドは現在、取得[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)プロパティ、アニメーションの開始値し、値に加え、最初の引数 (360) にその値から、回転します。 これにより、それぞれのアニメーションが常になる開始位置からの 360 度回転します。 そのため、アニメーションは、既に進行中は新しいアニメーションが呼び出される場合、現在の位置から開始され、360 度の増分値ではない位置になる可能性があります。

次のスクリーン ショットは、各プラットフォームで処理中の相対的な回転を表示します。

![](simple-images/relrotateto.png "相対回転アニメーション")

### <a name="scaling"></a>スケーリング

次のコード例では、使用方法を示します、 [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)アニメーション化するメソッド、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

このコードをアニメーション化、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)のスケール アップによって 2 倍のサイズを 2 秒 (2000 ミリ秒) を超えるインスタンス。 [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)メソッドは現在、取得[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)アニメーション、および最初の引数 (2) にその値から、スケールの開始のプロパティの値 (1 の既定値)。 これにより、イメージのサイズを 2 倍のサイズに合わせて拡張の効果があります。

次のスクリーン ショットは、各プラットフォームで実行中のスケーリングを示しています。

![](simple-images/scaleto.png "アニメーションのスケーリング")

### <a name="relative-scaling"></a>相対スケール

次のコード例では、使用方法を示します、 [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)アニメーション化するメソッド、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

このコードをアニメーション化、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)のスケール アップによって 2 倍のサイズを 2 秒 (2000 ミリ秒) を超えるインスタンス。 [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドは現在、取得[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)アニメーション、および (2) の最初の引数を加えた値をその値から、スケールの開始のプロパティの値。 これにより、それぞれのアニメーションが常になる 2 の開始位置からのスケーリングします。

### <a name="scaling-and-rotation-with-anchors"></a>対応するスケーリングとアンカーによる回転

[ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)と[ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)プロパティは、スケールまたは回転の中心を設定、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)と[ `Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)プロパティです。 そのため、その値にも影響する、 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)と[ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)メソッドです。

指定された、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)ですが、レイアウトの中央に配置されている、次のコード例を設定して、レイアウトを中心に回転を示しています、 [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)プロパティ:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

回転する、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 、レイアウトの中心のインスタンス、 [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)と[ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)プロパティは、ある値に設定する必要があります幅と高さの基準とした、`Image`です。 この例では、中心で、 `Image` 、レイアウトの中央に定義されているしているため、既定`AnchorX`0.5 の値に変更が不要です。 ただし、`AnchorY`の上端からの値を指定するプロパティが再定義、`Image`レイアウトの中心点にします。 これにより、`Image`完全に 360 度、レイアウトの中心点の周りの回転は、次のスクリーン ショットに示すようにします。

![](simple-images/rotate-anchors.png "アンカーの回転アニメーション")

### <a name="translation"></a>変換

次のコード例では、使用方法を示します、 [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)アニメーション化するメソッド、 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)と[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) のプロパティ[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

このコードをアニメーション化、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)変換することによって、水平方向および垂直方向に 1 秒 (1000 ミリ秒) を超えるインスタンス。 [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドは、同時に、イメージ 100 ピクセルを左に、100 ピクセル上方向に変換します。 これは、1 番目と 2 番目の引数が両方の負の数値であるためです。 正の数値を提供するには、右、下の画像は変換します。

次のスクリーン ショットは、各プラットフォームでの処理中の翻訳を表示します。

![](simple-images/translateto.png "翻訳のアニメーション")

> [!NOTE]
> 要素が最初に画面外レイアウト、画面上に変換し、場合は、翻訳した後、要素の入力レイアウト画面外でき、ユーザーが対話ことはできません。 そのため、ビューを最後の位置にレイアウトする必要があり、実行される変換し、必要なことをお勧めします。

### <a name="fading"></a>フェード

次のコード例では、使用方法を示します、 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)アニメーション化するメソッド、 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

このコードをアニメーション化、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 4 秒 (4000 ミリ秒単位) を超えるフェードインによるインスタンス。 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドは現在、取得[ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/)アニメーション、および最初の引数 (1) にその値ので、フェードの開始のプロパティの値。

次のスクリーン ショットは、各プラットフォームで実行中、フェードを表示します。

![](simple-images/fadeto.png "フェード アニメーション")

<a name="compound" />

## <a name="compound-animations"></a>複合のアニメーション

複合アニメーション、シーケンシャル、アニメーションとを組み合わせた使用して作成できます、`await`演算子を次のコード例で示したようにします。

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

この例では、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 6 秒 (6000 のミリ秒単位) を超えるに変換されます。 変換、`Image`を 5 つのアニメーションを使用して、`await`各アニメーションが順番に実行を示す演算子。 そのため、次のアニメーションのメソッドは、前のメソッドが完了した後を実行します。

<a name="composite" />

## <a name="composite-animations"></a>複合のアニメーション

複合のアニメーションは、2 つ以上のアニメーションを同時に実行しているアニメーションの組み合わせです。 次のコード例で示したように、待機中と非待機のアニメーションが混在して複合アニメーションを作成できます。

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

この例では、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)は拡大縮小され、同時に 4 秒 (4000 ミリ秒単位) を超えるを回転します。 スケーリング、`Image`回転と同時期に発生する 2 つの連続したアニメーションを使用します。 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)せずにメソッドを実行、`await`演算子を最初に、すぐに返します[ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)アニメーションを開始します。 `await`最初の演算子`ScaleTo`メソッドの呼び出し、2 つ目の遅延`ScaleTo`最初までメソッドの呼び出し`ScaleTo`メソッドの呼び出しが完了します。 この時点で、`RotateTo`アニメーションが完了した方法半分と`Image`180 度回転になります。 最後の 2 秒 (2000 ミリ秒) の中に、2 つ目`ScaleTo`アニメーションと`RotateTo`アニメーション両方を完了します。

### <a name="running-multiple-asynchronous-methods-concurrently"></a>複数の非同期メソッドを同時に実行します。

`static` `Task.WhenAny`と`Task.WhenAll`メソッドが複数の非同期メソッドを同時に、実行に使用して、複合アニメーションを作成するために使用できます。 両方のメソッドが返す、`Task`オブジェクトし、メソッドのコレクションを受け取る各戻り値、`Task`オブジェクト。 `Task.WhenAny`メソッドが完了するには、コレクション内の任意のメソッドの実行が完了すると次のコード例で示したようにします。

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

この例では、`Task.WhenAny`メソッドの呼び出しには、2 つのタスクが含まれています。 最初のタスクが 4 秒 (4000) (ミリ秒単位) を超えるイメージを回転し、2 番目のタスクは 2 秒 (2000 ミリ秒) を超えるイメージを縮小します。 2 番目のタスクが完了したら、`Task.WhenAny`メソッド呼び出しが完了します。 ただし、場合でも、 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドがまだ実行されている 2 つ目[ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)メソッドを開始できます。

`Task.WhenAll`メソッドが完了するには、コレクション内のすべてのメソッドが完了したら、次のコード例で示したようにします。

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

この例では、`Task.WhenAll`メソッドの呼び出しには、10 分以上が実行されるそれぞれの 3 つのタスクが含まれています。 各`Task`により異なる多数の 360 度回転 – に対して 307 循環[ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)、に対して 251 循環[ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)、およびに対して 199 循環[ `RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). これらの値は、素数、したがってようにすることは回転同期されていない繰り返しのパターンが発生しないためです。

次のスクリーン ショットは、各プラットフォームで進行中の複数の回転を表示します。

![](simple-images/multiple-rotations.png "複合のアニメーション")

## <a name="canceling-animations"></a>アニメーションのキャンセル

アプリケーションへの呼び出しに 1 つまたは複数のアニメーションを取り消すことができます、 `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/)メソッドを次のコード例で示したようにします。

```csharp
ViewExtensions.CancelAnimations (image);
```

これで現在実行されているすべてのアニメーションがすぐにキャンセルされます、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)インスタンス。

## <a name="summary"></a>まとめ

この記事の説明を作成してを使用してアニメーションを取り消し、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスです。 このクラスは、回転、拡大縮小、平行移動、およびフェードアウトする単純なアニメーションを構築するために使用できる拡張メソッドを提供[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)インスタンス。


## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [基本的なアニメーション (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
