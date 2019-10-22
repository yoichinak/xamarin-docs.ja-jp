---
title: ListView の対話機能
description: この記事では、選択、コンテキストアクション、およびプルから更新を実装することで、Xamarin. フォーム ListView にインタラクティビティを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: aa717792bdaefe24d957c9781934933b67aaf92b
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696869"
---
# <a name="listview-interactivity"></a>ListView の対話機能

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Xamarin [`ListView`](xref:Xamarin.Forms.ListView)クラスは、表示されるデータとのユーザー操作をサポートします。

## <a name="selection-and-taps"></a>選択とタップ

[@No__t_1](xref:Xamarin.Forms.ListView)選択モードは、 [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode)プロパティを[`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode)列挙体の値に設定することによって制御されます。

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)は、選択した項目を強調表示して、1つの項目を選択できることを示します。 これは既定値です。
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)は、項目を選択できないことを示します。

ユーザーが項目をタップすると、次の2つのイベントが発生します。

- 新しい項目が選択されたときに[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)発生します。
- 項目がタップされると[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)呼び出されます。

同じ項目を2回タップすると、2つの[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)イベントが発生しますが、1つの[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントのみが発生します。

> [!NOTE]
> [@No__t_3](xref:Xamarin.Forms.ListView.ItemTapped)イベントのイベント引数を格納している[`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)クラスには、 [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group)プロパティと[`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item)プロパティ、およびタップされた項目の[`ItemIndex`](xref:Xamarin.Forms.ListView)内のインデックスを表す値を持つ 0 プロパティがあります。 同様に、 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントのイベント引数を含む[`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)クラスには、 [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem)プロパティ、および選択された項目の `ListView` 内のインデックスを値とする `SelectedItemIndex` プロパティがあります。

[@No__t_1](xref:Xamarin.Forms.ListView.SelectionMode)プロパティが[`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)に設定されている場合、 [`ListView`](xref:Xamarin.Forms.ListView)内の項目を選択して、 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントと[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)イベントが発生し、 [`SelectionMode`1](xref:Xamarin.Forms.ListView.SelectedItem)プロパティが選択したの値に設定されます。品.

[@No__t_1](xref:Xamarin.Forms.ListView.SelectionMode)プロパティが[`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)に設定されている場合、 [`ListView`](xref:Xamarin.Forms.ListView)内の項目を選択することはできません。また、 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)イベントは発生せず、 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)プロパティは `SelectionMode`0 のままになります。 ただし、 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)イベントは発生しますが、タップしたときにタップされた項目が簡潔に強調表示されます。

項目が選択されていて、 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode)プロパティが[`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)から[`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)に変更されると、 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)プロパティが `null` に設定され[、0 のイベントが](xref:Xamarin.Forms.ListView.ItemSelected)1 の項目と共に発生します。

次のスクリーンショットは、既定の選択モードの[`ListView`](xref:Xamarin.Forms.ListView)を示しています。

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>選択の無効化

