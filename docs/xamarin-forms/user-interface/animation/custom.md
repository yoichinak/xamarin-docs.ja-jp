---
title: Xamarin.Forms でのカスタム アニメーション
description: この記事では、作成、アニメーションを取り消すと、複数のアニメーションを同期する Xamarin.FOrms のアニメーション クラスを使用して、既存のアニメーションの方法でアニメーション化されていないプロパティをアニメーション化するカスタムのアニメーションを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 157e044fd96cdeff87d8fb56029fe625b7312bf4
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056241"
---
# <a name="custom-animations-in-xamarinforms"></a>Xamarin.Forms でのカスタム アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)

_アニメーション クラスは、1 つまたは複数のアニメーション オブジェクトを作成する ViewExtensions クラスで拡張メソッドで、すべての Xamarin.Forms のアニメーションのビルディング ブロックです。この記事では、アニメーション クラスを使用して作成、アニメーションを取り消すと、複数のアニメーションを同期するには、既存のアニメーションの方法でアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成する方法を示します。_


作成時に、パラメーターの数を指定する必要があります、`Animation`をアニメーション化するプロパティの開始と終了値を含むオブジェクトとプロパティの値を変更するコールバック。 `Animation`オブジェクト実行および同期が可能な子アニメーションのコレクションの管理もできます。 詳細については、次を参照してください。[子アニメーション](#child)します。

作成したアニメーションの実行、 [ `Animation` ](xref:Xamarin.Forms.Animation)子アニメーションを含まない場合がありますか、クラスが呼び出すことによって実現されます、 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッド。 このメソッドは、期間と、他の項目の間で、アニメーションのアニメーションを繰り返すかどうかを制御するコールバックを指定します。

## <a name="creating-an-animation"></a>アニメーションを作成します。

作成するときに、 [ `Animation` ](xref:Xamarin.Forms.Animation)の次のコード例に示す、3 つのパラメーターの最小値が必要な場合は、通常、オブジェクトします。

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

このコードのアニメーションを定義する、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image) 2 の値に 1 の値からのインスタンス。 値を変更する使用されている最初の引数として指定されたコールバックに渡されるアニメーションの値は、Xamarin.Forms の派生、`Scale`プロパティ。

呼び出して、アニメーションが開始、 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッドは、次のコード例で示した。

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

なお、 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッドが返されません、`Task`オブジェクト。 代わりに、通知は、コールバック メソッドを通じて提供されます。

次の引数が指定されて、`Commit`メソッド。

- 最初の引数 (*所有者*)、アニメーションの所有者を識別します。 これには、アニメーションが適用される視覚的要素または、ページなどの別のビジュアル要素を指定できます。
- 2 番目の引数 (*名前*) 名前を持つアニメーションを識別します。 名前は、アニメーションを一意に識別するために、所有者と組み合わされます。 この一意の id、アニメーションが実行されているかどうかを使用できます ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String)))、またはそれをキャンセルする ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)))。
- 3 番目の引数 (*レート*) で定義されたコールバック メソッドを呼び出す間隔 (ミリ秒) の数を示す、 [ `Animation` ](xref:Xamarin.Forms.Animation)コンス トラクター
- 4 番目の引数 (*長さ*) (ミリ秒単位)、アニメーションの時間を示します。
- 5 番目の引数 (*イージング*)、アニメーションで使用されるイージング関数を定義します。 引数として、イージング関数を指定する代わりに、 [ `Animation` ](xref:Xamarin.Forms.Animation)コンス トラクター。 イージング関数の詳細については、次を参照してください。[イージング関数](~/xamarin-forms/user-interface/animation/easing.md)します。
- 6 番目の引数 (*完了*) は、アニメーションが完了したときに実行されるコールバックです。 このコールバックは、最初の引数を示す最終的な値では、2 番目の引数を 2 つの引数を受け取り、`bool`に設定されている`true`アニメーションが取り消された場合。 または、*完了*への引数としてコールバックを指定することができます、 [ `Animation` ](xref:Xamarin.Forms.Animation)コンス トラクター。 ただし、1 つのアニメーションで場合*完了*コールバックが両方で指定された、`Animation`コンス トラクターと`Commit`メソッドで指定されたコールバックのみ、`Commit`メソッドが実行されます。
- 7 番目の引数 (*繰り返します*) は、アニメーションを繰り返すことができるコールバックです。 アニメーションの終了時に呼び出され、返す`true`アニメーションを繰り返すよう指定する必要があります。

