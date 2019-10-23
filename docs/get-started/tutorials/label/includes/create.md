---
ms.openlocfilehash: 9d78f0e9bf3b8d6d227f76b2729be849e5cdd3a6
ms.sourcegitcommit: 5efbf5ab53532b3a74c80129ff4e0ca84b476d21
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72678725"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

このチュートリアルを完了するには、 **.NET によるモバイル開発**ワークロードがインストールされた、Visual Studio 2019 (最新リリース) が必要です。 さらに、iOS でチュートリアル アプリケーションを構築するには、ペアリング済みの Mac が必要になります。 Xamarin プラットフォームのインストールについては、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

1. Visual Studio を起動し、**LabelTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**LabelTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**の **LabelTutorial** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LabelTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
            <Label Text="Welcome to Xamarin.Forms!"
                   HorizontalOptions="Center" />
       </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Label`](xref:Xamarin.Forms.Label) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Label.Text`](xref:Xamarin.Forms.Button.Text) プロパティは表示されるテキストを指定し、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティは `Label` が水平方向の中央に配置されるように指定します。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android 上の中央揃えのラベルのスクリーンショット](../images/create-label.png "中央揃えのラベル")](../images/create-label-large.png#lightbox "中央揃えのラベル")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

このチュートリアルを完了するには、iOS と Android のプラットフォームのサポートがインストールされた Visual Studio for Mac (最新リリース) が必要です。 さらに、Xcode (最新リリース) も必要になります。 Xamarin プラットフォームのインストールについて詳しくは、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

1. Visual Studio for Mac を起動し、**LabelTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**LabelTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **LabelTutorial** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LabelTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
            <Label Text="Welcome to Xamarin.Forms!"
                   HorizontalOptions="Center" />
       </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Label`](xref:Xamarin.Forms.Label) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Label.Text`](xref:Xamarin.Forms.Button.Text) プロパティは表示されるテキストを指定し、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティはテキストが水平方向の中央に配置されるように指定します。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android 上の中央揃えのラベルのスクリーンショット](../images/create-label.png "中央揃えのラベル")](../images/create-label-large.png#lightbox "中央揃えのラベル")
