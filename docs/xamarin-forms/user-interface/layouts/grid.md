---
title: Xamarin.Forms行列
description: Xamarin.Formsグリッドは、その子をセルの行と列に編成するレイアウトです。
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9d2e697a07e033fd7c3c8d3efffa1d67f6c097c3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946339"
---
# <a name="xamarinforms-grid"></a>Xamarin.Forms行列

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)

[![Xamarin.Forms行列](grid-images/layouts.png "[!ファンド.NO LOC (Xamarin)] グリッド")](grid-images/layouts-large.png#lightbox "[!ファンド.NO LOC (Xamarin)] グリッド")

は、 [`Grid`](xref:Xamarin.Forms.Grid) その子を行と列に編成するレイアウトであり、比例または絶対的なサイズを持つことができます。 既定では、には `Grid` 1 つの行と1つの列が含まれます。 また、は、 `Grid` 他の子レイアウトを含む親レイアウトとして使用できます。

[`Grid`](xref:Xamarin.Forms.Grid)レイアウトはテーブルと混同しないようにしてください。表形式のデータを表示するためのものではありません。 HTML テーブルとは異なり、は、コンテンツをレイアウトすることを目的とし `Grid` ています。 表形式データを表示する場合は、 [ListView](~/xamarin-forms/user-interface/listview/index.md)、 [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)、または[TableView](~/xamarin-forms/user-interface/tableview.md)を使用することを検討してください。

[`Grid`](xref:Xamarin.Forms.Grid)クラスは、次のプロパティを定義します。

- [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty)型の `int` 。これは、親内のビューの列の配置を示す添付プロパティです `Grid` 。 このプロパティの既定値は 0 です。 検証コールバックは、プロパティが設定されている場合に、その値が0以上であることを確認します。
- [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)型の [`ColumnDefinitionCollection`](xref:Xamarin.Forms.ColumnDefinitionCollection) は、 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) グリッド列の幅を定義するオブジェクトの一覧です。
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing)型のは、 `double` グリッド列間の距離を示します。 このプロパティの既定値は、デバイスに依存しない6つの単位です。
- [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)型の。 `int` これは、親内のビューがまたがる列の合計数を示す添付プロパティです `Grid` 。 このプロパティの既定値は 1 です。 検証コールバックは、プロパティが設定されている場合、その値が1以上であることを確認します。
- [`Row`](xref:Xamarin.Forms.Grid.RowProperty)型の `int` 。これは、親内のビューの行の配置を示す添付プロパティです `Grid` 。 このプロパティの既定値は 0 です。 検証コールバックは、プロパティが設定されている場合に、その値が0以上であることを確認します。
- [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)型の [`RowDefinitionCollection`](xref:Xamarin.Forms.RowDefinitionCollection) は、 [`RowDefintion`](xref:Xamarin.Forms.RowDefinition) グリッド行の高さを定義するオブジェクトのリストです。
- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)型のは、 `double` グリッド行間の距離を示します。 このプロパティの既定値は、デバイスに依存しない6つの単位です。
- [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)型の。 `int` これは、親内のビューがまたがる行の合計数を示す添付プロパティです `Grid` 。 このプロパティの既定値は 1 です。 検証コールバックは、プロパティが設定されている場合、その値が1以上であることを確認します。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、プロパティは、データバインディングのターゲットとスタイルを設定できます。

クラスは、 [`Grid`](xref:Xamarin.Forms.Grid) `Layout<T>` 型のプロパティを定義するクラスから派生 `Children` `IList<T>` します。 `Children`プロパティは `ContentProperty` クラスのである `Layout<T>` ため、XAML から明示的に設定する必要はありません。

> [!TIP]
> レイアウトの最適なパフォーマンスを得るには、「[レイアウトのパフォーマンスを最適化](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)する」のガイドラインに従ってください。

## <a name="rows-and-columns"></a>行と列

既定では、に [`Grid`](xref:Xamarin.Forms.Grid) 1 つの行と1つの列が含まれます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridTutorial.MainPage">
    <Grid Margin="20,35,20,20">
        <Label Text="By default, a Grid contains one row and one column." />
    </Grid>
