---
title: "カスタムアニメーションの Xamarin.Forms " 説明: "この記事では、Xamarin. FOrms Animation クラスを使用してアニメーションを作成およびキャンセルする方法、複数のアニメーションを同期する方法、および既存のアニメーションメソッドでアニメーション化されていないプロパティをアニメーション化するカスタムアニメーションを作成する方法を示します。
ms. 製品: xamarin ms. assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 02/10/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="custom-animations-in-xamarinforms"></a>でのカスタムアニメーションXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_Animation クラスは、すべてのアニメーションのビルドブロックであり Xamarin.Forms 、ViewExtensions クラスの拡張メソッドは1つ以上のアニメーションオブジェクトを作成します。この記事では、Animation クラスを使用してアニメーションを作成およびキャンセルする方法、複数のアニメーションを同期する方法、および既存のアニメーションメソッドでアニメーション化されていないプロパティをアニメーション化するカスタムアニメーションを作成する方法について説明します。_

オブジェクトを作成するときには、アニメーション化する `Animation` プロパティの開始値と終了値、およびプロパティの値を変更するコールバックを含む、いくつかのパラメーターを指定する必要があります。 オブジェクトは、 `Animation` 実行および同期できる子アニメーションのコレクションを保持することもできます。 詳細については、「[子アニメーション](#child-animations)」を参照してください。

クラスを使用して作成されたアニメーションを実行すると、 [`Animation`](xref:Xamarin.Forms.Animation) 子アニメーションを含む場合と含まない場合があります。これを行うには、[ `Commit` ] (xref: Xamarin.Forms . animation. Commit () を呼び出し Xamarin.Forms ます。System.windows.media.animation.ianimatable>、System.string、System.string、、、および Xamarin.Forms 。イージング、system.string {system.string}、system.string {System. Boolean})) メソッドを実行してください。 このメソッドは、アニメーションの期間と、アニメーションを繰り返すかどうかを制御するコールバックを指定します。

さらに、 [`Animation`](xref:Xamarin.Forms.Animation) クラスには、 `IsEnabled` power 省モードがアクティブ化されている場合など、オペレーティングシステムによってアニメーションが無効にされているかどうかを確認するために調べることができるプロパティがあります。

## <a name="create-an-animation"></a>アニメーションを作成する

オブジェクトを作成するときは、 [`Animation`](xref:Xamarin.Forms.Animation) 通常、次のコード例に示すように、3つ以上のパラメーターが必要です。

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

このコードは、インスタンスのプロパティのアニメーションを、 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) 値1から値2に定義します。 によって派生されたアニメーション化された値 Xamarin.Forms は、プロパティの値を変更するために使用される最初の引数として指定されたコールバックに渡され `Scale` ます。

アニメーションは、[ `Commit` ] (xref: Xamarin.Forms . Animation. Commit () の呼び出しで開始さ Xamarin.Forms れます。System.windows.media.animation.ianimatable>、System.string、System.string、、、および Xamarin.Forms 。次のコード例に示すように、イージング、system.string {system.string}、system.string {System. Boolean}) メソッドを使用します。

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

[ `Commit` ] (Xref: Xamarin.Forms . Animation. Commit () であることに注意して Xamarin.Forms ください。System.windows.media.animation.ianimatable>、System.string、System.string、、、および Xamarin.Forms 。イージング, system.string {system.string {system.string}, system.string {System. Boolean})) メソッドは、オブジェクトを返しませんでした `Task` 。 代わりに、コールバックメソッドによって通知が提供されます。

メソッドでは、次の引数が指定されてい `Commit` ます。

