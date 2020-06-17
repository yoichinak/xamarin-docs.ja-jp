---
title:"第 19 章の概要: コレクション ビュー" description:"Xamarin.Forms でモバイル アプリを作成する: 第 19 章の概要: コレクション ビュー" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:0AEC3A5C-586E-4D0F-9895-67E99A053A79 author: davidbritch ms.author: dabritch ms.date:07/18/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-19-collection-views"></a>第 19 章の概要: コレクション ビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE] 
> このページの注記では、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

Xamarin.Forms では、コレクションを保持し、その要素を表示する 3 つのビューが定義されています。

- [`Picker`](xref:Xamarin.Forms.Picker) は、文字列項目を一覧表示する比較的短いリストで、ユーザーが文字列項目を選択するために使用できます
- [`ListView`](xref:Xamarin.Forms.ListView) は、多くの場合、通常は同じ型の項目を一覧表示する長いリストです。これもユーザーが項目を選択するために使用できます
- [`TableView`](xref:Xamarin.Forms.TableView) は、データを表示したり、ユーザー入力を管理したりするための "*セル*" (通常は、さまざまな種類や外観) のコレクションです

MVVM アプリケーションの場合、オブジェクトの選択可能なコレクションを表示するには、`ListView` を使用するのが一般的です。

## <a name="program-options-with-picker"></a>Picker を使用したプログラム オプション

[`Picker`](xref:Xamarin.Forms.Picker) は、`string` 項目の比較的短い一覧からユーザーがオプションを選択できるようにする必要がある場合に適しています。

### <a name="the-picker-and-event-handling"></a>Picker とイベント処理

[**PickerDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) サンプルは、XAML を使用して `Picker` [`Title`](xref:Xamarin.Forms.Picker.Title) を設定し、`string` 項目を [`Items`](xref:Xamarin.Forms.Picker.Items) コレクションに追加する方法を示しています。 ユーザーが `Picker`を選択すると、プラットフォームに依存する方法で項目が `Items` コレクションに表示されます。

[`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) イベントは、ユーザーが項目を選択したことを示します。 0 から始まる [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) プロパティは、選択された項目を示します。 選択された項目がない場合、`SelectedIndex` は &ndash;1 になります。

また、`SelectedIndex` を使用して、選択された項目を初期化することもできます。ただし、`Items` コレクションを記述した後に設定する必要があります。 XAML では、これは、プロパティ要素を使用して `SelectedIndex`を設定することを意味します。

### <a name="data-binding-the-picker"></a>Picker のデータ バインディング

`SelectedIndex` プロパティは、バインド可能プロパティによってサポートされますが、`Items` はサポートされないため、`Picker` でデータ バインディングを使用するのは困難です。 1 つのソリューションとして、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリで見られるように、`Picker` を [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) と組み合わせて使用する方法があります。 [**PickerBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) は、これがどのように動作するかを示しています。

> [!NOTE] 
> Xamarin.Forms `Picker` には、データ バインディングをサポートする `ItemsSource` プロパティと `SelectedItem` プロパティが含まれるようになりました。 [Picker](~/xamarin-forms/user-interface/picker/index.md)に関するページを参照してください。

## <a name="rendering-data-with-listview"></a>ListView によるデータのレンダリング

[`ListView`](xref:Xamarin.Forms.ListView) は、[`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1)から派生する唯一のクラスであり、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティと [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを継承します。

`ItemsSource` は、`IEnumerable` 型ですが、既定では `null` です。このため、明示的に初期化するか、データ バインディングを使用してコレクションに設定する (こちらの方が一般的) 必要があります。 このコレクション内の項目の型は、任意です。

`ListView` は、[`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) プロパティを定義し、`ItemsSource` コレクション内のいずれかの項目、または項目が選択されていない場合は `null` に設定します。 新しい項目が選択されると、`ListView` では、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントが発生します。

### <a name="collections-and-selections"></a>コレクションと選択項目

[**ListViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) サンプルでは、`ListView` に、`List<Color>` コレクション内の 17 個の `Color` 値が記述されています。 項目は選択できますが、既定では、魅力的ではない `ToString` 表現で表示されます。 この章では、いくつかの例で、その表示を修正し、目的どおりに魅力的な表示にする方法を示します。

### <a name="the-row-separator"></a>行区切り

iOS および Android の表示では、細い線によって行が区切られます。 [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) プロパティと [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor) プロパティを使用して、これを制御することができます。 `SeparatorVisibility` プロパティは、[`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility) 型で、次の 2 つのメンバーを持つ列挙体です。

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default): 既定の設定
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>選択された項目のデータ バインディング

