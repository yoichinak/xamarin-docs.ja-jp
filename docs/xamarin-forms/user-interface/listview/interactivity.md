---
title: ''
description: この記事では Xamarin.Forms 、選択、コンテキストアクション、およびプルから更新を実装して ListView にインタラクティビティを追加する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5142965216b328172ae7fa04cdc0c13590f5ff38
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139888"
---
# <a name="listview-interactivity"></a>ListView の対話機能

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

クラスは、表示さ Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) れるデータとのユーザー操作をサポートします。

## <a name="selection-and-taps"></a>選択とタップ

[`ListView`](xref:Xamarin.Forms.ListView)選択モードを制御するには、 [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) プロパティを列挙体の値に設定し [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) ます。

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)選択した項目を強調表示して、1つの項目を選択できることを示します。 これが既定値です。
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)項目を選択できないことを示します。

ユーザーが項目をタップすると、次の2つのイベントが発生します。

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)新しい項目が選択されたときに発生します。
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)項目がタップされたときに発生します。

同じ項目を2回タップすると、2つのイベントが発生し [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) ますが、1つのイベントのみが発生 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) します。

> [!NOTE]
> [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)イベントのイベント引数を格納しているクラス、 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) プロパティ、プロパティ、およびタップされた `ItemIndex` 項目の内のインデックスを表す値を持つプロパティが含まれてい [`ListView`](xref:Xamarin.Forms.ListView) ます。 同様に、 [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) イベントのイベント引数を含むクラスに [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) は、 [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) プロパティと、選択し `SelectedItemIndex` た項目の内のインデックスを表す値を持つプロパティがあり `ListView` ます。

プロパティがに設定されている場合、 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) の項目を選択し、 [`ListView`](xref:Xamarin.Forms.ListView) [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントとイベントが発生し、プロパティが選択され [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) た項目の値に設定されます。

プロパティがに設定されている場合、 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) の項目を [`ListView`](xref:Xamarin.Forms.ListView) 選択でき [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) ないため、イベントは発生せず、プロパティはその [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) まま `null` です。 ただし、 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントは発生しますが、タップしたときにタップされた項目が簡潔に強調表示されます。

項目が選択され、 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) プロパティがからに変更されると [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) 、 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) プロパティがに設定され、項目を使用して `null` [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントが発生し `null` ます。

次のスクリーンショットは、既定の選択モードのを示してい [`ListView`](xref:Xamarin.Forms.ListView) ます。

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>選択の無効化

選択を無効にするに [`ListView`](xref:Xamarin.Forms.ListView) は、 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) プロパティを次のように設定し [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) ます。

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>コンテキスト アクション

多くの場合、ユーザーはの項目に対してアクションを実行する必要があり `ListView` ます。 たとえば、メールアプリの電子メールの一覧を考えてみましょう。 IOS では、スワイプしてメッセージを削除することができます。

![](interactivity-images/context-default.png "ListView with Context Actions")

コンテキストアクションは、C# および XAML で実装できます。 以下では、その両方に関する特定のガイドを紹介しますが、まず、その両方についていくつかの重要な実装の詳細を見てみましょう。

コンテキストアクションは、要素を使用して作成され `MenuItem` ます。 オブジェクトの Tap イベントは、では `MenuItems` なく、自体によって発生し `MenuItem` `ListView` ます。 これは、セルに対して tap イベントがどのように処理されるかとは異なります。では、 `ListView` セルではなくイベントが発生します。 `ListView`がイベントを発生させるため、そのイベントハンドラーには、選択またはタップされた項目などのキー情報が与えられます。

既定では、に `MenuItem` 属しているセルを認識する方法はありません。 プロパティは、のの背後にあるオブジェクトなどの `CommandParameter` オブジェクトを `MenuItem` 格納するためにで使用でき `MenuItem` `ViewCell` ます。 プロパティは、 `CommandParameter` XAML と C# の両方で設定できます。

### <a name="xaml"></a>XAML