</ContentPage>
```

この例では、 [`Grid`](xref:Xamarin.Forms.Grid) に、1つの [`Label`](xref:Xamarin.Forms.Label) 場所に自動的に配置される単一の子が含まれています。

[![既定のグリッドレイアウトのスクリーンショット](grid-images/default.png "既定のグリッドレイアウト")](grid-images/default-large.png#lightbox "既定のグリッドレイアウト")

のレイアウト動作は、 [`Grid`](xref:Xamarin.Forms.Grid) プロパティとプロパティを使用して定義でき [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) ます。このプロパティは、 [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) それぞれオブジェクトとオブジェクトのコレクションです [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 。 これらのコレクションは、の行と列の特性を定義 `Grid` します。には、の各行に1つの [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) オブジェクト、およびの `Grid` 各列に1つのオブジェクトを含める必要があり [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) `Grid` ます。

クラスは、 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) [`Height`](xref:Xamarin.Forms.RowDefinition.Height) 型のプロパティを定義 [`GridLength`](xref:Xamarin.Forms.GridLength) し、 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) クラスは [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) 型のプロパティを定義し [`GridLength`](xref:Xamarin.Forms.GridLength) ます。 [`GridLength`](xref:Xamarin.Forms.GridLength)構造体は、 [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) 次の3つのメンバーを持つ列挙体に関して行の高さまたは列の幅を指定します。

- `Absolute`–行の高さまたは列の幅は、デバイスに依存しない単位 (XAML の数値) の値です。
- `Auto`–行の高さまたは列の幅は、(XAML の) セルの内容に基づいて自動的に設定され `Auto` ます。
- `Star`–未使用の行の高さまたは列の幅が比例して割り当てられます (XAML の数値の後に `*` )。

[`Grid`](xref:Xamarin.Forms.Grid)のプロパティを持つ行では、 `Height` `Auto` 垂直方向と同じように、その行のビューの高さが制限され [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 同様に、のプロパティを持つ列は、 `Width` `Auto` 水平方向と同様に機能し `StackLayout` ます。

> [!CAUTION]
> できるだけ少ない数の行と列が size に設定されるようにしてください [`Auto`](xref:Xamarin.Forms.GridLength.Auto) 。 自動サイズ調整された行または列はそれぞれ、レイアウト エンジンに追加のレイアウト計算を実行させることになります。 その代わりに可能であれば、固定サイズの行と列を使用してください。 または、行と列に対して、列挙値を使用して比例した領域を占めるように設定し [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) ます。

次の XAML は、 [`Grid`](xref:Xamarin.Forms.Grid) 3 つの行と2つの列を持つを作成する方法を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

この例では、の [`Grid`](xref:Xamarin.Forms.Grid) 高さがページ全体の高さになります。 は、 `Grid` 3 番目の行の高さが100デバイスに依存しない単位であることを認識します。 その高さを独自の高さから減算し、残りの高さを、星の前の数値に基づいて最初の行と2番目の行の間に均等に割り当てます。 この例では、最初の行の高さが2番目の行の2倍になっています。

2つの [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) オブジェクトは両方ともに設定されます [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) `*` 。これは、 `1*` 画面の幅が2つの列の下に均等に分割されることを意味します。

> [!IMPORTANT]
> プロパティの既定値 [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) は `*` です。 同様に、プロパティの既定値 [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) は `*` です。 そのため、これらの既定値が許容される場合は、これらのプロパティを設定する必要はありません。

子ビューは [`Grid`](xref:Xamarin.Forms.Grid) 、および添付プロパティを使用して特定のセルに配置でき [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) ます。 さらに、複数の行と列にまたがる子ビューを作成するには、 [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) プロパティと添付プロパティを使用し [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) ます。

次の XAML は同じ [`Grid`](xref:Xamarin.Forms.Grid) 定義を示しています。また、特定のセルに子ビューを配置し `Grid` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <BoxView Color="Green" />
        <Label Text="Row 0, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Column="1"
                 Color="Blue" />
        <Label Grid.Column="1"
               Text="Row 0, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Color="Teal" />
        <Label Grid.Row="1"
               Text="Row 1, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="Purple" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Row1, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Grid.ColumnSpan="2"
                 Color="Red" />
        <Label Grid.Row="2"
               Grid.ColumnSpan="2"
               Text="Row 2, Columns 0 and 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

> [!NOTE]
> `Grid.Row`プロパティと `Grid.Column` プロパティは両方とも0からインデックスが付けられます。したがって、は3番目 `Grid.Row="2"` の行を参照し、は `Grid.Column="1"` 2 番目の列を参照します。 さらに、これらのプロパティのどちらも既定値は0であるため、の最初の行または最初の列を占有する子ビューに対して設定する必要はありません [`Grid`](xref:Xamarin.Forms.Grid) 。

この例では、3つの [`Grid`](xref:Xamarin.Forms.Grid) 行すべてがビューとビューで占有されてい [`BoxView`](xref:Xamarin.Forms.BoxView) [`Label`](xref:Xamarin.Forms.Label) ます。 3番目の行は、デバイスに依存しない単位の上限である100です。最初の2つの行は、残りの領域を占めます (最初の行は2番目の行の2倍の2行になります)。 2つの列の幅は同じで、を `Grid` 半分に分割します。 `BoxView`3 番目の行のは、両方の列にまたがります。

[![基本的なグリッドレイアウトのスクリーンショット](grid-images/basic.png "基本的なグリッドレイアウト")](grid-images/basic-large.png#lightbox "基本的なグリッドレイアウト")

また、内の子ビューでは、 [`Grid`](xref:Xamarin.Forms.Grid) セルを共有できます。 子が XAML に表示される順序は、子がに配置される順序です `Grid` 。 前の例では、オブジェクトは [`Label`](xref:Xamarin.Forms.Label) オブジェクトの上にレンダリングされるため、表示されるだけです [`BoxView`](xref:Xamarin.Forms.BoxView) 。 オブジェクトがオブジェクトの `Label` 上にレンダリングされた場合、オブジェクトは表示されません `BoxView` 。

これに相当する C# コードを次に示します。

```csharp
public class BasicGridPageCS : ContentPage
{
    public BasicGridPageCS()
    {
        Grid grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = new GridLength(2, GridUnitType.Star) },
                new RowDefinition(),
                new RowDefinition { Height = new GridLength(100) }
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        // The BoxView and Label are in row 0 and column 0, and so only needs to be added to the
        // Grid.Children collection to get default row and column settings.
        grid.Children.Add(new BoxView
        {
            Color = Color.Green
        });
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        });

        // This BoxView and Label are in row 0 and column 1, which are specified as arguments
        // to the Add method.
        grid.Children.Add(new BoxView
        {
            Color = Color.Blue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 0);

        // Row 1
        // This BoxView and Label are in row 1 and column 0, which are specified as arguments
        // to the Add method overload.
        grid.Children.Add(new BoxView
        {
            Color = Color.Teal
        }, 0, 1, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row 1, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1, 1, 2); // These arguments indicate that that the child element goes in the column starting at 0 but ending before 1.
                        // They also indicate that the child element goes in the row starting at 1 but ending before 2.

        grid.Children.Add(new BoxView
        {
            Color = Color.Purple
        }, 1, 2, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row1, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 2, 1, 2);

        // Row 2
        // Alternatively, the BoxView and Label can be positioned in cells with the Grid.SetRow
        // and Grid.SetColumn methods.
        BoxView boxView = new BoxView { Color = Color.Red };
        Grid.SetRow(boxView, 2);
        Grid.SetColumnSpan(boxView, 2);
        Label label = new Label
        {
            Text = "Row 2, Column 0 and 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };
        Grid.SetRow(label, 2);
        Grid.SetColumnSpan(label, 2);

        grid.Children.Add(boxView);
        grid.Children.Add(label);

        Title = "Basic Grid demo";
        Content = grid;
    }
}
```

コードでは、オブジェクトの高さとオブジェクトの幅を指定するために、 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 多くの場合、 [`GridLength`](xref:Xamarin.Forms.GridLength) 列挙と組み合わせて構造体の値を使用し [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) ます。

上のコード例では、に子を追加し、それらが存在するセルを指定するためのいくつかの異なる方法も示して [`Grid`](xref:Xamarin.Forms.Grid) います。 `Add` *Left*、 *right*、 *top*、および*bottom*の各引数を指定するオーバーロードを使用しているときに、 *left*および*top*引数が常に内のセルを参照しているときに、 `Grid` *右*と*下*の引数がの外部にあるセルを参照しているように見え `Grid` ます。 これは、 *right*引数は常に*左*の引数よりも大きくする必要があり、*下*の引数は常に*top*引数よりも大きくする必要があるためです。 次の例では、2x2 を前提とし `Grid` て、両方のオーバーロードを使用して同等のコードを示してい `Add` ます。

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);           // first column, first row
grid.Children.Add(topRight, 1, 0);          // second column, first tow
grid.Children.Add(bottomLeft, 0, 1);        // first column, second row
grid.Children.Add(bottomRight, 1, 1);       // second column, second row

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);     // first column, first row
grid.Children.Add(topRight, 1, 2, 0, 1);    // second column, first tow
grid.Children.Add(bottomLeft, 0, 1, 1, 2);  // first column, second row
grid.Children.Add(bottomRight, 1, 2, 1, 2); // second column, second row
```

