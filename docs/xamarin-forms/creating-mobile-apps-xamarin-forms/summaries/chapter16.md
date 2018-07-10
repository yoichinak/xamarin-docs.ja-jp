---
title: 16 章の概要です。 データ バインディング
description: 'Xamarin.Forms によるモバイル アプリの作成: 16 章の概要。 データ バインディング'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 92cf7f0163c4f074c718e86b06cf4830ff857c58
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935603"
---
# <a name="summary-of-chapter-16-data-binding"></a>16 章の概要です。 データ バインディング

多くの場合、プログラマは、1 つのオブジェクトのプロパティが変更されたときを検出するイベント ハンドラーの記述の湧くや、別のオブジェクトのプロパティの値を変更するに使用します。 この処理を自動化するための手法で*データ バインディング*します。 データ バインディングは、通常、XAML で定義されておよびユーザー インターフェイスの定義の一部になります。

非常に多くの場合、これらのデータ バインディングは、ユーザー インターフェイス オブジェクトを基になるデータに接続します。 これでさらに調査されている手法[**第 18 章です。MVVM**](chapter18.md)します。 ただし、データ バインドは 2 つ以上のユーザー インターフェイス要素にも接続できます。 この章では、データ バインディングの初期の例のほとんどは、この方法を示しています。

## <a name="binding-basics"></a>バインディングの基礎

データ バインディングでは、いくつかのプロパティ、メソッド、およびクラスが関係します。

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)クラスから派生[ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/)データ バインディングの特性の多くをカプセル化します。
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)によってプロパティが定義されている、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラス
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)でメソッドが定義されても、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラス
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/)クラスは、3 つの追加定義`SetBinding`メソッド

次の 2 つのクラスは、バインドの XAML マークアップ拡張機能をサポートします。

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) サポート、`Binding`マークアップ拡張機能
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) サポート、`x:Reference`マークアップ拡張機能

データ バインディングにおける 2 つのインターフェイスが含まれます。

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) `System.ComponentModel`名前空間がプロパティが変更されたときに通知を実装するためには
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 1 つの型から別のデータ バインドの値を変換する小さなクラスを定義するために使用します。

データ バインディングは、同一のオブジェクト、または 2 つの異なるオブジェクト (一般) の 2 つのプロパティを接続します。 これら 2 つのプロパティとして参照されます、*ソース*と*ターゲット*します。 通常、ソース プロパティの変更により、ターゲット プロパティに存在する変更が、方向を反転することがあります。 関係なく。

- *ターゲット*プロパティはバックアップする必要があります、 [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *ソース*プロパティは通常、実装するクラスのメンバー [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

実装するクラス`INotifyPropertyChanged`発生、 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/)イベント プロパティの値が変更されたとき。 `BindableObject` 実装`INotifyPropertyChanged`自動的に発生して、`PropertyChanged`イベントによってプロパティがバックアップされている場合、`BindableProperty`値を変更するが、実装する、独自のクラスを記述できます`INotifyPropertyChanged`から派生することがなく`BindableObject`します。

## <a name="code-and-xaml"></a>コードと XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode)サンプル コードでデータ バインディングを設定する方法を示します。

- ソースが、`Value`のプロパティを `Slider`
- ターゲットが、`Opacity`のプロパティを `Label`

設定して 2 つのオブジェクトが接続されている、`BindingContext`の`Label`オブジェクトを`Slider`オブジェクト。 2 つのプロパティが呼び出すことによって接続されている、 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/)拡張メソッドを`Label`を参照する、`OpacityProperty`バインド可能なプロパティと`Value`のプロパティ、`Slider`で表した、文字列。

操作、`Slider`なります、`Label`フェードインおよびフェードアウトします。

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml)は XAML で設定するデータ バインドで同じプログラムです。 `BindingContext`の`Label`に設定されている、`x:Reference`マークアップ拡張機能を参照する、 `Slider`、および`Opacity`のプロパティ、`Label`に設定されている、`Binding`マークアップ拡張機能をその[ `Path`](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)プロパティを参照する、`Value`のプロパティ、`Slider`します。

## <a name="source-and-bindingcontext"></a>ソースと BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode)サンプル コードでその他の方法を示しています。 A`Binding`設定してオブジェクトが作成される、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/)プロパティを`Slider`オブジェクトと[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)プロパティを「値」にします。 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)メソッドの`BindableObject`がで呼び出され、`Label`オブジェクト。

[ `Binding`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/)を定義することが使用されても、`Binding`オブジェクト。

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml)サンプル XAML で同等の方法を示しています。 `Opacity`のプロパティ、`Label`に設定されている、`Binding`マークアップ拡張機能を[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)に設定、`Value`プロパティと[ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/)に設定、埋め込み`x:Reference`マークアップ拡張機能。

要約するは、バインディング ソース オブジェクトを参照する 2 つの方法があります。

- を通じて、`BindingContext`ターゲットのプロパティ
- を通じて、`Source`のプロパティ、`Binding`オブジェクト自体

