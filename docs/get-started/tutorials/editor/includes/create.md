---
ms.openlocfilehash: 76eab5a08f5ccc53a4db5370bf996aed75c142e7
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690023"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

このチュートリアルを完了するには、 **.NET によるモバイル開発** ワークロードがインストールされた、Visual Studio 2019 (最新リリース) が必要です。 さらに、iOS でチュートリアル アプリケーションを構築するには、ペアリング済みの Mac が必要になります。 Xamarin プラットフォームのインストールについては、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

1. Visual Studio を起動し、**EditorTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**EditorTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー** の **EditorTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

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

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Editor`](xref:Xamarin.Forms.Editor) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Editor.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) プロパティは、`Editor` が最初に表示されたときに表示されるプレースホルダー テキストを指定します。 さらに、[`HeightRequest`](xref:Xamarin.Forms.VisualElement) プロパティは、`Editor` の高さをデバイスに依存しない単位で指定します。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android 上の Editor のスクリーンショット](../images/create-editor.png "プレースホルダー テキストを含む Editor")](../images/create-editor-large.png#lightbox "プレースホルダー テキストを含む Editor")

    > [!NOTE]
    > Android では [`Editor`](xref:Xamarin.Forms.Editor) の高さが示されますが、iOS では示されません。

    Visual Studio で、アプリケーションを停止します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

このチュートリアルを完了するには、iOS と Android のプラットフォームのサポートがインストールされた Visual Studio for Mac (最新リリース) が必要です。 さらに、Xcode (最新リリース) も必要になります。 Xamarin プラットフォームのインストールについて詳しくは、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

1. Visual Studio for Mac を起動し、**EditorTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**EditorTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **EditorTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

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

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Editor`](xref:Xamarin.Forms.Editor) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Editor.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) プロパティは、`Editor` が最初に表示されたときに表示されるプレースホルダー テキストを指定します。 さらに、[`HeightRequest`](xref:Xamarin.Forms.VisualElement) プロパティは、`Editor` の高さをデバイスに依存しない単位で指定します。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android 上の Editor のスクリーンショット](../images/create-editor.png "プレースホルダー テキストを含む Editor")](../images/create-editor-large.png#lightbox "プレースホルダー テキストを含む Editor")

    > [!NOTE]
    > Android では [`Editor`](xref:Xamarin.Forms.Editor) の高さが示されますが、iOS では示されません。

    Visual Studio for Mac で、アプリケーションを停止します。
