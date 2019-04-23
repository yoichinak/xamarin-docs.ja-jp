---
ms.openlocfilehash: 3c88b71cea834f5e6ef20d43332904c052c6e3a6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037626"
---
以前は、データ バインディングを使用して [`ListView`](xref:Xamarin.Forms.ListView) にデータが取り込まれていました。 しかし、コレクション内の各オブジェクトが複数のデータ項目を定義しているコレクションへのデータ バインディングであるにもかかわらず、オブジェクトごとに単一のデータ項目のみ (`Monkey` オブジェクトの `Name` プロパティー) が表示されていました。

この演習では、[`ListView`](xref:Xamarin.Forms.ListView) の各行に複数のデータ項目が表示されるように **ListViewTutorial** プロジェクトを変更します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`ListView`](xref:Xamarin.Forms.Image) 宣言を変更して各行の外観をカスタマイズします。

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    このコードは、[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。これにより、[`ListView`](xref:Xamarin.Forms.ListView) の各行の外観を定義します。 `DataTemplate` の子は、[`Cell`](xref:Xamarin.Forms.Cell) 型であるか、その型から派生している必要があります。 このコードは、`Cell` から派生した [`ViewCell`](xref:Xamarin.Forms.ViewCell) を使用して、各 `ListView` 行のカスタム レイアウトを作成します。 `ViewCell` 内のレイアウトは、1 つの [`Image`](xref:Xamarin.Forms.Image) オブジェクトと 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクトを含む [`Grid`](xref:Xamarin.Forms.Grid) によってここで管理されます。 `Image` オブジェクト データは、その [`Source`](xref:Xamarin.Forms.Image.Source) プロパティを各 `Monkey` オブジェクトの `ImageUrl` プロパティにバインドします。一方、`Label` オブジェクトは、それらの [`Text`](xref:Xamarin.Forms.Label.Text) プロパティを各 `Monkey` オブジェクトの `Name` プロパティと `Location` プロパティにバインドします。

    既定では、[`ListView`](xref:Xamarin.Forms.ListView) のすべての行は同じ高さです。 ただし、このコードは、行の高さをそのコンテンツに合わせて調整できるように [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) プロパティを `true` に設定します。 これは、可変長の文字列を持つ各 `Monkey` オブジェクトの `Name` プロパティと `Location` プロパティに対応します。

    [`ListView`](xref:Xamarin.Forms.ListView) セルの外観について詳しくは、「[Customizing ListView Cell Appearance](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)」(ListView セルの外観をカスタマイズする) をご覧ください。 データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![項目がデータ テンプレートでテンプレート化された ListView のスクリーンショット](../images/customize-cell-appearance.png "テンプレート化されたデータを表示する ListView")](../images/customize-cell-appearance-large.png#lightbox "テンプレート化されたデータを表示する ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`ListView`](xref:Xamarin.Forms.Image) 宣言を変更して各行の外観をカスタマイズします。

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    このコードは、[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。これにより、[`ListView`](xref:Xamarin.Forms.ListView) の各行の外観を定義します。 `DataTemplate` の子は、[`Cell`](xref:Xamarin.Forms.Cell) 型であるか、その型から派生している必要があります。 このコードは、`Cell` から派生した [`ViewCell`](xref:Xamarin.Forms.ViewCell) を使用して、各 `ListView` 行のカスタム レイアウトを作成します。 `ViewCell` 内のレイアウトは、1 つの [`Image`](xref:Xamarin.Forms.Image) オブジェクトと 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクトを含む [`Grid`](xref:Xamarin.Forms.Grid) によってここで管理されます。 `Image` オブジェクト データは、その [`Source`](xref:Xamarin.Forms.Image.Source) プロパティを各 `Monkey` オブジェクトの `ImageUrl` プロパティにバインドします。一方、`Label` オブジェクトは、それらの [`Text`](xref:Xamarin.Forms.Label.Text) プロパティを各 `Monkey` オブジェクトの `Name` プロパティと `Location` プロパティにバインドします。

    既定では、[`ListView`](xref:Xamarin.Forms.ListView) のすべての行は同じ高さです。 ただし、このコードは、行の高さをそのコンテンツに合わせて調整できるように [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) プロパティを `true` に設定します。 これは、可変長の文字列を持つ各 `Monkey` オブジェクトの `Name` プロパティと `Location` プロパティに対応します。

    [`ListView`](xref:Xamarin.Forms.ListView) セルの外観について詳しくは、「[Customizing ListView Cell Appearance](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)」(ListView セルの外観をカスタマイズする) をご覧ください。 データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![項目がデータ テンプレートでテンプレート化された ListView のスクリーンショット](../images/customize-cell-appearance.png "テンプレート化されたデータを表示する ListView")](../images/customize-cell-appearance-large.png#lightbox "テンプレート化されたデータを表示する ListView")
