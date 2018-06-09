---
title: 19 章の概要です。 コレクション ビュー
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 19 章の概要です。 コレクション ビュー'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 62320b602a8999ff32ee01d60d10a2ea01fced9d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241843"
---
# <a name="summary-of-chapter-19-collection-views"></a>19 章の概要です。 コレクション ビュー

Xamarin.Forms では、コレクションを保持し、その要素を表示する 3 つのビューを定義します。

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ユーザーがいずれかを選択できる比較的短い文字列アイテムの一覧を示します
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 通常、同じ種類の項目の長い一覧は、多くの場合ともユーザーが 1 つを選択できるようにする書式設定します。
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) コレクションは、*セル*(通常、さまざまな種類と外観) データを表示したり、ユーザー入力を管理するには

MVVM アプリケーションを使用するが一般的、`ListView`オブジェクトの選択可能なコレクションを表示します。

## <a name="program-options-with-picker"></a>ピッカーを使用してプログラムのオプション

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)の比較的短い形式の一覧の中からオプションを選択できるようにする必要がある場合、適切な選択は、`string`項目。

### <a name="the-picker-and-event-handling"></a>ピッカーとイベントの処理

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo)サンプルでは XAML を使用して設定する方法、 `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/)プロパティを追加および`string`項目[ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション。 ユーザーが選択すると、 `Picker`、内の項目が表示されます、`Items`プラットフォームに依存する形でコレクション。

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベントは、ユーザーが項目を選択した場合を示します。 0 から始まる[ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティは、選択した項目を示します。 項目が選択されていない場合`SelectedIndex`equals &ndash;1 です。

使用することも`SelectedIndex`後に設定する必要がありますが、選択したアイテムを初期化するために、`Items`コレクションを格納します。 XAML では、つまりを設定するプロパティ要素を使用すること可能性あります`SelectedIndex`です。

### <a name="data-binding-the-picker"></a>データ ピッカーのバインディング

`SelectedIndex`プロパティにバインド可能なプロパティが支えられてが`Items`によるデータ バインディングを使用してではない、`Picker`困難です。 1 つのソリューションは、使用する、`Picker`と組み合わせて、 [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs)などの[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。 [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding)このしくみを示しています。

## <a name="rendering-data-with-listview"></a>ListView でデータの表示

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)から派生する唯一のクラスは、 [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)継承、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/)と[ `ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/)プロパティです。

`ItemsSource` 型は`IEnumerable`が`null`既定と明示的に初期化または (頻繁)、データ バインディングによってコレクションに設定する必要があります。 このコレクション内のアイテムは、任意の型指定できます。

`ListView` 定義、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)内の項目のいずれかにいずれかであるプロパティを設定、`ItemsSource`コレクションまたは`null`項目が選択されていない場合。 `ListView` 起動、 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)新しい項目が選択されているときにイベント。

### <a name="collections-and-selections"></a>コレクションと選択

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList)サンプルがいっぱいになった、 `ListView` 17`Color`の値が、`List<Color>`コレクション。 項目が選択可能は既定では表示が不適切になるの`ToString`表現します。 この章でいくつかの例では、その表示を修正し、必要に応じては、魅力的する方法を示します。

### <a name="the-row-separator"></a>行区切り記号

IOS および Android の表示では、細い線は、行を区切ります。 これを制御することができます、 [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/)と[ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/)プロパティです。 `SeparatorVisibility` プロパティの型は[ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/)、2 つのメンバーを列挙します。

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/)、既定の設定
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>データ バインディングの選択項目

`SelectedItem`ソースまたはデータ バインディングのターゲットにするためにバインド可能なプロパティでプロパティをサポートします。 既定の`BindingMode`は`OneWayToSource`が MVVM シナリオで特にの双方向データ バインディングのターゲットは一般にします。 [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray)サンプルは、この型のバインディングを示します。

### <a name="the-observablecollection-difference"></a>ObservableCollection の違い

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger)サンプル セット、`ItemsSource`のプロパティ、`ListView`を`List<DateTime>`コレクションし、新しい段階的に追加`DateTime`オブジェクトをコレクションすべて 2 番目のタイマーを使用します。

