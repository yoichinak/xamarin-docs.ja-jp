---
title: 19 章の概要です。 コレクション ビュー
description: 'Xamarin.Forms によるモバイル アプリの作成: 19 章の概要。 コレクション ビュー'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7ae6ff5bb08977ab83f95242770794b4c4363145
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935174"
---
# <a name="summary-of-chapter-19-collection-views"></a>19 章の概要です。 コレクション ビュー

Xamarin.Forms では、コレクションを保持し、その要素を表示する 3 つのビューを定義します。

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ユーザーがいずれかを選択できる比較的短い文字列アイテムの一覧を示します
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 通常、同じ種類の項目の長い一覧は、多くの場合と書式設定、ユーザーが 1 つを選択できるようになります
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) コレクションである*セル*(通常はさまざまな種類と外観) のデータを表示したり、ユーザーの入力を管理するには

MVVM アプリケーションを使用するが一般的、`ListView`オブジェクトの選択可能なコレクションを表示します。

## <a name="program-options-with-picker"></a>ピッカーを使用してプログラムのオプション

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)の比較的短い一覧の中からオプションを選択するユーザーを許可する必要がある場合、適切な選択は、`string`項目。

### <a name="the-picker-and-event-handling"></a>ピッカーとイベントの処理

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo)サンプル XAML を使用して設定する方法を示します、 `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/)プロパティを追加および`string`項目を[ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション。 ユーザーが選択すると、 `Picker`、内の項目が表示されます、`Items`プラットフォームに依存する形でのコレクション。

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベントは、ユーザーが項目を選択した場合を示します。 0 から始まる[ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティは、選択した項目を示します。 項目が選択されていない場合`SelectedIndex`equals &ndash;1。

使用することも`SelectedIndex`後に設定する必要がありますが、選択した項目を初期化するために、`Items`コレクションを格納します。 XAML、つまりに設定するプロパティ要素を使用することでしょうが`SelectedIndex`します。

### <a name="data-binding-the-picker"></a>データ連結ピッカー

`SelectedIndex`プロパティのバインド可能なプロパティによってバックアップしますが、`Items`によるデータ バインディングを使用してではない、`Picker`は困難です。 1 つのソリューションは、使用する、`Picker`と組み合わせて、 [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs)などの[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。 [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding)このしくみを示します。

## <a name="rendering-data-with-listview"></a>ListView でのデータの表示

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)が唯一のクラスから派生した[ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)から継承する、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/)と[ `ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/)プロパティ。

`ItemsSource` 種類は`IEnumerable`が`null`既定と明示的に初期化または (より一般的な) データ バインディングをコレクションに設定する必要があります。 このコレクション内の項目は、任意の型指定できます。

`ListView` 定義、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)内の項目のいずれかにいずれかであるプロパティの設定、`ItemsSource`コレクションまたは`null`項目が選択されていない場合。 `ListView` 起動、 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)イベントの新しい項目を選択します。

### <a name="collections-and-selections"></a>コレクションと選択

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList)サンプルの塗りつぶしを`ListView`17`Color`値、`List<Color>`コレクション。 項目を選択できますが、その延びると共に既定で表示された`ToString`表現。 この章のいくつかの例では、その表示を修正し、必要なことが、魅力的な方法を示します。

### <a name="the-row-separator"></a>行区切り記号

IOS と Android の表示では、細い線は、行を区切ります。 この操作を制御することができます、 [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/)と[ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/)プロパティ。 `SeparatorVisibility` プロパティの型は[ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/)、2 つのメンバーを列挙します。

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default)、既定の設定
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>データ バインディングの選択項目

`SelectedItem`ソースまたはデータ バインディングのターゲットにするためのプロパティのバインド可能なプロパティでバックアップします。 既定の`BindingMode`は`OneWayToSource`が特に MVVM シナリオでの双方向データ バインドのターゲットは一般にします。 [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray)サンプルは、この種類のバインドを示します。

### <a name="the-observablecollection-difference"></a>ObservableCollection の違い

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger)サンプル セット、`ItemsSource`のプロパティを`ListView`を`List<DateTime>`コレクションし、新しい段階的に追加`DateTime`オブジェクトをコレクションすべて 2 番目のタイマーを使用します。

