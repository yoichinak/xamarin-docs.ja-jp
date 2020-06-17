---
title:"第 27 章の概要: カスタム レンダラー" description:"Xamarin.Forms でモバイル アプリを作成する: 第 27 章カスタム レンダラーの概要 カスタム レンダラー" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:49961953-9336-4FD4-A42F-6D9B05FF52E7 author: davidbritch ms.author: dabritch ms.date:07/18/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-27-custom-renderers"></a>第 27 章カスタム レンダラーの概要 カスタム レンダラー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> このページの注記では、Xamarin.Forms が本に記載されている資料と異なる部分が示されています。

`Button` などの Xamarin.Forms 要素は、`ButtonRenderer` という名前のクラスにカプセル化されたプラットフォーム固有のボタンを使ってレンダリングされます。  [iOS バージョンの `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs)、[Android バージョンの `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)、および [UWP バージョンの `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs) をご確認ください。

この章では、独自のレンダラーを記述して、プラットフォーム固有のオブジェクトにマップするカスタム ビューを作成する方法について説明します。

## <a name="the-complete-class-hierarchy"></a>完全なクラス階層

Xamarin.Forms のプラットフォーム固有のコードを含むアセンブリには次の 4 つがあります。
次のリンクを使って、GitHub でソースを表示できます。

- [ **Xamarin.Forms.Platform**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (少量)
- [ **Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [ **Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [ **Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> 本書に記載されている `WinRT` アセンブリは、このソリューションの一部ではなくなりました。 

[**PlatformClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) サンプルでは、実行中のプラットフォームに対して有効なアセンブリのクラス階層を表示しています。

`ViewRenderer` という名前の重要なクラスがあることに気付くでしょう。 これは、プラットフォーム固有のレンダラーを作成するときに派生するクラスです。 これはターゲット プラットフォームのビュー システムに関連付けられているため、3 つの異なるバージョンに存在します。

iOS [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25) には、汎用引数があります。

- [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) に制約された `TView`
- [`UIKit.UIView`](xref:UIKit.UIView) に制約された `TNativeView`

Android [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17) には、汎用引数があります。

- [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) に制約された `TView`
- [`Android.Views.View`](xref:Android.Views.View) に制約された `TNativeView`

UWP [`ViewRenderer<TElement, TNativeElement>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6) には、異なる名前を持つ汎用引数があります。

- [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) に制約された `TElement`
- [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement) に制約された `TNativeElement`

レンダラーを記述するときは、`View` からクラスを派生させ、サポート対象プラットフォームごとに 1 つずつ、複数の `ViewRenderer` クラスを記述します。 各プラットフォーム固有の実装からは、`TNativeView` または `TNativeElement` パラメーターとして指定した型から派生するネイティブ クラスが参照されます。

## <a name="hello-custom-renderers"></a>Hello, custom renderers!

[**HelloRenderers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) プログラムは、自身の [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) クラスの `HelloView` という名前のカスタム ビューを参照します。

[`HelloView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) クラスは **HelloRenderers** プロジェクトに含まれており、単に `View` から派生します。

**HelloRenderers.iOS** プロジェクトの [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) クラスは、`ViewRenderer<HelloView, UILabel>` から派生します。 `OnElementChanged` のオーバーライドで、それはネイティブの iOS `UILabel` を作成して `SetNativeControl` を呼び出します。

**HelloRenderers.Droid** プロジェクトの [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) クラスは、`ViewRenderer<HelloView, TextView>` から派生します。 `OnElementChanged` のオーバーライドで、それは Android `TextView` を作成して `SetNativeControl` を呼び出します。

**HelloRenderers.UWP** とその他の Windows プロジェクトの [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) クラスは、`ViewRenderer<HelloView, TextBlock>` から派生します。 `OnElementChanged` のオーバーライドで、それは Windows `TextBlock` を作成して `SetNativeControl` を呼び出します。

`ViewRenderer` のすべての派生クラスには、`HelloView` クラスを特定の `HelloViewRenderer` クラスに関連付けるアセンブリ レベルの `ExportRenderer` 属性が含まれています。 これにより、Xamarin.Forms は個々のプラットフォーム プロジェクトでレンダラーを特定します。

[![Hello ビューのトリプル スクリーンショット](images/ch27fg02-small.png "カスタム レンダラー")](images/ch27fg02-large.png#lightbox "カスタム レンダラー")

## <a name="renderers-and-properties"></a>レンダラーとプロパティ

次のレンダラーのセットは楕円描画を実装しており、[**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) ソリューションのさまざまなプロジェクトに配置されています。

[`EllipseView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) クラスは、**Xamarin.FormsBook.Platform** プラットフォームにあります。 このクラスは `BoxView` に似ており、単一のプロパティ、`Color` 型の `Color` のみを定義します。

レンダラーは、レンダラーの `OnElementPropertyChanged` メソッドをオーバーライドすることにより、`View` に設定されたプロパティ値をネイティブ オブジェクトに転送できます。 このメソッド内 (およびほとんどのレンダラー内) で、次の 2 つのプロパティを使用できます。

- `Element`: Xamarin.Forms 要素
- `Control`: ネイティブ ビューまたはウィジェットまたはコントロール オブジェクト

これらのプロパティの型は、`ViewRenderer` へのジェネリック パラメーターによって決定されます。 この例では、`Element` の型は `EllipseView` です。

したがって、`OnElementPropertyChanged` のオーバーライドでは、`Element` の `Color` 値をネイティブの `Control` オブジェクトに転送できます (何らかの変換が必要になることがあります)。 次の 3 つのレンダラーがあります。

- iOS: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs)。楕円に [`EllipseUIView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) クラスを使用します。
- Android: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs)。楕円に [`EllipseDrawableView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) クラスを使用します。
- UWP: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs)。ネイティブの Windows [`Ellipse`](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) クラスを使用できます。

[**EllipseDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) クラスでは、これらの `EllipseView` オブジェクトのいくつかを表示しています。

[![楕円のデモのトリプル スクリーンショット](images/ch27fg03-small.png "EllipseView カスタム レンダラー")](images/ch27fg03-large.png#lightbox "EllipseView カスタム レンダラー")

[**BouncingBall**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) では、`EllipseView` が画面の端で跳ね返ります。

## <a name="renderers-and-events"></a>レンダラーとイベント

レンダラーでは、イベントを間接的に生成することもできます。 [`StepSlider`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) クラスは、通常の Xamarin.Forms `Slider` に似ていますが、`Minimum` 値と `Maximum` 値の間に複数の異なるステップを指定できます。

次の 3 つのレンダラーがあります。

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

レンダラーは、ネイティブ コントロールに対する変更を検出すると、`StepSlider` で定義されているバインド可能なプロパティを参照する `SetValueFromRenderer` を呼び出します (`StepSlider` によって `ValueChanged` イベントが開始されることになる変更)。

[**StepSliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) サンプルでは、この新しいスライダーの例を示しています。

## <a name="related-links"></a>関連リンク

- [第 27 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [第 27 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
