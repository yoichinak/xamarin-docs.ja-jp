---
title: Xamarin.Forms のグリッド
description: この記事では、Xamarin.Forms グリッド クラスを使用して、グリッドで、行と列を持つでビューを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 1d445e3ef8869c74f052eb1153774dfab51ffd45
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649596"
---
# <a name="xamarinforms-grid"></a>Xamarin.Forms のグリッド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

[`Grid`](xref:Xamarin.Forms.Grid) ビューの行と列に配置をサポートします。 比例サイズや絶対サイズを持つ行と列を設定できます。 `Grid`レイアウトは、従来のテーブルに混同しない必要があり、表形式のデータを提示するものではありません。 `Grid` 行、列またはセルの書式の概念はありません。 HTML のテーブルとは異なり`Grid`コンテンツをレイアウトするためのものが純粋です。

[![](grid-images/layouts-sml.png "Xamarin.Forms のレイアウト")](grid-images/layouts.png#lightbox "Xamarin.Forms のレイアウト")

この記事では説明します。

- **[目的](#purpose)** &ndash;の一般的な使用`Grid`します。
- **[使用状況](#usage)** &ndash;を使用する`Grid`目的の設計を実現するためにします。
  - **[行と列](#rows-and-columns)** &ndash;行と列の指定、`Grid`します。
  - **[ビューを配置する](#placing-views-in-a-grid)** &ndash;ビューを特定の行と列のグリッドに追加します。
  - **[間隔](#spacing)** &ndash;行と列の間のスペースを構成します。
  - **[スパン](#spans)** &ndash;複数の行または列にまたがるように要素を構成します。

![](grid-images/grid.png "グリッド探索")

## <a name="purpose"></a>目的

`Grid` グリッド ビューを配置するために使用できます。 これはさまざまなケースで役立ちます。

- 電卓アプリ内のボタンの配置
- IOS または Android のホーム画面のように、グリッドに整列ボタン/選択肢
- 1 つのディメンション (一部のツールバーのように) に等しいサイズのように、ビューを配置します。

## <a name="usage"></a>使用法

従来のテーブルとは異なり`Grid`コンテンツから行と列のサイズと数を推論することはありません。 代わりに、`Grid`が`RowDefinitions`と`ColumnDefinitions`コレクション。 これらは、行と列の数がレイアウトの定義を保持します。ビューに追加されます`Grid`行と列でビューを配置する必要がありますを識別する指定された行および列のインデックスを使用します。

### <a name="rows-and-columns"></a>行と列

行および列情報が格納されている`Grid`の`RowDefinitions`  &  `ColumnDefinitions`は各コレクションのプロパティの[ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition)と[ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)オブジェクトをそれぞれします。 `RowDefinition` 1 つのプロパティを持つ`Height`と`ColumnDefinition`1 つのプロパティを持つ`Width`します。 高さと幅のオプションは次のとおりです。

- **自動**&ndash;行または列の内容に合わせて自動的にサイズ。 として指定された[ `GridUnitType.Auto` ](xref:Xamarin.Forms.GridUnitType) (C#) またはとして`Auto`XAML でします。
- **Proportional(*)** &ndash;残りの領域の割合としての列と行のサイズを設定します。 値として指定し、 `GridUnitType.Star` (C#) と`#*`、XAML で`#`目的の値をされています。 1 つの行/列を指定する`*`使用可能な領域をいっぱいになるようになります。
- **絶対**&ndash;列と、特定の固定の高さと幅の値を持つ行のサイズを設定します。 値として指定し、 `GridUnitType.Absolute` (C#) と`#`、XAML で`#`目的の値をされています。

> [!NOTE]
> 列の幅の値として設定`*`Xamarin.Forms で既定では、することにより、列が使用可能なスペースを埋めることです。 行の高さの値として設定されて`*`既定。

次の 3 つの行と 2 つの列が必要なアプリケーションを考えてみます。 一番下の行を正確に 200px (縦) をある必要があり、一番上の行は、中央の行と高さが同じである 2 回必要があります。 左側の列に合わせたコンテンツ幅にする必要があるし、右側の列は、残りのスペースを埋める必要があります。

で XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C# の場合:

```csharp
Grid grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Auto) });
```

### <a name="placing-views-in-a-grid"></a>グリッド ビューを配置します。

ビューを配置する、`Grid`をグリッドに子として追加しに属している行と列を指定する必要があります。

XAML を使用して`Grid.Row`と`Grid.Column`配置を指定する各個々 のビュー。 なお`Grid.Row`と`Grid.Column`行と列の 0 から始まるリストに基づく場所を指定します。 つまり、4 × 4 のグリッドで右下のセルは (3, 3)、左上のセルは (0, 0)。

`Grid`に示す次の 4 つのセルが含まれています。

![](grid-images/label-grid.png "4 つのビューとグリッド")

で XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

C# の場合:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

上記のコードでは、4 つのラベル、2 つの列では、2 つの行とグリッドを作成します。 各ラベルが同じサイズにあるし、行をすべて使用可能な領域を使用する展開はことに注意してください。

上記の例ではビューに追加されます、 [ `Grid.Children` ](xref:Xamarin.Forms.Grid.Children)コレクションを使用して、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/)左と上の引数を指定するオーバー ロードします。 使用する場合、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) left を指定するオーバー ロード、右、上、および、下部にある引数の中に、左と上の引数は、常に内のセルを参照、 [ `Grid`](xref:Xamarin.Forms.Grid)右と外部にあるセルを参照する引数の下部にある場合があります、`Grid`します。 これは、右の引数は、左の引数より大きい必ず下部にある引数を最上位の引数よりも大きい必ずためです。 次の例は、両方を使用して同等のコードを示しています。`Add`オーバー ロードします。

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>スペース

`Grid` 行と列の間隔を制御するプロパティがあります。 次のプロパティをカスタマイズするために使用できる、 `Grid`:

- **ColumnSpacing** &ndash;列間のスペース量。 このプロパティの既定値は、6 です。
- **間隔を広げる**&ndash;行の間の領域の量。 このプロパティの既定値は、6 です。

次の XAML を指定します、`Grid`で 2 つの列、1 行、および 5 px 列間の間隔。

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C# の場合:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>範囲

グリッドを使用する場合に多くの場合は、要素を 1 つ以上の行または列を占有する必要があります。 簡単な電卓アプリケーションを検討してください。

![](grid-images/calculator.png "Calulator アプリケーション")

0 のボタンにまたがって表示される 2 つの列と同じように各プラットフォーム用の組み込みの計算ツールに注目してください。 これは、`ColumnSpan`プロパティで、要素が列の数が占有するを指定します。 そのボタンの XAML:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

および C# の場合。

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

静的メソッドのコードを注意してください、`Grid`クラスへの変更をなどの変更を配置する際に使用`ColumnSpan`と`RowSpan`します。 また、いつでも設定できるその他のプロパティとは異なりプロパティの静的メソッドを使用して設定する必要が既に、グリッドで前に、変更されました。

上記の電卓アプリケーションの完全な XAML は次のとおりです。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

両方のグリッドの上部にあるラベルと 0 個、ボタンが occuping 複数の列があることを確認します。 入れ子になったグリッドを使用して同様のレイアウトを達成できるが、 `ColumnSpan`  &  `RowSpan`アプローチの方が簡単です。

C# で実装します。

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>関連リンク

- [第 17 章である Xamarin.Forms によるモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [グリッド](xref:Xamarin.Forms.Grid)
- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