ただし、`ListView`しないは自動的に更新ため、`List<T>`コレクションに項目を追加またはコレクションから削除するときを示すために、通知のメカニズムがないです。

このようなシナリオで使用する程度に優れたクラスは[ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/)で定義されている、`System.Collections.ObjectModel`名前空間。 このクラスは、実装、 [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/)インターフェイスとその結果発生、 [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/)項目が追加または交換または内で移動されるときに、コレクションから、またはを削除するときにイベントコレクションです。 ときに、`ListView`ことを検出する内部的に実装するクラス`INotifyCollectionChanged`に設定されているその`ItemsSource`プロパティにハンドラーをアタッチ、`CollectionChanged`イベントし、コレクションが変更されたときの表示を更新します。

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger)サンプルでの使用`ObservableCollection`です。

### <a name="templates-and-cells"></a>テンプレートとセル

既定では、`ListView`各項目を使用してコレクションに項目を表示します。`ToString`メソッドです。 適切な方法では、項目を表示するテンプレートを定義します。

この機能を実験に使用することができます、 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。 このクラスを定義、静的な`All`型のプロパティ`IList<NamedColor>`141 を格納している`NamedColor`のパブリック フィールドに対応するオブジェクト、`Color`構造体。

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList)サンプル セット、`ItemsSource`の`ListView`をこの`NamedColor.All`プロパティが、完全に修飾されたクラスの名前にのみ、`NamedColor`オブジェクトは、表示されます。

`ListView` これらの項目を表示するテンプレートが必要です。 コードでは、設定することができます、 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/)プロパティによって定義された`ItemsView<TVisual>`を[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)オブジェクトを使用して、 [ `DataTemplate`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/)です派生物を参照して、 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)クラスです。 `Cell` 5 つの派生クラスがあります。

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &mdash; 2 つ`Label`ビュー (概念的に)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &mdash; 追加、`Image`を表示 `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &mdash; 含まれています、`Entry`で表示します。 `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &mdash; 含まれています、`Switch`で、 `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &mdash; `View` (子を伴う可能性があります)

呼び出す[ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)と[ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)上、`DataTemplate`値とを関連付けるためのオブジェクト、`Cell`プロパティ、またはへのデータ バインディングの設定、 `Cell`プロパティ内の項目のプロパティを参照する、`ItemsSource`コレクション。 これに示されている、 [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode)サンプルです。

各項目が表示されるよう、`ListView`小規模のビジュアル ツリーが、テンプレートから構築された、このビジュアル ツリー内のアイテムと、要素のプロパティ間のデータ バインドを確立します。 ハンドラーをインストールすると、このプロセスのアイデアを取得できます、 [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/)と[ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/)のイベント、 `ListView`、または、代替手段を使用して[ `DataTemplate`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/)アイテムのビジュアル ツリーを作成する必要があるたびに呼び出される関数を使用します。

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) XAML でのコードと同じプログラムを示しています。 A`DataTemplate`にタグが設定されている、`ItemTemplate`のプロパティ、 `ListView`、し、`TextCell`に設定されている、`DataTemplate`です。 コレクション内の項目のプロパティへのバインドが直接に設定されて、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/)と[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/)のプロパティ、`TextCell`です。

### <a name="custom-cells"></a>カスタムのセル

XAML で設定することは、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)を`DataTemplate`としてカスタム ビジュアル ツリーを定義し、 [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/)プロパティ`ViewCell`です。 (`View`のコンテンツ プロパティは、`ViewCell`ため、`ViewCell.View`タグは必要ありません)。[ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList)サンプルは、この手法を示します。

[![カスタム名前付きの色の一覧のスクリーン ショットをトリプル](images/ch19fg11-small.png "カスタム色リストの名前付き")](images/ch19fg11-large.png#lightbox "カスタム名前付きの色の一覧")

すべてのプラットフォームの権利をサイズ変更の取得は複雑になることができます。 [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/)プロパティ役に立つ場合によっては必要がありますに頼る、 [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/)効率が悪いプロパティが強制的に実行、`ListView`行のサイズを変更します。 IOS と Android の場合は、これら 2 つのプロパティのいずれかを使用する必要があります、適切な行のサイズ変更を取得します。

### <a name="grouping-the-listview-items"></a>ListView の項目をグループ化

`ListView` 項目のグループ化し、それらのグループ間の移動をサポートしています。 `ItemsSource`コレクションのコレクションにプロパティを設定する必要があります: オブジェクトを`ItemsSource`必要がありますに設定されている実装`IEnumerable`、コレクション内の各項目を実装する必要がありますもと`IEnumerable`です。 各グループが 2 つのプロパティを含める必要があります: グループと、3 文字の省略形のテキストの説明。

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリの 7 つのグループを作成する`NamedColor`オブジェクト。 [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList)サンプルでは、これらのグループを使用して、 [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/)のプロパティ`ListView`'éý' `true`、および、 [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/)と[ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/)プロパティは各グループ内のプロパティにバインドします。

### <a name="custom-group-headers"></a>カスタムのグループ ヘッダー

カスタム ヘッダーを作成することは、`ListView`グループに置き換えることで、`GroupDisplayBinding`を持つプロパティ、 [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)ヘッダーのテンプレートを定義します。

### <a name="listview-and-interactivity"></a>ListView コントロールと対話機能

アプリケーションがユーザーとのやり取りを取得する、通常、`ListView`にハンドラーをアタッチすることによって、`ItemSelected`または[ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/)イベント、または上のデータ バインドを設定して、`SelectedItem`プロパティです。 一部のセルの種類 (`EntryCell`と`SwitchCell`)、ユーザーとの対話を許可してもカスタムのセルがその自体を作ることがユーザーと対話します。 [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView)の 100 のインスタンスを作成[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs)ユーザーの 3 つを使用してそれぞれの色を変更することができ`Slider`要素。 プログラムでは、使用も、 [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs)で、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)です。

## <a name="listview-and-mvvm"></a>ListView コントロールと MVVM

`ListView` MVVM シナリオでは、大規模な役割を果たします。 ときに、 `IEnumerable` 、ViewModel にコレクションが存在し、多くの場合にバインドされている、`ListView`です。 また、多くの場合、コレクション内の項目を実装`INotifyPropertyChanged`テンプレートのプロパティにバインドします。

### <a name="a-collection-of-viewmodels"></a>ViewModels のコレクション

これには、探索、 [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリに基づいていくつかのクラスを作成する、 [XML データ ファイル](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml)と、この架空の学校の架空の生徒のイメージです。

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs)クラスから派生[ `ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)です。 [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs)クラスは、コレクションの`Student`オブジェクトおよびからも派生`ViewModelBase`です。 [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML ファイルをダウンロードし、すべてのオブジェクトをアセンブルします。

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList)プログラムの使用、`ImageCell`受講者とでは、そのイメージを表示する、 `ListView`:

[![学生のリストのスクリーン ショットをトリプル](images/ch19fg18-small.png "学生リスト")](images/ch19fg18-large.png#lightbox "学生一覧")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader)サンプルを追加、 [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/)プロパティ、のみ現れる Android でします。

### <a name="selection-and-the-binding-context"></a>選択およびバインディング コンテキスト

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail)プログラムのバインド、`BindingContext`の`StackLayout`を`SelectedItem`のプロパティ、`ListView`です。 これにより、選択した受講者に関する詳細情報を表示するプログラムです。

### <a name="context-menus"></a>コンテキスト メニュー

セルには、プラットフォームに固有の方法で実装されているコンテキスト メニューを定義できます。 このメニューを作成する追加[ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)オブジェクトを[ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/)のプロパティ、`Cell`です。

`MenuItem` 5 つのプロパティを定義します。

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) 型の `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) 型の `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) 型の `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) 型の `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) 型の `object`

`Command`と`CommandParameter`の各項目に対して ViewModel に目的のメニュー コマンドを実行するメソッドが含まれているプロパティを意味します。 MVVM 以外の場合、`MenuItem`も定義、 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/)イベント。

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu)この手法を実証します。 `Command`の各プロパティ`MenuItem`型のプロパティにバインドする`ICommand`で、`Student`クラスです。 設定、`IsDestructive`プロパティを`true`の`MenuItem`削除をするか、選択したオブジェクトを削除します。

