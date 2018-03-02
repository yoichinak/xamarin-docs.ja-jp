---
title: "ListView の対話機能"
description: "選択、スワイプして、削除、および更新するプルを実装することによって、ListView に対話機能を追加します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 74b275b77b6a70b3d074d68b14a97c73615753d2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="listview-interactivity"></a>ListView の対話機能

ListView では、次の方法で提示データとの対話をサポートします。

- [**選択 & タップ**](#selectiontaps) &ndash;タップし、項目の選択/deselections に応答します。 有効にするにまたは行の選択 (既定で有効になっている) を無効にします。
- [**コンテキストのアクション**](#Context_Actions) &ndash;スワイプして、削除する項目、たとえば、1 を公開して機能します。
- [**更新するプル**](#Pull_to_Refresh) &ndash;付属のユーザーが期待するネイティブの経験から更新するプル表現形式を実装します。

<a name="selectiontaps" />

## <a name="selection--taps"></a>選択 & タップ
`ListView` 一度に 1 つの項目の選択をサポートしています。 既定で選択しています。 2 つのイベントが発生した場合、ユーザーが項目をタップします。`ItemTapped`と`ItemSelected`です。 注こと、同じ項目を 2 回タップも複数は起動されません`ItemSelected`イベント、複数が起動されますが、`ItemTapped`イベント。 なお`ItemSelected`項目が選択されていない場合に呼び出されます。

注意してくださいを`ItemSelected`は項目が選択解除した場合とは、選択したときに呼び出されます。 つまり、null をチェックする必要があります`SelectedItem`で、`ItemSelected`使用する前に、イベント ハンドラー。

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>選択範囲を無効にします。

選択範囲を無効にする場合は、処理、`ItemSelected`イベントとセット、`SelectedItem`プロパティを null にします。

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

選択範囲を有効にします。

![](interactivity-images/selection-default.png "選択範囲が有効になっているを含む ListView")

Windows Phone など、いくつかのセルになお`SwitchCell`選択に応じて、表示状態を更新しません。

<a name="Context_Actions" />

## <a name="context-actions"></a>コンテキストのアクション
多くの場合、ユーザーが操作対象の項目を`ListView`です。 たとえば、メール アプリでの電子メールの一覧があるとします。 Ios、メッセージを削除するまでスワイプことができ、Windows Phone でメッセージを時間の長いキーを押してとできますそれを削除します。

![](interactivity-images/context-default.png "コンテキストのアクションを含む ListView")

C# と XAML では、コンテキストのアクションを実装できます。 下記で説明特定ガイド、両方が初めてみましょう両方のキーの実装の詳細を見ています。

使用してコンテキストのアクションが作成された`MenuItem`s。 Menuitem に対してタップ イベントが ListView ではない、MenuItem 自体によって発生します。 これは、ListView がセルではなく、イベントを生成、セルに対する tap イベントを処理する方法と異なります。 ListView がイベントを発生させて、ため、イベント ハンドラーにキー情報、項目が選択されているまたはタップといったが与えられます。

既定では、MenuItem に属しているセルを知る方法がありません。 `CommandParameter` 使用できるは`MenuItem`MenuItem の ViewCell の背後にあるオブジェクトなどのオブジェクトを格納します。 `CommandParameter` XAML と c# の両方で設定できます。

### <a name="c"></a>C#  

コンテキストのアクションは、いずれかで実装できます`Cell`サブクラス (グループ ヘッダーとして使用されていない限り) を作成して`MenuItem`に追加することと、`ContextActions`セルのコレクション。 次のものをコンテキストのアクションのプロパティを構成できます。

* **テキスト**&ndash;メニュー項目に表示される文字列。
* **クリックされた**&ndash;アイテムがクリックされたときに、イベント。
* **IsDestructive** &ndash; true の場合は (省略可能)、アイテムが表示される異なる方法で iOS でします。

ただし、1 つだけ必要がありますがある、セルに、複数のコンテキストのアクションを追加できます`IsDestructive`'éý'`true`です。 次のコード例は、コンテキストのアクションに追加する方法、 `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s 作成することも、XAML コレクションで宣言します。 次の XAML では、実装されている 2 つのコンテキストのアクションでカスタムのセルを示しています。

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
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

分離コード ファイルで、次を確認してください。、`Clicked`メソッドを実装します。

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>更新するプルします。
ユーザーは、データの一覧を取り出すと、その一覧は更新を予定になっています。 `ListView` この - の-すぐをサポートしています。 更新するプル機能を有効にするには、次のように設定します。 `IsPullToRefreshEnabled` true に設定します。

```csharp
listView.IsPullToRefreshEnabled = true;
```

ユーザーと更新にプルが抽出します。

![](interactivity-images/refresh-start.png "ListView プルで進行状況を更新するには")

ユーザーとプルは更新するには、プルがリリースされました。 これは、ユーザーがリストを更新している間には: ![ ] (interactivity-images/refresh-in-progress.png "ListView プル全体を更新するには")

Xamarin.Forms 1.4.3 時点でことに注意して、更新するプルは Windows Phone 8.1 でサポートされていません。 Windows phone 8 で更新するプル機能ではありませんネイティブ プラットフォーム、Xamarin.Forms で更新するプルの実装が提供されるようにします。 最後に、対応する Windows Phone では、リスト内のすべての要素はことができます (つまり、垂直方向のスクロールは必要ありません) 場合、画面に収まる場合そのプルの更新は機能しません。

ListView では、更新するプル イベントに応答することは、いくつかのイベントを公開します。

-  `RefreshCommand`呼び出されると、`Refreshing`というイベントです。 `IsRefreshing` 設定されます`true`です。
-  どのようなコードがコマンドまたはイベントで、リスト ビューの内容を更新するために必要なを実行する必要があります。
-  更新するときに完了したら、呼び出す`EndRefresh`設定または`IsRefreshing`に`false`したらリスト ビューを確認します。

`CanExecute`プロパティが守られていることができますコントロールを更新するプル コマンドを有効にするかどうか。



## <a name="related-links"></a>関連リンク

- [ListView の対話機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [1.4 のリリース ノート](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 リリース ノート](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