`SelectedItem` プロパティは、バインド可能プロパティでサポートされるため、データ バインディングのソースまたはターゲットのいずれかになります。 既定の `BindingMode` は `OneWayToSource` ですが、一般的には (特に MVVM シナリオの場合)、双方向のバインディングのターゲットになります。 [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) サンプルは、この種類のバインディングの例を示しています。

### <a name="the-observablecollection-difference"></a>ObservableCollection との違い

[**ListViewLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) サンプルでは、`ListView` の `ItemsSource` プロパティを `List<DateTime>` コレクションに設定し、その後、タイマーを使用して 1 秒ごとに新しい `DateTime` オブジェクトをコレクションに段階的に追加します。

しかし、`List<T>` コレクションには、項目がコレクションに追加されたか、コレクションから削除されたことを知らせる通知メカニズムがないため、`ListView` は自身を自動的に更新しません。

このようなシナリオで使用するのにはるかに適しているクラスは [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1) で、これを `System.Collections.ObjectModel` 名前空間で定義します。 このクラスは、[`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged) インターフェイスを実装し、項目がコレクションに追加されるか、コレクションから削除されたとき、またはコレクション内で置き換えられるか、移動されたときに、[`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) イベントを発生させます。 `ListView` 内部で、`INotifyCollectionChanged` を実装するクラスがその `ItemsSource` プロパティに設定されたことが検出されると、`CollectionChanged` イベントにハンドラーがアタッチされ、コレクションが変更されたときにその表示が更新されます。

[**ObservableLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) サンプルは、`ObservableCollection`の使用例を示しています。

### <a name="templates-and-cells"></a>テンプレートとセル

既定では、`ListView` は、各項目の `ToString` メソッドを使用して、コレクション内の項目を表示します。 これよりも優れた手法は、項目を表示するためのテンプレートを定義することです。

この機能を試すには、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリ内の [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) クラスを使用することができます。 このクラスは、`IList<NamedColor>` 型の静的な `All` プロパティを定義します。これには、`Color` 構造体のパブリック フィールドに対応する 141 個の `NamedColor` オブジェクトが含まれます。

[**NaiveNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) サンプルでは、`ListView` の `ItemsSource` をこの `NamedColor.All` プロパティに設定しますが、`NamedColor` オブジェクトの完全修飾クラス名のみが表示されます。

`ListView` は、これらの項目を表示するためのテンプレートを必要とします。 コードでは、[`Cell`](xref:Xamarin.Forms.Cell) クラスの派生クラスを参照する [`DataTemplate` コンストラクター](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type))を使用して、`ItemsView<TVisual>` で定義された [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) オブジェクトに設定することができます。 `Cell` には、次の 5 つの派生クラスがあります。

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; 2 つの `Label` ビューが含まれます (概念上)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; `Image` ビューを `TextCell` に追加します
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; `Label` 付きの `Entry` ビューが含まれます
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; `Label` 付きの `Switch` が含まれます
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; 任意の `View`を指定できます (子を含むビューも可)

次に、`DataTemplate` オブジェクトの [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) と [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) を呼び出して、値を `Cell` プロパティと関連付けるか、`ItemsSource` コレクション内の項目のプロパティを参照する `Cell` プロパティに対してデータ バインディングを設定します。 この例は、[**TextCellListCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) サンプルで示されています。

