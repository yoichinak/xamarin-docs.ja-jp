---
title: '第 15 章の概要: 対話型インターフェイス'
description: 'Xamarin.Forms でモバイル アプリを作成する:第 15 章の概要: 対話型インターフェイス'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "77131102"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>第 15 章の概要: 対話型インターフェイス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

この章では、ユーザーとの対話を可能にする 8 つの `View` 派生型について説明します。

## <a name="view-overview"></a>ビューの概要

Xamarin.Forms には、`View` から派生し、`Layout` からは派生しない、20 個のインスタンス化可能なクラスが含まれています。 それらのうち 6 つについては、これまでの章で説明しました。

- `Label`:[**第 2 章: アプリの詳細**](chapter02.md)
- `BoxView`:[**第 3 章: スタックのスクロール**](chapter03.md)
- `Button`:[**第 6 章: ボタンのクリック**](chapter06.md)
- `Image`:[**第 13 章: ビットマップ**](chapter13.md)
- `ActivityIndicator`:[**第 13 章: ビットマップ**](chapter13.md)
- `ProgressBar`:[**第 14 章: AbsoluteLayout**](chapter14.md)

この章の 8 つのビューを使用すると、ユーザーは基本的な .NET データ型を効果的に操作できます。

|データの種類|Views|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider)、[`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry)、[`Editor`](xref:Xamarin.Forms.Editor)、[`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker)、[`TimePicker`](xref:Xamarin.Forms.TimePicker)|

これらのビューは、基になるデータ型を視覚的に対話形式で表現したものと考えることができます。 この概念については、次の章「[**第 16 章: データ バインディング**](chapter16.md)」で詳しく説明します。

残りの 6 つのビューについては、以下の章で説明します。

- `WebView`:[**第 16 章: データ バインディング**](chapter16.md)
- `Picker`:[**第 19 章: コレクション ビュー**](chapter19.md)
- `ListView`:[**第 19 章: コレクション ビュー**](chapter19.md)
- `TableView`:[**第 19 章: コレクション ビュー**](chapter19.md)
- `Map`:[**第 28 章: 場所とマップ**](chapter28.md)
- `OpenGLView`:このブックでは説明されていません (Windows プラットフォームはサポートされていません)

## <a name="slider-and-stepper"></a>スライダーとステッパー

[`Slider`](xref:Xamarin.Forms.Slider) と [`Stepper`](xref:Xamarin.Forms.Stepper) の両方で、ユーザーは範囲から数値を選択できます。 `Slider` は連続した範囲ですが、`Stepper` には不連続値が含まれます。

### <a name="slider-basics"></a>スライダーの基本

[`Slider`](xref:Xamarin.Forms.Slider) は水平方向のバーで、左側の最小値から右側の最大値までの範囲の値を表します。 次の 3 つのパブリック プロパティが定義されます。

- `double` 型の [`Value`](xref:Xamarin.Forms.Slider.Value) (既定値 0)
- `double` 型の [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) (既定値 0)
- `double` 型の [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) (既定値 1)

これらのプロパティに使用されるバインド可能なプロパティによって、一貫性が確保されます。

- 3 つのプロパティすべてについて、バインド可能なプロパティに指定された [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) メソッドによって、`Value` が `Minimum` と `Maximum` の間にあることが確実になります。
- `MinimumProperty` の [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) メソッドでは、`Minimum` が `Maximum` 以上の値に設定されている場合は `false` が返されます。`MaximumProperty` についても同様です。 `validateValue` メソッドから `false` が返されると、`ArgumentException` が発生します。

`Slider` では、`Value` プロパティが (プログラムによって、またはユーザーが `Slider` を操作したときに) 変更された場合、[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) 引数を使用した [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) イベントが発生します。

[**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) サンプルに `Slider` の簡単な使用方法が示されています。

### <a name="common-pitfalls"></a>よくある落とし穴

コードと XAML の両方で、`Minimum` プロパティと `Maximum` プロパティは、指定した順序で設定されます。 これらのプロパティは、`Maximum` が常に `Minimum` よりも大きくなるように初期化してください。 それ以外の場合は、例外が発生します。

`Slider` プロパティを初期化すると、`Value` プロパティが変更され、`ValueChanged` イベントが発生する可能性があります。 ページの初期化中に、まだ作成されていないビューに `Slider` イベント ハンドラーがアクセスしないようにする必要があります。

`ValueChanged` イベントは、`Value` プロパティが変更されない限り、`Slider` の初期化中に発生することはありません。 `ValueChanged` ハンドラーは、コードから直接呼び出すことができます。

### <a name="slider-color-selection"></a>スライダーの色の選択

[**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) プログラムには、RGB 値を指定して色を対話形式で選択できる、3 つの `Slider` 要素が含まれています。

