---
ms.openlocfilehash: d9834c2d2974787de989b640449fddae6c1ce39b
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919348"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) イベントと [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントのハンドラーを設定するように [`Entry`](xref:Xamarin.Forms.Entry) 宣言を変更します。

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    このコードは、[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) イベントを `OnEntryTextChanged` という名前のイベント ハンドラーに設定し、[`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントを `OnEntryCompleted` という名前のイベント ハンドラーに設定します。 どちらのイベント ハンドラーも次の手順で作成されます。

1. **ソリューション エクスプローラー**の **EntryTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnEntryTextChanged` と `OnEntryCompleted` のイベント ハンドラーをクラスに追加します。

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    [`Entry`](xref:Xamarin.Forms.Entry) のテキストが変更されると、`OnEntryTextChanged` メソッドが実行されます。 `sender` 引数は、`TextChanged` イベントの発生を担当する `Entry` オブジェクトであり、`Entry` オブジェクトへのアクセスに使用できます。 [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 引数は、テキストの変更前と変更後の古いテキスト値と新しいテキスト値を提供します。

    Return キーで [`Entry`](xref:Xamarin.Forms.Entry) のテキストを確定すると、`OnEntryCompleted` メソッドが実行されます。 `sender` 引数は、`TextChanged` イベントの発生を担当する `Entry` オブジェクトであり、`Entry` オブジェクトへのアクセスに使用できます。

    > [!IMPORTANT]
    > [`Entry`](xref:Xamarin.Forms.Entry) に入力されたテキストはすべて [`Text`](xref:Xamarin.Forms.InputView.Text) プロパティに格納されます。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でのテキストを含む Entry のスクリーンショット](../images/text-changes.png "テキストを含む Entry")](../images/text-changes-large.png#lightbox "テキストを含む Entry")

    2 つのイベント ハンドラーにブレークポイントを設定し、[`Entry`](xref:Xamarin.Forms.Entry) にテキストを入力して、[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) と [`Completed`](xref:Xamarin.Forms.Entry.Completed) のイベントの発生を確認します。

    [`Entry`](xref:Xamarin.Forms.Entry) イベントの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」ガイドの「[Events and Interactivity](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity)」(イベントとインタラクティビティ) をご覧ください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) イベントと [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントのハンドラーを設定するように [`Entry`](xref:Xamarin.Forms.Entry) 宣言を変更します。

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    このコードは、[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) イベントを `OnEntryTextChanged` という名前のイベント ハンドラーに設定し、[`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントを `OnEntryCompleted` という名前のイベント ハンドラーに設定します。 どちらのイベント ハンドラーも次の手順で作成されます。

1. **Solution Pad** の **EntryTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnEntryTextChanged` と `OnEntryCompleted` のイベント ハンドラーをクラスに追加します。

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    [`Entry`](xref:Xamarin.Forms.Entry) のテキストが変更されると、`OnEntryTextChanged` メソッドが実行されます。 `sender` 引数は、`TextChanged` イベントの発生を担当する `Entry` オブジェクトであり、`Entry` オブジェクトへのアクセスに使用できます。 [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 引数は、テキストの変更前と変更後の古いテキスト値と新しいテキスト値を提供します。

    Return キーで [`Entry`](xref:Xamarin.Forms.Entry) のテキストを確定すると、`OnEntryCompleted` メソッドが実行されます。 `sender` 引数は、`TextChanged` イベントの発生を担当する `Entry` オブジェクトであり、`Entry` オブジェクトへのアクセスに使用できます。

    > [!IMPORTANT]
    > [`Entry`](xref:Xamarin.Forms.Entry) に入力されたテキストはすべて [`Text`](xref:Xamarin.Forms.InputView.Text) プロパティに格納されます。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でのテキストを含む Entry のスクリーンショット](../images/text-changes.png "テキストを含む Entry")](../images/text-changes-large.png#lightbox "テキストを含む Entry")

    2 つのイベント ハンドラーにブレークポイントを設定し、[`Entry`](xref:Xamarin.Forms.Entry) にテキストを入力して、[`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) と [`Completed`](xref:Xamarin.Forms.Entry.Completed) のイベントの発生を確認します。

    [`Entry`](xref:Xamarin.Forms.Entry) イベントの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」ガイドの「[Events and Interactivity](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity)」(イベントとインタラクティビティ) をご覧ください。
