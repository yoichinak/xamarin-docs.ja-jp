---
title: 第 15 章の概要です。 対話型のインターフェイス
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 第 15 章の概要です。 対話型のインターフェイス'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: aac49c9e74dd22642396ea8daf5ee3abd85de7bf
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241898"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>第 15 章の概要です。 対話型のインターフェイス

この章では、8 について説明`View`派生型、ユーザーとの対話を許可します。

## <a name="view-overview"></a>概要の表示

Xamarin.Forms にはから派生した 20 のインスタンス化可能なクラスが含まれています`View`ではなく`Layout`です。 これらの 6 つは、前の章で説明しています。

- `Label`: [**第 2 章します。アプリの構造**](chapter02.md)
- `BoxView`: [**第 3 章します。スタックをスクロール**](chapter03.md)
- `Button`: [**第 6 章します。ボタンのクリック**](chapter06.md)
- `Image`: [**第 13 章します。ビットマップ**](chapter13.md)
- `ActivityIndicator`: [**第 13 章します。ビットマップ**](chapter13.md)
- `ProgressBar`: [**章 14 です。AbsoluteLayout**](chapter14.md)

この章で 8 つのビューは、.NET の基本データ型と対話するユーザーを効果的に許可します。

|データの種類|Views|
|--- |--- |
|`Double`|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/), [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|
|`Boolean`|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|
|`String`|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/), [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|
|`DateTime`|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/), [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|

これらのビューの基になるデータの種類の視覚的な対話型表現と考えることができます。 この概念は次の章より調査[**章 16。データ バインディング**](chapter16.md)です。

次の章では、残りの 6 つのビューが含まれます。

- `WebView`: [**章 16。データ バインディング**](chapter16.md)
- `Picker`: [ **19 章します。コレクション ビュー**](chapter19.md)
- `ListView`: [ **19 章します。コレクション ビュー**](chapter19.md)
- `TableView`: [ **19 章します。コレクション ビュー**](chapter19.md)
- `Map`: [**章 28 です。場所とマップ**](chapter28.md)
- `OpenGLView`: このマニュアル (と Windows プラットフォームはサポートされていません) は、「not

## <a name="slider-and-stepper"></a>スライダーとステッパ

両方[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)と[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)ユーザーが、範囲から数値の値を選択できます。 `Slider`中に連続する範囲には、`Stepper`不連続の値が含まれます。

### <a name="slider-basics"></a>スライダーの基礎

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)は水平方向のバーの左側の最小値から、右側の最大値の範囲を表すです。 これには、次の 3 つのパブリック プロパティを定義します。

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) 型の`double`、既定値は 0
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) 型の`double`、既定値は 0
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) 型の`double`、既定値は 1

これらのプロパティをバインドできるプロパティは、対応することを確認します。

- すべての 3 つのプロパティの[ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/)バインド可能なプロパティにより指定されたメソッド`Value`間`Minimum`と`Maximum`です。
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/)メソッドを`MinimumProperty`を返します`false`場合`Minimum`より大きいか等しいを値に設定される`Maximum`、および同様の`MaximumProperty`します。 返す`false`から、`validateValue`メソッド原因、`ArgumentException`が発生します。

`Slider` 起動、 [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/)イベントと、 [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/)引数ときに、`Value`プロパティが変更された、ユーザーが操作するときまたはプログラムで、`Slider`です。

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo)サンプルでの簡単な使用、`Slider`です。

### <a name="common-pitfalls"></a>よくある落とし穴

コードと、XAML の両方、`Minimum`と`Maximum`プロパティは指定した順序で設定します。 これらのプロパティを初期化することを確認するように`Maximum`よりも大きいは常に`Minimum`です。 それ以外の場合は、例外が発生します。

初期化中、`Slider`プロパティが発生することができます、`Value`プロパティを変更して、`ValueChanged`イベントが発生します。 確認する必要があります、`Slider`イベント ハンドラーがページの初期化中にまだ作成されていないビューへのアクセスします。

`ValueChanged`中にイベントが発生しない`Slider`初期化しない限り、`Value`プロパティが変更されました。 呼び出すことができます、`ValueChanged`コードから直接ハンドラー。