[![R G B スライダーのトリプル スクリーンショット](images/ch15fg03-small.png "RGB スライダー")](images/ch15fg03-large.png#lightbox "RGB スライダー")

[**TextFade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) サンプルでは、2 つの `Slider` 要素を使用して、2 つの `Label` 要素を `AbsoluteLayout` 全体で移動し、一方をもう一方にフェードインしています。

### <a name="the-stepper-difference"></a>ステッパーの相違点

[`Stepper`](xref:Xamarin.Forms.Stepper) で定義されるプロパティとイベントは `Slider` と同じですが、`Maximum` プロパティは 100 に初期化され、`Stepper` では 4 つ目のプロパティが定義されます。

- `double` 型の [`Increment`](xref:Xamarin.Forms.Stepper.Increment) (1 に初期化されます)

視覚的には、`Stepper` は **&ndash;** および **+** というラベルの 2 つのボタンで構成されます。 **&ndash;** を押すことで、`Value` が `Increment` ずつ `Minimum` の最小値まで減少します。 **+** を押すと、`Value` は `Increment` ずつ `Maximum` の最大値まで増加します。

これは、[**StepperDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) サンプルで示されています。

## <a name="switch-and-checkbox"></a>スイッチとチェックボックス

[`Switch`](xref:Xamarin.Forms.Switch) では、ユーザーがブール値を指定できます。

### <a name="switch-basics"></a>スイッチの基本

視覚的には、`Switch` はオンとオフの切り替えができるトグルで構成されます。 このクラスでは、1 つのプロパティが定義されます。

- `bool` 型の [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)

`Switch` では 1 つのイベントが定義されます。

- [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) オブジェクトを伴う [`Toggled`](xref:Xamarin.Forms.Switch.Toggled)。`IsToggled` プロパティが変更されたときに発生します。

[**SwitchDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) プログラムに `Switch` が示されています。

### <a name="a-traditional-checkbox"></a>従来のチェックボックス

開発者によっては、`Switch` よりも従来の `CheckBox` が好まれる場合があります。 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには、`ContentView` から派生する `CheckBox` クラスが含まれています。 `CheckBox` は [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) ファイルと [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) ファイルによって実装されます。 `CheckBox` では 3 つのプロパティ (`Text`、`FontSize`、`IsChecked`) と `CheckedChanged` イベントが定義されます。

[**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) サンプルに、この `CheckBox` が示されています。

## <a name="typing-text"></a>テキスト入力

Xamarin.Forms には、ユーザーがテキストの入力と編集ができるようにする 3 つのビューが定義されています。

- 1 行のテキスト用の [`Entry`](xref:Xamarin.Forms.Entry)
- 複数行のテキスト用の [`Editor`](xref:Xamarin.Forms.Editor)
- 検索を目的とした 1 行のテキスト用の [`SearchBar`](xref:Xamarin.Forms.SearchBar)

`Entry` と `Editor` は、`View` の派生型である [`InputView`](xref:Xamarin.Forms.InputView) から派生します。 `SearchBar` は `View` から直接派生します。

### <a name="keyboard-and-focus"></a>キーボードとフォーカス

物理的なキーボードがないスマートフォンやタブレットでは、`Entry`、`Editor`、`SearchBar` の各要素すべてによって、仮想キーボードがポップアップ表示されます。 このキーボードが画面に表示されるかどうかは、入力フォーカスに関連しています。 ビューは、入力フォーカスを取得するために、[`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) プロパティと [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) プロパティの両方が `true` に設定されている必要があります。

2 つのメソッド、1 つの読み取り専用プロパティ、2 つのイベントが入力フォーカスに関係します。 これらはすべて、`VisualElement` によって定義されます。

- [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) メソッドでは、入力フォーカスを要素に設定することが試みられ、成功した場合は `true` が返されます。
- [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) メソッドでは、要素から入力フォーカスが削除されます。
- [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) 読み取り専用プロパティでは、要素に入力フォーカスがあるかどうかが示されます。
- [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) イベントによって、要素が入力フォーカスを取得したことが示されます。
- [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) イベントによって、要素が入力フォーカスを失ったことが示されます。

### <a name="choosing-the-keyboard"></a>キーボードの選択

`Entry` と `Editor` の派生元である [`InputView`](xref:Xamarin.Forms.InputView) では、1 つのプロパティだけが定義されます。

- [`Keyboard`](xref:Xamarin.Forms.Keyboard) 型の [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)

これにより、表示されるキーボードの種類が示されます。 一部のキーボードは、URI または数値用に最適化されています。

`Keyboard` クラスを使用すると、静的な [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) メソッドで、次のビット フラグを持つ列挙型である [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 型の引数を持つキーボードを定義できます。

- 0 に設定された `None`
- 1 に設定された [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence)
- 2 に設定された [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck)
- 4 に設定された [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions)
- \xFFFFFFFF に設定された [`All`](xref:Xamarin.Forms.KeyboardFlags.All)

1 段落以上のテキストが必要な場合に複数行の [`Editor`](xref:Xamarin.Forms.Editor) を使用するときは、キーボードを選択する方法として、`Keyboard.Create` を呼び出すことをお勧めします。 単一行の [`Entry`](xref:Xamarin.Forms.Entry) の場合、次に示す `Keyboard` の静的な読み取り専用プロパティが役に立ちます。

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) (小数点の有無にかかわらず、正の数値用)。

[`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) を使用すると、[**EntryKeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) プログラムに示されているように、これらのプロパティを XAML で指定できます。

### <a name="entry-properties-and-events"></a>入力のプロパティとイベント

単一行の [`Entry`](xref:Xamarin.Forms.Entry) では次のプロパティが定義されます。

- `string` 型の [`Text`](xref:Xamarin.Forms.InputView.Text) (`Entry` に表示されるテキスト)
- `Color` 型の [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)
- `string` 型の [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily)
- `double` 型の [`FontSize`](xref:Xamarin.Forms.Entry.FontSize)
- `FontAttributes` 型の [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes)
- `bool` 型の [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) (文字がマスクされます)
- `string` 型の [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) (何かが入力される前に `Entry` に表示される、薄い色のテキスト)
- `Color` 型の [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)

`Entry` では、2 つのイベントも定義されます。

- [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) オブジェクトを伴う [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) (`Text` プロパティが変更されたときに発生します)
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) (ユーザーの操作が終了し、キーボードが閉じられたときに発生します)。 ユーザーが完了を示す方法はプラットフォームに固有です。