各項目は、`ListView` によって表示されるため、テンプレートから小さいビジュアル ツリーが作成され、項目と、このビジュアル ツリー内の要素のプロパティとの間にデータ バインディングが確立されます。 このプロセスを理解するには、`ListView`の [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing) イベントおよび [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing) イベント用のハンドラーをインストールするか、項目のビジュアル ツリーを作成する必要があるたびに呼び出される関数を使用する代替の [`DataTemplate` コンストラクター](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) を使用します。

[**TextCellListXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) は、機能的に同じプログラム全体を XAML で示します。 `DataTemplate` タグが `ListView` の `ItemTemplate` プロパティに設定され、`TextCell` が `DataTemplate` に設定されます。 コレクション内の項目のプロパティへのバインディングは、`TextCell` の [`Text`](xref:Xamarin.Forms.TextCell.Text) プロパティと [`Detail`](xref:Xamarin.Forms.TextCell.Detail) プロパティに対して直接設定されます。

### <a name="custom-cells"></a>カスタム セル

XAML では、[`ViewCell`](xref:Xamarin.Forms.ViewCell) を `DataTemplate` に設定し、`ViewCell` の [`View`](xref:Xamarin.Forms.ViewCell.View) プロパティとしてカスタム ビジュアル ツリーを定義することができます (`View` は、`ViewCell` のコンテンツ プロパティであるため、`ViewCell.View` タグは必要ありません)。[**CustomNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) サンプルは、この手法を示しています。