> [!NOTE]
> さらに、子ビューをに追加するには、 [`Grid`](xref:Xamarin.Forms.Grid) メソッドとメソッドを使用します。これにより、 [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) [`AddVertical`](xref:Xamarin.Forms.Grid.IGridList`1.AddVertical*) 1 つの行または単一の列に子が追加され `Grid` ます。 これらの呼び出しが行われたときに、が `Grid` 行または列で展開され、適切なセルに子が自動的に配置されます。

### <a name="simplify-row-and-column-definitions"></a>行と列の定義を簡略化する

XAML では、 [`Grid`](xref:Xamarin.Forms.Grid) [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 行および列ごとにオブジェクトおよびオブジェクトを定義しなくても済む簡略化された構文を使用して、の行と列の特性を指定できます。 代わりに、 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) プロパティと [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) プロパティは、コンマ区切り値を含む文字列に設定でき [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) ます。これは、 Xamarin.Forms create `RowDefinition` オブジェクトおよび objects に組み込まれている型コンバーターからのもの `ColumnDefinition` です。

```xaml
<Grid RowDefinitions="1*, Auto, 25, 14, 20"
      ColumnDefinitions="*, 2*, Auto, 300">
    ...
</Grid>
```

この例では、に5つの [`Grid`](xref:Xamarin.Forms.Grid) 行と4つの列があります。 3番目の行と5番目の行は絶対高さに設定され、2番目の行はそのコンテンツに自動サイズ調整されます。 その後、残りの高さが最初の行に割り当てられます。

この列は絶対幅に設定され、3番目の列がそのコンテンツに自動サイズ調整されます。 残りの幅は、星の前の数値に基づいて、最初の列と2番目の列の間に比例して割り当てられます。 この例では、2番目の列の幅が最初の列の2倍になっています (はと同じであるため `*` `1*` )。

## <a name="space-between-rows-and-columns"></a>行と列の間のスペース

既定で [`Grid`](xref:Xamarin.Forms.Grid) は、行はデバイスに依存しない6つの領域単位で区切られます。 同様に、 `Grid` 列は、デバイスに依存しない6つの領域単位で区切られます。 これらの既定値は、 [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) プロパティとプロパティをそれぞれ設定することによって変更でき [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridSpacingPage"
             Title="Grid spacing demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        ..
    </Grid>
</ContentPage>
```

次の例では、 [`Grid`](xref:Xamarin.Forms.Grid) 行と列の間に空白を含まないを作成します。

[![セル間にスペースを使用しないグリッドのスクリーンショット](grid-images/spacing.png "セル間にスペースがないグリッド")](grid-images/spacing-large.png#lightbox "セル間にスペースがないグリッド")

> [!TIP]
> [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)プロパティと [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) プロパティを負の値に設定すると、セルの内容を重ねることができます。

これに相当する C# コードを次に示します。

```csharp
public GridSpacingPageCS()
{
    Grid grid = new Grid
    {
        RowSpacing = 0,
        ColumnSpacing = 0,
        // ...
    };
    // ...

    Content = grid;
}
```

## <a name="alignment"></a>アラインメント

の子ビューは、 [`Grid`](xref:Xamarin.Forms.Grid) プロパティとプロパティによってセル内に配置でき [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) ます。 これらのプロパティは、構造体から次のフィールドに設定でき [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) ます。

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)

> [!IMPORTANT]
> `AndExpands`構造体のフィールド [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) は、オブジェクトにのみ適用され [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

次の XAML は、 [`Grid`](xref:Xamarin.Forms.Grid) 9 個の同じサイズのセルを持つを作成し、それぞれの [`Label`](xref:Xamarin.Forms.Label) セルに異なる配置でを配置します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridAlignmentPage"
             Title="Grid alignment demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>

        <BoxView Color="AliceBlue" />
        <Label Text="Upper left"
               HorizontalOptions="Start"
               VerticalOptions="Start" />
        <BoxView Grid.Column="1"
                 Color="LightSkyBlue" />
        <Label Grid.Column="1"
               Text="Upper center"
               HorizontalOptions="Center"
               VerticalOptions="Start"/>
        <BoxView Grid.Column="2"
                 Color="CadetBlue" />
        <Label Grid.Column="2"
               Text="Upper right"
               HorizontalOptions="End"
               VerticalOptions="Start" />
        <BoxView Grid.Row="1"
                 Color="CornflowerBlue" />
        <Label Grid.Row="1"
               Text="Center left"
               HorizontalOptions="Start"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="DodgerBlue" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Center center"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="2"
                 Color="DarkSlateBlue" />
        <Label Grid.Row="1"
               Grid.Column="2"
               Text="Center right"
               HorizontalOptions="End"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Color="SteelBlue" />
        <Label Grid.Row="2"
               Text="Lower left"
               HorizontalOptions="Start"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="1"
                 Color="LightBlue" />
        <Label Grid.Row="2"
               Grid.Column="1"
               Text="Lower center"
               HorizontalOptions="Center"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="2"
                 Color="BlueViolet" />
        <Label Grid.Row="2"
               Grid.Column="2"
               Text="Lower right"
               HorizontalOptions="End"
               VerticalOptions="End" />
    </Grid>
</ContentPage>
```

この例では、各行 [`Label`](xref:Xamarin.Forms.Label) のオブジェクトはすべて垂直方向に整列されていますが、水平方向の配置は異なります。 別の方法として、 `Label` 各列のオブジェクトが水平方向に同一に配置されているが、異なる垂直方向の配置を使用していると考えることもできます。

[![グリッド内のセルの配置のスクリーンショット](grid-images/alignment.png "グリッド内のセルの配置")](grid-images/alignment-large.png#lightbox "グリッド内のセルの配置")

これに相当する C# コードを次に示します。

```csharp
public class GridAlignmentPageCS : ContentPage
{
    public GridAlignmentPageCS()
    {
        Grid grid = new Grid
        {
            RowSpacing = 0,
            ColumnSpacing = 0,
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        grid.Children.Add(new BoxView
        {
            Color = Color.AliceBlue
        });
        grid.Children.Add(new Label
        {
            Text = "Upper left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        });

        grid.Children.Add(new BoxView
        {
            Color = Color.LightSkyBlue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Start
        }, 1, 0);

        grid.Children.Add(new BoxView
        {
            Color = Color.CadetBlue
        }, 2, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Start
        }, 2, 0);

        // Row 1
        grid.Children.Add(new BoxView
        {
            Color = Color.CornflowerBlue
        }, 0, 1);
        grid.Children.Add(new Label
        {
            Text = "Center left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DodgerBlue
        }, 1, 1);
        grid.Children.Add(new Label
        {
            Text = "Center center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DarkSlateBlue
        }, 2, 1);
        grid.Children.Add(new Label
        {
            Text = "Center right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Center
        }, 2, 1);

        // Row 2
        grid.Children.Add(new BoxView
        {
            Color = Color.SteelBlue
        }, 0, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.End
        }, 0, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.LightBlue
        }, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.End
        }, 1, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.BlueViolet
        }, 2, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.End
        }, 2, 2);

        Title = "Grid alignment demo";
        Content = grid;
    }
}
```

## <a name="nested-grid-objects"></a>入れ子になった Grid オブジェクト

は、 [`Grid`](xref:Xamarin.Forms.Grid) 入れ子になった子 `Grid` オブジェクトまたはその他の子レイアウトを含む親レイアウトとして使用できます。 オブジェクトを入れ子にする場合、 `Grid` `Grid.Row` 、、 `Grid.Column` `Grid.RowSpan` 、およびの `Grid.ColumnSpan` 各添付プロパティは、常に親内のビューの位置を参照し `Grid` ます。

次の XAML は、オブジェクトの入れ子の例を示してい [`Grid`](xref:Xamarin.Forms.Grid) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:converters="clr-namespace:GridDemos.Converters"
             x:Class="GridDemos.Views.ColorSlidersGridPage"
             Title="Nested Grids demo">

    <ContentPage.Resources>
        <converters:DoubleToIntConverter x:Key="doubleToInt" />

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment"
                    Value="Center" />
        </Style>
    </ContentPage.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <BoxView x:Name="boxView"
                 Color="Black" />
        <Grid Grid.Row="1"
              Margin="20">
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
            </Grid.RowDefinitions>
            <Slider x:Name="redSlider"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="1"
                   Text="{Binding Source={x:Reference redSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0}'}" />
            <Slider x:Name="greenSlider"
                    Grid.Row="2"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="3"
                   Text="{Binding Source={x:Reference greenSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0}'}" />
            <Slider x:Name="blueSlider"
                    Grid.Row="4"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="5"
                   Text="{Binding Source={x:Reference blueSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Blue = {0}'}" />
        </Grid>
    </Grid>
</ContentPage>
```

