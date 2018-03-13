---
title: "16 章の概要です。 データ バインディング"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cf08874f66c9ab21cd0ede642c8c94821b6c5a2a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-16-data-binding"></a>16 章の概要です。 データ バインディング

プログラマは、多くの場合、不本意な状況を 1 つのオブジェクトのプロパティが変更されたときを検出するイベント ハンドラーを記述およびを使用して別のオブジェクトのプロパティの値を変更します。 方法でこのプロセスを自動化することができます*データ バインディング*です。 データ バインドは、通常 XAML で定義されているし、ユーザー インターフェイスの定義の一部になります。

非常に多くの場合、これらのデータ バインディングは、ユーザー インターフェイス オブジェクトを基になるデータに接続します。 これは、については、複数の手法[**第 18 章します。MVVM**](chapter18.md)です。 ただし、データ バインドは 2 つ以上のユーザー インターフェイス要素にも接続できます。 この章でのデータ バインディングの初期の例のほとんどは、この手法をデモンストレーションします。

## <a name="binding-basics"></a>バインドの基礎

いくつかのプロパティ、メソッド、およびクラスは、データ バインディングに関連します。

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)クラスから派生[ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/)データ バインディングの特性の多くをカプセル化
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)によってプロパティが定義されている、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラス
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)でメソッドが定義されても、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラス
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/)クラスには、3 台の追加を定義`SetBinding`メソッド

次の 2 つのクラスは、バインドの XAML マークアップ拡張機能をサポートします。

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) サポートしている、`Binding`マークアップ拡張機能
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) サポートしている、`x:Reference`マークアップ拡張機能

2 つのインターフェイスは、データ バインディングに関連します。

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) `System.ComponentModel`名前空間が、プロパティが変更されたときに通知を実装するためには
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 値をデータ バインド内の別の 1 つの型に変換する小さなクラスを定義するために使用します。

データ バインディングは、同一のオブジェクト、または 2 つの異なるオブジェクト (頻繁) の 2 つのプロパティを接続します。 これら 2 つのプロパティをいいます、*ソース*と*ターゲット*です。 通常、基になるプロパティの変更により、ターゲットで発生する変更が、方向を反転することがあります。 関係なく。

- *ターゲット*でプロパティをバックアップする必要があります、 [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *ソース*プロパティは、通常を実装するクラスのメンバー [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

実装するクラス`INotifyPropertyChanged`発生、 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/)イベント プロパティに値が変更されたときにします。 `BindableObject` 実装`INotifyPropertyChanged`自動的に発生し、`PropertyChanged`イベントによって、プロパティのバックアップ時に、`BindableProperty`が値を変更、書き込める、独自のクラスを実装する`INotifyPropertyChanged`から派生することがなく`BindableObject`です。

## <a name="code-and-xaml"></a>コードと XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode)サンプル コードで、データ バインディングを設定する方法を示します。

- ソースは、`Value`のプロパティ、 `Slider`
- ターゲットが、`Opacity`のプロパティ、 `Label`

設定して 2 つのオブジェクトが接続されている、`BindingContext`の`Label`オブジェクトを`Slider`オブジェクト。 2 つのプロパティを呼び出すことによって接続している、 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/)拡張メソッドを`Label`を参照する、`OpacityProperty`バインド可能なプロパティと`Value`のプロパティ、`Slider`として表現される、文字列。

操作する、`Slider`によって、`Label`フェードインおよびフェードアウトにします。

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) XAML で設定のデータ バインディングを使用して同じプログラムがします。 `BindingContext`の`Label`に設定されている、`x:Reference`マークアップ拡張機能を参照する、 `Slider`、および`Opacity`のプロパティ、`Label`に設定されている、`Binding`マークアップ拡張機能とその[ `Path`](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)プロパティを参照する、`Value`のプロパティ、`Slider`です。

## <a name="source-and-bindingcontext"></a>ソースと BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode)サンプル コードでその他の方法を示しています。 A`Binding`オブジェクトが設定して作成した、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/)プロパティを`Slider`オブジェクトおよび[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)プロパティを「値」にします。 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)メソッドの`BindableObject`で呼び出されるし、`Label`オブジェクト。

[ `Binding`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/)も使用できたを定義する、`Binding`オブジェクト。

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml)サンプル XAML で同等の方法を示しています。 `Opacity`のプロパティ、`Label`に設定されている、`Binding`マークアップ拡張機能を[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)に設定、`Value`プロパティと[ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/)に設定します。埋め込み`x:Reference`マークアップ拡張機能です。

要約するは、バインド ソース オブジェクトを参照する 2 つの方法があります。

- を介して、`BindingContext`ターゲットのプロパティ
- を介して、`Source`のプロパティ、`Binding`オブジェクト自体

両方を指定すると、2 つ目が優先されます。 利点、`BindingContext`ビジュアル ツリーを介した反映されることができます。 これは*非常に*複数のターゲット プロパティが、同じソース オブジェクトにバインドされている場合に便利です。

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo)プログラムはでは、この方法を示して、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)要素。 2 つ`Button`継承前後を移動するための要素、`BindingContext`を参照する、親の`WebView`です。 `IsEnabled` 2 つのボタンのプロパティは、単純なが`Binding`マークアップ拡張機能をボタンを対象とする`IsEnabled`プロパティの設定に基づきます、 [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/)と[ `CanGoForward`](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/)の読み取り専用のプロパティ、`WebView`です。

