---
title: 19章の概要。 コレクションビュー
description: Xamarin. Forms を使用した Mobile Apps の作成:19 章の概要。 コレクションビュー
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: bffbd2dec4a8494723597ba6e0f0af69e57f3718
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032866"
---
# <a name="summary-of-chapter-19-collection-views"></a>19章の概要。 コレクションビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE] 
> このページのメモでは、分岐が書籍に記載されている素材から見た領域を示しています。

Xamarin は、コレクションを保持し、その要素を表示する3つのビューを定義します。

- [`Picker`](xref:Xamarin.Forms.Picker)は、ユーザーが選択できる文字列項目の比較的短い一覧です。
- 多くの場合、 [`ListView`](xref:Xamarin.Forms.ListView)は、通常同じ種類および書式設定の項目の長い一覧であり、ユーザーはこれを選択できます。
- [`TableView`](xref:Xamarin.Forms.TableView)は、データを表示したりユーザー入力を管理したりするための (通常、さまざまな種類や外観を持つ)*セル*のコレクションです。

MVVM アプリケーションでは、オブジェクトの選択可能なコレクションを表示するために `ListView` を使用するのが一般的です。

## <a name="program-options-with-picker"></a>ピッカーを使用したプログラムオプション

[`Picker`](xref:Xamarin.Forms.Picker)は、ユーザーが比較的短い `string` 項目の一覧からオプションを選択できるようにする必要がある場合に適しています。

### <a name="the-picker-and-event-handling"></a>ピッカーとイベント処理

[**PickerDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo)サンプルでは、XAML を使用して `Picker` [`Title`](xref:Xamarin.Forms.Picker.Title)プロパティを設定し、 [`Items`](xref:Xamarin.Forms.Picker.Items)コレクションに `string` 項目を追加する方法を示します。 ユーザーが `Picker`を選択すると、プラットフォームに依存する方法で `Items` コレクション内の項目が表示されます。

