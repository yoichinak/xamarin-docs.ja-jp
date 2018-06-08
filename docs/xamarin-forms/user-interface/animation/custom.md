---
title: カスタム アニメーション
description: アニメーション クラスは、1 つまたは複数のアニメーション オブジェクトを作成する ViewExtensions クラスの拡張メソッドで、すべての Xamarin.Forms アニメーションのビルド ブロックです。 この記事では、作成し、アニメーションをキャンセル、複数のアニメーションを同期する、アニメーション クラスを使用して、既存のアニメーション メソッドでアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 238268a3ad2b82494f1d096d0cbeba97edb90366
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847785"
---
# <a name="custom-animations"></a>カスタム アニメーション

_アニメーション クラスは、1 つまたは複数のアニメーション オブジェクトを作成する ViewExtensions クラスの拡張メソッドで、すべての Xamarin.Forms アニメーションのビルド ブロックです。この記事では、作成し、アニメーションをキャンセル、複数のアニメーションを同期する、アニメーション クラスを使用して、既存のアニメーション メソッドでアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成する方法を示します。_


作成するときに、パラメーターの数を指定する必要があります、`Animation`をアニメーション化するプロパティの開始と終了値を含むオブジェクトとプロパティの値を変更するコールバック。 `Animation`オブジェクトを実行して同期できる子アニメーションのコレクションの管理もできます。 詳細については、次を参照してください。[子アニメーション](#child)です。

作成したアニメーションの実行、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)子アニメーションを含めることはできませんか、クラスが呼び出すことで実現、 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/)メソッドです。 このメソッドは、期間、アニメーション、およびその他の項目の間で、アニメーションを繰り返すかどうかを制御するコールバックを指定します。

## <a name="creating-an-animation"></a>アニメーションを作成します。

作成するときに、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)オブジェクト、通常、3 つのパラメーターの最小必須では、次のコード例で示したようにします。

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

このコードのアニメーションを定義する、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 の値を 1 の値からのインスタンス。 値を変更に使用されている最初の引数として指定されたコールバックに渡されるアニメーションの値は、Xamarin.Forms では、`Scale`プロパティです。

呼び出して、アニメーションが開始、 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/)メソッドを次のコード例で示したようにします。

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

なお、 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/)メソッドが返されません、`Task`オブジェクト。 代わりに、通知は、コールバック メソッドによって提供されます。

次の引数が指定されて、`Commit`メソッド。

- 最初の引数 (*所有者*)、アニメーションの所有者を識別します。 アニメーションが適用されている、視覚的要素またはページなどの他のビジュアル要素を指定できます。
- 2 番目の引数 (*名前*) 名前を持つアニメーションを識別します。 名前は、アニメーションを一意に識別する所有者と組み合わされます。 アニメーションが実行されているかどうかを決定する、この一意の id を使用することができます ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/))、またはそれをキャンセルする ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/))。
- 3 番目の引数 (*レート*) で定義されたコールバック メソッドを呼び出す間隔 (ミリ秒) の数を示す、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)コンス トラクター
- 4 番目の引数 (*長さ*)、アニメーション、ミリ秒単位の期間を示します。
- 5 番目の引数 (*イージング*)、アニメーションで使用するイージング関数を定義します。 イージング関数をへの引数として指定する代わりに、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)コンス トラクターです。 イージング関数の詳細については、次を参照してください。[イージング関数](~/xamarin-forms/user-interface/animation/easing.md)です。
- 6 番目の引数 (*完了*) は、アニメーションが完了したときに実行されるコールバックです。 このコールバックは、最終的な値と 2 番目の引数を示す最初の引数の 2 つの引数を受け取り、`bool`に設定されている`true`アニメーションが取り消された場合。 または、*完了*への引数として指定できるコールバック、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)コンス トラクターです。 は、1 つのアニメーション場合*完了*コールバックが両方で指定された、`Animation`コンス トラクターと`Commit`メソッドで指定されたコールバックのみ、`Commit`メソッドは実行されます。
- 7 番目の引数 (*繰り返します*) が繰り返されるアニメーションを許可するコールバック。 アニメーションの終了時に呼び出され、返す`true`アニメーションを繰り返す必要があることを示します。

