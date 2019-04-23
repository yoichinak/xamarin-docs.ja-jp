---
ms.openlocfilehash: 846ff203a26f38fdaabe5a5dd6eff1c6a6b39b08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61343466"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) イベントと [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントのハンドラーを設定するように [`Entry`](xref:Xamarin.Forms.Entry) 宣言を変更します。

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    このコードでは、[`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) イベントを `OnEntryTextChanged` という名前のイベント ハンドラーに設定し、[`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントを `OnEntryCompleted` という名前のイベント ハンドラーに設定します。 両方のイベント ハンドラーは次の手順で作成します。

1. **ソリューション エクスプローラー**の **LabelTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnEntryTextChanged` と `OnEntryCompleted` のイベント ハンドラーをクラスに追加します。

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
    > [`Entry`](xref:Xamarin.Forms.Entry) に入力されたテキストはすべて [`Text`](xref:Xamarin.Forms.Entry.Text) プロパティに格納されます。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でのテキストを含むエントリのスクリーン ショット](../images/text-changes.png "テキストを含むエントリ")](../images/text-changes-large.png#lightbox "テキストを含むエントリ")

    2 つのイベント ハンドラーにブレークポイントを設定し、[`Entry`](xref:Xamarin.Forms.Entry) にテキストを入力して、[`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) と [`Completed`](xref:Xamarin.Forms.Entry.Completed) のイベントの発生を確認します。

    [`Entry`](xref:Xamarin.Forms.Entry) イベントの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」ガイドの「[Events and Interactivity](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity)」(イベントとインタラクティビティ) をご覧ください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) イベントと [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントのハンドラーを設定するように [`Entry`](xref:Xamarin.Forms.Entry) 宣言を変更します。

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    このコードでは、[`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) イベントを `OnEntryTextChanged` という名前のイベント ハンドラーに設定し、[`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントを `OnEntryCompleted` という名前のイベント ハンドラーに設定します。 両方のイベント ハンドラーは次の手順で作成します。

1. **Solution Pad** の **LabelTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnEntryTextChanged` と `OnEntryCompleted` のイベント ハンドラーをクラスに追加します。

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
    > [`Entry`](xref:Xamarin.Forms.Entry) に入力されたテキストはすべて [`Text`](xref:Xamarin.Forms.Entry.Text) プロパティに格納されます。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でのテキストを含むエントリのスクリーン ショット](../images/text-changes.png "テキストを含むエントリ")](../images/text-changes-large.png#lightbox "テキストを含むエントリ")

    2 つのイベント ハンドラーにブレークポイントを設定し、[`Entry`](xref:Xamarin.Forms.Entry) にテキストを入力して、[`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) と [`Completed`](xref:Xamarin.Forms.Entry.Completed) のイベントの発生を確認します。

    [`Entry`](xref:Xamarin.Forms.Entry) イベントの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」ガイドの「[Events and Interactivity](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity)」(イベントとインタラクティビティ) をご覧ください。