[`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントは、ユーザーが項目を選択したことを示します。 0から始まる[`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティは、選択された項目を示します。 項目が選択されていない場合、`SelectedIndex` は &ndash;1 になります。

`SelectedIndex` を使用して選択した項目を初期化することもできますが、`Items` コレクションがいっぱいになった後に設定する必要があります。 XAML では、property 要素を使用して `SelectedIndex`を設定することがあります。

### <a name="data-binding-the-picker"></a>ピッカーのデータバインディング

`SelectedIndex` プロパティは、バインド可能なプロパティによってサポートされていますが `Items` ではありません。そのため、`Picker` でデータバインディングを使用することは困難です。 1つの解決策として、`Picker` を[`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs)と組み合わせて使用する方法があります。たとえば、「」という形式[**のライブラリを**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)使用します。 [**PickerBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding)は、このしくみを示しています。

> [!NOTE] 
> Xamarin. Forms `Picker` には、データバインディングをサポートする `ItemsSource` と `SelectedItem` のプロパティが含まれるようになりました。 「[ピッカー](~/xamarin-forms/user-interface/picker/index.md)」を参照してください。

## <a name="rendering-data-with-listview"></a>ListView を使用したデータのレンダリング

[`ListView`](xref:Xamarin.Forms.ListView)は、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)および[`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)プロパティを継承する[`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1)から派生する唯一のクラスです。

`ItemsSource` の型は `IEnumerable` ですが、既定では `null`、明示的に初期化するか、データバインディングを使用してコレクションに設定する必要があります。 このコレクション内の項目は、任意の型にすることができます。

`ListView` は、`ItemsSource` コレクション内のいずれかの項目に設定された[`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)プロパティを定義するか、項目が選択されていない場合は `null` します。 新しい項目が選択されたときに[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントを発生させる `ListView` ます。

### <a name="collections-and-selections"></a>コレクションと選択

[**ListViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList)サンプルでは、`List<Color>` コレクション内の 17 `Color` の値を使用して `ListView` を入力します。 項目は選択可能ですが、既定では、魅力的 `ToString` 表現で表示されます。 この章のいくつかの例では、その表示を修正し、必要に応じて魅力的なものにする方法を示しています。

### <a name="the-row-separator"></a>行区切り記号

IOS と Android では、細い線によって行が分離されます。 これは、 [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility)と[`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor)のプロパティを使用して制御できます。 `SeparatorVisibility` プロパティは、次の2つのメンバーを持つ列挙型[`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility)です。

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default)、既定の設定
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>選択した項目のデータバインディング

`SelectedItem` プロパティは、バインド可能なプロパティによってサポートされるため、データバインディングのソースまたはターゲットのいずれかになります。 既定の `BindingMode` は `OneWayToSource`ですが、一般的には、MVVM のシナリオでは、双方向のデータバインディングのターゲットになります。 [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray)サンプルでは、この種類のバインドを示します。

### <a name="the-observablecollection-difference"></a>System.collections.objectmodel.observablecollection の相違点

[**ListViewLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger)サンプルは、`ListView` の `ItemsSource` プロパティを `List<DateTime>` コレクションに設定し、タイマーを使用して1秒ごとに新しい `DateTime` オブジェクトを段階的にコレクションに追加します。

ただし、コレクションに対して項目が追加または削除されたことを示す通知メカニズムが `List<T>` コレクションにないため、`ListView` は自動的には更新されません。

このようなシナリオで使用するクラスが非常に優れているのは、`System.Collections.ObjectModel` 名前空間で定義されている[`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1)です。 このクラスは[`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged)インターフェイスを実装し、コレクションに対して項目が追加または削除されたとき、またはコレクション内で項目が置換または移動されたときに[`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged)イベントを発生させます。 `INotifyCollectionChanged` を実装しているクラスがその `ItemsSource` プロパティに設定されていることを `ListView` 内部で検出すると、ハンドラーが `CollectionChanged` イベントにアタッチされ、コレクションが変更されたときにその表示が更新されます。

[**ObservableLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger)サンプルでは、`ObservableCollection`の使用方法を示します。

### <a name="templates-and-cells"></a>テンプレートとセル

既定では、`ListView` は、各項目の `ToString` メソッドを使用して、コレクション内の項目を表示します。 より良い方法は、項目を表示するテンプレートを定義することです。

この機能を試してみるには、 [**Xamarin. フォーム**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリの[`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs)クラスを使用します。 このクラスは、`Color` 構造体のパブリックフィールドに対応する 141 `NamedColor` オブジェクトを含む `IList<NamedColor>` 型の静的な `All` プロパティを定義します。

[**NaiveNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList)サンプルでは、`ListView` の `ItemsSource` をこの `NamedColor.All` プロパティに設定しますが、`NamedColor` オブジェクトの完全修飾クラス名のみが表示されます。

これらの項目を表示するには、`ListView` テンプレートが必要です。 コードでは、 [`Cell`](xref:Xamarin.Forms.Cell)クラスの派生クラスを参照する[`DataTemplate` コンストラクター](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type))を使用して、`ItemsView<TVisual>` で定義された[`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定できます。 `Cell` には、次の5つの派生物があります。

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; に2つの `Label` ビューが含まれている (概念的に言う)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; `Image` ビューをに追加し `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; に `Entry` ビューが含まれ `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; に `Switch` が含まれ `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; は任意の `View` にすることができます (子の場合もあります)。

次に、 [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object))を呼び出し、`DataTemplate` オブジェクトで[`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))して値を `Cell` プロパティに関連付けるか、`Cell` コレクション内の項目のプロパティを参照する `ItemsSource` プロパティでデータバインディングを設定します。 これについては、 [**Textcelllistcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode)サンプルに記載されています。

各項目は `ListView`によって表示されるため、テンプレートから小さいビジュアルツリーが構築され、このビジュアルツリー内の要素の項目とプロパティの間にデータバインディングが確立されます。 このプロセスの概念を理解するには、`ListView`の[`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing)および[`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing)イベントのハンドラーをインストールするか、項目のビジュアルツリーが必要になるたびに呼び出される関数を使用する代替の[`DataTemplate` コンストラクター](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object}))を使用します。作成されます。

[**Textcelllistxaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml)は、機能的に同一のプログラムを xaml で完全に表示します。 `DataTemplate` タグが `ListView`の `ItemTemplate` プロパティに設定され、`TextCell` が `DataTemplate`に設定されます。 コレクション内の項目のプロパティへのバインドは、`TextCell`の[`Text`](xref:Xamarin.Forms.TextCell.Text)および[`Detail`](xref:Xamarin.Forms.TextCell.Detail)プロパティに直接設定されます。

### <a name="custom-cells"></a>カスタムセル

XAML では、`DataTemplate` に[`ViewCell`](xref:Xamarin.Forms.ViewCell)を設定し、`ViewCell`の[`View`](xref:Xamarin.Forms.ViewCell.View)プロパティとしてカスタムビジュアルツリーを定義することができます。 (`View` は `ViewCell` の content プロパティであるため、`ViewCell.View` タグは必要ありません)。[**Customnamedcolorlist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList)サンプルは、次の手法を示しています。

[![カスタム名前付き色の一覧のトリプルスクリーンショット](images/ch19fg11-small.png "カスタムの名前付き色の一覧")](images/ch19fg11-large.png#lightbox "カスタムの名前付き色の一覧")

すべてのプラットフォームに対してサイズ変更を可能にすることは、難しい場合があります。 [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight)プロパティは便利ですが、場合によっては[`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows)プロパティを使用する方が効率的ですが、`ListView` によって行のサイズが強制的に変更されることもあります。 IOS と Android では、これら2つのプロパティのいずれかを使用して、適切な行のサイズ変更を行う必要があります。

