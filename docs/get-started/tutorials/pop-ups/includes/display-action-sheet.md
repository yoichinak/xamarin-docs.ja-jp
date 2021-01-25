---
ms.openlocfilehash: 9ff398b53e88f38589733b1caac623e564e1ed39
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634833"
---

Xamarin.Forms には、アクション シートとして知られるモーダル ポップアップが用意されており、タスクの完了までユーザーをガイドするのに使用できます。 この演習では、[`Page`](xref:Xamarin.Forms.Page) クラスから [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) メソッドを使用して、タスクの完了までユーザーをガイドするアクション シートを表示します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、アクション シートを表示する新しい [`Button`](xref:Xamarin.Forms.Button) 宣言を追加します。

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

     [`Button.Text`](xref:Xamarin.Forms.Button.Text) プロパティは、`Button` に表示するテキストを指定します。 さらに、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントは、次の手順で作成する `OnDisplayActionSheetButtonClicked` という名前のイベント ハンドラーに設定されます。

1. **ソリューション エクスプローラー** の **PopupsTutorial** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnDisplayActionSheetButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) をタップすると、`OnDisplayActionSheetButtonClicked` メソッドが実行されます。 このメソッドは、[`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) メソッドを呼び出し、タスクを続行するための一連の代替方法をユーザーに提供します。 ユーザーがいずれかの代替方法を選択すると、その選択が `string` として返されます。

    > [!IMPORTANT]
    > [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) メソッドは非同期で、`await` キーワードで常に待機される必要があります。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 次に、[`ContentPage`](xref:Xamarin.Forms.ContentPage) に追加した [`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS および Android 上のアクション シートのスクリーンショット](../images/actionsheet.png "ユーザーにタスクを案内する Actionsheet")](../images/actionsheet-large.png#lightbox "ユーザーにタスクを案内する Actionsheet")

    アクション シート ダイアログで代替方法を選択した後、その選択が Visual Studio の **[出力]** ウィンドウに出力されることを確認します。 このウィンドウが表示されていない場合は、 **[表示] > [出力]** メニュー オプションを選択すると表示されます。

    Visual Studio で、アプリケーションを停止します。

    アクション シートの表示に関する詳細は、「[Display Pop-ups (ポップアップの表示)](~/xamarin-forms/user-interface/pop-ups.md)」ガイドの「[Guide users through tasks (タスクを通じたユーザーのガイド)](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks)」をご覧ください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、アクション シートを表示する新しい [`Button`](xref:Xamarin.Forms.Button) 宣言を追加します。

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

    [`Button.Text`](xref:Xamarin.Forms.Button.Text) プロパティは、`Button` に表示するテキストを指定します。 さらに、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントは、次の手順で作成する `OnDisplayActionSheetButtonClicked` という名前のイベント ハンドラーに設定されます。

1. **Solution Pad** の **PopupsTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnDisplayActionSheetButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) をタップすると、`OnDisplayActionSheetButtonClicked` メソッドが実行されます。 このメソッドは、[`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) メソッドを呼び出し、タスクを続行するための一連の代替方法をユーザーに提供します。 ユーザーがいずれかの代替方法を選択すると、その選択が `string` として返されます。

    > [!IMPORTANT]
    > [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) メソッドは非同期であり、`await` キーワードで常に待機される必要があります。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 次に、[`ContentPage`](xref:Xamarin.Forms.ContentPage) に追加した [`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS および Android 上のアクション シートのスクリーンショット](../images/actionsheet.png "ユーザーにタスクを案内する Actionsheet")](../images/actionsheet-large.png#lightbox "ユーザーにタスクを案内する Actionsheet")

    アクション シート ダイアログで代替方法を選択した後、その選択が Visual Studio for Mac の **[出力]** ウィンドウに出力されることを確認します。 このウィンドウが表示されていない場合は、 **[表示] > [その他のウィンドウ] > [アプリケーション出力]** メニュー オプションを選択すると表示されます。

    Visual Studio for Mac で、アプリケーションを停止します。

    アクション シートの表示に関する詳細は、「[Display Pop-ups (ポップアップの表示)](~/xamarin-forms/user-interface/pop-ups.md)」ガイドの「[Guide users through tasks (タスクを通じたユーザーのガイド)](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks)」をご覧ください。
