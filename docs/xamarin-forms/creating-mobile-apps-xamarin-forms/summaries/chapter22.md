---
title: 第 22 章の概要です。 アニメーション
description: Xamarin.Forms によるモバイル アプリの作成。第 22 章の概要です。 アニメーション
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 7b3695ce145c2ca58238e2c9a601923cbcefa182
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61333210"
---
# <a name="summary-of-chapter-22-animation"></a>第 22 章の概要です。 アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

Xamarin.Forms のタイマーを使用して、独自のアニメーションを作成できることを見てきましたまたは`Task.Delay`Xamarin.Forms のアニメーション機能を使用して、通常より簡単にします。 3 つのクラスは、これらのアニメーションを実装します。

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)、高レベルの方法
- [`Animation`](xref:Xamarin.Forms.Animation)、難しくなりますが、より汎用性の高い
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions)、、最も汎用性の高い、最下位レベルのアプローチ

一般に、アニメーションを対象にはバインド可能なプロパティで提供されるプロパティ。 これは必須ではありませんが、これらは、動的に変更に対応するプロパティだけです。

これらのアニメーションでは、XAML インターフェイスはありませんで説明した手法を使用して XAML にアニメーションを統合する[**第 23 章です。トリガーと動作**](chapter23.md)します。

## <a name="exploring-basic-animations"></a>基本的なアニメーションの調査

基本的なアニメーション機能が拡張メソッド、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラス。 派生した任意のオブジェクトにこれらのメソッドを適用`VisualElement`します。 最も単純なアニメーションのターゲットのプロパティの説明で変換[ `Chapter 21. Transforms`](chapter21.md)します。

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout)について説明する方法、`Clicked`のイベント ハンドラーを`Button`呼び出すことができます、 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))迅速に作成する拡張メソッド、円でボタンをクリックします。

`RotateTo`メソッドが変更された、`Rotation`のプロパティ、 `Button` 0 ~ 360 (既定) の 4 分の 1 秒間の過程でします。 場合、`Button`がタップされたもう一度、ただし、これは何も行いませんため、`Rotation`プロパティは、既に 360 度。

### <a name="setting-the-animation-duration"></a>アニメーションの時間の設定

2 番目の引数`RotateTo`は期間 (ミリ秒単位) です。 場合、大きな値に設定をタップすると、`Button`アニメーションの中には、新しいアニメーション開始位置として、現在の角度を開始します。

### <a name="relative-animations"></a>相対的なアニメーション

`RelRotateTo`メソッドは、既存の値に指定された値を追加することで相対的な回転を実行します。 この方法により、`Button`複数回、し、各をタップするに時間が増加、 `Rotation` 360 度のプロパティ。

### <a name="awaiting-animations"></a>アニメーションを待機しています

すべてのアニメーション メソッド`ViewExtensions`返す`Task<bool>`オブジェクト。 つまり、一連の順次を使用してアニメーションを定義できます`ContinueWith`または`await`します。 `bool`完了の戻り値は`false`アニメーションを中断することがなく完了している場合または`true`によって取り消された場合、 [ `CancelAnimation` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))メソッドによって開始されたすべてのアニメーションがキャンセルされる、その他のメソッドで`ViewExtensions`同じ要素に対して設定されています。

### <a name="composite-animations"></a>複合のアニメーション

複合のアニメーションを作成して非待機の待機中のアニメーションを混在させることができます。 これらのアニメーションは、`ViewExtensions`を対象とする、 `TranslationX`、 `TranslationY`、および`Scale`変換のプロパティ。

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

注意`TranslateTo`可能性のある両方の影響を与える、`TranslationX`と`TranslationY`プロパティ。

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll と Task.WhenAny

使用してアニメーションを同時に管理することも[ `Task.WhenAll` ](xref:System.Threading.Tasks.Task.WhenAll*)、複数のタスクがすべて終了、ときに通知して[ `Task.WhenAny` ](xref:System.Threading.Tasks.Task.WhenAny*)、ときに通知する最初のいくつかタスクは終了しました。

### <a name="rotation-and-anchors"></a>回転とアンカー

呼び出すときに、 `ScaleTo`、 `RelScaleTo`、`RotateTo`と`RelRotateTo`設定することができます、メソッド、 [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX)と[ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY)プロパティを示すために、拡大縮小および回転の中心。

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton)を回転させて、この手法を示します、`Button`ページの中心です。

### <a name="easing-functions"></a>イージング関数