ただし、`ListView`は自動的に更新されますので、`List<T>`コレクション アイテムに追加またはコレクションから削除するときを示す通知メカニズムはありません。

このようなシナリオで使用するはるかに優れたクラスは、 [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/)で定義されている、`System.Collections.ObjectModel`名前空間。 このクラスは、実装、 [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/)インターフェイスとその結果発生、 [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/)項目の追加または交換または内で移動されるときに、コレクション、またはを削除したときにイベントコレクションです。 ときに、`ListView`ことを検出した、内部的に実装するクラス`INotifyCollectionChanged`に設定されているその`ItemsSource`プロパティ ハンドラーをアタッチ、`CollectionChanged`イベント コレクションが変更されたときに表示が更新と。

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger)サンプルの使用を示します`ObservableCollection`します。

### <a name="templates-and-cells"></a>テンプレートとセル

既定で、`ListView`の各項目を使用してそのコレクション アイテムを表示`ToString`メソッド。 優れたアプローチでは、項目を表示するテンプレートを定義する必要があります。

この機能をテストするを使用できます、 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。 このクラスの定義、静的な`All`型のプロパティ`IList<NamedColor>`141 を格納している`NamedColor`のパブリック フィールドに対応するオブジェクト、`Color`構造体。

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList)サンプル セット、`ItemsSource`の`ListView`この`NamedColor.All`プロパティがの完全修飾クラス名のみ、`NamedColor`オブジェクトは、表示されます。

