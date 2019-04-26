---
title: 第 15 章の概要です。 対話型インターフェイス
description: Xamarin.Forms によるモバイル アプリの作成。第 15 章の概要です。 対話型インターフェイス
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 974ef2a141ec7175c2306d4af63f534a5c77ecb2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61335297"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>第 15 章の概要です。 対話型インターフェイス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

この章では、8 について説明します`View`ユーザーとの対話を許可する派生クラス。

## <a name="view-overview"></a>概要の表示

Xamarin.Forms には 20 のインスタンス化可能なクラスから派生した`View`なく`Layout`します。 これらの 6 つ前の章で説明します。

- `Label`:[**第 2 章です。アプリの詳細**](chapter02.md)
- `BoxView`:[**第 3 章です。スタックをスクロール**](chapter03.md)
- `Button`:[**第 6 章です。ボタンのクリック**](chapter06.md)
- `Image`:[**第 13 章です。ビットマップ**](chapter13.md)
- `ActivityIndicator`:[**第 13 章です。ビットマップ**](chapter13.md)
- `ProgressBar`:[**第 14 章です。AbsoluteLayout**](chapter14.md)

この章では 8 つのビューは、.NET の基本データ型と対話するユーザーを効果的に許可します。

|データの種類|Views|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

これらのビューは、基になるデータ型のインタラクティブな視覚的な表現として考えることができます。 この概念は、[次へ] の章より探索[ **16 章です。データ バインディング**](chapter16.md)します。

次の章では、残りの 6 つのビューが含まれます。

- `WebView`:[**第 16 章です。データ バインディング**](chapter16.md)
- `Picker`:[**第 19 章です。コレクション ビュー**](chapter19.md)
- `ListView`:[**第 19 章です。コレクション ビュー**](chapter19.md)
- `TableView`:[**第 19 章です。コレクション ビュー**](chapter19.md)
- `Map`:[**第 28 章です。場所とマップ**](chapter28.md)
- `OpenGLView`:この本 (と Windows プラットフォームはサポートされていません) は、「not

## <a name="slider-and-stepper"></a>スライダーとステッパ

両方[ `Slider` ](xref:Xamarin.Forms.Slider)と[ `Stepper` ](xref:Xamarin.Forms.Stepper)ユーザーが範囲から数値を選択できます。 `Slider`中に継続的な範囲は、`Stepper`不連続の値が含まれます。

### <a name="slider-basics"></a>スライダーの基礎

[ `Slider` ](xref:Xamarin.Forms.Slider)は水平バーの左側に最小値から、右側の最大値の範囲を表します。 これには、次の 3 つのパブリック プロパティを定義します。

- [`Value`](xref:Xamarin.Forms.Slider.Value) 型の`double`、既定値は 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 型の`double`、既定値は 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 型の`double`、既定値は 1

これらのプロパティをバインドできるプロパティは、一貫性のあるいることを確認します。

- すべての 3 つのプロパティの[ `coerceValue` ](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate)バインド可能なプロパティにより指定されたメソッド`Value`間`Minimum`と`Maximum`します。
- [ `validateValue` ](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate)メソッド`MinimumProperty`返します`false`場合`Minimum`より大きいまたは等しい値に設定されて`Maximum`との類似`MaximumProperty`します。 返す`false`から、`validateValue`メソッド、`ArgumentException`が発生します。

`Slider` 起動、 [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged)イベントを[ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs)引数ときに、`Value`プロパティの変更をユーザーが操作するプログラムまたはときに、`Slider`します。

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo)サンプルでの簡単な使用、`Slider`します。

### <a name="common-pitfalls"></a>よくある落とし穴

コードと、XAML の両方、`Minimum`と`Maximum`プロパティは、指定した順序で設定されます。 これらのプロパティを初期化することを確認するように`Maximum`よりも大きいは常に`Minimum`します。 それ以外の場合は、例外が発生します。

初期化、`Slider`プロパティが発生することができます、`Value`プロパティを変更して、`ValueChanged`イベントが。 いることを確認する必要があります、`Slider`イベント ハンドラーはページの初期化中にまだ作成されていないビューへのアクセスします。

`ValueChanged`中にイベントが発生しない`Slider`初期化しない限り、`Value`プロパティの変更。 呼び出すことができます、`ValueChanged`コードから直接ハンドラー。

### <a name="slider-color-selection"></a>スライダーの色の選択

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders)プログラムでは、3 つ含まれている`Slider`対話形式の RGB 値を指定することで色を選択するための要素。

