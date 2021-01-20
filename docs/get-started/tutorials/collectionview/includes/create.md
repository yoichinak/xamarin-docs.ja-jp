---
ms.openlocfilehash: bd9d4bbd04f504400bd14bb3a18bf4c2b16a5849
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98558904"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

このチュートリアルを完了するには、 **.NET によるモバイル開発** ワークロードがインストールされた、Visual Studio 2019 (最新リリース) が必要です。 さらに、iOS でチュートリアル アプリケーションを構築するには、ペアリング済みの Mac が必要になります。 Xamarin プラットフォームのインストールについては、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

1. Visual Studio を起動し、**CollectionViewTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# と XAML のスニペットでは、**CollectionViewTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー** の **[CollectionViewTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="CollectionViewTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <CollectionView>
                <CollectionView.ItemsSource>
                    <x:Array Type="{x:Type x:String}">
                      <x:String>Baboon</x:String>
                      <x:String>Capuchin Monkey</x:String>
                      <x:String>Blue Monkey</x:String>
                      <x:String>Squirrel Monkey</x:String>
                      <x:String>Golden Lion Tamarin</x:String>
                      <x:String>Howler Monkey</x:String>
                      <x:String>Japanese Macaque</x:String>
                    </x:Array>
                </CollectionView.ItemsSource>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`CollectionView`](xref:Xamarin.Forms.CollectionView) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティでは、表示する項目を指定します。これらは文字列の配列で定義されます。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android 上の CollectionView のスクリーンショット](../images/create-collectionview.png "データを表示する CollectionView")](../images/create-collectionview-large.png#lightbox "データを表示する CollectionView")

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

このチュートリアルを完了するには、iOS と Android のプラットフォームのサポートがインストールされた Visual Studio for Mac (最新リリース) が必要です。 さらに、Xcode (最新リリース) も必要になります。 Xamarin プラットフォームのインストールについて詳しくは、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

1. Visual Studio for Mac を起動し、**CollectionViewTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# と XAML のスニペットでは、**CollectionViewTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **[CollectionViewTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="CollectionViewTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <CollectionView>
                <CollectionView.ItemsSource>
                    <x:Array Type="{x:Type x:String}">
                      <x:String>Baboon</x:String>
                      <x:String>Capuchin Monkey</x:String>
                      <x:String>Blue Monkey</x:String>
                      <x:String>Squirrel Monkey</x:String>
                      <x:String>Golden Lion Tamarin</x:String>
                      <x:String>Howler Monkey</x:String>
                      <x:String>Japanese Macaque</x:String>
                    </x:Array>
                </CollectionView.ItemsSource>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`CollectionView`](xref:Xamarin.Forms.CollectionView) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティでは、表示する項目を指定します。これらは文字列の配列で定義されます。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android 上の CollectionView のスクリーンショット](../images/create-collectionview.png "データを表示する CollectionView")](../images/create-collectionview-large.png#lightbox "データを表示する CollectionView")