`ListView` これらの項目を表示するテンプレートが必要です。 コードでは、設定することができます、 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/)プロパティによって定義された`ItemsView<TVisual>`を[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)オブジェクトを使用して、 [ `DataTemplate`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/)を派生クラスの参照、 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)クラス。 `Cell` 5 つの派生クラスがあります。

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &mdash; 2 つ`Label`(概念的には) ビュー
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &mdash; 追加、`Image`を表示 `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &mdash; 含まれています、`Entry`で表示します。 `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &mdash; 含まれています、`Switch`で、 `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &mdash; いずれかを指定できます`View`(子を持つ可能性があります)

呼び出して[ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)と[ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)上、`DataTemplate`オブジェクトの値を関連付ける、`Cell`プロパティ、またはデータ バインディングを設定する、 `Cell`プロパティ内の項目のプロパティを参照する、`ItemsSource`コレクション。 これは、方法については、 [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode)サンプル。

各項目が表示されるよう、 `ListView`、小規模のビジュアル ツリーは、テンプレートから構築され、このビジュアル ツリー内のアイテムと、要素のプロパティのデータ バインドが確立されています。 このプロセスの考え方のハンドラーをインストールすることによって取得できます、 [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/)と[ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/)のイベント、 `ListView`、または別の方法を使用して[ `DataTemplate`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/)アイテムのビジュアル ツリーを作成する必要がありますたびに呼び出される関数を使用します。

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml)全体を XAML でのコードと同じプログラムを示しています。 A`DataTemplate`にタグが設定されている、`ItemTemplate`のプロパティ、`ListView`をクリックし、`TextCell`に設定されている、`DataTemplate`します。 コレクション内の項目のプロパティへのバインドはで直接設定、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/)と[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/)のプロパティ、`TextCell`します。

### <a name="custom-cells"></a>カスタムのセル

XAML で設定することは、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)を`DataTemplate`としてカスタムのビジュアル ツリーを定義し、 [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/)プロパティの`ViewCell`します。 (`View`の content プロパティは、`ViewCell`ため、`ViewCell.View`タグは必要ありません)。[ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList)サンプルは、この手法を示します。

[![カスタムという名前の色の一覧のスクリーン ショットをトリプル](images/ch19fg11-small.png "という名前の色の一覧のカスタム")](images/ch19fg11-large.png#lightbox "カスタムという名前の色の一覧")

すべてのプラットフォームに適したサイズを取得すると、厄介なことができます。 [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/)プロパティは役立ちますが、場合によってに頼る必要あります、 [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/)非効率であるプロパティが強制的に、`ListView`行のサイズを変更します。 IOS と Android では、これら 2 つのプロパティのいずれかを使用する必要があります、適切な行のサイズ変更を取得します。

### <a name="grouping-the-listview-items"></a>ListView の項目をグループ化

`ListView` 項目のグループ化し、それらのグループ間の移動をサポートしています。 `ItemsSource`コレクションのコレクションにプロパティを設定する必要があります。 オブジェクトを`ItemsSource`必要がありますに設定されている実装`IEnumerable`、し、コレクション内の各項目を実装する必要がありますも`IEnumerable`。 各グループは、2 つのプロパティを含める必要があります: グループと、3 文字の省略形のテキストの説明。

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリの 7 つのグループが作成`NamedColor`オブジェクト。 [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList)サンプルでは、これらのグループを使用して、 [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/)プロパティの`ListView`に設定`true`、および、 [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/)と[ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/)プロパティは、各グループのプロパティにバインドします。

### <a name="custom-group-headers"></a>カスタムのグループ ヘッダー

カスタム ヘッダーを作成することは、`ListView`置き換えることで、グループ、`GroupDisplayBinding`プロパティを[ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)ヘッダーのテンプレートを定義します。

### <a name="listview-and-interactivity"></a>ListView と対話機能

アプリケーションがユーザーのやり取りを取得する一般的に、`ListView`ハンドラーをアタッチすることにより、`ItemSelected`または[ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/)イベント、またはデータ バインディングを設定して、`SelectedItem`プロパティ。 セルの種類をいくつかが、(`EntryCell`と`SwitchCell`)、ユーザーの操作を許可してもをカスタムのセルがその自体を作成することがユーザーと対話します。 [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView)の 100 のインスタンスを作成[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 、ユーザーの 3 つを使用して各色を変更できるように`Slider`要素。 プログラムの使用も、 [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs)で、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)します。

## <a name="listview-and-mvvm"></a>ListView と MVVM

`ListView` MVVM のシナリオで大きな役割を果たします。 たびに、 `IEnumerable` ViewModel にコレクションが存在する、多くの場合にバインドされている、`ListView`します。 多くの場合、コレクション内の項目を実装することも、`INotifyPropertyChanged`テンプレートのプロパティにバインドします。

### <a name="a-collection-of-viewmodels"></a>Viewmodel のコレクション

これには、探索する、 [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)ライブラリに基づくいくつかのクラスが作成、 [XML データ ファイル](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml)と、この架空の学校の架空の受講者のイメージ。

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs)クラスから派生[ `ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)します。 [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs)クラスのコレクションである`Student`オブジェクトおよびからも派生`ViewModelBase`します。 [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML ファイルをダウンロードし、すべてのオブジェクトをアセンブルします。

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList)プログラムの使用、`ImageCell`受講者とでは、そのイメージを表示する、 `ListView`:

[![学生の一覧の 3 倍になるスクリーン ショット](images/ch19fg18-small.png "の生徒一覧")](images/ch19fg18-large.png#lightbox "の生徒一覧")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader)サンプルを追加、 [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/)プロパティのみが表示されます Android で。

### <a name="selection-and-the-binding-context"></a>選択およびバインディング コンテキスト

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail)バインドをプログラム、`BindingContext`の`StackLayout`を`SelectedItem`のプロパティ、`ListView`します。 これにより、選択した学生に関する詳細情報を表示するプログラムです。

### <a name="context-menus"></a>コンテキスト メニュー

セルには、プラットフォーム固有の方法で実装されているコンテキスト メニューを定義できます。 このメニューを作成する追加[ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)オブジェクトを[ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/)のプロパティ、`Cell`します。

`MenuItem` 5 つのプロパティを定義します。

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) 型の `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) 型の `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) 型の `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) 型の `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) 型の `object`

`Command`と`CommandParameter`プロパティは、各項目の ViewModel に目的のメニュー コマンドを実行するメソッドが含まれているを意味します。 MVVM 以外の場合、`MenuItem`も定義、 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/)イベント。

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu)この手法を実証します。 `Command`の各プロパティ`MenuItem`型のプロパティにバインドされて`ICommand`で、`Student`クラス。 設定、`IsDestructive`プロパティを`true`の`MenuItem`を削除または選択したオブジェクトを削除します。