この例では、ルート [`Grid`](xref:Xamarin.Forms.Grid) レイアウトの [`BoxView`](xref:Xamarin.Forms.BoxView) 最初の行にが、2番目の行に子が含まれてい `Grid` ます。 子に `Grid` は、に [`Slider`](xref:Xamarin.Forms.Slider) よって表示される色を操作するオブジェクト `BoxView` と、 [`Label`](xref:Xamarin.Forms.Label) それぞれの値を表示するオブジェクトが含まれ `Slider` ます。

[![入れ子になったグリッドのスクリーンショット](grid-images/nesting.png "入れ子になった Grid オブジェクト")](grid-images/nesting-large.png#lightbox "入れ子になった Grid オブジェクト")

> [!IMPORTANT]
> [`Grid`](xref:Xamarin.Forms.Grid)オブジェクトやその他のレイアウトの入れ子を深くするほど、入れ子になったレイアウトの方がパフォーマンスに影響します。 詳細については、「[適切なレイアウトを選択する](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout)」を参照してください。

これに相当する C# コードを次に示します。

```csharp
public class ColorSlidersGridPageCS : ContentPage
{
    BoxView boxView;
    Slider redSlider;
    Slider greenSlider;
    Slider blueSlider;

    public ColorSlidersGridPageCS()
    {
        // Create an implicit style for the Labels
        Style labelStyle = new Style(typeof(Label))
        {
            Setters =
            {
                new Setter { Property = Label.HorizontalTextAlignmentProperty, Value = TextAlignment.Center }
            }
        };
        Resources.Add(labelStyle);

        // Root page layout
        Grid rootGrid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition()
            }
        };

        boxView = new BoxView { Color = Color.Black };
        rootGrid.Children.Add(boxView);

        // Child page layout
        Grid childGrid = new Grid
        {
            Margin = new Thickness(20),
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            }
        };

        DoubleToIntConverter doubleToInt = new DoubleToIntConverter();

        redSlider = new Slider();
        redSlider.ValueChanged += OnSliderValueChanged;
        childGrid.Children.Add(redSlider);

        Label redLabel = new Label();
        redLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Red = {0}", source: redSlider));
        Grid.SetRow(redLabel, 1);
        childGrid.Children.Add(redLabel);

        greenSlider = new Slider();
        greenSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(greenSlider, 2);
        childGrid.Children.Add(greenSlider);

        Label greenLabel = new Label();
        greenLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Green = {0}", source: greenSlider));
        Grid.SetRow(greenLabel, 3);
        childGrid.Children.Add(greenLabel);

        blueSlider = new Slider();
        blueSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(blueSlider, 4);
        childGrid.Children.Add(blueSlider);

        Label blueLabel = new Label();
        blueLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Blue = {0}", source: blueSlider));
        Grid.SetRow(blueLabel, 5);
        childGrid.Children.Add(blueLabel);

        // Place the child Grid in the root Grid
        rootGrid.Children.Add(childGrid, 0, 1);

        Title = "Nested Grids demo";
        Content = rootGrid;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        boxView.Color = new Color(redSlider.Value, greenSlider.Value, blueSlider.Value);
    }
}
```

## <a name="related-links"></a>関連リンク

- [グリッドのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)
- [レイアウトオプションXamarin.Forms](layout-options.md)
- [レイアウトの選択 Xamarin.Forms](choose-layout.md)
- [アプリのパフォーマンスを向上させる Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
