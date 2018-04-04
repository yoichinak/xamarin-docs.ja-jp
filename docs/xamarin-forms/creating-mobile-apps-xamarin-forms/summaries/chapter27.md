---
title: 27 章の概要です。 カスタム レンダラー
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7a117373a11a057afabbad3c0c6005a9c2a49c3a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-27-custom-renderers"></a>27 章の概要です。 カスタム レンダラー

など、Xamarin.Forms 要素`Button`という名前のクラスにカプセル化されたプラットフォームに固有のボタンでレンダリングされて`ButtonRenderer`です。  ここでは、 [iOS 版の`ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs)では、[の Android バージョン`ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)、および[の Windows ランタイムのバージョン`ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs)です。

この章では、プラットフォーム固有のオブジェクトにマップするカスタム ビューを作成する、独自のレンダラーを記述する方法について説明します。

## <a name="the-complete-class-hierarchy"></a>完全なクラス階層

Xamarin.Forms プラットフォーム固有のコードを含む 7 つのアセンブリがあります。
これらのリンクを使用して GitHub のソースを表示できます。

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (非常に小さい)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (larger than the next three)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy)サンプルが実行されているプラットフォームに対して有効では、アセンブリのクラス階層を表示します。

という名前の重要なクラスかがわかります`ViewRenderer`です。 これは、プラットフォーム固有のレンダラーを作成するときから派生するクラスです。 3 つの異なるバージョンに存在するため、ターゲット プラットフォームのシステムの表示に関連付けられています。

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26)ジェネリック引数を持ちます。

- `TView` 制限 [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` 制限 [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14)ジェネリック引数を持ちます。

- `TView` 制限 [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` 制限 [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Windows ランタイム[ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12)ジェネリック引数をという名前が異なります。

- `TElement` 制限 [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` 制限 [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

クラスを派生レンダラーを作成するときに`View`、複数の書き込みと`ViewRenderer`クラス、サポートされているプラットフォームごとに 1 つです。 各プラットフォームに固有の実装は、ネイティブなとして指定した型から派生するクラスを参照、`TNativeView`または`TNativeElement`パラメーター。

## <a name="hello-custom-renderers"></a>カスタム レンダラー、こんにちは!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers)プログラムという名前のカスタム ビューが参照`HelloView`でその[ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs)クラスです。

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs)でクラスが含まれる、 **HelloRenderers**プロジェクトし、だけから派生`View`です。

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs)クラス内で、 **HelloRenderers.iOS**から派生したプロジェクト`ViewRenderer<HelloView, UILabel>`です。 `OnElementChanged` 、上書きを作成するネイティブの iOS`UILabel`と呼び出し`SetNativeControl`です。

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs)クラス内で、 **HelloRenderers.Droid**から派生したプロジェクト`ViewRenderer<HelloView, TextView>`です。 `OnElementChanged` 、上書きを作成、Android`TextView`と呼び出し`SetNativeControl`です。

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs)クラス内で、 **HelloRenderers.UWP**から派生したその他の Windows プロジェクト`ViewRenderer<HelloView, TextBlock>`です。 `OnElementChanged` Windows を作成、上書き、`TextBlock`と呼び出し`SetNativeControl`です。

すべて、`ViewRenderer`派生物を含む、`ExportRenderer`を関連付けるアセンブリ レベルの属性、`HelloView`特定した`HelloViewRenderer`クラスです。 これは、個々 のプラットフォーム プロジェクトでは、Xamarin.Forms がレンダラーを検索する方法です。

[![こんにちはビューのスクリーン ショットをトリプル](images/ch27fg02-small.png "カスタム レンダラー")](images/ch27fg02-large.png#lightbox "カスタム レンダラー")

## <a name="renderers-and-properties"></a>レンダラーとプロパティ

次の一連のレンダラーの楕円の描画を実装しのさまざまなプロジェクト内にある、 [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューションです。

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs)クラスが含まれて、 **Xamarin.FormsBook.Platform**プラットフォームです。 クラスがに似ていますが`BoxView`プロパティを 1 つだけを定義および:`Color`型の`Color`します。

レンダラーに設定されるプロパティ値を転送できます、`View`オーバーライドすることでネイティブ オブジェクトに、`OnElementPropertyChanged`レンダラー内のメソッドです。 このメソッド内で (およびレンダラーの多くは、)、2 つのプロパティを使用できます。

- `Element`、Xamarin.Forms 要素
- `Control`、ネイティブのビューまたはウィジェットまたはコントロールのオブジェクト

これらのプロパティの型パラメーターによって決定されます、ジェネリックに`ViewRenderer`です。 この例では`Element`の種類は`EllipseView`します。

`OnElementPropertyChanged`オーバーライドが転送できるため、`Color`の値、`Element`ネイティブに`Control`オブジェクト、ある種の変換の可能性があります。 次の 3 つのレンダラーは次のとおりです。

- iOS: [ `EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs)が使用される、 [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs)楕円のクラスです。
- Android: [ `EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs)が使用される、 [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs)楕円のクラスです。
- Windows ランタイム: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs)、ネイティブの Windows を使用してを[ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx)クラスです。

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo)クラスは、これらのいくつか表示`EllipseView`オブジェクト。

[![楕円デモのトリプル スクリーン ショット](images/ch27fg03-small.png "EllipseView カスタム レンダラー")](images/ch27fg03-large.png#lightbox "EllipseView カスタム レンダラー")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) bounces、`EllipseView`画面の辺はオフです。

## <a name="renderers-and-events"></a>レンダラーとイベント

レンダラーのイベントを直接生成することもできます。 [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs)クラスは通常 Xamarin.Forms のような`Slider`間で個別のステップの数を指定できますが、`Minimum`と`Maximum`値。

次の 3 つのレンダラーは次のとおりです。

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows ランタイム: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

レンダラーがネイティブのコントロールへの変更を検出し、呼び出す`SetValueFromRenderer`で定義されているバインド可能なプロパティを参照する、 `StepSlider`、どの原因への変更、`StepSlider`を起動、`ValueChanged`イベント。

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo)サンプルは、この新しいスライダーのつまみを示します。



## <a name="related-links"></a>関連リンク

- [章 27 フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [章 27 サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
