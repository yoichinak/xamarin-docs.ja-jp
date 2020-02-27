---
title: Xamarin.Forms でのカスタム アニメーション
description: この記事では、作成、アニメーションを取り消すと、複数のアニメーションを同期する Xamarin.FOrms のアニメーション クラスを使用して、既存のアニメーションの方法でアニメーション化されていないプロパティをアニメーション化するカスタムのアニメーションを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
ms.openlocfilehash: 405d7990b622b890aa3d66bd632662f086441666
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635758"
---
# <a name="custom-animations-in-xamarinforms"></a>Xamarin.Forms でのカスタム アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_Animation クラスは、すべての Xamarin. フォームアニメーションのビルドブロックであり、ViewExtensions クラスの拡張メソッドを使用して1つ以上のアニメーションオブジェクトを作成します。この記事では、Animation クラスを使用してアニメーションを作成およびキャンセルする方法、複数のアニメーションを同期する方法、および既存のアニメーションメソッドでアニメーション化されていないプロパティをアニメーション化するカスタムアニメーションを作成する方法について説明します。_

`Animation` オブジェクトを作成する場合は、アニメーション化するプロパティの開始値と終了値、およびプロパティの値を変更するコールバックを含む、いくつかのパラメーターを指定する必要があります。 `Animation` オブジェクトは、実行および同期できる子アニメーションのコレクションを保持することもできます。 詳細については、「[子アニメーション](#child)」を参照してください。

[`Animation`](xref:Xamarin.Forms.Animation)クラスを使用して作成されたアニメーションを実行すると、子アニメーションを含む場合と含まない場合があり、 [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッドを呼び出すことで実現されます。 このメソッドは、期間と、他の項目の間で、アニメーションのアニメーションを繰り返すかどうかを制御するコールバックを指定します。

さらに、 [`Animation`](xref:Xamarin.Forms.Animation)クラスには、`IsEnabled` のプロパティがあります。このプロパティを調べて、power 省モードがアクティブになっている場合など、オペレーティングシステムによってアニメーションが無効になっているかどうかを判断できます。

## <a name="create-an-animation"></a>アニメーションを作成する

[`Animation`](xref:Xamarin.Forms.Animation)オブジェクトを作成するときは、通常、次のコード例に示すように、少なくとも3つのパラメーターが必要です。

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

このコードは、 [`Image`](xref:Xamarin.Forms.Image)インスタンスの[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティのアニメーションを、値1から値2に定義します。 Xamarin. Forms によって派生したアニメーション化された値は、最初の引数として指定されたコールバックに渡されます。このコールバックは、`Scale` プロパティの値を変更するために使用されます。

アニメーションは、次のコード例に示すように、 [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッドの呼び出しを使用して開始されます。

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

[`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッドが `Task` オブジェクトを返さないことに注意してください。 代わりに、通知は、コールバック メソッドを通じて提供されます。

`Commit` メソッドでは、次の引数が指定されています。

- 1つ目の引数 (*owner*) は、アニメーションの所有者を識別します。 これには、アニメーションが適用される視覚的要素または、ページなどの別のビジュアル要素を指定できます。
- 2番目の引数 (*name*) は、名前を持つアニメーションを識別します。 名前は、アニメーションを一意に識別するために、所有者と組み合わされます。 この一意の id を使用して、アニメーションが実行中かどうかを判断し ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String)))、キャンセルする ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))) ことができます。
- 3番目の引数 (*rate*) は、 [`Animation`](xref:Xamarin.Forms.Animation)コンストラクターで定義されているコールバックメソッドへの各呼び出しの間のミリ秒数を示します。
- 4番目の引数 (*長さ*) は、アニメーションの継続時間をミリ秒単位で示します。
- 5番目の引数 (*イージング*) は、アニメーションで使用するイージング関数を定義します。 また、イージング関数を[`Animation`](xref:Xamarin.Forms.Animation)コンストラクターの引数として指定することもできます。 イージング関数の詳細については、「[イージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。
- 6番目の引数 (*完成*) は、アニメーションが完了したときに実行されるコールバックです。 このコールバックは2つの引数を受け取ります。最初の引数は最後の値を示し、2番目の引数はアニメーションが取り消された場合に `true` に設定された `bool` になります。 または、*完了*したコールバックを[`Animation`](xref:Xamarin.Forms.Animation)コンストラクターの引数として指定することもできます。 ただし、1つのアニメーションで、*完了*したコールバックが `Animation` コンストラクターと `Commit` メソッドの両方で指定されている場合、`Commit` メソッドで指定されたコールバックだけが実行されます。
- 7番目の引数 (*repeat*) は、アニメーションを繰り返すことができるコールバックです。 これは、アニメーションの最後に呼び出され、アニメーションを繰り返す必要があることを示す `true` を返します。

全体的な効果として、 [`Linear`](xref:Xamarin.Forms.Easing.Linear)イージング関数を使用して、 [`Image`](xref:Xamarin.Forms.Image)の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティを1から2秒 (2000 ミリ秒) に増加させるアニメーションを作成します。 アニメーションが完了するたびに、その `Scale` プロパティが1にリセットされ、アニメーションが繰り返されます。

> [!NOTE]
> 各アニメーションの `Animation` オブジェクトを作成し、各アニメーションで `Commit` メソッドを呼び出すことによって、互いに独立して実行される同時実行アニメーションを構築できます。

<a name="child" />

### <a name="child-animations"></a>子アニメーション

[`Animation`](xref:Xamarin.Forms.Animation)クラスは、子アニメーションもサポートします。これには、他の `Animation` オブジェクトが追加される `Animation` オブジェクトの作成が含まれます。 これにより、一連のアニメーションを実行および同期ができます。 次のコード例では、作成と実行される子アニメーションを示しています。

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

またはより簡潔に、次のコード例に示すよう、コード例を記述できます。

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

どちらのコード例でも、親[`Animation`](xref:Xamarin.Forms.Animation)オブジェクトが作成され、その後、追加の `Animation` オブジェクトが追加されます。 [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))メソッドの最初の2つの引数は、子アニメーションを開始および終了するタイミングを指定します。 引数の値は 0 ~ 1 の範囲でし、指定された子アニメーションになるはアクティブな親アニメーション内の相対的な期間を表す必要があります。 したがって、この例では、アニメーションの前半で `scaleUpAnimation` がアクティブになり、アニメーションの後半で `scaleDownAnimation` がアクティブになり、`rotateAnimation` が全体としてアクティブになります。

全体的な影響は、アニメーションが 4 秒 (4000 ミリ秒) 間に発生することです。 `scaleUpAnimation` は、 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)のプロパティを2秒以上にわたってアニメーション化します。 次に、2秒以上 `Scale` のプロパティを2から1にアニメーション化 `scaleDownAnimation` ます。 両方のスケールアニメーションが発生している間、`rotateAnimation` は[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)プロパティを 0 ~ 360 の4秒間でアニメーション化します。 変更のアニメーションにイージング関数が使用しても注意してください。 [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)イージング関数を実行すると、 [`Image`](xref:Xamarin.Forms.Image)が大きくなる前に最初に縮小され、 [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)イージング関数によって `Image` が完全なアニメーションの最後に向かって実際のサイズより小さくなります。

子アニメーションを使用する[`Animation`](xref:Xamarin.Forms.Animation)オブジェクトとは、次のような違いがいくつかあります。

- 子アニメーションを使用する場合、子アニメーションでの*完了*コールバックは、子がいつ完了したかを示します。また、`Commit` メソッドに渡された*完成*したコールバックは、アニメーション全体が完了したことを示します。
- 子アニメーションを使用する場合、`Commit` メソッドの*繰り返し*コールバックから `true` を返すと、アニメーションは繰り返されませんが、アニメーションは新しい値なしで引き続き実行されます。
- イージング関数を `Commit` メソッドに含め、イージング関数が1より大きい値を返す場合、アニメーションは終了します。 イージング関数は 0 より小さい値を返す場合は、値が 0 に固定されます。 0未満または1より大きい値を返すイージング関数を使用するには、`Commit` メソッドではなく、子アニメーションのいずれかで指定する必要があります。

[`Animation`](xref:Xamarin.Forms.Animation)クラスには、親 `Animation` オブジェクトに子アニメーションを追加するために使用できる[`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double))メソッドも含まれています。 ただし、 *begin*引数と*finish*引数の値は 0 ~ 1 に制限されませんが、0 ~ 1 の範囲に対応する子アニメーションの部分だけがアクティブになります。 たとえば、`WithConcurrent` メソッドの呼び出しで、1 ~ 6 の[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティを対象とする子アニメーションが定義されていて、*開始*値と*終了*値が-2 および3の場合、*開始*値-2 は1の `Scale` 値に対応し、3の*終了*値は `Scale` 値6に対応します。 0 ~ 1 の範囲外の値は、アニメーションには何も表示されません。そのため、`Scale` プロパティは3から6までアニメーション化されます。

## <a name="cancel-an-animation"></a>アニメーションをキャンセルする

アプリケーションでは、次のコード例に示すように、 [`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))の拡張メソッドを呼び出すことで、アニメーションをキャンセルできます。

```csharp
this.AbortAnimation ("SimpleAnimation");
```

アニメーションがアニメーションの所有者とアニメーション名の組み合わせによって一意に識別することに注意してください。 そのため、名前と所有者は、アニメーションを実行している必要がある指定した場合、アニメーションをキャンセルするを指定します。 そのため、このコード例では、ページによって所有されている `SimpleAnimation` という名前のアニメーションを直ちに取り消します。

## <a name="create-a-custom-animation"></a>カスタムアニメーションを作成する

ここまでの例では、 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスのメソッドを使用して同等に実現できるアニメーションを示しています。 ただし、 [`Animation`](xref:Xamarin.Forms.Animation)クラスの利点は、アニメーション化された値が変更されたときに実行されるコールバックメソッドにアクセスできることです。 これにより、必要なすべてのアニメーションを実装するためにコールバックします。 たとえば、次のコード例では、 [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))メソッドによって作成された[`Color`](xref:Xamarin.Forms.Color)値に設定することにより、ページの[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)プロパティをアニメーション化します。このプロパティには、0 ~ 1 の範囲の色相値を指定します。

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

結果として得られるアニメーションは、先行する背景色をページの背景の外観を提供します。

ベジエ曲線アニメーションなど、複雑なアニメーションを作成するその他の例については、「 [Xamarin. フォームで Mobile Apps を作成する](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)」の[第22章](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)を参照してください。

## <a name="create-a-custom-animation-extension-method"></a>カスタムアニメーション拡張メソッドを作成する

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスの拡張メソッドは、プロパティを現在の値から指定された値にアニメーション化します。 これにより、たとえば、ある値から別の値に色をアニメーション化するために使用できる `ColorTo` のアニメーションメソッドを作成するのが困難になります。これは、次のような場合です。

- [`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスで定義されている唯一の[`Color`](xref:Xamarin.Forms.Color)プロパティは[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)ですが、アニメーション化するために必要な `Color` プロパティであるとは限りません。
- 多くの場合、 [`Color`](xref:Xamarin.Forms.Color)プロパティの現在の値は[`Color.Default`](xref:Xamarin.Forms.Color.Default)であり、実際の色ではなく、補間計算では使用できません。

この問題を解決するには、`ColorTo` メソッドが特定の[`Color`](xref:Xamarin.Forms.Color)プロパティを対象としないようにします。 代わりに、補間された `Color` 値を呼び出し元に渡すコールバックメソッドを使用して書き込むことができます。 さらに、メソッドは、`Color` の引数を開始および終了します。

`ColorTo` メソッドは、 [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions)クラスの[`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*)メソッドを使用してその機能を提供する拡張メソッドとして実装できます。 これは、次のコード例に示すように、`Animate` メソッドを使用して、型 `double`ではないプロパティを対象とすることができるためです。

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*)メソッドには、コールバックメソッドである*変換*引数が必要です。 このコールバックへの入力は、常に 0 ~ 1 の範囲内の `double` です。 したがって、`ColorTo` メソッドは、0から1までの範囲の `double` を受け取り、その値に対応する[`Color`](xref:Xamarin.Forms.Color)値を返す独自の変換 `Func` を定義します。 `Color` 値は、指定された2つの`A`引数の[`R`](xref:Xamarin.Forms.Color.R)、 [`G`](xref:Xamarin.Forms.Color.G)、 [`B`](xref:Xamarin.Forms.Color.B)、および[`Color`](xref:Xamarin.Forms.Color.A)の値を補間することによって計算されます。 `Color` 値は、アプリケーションのコールバックメソッドに特定のプロパティに渡されます。

この方法では、次のコード例に示すように、`ColorTo` メソッドで任意の[`Color`](xref:Xamarin.Forms.Color)プロパティをアニメーション化できます。

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

このコード例では、`ColorTo` メソッドによって、 [`Label`](xref:Xamarin.Forms.Label)の[`TextColor`](xref:Xamarin.Forms.Label.TextColor)と[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)プロパティ、ページの `BackgroundColor` プロパティ、および[`Color`](xref:Xamarin.Forms.BoxView)の[`BoxView`](xref:Xamarin.Forms.BoxView.Color)プロパティがアニメーション化されます。

## <a name="related-links"></a>関連リンク

- [カスタムアニメーション (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [Animation API](xref:Xamarin.Forms.Animation)
- [アニメーション拡張 API](xref:Xamarin.Forms.AnimationExtensions)