全体的な影響が増加するアニメーションを作成するには、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image) 1 から 2 を超える 2 秒 (2000 ミリ秒) を使用して、 [ `Linear`](xref:Xamarin.Forms.Easing.Linear)イージング機能。 アニメーションが完了するたびにその`Scale`プロパティは 1 にリセットされ、アニメーションが繰り返されます。

> [!NOTE]
> それぞれ個別に実行される同時実行のアニメーションを作成して構築できる、`Animation`し、それぞれのアニメーションのオブジェクト、`Commit`メソッドをそれぞれのアニメーション。

<a name="child" />

### <a name="child-animations"></a>子アニメーション

[ `Animation` ](xref:Xamarin.Forms.Animation)クラスもサポートしています、子のアニメーションを作成する必要がありますが、`Animation`する他のオブジェクト`Animation`オブジェクトが追加されます。 これにより、一連のアニメーションを実行および同期ができます。 次のコード例では、作成と実行される子アニメーションを示しています。

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

両方のコード例については、親で[ `Animation` ](xref:Xamarin.Forms.Animation)に、その他のオブジェクトが作成される`Animation`オブジェクトが追加されます。 最初の 2 つの引数、 [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))メソッドでは、開始日と子アニメーションを終了するタイミングを指定します。 引数の値は 0 ~ 1 の範囲でし、指定された子アニメーションになるはアクティブな親アニメーション内の相対的な期間を表す必要があります。 そのため、この例では、 `scaleUpAnimation` 、アニメーションの前半にアクティブになる、 `scaleDownAnimation` 、アニメーションの後半にはアクティブになると`rotateAnimation`有効期間全体になります。

全体的な影響は、アニメーションが 4 秒 (4000 ミリ秒) 間に発生することです。 `scaleUpAnimation`をアニメーション化、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)プロパティを 1 から 2、2 秒以上にします。 `scaleDownAnimation`アニメーション化し、`Scale`プロパティを 2 から 1、2 秒以上にします。 両方のスケールのアニメーションの実行中、`rotateAnimation`をアニメーション化、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)プロパティを 0 から 360、4 秒間にします。 変更のアニメーションにイージング関数が使用しても注意してください。 [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn)イージング機能により、 [ `Image` ](xref:Xamarin.Forms.Image)大きく、取得する前に最初に圧縮して、 [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut)イージング関数により、`Image`に完全なアニメーションの終了に向けて、実際のサイズよりも小さくなります。

多くの違いがある、 [ `Animation` ](xref:Xamarin.Forms.Animation)ことと、子のアニメーションを使用してオブジェクト。

- 子のアニメーションを使用する場合、*完了*子アニメーションのコールバックでは、子プロセスが完了すると、ことを示します、*完了*にコールバックが渡される、`Commit`メソッドかを示す、全体のアニメーションが完了しました。
- 子アニメーションを使用する場合は、返す`true`から、*繰り返します*でコールバック、`Commit`メソッドでは、アニメーションを繰り返すは発生しませんが、アニメーションが新しい値を使用せず実行し続けます。
- イージング関数を含める場合、`Commit`メソッド、およびイージング関数値を返します 1 より大きい、アニメーションは終了されます。 イージング関数は 0 より小さい値を返す場合は、値が 0 に固定されます。 0 より小さいか、1 より大きい値を返すイージング関数を使用するではなく、子アニメーションのいずれかで指定する必要があります、`Commit`メソッド。

[ `Animation` ](xref:Xamarin.Forms.Animation)クラスも含まれています。 [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double))を親に子アニメーションの追加に使用できるメソッドを`Animation`オブジェクト。 ただし、その*開始*と*完了*引数の値は 0、1 に制限されませんが、0 ~ 1 の範囲に対応する子アニメーションの部分のみがアクティブになります。 たとえば場合、`WithConcurrent`メソッドの呼び出しを対象とする子アニメーションを定義します、 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)プロパティを 1 から 6 が*開始*と*完了*の値-2 と 3 で、*開始*-2 の値に対応、`Scale`値は 1 です、*完了*3 の値に対応、 `Scale` 6 の値。 0 ~ 1 の範囲外の値、アニメーションの一部再生しないため、`Scale`を 6 に、プロパティが 3 からアニメーションだけです。

## <a name="canceling-an-animation"></a>アニメーションのキャンセル

アプリケーションへの呼び出しで、アニメーションを取り消すことができます、 [ `AbortAnimation` ](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))拡張メソッドを次のコード例で示した。

```csharp
this.AbortAnimation ("SimpleAnimation");
```