[**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) サンプルに、これら 2 つのイベントが示されています。

### <a name="the-editor-difference"></a>エディターの相違点

複数行の [`Editor`](xref:Xamarin.Forms.Editor) では、`Entry` と同じ `Text` プロパティと `Font` プロパティが定義されますが、その他のプロパティは定義されません。 `Editor` では、`Entry` と同じ 2 つのプロパティも定義されます。

[**JustNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) は、`Editor` の内容を保存して復元する自由形式のノート作成プログラムです。

### <a name="the-searchbar"></a>SearchBar

[`SearchBar`](xref:Xamarin.Forms.SearchBar) は `InputView` から派生しないため、`Keyboard` プロパティがありません。 ただし、`Entry` で定義される `Text`、`Font`、`Placeholder` のすべてのプロパティがあります。 さらに、`SearchBar` では 3 つの追加プロパティが定義されます。

- `Color` 型の [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)
- [`ICommand`](xref:System.Windows.Input.ICommand) 型の [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) (データ バインディングと MVVM で使用します)
- `Object` 型の [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) (`SearchCommand` で使用します)

プラットフォーム固有のキャンセル ボタンでテキストが消去されます。 `SearchBar` には、プラットフォーム固有の検索ボタンもあります。 これらのボタンのいずれかを押すと、`SearchBar` によって定義される 2 つのイベントのいずれかが発生します。

- [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) オブジェクトを伴う [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[**SearchBarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) サンプルに、`SearchBar` が示されています。

## <a name="date-and-time-selection"></a>日付と時刻の選択

[`DatePicker`](xref:Xamarin.Forms.DatePicker) ビューと [`TimePicker`](xref:Xamarin.Forms.TimePicker) ビューでは、ユーザーが日付または時刻を指定できるプラットフォーム固有のコントロールが実装されます。

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) では 4 つのプロパティが定義されます。

- `DateTime` 型の [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) (1900 年 1 月 1 日に初期化されます)
- `DateTime` 型の [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) (2100 年 12 月 31 日に初期化されます)
- `DateTime` 型の [`Date`](xref:Xamarin.Forms.DatePicker.Date) (`DateTime.Today` に初期化されます)
- `string` 型の [`Format`](xref:Xamarin.Forms.DatePicker.Format) (短い日付のパターンである "d" に初期化された .NET 書式指定文字列。米国では "7/20/1969" のような日付表示になります)

プロパティをプロパティ要素として表現し、カルチャに依存しない短い日付形式 ("7/20/1969") を使用することによって、XAML で `DateTime` プロパティを設定できます。   

[**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) サンプルでは、ユーザーが選択した 2 つの日付の間の日数を計算しています。

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (または TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) では 2 つのプロパティを定義し、イベントは定義しません。

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) は、`DateTime` 型ではなく `TimeSpan` 型であり、午前 0 時からの経過時間を示します。
- `string` 型の [`Format`](xref:Xamarin.Forms.TimePicker.Format) は、短い時刻のパターンである "t" に初期化された .NET 書式指定文字列であり、米国では "1:45 PM" のような時間表示になります。

[**SetTimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) プログラムに、`TimePicker` を使用してタイマーの時間を指定する方法が示されています。 このプログラムは、フォアグラウンドに保持している場合にのみ機能します。

また、**SetTimer** では、`Page` の [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) メソッドを使用して警告ボックスを表示する方法も示されています。

## <a name="related-links"></a>関連リンク

- [第 15 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [第 15 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [エントリ](~/xamarin-forms/user-interface/text/entry.md)
- [[エディター]](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
