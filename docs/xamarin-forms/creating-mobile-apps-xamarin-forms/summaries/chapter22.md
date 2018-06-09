---
title: 22 章の概要です。 アニメーション
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 22 章の概要です。 アニメーション'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b25fed9a86b82e56cb3b2bf5e3276c8ff63f4e35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241970"
---
# <a name="summary-of-chapter-22-animation"></a>22 章の概要です。 アニメーション

Xamarin.Forms タイマーを使用して、独自のアニメーションを作成することを確認したか、 `Task.Delay`Xamarin.Forms のアニメーション機能を使用して、通常より簡単にします。 3 つのクラスは、これらのアニメーションを実装します。

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)、高度なアプローチ
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)を困難になりますが、柔軟性の高い
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)で最も用途の広い最下位レベルのアプローチ

一般に、アニメーションは、バインド可能なプロパティでバックアップされているプロパティを対象します。 これは必須ではありませんが、これらは動的に変更に対応するプロパティだけです。

これらのアニメーションでは、XAML インターフェイスはありませんで説明した手法を使用して、XAML にアニメーションを統合することができます[**章 23 です。トリガーおよび動作**](chapter23.md)です。

## <a name="exploring-basic-animations"></a>基本的なアニメーションを調べる

基本的なアニメーション関数には、拡張メソッドで見つかった、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスです。 派生した任意のオブジェクトにこれらのメソッドを適用`VisualElement`です。 最も単純なアニメーション対象のプロパティの説明に変換[ `Chapter 21. Transforms`](chapter21.md)です。

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout)を示していますが、どのように`Clicked`のイベント ハンドラー、`Button`呼び出すことができます、 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)回転する拡張メソッド、円のボタンをクリックします。

`RotateTo`メソッドが変更された、`Rotation`のプロパティ、 `Button` 0 ~ 360 の (既定) の 4 分の 1 秒間にします。 場合、`Button`タップは、もう一度、ただし、これは何もため、`Rotation`プロパティが 360 度では既にです。

### <a name="setting-the-animation-duration"></a>アニメーションの継続時間を設定

2 番目の引数`RotateTo`は、時間 (ミリ秒)。 場合、大きな値に設定をタップすると、`Button`アニメーションの中には、新しいアニメーション開始位置として、現在の角度を開始します。

### <a name="relative-animations"></a>相対アニメーション

`RelRotateTo`メソッドは、既存の値に指定された値を追加することで相対的な回転を実行します。 このメソッドを使用する、`Button`複数回、および各タップするに時間が増加、 `Rotation` 360 ° プロパティです。

### <a name="awaiting-animations"></a>アニメーションを待機しています

内のすべてのアニメーション メソッド`ViewExtensions`返す`Task<bool>`オブジェクト。 これは、一連を使用してシーケンシャルなアニメーションを定義できることを意味`ContinueWith`または`await`です。 `bool`完了の戻り値は`false`アニメーションが中断されることがなく終了した場合または`true`によって取り消された場合、 [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/)によって開始されたすべてのアニメーションがキャンセルされるメソッド、その他のメソッドで`ViewExtensions`同じ要素に対して設定されます。

### <a name="composite-animations"></a>複合のアニメーション

複合アニメーションを作成して非待機の待機中のアニメーションを混在させることができます。 これらでのアニメーション`ViewExtensions`をターゲットとする、 `TranslatonX`、 `TranslationY`、および`Scale`変換プロパティ。

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

注意して`TranslateTo`可能性のある両方の影響を与える、`TranslationX`と`TranslationY`プロパティです。

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll と Task.WhenAny

使用して同時のアニメーションを管理することも[ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/)、複数のタスクがすべて終了されたとき、に通知して[ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/)、これがシグナル状態のいくつかの最初のタスクが完了します。

### <a name="rotation-and-anchors"></a>回転とアンカー

呼び出すときに、 `ScaleTo`、 `RelScaleTo`、 `RotateTo`、および`RelRotateTo`メソッドを設定できます、 [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)と[ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)プロパティを示すために、スケールおよび回転の中心です。

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton)を回転させてこの手法を示します、`Button`ページを中心にします。

### <a name="easing-functions"></a>イージング関数