通常、アニメーションは終了値には、開始値から線形です。 イージング関数と、アニメーションを高速化、またはその過程で速度が低下する可能性があります。 アニメーションのメソッドに省略可能な最後の引数が型の[ `Easing` ](xref:Xamarin.Forms.Easing)、型の 11 静的読み取り専用のフィールドを定義するクラス`Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear)、既定値
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)、 [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut)、と [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)、 [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut)、と [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) および [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) および [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

`In`サフィックスは、アニメーションの先頭に効果があることを示します`Out`最後に、ことを意味し、`InOut`先頭とアニメーションの終了になることを意味します。

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton)イージング関数の使用を示します。

### <a name="your-own-easing-functions"></a>独自のイージング関数

定義することもする独自のイージング関数を渡すことによって、 [ `Func<double, double>` ](xref:System.Func`2)を[`Easing`コンス トラクター](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double}))します。 `Easing` 暗黙的な変換を定義も`Func<double, double>`に`Easing`します。 イージング関数の引数は常に 0 ~ 1 の範囲内にアニメーションが最初から最後まで直線的に進みます。 関数は、*通常*0 ~ 1 の範囲の値を返しますが、負の値または 1 より大きいについて簡単にする可能性があります (の場合と同様、`SpringIn`と`SpringOut`関数) か、何をしていることがわかっている場合、ルールを破る可能性があります。

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale)サンプルでは、カスタム イージング関数と[ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase)別に示します。

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton)サンプルは、カスタム イージング関数とも変更する方法も示します、`AnchorX`と`AnchorY`回転アニメーションのシーケンス内のプロパティ。

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、 [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs)カスタム イージングを使用するクラスがクリックしてされたときにボタンを jiggle 関数。 [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo)ことを示します。

### <a name="entrance-animations"></a>Entrance のアニメーション

1 つの一般的な種類のアニメーションでは、最初のページに表示されるときに発生します。 このようなアニメーションを開始することができます、 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)ページのオーバーライドします。 これらのアニメーション、表示するページの XAML を設定することをお勧め*後*アニメーションを初期化して、コードからレイアウトをアニメーション化します。

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance)使用して、 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))ページの内容にフェードインする拡張メソッド。

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance)使用して、 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))辺からページの内容にスライドする拡張メソッド。

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance)使用して、 [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))をアニメーション化する拡張メソッド、`RotationY`プロパティ。 A [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドも使用します。

### <a name="forever-animations"></a>無限のアニメーション

その一方で、「無期限」アニメーション実行プログラムが終了するまでです。 これらはデモンストレーションのためのものでは一般にします。

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation)使用して[ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))アニメーションを 2 つのテキストをフェードインおよびフェードアウトします。

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation)回文ですが表示され、順番に個々 の文字に 180 度回転すべて上下ので。 文字列全体を読み取り、元の文字列と同じに 180 度反転します。

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation)サンプルは、単純なを回転`BoxView`ヘリコプター画面の中央の周りの回転中にします。

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes)回転`BoxView`画面を中心にスポークし、それ自体興味深いパターンを作成する各スポークを回転します。

[![スポークの回転の 3 倍になるスクリーン ショット](images/ch22fg21-small.png "スポーク回転")](images/ch22fg21-large.png#lightbox "スポークの回転")

ただし、段階的に増やすと、`Rotation`として、要素のプロパティは、長期動作しない可能性があります、 [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown)サンプルを示します。

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage)使用して[ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、 [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、および[ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))ビットマップが 3D 空間で回転させるかのように見えるようにします。

### <a name="animating-the-bounds-property"></a>Bounds プロパティをアニメーション化

唯一の拡張メソッドで`ViewExtensions`が説明されていない[ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))、これは、読み取り専用に効果的にアニメーション化[ `Bounds` ](xref:Xamarin.Forms.VisualElement.Bounds)プロパティを呼び出すことによって、 [`Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))メソッド。 このメソッドを呼び出してして通常`Layout`派生クラスで説明するように[**第 26 章です。CustomLayouts**](chapter26.md)します。

`LayoutTo`メソッドは特別な目的に限定する必要があります。 [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox)プログラムでの圧縮および展開に使用、`BoxView`ように、ページの側面から跳ね返ります。

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)使用して`LayoutTo`を移動するタイルで、従来の実装 15 ~ 16 パズル番号付きのタイルではなく、スクランブルされたイメージが表示されます。

[![Xamarin Xuzzle の 3 倍になるスクリーン ショット](images/ch22fg26-small.png "Xuzzle パズル ゲーム")](images/ch22fg26-large.png#lightbox "Xuzzle パズル ゲーム")

### <a name="your-own-awaitable-animations"></a>待機可能なアニメーション

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation)サンプルは、待機可能なアニメーションを作成します。 返すことができる重要なクラス、`Task`メソッドと、アニメーションが完了したときにシグナルからオブジェクトが[ `TaskCompletionSource`](xref:System.Threading.Tasks.TaskCompletionSource`1)します。

