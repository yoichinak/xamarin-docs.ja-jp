---
ms.openlocfilehash: 41c1c8ae97c62a3eb2a73681b215e7687d0e473c
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98558909"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) プロパティが `Single` に設定され、[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベント用のハンドラーが設定されるように [`CollectionView`](xref:Xamarin.Forms.CollectionView) 宣言を変更します。

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged" />
    ```

    このコード セットでは、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で 1 つの項目の選択が有効になり、[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベントが `OnSelectionChanged`という名前のイベント ハンドラーに設定されます。 イベント ハンドラーは次のステップで作成されます。

1. **ソリューション エクスプローラー** の **[CollectionViewTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnSelectionChanged` イベント ハンドラーをクラスに追加します。

    ```csharp
    void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        Monkey selectedItem = e.CurrentSelection[0] as Monkey;
    }
    ```

    [`CollectionView`](xref:Xamarin.Forms.CollectionView) 内の項目を選択すると、`OnSelectionChanged` メソッドを実行する [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベントが発生します。 メソッドに対する `sender` 引数は、イベントの発生を担当する `CollectionView` オブジェクトであり、`CollectionView` オブジェクトへのアクセスに使用できます。 `OnSelectionChanged` メソッドの [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) 引数によって、選択された項目が提供されます。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、項目の選択に応答する CollectionView のスクリーンショット](../images/item-selection.png "CollectionView の項目の選択")](../images/item-selection-large.png#lightbox "CollectionView の項目の選択")

    `OnSelectionChanged` イベント ハンドラーにブレークポイントを設定し、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で項目を選択します。 `selectedItem` 変数の値を調べて、選択した項目のデータが含まれていることを確認します。

    項目の選択の詳細については、「[Xamarin.Forms CollectionView の選択](~/xamarin-forms/user-interface/collectionview/selection.md)」を参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) プロパティが `Single` に設定され、[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベント用のハンドラーが設定されるように [`CollectionView`](xref:Xamarin.Forms.CollectionView) 宣言を変更します。

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged" />
    ```

    このコード セットでは、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で 1 つの項目の選択が有効になり、[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベントが `OnSelectionChanged`という名前のイベント ハンドラーに設定されます。 イベント ハンドラーは次のステップで作成されます。

1. **Solution Pad** の **[CollectionViewTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnSelectionChanged` イベント ハンドラーをクラスに追加します。

    ```csharp
    void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        Monkey selectedItem = e.CurrentSelection[0] as Monkey;
    }
    ```

    [`CollectionView`](xref:Xamarin.Forms.CollectionView) 内の項目を選択すると、`OnSelectionChanged` メソッドを実行する [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベントが発生します。 メソッドに対する `sender` 引数は、イベントの発生を担当する `CollectionView` オブジェクトであり、`CollectionView` オブジェクトへのアクセスに使用できます。 `OnSelectionChanged` メソッドの [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) 引数によって、選択された項目が提供されます。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、項目の選択に応答する CollectionView のスクリーンショット](../images/item-selection.png "CollectionView の項目の選択")](../images/item-selection-large.png#lightbox "CollectionView の項目の選択")

    `OnSelectionChanged` イベント ハンドラーにブレークポイントを設定し、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で項目を選択します。 `selectedItem` 変数の値を調べて、選択した項目のデータが含まれていることを確認します。

    項目の選択の詳細については、「[Xamarin.Forms CollectionView の選択](~/xamarin-forms/user-interface/collectionview/selection.md)」を参照してください。
