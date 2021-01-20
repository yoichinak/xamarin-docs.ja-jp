---
ms.openlocfilehash: 4464b5cc4220072de3d55d76f568dab938d3d805
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98558905"
---
以前は、データ バインディングを使用して [`CollectionView`](xref:Xamarin.Forms.CollectionView) にデータが取り込まれていました。 しかし、コレクション内の各オブジェクトが複数のデータ項目を定義しているコレクションへのデータ バインディングであるにもかかわらず、オブジェクトごとに単一のデータ項目のみ (`Monkey` オブジェクトの `Name` プロパティー) が表示されていました。

この演習では、[`CollectionView`](xref:Xamarin.Forms.CollectionView) の各行に複数のデータ項目が表示されるように **CollectionViewTutorial** プロジェクトを変更します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`CollectionView`](xref:Xamarin.Forms.CollectionView) 宣言を変更してデータの各項目の外観をカスタマイズします。

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10"
                      RowDefinitions="Auto, *"
                      ColumnDefinitions="Auto, *">
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
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    このコードは、[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。これにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) の各項目の外観を定義します。 `DataTemplate` の子は [`Grid`](xref:Xamarin.Forms.Grid) です。これには、[`Image`](xref:Xamarin.Forms.Image) オブジェクトと 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクトが含まれています。 `Image` オブジェクト データは、その [`Source`](xref:Xamarin.Forms.Image.Source) プロパティを各 `Monkey` オブジェクトの `ImageUrl` プロパティにバインドします。一方、`Label` オブジェクトは、それらの [`Text`](xref:Xamarin.Forms.Label.Text) プロパティを各 `Monkey` オブジェクトの `Name` プロパティと `Location` プロパティにバインドします。

    [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目の外観の詳細については、「[項目の外観を定義する](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)」を参照してください。 データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![項目がデータ テンプレートでテンプレート化された CollectionView のスクリーンショット](../images/customize-item-appearance.png "テンプレート化されたデータを表示する CollectionView")](../images/customize-item-appearance-large.png#lightbox "テンプレート化されたデータを表示する CollectionView")

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`CollectionView`](xref:Xamarin.Forms.CollectionView) 宣言を変更してデータの各項目の外観をカスタマイズします。

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10"
                      RowDefinitions="Auto, *"
                      ColumnDefinitions="Auto, *">
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
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    このコードは、[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定します。これにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) の各項目の外観を定義します。 `DataTemplate` の子は [`Grid`](xref:Xamarin.Forms.Grid) です。これには、[`Image`](xref:Xamarin.Forms.Image) オブジェクトと 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクトが含まれています。 `Image` オブジェクト データは、その [`Source`](xref:Xamarin.Forms.Image.Source) プロパティを各 `Monkey` オブジェクトの `ImageUrl` プロパティにバインドします。一方、`Label` オブジェクトは、それらの [`Text`](xref:Xamarin.Forms.Label.Text) プロパティを各 `Monkey` オブジェクトの `Name` プロパティと `Location` プロパティにバインドします。

    [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目の外観の詳細については、「[項目の外観を定義する](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)」を参照してください。 データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![項目がデータ テンプレートでテンプレート化された CollectionView のスクリーンショット](../images/customize-item-appearance.png "テンプレート化されたデータを表示する CollectionView")](../images/customize-item-appearance-large.png#lightbox "テンプレート化されたデータを表示する CollectionView")
