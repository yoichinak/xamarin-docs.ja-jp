# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベント用のハンドラーを設定するように [`Button`](xref:Xamarin.Forms.Button) 宣言を変更します。

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    このコードは、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを次の手順で作成する `OnButtonClicked` という名前のイベント ハンドラーに設定します。

1. **ソリューション エクスプローラー**の **ButtonTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) をタップすると、`OnButtonClicked` メソッドが実行されます。 `sender` 引数は、`Clicked` イベントの発生を担当する `Button` オブジェクトであり、`Button` オブジェクトへのアクセスに使用できます。 このイベント ハンドラーは、`Button` によって表示されるテキストを更新します。

    > [!NOTE]
    > `Clicked` イベントのほかに、`Button` は [`Pressed`](xref:Xamarin.Forms.Button.Pressed) イベントと [`Released`](xref:Xamarin.Forms.Button.Released) イベントも定義します。 詳細については、「[Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md)」ガイドの「[Pressing and releasing the button](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button)」 (ボタンを押して放す) を参照してください。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Button`](xref:Xamarin.Forms.Button) をクリックして、表示されるテキストが変わっていることを確認します。

    [![iOS と Android で、クリックを受信すると変わる Button テキストのスクリーンショット](../images/handle-button-click.png "ボタン クリックの処理")](../images/handle-button-click-large.png#lightbox "ボタン クリックの処理")

    ボタン クリックの処理の詳細については、「[Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md)」ガイドの「[Handling button clicks](~/xamarin-forms/user-interface/button.md#handling-button-clicks)」 (ボタン クリックの処理) を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベント用のハンドラーを設定するように [`Button`](xref:Xamarin.Forms.Button) 宣言を変更します。

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    このコードは、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを次の手順で作成する `OnButtonClicked` という名前のイベント ハンドラーに設定します。

1. **Solution Pad** の **ButtonTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) をタップすると、`OnButtonClicked` メソッドが実行されます。 `sender` 引数は、`Clicked` イベントの発生を担当する `Button` オブジェクトであり、`Button` オブジェクトへのアクセスに使用できます。 このイベント ハンドラーは、`Button` によって表示されるテキストを更新します。

    > [!NOTE]
    > `Clicked` イベントのほかに、`Button` は [`Pressed`](xref:Xamarin.Forms.Button.Pressed) イベントと [`Released`](xref:Xamarin.Forms.Button.Released) イベントも定義します。 詳細については、「[Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md)」ガイドの「[Pressing and releasing the button](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button)」 (ボタンを押して放す) を参照してください。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Button`](xref:Xamarin.Forms.Button) をクリックして、表示されるテキストが変わっていることを確認します。

    [![iOS と Android で、クリックを受信すると変わる Button テキストのスクリーンショット](../images/handle-button-click.png "ボタン クリックの処理")](../images/handle-button-click-large.png#lightbox "ボタン クリックの処理")

    ボタン クリックの処理の詳細については、「[Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md)」ガイドの「[Handling button clicks](~/xamarin-forms/user-interface/button.md#handling-button-clicks)」 (ボタン クリックの処理) を参照してください。