[@No__t_1](xref:Xamarin.Forms.ListView)選択を無効にするには、 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode)プロパティを[`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)に設定します。

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>コンテキスト アクション

多くの場合、ユーザーは `ListView` の項目に対してアクションを実行する必要があります。 たとえば、メールアプリの電子メールの一覧を考えてみましょう。 IOS では、スワイプしてメッセージを削除することができます。

![](interactivity-images/context-default.png "ListView with Context Actions")

コンテキストアクションは、XAML およびC# XAML で実装できます。 以下では、その両方に関する特定のガイドを紹介しますが、まず、その両方についていくつかの重要な実装の詳細を見てみましょう。

コンテキストアクションは `MenuItem` 要素を使用して作成されます。 @No__t_0 オブジェクトの [イベント] をタップすると、`ListView` ではなく `MenuItem` 自体によって発生します。 これは、セルに対して tap イベントを処理する方法とは異なり、`ListView` はセルではなくイベントを発生させます。 @No__t_0 によってイベントが発生しているため、そのイベントハンドラーには、選択またはタップされた項目などのキー情報が与えられます。

既定では、`MenuItem` には、所属するセルを認識する方法がありません。 @No__t_0 プロパティは、`MenuItem` の `ViewCell` の背後にあるオブジェクトなどのオブジェクトを格納するために `MenuItem` で使用できます。 @No__t_0 プロパティは、XAML とC#の両方で設定できます。

### <a name="xaml"></a>XAML

`MenuItem` 要素は、XAML コレクション内に作成できます。 次の XAML は、2つのコンテキストアクションが実装されたカスタムセルを示しています。

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

分離コードファイルで、`Clicked` メソッドが実装されていることを確認します。

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
> Android 用の `NavigationPageRenderer` には、カスタム `Drawable` からアイコンを読み込むために使用できる、オーバーライド可能な `UpdateMenuItemIcon` メソッドがあります。 このオーバーライドにより、Android 上の `MenuItem` インスタンスのアイコンとして SVG イメージを使用できるようになります。

### <a name="code"></a>コード

コンテキストアクションは、(グループヘッダーとして使用されていない限り) 任意の `Cell` サブクラスに実装できます。 `MenuItem` インスタンスを作成して、それをセルの `ContextActions` コレクションに追加します。 コンテキストアクションに対して次のプロパティを構成できます。

- メニュー項目に表示される文字列 &ndash;**テキスト**。
- 項目がクリックされたときにイベントを &ndash;**クリック**します。
- **Isdestructive** &ndash; (省略可能) true の場合、項目は iOS で異なる方法で表示されます。

複数のコンテキストアクションをセルに追加することはできますが、`true` に設定できるのは1つだけ `IsDestructive` 必要があります。 次のコードは、コンテキストアクションを `ViewCell` に追加する方法を示しています。

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

ユーザーは、データの一覧を取得して一覧を更新することを期待しています。 [@No__t_1](xref:Xamarin.Forms.ListView)コントロールでは、この機能がすぐにサポートされます。 プルから更新機能を有効にするには、 [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled)を `true` に設定します。

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

これに相当する C# コードを次に示します。

```csharp
listView.IsPullToRefreshEnabled = true;
```

更新中にスピンボタンが表示されます。これは既定では黒です。 ただし、`RefreshControlColor` プロパティを[`Color`](xref:Xamarin.Forms.Color)に設定することにより、IOS および Android ではスピンボタンの色を変更できます。

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

これに相当する C# コードを次に示します。

```csharp
listView.RefreshControlColor = Color.Red;
```

次のスクリーンショットは、ユーザーがプルしているときのプルツーリフレッシュを示しています。

![](interactivity-images/refresh-start.png "ListView Pull to Refresh In-Progress")

次のスクリーンショットは、ユーザーがプルを解放してから、 [`ListView`](xref:Xamarin.Forms.ListView)の更新中にスピンボタンが表示されるようになった後のプル更新を示しています。

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing)イベントを発生させて更新を開始する[`ListView`](xref:Xamarin.Forms.ListView) 、 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing)プロパティが `true` に設定されます。 @No__t_0 の内容を更新するために必要なすべてのコードは、`Refreshing` イベントのイベントハンドラー、または[`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)によって実行されるメソッドによって実行される必要があります。 @No__t_0 が更新されたら、更新が完了したことを示すために、`IsRefreshing` プロパティを `false` に設定するか、 [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh)メソッドを呼び出す必要があります。

> [!NOTE]
> [@No__t_1](xref:Xamarin.Forms.ListView.RefreshCommand)を定義するときに、コマンドの `CanExecute` メソッドを指定して、コマンドを有効または無効にすることができます。

## <a name="detect-scrolling"></a>スクロールの検出

[`ListView`](xref:Xamarin.Forms.ListView)は、スクロールが発生したことを示すために発生する `Scrolled` イベントを定義します。 次の XAML の例は、`Scrolled` イベントのイベントハンドラーを設定する `ListView` を示しています。

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

これに相当する C# コードを次に示します。

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

このコード例では、`Scrolled` イベントが発生すると、`OnListViewScrolled` イベントハンドラーが実行されます。

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

@No__t_0 イベントハンドラーは、イベントに付随する `ScrolledEventArgs` オブジェクトの値を出力します。

## <a name="related-links"></a>関連リンク

- [ListView のインタラクティビティ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