### <a name="slider-color-selection"></a>スライダーの色の選択

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders)プログラムでは、3 種類を含む`Slider`対話形式で、RGB 値を指定して色を選択するように要求する要素。

[![R G B スライダーのトリプル スクリーン ショット](images/ch15fg03-small.png "RGB スライダー")](images/ch15fg03-large.png#lightbox "RGB スライダー")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade)サンプルを使用して 2 つ`Slider`2 つを移動する要素`Label`要素を`AbsoluteLayout`フェードを他のいずれかとします。

### <a name="the-stepper-difference"></a>ステッパの違い

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)同じプロパティおよびイベントとして定義`Slider`ですが、`Maximum`プロパティは 100 に初期化と`Stepper`4 番目のプロパティを定義します。

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) 型の`double`1 に初期化されて、

視覚的に、`Stepper`というラベルの付いた 2 つのボタンから成る**&ndash;** と **+** です。 キーを押して**&ndash;** 減少`Value`によって`Increment`を最小限に抑えるの`Minimum`します。 キーを押して**+** 増加`Value`によって`Increment`最大`Maximum`です。

示されるこれは、 [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo)サンプルです。

## <a name="switch-and-checkbox"></a>スイッチとチェック ボックス

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)ブール値を指定することができます。

### <a name="switch-basics"></a>スイッチの基礎

視覚的に、`Switch`オンとオフにすることができます、切り替えで構成されます。 クラスには、1 つのプロパティを定義します。

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) 型の `bool`

`Switch` 1 つのイベントを定義します。

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) と共に、 [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/)オブジェクト、ときに発生した、`IsToggled`プロパティが変更されました。

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo)プログラムを示しています、`Switch`です。

### <a name="a-traditional-checkbox"></a>従来のチェック ボックス

一部の開発者を使用してより従来`CheckBox`を`Switch`です。 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリに含まれる、`CheckBox`から派生したクラス`ContentView`です。 `CheckBox` によって実装される、 [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml)と[CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs)ファイル。 `CheckBox` 次の 3 つのプロパティを定義します (`Text`、 `FontSize`、および`IsChecked`) および`CheckedChanged`イベント。

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo)サンプルではこの`CheckBox`です。

## <a name="typing-text"></a>テキストを入力します。

Xamarin.Forms では、ユーザー入力し、編集を許可する 3 つのビューを定義します。

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) テキストの単一行
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) 複数行のテキストの
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 検索用のテキストの 1 つの行。

`Entry` および`Editor`から派生[ `InputView`](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/)から派生した`View`です。 `SearchBar` 直接派生`View`です。

### <a name="keyboard-and-focus"></a>キーボード フォーカスと

携帯電話とタブレット物理キーボード、せずに、 `Entry`、 `Editor`、および`SearchBar`要素はすべてがポップアップ表示して、仮想キーボードを発生します。 画面上のこのキーボードの存在は、入力フォーカスに関連付けられます。 ビューには、両方が必要、 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/)と[ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/)プロパティに設定`true`入力フォーカスを取得します。

2 つのメソッド、1 つの読み取り専用プロパティ、および 2 つのイベントが入力フォーカスを持つ関連します。 これらのすべてで定義されて`VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/)メソッドは、要素への入力フォーカスを設定しようとし、返します`true`正常終了した場合
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/)メソッドの入力フォーカスを要素から削除します。
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/)読み取り専用プロパティは、要素に入力フォーカスがかどうかを示します
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/)要素が入力フォーカスを取得するときにイベントを示します
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/)イベントは、要素が入力フォーカスを失ったことを示します

### <a name="choosing-the-keyboard"></a>キーボードを選択します。

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/)元となるクラス`Entry`と`Editor`派生 1 つのプロパティを定義します。

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) 型の [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

これは、表示されているキーボードの種類を示します。 Uri または数値は、一部のキーボードが最適化されています。

`Keyboard`クラスは、静的でキーボードを定義できます。 [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/)型の引数を持つメソッド[ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/)、次のビット フラグの列挙体。

- `None` 0 に設定します。
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) 1 に設定します。
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) 2 に設定します。
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) 4 に設定します。
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) \xFFFFFFFF に設定します。

複数行を使用するときに[ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)テキストの段落以上が予想される場合を呼び出す`Keyboard.Create`キーボードを選択するための適切なアプローチです。 単一行の[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)、静的な読み取り専用の次のプロパティ`Keyboard`は便利です。

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) 小数点の有無の正の数。

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/)に示すように、XAML でこれらのプロパティを指定できます、 [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards)プログラムです。

### <a name="entry-properties-and-events"></a>エントリのプロパティとイベント

単一行[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)次のプロパティを定義します。

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) 型の`string`に表示されるテキスト、 `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) 型の `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) 型の `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) 型の `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) 型の `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) 型の`bool`、それが原因でマスクする文字
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) 型の`string`、灰色のテキストに表示されるため、`Entry`入力内容は、前に
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) 型の `Color`