[![名前付きカスタム カラー リストのトリプル スクリーンショット](images/ch19fg11-small.png "名前付きカスタム カラー リスト")](images/ch19fg11-large.png#lightbox "名前付きカスタム カラー リスト")

すべてのプラットフォームに適したサイズに調整するのは困難な場合があります。 [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight) は有用ですが、場合によっては、[`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) プロパティを使用する必要があり、効率が低下し、`ListView` の行のサイズが強制的に変更されることがあります。 iOS および Android では、これら 2 つのプロパティのいずれかを使用して、行を適切なサイズに調整する必要があります。

### <a name="grouping-the-listview-items"></a>ListView 項目のグループ化

`ListView` では、項目のグループ化とそれらのグループ間での移動がサポートされます。 `ItemsSource` プロパティをコレクションのコレクションに設定する必要があります。`ItemsSource` を設定するオブジェクトは、`IEnumerable` を実装する必要があり、コレクション内の各アイテムも `IEnumerable` を実装する必要があります。 各グループには、2 つのプロパティ (グループのテキスト説明と 3 文字の省略形) を含める必要があります。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリ内の [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) クラスは、`NamedColor` オブジェクトの 7 つのグループを作成します。 [**ColorGroupList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) サンプルは、`ListView` の [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled) プロパティを `true` に設定し、[`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) プロパティと [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) プロパティを各グループのプロパティにバインドしたグループの使用方法を示しています。

### <a name="custom-group-headers"></a>カスタム グループ ヘッダー

`GroupDisplayBinding` プロパティを、ヘッダーのテンプレートを定義する [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) に置き換えることにより、`ListView` グループのカスタム ヘッダーを作成できます。

### <a name="listview-and-interactivity"></a>ListView と対話機能

一般に、アプリケーションでは、ハンドラーを `ItemSelected` イベントまたは [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントにアタッチするか、`SelectedItem` プロパティに対してデータ バインディングを設定することにより、`ListView` のユーザー操作を取得します。 しかし、一部のセルの種類 (`EntryCell` および `SwitchCell`) ではユーザー操作が許可されます。また、自身でユーザーと対話するカスタム セルを作成することもできます。 [**InteractiveListView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) では、[`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) の 100 個のインスタンスが作成され、ユーザーは、3 つ 1 組の `Slider` 要素を使用して各色を変更できます。 このプログラムでは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 内の [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) も使用されます。

## <a name="listview-and-mvvm"></a>ListView と MVVM

`ListView` は、MVVM シナリオで大きな役割を果たします。 `IEnumerable` コレクションが ViewModel にあれば、多くの場合、`ListView` にバインドされます。 また、そのコレクション内の項目は、多くの場合、テンプレート内のプロパティとバインドするために `INotifyPropertyChanged` を実装しています。

### <a name="a-collection-of-viewmodels"></a>ViewModel のコレクション

これを調べるために、[**SchoolOfFineArts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) ライブラリは、この架空の学校の架空の学生の [XML データ ファイル](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml)とイメージに基づいて、いくつかのクラスを作成します。

[`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) クラスは、[`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs) から派生します。 [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) クラスは、`Student` オブジェクトのコレクションで、これも `ViewModelBase` から派生します。 [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) は、XML ファイルをダウンロードし、すべてのオブジェクトをアセンブルします。

[**StudentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) プログラムは、`ImageCell` を使用して、学生とそのイメージを `ListView` に表示します。

[![学生リストのトリプル スクリーンショット](images/ch19fg18-small.png "学生リスト")](images/ch19fg18-large.png#lightbox "学生リスト")

[**ListViewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) サンプルでは、[`Header`](xref:Xamarin.Forms.ListView.Header) プロパティを追加しますが、Android でのみ表示されます。

### <a name="selection-and-the-binding-context"></a>選択とバインディング コンテキスト

[**SelectedStudentDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) プログラムは、`StackLayout` の `BindingContext` を `ListView` の `SelectedItem` プロパティにバインドします。 これにより、プログラムは、選択された学生の詳細情報を表示できます。

### <a name="context-menus"></a>コンテキスト メニュー

セルでは、プラットフォーム固有の方法で実装されるコンテキスト メニューを定義できます。 このメニューを作成するには、[`MenuItem`](xref:Xamarin.Forms.MenuItem) オブジェクトを `Cell` の [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) プロパティに追加します。

`MenuItem` は、次の 5 つのプロパティを定義します。

- `string` 型の [`Text`](xref:Xamarin.Forms.MenuItem.Text)
- `FileImageSource` 型の [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)
- `bool` 型の [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)
- `ICommand` 型の [`Command`](xref:Xamarin.Forms.MenuItem.Command)
- `object` 型の [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)

`Command` プロパティと `CommandParameter` プロパティは、各項目の ViewModel に、目的のメニュー コマンドを実行するメソッドが含まれていることを意味します。 MVVM 以外のシナリオでは、`MenuItem` は、[`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) イベントも定義します。

[**CellContextMenu**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) は、この手法を示しています。 各 `MenuItem` の `Command` プロパティは、`Student` クラス内の `ICommand` 型のプロパティにバインドされます。 選択されたオブジェクトを削除する `MenuItem` については、`IsDestructive` プロパティを `true` に設定します。

### <a name="varying-the-visuals"></a>ビジュアルのバリエーション

プロパティによっては、`ListView` 内の項目のビジュアルを多少変化させることが必要になる場合があります。 たとえば、学生の成績評価の平均値が 2.0 を下回る場合、[**ColorCodedStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) サンプルは、その学生の名前を赤色で表示します。
これは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリ内のバインド データ コンバーター [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)を使用することによって実現されます。

### <a name="refreshing-the-content"></a>コンテンツの更新

`ListView` では、データを最新の情報に更新するためのプルダウン ジェスチャがサポートされます。 これを有効にするには、プログラムで、[`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) プロパティを `true` に設定する必要があります。 `ListView` は、その [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) プロパティを `true` に設定し、[`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) イベントを発生させて、(MVVM シナリオの場合) その [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) プロパティの `Execute` メソッドを呼び出すことにより、プルダウン ジェスチャに応答します。

この後、`Refresh` イベントを処理するコードまたは `RefreshCommand` により、`ListView` によって表示されるデータを更新して、`IsRefreshing` を `false`に戻すことができます。

[**RssFeed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) サンプルは、`RefreshCommand` プロパティと `IsRefreshing` プロパティを実装する [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) をデータ バインディングに使用する方法を示しています。

## <a name="the-tableview-and-its-intents"></a>TableView とその目的

通常、`ListView` では同じ型の複数のインスタンスが表示されますが、[`TableView`](xref:Xamarin.Forms.TableView) では、通常、さまざまな型の複数のプロパティのユーザー インターフェイスを提供することに重点が置かれています。 各項目は、プロパティを表示するため、またはユーザー インターフェイスを提供するために、自身の [`Cell`](xref:Xamarin.Forms.Cell) 派生クラスに関連付けられます。

### <a name="properties-and-hierarchies"></a>プロパティと階層

`TableView` では、次の 4 つのプロパティのみを定義します。

- [`TableIntent`](xref:Xamarin.Forms.TableIntent) 型の [`Intent`](xref:Xamarin.Forms.TableView.Intent) (列挙体)
- [`TableRoot`](xref:Xamarin.Forms.TableRoot) 型の [`Root`](xref:Xamarin.Forms.TableView.Root) (`TableView` のコンテンツ プロパティ)
- `int` 型の [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)
- `bool` 型の [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)

`TableIntent` 列挙体は、目的とする `TableView` の使用方法を示します。

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

これらのメンバーは、`TableView` のいくつかの用途も提案します。

他にもテーブルの定義に関係するクラスがいくつかあります。

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) は、`BindableObject` から派生する抽象クラスで、[`Title`](xref:Xamarin.Forms.TableSectionBase.Title) プロパティを定義します

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) は、`TableSectionBase` から派生する抽象クラスで、`IList<T>` と `INotifyCollectionChanged` を実装します

