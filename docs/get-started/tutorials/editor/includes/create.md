# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**EditorTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**EditorTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**の **EditorTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Editor`](xref:Xamarin.Forms.Editor) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) プロパティは、`Editor` が最初に表示されたときに表示されるプレースホルダー テキストを指定します。 さらに、[`HeightRequest`](xref:Xamarin.Forms.VisualElement) プロパティは、`Editor` の高さをデバイスに依存しない単位で指定します。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での Editor のスクリーンショット](../images/create-editor.png "プレース ホルダー テキストを含む Editor")](../images/create-editor-large.png#lightbox "プレース ホルダー テキストを含む Editor")

    > [!NOTE]
    > Android では [`Editor`](xref:Xamarin.Forms.Editor) の高さが示されますが、iOS では示されません。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**EditorTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**EditorTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **EditorTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Editor`](xref:Xamarin.Forms.Editor) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) プロパティは、`Editor` が最初に表示されたときに表示されるプレースホルダー テキストを指定します。 さらに、[`HeightRequest`](xref:Xamarin.Forms.VisualElement) プロパティは、`Editor` の高さをデバイスに依存しない単位で指定します。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での Editor のスクリーンショット](../images/create-editor.png "プレース ホルダー テキストを含む Editor")](../images/create-editor-large.png#lightbox "プレース ホルダー テキストを含む Editor")

    > [!NOTE]
    > Android では [`Editor`](xref:Xamarin.Forms.Editor) の高さが示されますが、iOS では示されません。
