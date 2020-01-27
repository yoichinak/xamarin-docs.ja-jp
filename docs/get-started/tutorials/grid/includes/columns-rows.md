---
ms.openlocfilehash: d87289e481b69592b68627d053e937856d3d6067
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "61376011"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`Grid`](xref:Xamarin.Forms.Grid) 宣言を変更して列と行を定義し、列と行にコンテンツを配置します。

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    このコードは、[`Grid`](xref:Xamarin.Forms.Grid) の列と行を定義し、[`Label`](xref:Xamarin.Forms.Label) インスタンスを特定の列と行に配置します。 列と行のデータは、[`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) プロパティと [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) プロパティに格納されます。これらのプロパティはそれぞれ、[`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) オブジェクトと [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) オブジェクトのコレクションです。 各 `ColumnDefinition` の幅は、[`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) プロパティによって設定され、各 `RowDefinition` の高さは [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) プロパティによって設定されます。 有効な幅と高さの値は次のとおりです。

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto): コンテンツに合わせて行または列のサイズを設定します。
    - 比例値: 残りのスペースに比例して、列と行のサイズを設定します。 比例値は、末尾に `*` を付けて示されます。
    - 絶対値: 特定の固定値で列または行のサイズを設定します。

    そのため、上記のコードでは、各列の幅は [`Grid`](xref:Xamarin.Forms.Grid) の半分ですが、各行の高さは 50 (デバイスに依存しない単位) になっています。

    [`Grid`](xref:Xamarin.Forms.Grid) 内の各[`Label`](xref:Xamarin.Forms.Label) の位置は、0 から始まるインデックスを使用して、[`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) と [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) の添付プロパティで指定されます。 したがって、最初の列は列 0 で、最初の行は行 0 です。 最初の `Label` には、これらの添付プロパティがありません。これは、添付プロパティを設定しない子ビューはすべて自動的に列 0 と行 0 にレンダリングされるためです。

    > [!NOTE]
    > [`Grid`](xref:Xamarin.Forms.Grid) 内の列と行の間隔は、[`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) プロパティと [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) プロパティで設定できます。 詳しくは、「[Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」ガイドの「[Spacing](~/xamarin-forms/user-interface/layouts/grid.md#spacing)」 (間隔) を参照してください。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、列と行にレイアウトされたコンテンツがある Grid のスクリーンショット](../images/columns-rows.png "列と行にコンテンツがある Grid")](../images/columns-rows-large.png#lightbox "列と行にコンテンツがある Grid")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`Grid`](xref:Xamarin.Forms.Grid) 宣言を変更して列と行を定義し、列と行にコンテンツを配置します。

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    このコードは、[`Grid`](xref:Xamarin.Forms.Grid) の列と行を定義し、[`Label`](xref:Xamarin.Forms.Label) インスタンスを特定の列と行に配置します。 列と行のデータは、[`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) プロパティと [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) プロパティに格納されます。これらのプロパティはそれぞれ、[`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) オブジェクトと [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) オブジェクトのコレクションです。 各 `ColumnDefinition` の幅は、[`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) プロパティによって設定され、各 `RowDefinition` の高さは [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) プロパティによって設定されます。 有効な幅と高さの値は次のとおりです。

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto): コンテンツに合わせて行または列のサイズを設定します。
    - 比例値: 残りのスペースに比例して、列と行のサイズを設定します。 比例値は、末尾に `*` を付けて示されます。
    - 絶対値: 特定の固定値で列または行のサイズを設定します。

    そのため、上記のコードでは、各列の幅は [`Grid`](xref:Xamarin.Forms.Grid) の半分ですが、各行の高さは 50 (デバイスに依存しない単位) になっています。

    [`Grid`](xref:Xamarin.Forms.Grid) 内の各[`Label`](xref:Xamarin.Forms.Label) の位置は、0 から始まるインデックスを使用して、[`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) と [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) の添付プロパティで指定されます。 したがって、最初の列は列 0 で、最初の行は行 0 です。 最初の `Label` には、これらの添付プロパティがありません。これは、添付プロパティを設定しない子ビューはすべて自動的に列 0 と行 0 にレンダリングされるためです。

    > [!NOTE]
    > [`Grid`](xref:Xamarin.Forms.Grid) 内の列と行の間隔は、[`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) プロパティと [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) プロパティで設定できます。 詳しくは、「[Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」ガイドの「[Spacing](~/xamarin-forms/user-interface/layouts/grid.md#spacing)」 (間隔) を参照してください。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、列と行にレイアウトされたコンテンツがある Grid のスクリーンショット](../images/columns-rows.png "列と行にコンテンツがある Grid")](../images/columns-rows-large.png#lightbox "列と行にコンテンツがある Grid")
