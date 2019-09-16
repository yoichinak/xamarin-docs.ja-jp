---
ms.openlocfilehash: a537bdaf2015dd7d179545cb7587934818e960d7
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2019
ms.locfileid: "71004148"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントと [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントのハンドラーを設定するように [`ListView`](xref:Xamarin.Forms.ListView) 宣言を変更します。

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    このコードは、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントを `OnListViewItemSelected` という名前のイベント ハンドラーに設定し、[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントを `OnListViewItemTapped` という名前のイベント ハンドラーに設定します。 どちらのイベント ハンドラーも次の手順で作成します。

1. **ソリューション エクスプローラー**の **ListViewTutorial** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnListViewItemSelected` と `OnListViewItemTapped` のイベント ハンドラーをクラスに追加します。

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    [`ListView`](xref:Xamarin.Forms.ListView) 内の項目がタップされると、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントと[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントが発生し、それぞれ `OnListViewItemSelected` メソッドと `OnListItemTapped` メソッドを実行します。 両方のメソッドに対する `sender` 引数は、イベントの発生を担当する `ListView` オブジェクトであり、`ListView` オブジェクトへのアクセスに使用できます。 `OnListViewItemSelected` メソッド内の [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) 引数は選択した項目を提供し、`OnListViewItemTapped` メソッド内の [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) 引数はタップされた項目を提供します。

    > [!IMPORTANT]
    > [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントは、新しい項目が [`ListView`](xref:Xamarin.Forms.ListView) で選択されている場合にのみ発生します。 そのため、同じ項目を 2 回タップすると、2 つの [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントが発生しますが、発生するのは 1 つの `ItemSelected` イベントだけです。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、項目の選択とタップに応答する ListView のスクリーンショット](../images/item-selection.png "ListView 項目の選択")](../images/item-selection-large.png#lightbox "ListView 項目の選択")

    2 つのイベント ハンドラーにブレークポイントを設定し、[`ListView`](xref:Xamarin.Forms.ListView) 内の項目をタップします。 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントが新しい項目が [`ListView`](xref:Xamarin.Forms.ListView) で選択された場合にのみ発生するのに対し、[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントは項目がタップされるたびに発生することに注意してください。

    項目の選択とタップに関する詳細は、「[ListView Interactivity](~/xamarin-forms/user-interface/listview/interactivity.md)」 (ListView インタラクティビティ) ガイドの「[Selection & Taps](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and-taps)」 (選択とタップ) を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントと [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントのハンドラーを設定するように [`ListView`](xref:Xamarin.Forms.ListView) 宣言を変更します。

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    このコードは、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントを `OnListViewItemSelected` という名前のイベント ハンドラーに設定し、[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントを `OnListViewItemTapped` という名前のイベント ハンドラーに設定します。 どちらのイベント ハンドラーも次の手順で作成します。

1. **Solution Pad** の **ListViewTutorial** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnListViewItemSelected` と `OnListViewItemTapped` のイベント ハンドラーをクラスに追加します。

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    [`ListView`](xref:Xamarin.Forms.ListView) 内の項目がタップされると、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントと[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントが発生し、それぞれ `OnListViewItemSelected` メソッドと `OnListItemTapped` メソッドを実行します。 両方のメソッドに対する `sender` 引数は、イベントの発生を担当する `ListView` オブジェクトであり、`ListView` オブジェクトへのアクセスに使用できます。 `OnListViewItemSelected` メソッド内の [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) 引数は選択した項目を提供し、`OnListViewItemTapped` メソッド内の [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) 引数はタップされた項目を提供します。

    > [!IMPORTANT]
    > [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントは、新しい項目が [`ListView`](xref:Xamarin.Forms.ListView) で選択されている場合にのみ発生します。 そのため、同じ項目を 2 回タップすると、2 つの [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントが発生しますが、発生するのは 1 つの `ItemSelected` イベントだけです。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、項目の選択とタップに応答する ListView のスクリーンショット](../images/item-selection.png "ListView 項目の選択")](../images/item-selection-large.png#lightbox "ListView 項目の選択")

    2 つのイベント ハンドラーにブレークポイントを設定し、[`ListView`](xref:Xamarin.Forms.ListView) 内の項目をタップします。 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントが新しい項目が [`ListView`](xref:Xamarin.Forms.ListView) で選択された場合にのみ発生するのに対し、[`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントは項目がタップされるたびに発生することに注意してください。

    項目の選択とタップに関する詳細は、「選択とタップ」 (~/xamarin-forms/user-interface/listview/interactivity.md#selection-and) を参照してください。