全体的な効果が増加するアニメーションを作成するには、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)のプロパティ、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 1 ~ 2、2 秒を超える (2000 ミリ秒) を使用して、 [ `Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/)イージング機能。 アニメーションが完了するたびにその`Scale`プロパティが 1 にリセットされ、アニメーションが繰り返されます。

> [!NOTE]
> 作成することで、それぞれ個別に実行される同時実行のアニメーションを構築する、`Animation`各アニメーションのオブジェクトを呼び出すことで、`Commit`各アニメーションのメソッドです。

<a name="child" />

### <a name="child-animations"></a>子アニメーション

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)クラスもサポートしている子アニメーションを作成する必要があります、`Animation`する他のオブジェクト`Animation`オブジェクトが追加されます。 これにより、一連のアニメーションを実行し、同期できます。 次のコード例では、作成と子アニメーションの実行を示しています。

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

両方のコード例については、親の[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)に、その他のオブジェクトが作成される`Animation`オブジェクトが追加されます。 最初の 2 つの引数、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/)メソッドは、開始日と子アニメーションを終了するタイミングを指定します。 引数の値は、0 と 1 の間であるし、親アニメーションが指定された子アニメーションをアクティブになることの相対的な期間を表す必要があります。 そのため、この例で、 `scaleUpAnimation` 、アニメーションの最初の半分をアクティブになる、`scaleDownAnimation`アクティブ、アニメーションの下半分になります、`rotateAnimation`有効期間全体になります。

全体的な効果は、アニメーションが 4 秒 (4000 ミリ秒) 間に発生することです。 `scaleUpAnimation`をアニメーション化、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)プロパティを 1 から 2、2 秒以上にします。 `scaleDownAnimation`アニメーション化し、 `Scale` 2 からプロパティを 1、2 秒を超える。 両方の小数点以下桁数のアニメーションの実行中、`rotateAnimation`をアニメーション化、 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)プロパティを 0 から 360、4 秒間にします。 スケーリングのアニメーションにイージング関数が使用しても注意してください。 [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/)イージング機能により、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)を最初に大きく、取得する前に圧縮して、 [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)イージング関数により、`Image`に完全なアニメーションの終了するころに、実際のサイズよりも小さくなります。

多くの違いがある、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)子アニメーションを使用するオブジェクトとが 1 つ。

- 子アニメーションを使用する場合、*完了*子アニメーションのコールバックでは、子プロセスが完了すると、ことを示します、*完了*に渡されたコールバック、`Commit`メソッドかを示す、アニメーション全体が完了しました。
- 子アニメーションを使用する場合を返す`true`から、*繰り返します*のコールバックを`Commit`メソッドでアニメーションを繰り返すには発生しませんが、アニメーションが新しい値を使用せず実行し続けます。
- イージング関数を含む場合に、`Commit`メソッド、およびイージング関数値を返します 1 より大きい、アニメーションは終了されます。 イージング関数は 0 より小さい値を返す場合、値は 0 に固定されます。 0 未満か、1 より大きい値を返すイージング関数を使用するのではなく子アニメーションのいずれかに指定している必要があります、`Commit`メソッドです。

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)クラスも含まれます[ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/)を親に子アニメーションの追加に使用できるメソッドを`Animation`オブジェクト。 ただし、その*開始*と*完了*引数の値が 0 ~ 1 に制限はありませんが、0 ~ 1 の範囲に対応する子アニメーションの部分だけがアクティブになります。 たとえば場合、`WithConcurrent`メソッドの呼び出しを対象とする子アニメーションを定義する、 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)プロパティを 1 から 6、ただし*開始*と*完了*の値-2 と 3、*開始*-2 の値に対応、`Scale`値 1、および*完了*3 の値に対応、 `Scale` 6 の値。 0 ~ 1 の範囲外の値をアニメーションに一部再生ないため、`Scale`プロパティのみアニメーション化される 3 から 6 にします。

## <a name="canceling-an-animation"></a>アニメーションのキャンセル

アプリケーションへの呼び出しにアニメーションを取り消すことができます、 [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)拡張メソッドを次のコード例で示したようにします。

```csharp
this.AbortAnimation ("SimpleAnimation");
```

アニメーションをアニメーション所有者、およびアニメーション名の組み合わせによって一意に識別することに注意してください。 そのため、名前と所有者指定されてとアニメーションを実行している必要があります指定するアニメーションをキャンセルします。 そのため、コード例はすぐに [キャンセル] という名前のアニメーション`SimpleAnimation`ページが所有します。

## <a name="creating-a-custom-animation"></a>カスタム アニメーションを作成します。

これまでは、ここに示した例のメソッドと共に均等に達成できるアニメーションを説明しました、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスです。 ただしの利点、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)クラスは、アニメーション化された値が変更されたときに実行されるコールバック メソッドへのアクセスを持っていること。 これにより、すべての必要なアニメーションを実装するコールバック。 たとえば、次のコード例をアニメーション化、 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)プロパティを設定することによって、ページの[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)によって作成された値、 [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)色合いの値が 0 から 1 までの範囲を持つ、メソッド。

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

結果として得られるアニメーションは、先行する背景色をページの背景の外観を提供します。

ベジエ曲線アニメーションなどの複雑なアニメーションを作成する例を参照してください。[章 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)の[Xamarin.Forms を使用したモバイル アプリを作成する](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)です。

## <a name="creating-a-custom-animation-extension-method"></a>カスタム アニメーションの拡張メソッドを作成します。

拡張メソッドにおいて、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスは、現在の値から、指定した値のプロパティをアニメーション化します。 これにより、困難を作成するなど、 `ColorTo` 1 つの値からを別の色をアニメーション化するために使用できるアニメーション メソッド。

- 唯一[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)によって定義されたプロパティ、 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)クラスは[ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)、これは必ずしも必要な`Color`プロパティアニメーション化します。
- 現在の値では多くの場合、 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)プロパティは[ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)、実際の色、そうでないし、補間の計算で使用することはできません。

この問題を解決するにはないように、`ColorTo`メソッド、特定のターゲット[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)プロパティです。 代わりに、それを作成すると、補間で渡されるコールバック メソッド`Color`呼び出し元に戻す値です。 メソッドの開始し、終了のさらに、`Color`引数。

`ColorTo`を使用する拡張メソッドとしてメソッドを実装することができます、 [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/)メソッドで、 [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)その機能を提供するクラス。 これは、ため、`Animate`メソッドは、型ではないターゲットのプロパティを使用することができます`double`の次のコード例に示すように。

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

[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/)メソッドが必要な*変換*はコールバック メソッドの引数。 このコールバックへの入力は、常に、 `double` 0 から 1 までの範囲です。 したがって、`ColorTo`メソッドは、独自の変換を定義`Func`を受け入れる、 `double` 、0 ~ 1 の場合、およびそのを返します、 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)その値に対応する値。 `Color`値の補間して計算されます、 [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)、 [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)、 [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)、および[ `A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)指定された 2 つの値`Color`引数。 `Color`値は、特定のプロパティをアプリケーションのコールバック メソッドに渡されます。

このアプローチにより、`ColorTo`いずれかをアニメーション化するメソッド[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)プロパティ、次のコード例で示したように。

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

このコード例では、`ColorTo`メソッドをアニメーション化、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)と[ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)のプロパティ、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)、 `BackgroundColor`、ページのプロパティと[ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/)のプロパティ、 [ `BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)です。

## <a name="summary"></a>まとめ

この記事には、使用する方法が示されている、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)クラスを作成し、アニメーションをキャンセル、複数のアニメーションを同期およびは既存のアニメーションでアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成します。メソッド。 `Animation`クラスは、すべての Xamarin.Forms アニメーションのビルド ブロックです。


## <a name="related-links"></a>関連リンク

- [カスタム アニメーション (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [アニメーション](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
