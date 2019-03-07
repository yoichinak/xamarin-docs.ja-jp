# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**ImageTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**ImageTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**の **[ImageTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Image`](xref:Xamarin.Forms.Image) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Image.Source`](xref:Xamarin.Forms.Image.Source) プロパティでは、URI を使用して、表示するイメージを指定します。 [`Image.Source`](xref:Xamarin.Forms.Image.Source) プロパティの型は [`ImageSource`](xref:Xamarin.Forms.ImageSource) です。これにより、イメージをファイル、URI、またはリソースから取得できます。 詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」 (Xamarin.Forms のイメージ) ガイドの「[Displaying images](~/xamarin-forms/user-interface/images.md#displaying-images)」 (イメージの表示) を参照してください。

    [`HeightRequest`](xref:Xamarin.Forms.VisualElement) プロパティでは、`Image` の高さをデバイスに依存しない単位で指定します。

    > [!NOTE]
    > この例では、[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティを設定する必要はありません。 既定では、[`Image`](xref:Xamarin.Forms.Image) でイメージの縦横比が維持されるためです。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android での Image のスクリーンショット](../images/create-image.png "イメージを表示する Image ビュー")](../images/create-image-large.png#lightbox "イメージを表示する Image ビュー")

    > [!NOTE]
    > [`Image`](xref:Xamarin.Forms.Image) ビューでは、ダウンロードしたイメージが 24 時間、自動的にキャッシュされます。 詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」 (Xamarin.Forms のイメージ) ガイドの「[Downloaded image caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)」 (ダウンロードしたイメージのキャッシュ) を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**ImageTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**ImageTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **[ImageTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Image`](xref:Xamarin.Forms.Image) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Image.Source`](xref:Xamarin.Forms.Image.Source) プロパティでは、URI を使用して、表示するイメージを指定します。 [`Image.Source`](xref:Xamarin.Forms.Image.Source) プロパティの型は [`ImageSource`](xref:Xamarin.Forms.ImageSource) です。これにより、イメージをファイル、URI、またはリソースから取得できます。 詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」 (Xamarin.Forms のイメージ) ガイドの「[Displaying images](~/xamarin-forms/user-interface/images.md#displaying-images)」 (イメージの表示) を参照してください。

    [`HeightRequest`](xref:Xamarin.Forms.VisualElement) プロパティでは、`Image` の高さをデバイスに依存しない単位で指定します。

    > [!NOTE]
    > この例では、[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティを設定する必要はありません。 既定では、[`Image`](xref:Xamarin.Forms.Image) でイメージの縦横比が維持されるためです。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android での Image のスクリーンショット](../images/create-image.png "イメージを表示する Image ビュー")](../images/create-image-large.png#lightbox "イメージを表示する Image ビュー")

    > [!NOTE]
    > [`Image`](xref:Xamarin.Forms.Image) ビューでは、ダウンロードしたイメージが 24 時間、自動的にキャッシュされます。 詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」 (Xamarin.Forms のイメージ) ガイドの「[Downloaded image caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)」 (ダウンロードしたイメージのキャッシュ) を参照してください。