- 1つ目の引数 (*owner*) は、アニメーションの所有者を識別します。 これは、アニメーションが適用されるビジュアル要素、またはページなどの別のビジュアル要素にすることができます。
- 2番目の引数 (*name*) は、名前を持つアニメーションを識別します。 名前は、アニメーションを一意に識別するために所有者と結合されます。 この一意の id を使用して、アニメーションが実行されているかどうかを判断できます ([ `AnimationIsRunning` ] (xref:) Xamarin.Forms 。AnimationIsRunning ( Xamarin.Forms .System.windows.media.animation.ianimatable>、System.string))、または取り消します ([ `AbortAnimation` ] (xref:) Xamarin.Forms 。アニメーション拡張機能。 AbortAnimation ( Xamarin.Forms .System.windows.media.animation.ianimatable>、System.string)) を指定します。
- 3番目の引数 (*rate*) は、コンストラクターで定義されているコールバックメソッドへの各呼び出しの間のミリ秒数を示します。 [`Animation`](xref:Xamarin.Forms.Animation)
- 4番目の引数 (*長さ*) は、アニメーションの継続時間をミリ秒単位で示します。
- 5番目の引数 (*イージング*) は、アニメーションで使用するイージング関数を定義します。 また、イージング関数をコンストラクターの引数として指定することもでき [`Animation`](xref:Xamarin.Forms.Animation) ます。 イージング関数の詳細については、「[イージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。
- 6番目の引数 (*完成*) は、アニメーションが完了したときに実行されるコールバックです。 このコールバックは、2つの引数を受け取ります。最初の引数は最後の値を示し、2番目の引数は `bool` `true` アニメーションがキャンセルされた場合にに設定されます。 または、*完了*したコールバックをコンストラクターの引数として指定でき [`Animation`](xref:Xamarin.Forms.Animation) ます。 ただし、1つのアニメーションで、*完了*したコールバックがコンストラクターとメソッドの両方で指定されている場合は、 `Animation` `Commit` メソッドで指定されたコールバックだけが `Commit` 実行されます。
- 7番目の引数 (*repeat*) は、アニメーションを繰り返すことができるコールバックです。 これはアニメーションの最後に呼び出され、を返す `true` ことは、アニメーションを繰り返す必要があることを示します。

全体の効果とし [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) て、イージング関数を使用して、のプロパティを 1 ~ 2 秒 (2000 ミリ秒間) に増加させるアニメーションを作成し [`Linear`](xref:Xamarin.Forms.Easing.Linear) ます。 アニメーションが完了するたびに、 `Scale` プロパティが1にリセットされ、アニメーションが繰り返されます。

> [!NOTE]
> 各アニメーション `Animation` のオブジェクトを作成し、各アニメーションに対してメソッドを呼び出すことによって、互いに独立して実行される同時実行アニメーションを構築でき `Commit` ます。

### <a name="child-animations"></a>子アニメーション

[`Animation`](xref:Xamarin.Forms.Animation)クラスは、 `Animation` 他のオブジェクトが追加されるオブジェクトの作成を含む子アニメーションもサポートし `Animation` ます。 これにより、一連のアニメーションの実行と同期が可能になります。 次のコード例は、子アニメーションを作成して実行する方法を示しています。

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

また、次のコード例に示すように、コード例をより簡潔に記述することもできます。

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

どちらのコード例でも、親 [`Animation`](xref:Xamarin.Forms.Animation) オブジェクトが作成され、追加された `Animation` オブジェクトが追加されます。 [ `Add` ] (Xref: に対する最初の2つの引数 Xamarin.Forms 。Animation。 Add (system.string, system.string, Xamarin.Forms .Animation) メソッドは、子アニメーションを開始および終了するタイミングを指定します。 引数の値は0から1までの範囲で指定する必要があります。また、指定された子アニメーションがアクティブになる親アニメーション内の相対的な期間を表します。 したがって、この例では、アニメーションの前半でがアクティブになり、アニメーションの後半でがアクティブになり、が `scaleUpAnimation` `scaleDownAnimation` 全体として `rotateAnimation` アクティブになります。

全体的な影響は、アニメーションが4秒 (4000 ミリ秒) を超えて発生することです。 は、2秒を超えて、1 ~ 2 のプロパティをアニメーション化し `scaleUpAnimation` [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) ます。 次に、は 2 `scaleDownAnimation` `Scale` 秒以上、プロパティを2から1にアニメーション化します。 両方のスケールアニメーションが発生している間、では、 `rotateAnimation` プロパティが 0 ~ 360 の4秒を超えてアニメーション化され [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) ます。 スケーリングアニメーションでもイージング関数が使用されていることに注意してください。 [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)イージング関数を実行すると、が大きくなる前にが [`Image`](xref:Xamarin.Forms.Image) 最初に縮小され、 [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) イージング関数によって、が `Image` 完全なアニメーションの終わりに向かって実際のサイズよりも小さくなります。

子アニメーションを使用するオブジェクトには、次のような違いがいくつ [`Animation`](xref:Xamarin.Forms.Animation) かあります。

- 子アニメーションを使用する場合、子アニメーションでの*完了*コールバックは、子がいつ完了したかを示し、メソッドに渡された*完成*したコールバックが `Commit` アニメーション全体が完了したことを示します。
- 子アニメーションを使用する場合、 `true` メソッドの*繰り返し*コールバックからを返すと、 `Commit` アニメーションは繰り返されませんが、アニメーションは新しい値なしで実行され続けます。
- イージング関数をメソッドに含め `Commit` 、イージング関数が1より大きい値を返す場合、アニメーションは終了します。 イージング関数が0未満の値を返す場合、値は0にクランプされます。 0未満または1より大きい値を返すイージング関数を使用するには、メソッドではなく、子アニメーションのいずれかで指定する必要があり `Commit` ます。

クラスには、 [`Animation`](xref:Xamarin.Forms.Animation) [ `WithConcurrent` ] (xref: も含まれます Xamarin.Forms 。アニメーション。 WithConcurrent 実行 ( Xamarin.Forms .Animation、system.string、system.string) の各メソッドを使用して、親オブジェクトに子アニメーションを追加でき `Animation` ます。 ただし、 *begin*引数と*finish*引数の値は 0 ~ 1 に制限されませんが、0 ~ 1 の範囲に対応する子アニメーションの部分だけがアクティブになります。 たとえば、 `WithConcurrent` メソッド呼び出しで、1 ~ 6 のプロパティを対象とする子アニメーションが定義されていて、 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) *開始*値と*終了*値が-2 および3の場合、*開始*値-2 は `Scale` 1 の値に、3の*終了*値は `Scale` 6 の値に対応します。 0 ~ 1 の範囲外の値は、アニメーションの一部を再生しないので、 `Scale` プロパティは3から6までアニメーション化されます。

## <a name="cancel-an-animation"></a>アニメーションをキャンセルする

アプリケーションでは、[ `AbortAnimation` ] (xref: を呼び出してアニメーションをキャンセルできます Xamarin.Forms 。アニメーション拡張機能。 AbortAnimation ( Xamarin.Forms .System.windows.media.animation.ianimatable>, System.string)) 拡張メソッドを次のコード例に示すように指定します。

```csharp
this.AbortAnimation ("SimpleAnimation");
```

アニメーションは、アニメーションの所有者とアニメーション名の組み合わせによって一意に識別されることに注意してください。 そのため、アニメーションを実行するときに指定する所有者と名前を指定して、アニメーションをキャンセルする必要があります。 そのため、このコード例では、 `SimpleAnimation` ページによって所有されているという名前のアニメーションを直ちに取り消します。

## <a name="create-a-custom-animation"></a>カスタムアニメーションを作成する

ここまでの例では、クラスのメソッドを使用して同等に実現できるアニメーションを示してい [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) ます。 ただし、クラスの利点は、アニメーション化された値が変更され [`Animation`](xref:Xamarin.Forms.Animation) たときに実行されるコールバックメソッドにアクセスできることです。 これにより、コールバックで必要なアニメーションを実装できます。 たとえば、次のコード例では、 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) ページのプロパティをアニメーション化して、 [`Color`](xref:Xamarin.Forms.Color) メソッドによって作成された値に設定します。この場合、 [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) 色合いの値は0から1までの範囲で指定します。

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

結果として得られるアニメーションでは、レインボーの色によってページの背景を進めることができます。

ベジエ曲線アニメーションなど、複雑なアニメーションを作成するその他の例については、「」の「 [ Xamarin.Forms Mobile Apps の作成](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)」の[第22章](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)を参照してください。

## <a name="create-a-custom-animation-extension-method"></a>カスタムアニメーション拡張メソッドを作成する

クラスの拡張メソッドは、 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) プロパティを現在の値から指定された値にアニメーション化します。 これにより、たとえば、ある `ColorTo` 値から別の値に色をアニメーション化するために使用できるアニメーションメソッドを作成するのが困難になります。これは、次のような場合です。