### <a name="grouping-the-listview-items"></a>ListView 項目のグループ化

`ListView` は、項目のグループ化とそれらのグループ間での移動をサポートします。 `ItemsSource` プロパティは、コレクションのコレクションに設定する必要があります。 `ItemsSource` がに設定されているオブジェクトは `IEnumerable`を実装する必要があります。また、コレクション内の各項目も `IEnumerable`を実装する必要があります。 各グループには、グループのテキスト説明と3文字の省略形の2つのプロパティが含まれている必要があります。

[`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs)クラスで[**は、`NamedColor`** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)オブジェクトの7つのグループが作成されます。 [**Colorgrouplist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList)サンプルは、`true`に設定された `ListView` の[`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled)プロパティと、各グループのプロパティにバインドされた[`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding)および[`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding)プロパティを使用して、これらのグループを使用する方法を示しています。

### <a name="custom-group-headers"></a>カスタムグループヘッダー

`GroupDisplayBinding` プロパティをヘッダーのテンプレートを定義する[`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)に置き換えることによって、`ListView` グループのカスタムヘッダーを作成できます。

### <a name="listview-and-interactivity"></a>ListView と対話機能

一般に、アプリケーションは、`ItemSelected` または[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)イベントにハンドラーをアタッチするか、`SelectedItem` プロパティでデータバインディングを設定することによって、`ListView` とのユーザー操作を取得します。 ただし、一部のセルの種類 (`EntryCell` および `SwitchCell`) はユーザーの操作を許可します。また、ユーザーと対話するカスタムセルを作成することもできます。 [**Interactivelistview**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView)によって[`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs)の100インスタンスが作成され、ユーザーは `Slider` 要素の trio を使用して各色を変更できます。 また、プログラムでは、 [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs)を使用し[**ます。** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

## <a name="listview-and-mvvm"></a>ListView と MVVM

`ListView` は、MVVM のシナリオで大きな役割を果たします。 `IEnumerable` コレクションがビューモデルに存在する場合は、`ListView`にバインドされることがよくあります。 また、コレクション内の項目は、多くの場合、テンプレート内のプロパティにバインドするために `INotifyPropertyChanged` を実装します。

### <a name="a-collection-of-viewmodels"></a>Viewmodel のコレクション

これを調べるために、 [**SchoolOfFineArts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリは、この架空の学校の[XML データファイル](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml)と架空の学生のイメージに基づいて、いくつかのクラスを作成します。

[`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs)クラスは[`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)から派生します。 [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs)クラスは `Student` オブジェクトのコレクションであり、`ViewModelBase`から派生します。 [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs)によって XML ファイルがダウンロードされ、すべてのオブジェクトがアセンブルされます。

[**StudentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList)プログラムは、`ImageCell` を使用して、`ListView`に学生とそのイメージを表示します。

[![生徒リストのトリプルスクリーンショット](images/ch19fg18-small.png "学生リスト")](images/ch19fg18-large.png#lightbox "学生リスト")

[**ListViewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader)サンプルでは[`Header`](xref:Xamarin.Forms.ListView.Header)プロパティが追加されますが、Android にのみ表示されます。

### <a name="selection-and-the-binding-context"></a>選択とバインドコンテキスト

[**SelectedStudentDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail)プログラムは、`StackLayout` の `BindingContext` を `ListView`の `SelectedItem` プロパティにバインドします。 これにより、プログラムでは、選択した学生に関する詳細情報を表示できます。

### <a name="context-menus"></a>コンテキスト メニュー

セルは、プラットフォーム固有の方法で実装されるコンテキストメニューを定義できます。 このメニューを作成するには、 [`MenuItem`](xref:Xamarin.Forms.MenuItem)オブジェクトを `Cell`の[`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions)プロパティに追加します。

`MenuItem` は5つのプロパティを定義します。

- `string` 型の [`Text`](xref:Xamarin.Forms.MenuItem.Text)
- `FileImageSource` 型の [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)
- `bool` 型の [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)
- `ICommand` 型の [`Command`](xref:Xamarin.Forms.MenuItem.Command)
- `object` 型の [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)

`Command` プロパティと `CommandParameter` プロパティは、各項目のビューモデルに、目的のメニューコマンドを実行するメソッドが含まれていることを意味します。 MVVM 以外のシナリオでは、`MenuItem` も[`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked)イベントを定義します。

[**Cellcontextmenu**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu)は、この手法を示しています。 各 `MenuItem` の `Command` プロパティは、`Student` クラスの `ICommand` 型のプロパティにバインドされます。 選択したオブジェクトを削除または削除する `MenuItem` については、`IsDestructive` プロパティを `true` に設定します。

### <a name="varying-the-visuals"></a>視覚エフェクトの変更

場合によっては、プロパティに基づいて、`ListView` 内の項目の視覚エフェクトが多少変化することがあります。 たとえば、生徒の成績基準の平均が2.0 未満の場合、 [**Colorcodedstudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents)サンプルではその学生の名前が赤で表示されます。
これは、 [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)のバインド値コンバーターを使用することによって実現され[**ます。** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

### <a name="refreshing-the-content"></a>コンテンツを更新しています

`ListView` では、データを更新するためのプルダウンジェスチャがサポートされています。 この機能を有効にするには、プログラムで[`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled)プロパティを `true` に設定する必要があります。 `ListView` は、 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing)プロパティを `true`に設定し、 [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing)イベントと (MVVM シナリオの場合) [`Execute`](xref:Xamarin.Forms.ListView.RefreshCommand)プロパティの`RefreshCommand`メソッドを呼び出すことによって、プルダウンジェスチャに応答します。

`Refresh` イベントまたは `RefreshCommand` のコード処理では、`ListView` によって表示されるデータを更新し、`IsRefreshing` を `false`に戻すことができます。

[**の rss フィード**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed)サンプルでは、データバインディングの `RefreshCommand` と `IsRefreshing` プロパティを実装する[`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs)の使用方法を示します。

## <a name="the-tableview-and-its-intents"></a>TableView とそのインテント

`ListView` 通常、同じ種類の複数のインスタンスが表示されますが、 [`TableView`](xref:Xamarin.Forms.TableView)は通常、さまざまな種類の複数のプロパティのユーザーインターフェイスを提供することに重点を置いています。 各項目は、プロパティを表示したり、そのユーザーインターフェイスを提供したりするための独自の[`Cell`](xref:Xamarin.Forms.Cell)派生要素に関連付けられています。

### <a name="properties-and-hierarchies"></a>プロパティと階層

`TableView` は、次の4つのプロパティのみを定義します。

- 列挙型[`TableIntent`](xref:Xamarin.Forms.TableIntent)型の[`Intent`](xref:Xamarin.Forms.TableView.Intent)
- の content プロパティ[`TableRoot`](xref:Xamarin.Forms.TableRoot)型の[`Root`](xref:Xamarin.Forms.TableView.Root) `TableView`
- `int` 型の [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)
- `bool` 型の [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)

`TableIntent` 列挙体は、`TableView`の使用方法を示します。

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

これらのメンバーは、`TableView`の一部の使用も提案します。

テーブルの定義には、他にもいくつかのクラスが関係しています。

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase)は `BindableObject` から派生し[`Title`](xref:Xamarin.Forms.TableSectionBase.Title)プロパティを定義する抽象クラスです。

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1)は `TableSectionBase` から派生し、`IList<T>` とを実装する抽象クラスです `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection)は `TableSectionBase<Cell>` から派生します

- [`TableRoot`](xref:Xamarin.Forms.TableRoot)は `TableSectionBase<TableSection>` から派生します

つまり、`TableView` には、`TableRoot` オブジェクトに設定する `Root` プロパティがあります。これは `TableSection` オブジェクトのコレクションであり、それぞれが `Cell` オブジェクトのコレクションです。 テーブルには複数のセクションがあり、各セクションには複数のセルがあります。 テーブル自体にはタイトルを付けることができ、各セクションにはタイトルを付けることができます。 `TableView` は `Cell` 派生物を使用しますが、`DataTemplate`を使用することはありません。

### <a name="a-prosaic-form"></a>Prosaic フォーム

[**Entryform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm)サンプルでは、 [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)ビューモデルを定義しています。このインスタンスが `TableView`の `BindingContext` になります。 `TableSection` 内の各 `Cell` 派生元は、`PersonalInformation` クラスのプロパティへのバインドを持つことができます。

### <a name="custom-cells"></a>カスタムセル

[**Conditionalcells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells)サンプルは、 **entryform**で展開します。 [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)クラスには、2つの追加プロパティの適用性を制御するブール型プロパティが含まれています。 この2つの追加プロパティについては、プログラムは[PickerCell](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml)と[PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs)に基づいてカスタムの `PickerCell` を使用し[**ます**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)。

2つの `PickerCell` 要素の `IsEnabled` プロパティは `ProgrammerInformation`のブール型プロパティにバインドされていますが、この手法は機能しないと思われます。これにより、次のサンプルが表示されます。

### <a name="conditional-sections"></a>条件付きセクション

[**Conditionalsection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection)サンプルでは、別の `TableSection`のブール型項目の選択に条件を持つ2つの項目を配置します。 分離コードファイルは、このセクションを `TableView` から削除するか、ブール型プロパティに基づいて追加し直します。

### <a name="a-tableview-menu"></a>TableView メニュー

`TableView` のもう1つの用途はメニューです。 [**Menucommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands)サンプルは、画面の周りを少し `BoxView` 移動できるメニューを示しています。

## <a name="related-links"></a>関連リンク

- [第19章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ピッカー](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
