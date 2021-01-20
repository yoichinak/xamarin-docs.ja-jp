---
ms.openlocfilehash: 9e78d92bdd2d6b0b398ef30ba5f30f71ef64cfd3
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98557177"
---
この演習では、以前に作成したデータ アクセス クラスを使用するためのユーザー インターフェイスを作成します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー** の **[LocalDatabaseTutorial]** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。 次に、 **[MainPage.xaml]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

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
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Age}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 2 つの [`Entry`](xref:Xamarin.Forms.Entry) インスタンス、[`Button`](xref:Xamarin.Forms.Button)、および [`CollectionView`](xref:Xamarin.Forms.CollectionView) から構成される、ページのユーザー インターフェイスを宣言によって定義します。 各 `Entry` にはその [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) プロパティが設定されており、これにより、ユーザー入力の前に表示されるプレースホルダー テキストが指定されます。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `CollectionView` ではその [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティが [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定されます。その場合、`StackLayout` と 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクトを使用して、`CollectionView` の各行の外観が定義されます。 `Label` オブジェクトではその `Text` プロパティが、各 `Person` オブジェクトの `Name` と `Age` のプロパティにそれぞれバインドされます。

    さらに、[`Entry`](xref:Xamarin.Forms.Entry) インスタンスと [`CollectionView`](xref:Xamarin.Forms.CollectionView) には、`x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは割り当てられた名前を使用して、これらのオブジェクトにアクセスできます。

1. **ソリューション エクスプローラー** の **[LocalDatabaseTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnAppearing` オーバーライドと `OnButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        collectionView.ItemsSource = await App.Database.GetPeopleAsync();
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
            collectionView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    `OnAppearing` メソッドでは、データベースに格納されたデータを [`CollectionView`](xref:Xamarin.Forms.CollectionView) に設定します。 [`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行される、`OnButtonClicked` メソッドでは、両方の [`Entry`](xref:Xamarin.Forms.Entry) インスタンスをクリアし、`CollectionView` のデータを更新する前に、データベースに入力されたデータを保存します。

    > [!NOTE]
    > `OnAppearing` メソッドのオーバーライドは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) がレイアウトされた後、それが表示される直前に実行されます。 したがって、これは Xamarin.Forms ビューのコンテンツを設定するのに適した場所です。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    データの各項目の [`Button`](xref:Xamarin.Forms.Button) をタップして、データのいくつかの項目を入力します。 これにより、データベースにデータが保存され、[`CollectionView`](xref:Xamarin.Forms.CollectionView) にデータベースのすべてのデータが再設定されます。

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
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Age}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    このコードでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の 2 つの [`Entry`](xref:Xamarin.Forms.Entry) インスタンス、[`Button`](xref:Xamarin.Forms.Button)、および [`CollectionView`](xref:Xamarin.Forms.CollectionView) から構成される、ページのユーザー インターフェイスを宣言によって定義します。 各 `Entry` にはその [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) プロパティが設定されており、これにより、ユーザー入力の前に表示されるプレースホルダー テキストが指定されます。 `Button` ではその [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントを、次の手順で作成される `OnButtonClicked` という名前のイベント ハンドラーに設定します。 `CollectionView` ではその [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティが [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定されます。その場合、`StackLayout` と 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクトを使用して、`CollectionView` の各行の外観が定義されます。 `Label` オブジェクトではその `Text` プロパティが、各 `Person` オブジェクトの `Name` と `Age` のプロパティにそれぞれバインドされます。

    さらに、[`Entry`](xref:Xamarin.Forms.Entry) インスタンスと [`ListView`](xref:Xamarin.Forms.ListView) には、`x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは割り当てられた名前を使用して、これらのオブジェクトにアクセスできます。

1. **Solution Pad** の **[LocalDatabaseTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** で、`OnAppearing` オーバーライドと `OnButtonClicked` イベント ハンドラーをクラスに追加します。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        collectionView.ItemsSource = await App.Database.GetPeopleAsync();
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
            collectionView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    `OnAppearing` メソッドでは、データベースに格納されたデータを [`CollectionView`](xref:Xamarin.Forms.CollectionView) に設定します。 [`Button`](xref:Xamarin.Forms.Button) がタップされたときに実行される、`OnButtonClicked` メソッドでは、両方の [`Entry`](xref:Xamarin.Forms.Entry) インスタンスをクリアし、`CollectionView` のデータを更新する前に、データベースに入力されたデータを保存します。

    > [!NOTE]
    > `OnAppearing` メソッドのオーバーライドは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) がレイアウトされた後、それが表示される直前に実行されます。 したがって、これは Xamarin.Forms ビューのコンテンツを設定するのに適した場所です。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    データの各項目の [`Button`](xref:Xamarin.Forms.Button) をタップして、データのいくつかの項目を入力します。 これにより、データベースにデータが保存され、[`CollectionView`](xref:Xamarin.Forms.CollectionView) にデータベースのすべてのデータが再設定されます。

    [![iOS および Android での、ローカル SQLite.NET データベースのデータ永続化のスクリーンショット](../images/consume-data-access-classes.png "ローカル データベースのデータ永続化")](../images/consume-data-access-classes-large.png#lightbox "ローカル データベースのデータ永続化")

    Xamarin.Forms のローカル データベースの詳細については、[Xamarin.Forms のローカル データベース (ガイド)](~/xamarin-forms/data-cloud/data/databases.md) に関するページを参照してください