両方が指定されている場合は、2 つ目が優先されます。 利点、`BindingContext`ビジュアル ツリーを通じて伝達されるです。 これは*非常に*複数のターゲット プロパティが同じソース オブジェクトにバインドされている場合に便利です。

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo)プログラムでは、この方法を示して、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)要素。 2 つ`Button`継承前後を移動するための要素を`BindingContext`を参照する親の`WebView`します。 `IsEnabled`の 2 つのボタンのプロパティは、単純なが`Binding`マークアップ拡張機能をボタンを対象とする`IsEnabled`の設定に基づいて、プロパティ、 [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/)と[ `CanGoForward`](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/)の読み取り専用プロパティ、`WebView`します。

## <a name="the-binding-mode"></a>バインド モード

設定、 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/)プロパティの`Binding`のメンバーに、 [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/)列挙体。

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) ソース プロパティの変更がターゲットに影響するため
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) ターゲット プロパティの変更がソースに影響するため
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) ソースとターゲットの変更が互いに影響するため
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) 使用する、 [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode)ときに指定されたターゲット`BindableProperty`が作成されました。 指定されていない場合、既定値は`OneWay`の通常のバインド可能なプロパティと`OneWayToSource`のバインド可能なプロパティが読み取り専用です。

一般に MVVM シナリオでのデータ バインドの対象とする可能性があるプロパティを`DefaultBindingMode`の`TwoWay`します。 これらの数値は、次のとおりです。

- `Value` プロパティの`Slider`と `Stepper`
- `IsToggled` プロパティ `Switch`
- `Text` プロパティの`Entry`、`Editor`と `SearchBar`
- `Date` プロパティ `DatePicker`
- `Time` プロパティ `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes)サンプルでは、対象のデータ バインディングを次の 4 つのバインド モード、`FontSize`のプロパティを`Label`ソースであり、 `Value`プロパティを`Slider`します。 これにより、各`Slider`の対応するフォント サイズを制御する`Label`します。 `Slider`ために、要素が初期化されていない、`DefaultBindingMode`の`FontSize`プロパティは`OneWay`します。

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding)サンプルでバインディングの設定、`Value`のプロパティ、`Slider`を参照する、`FontSize`の各プロパティ`Label`します。 初期化でより適切にうまく機能する旧バージョンと、これが表示されます、`Slider`要素のため、`Value`のプロパティ、`Slider`が、`DefaultBindingMode`の`TwoWay`します。

[![バインドを逆の 3 倍になるスクリーン ショット](images/ch16fg06-small.png "バインド リバース")](images/ch16fg06-large.png#lightbox "バインドの反転")

これが MVVM では、バインドを定義する方法に似ており、この種類のバインドを頻繁に使用します。

## <a name="string-formatting"></a>文字列の書式設定

型のターゲット プロパティが`string`、使用することができます、 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/)プロパティによって定義された`BindingBase`をソースに変換する、`string`します。 設定、`StringFormat`プロパティは、静的で使用する文字列の書式設定、.NET を[ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/)形式オブジェクトを表示します。 マークアップ拡張機能内でこの書式指定文字列を使用する場合は、埋め込みのマークアップ拡張機能の中かっこと混同しないように単一引用符で囲みます。

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues)サンプルを使用する方法を示します`StringFormat`XAML でします。

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings)へのバインドと、ページのサイズを表示するサンプルに示します、`Width`と`Height`のプロパティ、`ContentPage`します。

## <a name="why-is-it-called-path"></a>"Path"を理由と呼ばれるでしょうか。

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)プロパティの`Binding`一連のプロパティとピリオドで区切られたインデクサーがあるためにと呼ばれるようにします。 [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos)サンプルがいくつかの例を示しています。

## <a name="binding-value-converters"></a>値コンバーターのバインディング

バインディングのソースとターゲットのプロパティは、さまざまな種類が、バインディング コンバーターを使用して型の間で変換できます。 これは、実装するクラス、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)インターフェイスし、2 つのメソッドが含まれています: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/)ソースをターゲットに変換して[ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/)ターゲットをソースに変換します。

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリは、変換の例を`int`を`bool`します。 示されていることが、 [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler)サンプルについては、できるだけ、`Button`に少なくとも 1 つの文字が入力した場合、`Entry`します。

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs)クラスに変換します、`bool`を`string`にどのようなテキストを返す必要があるかを指定する 2 つのプロパティを定義および`false`と`true`値。
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs)は似ています。 [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText)サンプルに基づいてさまざまな色に異なるテキストを表示するこれら 2 つのコンバーターの使用例、`Switch`設定します。

ジェネリック[ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs)置き換えることができます、`BoolToStringConverter`と`BoolToColorConverter`、一般化されたとして機能し、 `bool`-に-任意の型のオブジェクトのコンバーター。

## <a name="bindings-and-custom-views"></a>バインディングとカスタム ビュー

データ バインドを使用してカスタム コントロールを簡素化することができます。 [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs)コード ファイルを定義します`Text`、 `TextColor`、 `FontSize`、 `FontAttributes`、および`IsChecked`プロパティ、コントロールのビジュアルのすべてのロジックはありません。
代わりに、 [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml)ファイルがデータ バインドを使用で、コントロールのビジュアルのすべてのマークアップを含む、`Label`分離コード ファイルで定義されたプロパティに基づいて要素。

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo)サンプルでは、`NewCheckBox`カスタム コントロール。



## <a name="related-links"></a>関連リンク

- [16 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [16 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
