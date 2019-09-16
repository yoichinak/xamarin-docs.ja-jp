---
title: ListView の対話機能
description: この記事では、選択、コンテキスト アクション、およびプルして更新を実装することで、Xamarin.Forms ListView に対話機能を追加する方法について説明します。
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: e2d51f42339b1ff2a99f2a00bb5a9e662fb01d87
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998076"
---
# <a name="listview-interactivity"></a>ListView の対話機能

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView)クラスは、表示されるデータとのユーザー操作をサポートします。

## <a name="selection-and-taps"></a>選択とタップ

[ `ListView` ](xref:Xamarin.Forms.ListView)選択モードが設定によって制御される、 [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode)プロパティの値を[ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode)列挙体。

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) を強調表示されている選択項目を 1 つの項目を選択できることを示します。 これが既定値です。
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) 項目を選択できないことを示します。

ユーザーが項目をタップする 2 つのイベントが発生します。

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 新しい項目が選択されていると発生します。
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 項目がタップされたときに発生します。

同じ項目を 2 回タップすると 2 つが起動されます[ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) 、イベントはのみ火災単一[ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)イベント。

> [!NOTE]
> `ItemIndex` [`ListView`](xref:Xamarin.Forms.ListView) [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item)イベントのイベント引数を格納している[クラス、プロパティ、プロパティ、およびタップされた項目の内のインデックスを表す値を持つプロパティが含まれています。`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)同様に`ListView` 、 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントのイベント引数を含むクラスには、 [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem)プロパティと`SelectedItemIndex` 、選択した項目の内のインデックスを表す値を持つプロパティがあります。

ときに、 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode)プロパティに設定されて[ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single)、項目を[ `ListView` ](xref:Xamarin.Forms.ListView)選択できる、 [ `ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)と[ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped)イベントは発生し、 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)プロパティは、選択した項目の値に設定されます。

ときに、 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode)プロパティに設定されて[ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None)、項目を[ `ListView` ](xref:Xamarin.Forms.ListView)選択することはできません、 [ `ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントは発生しませんが、および[ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)プロパティが引き続き`null`します。 ただし、 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped)イベントも発生して、タップ中に、タップされた項目が簡単に強調表示されます。

項目が選択されている場合、 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode)からプロパティが変更された[ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single)に[ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None)、 [ `SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)プロパティに設定する`null`と[ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)で発生する、`null`項目。

次のスクリーン ショットに示す、 [ `ListView` ](xref:Xamarin.Forms.ListView)既定の選択モードにします。

![](interactivity-images/selection-default.png "ListView で選択が有効な")

### <a name="disable-selection"></a>選択の無効化

無効にする[ `ListView` ](xref:Xamarin.Forms.ListView)選択セット、 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode)プロパティを[ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>コンテキスト アクション

多くの場合、ユーザーは、アクション内の項目を実行する必要が、`ListView`します。 たとえば、メール アプリで電子メールの一覧を検討してください。 Ios では、メッセージを削除するスワイプできます。

![](interactivity-images/context-default.png "コンテキスト アクションを含む ListView")

コンテキスト アクションは、C# と XAML で実装できます。 以下が見つかりますごとのガイドの場合がまずみましょう両方のキーの実装の詳細について見ています。

コンテキストアクションは、要素`MenuItem`を使用して作成されます。 オブジェクトの`MenuItems` Tap イベントは、では`MenuItem`なく`ListView`、自体によって発生します。 これは、セルに対して tap イベントがどのように処理`ListView`されるかとは異なります。では、セルではなくイベントが発生します。 `ListView`がイベントを発生させるため、そのイベントハンドラーには、選択またはタップされた項目などのキー情報が与えられます。

既定では、 `MenuItem`に属しているセルを認識する方法はありません。 プロパティは、 `MenuItem` `MenuItem`のの背後にあるオブジェクトなどのオブジェクトを格納するためにで使用できます。`ViewCell` `CommandParameter` プロパティは、XAML とC#の両方で設定できます。 `CommandParameter`

### <a name="xaml"></a>XAML

`MenuItem`要素は、XAML コレクション内に作成できます。 次の XAML では、実装した 2 つのコンテキスト アクションでカスタムのセルを示しています。

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore"
                      CommandParameter="{Binding .}"
                      Text="More" />
            <MenuItem Clicked="OnDelete"
                      CommandParameter="{Binding .}"
                      Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

分離コード ファイルで確認、`Clicked`メソッドが実装されます。

```csharp
public void OnMore (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer`の android デバイスには、オーバーライド可能な`UpdateMenuItemIcon`カスタムからアイコンの読み込みに使用できるメソッド`Drawable`します。 このオーバーライドでは、アイコンとして SVG イメージを使用できる`MenuItem`Android 上のインスタンス。

### <a name="code"></a>コード

コンテキストアクションは、インスタンスを作成`Cell` `MenuItem` `ContextActions`してそれをセルのコレクションに追加することで、任意のサブクラス (グループヘッダーとして使用されていない場合) で実装できます。 次のものをコンテキスト アクションのプロパティを構成できます。

- **テキスト**&ndash;メニュー項目に表示される文字列。
- **クリックされた**&ndash;項目がクリックされたときのイベント。
- **Isdestructive**&ndash; (省略可能) true の場合、項目は iOS では異なる方法で表示されます。

複数のコンテキストのアクションは、1 つだけである必要がありますが、セルに追加できる`IsDestructive`設定`true`します。 次のコードでは、コンテキスト アクションに追加される方法を示しています、 `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

## <a name="pull-to-refresh"></a>プルして更新

データの一覧をプルダウンと、その一覧は更新ことを期待するユーザーになっています。 コントロール[`ListView`](xref:Xamarin.Forms.ListView)では、この機能がすぐにサポートされます。 プルから更新機能を有効にするには[`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) 、 `true`をに設定します。

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

同等の C# コードに示します。

```csharp
listView.IsPullToRefreshEnabled = true;
```

更新中にスピンボタンが表示されます。これは既定では黒です。 ただし、 `RefreshControlColor`プロパティをに設定[`Color`](xref:Xamarin.Forms.Color)することにより、iOS と Android ではスピンボタンの色を変更できます。

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

同等の C# コードに示します。

```csharp
listView.RefreshControlColor = Color.Red;
```

次のスクリーンショットは、ユーザーがプルしているときのプルツーリフレッシュを示しています。

![](interactivity-images/refresh-start.png "ListView のプルで進行状況を更新するには")

次のスクリーンショットは、ユーザーがプルを解放した後のプルから更新を示しています。 [`ListView`](xref:Xamarin.Forms.ListView)これは、の更新中にスピンボタンが表示されます。

![](interactivity-images/refresh-in-progress.png "ListView の更新が完了しました")

[`ListView`](xref:Xamarin.Forms.ListView)イベントを発生させて更新を開始[`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing)し、プロパティをに`true`設定します。 [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) の`ListView`内容を更新するために必要なすべてのコードは、 `Refreshing`イベントのイベントハンドラー、またはによっ[`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)て実行されるメソッドによって実行される必要があります。 が更新されたら`IsRefreshing` 、更新が完了したことを示す[`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh)ために、プロパティをに`false`設定するか、メソッドを呼び出す必要があります。 `ListView`

> [!NOTE]
> を[`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)定義するときに`CanExecute` 、コマンドのメソッドを指定して、コマンドを有効または無効にすることができます。

## <a name="related-links"></a>関連リンク

- [ListView の対話機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
