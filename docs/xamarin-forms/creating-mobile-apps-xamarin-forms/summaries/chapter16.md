---
title: '第 16 章の概要: データ バインディング'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 16 章の概要: データ バインディング'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ece93730100001e8339a5f50cdb7ac437d96fa62
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136735"
---
# <a name="summary-of-chapter-16-data-binding"></a>第 16 章の概要: データ バインディング

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)

> [!NOTE] 
> このページの注記では、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

プログラマーは、あるオブジェクトのプロパティが変更されたことを検出するイベント ハンドラーを作成し、それを使って別のオブジェクトのプロパティの値を変更することがよくあります。 このプロセスは、"*データ バインディング*" の手法を使用して自動化できます。 データ バインディングは、通常、XAML で定義され、ユーザー インターフェイスの定義の一部になります。

非常に多くの場合、このようなデータ バインディングによって、ユーザー インターフェイス オブジェクトが基になるデータに接続されます。 これは、次でさらに詳しく調べる手法です: [**第18章: MVVM**](chapter18.md)。 ただし、データ バインディングでは、2 つ以上のユーザー インターフェイス要素を接続することもできます。 この章に記載されているデータ バインディングの初期の例のほとんどは、この手法を示しています。

## <a name="binding-basics"></a>バインディングの基礎

データ バインディングでは、いくつかのプロパティ、メソッド、およびクラスが使用されます。

- [`Binding`](xref:Xamarin.Forms.Binding) クラスは [`BindingBase`](xref:Xamarin.Forms.BindingBase) から派生し、データ バインディングの多くの特性をカプセル化します
- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは、[`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスによって定義されます
- [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) メソッドも、[`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスによって定義されます
- [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) クラスは、3 つの追加の `SetBinding` メソッドを定義します

次の 2 つのクラスでは、バインディング用の XAML マークアップ拡張機能がサポートされています。

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) では `Binding` マークアップ拡張機能がサポートされています
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) では `x:Reference` マークアップ拡張機能がサポートされています

データ バインディングでは、次の 2 つのインターフェイスが使用されます。

- `System.ComponentModel` 名前空間の [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) を使って、プロパティが変更されたときの通知が実装されます
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) は、データ バインディングにおいて値をある型から別の型に変換する、小規模のクラスを定義するために使用されます

データ バインディングによって、同じオブジェクトの 2 つのプロパティ、または (より一般的には) 2 つの異なるオブジェクトの 2 つのプロパティが接続されます。 これらの 2 つのプロパティは、"*ソース*" と "*ターゲット*" と呼ばれます。 一般に、ソース プロパティの変更によってターゲット プロパティの変更が発生しますが、この方向は逆になることがあります。 いずれにしても:

- "*ターゲット*" プロパティは [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) によってサポートされている必要があります
- "*ソース*" プロパティは、通常、[`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) を実装するクラスのメンバーです

`INotifyPropertyChanged` を実装するクラスでは、プロパティの値が変更されると [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) イベントが発生します。 `BindableObject` では `INotifyPropertyChanged` が実装されているため、`BindableProperty` によってサポートされているプロパティの値が変更されると `PropertyChanged` イベントが自動的に発生します。ただし、`INotifyPropertyChanged` を実装する独自のクラスを、`BindableObject` から派生することなく作成することもできます。

## <a name="code-and-xaml"></a>コードと XAML

[**OpacityBindingCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) サンプルでは、コードでデータ バインディングを設定する方法が示されています。

- ソースは、`Slider` の `Value` プロパティです
- ターゲットは、`Label` の `Opacity` プロパティです

2 つのオブジェクトは、`Label` オブジェクトの `BindingContext` を `Slider` オブジェクトに設定することによって接続されます。 2 つのプロパティは、バインド可能なプロパティ `OpacityProperty` と、文字列として表される `Slider` の `Value` プロパティを参照する、`Label` の [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) 拡張メソッドを呼び出すことによって接続されます。

`Slider` を操作すると、`Label` の表示がフェード イン、フェード アウトされます。

[**OpacityBindingXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) は、XAML でデータ バインディングが設定された同じプログラムです。 `Label` の `BindingContext` は、`Slider` を参照する `x:Reference` マークアップ拡張に設定され、`Label` の `Opacity` プロパティは、`Slider` の `Value` プロパティを参照するその [`Path`](xref:Xamarin.Forms.Binding.Path) プロパティを使用して、`Binding` マークアップ拡張に設定されます。

## <a name="source-and-bindingcontext"></a>ソースと BindingContext

[**BindingSourceCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) サンプルでは、コードにおける別のアプローチが示されています。 `Binding` オブジェクトは、[`Source`](xref:Xamarin.Forms.Binding.Source) プロパティを `Slider` オブジェクトに設定し、[`Path`](xref:Xamarin.Forms.Binding.Path) プロパティを "Value" に設定することで作成されます。 次に `BindableObject` の [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) メソッドが、`Label` オブジェクトで呼び出されます。

[`Binding` コンストラクター](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) を使用して `Binding` オブジェクトを定義することもできます。

[**BindingSourceXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) サンプルには、XAML での同等の手法が示されています。 `Label` の `Opacity` プロパティは `Binding` マークアップ拡張に設定され、[`Path`](xref:Xamarin.Forms.Binding.Path) は `Value` プロパティに、[`Source`](xref:Xamarin.Forms.Binding.Source) は埋め込みの `x:Reference` マークアップ拡張に設定されます。

要約すると、バインディング ソース オブジェクトを参照するには、次の 2 つの方法があります。

- ターゲットの `BindingContext` プロパティを使用する
- `Binding` オブジェクト自体の `Source` プロパティを使用する

両方を指定した場合は、2 番目が優先されます。 `BindingContext` の利点は、それがビジュアル ツリーを通じて伝達されることです。 これは、複数のターゲット プロパティが同じソース オブジェクトにバインドされている場合に、"*非常に*" 便利です。

[**WebViewDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) プログラムでは、[`WebView`](xref:Xamarin.Forms.WebView) 要素を使ってこの手法が示されています。 前後に移動するための 2 つの `Button` 要素は、`WebView` を参照するその親から `BindingContext` を継承します。 次に、2 つのボタンの `IsEnabled` プロパティでは、`WebView` の読み取り専用プロパティ [`CanGoBack`](xref:Xamarin.Forms.WebView.CanGoBack) および [`CanGoForward`](xref:Xamarin.Forms.WebView.CanGoForward) の設定に基づいて、ボタンの `IsEnabled` プロパティをターゲットとする `Binding` マークアップ拡張が設定されています。

## <a name="the-binding-mode"></a>バインディング モード

`Binding` の [`Mode`](xref:Xamarin.Forms.BindingBase.Mode) プロパティを、次の [`BindingMode`](xref:Xamarin.Forms.BindingMode) 列挙型のメンバーに設定します。

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay): ソース プロパティに対する変更がターゲットに影響を与えるようにします
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource): ターゲット プロパティに対する変更がソースに影響を与えるようにします
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay): ソースとターゲットに対する変更が相互に影響を与えるようにします
- [`Default`](xref:Xamarin.Forms.BindingMode.Default): ターゲットの `BindableProperty` が作成されたときに指定された [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) を使用します。 何も指定されていなかった場合、既定値は、通常のバインド可能なプロパティの場合は `OneWay`、読み取り専用のバインド可能なプロパティの場合は `OneWayToSource` になります。

> [!NOTE]
> `BindingMode` 列挙型には、ソース プロパティが変更されたときではなく、バインディング コンテキストが変更されたときにのみバインディングを適用するための `OnTime` も含まれるようになりました。

MVVM シナリオにおいてデータ バインディングのターゲットとなる可能性のあるプロパティには、通常、`TwoWay` の `DefaultBindingMode` が設定されています。 これらのボタンの役割は、次のとおりです。

- `Slider` と `Stepper` の `Value` プロパティ
- `Switch` の `IsToggled` プロパティ
- `Entry`、`Editor`、`SearchBar` の `Text` プロパティ
- `DatePicker` の `Date` プロパティ
- `TimePicker` の `Time` プロパティ

[**BindingModes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) サンプルでは、ターゲットが `Label` の `FontSize` プロパティであり、ソースが `Slider` の `Value` プロパティであるデータ バインディングを使用した、4 つのバインディング モードが示されています。 これにより、各 `Slider` で対応する `Label` のフォント サイズを制御できるようになります。 ただし、`Slider` 要素は初期化されません。`FontSize` プロパティの `DefaultBindingMode` が `OneWay` であるためです。

[**ReverseBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) サンプルでは、各 `Label` の `FontSize` プロパティを参照する、`Slider` の `Value` プロパティのバインディングが設定されています。 これは後退しているように見えますが、`Slider` の `Value` プロパティの `DefaultBindingMode` が `TwoWay` であるため、`Slider` 要素をより適切に初期化することができます。

[![リバース バインディングのトリプル スクリーンショット](images/ch16fg06-small.png "Reverse Binding")](images/ch16fg06-large.png#lightbox "Reverse Binding")

これは、MVVM でバインディングが定義される方法に似ています。この種類のバインディングは頻繁に使用します。

## <a name="string-formatting"></a>文字列の書式設定

ターゲット プロパティの型が `string` の場合、`BindingBase` によって定義されている [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat) プロパティを使用して、ソースを `string` に変換できます。 オブジェクトを表示する静的な [`String.Format`](xref:System.String.Format(System.String,System.Object)) 書式設定を使用して、`StringFormat` プロパティを、使用する .NET 書式設定文字列に設定します。 マークアップ拡張内でこの書式設定文字列を使用する場合は、中かっこが埋め込みマークアップ拡張と間違えられないように、これを単一引用符で囲んでください。

[**ShowViewValues**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) サンプルでは、XAML で `StringFormat` を使用する方法が示されています。

[**WhatSizeBindings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) サンプルでは、`ContentPage` の `Width` および `Height` プロパティへのバインディングを使用して、ページのサイズを表示する方法が示されています。

## <a name="why-is-it-called-path"></a>"Path" と呼ばれる理由

`Binding` の [`Path`](xref:Xamarin.Forms.Binding.Path) プロパティがこのように呼ばれるのは、ピリオドで区切られた一連のプロパティとインデクサーを指定できるためです。 [**BindingPathDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) サンプルでは、いくつかの例が示されています。

## <a name="binding-value-converters"></a>バインディングの値コンバーター

バインディングのソース プロパティとターゲット プロパティが異なる型である場合は、バインディング コンバーターを使用して型の間で変換を行うことができます。 これは、[`IValueConverter`](xref:Xamarin.Forms.IValueConverter) インターフェイスを実装したクラスであり、2 つのメソッドが含まれています。ソースをターゲットに変換するための [`Convert`](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) と、ターゲットをソースに変換するための [`ConvertBack`](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) です。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`IntToBoolConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) クラスは、`int` を `bool` に変換する 1 つの例です。 これは [**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) サンプルで示されています。そこでは、少なくとも 1 つの文字が `Entry` に入力された場合にのみ、`Button` が有効になります。