## <a name="deeper-into-animations"></a>アニメーションを深く

Xamarin.Forms のアニメーション システムは少し混乱することはできます。 加え、`Easing`クラス、アニメーション システムを構成、 `ViewExtensions`、 `Animation`、および`AnimationExtension`クラス。

### <a name="viewextensions-class"></a>ViewExtensions クラス

既に説明した[ `ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)します。 返す 9 つのメソッドを定義`Task<bool>`と[ `CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))します。 9 つのメソッド ターゲットの 7 は変換のプロパティです。 その他の 2 つは[ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、ターゲット、 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity)プロパティ、および[ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))、呼び出す、 [ `Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))メソッド。

### <a name="animation-class"></a>アニメーション クラス

[ `Animation` ](xref:Xamarin.Forms.AnimationExtensions)クラスには、[コンス トラクター](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action))コールバックと完成したメソッド、およびアニメーションのパラメーターを定義する 5 つの引数を使用します。

子アニメーションを使用して追加することができます[ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))、 [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation))、 [ `WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double))のオーバー ロードと[ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

アニメーション オブジェクトが、呼び出しで開始し、 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean}))メソッド。

### <a name="animationextensions-class"></a>AnimationExtensions クラス

[ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions)クラスには、ほとんどの拡張メソッドが含まれています。 いくつかのバージョン、`Animate`メソッドとジェネリック[ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*)メソッドは、必要なだけのアニメーション関数では実際に、汎用的なので。

## <a name="working-with-the-animation-class"></a>アニメーション クラスの使用

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations)サンプルでは、 [ `Animation` ](xref:Xamarin.Forms.Animation)でいくつかの異なるアニメーションのクラス。

### <a name="child-animations"></a>子アニメーション

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations)もサンプルでは、(非常に似ています) のことをアニメーションを使用して子[ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))と[ `Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation))メソッド。

### <a name="beyond-the-high-level-animation-methods"></a>高度なアニメーション方法

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations)を対象となるプロパティを上回るアニメーションを実行する方法も示します、`ViewExtensions`メソッド。 1 つの例では、一連の期間表示長い;別の例では、`BackgroundColor`プロパティをアニメーション化します。

### <a name="more-of-your-own-awaitable-methods"></a>待機可能な方法の詳細

[ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))メソッドの`ViewExtensions`では機能しません、 [ `Easing.SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut)関数。 1 を超えるイージングの出力を取得すると停止します。

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリが含まれています、 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)クラス[ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12)と[ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49)拡張メソッド、この問題がないだけでなく[ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44)と[ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71)ものを取り消すためのメソッドアニメーション。

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance)示します、`TranslateXTo`メソッド。

`MoreExtensions`クラスも含まれています、 [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) X と Y の翻訳を結合する拡張メソッドと[ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113)メソッド。

### <a name="implementing-a-bezier-animation"></a>ベジエ アニメーションを実装します。

ベジエ スプラインのパスに沿った要素を移動するアニメーションを開発することもできます。 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリが含まれています、 [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs)ベジエ スプラインをカプセル化する構造体と[ `BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs)コントロールの向きを列挙します。

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)クラスが含まれています、 [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118)拡張メソッドをおよび[ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161)メソッド。

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) Beizer パス上の要素をアニメーション化することを示します。

## <a name="working-with-animationextensions"></a>AnimationExtensions の操作

標準のコレクションから不足しているアニメーションの 1 つは、色のアニメーションです。 問題が 2 つの間を補間する適切な方法がない`Color`値。 個々 の RGB 値を補間することができますが、同様に妥当、HSL 値を補します。

このため、 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには 2 つ`Color`アニメーション メソッド: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)と[ `HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188)します。 (2 つのキャンセル メソッドもあります。 [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183)と[ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206))。

両方のメソッドを使用するを使用して、 [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211)、広範なジェネリックを呼び出すことによって、アニメーションを実行する[ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*)メソッド[ `AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions)します。

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations)サンプルでは、これら 2 つの種類の色のアニメーションの使用方法を示します。

## <a name="structuring-your-animations"></a>アニメーションの構成

XAML でアニメーションを表現し、それらを MVVM と組み合わせて使用すると便利ですがあります。 [次へ] の章では、これについては[**第 23 章です。トリガーと動作**](chapter23.md)します。



## <a name="related-links"></a>関連リンク

- [第 22 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [第 22 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [アニメーション](~/xamarin-forms/user-interface/animation/index.md)