通常、アニメーションは、終了値を開始値から線形。 イージング関数と、アニメーションを高速化、またはそのコース経由での速度が低下する可能性があります。 アニメーション メソッドに省略可能な最後の引数が型の[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)、型の 11 静的な読み取り専用のフィールドを定義するクラスを`Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/)、既定値
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/)、 [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/)、および [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/)、 [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/)、および [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) および [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) および [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

`In`サフィックスでは、アニメーションの開始時に効果があることを示します`Out`終了時に、ことを意味し、`InOut`の先頭と末尾のアニメーションであることを意味します。

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton)イージング関数の使用を示します。

### <a name="your-own-easing-functions"></a>イージング関数

定義することも、独自のイージング関数を渡すことによって、 [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/)を[`Easing`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/)です。 `Easing` 暗黙的な変換を定義も`Func<double, double>`に`Easing`です。 イージング関数の引数は常に 0 ~ 1 の範囲の最初から最後まで直線的に、アニメーションの進行中です。 関数は、*通常*0 ~ 1 の範囲の値を返しますが、簡単に負の値または 1 より大きい (の場合と同様、`SpringIn`と`SpringOut`関数) または実行していることがわかっている場合、規則を中断する可能性があります。

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale)サンプルでは、カスタム イージング関数と[ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase)別を示しています。

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton)またサンプルでは、カスタム イージング関数とも変更する方法、`AnchorX`と`AnchorY`回転アニメーションのシーケンス内のプロパティです。

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、 [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs)カスタム イージングを使用するクラスがクリックしてされたときにボタンを jiggle に機能します。 [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo)サンプルでは、ことを示しています。

### <a name="entrance-animations"></a>アニメーションの開始

アニメーションの 1 つの一般的な種類は、最初のページに表示されるときに発生します。 このようなアニメーションを開始することができます、 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/)ページの上書きします。 これらのアニメーション、表示するページの表示方法を XAML を設定する最善の*後*アニメーションを初期化して、コードからレイアウトをアニメーション化します。

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance)サンプルは、 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)ページの内容がフェードインする拡張メソッド。

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance)サンプルは、 [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)辺からページの内容にスライドする拡張メソッド。

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance)サンプルは、 [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)アニメーション化する拡張メソッド、`RotationY`プロパティです。 A [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドも使用します。

### <a name="forever-animations"></a>無限のアニメーション

その他の極端な例として、「無期限」アニメーション実行プログラムが終了するまでします。 これらは、デモンストレーションのために通常使用ものです。

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation)サンプルは[ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)アニメーションを 2 つのテキストのフェードインとフェードアウトします。

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) 、回文です。 を表示し、順番に個々 の文字 180 度回転すべて上下しています。 文字列全体を読み取り、元の文字列と同じに 180 ° 反転します。

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation)サンプルは、単純なを回転`BoxView`ヘリコプター中画面を中心として回転します。

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes)回転`BoxView`画面の中央を中心スポークし、それ自体の興味深いパターンを作成する各スポークを回転します。

[![スポーク回転のトリプル スクリーン ショット](images/ch22fg21-small.png "スポーク回転")](images/ch22fg21-large.png#lightbox "スポーク回転")

ただし、徐々 に増やすと、`Rotation`として、要素のプロパティは、長期的な機能しない可能性があります、 [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown)サンプルを示します。

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage)サンプルは[ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)、 [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)、および[ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)ビットマップが 3D スペースで回転させるかのように見えるようにします。

### <a name="animating-the-bounds-property"></a>境界のプロパティをアニメーション化

唯一の拡張メソッドで`ViewExtensions`が説明されていない[ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/)、これは、読み取り専用に効果的にアニメーション化[ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/)プロパティを呼び出して、 [`Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)メソッドです。 通常、このメソッドは呼び出される`Layout`派生型で説明するように[**章 26 です。CustomLayouts**](chapter26.md)です。

`LayoutTo`メソッドは特別な目的に限定する必要があります。 [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox)プログラムを使用して、圧縮し、展開を`BoxView`ように、ページの横の跳ね返ります。

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)サンプルは`LayoutTo`を移動するタイルで、クラシックの実装 15 ~ 16 パズル番号付きのタイルではなく、スクランブルされたイメージを表示します。

[![Xamarin Xuzzle のトリプル スクリーン ショット](images/ch22fg26-small.png "Xuzzle パズル ゲーム")](images/ch22fg26-large.png#lightbox "Xuzzle パズル ゲーム")

### <a name="your-own-awaitable-animations"></a>独自の待機可能アニメーション

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation)サンプルは、待機可能アニメーションを作成します。 返すことができる、重要なクラス、`Task`信号アニメーションが完了したときに、メソッドからのオブジェクトが[ `TaskCompletionSource`](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/)です。

