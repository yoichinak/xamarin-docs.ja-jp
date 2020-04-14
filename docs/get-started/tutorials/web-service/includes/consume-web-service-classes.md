---
ms.openlocfilehash: 90f3f9ff5ed29a1ae2c93e355fc15bc6550d78dd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135132"
---
この演習では、`RestService` クラスを使用し、その後、[OpenWeatherMap](https://openweathermap.org/) Web API からデータを取得するためのユーザー インターフェイスを作成します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー**の **[WebServiceTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    このコードでは、[`Grid`](xref:Xamarin.Forms.Grid) の中の [`Entry`](xref:Xamarin.Forms.Entry)、[`Button`](xref:Xamarin.Forms.Button)、および一連の [`Label`](xref:Xamarin.Forms.Label) インスタンスから構成される、ページのユーザー インターフェイスを宣言によって定義します。 `Entry` には、その [`Text`](xref:Xamarin.Forms.InputView.Text) プロパティを設定することで、事前に "Seattle" が入力されています。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `Label` インスタンスの半分には静的テキストが表示されており、残りのインスタンスは `WeatherData` プロパティにデータ バインディングされます。 実行時に、データ バインディングを使用する `Label` インスタンスでは、バインド式で使用するための `WeatherData` オブジェクトのそれぞれの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティが参照されます。 データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

    また、[`Entry`](xref:Xamarin.Forms.Entry) には `x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは、割り当てられた名前を使用してオブジェクトにアクセスできます。

1. **ソリューション エクスプローラー**の **[WebServiceTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
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
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行される、`OnButtonClicked` メソッドでは、`RestService.GetWeatherDataAsync` メソッドを呼び出して、名前が [`Entry`](xref:Xamarin.Forms.Entry) に入力されている都市の気象データを取得します。 `GetWeatherDataAsync` メソッドでは、呼び出される Web API の URI を表す `string` 引数が必要です。これは、`GenerateRequestUri` メソッドによって生成されます。 このメソッドでは、`OpenWeatherMapEndpoint` 定数に格納されているエンドポイント アドレスを受け取り、クエリ パラメーターを追加して、以下を指定します。

    - 気象データが要求されている都市。
    - 気象データを返す単位。
    - 個人用 API キー。

    要求された気象データの取得後、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) は `WeatherData` オブジェクトに設定されます。 `BindingContext` プロパティの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) ガイドの「[Bindings with a Binding Context](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context)」 (バインディング コンテキストを使用するバインディング) を参照してください。

    > [!IMPORTANT]
    > [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティはビジュアル ツリーを介して継承されます。 したがって、それが [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに設定されているため、`ContentPage` の子オブジェクトは [`Label`](xref:Xamarin.Forms.Label) インスタンスを含むその値を継承します。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Button`](xref:Xamarin.Forms.Button) をタップして、Seattle の現在の気象データを取得します。

    [![iOS および Android での、Seattle の気象データのスクリーンショット](../images/consume-web-service.png "Seattle の気象データ")](../images/consume-web-service-large.png#lightbox "Seattle の気象データ")

    > [!IMPORTANT]
    > 個人用 OpenWeatherMap API キーは、`Constants` クラスの `OpenWeatherMapAPIKey` 定数値として設定する必要があります。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    このコードでは、[`Grid`](xref:Xamarin.Forms.Grid) の中の [`Entry`](xref:Xamarin.Forms.Entry)、[`Button`](xref:Xamarin.Forms.Button)、および一連の [`Label`](xref:Xamarin.Forms.Label) インスタンスから構成される、ページのユーザー インターフェイスを宣言によって定義します。 `Entry` には、その [`Text`](xref:Xamarin.Forms.InputView.Text) プロパティを設定することで、事前に "Seattle" が入力されています。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `Label` インスタンスの半分には静的テキストが表示されており、残りのインスタンスは `WeatherData` プロパティにデータ バインディングされます。 実行時に、データ バインディングを使用する `Label` インスタンスでは、バインド式で使用するための `WeatherData` オブジェクトのそれぞれの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティが参照されます。 データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

    また、[`Entry`](xref:Xamarin.Forms.Entry) には `x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは、割り当てられた名前を使用してオブジェクトにアクセスできます。

    Xamarin.Forms での REST ベース Web サービスの使用について詳しくは、[RESTful Web サービスの使用 (ガイド)](~/xamarin-forms/data-cloud/web-services/rest.md) に関するページを参照してください。

1. **Solution Pad** の **[WebServiceTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
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
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行される、`OnButtonClicked` メソッドでは、`RestService.GetWeatherDataAsync` メソッドを呼び出して、名前が [`Entry`](xref:Xamarin.Forms.Entry) に入力されている都市の気象データを取得します。 `GetWeatherDataAsync` メソッドでは、呼び出される Web API の URI を表す `string` 引数が必要です。これは、`GenerateRequestUri` メソッドによって生成されます。 このメソッドでは、`OpenWeatherMapEndpoint` 定数に格納されているエンドポイント アドレスを受け取り、クエリ パラメーターを追加して、以下を指定します。

    - 気象データが要求されている都市。
    - 気象データを返す単位。
    - 個人用 API キー。

    要求された気象データの取得後、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) は `WeatherData` オブジェクトに設定されます。 `BindingContext` プロパティの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) ガイドの「[Bindings with a Binding Context](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context)」 (バインディング コンテキストを使用するバインディング) を参照してください。

    > [!IMPORTANT]
    > [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティはビジュアル ツリーを介して継承されます。 したがって、それが [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに設定されているため、`ContentPage` の子オブジェクトは [`Label`](xref:Xamarin.Forms.Label) インスタンスを含むその値を継承します。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Button`](xref:Xamarin.Forms.Button) をタップして、Seattle の現在の気象データを取得します。

    [![iOS および Android での、Seattle の気象データのスクリーンショット](../images/consume-web-service.png "Seattle の気象データ")](../images/consume-web-service-large.png#lightbox "Seattle の気象データ")

    > [!IMPORTANT]
    > 個人用 OpenWeatherMap API キーは、`Constants` クラスの `OpenWeatherMapAPIKey` 定数値として設定する必要があります。

    Xamarin.Forms での REST ベース Web サービスの使用について詳しくは、[RESTful Web サービスの使用 (ガイド)](~/xamarin-forms/data-cloud/web-services/rest.md) に関するページを参照してください。
