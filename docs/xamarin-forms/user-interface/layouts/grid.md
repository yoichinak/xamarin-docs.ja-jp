---
title: グリッド
description: グリッド内のビューを提供します。
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: be1a3274ff4c9a15b7ee13c29d3176229a70642c
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2018
---
# <a name="grid"></a>グリッド

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) ビューの行と列に配置をサポートします。 比例してサイズまたは絶対サイズを持つ行と列を設定できます。 `Grid`レイアウトは、従来のテーブルと混同しないでし、表形式のデータを表示するものではありません。 `Grid` 行、列またはセルの書式設定の概念はありません。 HTML テーブルとは異なり`Grid`コンテンツをレイアウトするため、純粋な目的としています。

[![](grid-images/layouts-sml.png "Xamarin.Forms レイアウト")](grid-images/layouts.png#lightbox "Xamarin.Forms レイアウト")

この記事を取り上げます。

- **[目的](#Purpose)** &ndash;の一般的な使用`Grid`です。
- **[使用状況](#Usage)** &ndash;の使用方法`Grid`目的の設計を実現するためにします。
  - **[行と列](#Rows_and_Columns)** &ndash;の行と列の指定、`Grid`です。
  - **[ビューを配置する](#Placing_Views)** &ndash;ビューを特定の行と列のグリッドに追加します。
  - **[間隔](#Spacing)** &ndash;行と列の間のスペースを構成します。
  - **[スパン](#Spans)** &ndash;複数の行または列にまたがるように要素を構成します。

![](grid-images/grid.png "グリッドの探索")

## <a name="purpose"></a>目的

`Grid` グリッド ビューを配置するために使用します。 これは、機能は、ケースの数に便利です。

- 電卓アプリ内のボタンの配置
- IOS または Android のホーム画面のように、グリッドに並べたりボタン/選択肢
- 1 つのディメンション (一部のツールバーに似た) のサイズのように、ビューを配置します。

## <a name="usage"></a>使用法

従来のテーブルとは異なり`Grid`数およびコンテンツの行と列のサイズを推論することはありません。 代わりに、`Grid`が`RowDefinitions`と`ColumnDefinitions`コレクション。 これらは、行と列の数がレイアウトの定義を保持します。ビューに追加`Grid`行と列でビューを配置する必要がありますを識別する指定した行と列のインデックスでは、します。

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>行と列

行および列情報が格納されている`Grid`の`RowDefinitions`  &  `ColumnDefinitions`各コレクションであるプロパティの[ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/)と[ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)オブジェクトをそれぞれします。 `RowDefinition` 1 つのプロパティを持つ`Height`、および`ColumnDefinition`1 つのプロパティを持つ`Width`します。 高さと幅のオプションは次のとおりです。

- **自動**&ndash;行または列の内容に合わせて自動的にサイズ。 として指定された[ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) (C#) またはとして`Auto`XAML でします。
- **Proportional(*)** &ndash;残りの領域の割合として列および行のサイズを設定します。 値として指定し、 `GridUnitType.Star` C# の場合と`#*`XAML で`#`目的の値をされています。 行/列が 1 つを指定する`*`使用可能な領域を覆うようになります。
- **絶対**&ndash;固定、特定の高さと幅の値を持つ行と列のサイズを設定します。 値として指定し、 `GridUnitType.Absolute` C# の場合と`#`XAML で`#`目的の値をされています。

> [!NOTE]
> 列の幅の値として設定は、' * ' Xamarin.Forms で既定では、これにより、列が使用可能な領域いっぱいになります。

3 つの行と 2 つの列が必要とするアプリを検討してください。 一番下の行が縦 200px 正確である必要があり、先頭の行が中央の行と高さが同じ 2 回クリックする必要があります。 左の列が、コンテンツに合わせて十分な幅にする必要があり、右側の列が残りの領域を入力する必要があります。

XAML:

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
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>グリッド内のビューを配置します。

ビューを配置する、`Grid`グリッドに子として追加し、内で所属する行と列を指定する必要があります。

XAML では、次のように使用します。`Grid.Row`と`Grid.Column`配置を指定する各個々 のビューにします。 なお`Grid.Row`と`Grid.Column`行と列の 0 から始まるリストに基づく場所を指定します。 つまり、4 × 4 のグリッドで左上のセル (0, 0) は、右下のセル (3, 3)。

`Grid`表示次に示す 4 つのセルが含まれています。

![](grid-images/label-grid.png "4 つのビューを含むグリッド")

XAML:

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
grid.Children.Add(topRight, 1, 0;
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

上記のコードは、次の 4 つのラベル、2 つの列、および 2 つの行をグリッドを作成します。 各ラベルが同じサイズを持つ行をすべて使用可能な領域を使用する展開を注意してください。

上記の例ではビューに追加されます、 [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/)コレクションを使用して、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/)左と上の引数を指定するオーバー ロードします。 使用する場合、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/)左を指定するオーバー ロード、右、上部と下部にある引数、while、左と上の引数が常に内のセルを参照してください、 [ `Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)右側と外側にあるセルを参照する引数の下部にある場合があります、`Grid`です。 これは、右の引数を常に、左の引数よりも大きい値にする必要があります、下部にある引数を常に最上位の引数よりも大きい値にする必要があります。 次の例は、両方を使用して同等のコードを示しています。`Add`オーバー ロードします。

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

`Grid` 行と列の間隔を制御するプロパティがあります。  次のプロパティをカスタマイズするために使用できる、 `Grid`:

- **ColumnSpacing** &ndash;列間のスペースの量。
- **RowSpacing** &ndash;行の間の領域の量。

次の XAML を指定します、`Grid`で 2 つの列、1 行、および 5 px 列間の間隔。

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C# の場合:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>範囲

グリッドを使用する場合に多くの場合、要素がある 1 つ以上の行または列を占有する必要があります。 簡単な電卓アプリケーションを考えてみます。

![](grid-images/calculator.png "Calulator アプリケーション")

0 のボタンにまたがって表示される 2 つの列と同じように各プラットフォームの組み込みの電卓に注目してください。 これは、`ColumnSpan`プロパティは、要素が列の数で占有する必要がありますを指定します。 そのボタンの XAML:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

および C# の場合。

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

静的メソッドのコードを注意してください、`Grid`クラスを使用して変更を含む位置の変更を加えるを`ColumnSpan`と`RowSpan`です。 また、いつでも設定できるその他のプロパティとは異なり設定された静的メソッドを使用する必要があります既に、グリッドで前に変更されます。

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

両方のグリッドの上部にあるラベルとゼロ ボタンが occuping 以上の 1 つの列があることを確認します。 同様のレイアウトは、入れ子になったグリッドを使用して行うことも、 `ColumnSpan`  &  `RowSpan`アプローチの方が簡単です。

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

- [Xamarin.Forms、Chapter 17 を使用したモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [グリッド](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