### <a name="varying-the-visuals"></a>ビジュアルの変更

内の項目のビジュアルのわずかな変化をおくことがあります、`ListView`プロパティに基づいています。 たとえばと学生の成績の平均を下回る 2.0 では、 [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents)サンプルでは、赤でそのスチューデントの名前が表示されます。
これは、バインディングの値コンバーターを使用して実現[ `ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。

### <a name="refreshing-the-content"></a>コンテンツを更新します。

`ListView`そのデータを更新する場合、プルダウンで表示されるジェスチャをサポートしています。 プログラムを設定する必要があります、 [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/)プロパティを`true`これを有効にします。 `ListView`設定によって、プルダウンで表示されるジェスチャに応答その[ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/)プロパティを`true`、および発生させることによって、 [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/)イベントおよび (MVVM シナリオ) を呼び出す`Execute`のメソッド、 [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/)プロパティです。

コードの処理、`Refresh`イベントまたは`RefreshCommand`によって表示されるデータを更新する可能性があります、`ListView`設定と`IsRefreshing`に`false`です。

[**の rss フィード**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed)サンプルの使用例、 [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs)を実装する`RefreshCommand`と`IsRefreshing`データ バインドのプロパティです。

## <a name="the-tableview-and-its-intents"></a>テーブルとその目的

中に、`ListView`一般に、同じ型での複数のインスタンスが表示されます、 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)は一般にさまざまな種類の複数のプロパティのユーザー インターフェイスを提供することに重点を置きます。 各項目は、独自に関連付けられた[ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)プロパティを表示するかをユーザー インターフェイスを提供するために派生します。

### <a name="properties-and-hierarchies"></a>プロパティと階層

`TableView` 4 つだけのプロパティを定義します。

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) 型の[ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/)、列挙型
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) 型の[ `TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)の content プロパティ `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) 型の `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) 型の `bool`

`TableIntent`列挙体を使用する方法を示します、 `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

これらのメンバーでは、いくつかの使用もお勧め、`TableView`です。

テーブルを定義するのには、その他のいくつかのクラスが関係します。

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) 抽象クラスから派生した`BindableObject`を定義し、 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/)プロパティ

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) 抽象クラスから派生した`TableSectionBase`を実装して`IList<T>`と `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) を派生します。 `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) を派生します。 `TableSectionBase<TableSection>`

つまり、`TableView`が、`Root`を設定できるプロパティ、`TableRoot`コレクションであるオブジェクトの`TableSection`オブジェクトのコレクションは、それぞれの`Cell`オブジェクト。 テーブルは複数のセクションを持ち、各セクションは、複数のセルをします。 テーブル自体は、タイトルを持つことができ、各セクションでは、タイトルを持つことができます。 `TableView`利用`Cell`から派生したとは言えませんの使用`DataTemplate`です。

### <a name="a-prosaic-form"></a>Prosaic フォーム

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm)サンプルでは定義、 [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)をインスタンスが、モデルの表示、`BindingContext`の`TableView`です。 各`Cell`で派生その`TableSection`のプロパティへのバインドを持つことができますし、`PersonalInformation`クラスです。

### <a name="custom-cells"></a>カスタムのセル

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells)サンプルを拡張**EntryForm**です。 [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)クラスには、2 つのプロパティの適用性を制御するブール型プロパティが含まれています。 これら 2 つのプロパティの使用カスタム`PickerCell`に基づいて、 [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml)と[PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。

`IsEnabled` 、2 つのプロパティ`PickerCell`要素がブール型プロパティにバインドされて`ProgrammerInformation`、この手法はしないようには、次のサンプルをメッセージが表示されます。

### <a name="conditional-sections"></a>条件付きセクション

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection)サンプルでは 2 つの項目を別個のブール型の項目の選択範囲で条件付きである`TableSection`です。 分離コード ファイルからのこのセクションの削除、`TableView`または戻るに基づいてブール型プロパティを追加します。

### <a name="a-tableview-menu"></a>テーブルのメニュー

別の使用、`TableView`メニューします。 [ **Menucommand** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands)サンプルでは少し移動するメニュー`BoxView`画面中です。



## <a name="related-links"></a>関連リンク

- [19 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