`Entry`も 2 つのイベントを定義します。

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/)オブジェクト、発生するたびに、`Text`プロパティの変更
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/)、ユーザーが終了し、キーボードが閉じられたときに発生します。 ユーザーは、プラットフォーム固有の方法で入力候補を示します。

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations)サンプルは、これら 2 つのイベントを示します。

### <a name="the-editor-difference"></a>エディターの違い

複数行[ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)同じ`Text`と`Font`プロパティとして`Entry`が、その他のプロパティではありません。 `Editor` また、同じ 2 つのプロパティとして、定義`Entry`です。

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes)自由形式のノートを取るプログラムの保存および復元の内容を`Editor`です。

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)から派生していない`InputView`があるないため、`Keyboard`プロパティです。 それがすべて、 `Text`、 `Font`、および`Placeholder`プロパティを`Entry`を定義します。 さらに、 `SearchBar` 3 つのプロパティを定義します。

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) 型の `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) 型の[ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/)データ バインドおよび MVVM と一緒に使用します。
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) 型の`Object`での使用 `SearchCommand`

プラットフォーム固有では、テキスト ボタンの消去をキャンセルします。 `SearchBar`プラットフォームに固有の検索 ボタンがあります。 いずれかの 2 つのイベントを発生させますこれらのボタンのいずれかのキーを押している`SearchBar`を定義します。

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) と共に、 [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/)オブジェクト
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo)サンプルでは、`SearchBar`です。

## <a name="date-and-time-selection"></a>日付と時刻の選択

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)と[ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)ビューは、日付または時刻を指定するユーザーに許可するプラットフォーム固有の制御を実装します。

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) 4 つのプロパティを定義します。

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) 型の`DateTime`1900 年 1 月 1 日に初期化されて、
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) 型の`DateTime`2100 年 12 月 31 日に初期化されて、
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) 型の`DateTime`に初期化されて、 `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) 型の`string`、「7/20/1969」米国でのような日付の表示の結果として得られる .NET"d"、短い日付のパターンに初期化文字列の書式設定します。

設定することができます、`DateTime`プロパティをプロパティ要素として表現して、インバリアント カルチャの短い日付を使用して XAML でのプロパティ (「7/20/1969」) を書式設定します。   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates)サンプルは、ユーザーが選択した 2 つの日付までの日数の数を計算します。

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (か、TimeSpanPicker)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) 2 つのプロパティとイベントを定義します。

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) 型は`TimeSpan`なく`DateTime`、午前 0 時から経過した時間を示す
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) 型の`string`.NET の米国の"t"、"1時 45分 PM"のように時刻の表示の結果として得られる、短い形式の時刻のパターンに初期化文字列の書式設定します。

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer)プログラムを使用する方法を示しています、`TimePicker`タイマーの時間を指定します。 プログラムは、フォア グラウンドで保持する場合にのみ機能します。

**SetTimer**も使用方法を示します、 [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/)メソッドの`Page`アラート ボックスを表示します。



## <a name="related-links"></a>関連リンク

- [第 15 章「フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [第 15 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [エントリ](~/xamarin-forms/user-interface/text/entry.md)
- [[エディター]](~/xamarin-forms/user-interface/text/editor.md)
