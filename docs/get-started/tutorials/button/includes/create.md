# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**ButtonTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**ButtonTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**の **[ButtonTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>    
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Button`](xref:Xamarin.Forms.Button) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Button.Text`](xref:Xamarin.Forms.Button.Text) プロパティでは、`Button` に表示するテキストを指定します。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android での Button のスクリーンショット](../images/create-button.png "テキストを含む Button")](../images/create-button-large.png#lightbox "テキストを含む Button")

    既定では、[`Button`](xref:Xamarin.Forms.Button) によって、許可されるすべてのスペース、この場合はその親 ([`StackLayout`](xref:Xamarin.Forms.StackLayout)) の幅全体が占有される傾向があることに注意してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**ButtonTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**ButtonTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **[ButtonTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Button`](xref:Xamarin.Forms.Button) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Button.Text`](xref:Xamarin.Forms.Button.Text) プロパティでは、`Button` に表示するテキストを指定します。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android での Button のスクリーンショット](../images/create-button.png "テキストを含む Button")](../images/create-button-large.png#lightbox "テキストを含む Button")

    既定では、[`Button`](xref:Xamarin.Forms.Button) によって、許可されるすべてのスペース、この場合はその親 ([`StackLayout`](xref:Xamarin.Forms.StackLayout)) の幅全体が占有される傾向があることに注意してください。
