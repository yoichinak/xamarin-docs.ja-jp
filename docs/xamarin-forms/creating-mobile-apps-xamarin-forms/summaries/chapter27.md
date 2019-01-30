---
title: 第 27 章の概要です。 カスタム レンダラー
description: Xamarin.Forms によるモバイル アプリの作成。第 27 章の概要です。 カスタム レンダラー
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 96d06626fe0a8a4bb5aca59de454f707d4dfc731
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233875"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>第 27 章の概要です。 カスタム レンダラー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

など、Xamarin.Forms 要素`Button`という名前のクラスにカプセル化されたプラットフォームに固有のボタンでレンダリングされて`ButtonRenderer`します。  ここでは、[の iOS バージョン`ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs)、[の Android バージョン`ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)、および[UWP バージョンの`ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs)。

この章では、プラットフォーム固有のオブジェクトにマップするカスタム ビューを作成、独自のレンダラーを作成する方法について説明します。

## <a name="the-complete-class-hierarchy"></a>完全なクラス階層

Xamarin.Forms のプラットフォーム固有のコードが含まれている 4 つのアセンブリがあります。
これらのリンクを使用して GitHub でソースを表示できます。

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (非常に小さい)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> `WinRT`書籍に記載されているアセンブリは、このソリューションの一部では不要になった。 

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy)サンプルが実行されているプラットフォームに対して有効なアセンブリのクラス階層が表示されます。

という名前の重要なクラスがわかります`ViewRenderer`します。 これは、プラットフォーム固有のレンダラーを作成するときから派生するクラスです。 ターゲット プラットフォームのシステム ビューに結び付けられているので、次の 3 つの異なるバージョンで存在します。

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25)はジェネリック引数があります。

- `TView` 制限されます。 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` 制限されます。 [`UIKit.UIView`](xref:UIKit.UIView)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17)はジェネリック引数があります。

- `TView` 制限されます。 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` 制限されます。 [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

UWP [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6)ジェネリック引数をという名前が異なります。

- `TElement` 制限されます。 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` 制限されます。 [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement)

クラスを派生するレンダラーを作成するときに`View`を作成し、複数`ViewRenderer`クラス、サポートされているプラットフォームごとに 1 つ。 各プラットフォームに固有の実装はネイティブなクラスとして指定した型から派生した、参照、`TNativeView`または`TNativeElement`パラメーター。

## <a name="hello-custom-renderers"></a>こんにちは, カスタム レンダラー!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers)プログラムという名前のカスタム ビューが参照`HelloView`でその[ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs)クラス。

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs)でクラスが含まれる、 **HelloRenderers**プロジェクト、単にから派生して`View`します。

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs)クラス、 **HelloRenderers.iOS**から派生したプロジェクト`ViewRenderer<HelloView, UILabel>`します。 `OnElementChanged`のオーバーライドではネイティブの iOS 作成`UILabel`と呼び出し`SetNativeControl`します。

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs)クラス、 **HelloRenderers.Droid**から派生したプロジェクト`ViewRenderer<HelloView, TextView>`します。 `OnElementChanged`のオーバーライドでは、Android 作成`TextView`と呼び出し`SetNativeControl`します。

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs)クラス、 **HelloRenderers.UWP**から派生したその他の Windows プロジェクトと`ViewRenderer<HelloView, TextBlock>`します。 `OnElementChanged`のオーバーライドでは、Windows 作成`TextBlock`と呼び出し`SetNativeControl`します。

すべて、`ViewRenderer`派生物を含む、`ExportRenderer`を関連付けるアセンブリ レベルの属性、`HelloView`特定のクラス`HelloViewRenderer`クラス。 これは、Xamarin.Forms が個別のプラットフォーム プロジェクトのレンダラーを検索する方法です。

[![こんにちはビューのスクリーン ショットをトリプル](images/ch27fg02-small.png "カスタム レンダラー")](images/ch27fg02-large.png#lightbox "カスタム レンダラー")

## <a name="renderers-and-properties"></a>レンダラーとプロパティ

次の一連のレンダラーが楕円の描画を実装しのさまざまなプロジェクトが配置されている、 [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)ソリューション。

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs)クラスは、 **Xamarin.FormsBook.Platform**プラットフォーム。 クラスと似ています`BoxView`を 1 つのプロパティを定義します。`Color`型の`Color`します。

レンダラーに設定されたプロパティ値を転送できる、`View`をオーバーライドすることでネイティブ オブジェクトに、`OnElementPropertyChanged`レンダラーのメソッド。 このメソッド内 (およびレンダラーの多くは、)、2 つのプロパティを使用できます。

- `Element`、Xamarin.Forms 要素
- `Control`、、のネイティブ ビューまたはウィジェットまたはコントロールのオブジェクト

これらのプロパティの種類は、ジェネリック パラメーターによって決まります`ViewRenderer`します。 この例で`Element`の種類は`EllipseView`します。

`OnElementPropertyChanged`オーバーライドが転送できるため、`Color`の値、`Element`ネイティブ`Control`オブジェクトは、何らかの変換の可能性があります。 次の 3 つのレンダラーは次のとおりです。

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs)、使用、 [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs)楕円のクラス。
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs)、使用、 [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs)楕円のクラス。
- UWP: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs)、ネイティブの Windows を使用する[ `Ellipse` ](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)クラス。

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo)クラスでは、これらのいくつかが表示されます`EllipseView`オブジェクト。

[![楕円のデモのスクリーン ショットをトリプル](images/ch27fg03-small.png "EllipseView カスタム レンダラー")](images/ch27fg03-large.png#lightbox "EllipseView カスタム レンダラー")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) bounces、`EllipseView`画面の両側はオフです。

## <a name="renderers-and-events"></a>レンダラーとイベント

レンダラーでは直接イベントを生成することもできます。 [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs)クラスは、通常の Xamarin.Forms のような`Slider`間で個別のステップ数を指定できますが、`Minimum`と`Maximum`値。

次の 3 つのレンダラーは次のとおりです。

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

レンダラーがネイティブのコントロールへの変更を検出し、呼び出す`SetValueFromRenderer`で定義されているバインド可能なプロパティを参照する、 `StepSlider`、原因となるへの変更、`StepSlider`させる、`ValueChanged`イベント。

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo)サンプルでは、この新しいスライダー。



## <a name="related-links"></a>関連リンク

- [第 27 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [第 27 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
