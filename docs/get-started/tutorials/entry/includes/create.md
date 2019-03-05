# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**EntryTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**EntryTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**の **EntryTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EntryTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry Placeholder="Enter text" />
        </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Entry`](xref:Xamarin.Forms.Entry) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) プロパティは、`Entry` が最初に表示されたときに表示されるプレースホルダー テキストを指定します。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での Entry のスクリーンショット](../images/create-entry.png "プレース ホルダー テキストを含む Entry")](../images/create-entry-large.png#lightbox "プレース ホルダー テキストを含む Entry")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**EntryTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**EntryTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **EntryTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EntryTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry Placeholder="Enter text" />
        </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Entry`](xref:Xamarin.Forms.Entry) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) プロパティは、`Entry` が最初に表示されたときに表示されるプレースホルダー テキストを指定します。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での Entry のスクリーンショット](../images/create-entry.png "プレース ホルダー テキストを含む Entry")](../images/create-entry-large.png#lightbox "プレース ホルダー テキストを含む Entry")