[`BoolToStringConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) クラスでは、`bool` が `string` に変換され、`false` と `true` の値に対して返されるテキストを指定するために、2 つのプロパティが定義されています。
[`BoolToColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) も似ています。 [**SwitchText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) サンプルでは、これら 2 つのコンバーターを使用して、`Switch` 設定に基づいてさまざまなテキストをさまざまな色で表示する方法が示されています。

ジェネリックの [`BoolToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) では、`BoolToStringConverter` と `BoolToColorConverter` を置き換えることができます。これは、汎用的な、`bool` からオブジェクトへの任意の型のコンバーターとして機能します。

## <a name="bindings-and-custom-views"></a>バインディングとカスタム ビュー

データ バインディングを使用してカスタム コントロールを簡略化できます。 [`NewCheckBox.cs`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) コード ファイルでは、`Text`、`TextColor`、`FontSize`、`FontAttributes`、および `IsChecked` プロパティが定義されていますが、コントロールのビジュアル用のロジックはまったく含まれていません。
代わりに、[`NewCheckBox.cs.xaml`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) ファイルには、分離コード ファイル内で定義されているプロパティに基づき、`Label` 要素のデータ バインディングを使用して、コントロールのビジュアルに対するすべてのマークアップが含まれています。

[**NewCheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) サンプルでは、`NewCheckBox` カスタム コントロールが示されています。

## <a name="related-links"></a>関連リンク

- [第 16 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [第 16 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