[![R G B スライダーの 3 倍になるスクリーン ショット](images/ch15fg03-small.png "RGB スライダー")](images/ch15fg03-large.png#lightbox "RGB スライダー")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade)サンプルを使用して 2 つ`Slider`2 つを移動する要素`Label`要素間で、`AbsoluteLayout`フェードインを他のいずれかとします。

### <a name="the-stepper-difference"></a>ステッパの違い

[ `Stepper` ](xref:Xamarin.Forms.Stepper)同じプロパティとイベントとして定義`Slider`が、`Maximum`プロパティは 100 に初期化と`Stepper`4 番目のプロパティを定義します。

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 型の`double`1 に初期化されました。

視覚的に、`Stepper`というラベルの付いた 2 つのボタンから成る **&ndash;** と **+** します。 キーを押して **&ndash;** 減少`Value`によって`Increment`を最小限の`Minimum`します。 キーを押して **+** 増加`Value`によって`Increment`最大`Maximum`します。

これを示します、 [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo)サンプル。

## <a name="switch-and-checkbox"></a>スイッチとチェック ボックス

[ `Switch` ](xref:Xamarin.Forms.Switch)をブール値を指定できます。

### <a name="switch-basics"></a>スイッチの基礎

視覚的に、`Switch`のオンとオフにすることを切り替えで構成されます。 クラスには、1 つのプロパティを定義します。

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 型の `bool`

`Switch` 1 つのイベントを定義します。

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) と共に、 [ `ToggledEventArgs` ](xref:Xamarin.Forms.ToggledEventArgs)ときに発生したオブジェクト、`IsToggled`プロパティの変更。

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo)プログラムを示して、`Switch`します。

### <a name="a-traditional-checkbox"></a>従来のチェック ボックス

一部の開発者がより従来方`CheckBox`を`Switch`します。 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリが含まれています、`CheckBox`クラスから派生した`ContentView`します。 `CheckBox` によって実装される、 [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml)と[CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs)ファイル。 `CheckBox` 3 つのプロパティを定義します (`Text`、 `FontSize`、および`IsChecked`) と`CheckedChanged`イベント。

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo)サンプルではこの`CheckBox`します。

## <a name="typing-text"></a>テキストを入力します。

Xamarin.Forms では、ユーザーを入力し、テキストを編集できる 3 つのビューを定義します。

- [`Entry`](xref:Xamarin.Forms.Entry) 1 つの行のテキスト
- [`Editor`](xref:Xamarin.Forms.Editor) 複数行のテキストの
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 検索用のテキストの 1 つの行。

`Entry` `Editor`から派生[ `InputView`](xref:Xamarin.Forms.InputView)から派生した`View`します。 `SearchBar` 直接派生`View`します。

### <a name="keyboard-and-focus"></a>キーボードとフォーカス

携帯電話とタブレット物理キーボードは、なし、 `Entry`、 `Editor`、および`SearchBar`をポップアップ表示して、仮想キーボードが発生するすべての要素。 画面上のこのキーボードの有無に関連する入力フォーカスをします。 ビューには、両方が必要、 [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible)と[ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)プロパティに設定`true`入力フォーカスを取得します。

入力フォーカスを持つ 2 つのメソッド、1 つの読み取り専用プロパティ、および 2 つのイベントがあります。 これらすべてで定義されて`VisualElement`:

- [ `Focus` ](xref:Xamarin.Forms.VisualElement.Focus)メソッドは、要素に入力フォーカスを設定しようとし、返します`true`成功した場合
- [ `Unfocus` ](xref:Xamarin.Forms.VisualElement.Unfocus)メソッドは、要素から入力フォーカスを削除します。
- [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused)読み取り専用プロパティでは、要素に入力フォーカスがかどうかを示します
- [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused)イベントは、要素が入力フォーカスを取得する場合を示します
- [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused)イベントは、要素が入力フォーカスを失ったことを示します

### <a name="choosing-the-keyboard"></a>キーボードの選択

[ `InputView` ](xref:Xamarin.Forms.InputView)元となるクラス`Entry`と`Editor`派生 1 つのプロパティを定義します。

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 型の [`Keyboard`](xref:Xamarin.Forms.Keyboard)

これは、表示されるキーボードの種類を示します。 Uri または数値は、一部のキーボードが最適化されています。

`Keyboard`静的でキーボードを定義するクラスを使用できます[ `Keyboard.Create` ](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags))型の引数を持つメソッド[ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags)、ビット フラグ列挙体。

- `None` 0 に設定します。
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) 1 に設定します。
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) 2 に設定します。
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) 4 に設定します。
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) \xFFFFFFFF に設定します。

