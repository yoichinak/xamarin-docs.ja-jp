---
title: 第 15 章の概要です。 対話型インターフェイス
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 15 章の概要。 対話型インターフェイス'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131102"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>第 15 章の概要です。 対話型インターフェイス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

この章では、ユーザーとの対話を可能にする8つの `View` 派生型について説明します。

## <a name="view-overview"></a>概要の表示

Xamarin. フォームには、`View` から派生し、`Layout`ではない、20個のインスタンス化可能なクラスが含まれています。 これらの 6 つ前の章で説明します。

- `Label`: [**第2章アプリの構造**](chapter02.md)
- `BoxView`: [**第3章スタックのスクロール**](chapter03.md)
- `Button`: [**第6章ボタンのクリック**](chapter06.md)
- `Image`: [**第13章ビットマップ**](chapter13.md)
- `ActivityIndicator`: [**第13章ビットマップ**](chapter13.md)
- `ProgressBar`: [**第14章AbsoluteLayout**](chapter14.md)

この章では 8 つのビューは、.NET の基本データ型と対話するユーザーを効果的に許可します。

|データ型|ビュー|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider)、[`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry)、 [`Editor`](xref:Xamarin.Forms.Editor)、 [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker)、[`TimePicker`](xref:Xamarin.Forms.TimePicker)|

これらのビューは、基になるデータ型のインタラクティブな視覚的な表現として考えることができます。 この概念については、次の章の章でさらに詳しく説明し[**ます。データバインディング**](chapter16.md)。

次の章では、残りの 6 つのビューが含まれます。

- `WebView`: [**第16章データバインディング**](chapter16.md)
- `Picker`: [**第19章コレクションビュー**](chapter19.md)
- `ListView`: [**第19章コレクションビュー**](chapter19.md)
- `TableView`: [**第19章コレクションビュー**](chapter19.md)
- `Map`: [**第28章場所とマップ**](chapter28.md)
- `OpenGLView`: 本書では取り上げていません (Windows プラットフォームはサポートされていません)

## <a name="slider-and-stepper"></a>スライダーとステッパ

[`Slider`](xref:Xamarin.Forms.Slider)と[`Stepper`](xref:Xamarin.Forms.Stepper)の両方で、ユーザーは範囲から数値を選択できます。 `Slider` は連続した範囲ですが、`Stepper` には不連続値が含まれます。

### <a name="slider-basics"></a>スライダーの基礎

[`Slider`](xref:Xamarin.Forms.Slider)は、左側の最小値から右側の最大値までの範囲の値を表す水平バーです。 これには、次の 3 つのパブリック プロパティを定義します。

- `double`型の[`Value`](xref:Xamarin.Forms.Slider.Value) 、既定値は0です。
- `double`型の[`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 、既定値は0です。
- `double`型の[`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 、既定値は1です。

これらのプロパティをバインドできるプロパティは、一貫性のあるいることを確認します。

- 3つのプロパティすべてについて、バインド可能なプロパティに指定された[`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate)メソッドによって、`Value` が `Minimum` と `Maximum`の間にあることが保証されます。
- `Minimum` が `Maximum`以上の値に設定されている場合、`MinimumProperty` の[`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate)メソッドは `false` を返します。 `MaximumProperty`に似ています。 `validateValue` メソッドから `false` を返すと、`ArgumentException` が発生します。

プログラムによって、またはユーザーが `Slider`を操作したときに `Value` プロパティが変更されたときに、 [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)引数を使用して[`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged)イベントを起動 `Slider` ます。

[**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo)サンプルでは、`Slider`を簡単に使用する方法を示します。

### <a name="common-pitfalls"></a>よくある落とし穴

コードと XAML の両方で、`Minimum` プロパティと `Maximum` プロパティは、指定した順序で設定されます。 `Maximum` が常に `Minimum`よりも大きいように、これらのプロパティを必ず初期化してください。 それ以外の場合は、例外が発生します。

`Slider` プロパティを初期化すると、`Value` プロパティが変更され、`ValueChanged` イベントが発生する可能性があります。 `Slider` イベントハンドラーが、ページの初期化中にまだ作成されていないビューにアクセスしないようにする必要があります。

`ValueChanged` イベントは、`Value` プロパティが変更されない限り、`Slider` の初期化中に発生しません。 `ValueChanged` ハンドラーは、コードから直接呼び出すことができます。

### <a name="slider-color-selection"></a>スライダーの色の選択

[**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders)プログラムには3つの `Slider` 要素が含まれており、RGB 値を指定して色を対話形式で選択できます。

[![R G B スライダーのトリプルスクリーンショット](images/ch15fg03-small.png "RGB スライダー")](images/ch15fg03-large.png#lightbox "RGB スライダー")

[**Textfade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade)サンプルでは、2つの `Slider` 要素を使用して、2つの `Label` 要素を `AbsoluteLayout` に移動し、一方を別の要素にフェードします。

### <a name="the-stepper-difference"></a>ステッパの違い

[`Stepper`](xref:Xamarin.Forms.Stepper)は `Slider` と同じプロパティとイベントを定義しますが、`Maximum` プロパティは100に初期化され、`Stepper` は4番目のプロパティを定義します。

- `double`型の[`Increment`](xref:Xamarin.Forms.Stepper.Increment) 。1に初期化されます。

視覚的には、`Stepper` は **&ndash;** と **+** というラベルが付いた2つのボタンで構成されています。 **&ndash;** を押すと、最小 `Minimum`に `Increment` によって `Value` が減少します。 **+** を押すと、最大 `Maximum`に `Increment` によって `Value` が増加します。

これは、 [**Stepperdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo)サンプルによって示されています。

## <a name="switch-and-checkbox"></a>スイッチとチェック ボックス

[`Switch`](xref:Xamarin.Forms.Switch)を使用すると、ユーザーはブール値を指定できます。

### <a name="switch-basics"></a>スイッチの基礎

視覚的には、`Switch` はオフにしてオンにできるトグルで構成されています。 クラスには、1 つのプロパティを定義します。

- [ 型の `IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)`bool`

`Switch` は1つのイベントを定義します。

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) 、 [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs)オブジェクトを伴い、`IsToggled` プロパティが変更されたときに発生します。

[**Switchdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo)プログラムは、`Switch`を示しています。

### <a name="a-traditional-checkbox"></a>従来のチェック ボックス

開発者によっては、`Switch`に対して従来の `CheckBox` を好むことがあります。 [**Xamarin. フォーム**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリには、`ContentView`から派生した `CheckBox` クラスが含まれています。 `CheckBox` は、 [CheckBox .xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml)および[CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs)ファイルによって実装されます。 `CheckBox` は、3つのプロパティ (`Text`、`FontSize`、および `IsChecked`) と、`CheckedChanged` イベントを定義します。

[**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo)サンプルでは、この `CheckBox`を示します。

## <a name="typing-text"></a>テキストを入力します。

Xamarin.Forms では、ユーザーを入力し、テキストを編集できる 3 つのビューを定義します。

- 1行のテキストの[`Entry`](xref:Xamarin.Forms.Entry)
- 複数行のテキストの[`Editor`](xref:Xamarin.Forms.Editor)
- 検索用に1行のテキストを[`SearchBar`](xref:Xamarin.Forms.SearchBar)します。

`Entry` と `Editor` は `View`から派生した[`InputView`](xref:Xamarin.Forms.InputView)から派生します。 `SearchBar` は `View`から直接派生します。

### <a name="keyboard-and-focus"></a>キーボードとフォーカス

スマートフォンやタブレットでは、`Entry`、`Editor`、および `SearchBar` の各要素によって、仮想キーボードがポップアップ表示されます。 画面上のこのキーボードの有無に関連する入力フォーカスをします。 入力フォーカスを取得するには、ビューの[`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)と[`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティの両方が `true` に設定されている必要があります。

入力フォーカスを持つ 2 つのメソッド、1 つの読み取り専用プロパティ、および 2 つのイベントがあります。 これらはすべて `VisualElement`によって定義されています。

- [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)メソッドは、入力フォーカスを要素に設定しようと試み、成功した場合は `true` を返します。
- [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)メソッドは、要素から入力フォーカスを削除します。
- [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)読み取り専用プロパティは、要素に入力フォーカスがあるかどうかを示します。
- [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)イベントは、要素が入力フォーカスを取得したことを示します。
- [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)イベントは、要素が入力フォーカスを失ったことを示します。

### <a name="choosing-the-keyboard"></a>キーボードの選択

`Entry` と `Editor` の派生元である[`InputView`](xref:Xamarin.Forms.InputView)クラスは、1つのプロパティのみを定義します。

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 型の [`Keyboard`](xref:Xamarin.Forms.Keyboard)

これは、表示されるキーボードの種類を示します。 Uri または数値は、一部のキーボードが最適化されています。

`Keyboard` クラスを使用すると、次のビットフラグを持つ列挙体である型[`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags)の引数を持つ静的[`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags))メソッドを使用して、キーボードを定義できます。

- 0に設定 `None`
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence)を1に設定します。
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck)を2に設定
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions)を4に設定します。
- \xFFFFFFFF に設定される[`All`](xref:Xamarin.Forms.KeyboardFlags.All)

段落または複数のテキストが必要なときに複数行[`Editor`](xref:Xamarin.Forms.Editor)を使用する場合、キーボードを選択するには `Keyboard.Create` を呼び出すことをお勧めします。 単一行[`Entry`](xref:Xamarin.Forms.Entry)の場合、`Keyboard` の次の静的な読み取り専用プロパティが役に立ちます。

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- 小数点の有無にかかわらず、正の数値を[`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric)します。

[`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter)では、 [**entrykeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards)プログラムで示すように、これらのプロパティを XAML で指定できます。

### <a name="entry-properties-and-events"></a>エントリのプロパティとイベント

単一行[`Entry`](xref:Xamarin.Forms.Entry)は、次のプロパティを定義します。

- `string`型の[`Text`](xref:Xamarin.Forms.InputView.Text) 、`Entry` に表示されるテキストです。
- [ 型の `TextColor`](xref:Xamarin.Forms.InputView.TextColor)`Color`
- [ 型の `FontFamily`](xref:Xamarin.Forms.Entry.FontFamily)`string`
- [ 型の `FontSize`](xref:Xamarin.Forms.Entry.FontSize)`double`
- [ 型の `FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes)`FontAttributes`
- 文字をマスクする `bool`型の[`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword)
- `string`型の[`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) 。何も入力しないうちに `Entry` に表示される dimly の色付きテキスト
- [ 型の `PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)`Color`

`Entry` には、次の2つのイベントも定義されています。

- `Text` プロパティが変更されるたびに発生する[`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)オブジェクトを使用して[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)します。
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)、ユーザーが終了し、キーボードが閉じたときに発生します。 ユーザーは、プラットフォーム固有の方法で完了を示します。

[**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations)サンプルでは、これら2つのイベントを示します。

### <a name="the-editor-difference"></a>エディターの違い

複数行[`Editor`](xref:Xamarin.Forms.Editor)は、同じ `Text` と `Font` プロパティを `Entry` として定義しますが、他のプロパティは定義しません。 また `Editor` は、`Entry`と同じ2つのプロパティも定義します。

[**ジャストノート**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes)は、`Editor`の内容を保存して復元する自由形式のノート作成プログラムです。

### <a name="the-searchbar"></a>SearchBar

[`SearchBar`](xref:Xamarin.Forms.SearchBar)は `InputView`から派生していないので、`Keyboard` のプロパティがありません。 ただし、`Entry` で定義されているすべての `Text`、`Font`、および `Placeholder` プロパティがあります。 さらに、`SearchBar` は次の3つのプロパティを定義します。

- [ 型の `CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)`Color`
- データバインディングと MVVM で使用する[`ICommand`](xref:System.Windows.Input.ICommand)型の[`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)
- で使用する `Object`型の[`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) `SearchCommand`

プラットフォーム固有では、テキスト ボタンの消去をキャンセルします。 `SearchBar` には、プラットフォーム固有の [検索] ボタンもあります。 これらのボタンのいずれかを押すと、`SearchBar` によって定義される2つのイベントのいずれかが発生します。

- [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)オブジェクトを伴う[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[**Searchbardemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo)サンプルでは、`SearchBar`について説明します。

## <a name="date-and-time-selection"></a>日付と時刻の選択

[`DatePicker`](xref:Xamarin.Forms.DatePicker)ビューおよび[`TimePicker`](xref:Xamarin.Forms.TimePicker)ビューは、ユーザーが日付または時刻を指定できるようにするプラットフォーム固有のコントロールを実装します。

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker)は、次の4つのプロパティを定義します。

- `DateTime`型の[`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 。1900年1月1日に初期化されます。
- `DateTime`型の[`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 。2100年12月31日に初期化されます。
- `DateTime`型の[`Date`](xref:Xamarin.Forms.DatePicker.Date) `DateTime.Today`
- `string`型の[`Format`](xref:Xamarin.Forms.DatePicker.Format) 、"d" に初期化された .net 書式指定文字列 (短い日付パターン)。この結果、US に "7/20/1969" のような日付が表示されます。

プロパティをプロパティ要素として表現し、カルチャに依存しない短い日付形式 ("7/20/1969") を使用することによって、XAML の `DateTime` プロパティを設定できます。   

[**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates)サンプルでは、ユーザーが選択した2つの日付の間の日数を計算します。

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (または、TimeSpanPicker ですか?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker)は、次の2つのプロパティを定義し、イベントはありません。

- [`Time`](xref:Xamarin.Forms.TimePicker.Time)は `DateTime`ではなく `TimeSpan` 型で、深夜から経過した時間を示します。
- `string`型の[`Format`](xref:Xamarin.Forms.TimePicker.Format) 、短い時刻パターンである "t" に初期化された .net 書式指定文字列。これにより、US に "1:45 PM" のような時間が表示されます。

[**SetTimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer)プログラムは、`TimePicker` を使用してタイマーの時間を指定する方法を示しています。 プログラムは、フォア グラウンドで保持する場合にのみ機能します。

また、 **SetTimer**は `Page` の[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))方法を使用して警告ボックスを表示する方法も示します。

## <a name="related-links"></a>関連リンク

- [第15章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [第15章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [エントリ](~/xamarin-forms/user-interface/text/entry.md)
- [[エディター]](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
