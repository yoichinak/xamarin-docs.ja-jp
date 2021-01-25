---
ms.openlocfilehash: 89b56e3733d1c75ed616f79508d555ccb0c51f00
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634889"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`Grid`](xref:Xamarin.Forms.Grid) 宣言を変更して列と行を定義し、列と行にまたがるコンテンツを配置します。

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    このコードでは、[`Grid`](xref:Xamarin.Forms.Grid) の列と行を定義し、[`Label`](xref:Xamarin.Forms.Label) インスタンスを特定の列と行に配置します。 最初の `Label` では [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 添付プロパティを設定し、そのテキストが複数の列にまたがるようにします。 `ColumnSpan` プロパティは 2 に設定されます。これは、`Label` がまたがる列の数を表します。 2 番目の `Label` では [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 添付プロパティを設定し、そのテキストが複数の行にまたがるようにします。 `RowSpan` プロパティは 2 に設定されます。これは、`Label` がまたがる行の数を表します。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS リモート シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での、複数の列と行にまたがるコンテンツがある Grid のスクリーンショット](../images/span-columns-rows.png "列と行にまたがるコンテンツがある Grid")](../images/span-columns-rows-large.png#lightbox "列と行にまたがるコンテンツがある Grid")

    Visual Studio で、アプリケーションを停止します。

    列と行のスパニングの詳細については、「[Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」 (Xamarin.Forms のグリッド) ガイドの「[Rows and columns](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns)」 (行および列) を参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`Grid`](xref:Xamarin.Forms.Grid) 宣言を変更して列と行を定義し、列と行にまたがるコンテンツを配置します。

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    このコードでは、[`Grid`](xref:Xamarin.Forms.Grid) の列と行を定義し、[`Label`](xref:Xamarin.Forms.Label) インスタンスを特定の列と行に配置します。 最初の `Label` では [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 添付プロパティを設定し、そのテキストが複数の列にまたがるようにします。 `ColumnSpan` プロパティは 2 に設定されます。これは、`Label` がまたがる列の数を表します。 2 番目の `Label` では [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 添付プロパティを設定し、そのテキストが複数の行にまたがるようにします。 `RowSpan` プロパティは 2 に設定されます。これは、`Label` がまたがる行の数を表します。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での、複数の列と行にまたがるコンテンツがある Grid のスクリーンショット](../images/span-columns-rows.png "列と行にまたがるコンテンツがある Grid")](../images/span-columns-rows-large.png#lightbox "列と行にまたがるコンテンツがある Grid")

    Visual Studio for Mac で、アプリケーションを停止します。

    列と行のスパニングの詳細については、「[Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」 (Xamarin.Forms のグリッド) ガイドの「[Rows and columns](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns)」 (行および列) を参照してください。