複数行を使用する場合[ `Editor` ](xref:Xamarin.Forms.Editor)テキストの段落以上が必要ですが、ときに呼び出す`Keyboard.Create`キーボードを選択するための適切なアプローチです。 単一行の[ `Entry` ](xref:Xamarin.Forms.Entry)、静的な読み取り専用の次のプロパティ`Keyboard`は便利です。

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) 小数点 10 進数の有無の正の数。

[ `KeyboardTypeConverter` ](xref:Xamarin.Forms.KeyboardTypeConverter)に示すように、XAML でこれらのプロパティを指定することにより、 [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards)プログラム。

### <a name="entry-properties-and-events"></a>エントリのプロパティとイベント

単一行[ `Entry` ](xref:Xamarin.Forms.Entry)次のプロパティを定義します。

- [`Text`](xref:Xamarin.Forms.Entry.Text) 型の`string`に表示されるテキスト、 `Entry`
- [`TextColor`](xref:Xamarin.Forms.Entry.TextColor) 型の `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) 型の `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) 型の `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) 型の `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) 型の`bool`、それが原因でマスクされる文字
- [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 型の`string`に表示される灰色のテキストの`Entry`入力内容は、前に
- [`PlaceholderColor`](xref:Xamarin.Forms.Entry.PlaceholderColor) 型の `Color`

`Entry`も 2 つのイベントを定義します。

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs)たびに発生したオブジェクト、`Text`プロパティの変更
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)、、ユーザーが終了し、キーボードを閉じるときに発生します。 ユーザーは、プラットフォーム固有の方法で完了を示します。

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations)サンプルは、これら 2 つのイベントを示します。

### <a name="the-editor-difference"></a>エディターの違い

複数行[ `Editor` ](xref:Xamarin.Forms.Editor)同じ`Text`と`Font`プロパティとして`Entry`が、その他のプロパティではありません。 `Editor` 同じ 2 つのプロパティを定義も`Entry`します。

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes)の保存および復元の内容を自由にメモを取るプログラム、`Editor`します。

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar)から派生していない`InputView`があるないため、`Keyboard`プロパティ。 それがすべて、 `Text`、 `Font`、および`Placeholder`プロパティを`Entry`を定義します。 さらに、 `SearchBar` 3 つのプロパティを定義します。

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) 型の `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) 型の[ `ICommand` ](xref:System.Windows.Input.ICommand)データ バインドと MVVM での使用
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) 型の`Object`での使用 `SearchCommand`

プラットフォーム固有では、テキスト ボタンの消去をキャンセルします。 `SearchBar`プラットフォームに固有の検索 ボタンがあります。 いずれかの 2 つのイベントを発生させますそのボタンのいずれかのキーを押している`SearchBar`を定義します。

- [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) と共に、 [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs)オブジェクト
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo)サンプルでは、`SearchBar`します。

## <a name="date-and-time-selection"></a>日付と時刻の選択

[ `DatePicker` ](xref:Xamarin.Forms.DatePicker)と[ `TimePicker` ](xref:Xamarin.Forms.TimePicker)ビューは、日付または時刻を指定するユーザーを許可するプラットフォーム固有のコントロールを実装します。

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) 4 つのプロパティを定義します。

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 型の`DateTime`1900 年 1 月 1 日に初期化されました。
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 型の`DateTime`2100 年 12 月 31 日に初期化されました。
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) 型の`DateTime`に初期化されました。 `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) 型の`string`、「7/20/1969」米国内のような日付の表示で .NET 書式設定文字列"d"、短い日付のパターンに初期化します。

設定することができます、`DateTime`プロパティ要素としてプロパティを表現して、インバリアント カルチャの短い日付を使用して XAML でプロパティ (「7/20/1969」) を書式設定します。   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates)サンプルは、ユーザーが選択した 2 つの日付間の日数を計算します。

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (または、TimeSpanPicker ですか?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) 2 つのプロパティとイベントを定義します。

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) 種類は`TimeSpan`なく`DateTime`、午前 0 時から経過した時間を示す
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) 型の`string`.NET の米国の"t"、"1時 45分 PM"のように時刻の表示で、短い形式の時刻パターンに初期化文字列の書式設定します。

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer)プログラムが使用する方法を示します、`TimePicker`タイマーの時間を指定します。 プログラムは、フォア グラウンドで保持する場合にのみ機能します。

**SetTimer**も示しますを使用して、 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))メソッドの`Page`警告ボックスを表示します。



## <a name="related-links"></a>関連リンク

- [第 15 章「フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [第 15 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [エントリ](~/xamarin-forms/user-interface/text/entry.md)
- [[エディター]](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