## <a name="deeper-into-animations"></a>アニメーションに進む

Xamarin.Forms アニメーション システムは、少しわかりにくいできます。 加え、`Easing`クラス、アニメーションのシステムを構成、 `ViewExtensions`、 `Animation`、および`AnimationExtension`classses です。

### <a name="viewextensions-class"></a>ViewExtensions クラス

既に見た[ `ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)です。 返す 9 つのメソッドを定義`Task<bool>`と[ `CancelAnimations`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/)です。 9 つのメソッドのターゲットの 7 は変換のプロパティです。 その他の 2 つは[ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)、どのターゲット、 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/)プロパティ、および[ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/)、呼び出し、 [ `Layout`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)メソッドです。

### <a name="animation-class"></a>アニメーション クラス

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)クラスには、[コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/)コールバックし、完成したメソッド、およびアニメーションのパラメーターを定義する 5 つの引数を使用します。

追加できる子アニメーション[ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/)、 [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/)、 [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/)、およびとのオーバー ロード[ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

アニメーション オブジェクトは呼び出しを開始し、 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/)メソッドです。

### <a name="animationextensions-class"></a>AnimationExtensions クラス

[ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)クラスには、ほとんどの拡張メソッドが含まれています。 いくつかのバージョンの`Animate`メソッドおよびジェネリック[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/)メソッドは、必要な唯一のアニメーション関数が実際にある多様なためです。

## <a name="working-with-the-animation-class"></a>アニメーション クラスを使用した作業

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations)サンプルでは、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)いくつかの異なるアニメーションを持つクラス。

### <a name="child-animations"></a>子アニメーション

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations)またサンプルでは、(非常に似ています) のためのアニメーションを使用して子[ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/)と[ `Insert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/)メソッドです。

### <a name="beyond-the-high-level-animation-methods"></a>高度なアニメーション方法

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations)サンプルでのプロパティの対象以外にアニメーションを実行する方法も示します、`ViewExtensions`メソッドです。 1 つの例では、一連の期間の表示長くなります。別の例では、`BackgroundColor`プロパティをアニメーション化します。

### <a name="more-of-your-own-awaitable-methods"></a>複数の待機可能メソッド

[ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)メソッドの`ViewExtensions`では機能しません、 [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)関数。 それは、1 を超えるイージングの出力を取得したときに停止します。

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリに含まれる、 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)クラス[ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12)と[ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49)にこの問題がない拡張メソッドとして[ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44)と[ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71)ものを取り消すためのメソッドアニメーション。

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance)を示しています、`TranslateXTo`メソッドです。

`MoreExtensions`クラスも含まれています、 [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) X と Y の翻訳を結合する拡張メソッドと[ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113)メソッドです。

### <a name="implementing-a-bezier-animation"></a>ベジエ アニメーションを実装します。

ベジエ スプラインのパスに沿った要素を移動するアニメーションを開発することもできます。 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリに含まれる、 [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs)ベジエ スプラインをカプセル化する構造体と[ `BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs)コントロールの向きを列挙します。

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)クラスに含まれる、 [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118)拡張メソッドと[ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161)メソッドです。

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop)サンプルでは、Beizer パス上の要素をアニメーション化します。

## <a name="working-with-animationextensions"></a>AnimationExtensions の操作

標準のコレクションに表示されないのアニメーションの 1 つの型は、カラー アニメーションです。 この問題は 2 つの間を補間する適切な方法はありません`Color`値。 個々 の RGB 値を補間することが有効とだけが補間 HSL 値。

このため、 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには 2 つ`Color`アニメーション メソッド: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)と[ `HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188)です。 (2 つのキャンセル方法もあります。 [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183)と[ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206))。

両方のメソッドを使用するを使用して[ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211)、広範なジェネリックを呼び出すことによって、アニメーションを実行する[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/)メソッド[ `AnimationExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)です。

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations)サンプルでは、カラー アニメーションのこれら 2 つの型の使用方法を示します。

## <a name="structuring-your-animations"></a>アニメーションを構成します。

XAML でのアニメーションの express や、それらを使用する MVVM と組み合わせて役に立つ場合があります。 これについては、次の章で説明[**章 23 です。トリガーおよび動作**](chapter23.md)です。



## <a name="related-links"></a>関連リンク

- [22 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [22 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [アニメーション](~/xamarin-forms/user-interface/animation/index.md)