- クラスで定義されている唯一の [`Color`](xref:Xamarin.Forms.Color) プロパティ [`VisualElement`](xref:Xamarin.Forms.VisualElement) はです [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 。これは、アニメーション化するのに必要なプロパティではありません `Color` 。
- 多くの場合、プロパティの現在の値 [`Color`](xref:Xamarin.Forms.Color) は [`Color.Default`](xref:Xamarin.Forms.Color.Default) であり、実際の色ではなく、補間計算では使用できません。

この問題を解決するには、メソッドが特定のプロパティを対象としていないようにし `ColorTo` [`Color`](xref:Xamarin.Forms.Color) ます。 代わりに、補間の値を呼び出し元に渡すコールバックメソッドを使用して書き込むことができ `Color` ます。 また、メソッドは start 引数と end 引数を受け取り `Color` ます。

メソッドは、 `ColorTo` クラスのメソッドを使用してその機能を提供する拡張メソッドとして実装でき [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) ます。 これは、 `Animate` `double` 次のコード例に示すように、メソッドを使用して型ではないプロパティを対象にすることができるためです。

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

メソッドには、 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) コールバックメソッドである*変換*引数が必要です。 このコールバックへの入力は、常に `double` 0 ~ 1 の範囲内です。 したがって、 `ColorTo` メソッドは、 `Func` `double` 0 から1までの範囲を受け取り、その値に対応する値を返す独自の変換を定義し [`Color`](xref:Xamarin.Forms.Color) ます。 `Color`値は、指定された [`R`](xref:Xamarin.Forms.Color.R) [`G`](xref:Xamarin.Forms.Color.G) [`B`](xref:Xamarin.Forms.Color.B) [`A`](xref:Xamarin.Forms.Color.A) 2 つの引数の、、、および `Color` の各値を補間することによって計算されます。 `Color`値は、アプリケーションのコールバックメソッドに特定のプロパティに渡されます。

この方法では、 `ColorTo` [`Color`](xref:Xamarin.Forms.Color) 次のコード例に示すように、メソッドで任意のプロパティをアニメーション化できます。

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

このコード例では、メソッドは、のプロパティ `ColorTo` とプロパティ、ページのプロパティ、およびのプロパティをアニメーション化し [`TextColor`](xref:Xamarin.Forms.Label.TextColor) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`Label`](xref:Xamarin.Forms.Label) `BackgroundColor` [`Color`](xref:Xamarin.Forms.BoxView.Color) [`BoxView`](xref:Xamarin.Forms.BoxView) ます。

## <a name="related-links"></a>関連リンク

- [カスタムアニメーション (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [Animation API](xref:Xamarin.Forms.Animation)
- [アニメーション拡張 API](xref:Xamarin.Forms.AnimationExtensions)
