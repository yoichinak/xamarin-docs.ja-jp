# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**StackLayoutTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**StackLayoutTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**の **[StackLayoutTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 3 つの [`Label`](xref:Xamarin.Forms.Label) インスタンスから構成される、ページのユーザー インターフェイスを宣言によって定義します。 [`StackLayout`](xref:Xamarin.Forms.StackLayout) では、単一行にその子ビュー (`Label` インスタンス) を配置します。既定では、垂直に配置されます。 さらに、[`Margin`](xref:Xamarin.Forms.View.Margin) プロパティは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) 内の `StackLayout` のレンダリング位置を示します。

    > [!NOTE]
    > [`Margin`](xref:Xamarin.Forms.View.Margin) プロパティに加え、[`Padding`](xref:Xamarin.Forms.Layout.Padding) および [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) プロパティを [`StackLayout`](xref:Xamarin.Forms.StackLayout) に設定することもできます。 [`Padding`](xref:Xamarin.Forms.Layout.Padding) プロパティの値では、`StackLayout` のビュー間の距離を指定し、[`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) プロパティの値では、`StackLayout` の各子要素の間のスペース量を指定します。 詳細については「[Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」 (余白とスペース) を参照してください。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での StackLayout の子ビューのスクリーンショット](../images/create-stacklayout.png "Label インスタンスを含む StackLayout")](../images/create-stacklayout-large.png#lightbox "Label インスタンスを含む StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) の詳細については、「[Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)」 (Xamarin.Forms の StackLayout) を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**StackLayoutTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**StackLayoutTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **StackLayoutTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 3 つの [`Label`](xref:Xamarin.Forms.Label) インスタンスから構成される、ページのユーザー インターフェイスを宣言によって定義します。 [`StackLayout`](xref:Xamarin.Forms.StackLayout) では、単一行にその子ビュー (`Label` インスタンス) を配置します。既定では、垂直に配置されます。 さらに、[`Margin`](xref:Xamarin.Forms.View.Margin) プロパティは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) 内の `StackLayout` のレンダリング位置を示します。

    > [!NOTE]
    > [`Margin`](xref:Xamarin.Forms.View.Margin) プロパティに加え、[`Padding`](xref:Xamarin.Forms.Layout.Padding) および [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) プロパティを [`StackLayout`](xref:Xamarin.Forms.StackLayout) に設定することもできます。 [`Padding`](xref:Xamarin.Forms.Layout.Padding) プロパティの値では、`StackLayout` のビュー間の距離を指定し、[`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) プロパティの値では、`StackLayout` の各子要素の間のスペース量を指定します。 詳細については「[Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」 (余白とスペース) を参照してください。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での StackLayout の子ビューのスクリーンショット](../images/create-stacklayout.png "Label インスタンスを含む StackLayout")](../images/create-stacklayout-large.png#lightbox "Label インスタンスを含む StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) の詳細については、「[Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)」 (Xamarin.Forms の StackLayout) を参照してください。
