---
title: 第 22 章アニメーションの概要 アニメーション
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 22 章アニメーションの概要 アニメーション'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 935be5bd6696600644463eb4ec26410b546f42a0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "70770995"
---
# <a name="summary-of-chapter-22-animation"></a>第 22 章アニメーションの概要 アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

Xamarin.Forms のタイマーまたは `Task.Delay` を使って独自のアニメーションを作成できることを確認しましたが、通常は、Xamarin.Forms によって提供されるアニメーション機能を使う方が簡単です。 次の 3 つのクラスでこれらのアニメーションを実装できます。

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions): 高度なアプローチ
- [`Animation`](xref:Xamarin.Forms.Animation): 汎用性は高いが難易度も高いアプローチ
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions): 最も汎用性が高く簡単なアプローチ

通常、アニメーションでは、バインド可能なプロパティによってサポートされるプロパティをターゲットとします。 これは要件ではありませんが、これらのプロパティだけが変化に動的に対応できます。

これらのアニメーション用の XAML インターフェイスはありませんが、「[**第 23 章トリガーと動作**](chapter23.md)」で説明されている手法を使うと、アニメーションを XAML に統合できます。

## <a name="exploring-basic-animations"></a>基本的なアニメーションについて知る

基本的なアニメーションの関数は、[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) クラスにある拡張メソッドです。 これらのメソッドは、`VisualElement` から派生するすべてのオブジェクトに適用されます。 最もシンプルなアニメーションは、「[`Chapter 21. Transforms`](chapter21.md)」で説明されている変換プロパティをターゲットとしています。