### <a name="varying-the-visuals"></a>ビジュアルの変更

内の項目のビジュアルと比べてたい場合があります、`ListView`プロパティに基づいています。 たとえば、ときに学生の成績の平均を下回る 2.0 では、 [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents)サンプルでは、赤で学生の名前が表示されます。
これは、情報は、バインディングの値コンバーターを使用して[ `ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)の[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。

### <a name="refreshing-the-content"></a>コンテンツを更新します。

`ListView`そのデータの更新のプルダウン ジェスチャをサポートしています。 プログラムを設定する必要があります、 [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/)プロパティを`true`これを有効にします。 `ListView`を設定してプルダウン ジェスチャに応答の[ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/)プロパティを`true`、発生させることにより、 [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/)イベントと (の MVVM シナリオ) を呼び出す`Execute`のメソッド、 [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/)プロパティ。

コードの処理、`Refresh`イベントまたは`RefreshCommand`によって表示されるデータを更新する可能性があります、`ListView`設定と`IsRefreshing`に`false`します。

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed)サンプルの使用例、 [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs)を実装する`RefreshCommand`と`IsRefreshing`データ バインディングのプロパティ。

## <a name="the-tableview-and-its-intents"></a>テーブル、およびそのインテント

中に、`ListView`一般に、同じ型での複数のインスタンスが表示されます、 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)さまざまな種類の複数のプロパティのユーザー インターフェイスを提供することには、一般に重点を置いています。 各項目は、独自に関連付けられて[ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)プロパティを表示するか、ユーザー インターフェイスを提供することを派生します。

### <a name="properties-and-hierarchies"></a>プロパティと階層

`TableView` 4 つのみのプロパティを定義します。

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) 型の[ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/)、列挙型
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) 型の[ `TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)の content プロパティ `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) 型の `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) 型の `bool`

`TableIntent`列挙型を使用する方法を示します、 `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

これらのメンバーでは、いくつかの使用にもお勧め、`TableView`します。

テーブルを定義するその他のいくつかのクラスが含まれます。

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) 抽象クラスから派生した`BindableObject`を定義し、 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/)プロパティ

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) 抽象クラスから派生した`TableSectionBase`実装と`IList<T>`と `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) を派生します。 `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) を派生します。 `TableSectionBase<TableSection>`

簡単に言えば、`TableView`が、`Root`に設定するプロパティを`TableRoot`コレクションであるオブジェクトの`TableSection`オブジェクトのコレクションは、それぞれの`Cell`オブジェクト。 テーブルは複数のセクションを備え、各セクションには複数のセル。 テーブル自体はことができます、タイトルと各セクションは、タイトルがあることができます。 `TableView`活用`Cell`派生クラスでは、これを行わないの使用`DataTemplate`します。

### <a name="a-prosaic-form"></a>Prosaic フォーム

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm)サンプルを定義、 [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)がのインスタンスが、ビュー モデル、`BindingContext`の`TableView`します。 各`Cell`で派生その`TableSection`のプロパティへのバインドには、`PersonalInformation`クラス。

### <a name="custom-cells"></a>カスタムのセル

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells)サンプルを拡張**EntryForm**します。 [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)クラスには、2 つのプロパティの適用性を制御するブール型プロパティが含まれています。 これら 2 つのプロパティをプログラムは、カスタムを使用して`PickerCell`に基づいて、 [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml)と[PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。

ですが、 `IsEnabled` 、2 つのプロパティ`PickerCell`要素がブール型プロパティにバインドされて`ProgrammerInformation`、この手法はしていないするには、次のサンプルが求められます。

### <a name="conditional-sections"></a>条件付きセクション

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection)サンプルでは、2 つの項目は個別のブール型の項目の選択範囲で条件付き`TableSection`します。 分離コード ファイルからこのセクションの削除、`TableView`または戻るに基づいてブール型プロパティを追加します。

### <a name="a-tableview-menu"></a>テーブルのメニュー

別の用途を`TableView`メニューします。 [ **Menucommand** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands)サンプルでは少し移動することができます] メニューの [`BoxView`画面。



## <a name="related-links"></a>関連リンク

- [19 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
