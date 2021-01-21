---
ms.openlocfilehash: bf46d8ab4ca124bc36dd971513a78147e71a0039
ms.sourcegitcommit: 4d260b655cb52b990dda79c239a9721f2e964625
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570701"
---
この演習では、`RestService` クラスを使用し、GitHub Web API から .NET リポジトリ データを取得するためのユーザー インターフェイスを作成します。 取得したデータは、[`CollectionView`](xref:Xamarin.Forms.CollectionView) によって表示されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー** の **[WebServiceTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Get Repository Data"
                    Clicked="OnButtonClicked" />
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Description}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                            <Label Text="{Binding GitHubHomeUrl}"
                                   TextColor="Gray"
                                   FontSize="Caption" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、[`Button`](xref:Xamarin.Forms.Button) と [`CollectionView`](xref:Xamarin.Forms.CollectionView) から構成されるページのユーザー インターフェイスが宣言によって定義されます。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `CollectionView` は [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。これにより、`CollectionView` の各項目の外観が定義されます。 `DataTemplate` の子は [`StackLayout`](xref:Xamarin.Forms.StackLayout) です。これには、[`Label`](xref:Xamarin.Forms.Label) オブジェクトが含まれています。 `Label` オブジェクトではその [`Text`](xref:Xamarin.Forms.Label.Text) プロパティが、各 `Repository` オブジェクトの `Name`、`Description`、`GitHubHomeUrl` のプロパティにバインドされます。 データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

    また、[`CollectionView`](xref:Xamarin.Forms.CollectionView) には `x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは、割り当てられた名前を使用してオブジェクトにアクセスできます。

1. **ソリューション エクスプローラー** の **[WebServiceTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                List<Repository> repositories = await _restService.GetRepositoriesAsync(Constants.GitHubReposEndpoint);
                collectionView.ItemsSource = repositories;
            }
        }
    }
    ```

    `OnButtonClicked` メソッドは、[`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行され、GitHub Web API から .NET リポジトリ データを取得するための `RestService.GetRepositoriesAsync` メソッドを呼び出します。 `GetRepositoriesAsync` メソッドでは、呼び出される Web API の URI を表す `string` 引数が必要です。これは、`Constants.GitHubReposEndpoint` フィールドによって返されます。

    要求されたデータを取得した後、[`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティが、取得したデータに設定されます。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 GitHub から .NET リポジトリ データを取得するには、[`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS と Android における GitHub .NET リポジトリのスクリーンショット](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)

    Xamarin.Forms での REST ベース Web サービスの使用について詳しくは、[RESTful Web サービスの使用 (ガイド)](~/xamarin-forms/data-cloud/web-services/rest.md) に関するページを参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Get Repository Data"
                    Clicked="OnButtonClicked" />
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Description}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                            <Label Text="{Binding GitHubHomeUrl}"
                                   TextColor="Gray"
                                   FontSize="Caption" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、[`Button`](xref:Xamarin.Forms.Button) と [`CollectionView`](xref:Xamarin.Forms.CollectionView) から構成されるページのユーザー インターフェイスが宣言によって定義されます。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `CollectionView` は [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。これにより、`CollectionView` の各項目の外観が定義されます。 `DataTemplate` の子は [`StackLayout`](xref:Xamarin.Forms.StackLayout) です。これには、[`Label`](xref:Xamarin.Forms.Label) オブジェクトが含まれています。 `Label` オブジェクトではその [`Text`](xref:Xamarin.Forms.Label.Text) プロパティが、各 `Repository` オブジェクトの `Name`、`Description`、`GitHubHomeUrl` のプロパティにバインドされます。 データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

    また、[`CollectionView`](xref:Xamarin.Forms.CollectionView) には `x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは、割り当てられた名前を使用してオブジェクトにアクセスできます。

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                List<Repository> repositories = await _restService.GetRepositoriesAsync(Constants.GitHubReposEndpoint);
                collectionView.ItemsSource = repositories;
            }
        }
    }
    ```

    `OnButtonClicked` メソッドは、[`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行され、GitHub Web API から .NET リポジトリ データを取得するための `RestService.GetRepositoriesAsync` メソッドを呼び出します。 `GetRepositoriesAsync` メソッドでは、呼び出される Web API の URI を表す `string` 引数が必要です。これは、`Constants.GitHubReposEndpoint` フィールドによって返されます。

    要求されたデータを取得した後、[`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティが、取得したデータに設定されます。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 GitHub から .NET リポジトリ データを取得するには、[`Button`](xref:Xamarin.Forms.Button) をタップします。

    [![iOS と Android における GitHub .NET リポジトリのスクリーンショット](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)

    Xamarin.Forms での REST ベース Web サービスの使用について詳しくは、[RESTful Web サービスの使用 (ガイド)](~/xamarin-forms/data-cloud/web-services/rest.md) に関するページを参照してください。
