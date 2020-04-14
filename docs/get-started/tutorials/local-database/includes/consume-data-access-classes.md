---
ms.openlocfilehash: caee3eeda90a560f032c17657072ae5ba5023a69
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135104"
---
この演習では、以前に作成したデータ アクセス クラスを使用するためのユーザー インターフェイスを作成します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー**の **[LocalDatabaseTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 2 つの [`Entry`](xref:Xamarin.Forms.Entry) インスタンス、[`Button`](xref:Xamarin.Forms.Button)、および [`ListView`](xref:Xamarin.Forms.ListView) から構成される、ページのユーザー インターフェイスを宣言によって定義します。 各 `Entry` にはその [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) プロパティが設定されており、これにより、ユーザー入力の前に表示されるプレースホルダー テキストが指定されます。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `ListView` ではその [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。その場合、[`TextCell`](xref:Xamarin.Forms.TextCell) を使用して、[`ListView`](xref:Xamarin.Forms.ListView) の各行の外観を定義します。 `TextCell` ではその [`Text`](xref:Xamarin.Forms.TextCell.Text) および [`Detail`](xref:Xamarin.Forms.TextCell.Detail) プロパティを、各 `Person` オブジェクトの `Name` および `Age` プロパティにそれぞれデータ バインドします。

    さらに、[`Entry`](xref:Xamarin.Forms.Entry) インスタンスと [`ListView`](xref:Xamarin.Forms.ListView) には、`x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは割り当てられた名前を使用して、これらのオブジェクトにアクセスできます。

1. **ソリューション エクスプローラー**の **[LocalDatabaseTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnAppearing` オーバーライドと `OnButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    `OnAppearing` メソッドでは、データベースに格納されたデータを [`ListView`](xref:Xamarin.Forms.ListView) に設定します。 [`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行される、`OnButtonClicked` メソッドでは、両方の [`Entry`](xref:Xamarin.Forms.Entry) インスタンスをクリアし、`ListView` のデータを更新する前に、データベースに入力されたデータを保存します。

    > [!NOTE]
    > `OnAppearing` メソッドのオーバーライドは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) がレイアウトされた後、それが表示される直前に実行されます。 したがって、これは Xamarin.Forms ビューのコンテンツを設定するのに適した場所です。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    データの各項目の [`Button`](xref:Xamarin.Forms.Button) をタップして、データのいくつかの項目を入力します。 これにより、データベースにデータが保存され、[`ListView`](xref:Xamarin.Forms.ListView) にデータベースのすべてのデータが再設定されます。

    [![iOS および Android での、ローカル SQLite.NET データベースのデータ永続化のスクリーンショット](../images/consume-data-access-classes.png "ローカル データベースのデータ永続化")](../images/consume-data-access-classes-large.png#lightbox "ローカル データベースのデータ永続化")

    Xamarin.Forms のローカル データベースの詳細については、[Xamarin.Forms のローカル データベース (ガイド)](~/xamarin-forms/data-cloud/data/databases.md) に関するページを参照してください

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[LocalDatabaseTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 2 つの [`Entry`](xref:Xamarin.Forms.Entry) インスタンス、[`Button`](xref:Xamarin.Forms.Button)、および [`ListView`](xref:Xamarin.Forms.ListView) から構成される、ページのユーザー インターフェイスを宣言によって定義します。 各 `Entry` にはその [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) プロパティが設定されており、これにより、ユーザー入力の前に表示されるプレースホルダー テキストが指定されます。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `ListView` ではその [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。その場合、[`TextCell`](xref:Xamarin.Forms.TextCell) を使用して、[`ListView`](xref:Xamarin.Forms.ListView) の各行の外観を定義します。 `TextCell` ではその [`Text`](xref:Xamarin.Forms.TextCell.Text) および [`Detail`](xref:Xamarin.Forms.TextCell.Detail) プロパティを、各 `Person` オブジェクトの `Name` および `Age` プロパティにそれぞれデータ バインドします。

    さらに、[`Entry`](xref:Xamarin.Forms.Entry) インスタンスと [`ListView`](xref:Xamarin.Forms.ListView) には、`x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは割り当てられた名前を使用して、これらのオブジェクトにアクセスできます。

1. **Solution Pad** の **[LocalDatabaseTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnAppearing` オーバーライドと `OnButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    `OnAppearing` メソッドでは、データベースに格納されたデータを [`ListView`](xref:Xamarin.Forms.ListView) に設定します。 [`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行される、`OnButtonClicked` メソッドでは、両方の [`Entry`](xref:Xamarin.Forms.Entry) インスタンスをクリアし、`ListView` のデータを更新する前に、データベースに入力されたデータを保存します。

    > [!NOTE]
    > `OnAppearing` メソッドのオーバーライドは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) がレイアウトされた後、それが表示される直前に実行されます。 したがって、これは Xamarin.Forms ビューのコンテンツを設定するのに適した場所です。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    データの各項目の [`Button`](xref:Xamarin.Forms.Button) をタップして、データのいくつかの項目を入力します。 これにより、データベースにデータが保存され、[`ListView`](xref:Xamarin.Forms.ListView) にデータベースのすべてのデータが再設定されます。

    [![iOS および Android での、ローカル SQLite.NET データベースのデータ永続化のスクリーンショット](../images/consume-data-access-classes.png "ローカル データベースのデータ永続化")](../images/consume-data-access-classes-large.png#lightbox "ローカル データベースのデータ永続化")

    Xamarin.Forms のローカル データベースの詳細については、[Xamarin.Forms のローカル データベース (ガイド)](~/xamarin-forms/data-cloud/data/databases.md) に関するページを参照してください