- [`TableSection`](xref:Xamarin.Forms.TableSection) は、`TableSectionBase<Cell>` から派生します

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) は、`TableSectionBase<TableSection>` から派生します

要するに、`TableView` には `Root` プロパティがあり、これを `TableRoot` オブジェクトに設定します。このオブジェクトは `TableSection` オブジェクトのコレクションで、その各オブジェクトは `Cell` オブジェクトのコレクションです。 テーブルには複数のセクションがあり、各セクションには複数のセルがあります。 テーブル自体にタイトルを付けることができ、各セクションにもタイトルを付けることができます。 `TableView` では、`Cell` 派生クラスを使用しますが、`DataTemplate`は使用しません。

### <a name="a-prosaic-form"></a>平凡なフォーム

[**EntryForm**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) サンプルでは、[`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) ビュー モデルを定義します。このビュー モデルのインスタンスは、`TableView`の `BindingContext` になります。 その `TableSection` 内の各 `Cell` 派生クラスには、`PersonalInformation` クラスのプロパティへのバインディングがあります。

### <a name="custom-cells"></a>カスタム セル

[**ConditionalCells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) は、**EntryForm** を拡張します。 [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) クラスには、2 つの追加プロパティの適用性を制御するブール型プロパティが含まれます。 これら 2 つの追加プロパティについて、プログラムでは、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリ内の [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) および [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) に基づいてカスタムの `PickerCell` を使用します。

2 つの `PickerCell` 要素の `IsEnabled` プロパティは `ProgrammerInformation`内のブール型プロパティにバインドされますが、この手法は機能しないようで、次のサンプルを要求します。

### <a name="conditional-sections"></a>条件付きセクション

[**ConditionalSection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) サンプルは、ブール型項目の選択を条件とする 2 つの項目を個別の `TableSection`に配置します。 分離コード ファイルは、ブール型プロパティに基づいて、このセクションを `TableView` から削除するか、元に戻します。

### <a name="a-tableview-menu"></a>TableView メニュー

`TableView` のもう 1 つの用途は、メニューです。 [**MenuCommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) サンプルは、小さな `BoxView` を画面上で移動させるメニューの例を示しています。

## <a name="related-links"></a>関連リンク

- [第 19 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [第 19 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ピッカー](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