[**AnimationTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) では、`Button` の `Clicked` イベント ハンドラーが、[`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 拡張メソッドを呼び出して円内のボタンを回転させる方法の例を示しています。

`RotateTo` メソッドは、`Button` の `Rotation` プロパティを 4 分の 1 秒 (既定) の間に 0 から 360 に変更します。 ただし、`Button` が再度タップされた場合は、`Rotation` プロパティが既に360 度になっているため何も行いません。

### <a name="setting-the-animation-duration"></a>アニメーションの時間の設定

`RotateTo` の 2 番目の引数は、ミリ秒単位の時間です。 これが大きな値に設定されている場合、アニメーションの実行中に `Button` をタップすることで、現在の角度から始まる新しいアニメーションが開始します。

### <a name="relative-animations"></a>相対アニメーション

`RelRotateTo` メソッドは、指定された値を既存の値に追加することで、相対回転を実行します。 このメソッドでは `Button` を複数回タップすることができ、その度に `Rotation` プロパティが 360 度ずつ増加します。

### <a name="awaiting-animations"></a>待機アニメーション

`ViewExtensions` 内のアニメーション メソッドはすべて `Task<bool>` オブジェクトを返します。 これは、`ContinueWith` または `await` を使って一連の連続するアニメーションを定義できることを意味します。 `bool` 完了の戻り値は、アニメーションが中断せずに完了した場合は `false`、[`CancelAnimation`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) メソッドによってキャンセルされた場合は `true` になります。このメソッドは、同じ要素に設定されている `ViewExtensions` 内の他のメソッドによって開始されたすべてのアニメーションをキャンセルします。

### <a name="composite-animations"></a>複合アニメーション

待機アニメーションと非待機アニメーションを組み合わせて、複合アニメーションを作成できます。 これらは、`TranslationX`、`TranslationY`、および `Scale` 変換プロパティをターゲットとする、`ViewExtensions` 内のアニメーションです。

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

`TranslateTo` は `TranslationX` と `TranslationY` の両方のプロパティに影響する可能性があることに注意してください。

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll と Task.WhenAny

複数のタスクがすべて完了したときに通知する [`Task.WhenAll`](xref:System.Threading.Tasks.Task.WhenAll*) と、複数のタスクの一番目が完了したときに通知する [`Task.WhenAny`](xref:System.Threading.Tasks.Task.WhenAny*) を使って、同時実行アニメーションを管理することもできます。

### <a name="rotation-and-anchors"></a>回転とアンカー

`ScaleTo`、`RelScaleTo`、`RotateTo`、および `RelRotateTo` の各メソッドを呼び出すときに、[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) および [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) プロパティを設定して、拡大縮小と回転の中心を示すことができます。

[**CircleButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) では、ページの中央を軸にして `Button` を回転させることで、この手法を示しています。

### <a name="easing-functions"></a>イージング関数

通常、アニメーションは開始値から終了値までの線形になります。 イージング関数を使うと、アニメーションの途中で速度が上下することがあります。 アニメーション メソッドの最後の省略可能な引数の型は [`Easing`](xref:Xamarin.Forms.Easing) です。これは、`Easing` 型の読み取り専用の 11 個の静的フィールドを定義するクラスです。

- [`Linear`](xref:Xamarin.Forms.Easing.Linear) (既定値)
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)、[`SinOut`](xref:Xamarin.Forms.Easing.SinOut)、および [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)、[`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut)、および [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) および [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) および [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

サフィックス `In` はアニメーションの先頭に、`Out` は末尾に、`InOut` はアニメーションの先頭と末尾にそれぞれ効果があることを示します。

[**BounceButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) サンプルでは、イージング関数の使い方を示しています。

### <a name="your-own-easing-functions"></a>独自のイージング関数

[`Func<double, double>`](xref:System.Func`2) を [`Easing` コンストラクター](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double}))に渡すことで、イージング関数を独自に定義することもできます。 `Easing` はまた、`Func<double, double>` から `Easing` への暗黙的な変換も定義します。 イージング関数の引数は、常に 0 から 1 の範囲内になります。これは、アニメーションが最初から最後まで線形で進むためです。 この関数は、"*通常は*" 0 から 1 の範囲の値を返しますが、(`SpringIn` や `SpringOut` 関数のように) 少しだけマイナスまたは 1 を超える値を返すこともできます。また、実行している内容がわかっている場合は、ルールを無視することもできます。

[**UneasyScale**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) サンプルでは、カスタム イージング関数の例を示し、[**CustomCubicEase**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) ではもう 1 つの例を示しています。

[**SwingButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) サンプルでは、同様にカスタム イージング関数の例を示しているほか、回転アニメーションのシーケンス内で `AnchorX` および `AnchorY` プロパティを変更する手法も示しています。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには [`JiggleButton`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) クラスがあり、カスタム イージング関数を使って、クリックされたボタンを揺らすことができます。 [**JiggleButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) サンプルでは、その方法を示しています。

### <a name="entrance-animations"></a>開始アニメーション

一般的なアニメーションの種類の 1 つに、ページが最初に表示されたときに発生するものがあります。 このようなアニメーションは、ページの [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) オーバーライドで開始できます。 これらのアニメーションの場合は、アニメーションの "*後に*" ページを表示する方法を XAML に設定し、コードからレイアウトを初期化してアニメーション化することをお勧めします。

[**FadingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) サンプルでは、[`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 拡張メソッドを使ってページのコンテンツをフェード インします。

[**SlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) サンプルでは、[`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 拡張メソッドを使ってページのコンテンツをサイドからスライド インします。

[**SwingingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) サンプルでは、[`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 拡張メソッドを使って `RotationY` プロパティをアニメーション化します。 [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) メソッドも使用できます。

### <a name="forever-animations"></a>無期限アニメーション

一方、"無期限" アニメーションは、プログラムが終了するまで実行されます。 これらは一般的に、デモンストレーションのみを目的としています。

[**FadingTextAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) サンプルでは、[`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) アニメーションを使って 2 つのテキストをフェード インおよびフェード アウトします。

[**PalindromeAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) では、回文を表示した後、各文字を順番に 180 度ずつ回転して、文字を上下に反転させます。 次に、元の文字列と同じになるように、文字列全体を 180 度反転させます。

[**CopterAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) サンプルでは、シンプルな `BoxView` ヘリコプターを回転させながら、画面の中央を軸にして旋回させます。

[**RotatingSpokes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) は、画面の中央を軸にして `BoxView` スポークを旋回させた後、各スポーク自体を回転させて興味深いパターンを作成します。

[![スポークの回転のトリプル スクリーンショット](images/ch22fg21-small.png "スポークの回転")](images/ch22fg21-large.png#lightbox "スポークの回転")

ただし、[**RotationBreakdown**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) サンプルで示しているように、要素の `Rotation` プロパティを段階的に増加させていくと、長期的には機能しなくなる可能性があります。

[**SpinningImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) サンプルでは、[`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、[`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、および [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) を使って、ビットマップが 3D 空間で回転しているように見せています。

### <a name="animating-the-bounds-property"></a>バインドされたプロパティのアニメーション化

最後に説明する `ViewExtensions` の拡張メソッドは、[`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) メソッドを呼び出すことで読み取り専用の [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) プロパティを効果的にアニメーション化する、[`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) です。 このメソッドは通常、`Layout` 派生によって呼び出されます。これについては、「[**第 26 章カスタム レイアウト**](chapter26.md)」で説明します。

`LayoutTo` メソッドは、特別な目的にのみ使用してください。 [**BouncingBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) プログラムではこれを使い、ページの端で跳ね返る `BoxView` を圧縮および拡張します。

[**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) サンプルでは、番号付きのタイルではなくスクランブルされた画像を表示するクラッシク ゲームの 15-16 パズルの実装で、`LayoutTo` を使ってタイルを移動します。

[![Xamarin Xuzzle のトリプル スクリーンショット](images/ch22fg26-small.png "Xuzzle パズル ゲーム")](images/ch22fg26-large.png#lightbox "Xuzzle パズル ゲーム")

### <a name="your-own-awaitable-animations"></a>独自の待機可能アニメーション

[**TryAwaitableAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) サンプルでは、待機可能アニメーションを作成します。 メソッドから `Task` オブジェクトを返し、アニメーションが完了したときに通知することができる重要なクラスは [`TaskCompletionSource`](xref:System.Threading.Tasks.TaskCompletionSource`1)です。

## <a name="deeper-into-animations"></a>アニメーションについてもっと詳しく知る

Xamarin.Forms アニメーション システムには、少しわかりにくい部分があります。 このアニメーション システムは、`Easing` クラスに加えて、`ViewExtensions`、`Animation`、および `AnimationExtension` クラスで構成されます。

### <a name="viewextensions-class"></a>ViewExtensions クラス

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) については、既に説明しました。 これは、`Task<bool>` と [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) を返す 9 つのメソッドを定義します。 9 つのメソッドのうち 7 つは、変換プロパティをターゲットとしています。 残りの 2 つは、[`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) プロパティをターゲットとする [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) と、[`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) メソッドを呼び出す [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) です。

### <a name="animation-class"></a>Animation クラス

[`Animation`](xref:Xamarin.Forms.AnimationExtensions) クラスの[コンストラクター](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action))には、コールバックと完了したメソッドを定義する 5 つの引数と、アニメーションのパラメーターを指定します。

子アニメーションを追加するには、[`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))、[`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation))、[`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double))、および [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)) のオーバーロードを使います。

アニメーション オブジェクトは、[`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) メソッドの呼び出しで開始されます。

### <a name="animationextensions-class"></a>AnimationExtensions クラス

[`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) クラスに含まれるのは、ほとんどが拡張メソッドです。 `Animate` メソッドにはいくつかのバージョンがありますが、ジェネリックな [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) メソッドは非常に汎用性が高いため、このアニメーション関数だけで十分と言えます。

## <a name="working-with-the-animation-class"></a>Animation クラスを使用する

[**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) サンプルでは、複数の異なるアニメーションを使った [`Animation`](xref:Xamarin.Forms.Animation) クラスの例を示しています。

### <a name="child-animations"></a>子アニメーション

[**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) サンプルでは、子アニメーションの例も示しており、(よく似た) [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) および [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) メソッドを利用しています。

### <a name="beyond-the-high-level-animation-methods"></a>さらに高度なアニメーション メソッド

[**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) サンプルでは、`ViewExtensions` メソッドのターゲット プロパティを超えるアニメーションを実行する方法も示しています。 1 つの例では一連の期間が長くなり、また別の例では `BackgroundColor` プロパティがアニメーション化されます。

### <a name="more-of-your-own-awaitable-methods"></a>独自の待機可能メソッドについての詳細

`ViewExtensions` の [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) メソッドは、[`Easing.SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 関数では機能しません。 イージング出力が 1 を超えると停止します。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) クラスがあり、この問題のない [`TranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) および [`TranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) 拡張メソッドのほか、これらのアニメーションをキャンセルするための [`CancelTranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) および [`CancelTranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) メソッドが含まれています。

[**SpringSlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) では、`TranslateXTo` メソッドの例を示しています。

`MoreExtensions` クラスには、X と Y の変換を組み合わせる [`TranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) 拡張メソッドと、[`CancelTranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) メソッドも含まれています。

### <a name="implementing-a-bezier-animation"></a>ベジエ アニメーションの実装

ベジエ スプラインのパスに沿って要素を移動するアニメーションを作成することもできます。 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには、ベジエ スプラインをカプセル化する [`BezierSpline`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) 構造体と、向きを制御する [`BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) 列挙型が含まれています。

[`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) クラスには、[`BezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) 拡張メソッドと [`CancelBezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) メソッドが含まれています。

[**BezierLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) サンプルでは、ベジエ パスに沿って要素をアニメーション化する例を示しています。

## <a name="working-with-animationextensions"></a>AnimationExtensions を使用する

標準コレクションに含まれていないアニメーションの 1 つに、カラー アニメーションがありす。 問題は、2 つの `Color` 値を補間する適切な方法がないことです。 個々の RGB 値を補間することはできますが、HSL 値を補間することも有効です。

このため、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) クラスには、2 つの `Color` アニメーション メソッド [`RgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166) および [`HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188) が含まれています (2 つのキャンセル メソッド [`CancelRgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) および [`CancelHslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206) もあります)。

どちらのメソッドでも [`ColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211) を利用し、[`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) で広範なジェネリック [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) メソッドを呼び出すことでアニメーションを実行します。

[**ColorAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) サンプルでは、これら 2 種類のカラー アニメーションの使用方法を示しています。

## <a name="structuring-your-animations"></a>アニメーションを構成する

XAML でアニメーションを表現し、それらを MVVM と組み合わせて使用すると便利な場合があります。 これについては、次の「[**第 23 章トリガーと動作**](chapter23.md)」で説明しています。

## <a name="related-links"></a>関連リンク

- [第 22 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [第 22 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [アニメーション](~/xamarin-forms/user-interface/animation/index.md)