アニメーションがアニメーションの所有者とアニメーション名の組み合わせによって一意に識別することに注意してください。 そのため、名前と所有者は、アニメーションを実行している必要がある指定した場合、アニメーションをキャンセルするを指定します。 そのため、コード例でという名前のアニメーション直ちにキャンセル`SimpleAnimation`が所有するページ。

## <a name="creating-a-custom-animation"></a>カスタム アニメーションを作成します。

これまでには、ここに示す例には、アニメーションのメソッドと共に均等にすることができましたが示されていますが、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラス。 ただし、利点、 [ `Animation` ](xref:Xamarin.Forms.Animation)クラスは、アニメーション化された値が変更されたときに実行されるコールバック メソッドへのアクセスを持っていること。 これにより、必要なすべてのアニメーションを実装するためにコールバックします。 たとえば、次のコード例をアニメーション化、 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)プロパティ設定することで、ページの[ `Color` ](xref:Xamarin.Forms.Color)によって作成された値、 [ `Color.FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))色相値の 0 から 1 までのメソッド。

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

結果として得られるアニメーションは、先行する背景色をページの背景の外観を提供します。

ベジエ曲線のアニメーションを含む、複雑なアニメーションを作成する例を参照してください。[第 22 章](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)の[を Xamarin.Forms での Mobile Apps の作成](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)です。

## <a name="creating-a-custom-animation-extension-method"></a>カスタム アニメーションの拡張メソッドを作成します。

拡張メソッドにおいて、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスは、指定した値には、現在の値からプロパティをアニメーション化します。 これにより、困難を作成するなど、 `ColorTo` 1 つの値からを別の色をアニメーション化するために使用できるアニメーション メソッド。

- 唯一[ `Color` ](xref:Xamarin.Forms.Color)によって定義されるプロパティ、 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスは[ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)、目的、必ずしもこれ`Color`プロパティアニメーション化します。
- 現在の値では多くの場合、 [ `Color` ](xref:Xamarin.Forms.Color)プロパティは[ `Color.Default` ](xref:Xamarin.Forms.Color.Default)、実際の色、そうでないし、補間の計算で使用することはできません。

この問題を解決するにはしない、`ColorTo`メソッド、特定のターゲット[ `Color` ](xref:Xamarin.Forms.Color)プロパティ。 補間を通過するコールバック メソッドを使用して記述する代わりに、`Color`呼び出し元に戻す値です。 メソッドの開始し、終了がさらに、`Color`引数。

`ColorTo`メソッドを使用する拡張メソッドとして実装することができます、 [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*)メソッドで、 [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions)その機能を提供するクラス。 これは、ため、`Animate`メソッドを使用するターゲットのプロパティの型ではない`double`次のコード例に示すように。

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

[ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*)メソッドが必要です、*変換*引数で、コールバック メソッドです。 このコールバックへの入力は常に、 `double` 0 から 1 までです。 そのため、`ColorTo`メソッドは、独自の変換を定義する`Func`を受け入れる、 `double` 、0 ~ 1 の場合、およびその返します、 [ `Color` ](xref:Xamarin.Forms.Color)その値に対応する値。 `Color`値を補間して計算されます、 [ `R` ](xref:Xamarin.Forms.Color.R)、 [ `G` ](xref:Xamarin.Forms.Color.G)、 [ `B` ](xref:Xamarin.Forms.Color.B)、および[ `A`](xref:Xamarin.Forms.Color.A)指定された 2 つの値`Color`引数。 `Color`値は、特定のプロパティをアプリケーションのコールバック メソッドに渡されます。

このアプローチにより、`ColorTo`いずれかをアニメーション化するメソッド[ `Color` ](xref:Xamarin.Forms.Color)プロパティは、次のコード例で示した。

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

このコード例では、`ColorTo`メソッドをアニメーション化、 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)と[ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)のプロパティを[ `Label` ](xref:Xamarin.Forms.Label)、 `BackgroundColor`、ページのプロパティと[ `Color` ](xref:Xamarin.Forms.BoxView.Color)のプロパティを[ `BoxView`](xref:Xamarin.Forms.BoxView)します。

## <a name="summary"></a>まとめ

この記事では、使用する方法を示しました、 [ `Animation` ](xref:Xamarin.Forms.Animation)クラスを作成、アニメーションをキャンセル、複数のアニメーションを同期するには、既存のアニメーションによってアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成するにはメソッド。 `Animation`クラスは、すべての Xamarin.Forms のアニメーションのビルド ブロックです。


## <a name="related-links"></a>関連リンク

- [カスタム アニメーション (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [アニメーション](xref:Xamarin.Forms.Animation)
- [AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