`MenuItem`要素は、XAML コレクション内に作成できます。 次の XAML は、2つのコンテキストアクションが実装されたカスタムセルを示しています。

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

分離コードファイルで、 `Clicked` メソッドが実装されていることを確認します。

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
> Android 用には、 `NavigationPageRenderer` `UpdateMenuItemIcon` カスタムからアイコンを読み込むために使用できる、オーバーライド可能なメソッドがあり `Drawable` ます。 このオーバーライドにより、Android 上のインスタンスのアイコンとして SVG イメージを使用できるようになり `MenuItem` ます。

### <a name="code"></a>コード

コンテキストアクションは、 `Cell` インスタンスを作成してそれを `MenuItem` セルのコレクションに追加することで、任意のサブクラス (グループヘッダーとして使用されていない場合) で実装でき `ContextActions` ます。 コンテキストアクションに対して次のプロパティを構成できます。

- **テキスト** &ndash;メニュー項目に表示される文字列。
- **クリック** &ndash;項目がクリックされたときのイベント。
- **Isdestructive** &ndash;(省略可能) true の場合、項目は iOS では異なる方法で表示されます。

複数のコンテキストアクションを1つのセルに追加できますが、をに設定する必要があるのは1つだけ `IsDestructive` `true` です。 次のコードは、コンテキストアクションをに追加する方法を示してい `ViewCell` ます。

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

## <a name="pull-to-refresh"></a>引っ張って更新

ユーザーは、データの一覧を取得して一覧を更新することを期待しています。 [`ListView`](xref:Xamarin.Forms.ListView)コントロールでは、この機能がすぐにサポートされます。 プルから更新機能を有効にするには、 [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) をに設定し `true` ます。

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

同等の C# コードを次に示します。

```csharp
listView.IsPullToRefreshEnabled = true;
```

更新中にスピンボタンが表示されます。これは既定では黒です。 ただし、プロパティをに設定することにより、iOS と Android ではスピンボタンの色を変更でき `RefreshControlColor` [`Color`](xref:Xamarin.Forms.Color) ます。

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

同等の C# コードを次に示します。

```csharp
listView.RefreshControlColor = Color.Red;
```

次のスクリーンショットは、ユーザーがプルしているときのプルツーリフレッシュを示しています。

![](interactivity-images/refresh-start.png "ListView Pull to Refresh In-Progress")

次のスクリーンショットは、ユーザーがプルを解放した後のプルから更新を示しています。これは、の更新中にスピンボタンが表示され [`ListView`](xref:Xamarin.Forms.ListView) ます。

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`ListView`](xref:Xamarin.Forms.ListView)イベントを発生させて [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) 更新を開始し、 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) プロパティをに設定し `true` ます。 の内容を更新するために必要なすべてのコードは、 `ListView` イベントのイベントハンドラー、またはによって実行されるメソッドによって実行される必要があり `Refreshing` [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) ます。 が更新されたら `ListView` 、 `IsRefreshing` `false` [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) 更新が完了したことを示すために、プロパティをに設定するか、メソッドを呼び出す必要があります。

> [!NOTE]
> を定義するときに、コマンドのメソッドを指定して、 [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) `CanExecute` コマンドを有効または無効にすることができます。

## <a name="detect-scrolling"></a>スクロールの検出

[`ListView`](xref:Xamarin.Forms.ListView)`Scrolled`スクロールが発生したことを示すために発生するイベントを定義します。 次の XAML の例は、 `ListView` イベントのイベントハンドラーを設定するを示してい `Scrolled` ます。

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

同等の C# コードを次に示します。

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

このコード例では、イベント `OnListViewScrolled` の発生時にイベントハンドラーが実行され `Scrolled` ます。

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

イベント `OnListViewScrolled` ハンドラーは、 `ScrolledEventArgs` イベントに付随するオブジェクトの値を出力します。

## <a name="related-links"></a>関連リンク

- [ListView のインタラクティビティ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
