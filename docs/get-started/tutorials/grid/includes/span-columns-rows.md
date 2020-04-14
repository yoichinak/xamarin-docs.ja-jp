---
ms.openlocfilehash: c826ee87c006b05322af8c9312bdf3120df8b357
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "61375986"
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

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS リモート シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での、複数の列と行にまたがるコンテンツがある Grid のスクリーンショット](../images/span-columns-rows.png "列と行にまたがるコンテンツがある Grid")](../images/span-columns-rows-large.png#lightbox "列と行にまたがるコンテンツがある Grid")

    列と行のスパニングの詳細については、「[Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」 (Xamarin.Forms のグリッド) ガイドの「[Spans](~/xamarin-forms/user-interface/layouts/grid.md#spans)」 (スパン) を参照してください。

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

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android での、複数の列と行にまたがるコンテンツがある Grid のスクリーンショット](../images/span-columns-rows.png "列と行にまたがるコンテンツがある Grid")](../images/span-columns-rows-large.png#lightbox "列と行にまたがるコンテンツがある Grid")

    列と行のスパニングの詳細については、「[Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」 (Xamarin.Forms のグリッド) ガイドの「[Spans](~/xamarin-forms/user-interface/layouts/grid.md#spans)」 (スパン) を参照してください。
