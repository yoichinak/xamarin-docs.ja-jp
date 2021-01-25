---
ms.openlocfilehash: d32dd7d641861f1a95ff35dbbf89a7671594e873
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634834"
---
Xamarin.Forms には、ユーザーにアラートを表示する、または簡単な質問をする、アラートと呼ばれる、モーダル ポップアップがあります。 この演習では、[`Page`](xref:Xamarin.Forms.Page) クラスから [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドを使用して、ユーザーにアラートを表示したり、簡単な質問をします。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

このチュートリアルを完了するには、 **.NET によるモバイル開発** ワークロードがインストールされた、Visual Studio 2019 (最新リリース) が必要です。 さらに、iOS でチュートリアル アプリケーションを構築するには、ペアリング済みの Mac が必要になります。 Xamarin プラットフォームのインストールについては、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

1. Visual Studio を起動し、**PopupsTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**PopupsTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー** の **[PopupsTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 2 つの [`Button`](xref:Xamarin.Forms.Button) オブジェクトから構成される、ページのユーザー インターフェイスを宣言によって定義します。 [`Button.Text`](xref:Xamarin.Forms.Button.Text) プロパティでは、各 `Button` に表示されるテキストを指定し、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントは、次の手順で作成されるイベント ハンドラーに設定されます。

1. **ソリューション エクスプローラー** の **[PopupsTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnDisplayAlertButtonClicked` と `OnDisplayAlertQuestionButtonClicked` のイベント ハンドラーをクラスに追加します。

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) がタップされると、それぞれのイベント ハンドラー メソッドが実行されます。 `OnDisplayAlertButtonClicked` メソッドでは、単一のキャンセル ボタンがあるモーダル アラートを表示するために、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドを呼び出します。 アラートが閉じられたら、ユーザーはアプリケーションの操作を続行できます。

    `OnDisplayAlertQuestionButtonClicked` メソッドでは、承認ボタンとキャンセル ボタンがあるモダール アラートを表示するために、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドのオーバーロードを呼び出します。 ユーザーがいずれかのボタンを選択した後、その選択が `boolean` として返されます。

    > [!IMPORTANT]
    > [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドは非同期であり、`await` キーワードで常に待機される必要があります。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 次に、最初の [`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS および Android でのアラートのスクリーンショット](../images/alert.png "アラート:")](../images/alert-large.png#lightbox "アラート:")

    アラートを閉じた後、2 番目の [`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS および Android での、質問をするアラートのスクリーンショット](../images/alert-question.png "質問をするアラート")](../images/alert-question-large.png#lightbox "質問をするアラート")

    質問への回答を選択した後、回答が Visual Studio の **[出力]** ウィンドウに出力されることを確認します。 このウィンドウが表示されていない場合は、 **[表示] > [出力]** メニュー オプションを選択すると表示されます。

    Visual Studio で、アプリケーションを停止します。

    アラートの表示について詳しくは、「[Display Pop-ups (ポップアップの表示)](~/xamarin-forms/user-interface/pop-ups.md)」ガイドの「[Display an alert (アラートを表示する)](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert)」をご覧ください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

このチュートリアルを完了するには、iOS と Android のプラットフォームのサポートがインストールされた Visual Studio for Mac (最新リリース) が必要です。 さらに、Xcode (最新リリース) も必要になります。 Xamarin プラットフォームのインストールについて詳しくは、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

1. Visual Studio for Mac を起動し、**PopupsTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**PopupsTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **[PopupsTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 2 つの [`Button`](xref:Xamarin.Forms.Button) オブジェクトから構成される、ページのユーザー インターフェイスを宣言によって定義します。 [`Button.Text`](xref:Xamarin.Forms.Button.Text) プロパティでは、各 `Button` に表示されるテキストを指定し、[`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントは、次の手順で作成されるイベント ハンドラーに設定されます。

1. **Solution Pad** の **[PopupsTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnDisplayAlertButtonClicked` と `OnDisplayAlertQuestionButtonClicked` のイベント ハンドラーをクラスに追加します。

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) がタップされると、それぞれのイベント ハンドラー メソッドが実行されます。 `OnDisplayAlertButtonClicked` メソッドでは、単一のキャンセル ボタンがあるモーダル アラートを表示するために、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドを呼び出します。 アラートが閉じられたら、ユーザーはアプリケーションの操作を続行できます。

    `OnDisplayAlertQuestionButtonClicked` メソッドでは、承認ボタンとキャンセル ボタンがあるモダール アラートを表示するために、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドのオーバーロードを呼び出します。 ユーザーがいずれかのボタンを選択した後、その選択が `boolean` として返されます。

    > [!IMPORTANT]
    > [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドは非同期であり、`await` キーワードで常に待機される必要があります。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 次に、最初の [`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS および Android でのアラートのスクリーンショット](../images/alert.png "アラート:")](../images/alert-large.png#lightbox "アラート:")

    アラートを閉じた後、2 番目の [`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS および Android での、質問をするアラートのスクリーンショット](../images/alert-question.png "質問をするアラート")](../images/alert-question-large.png#lightbox "質問をするアラート")

    質問への回答を選択した後、回答が Visual Studio for Mac の **[アプリケーション出力]** ウィンドウに出力されることを確認します。 このウィンドウが表示されていない場合は、 **[表示] > [その他のウィンドウ] > [アプリケーション出力]** メニュー オプションを選択すると表示されます。

    Visual Studio for Mac で、アプリケーションを停止します。

    アラートの表示について詳しくは、「[Display Pop-ups (ポップアップの表示)](~/xamarin-forms/user-interface/pop-ups.md)」ガイドの「[Display an alert (アラートを表示する)](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert)」をご覧ください。