## <a name="the-binding-mode"></a>バインド モード

設定、 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/)プロパティの`Binding`のメンバーに、 [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/)列挙します。

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) ソース プロパティの変更がターゲットに影響するため
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) ターゲット プロパティの変更がソースに影響するため
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) ソースとターゲットの変更は互いに影響するため
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 使用する、 [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/)時に指定されたターゲット`BindableProperty`作成されました。 None が指定されている場合、既定では`OneWay`の通常のバインド可能なプロパティおよび`OneWayToSource`のバインド可能なプロパティが読み取り専用です。

一般に MVVM シナリオ内のデータ バインディングのターゲットをする可能性があるプロパティ、`DefaultBindingMode`の`TwoWay`します。 これらの数値は、次のとおりです。

- `Value` プロパティの`Slider`と `Stepper`
- `IsToggled` プロパティ `Switch`
- `Text` プロパティの`Entry`、 `Editor`、および `SearchBar`
- `Date` プロパティ `DatePicker`
- `Time` プロパティ `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes)サンプルでは、ターゲットの場所は、データ バインディングで、次の 4 つのバインド モード、`FontSize`のプロパティ、`Label`ソースは、 `Value`プロパティ、`Slider`です。 これにより、各`Slider`対応のフォント サイズを制御する`Label`です。 `Slider`ために、要素が初期化されません、`DefaultBindingMode`の`FontSize`プロパティは`OneWay`します。

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding)サンプルでのバインディングの設定、`Value`のプロパティ、`Slider`を参照する、`FontSize`の各プロパティ`Label`です。 逆の順序で表示されますが、初期化でより適切に動作が、`Slider`要素のため、`Value`のプロパティ、`Slider`が、`DefaultBindingMode`の`TwoWay`します。

[![バインドを逆のトリプル スクリーン ショット](images/ch16fg06-small.png "バインド反転")](images/ch16fg06-large.png#lightbox "バインド反転")

これは MVVM でのバインディングを定義する方法に似ています、この種類のバインドを頻繁に使用します。

## <a name="string-formatting"></a>文字列の書式設定

型のターゲット プロパティが`string`、使用することができます、 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/)プロパティによって定義された`BindingBase`をソースに変換する、`string`です。 設定、`StringFormat`プロパティ、静的で使用する文字列の書式設定、.NET を[ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/)オブジェクトを表示する形式。 この書式指定文字列内でのマークアップ拡張機能を使用する場合は、埋め込みのマークアップ拡張機能の中かっこと混同しないように単一引用符で囲みます。

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues)サンプルを使用する方法を示します`StringFormat`XAML でします。

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings)へのバインドで、ページのサイズを表示するサンプル、`Width`と`Height`のプロパティ、`ContentPage`です。

## <a name="why-is-it-called-path"></a>"Path"を理由と呼ばれますか。

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)プロパティ`Binding`と呼ばれる一連のプロパティとインデクサーのピリオドで区切られたであることができます。 [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos)サンプルがいくつかの例を示しています。

## <a name="binding-value-converters"></a>値コンバーターのバインド

バインディングのソースとターゲットのプロパティは、さまざまな種類が、バインドのコンバーターを使用して型の間で変換できます。 これは、クラスを実装するクラス、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)インターフェイスし、2 つのメソッドが含まれています: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/)ソースをターゲットに変換して[ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/)ターゲットをソースに変換します。

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリが変換するための使用例、`int`を`bool`です。 示されることは、 [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler)サンプルでは、有効にするだけ、`Button`に少なくとも 1 つの文字が型指定されている場合、`Entry`です。

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs)クラスに変換、`bool`を`string`にどのようなテキストを返す必要があるを指定する 2 つのプロパティを定義および`false`と`true`値。
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs)と似ています。 [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText)サンプルに基づいてさまざまな色に異なるテキストを表示するこれら 2 つのコンバーターの使用例、`Switch`設定します。

ジェネリック[ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs)置き換えることができます、`BoolToStringConverter`と`BoolToColorConverter`、一般化されたとして使用され、 `bool`-を-任意の型のオブジェクトのコンバーター。

## <a name="bindings-and-custom-views"></a>バインディングおよびカスタム ビュー

データ バインディングを使用してカスタム コントロールを簡略化することができます。 [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs)コード ファイルを定義`Text`、 `TextColor`、 `FontSize`、 `FontAttributes`、および`IsChecked`プロパティ、コントロールのビジュアルのすべてのロジックがありません。
代わりに、 [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml)ファイルでは、データ バインディングによってコントロールのビジュアルのすべてのマークアップを含むで、`Label`要素の分離コード ファイルで定義されたプロパティに基づきます。

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo)サンプルでは、`NewCheckBox`カスタム コントロールです。



## <a name="related-links"></a>関連リンク

- [16 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [16 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
