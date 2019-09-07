---
title: Xamarin.Forms BoxView
description: この記事では、装飾、グラフィック、および Xamarin.Forms アプリケーションでの操作を色付きの四角形を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/26/2018
ms.openlocfilehash: 2da2af2a57fb0ec737927024d497530c2a3aac5b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759879"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)

[`BoxView`](xref:Xamarin.Forms.BoxView) 指定した幅、高さ、および色の単純な四角形をレンダリングします。 使用することができます`BoxView`の装飾は、基本的なグラフィックス、およびタッチ機能によるユーザーとの対話します。

Xamarin.Forms には、組み込みのベクター グラフィックス システムがあるないため、`BoxView`補正するために役立ちます。 この記事で使用中で説明されているサンプル プログラムの一部`BoxView`グラフィックのレンダリングにします。 `BoxView`と、特定の幅と太さの線と同じようにサイズを使用して、角度で回転し、`Rotation`プロパティ。

`BoxView`単純なグラフィックスを模倣することができます、調査したい場合があります[Xamarin.Forms で SkiaSharp を使用して](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)のより高度なグラフィックスの要件。

この記事では、次のトピックについて説明します。

- **[BoxView 色とサイズ設定](#colorandsize)** &ndash;設定、`BoxView`プロパティ。
- **[文字装飾のレンダリング](#textdecorations)** &ndash;を使用して、`BoxView`の線を表示します。
- **[BoxView 色のカラーを一覧表示する](#listingcolors)** &ndash;で、システム カラーをすべて表示、`ListView`します。
- **[サブクラス化 BoxView でゲームの人生を再生](#subclassing)** &ndash;有名なオートマトンを実装します。
- **[デジタル時計を作成する](#digitalclock)** &ndash;ドット マトリックスの表示をシミュレートします。
- **[作成、アナログ時計](#analogclock)** &ndash;変換およびアニメーション化`BoxView`要素。

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>設定 BoxView 色とサイズ

通常の次のプロパティを設定します`BoxView`:

- [`Color`](xref:Xamarin.Forms.BoxView.Color) その色を設定します。
- [`CornerRadius`](xref:Xamarin.Forms.BoxView.CornerRadius) その角の半径を設定します。
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 幅を設定する、`BoxView`デバイスに依存しない単位。
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 高さを設定する、`BoxView`します。

`Color`プロパティの型は`Color`; に、プロパティを設定できます`Color`値の 141 静的読み取り専用フィールドを含む名前付きの色からアルファベット順に至るまで`AliceBlue`に`YellowGreen`します。

`CornerRadius`プロパティの型は[ `CornerRadius` ](xref:Xamarin.Forms.CornerRadius); プロパティは、1 つに設定できます`double`角の半径の値を uniform または`CornerRadius`4 で定義された構造体`double`に適用される値左、右上、左、下の右下、`BoxView`します。

`WidthRequest`と`HeightRequest`プロパティは、場合のみ、役割を果たす、`BoxView`は*制約のない*レイアウトにします。 これは、場合、レイアウト コンテナーは、子 's、たとえば、する場合のサイズを把握する必要がある場合、`BoxView`で自動サイズ設定するセルの子である、`Grid`レイアウト。 A`BoxView`でないことも制限されたときにその`HorizontalOptions`と`VerticalOptions`プロパティが以外の値に設定されて`LayoutOptions.Fill`します。 場合、 `BoxView` 、制約がありませんが、`WidthRequest`と`HeightRequest`プロパティが設定されていない、し、幅または高さが 40 単位、またはモバイル デバイスに約 1/4 インチの既定値に設定します。

`WidthRequest`と`HeightRequest`プロパティが無視される場合、`BoxView`は*制約付き*をレイアウト コンテナーの場合に独自のサイズを課すレイアウトでは、`BoxView`します。

A `BoxView` 1 つのディメンションでは制限し、制約、それ以外のことができます。 たとえば場合、`BoxView`垂直方向の子である`StackLayout`の垂直方向、`BoxView`は制約し、水平方向は一般に制約付き。 ただし、その水平ディメンションには例外があります。の`BoxView` プロパティ`HorizontalOptions`が以外`LayoutOptions.Fill`の値に設定されている場合、水平ディメンションも制約されません。 ことも、`StackLayout`自体に制約なしの水平方向、その場合、`BoxView`も水平方向に制約されません。

[ **BasicBoxView** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)サンプル表示を 1 インチの正方形無制限`BoxView`そのページの中央に。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             CornerRadius="10"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

結果を次に示します。

[![基本的な BoxView](boxview-images/basicboxview-small.png "基本的な BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

場合、`VerticalOptions`と`HorizontalOptions`プロパティから削除されて、`BoxView`タグを付けるかに設定されます`Fill`、`BoxView`ページのサイズによって制約になり、ページ全体に展開します。

A`BoxView`の子にすることもできます、`AbsoluteLayout`します。 その場合、両方の場所とサイズの`BoxView`を使用して設定、`LayoutBounds`添付プロパティのバインド可能な。 `AbsoluteLayout`については、情報の記事で説明[ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md)します。

次のサンプル プログラムでこれらすべてのケースの例を確認します。

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>文字装飾のレンダリング

使用することができます、`BoxView`水平および垂直の線の形式で、ページの単純ないくつか飾りを追加します。 [ **TextDecoration** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)のサンプルで例示します。 定義されているすべてのプログラムのビジュアルは、 **MainPage.xaml**をいくつかを含むファイル`Label`と`BoxView`内の要素、`StackLayout`次に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

子であるすべてのマークアップに続く、`StackLayout`します。 このマークアップから成る複数の種類の装飾的な`BoxView`で使用される要素、`Label`要素。

[![テキスト装飾](boxview-images/textdecoration-small.png "装飾")](boxview-images/textdecoration-large.png#lightbox "テキスト装飾")

ページの上部にあるスタイリッシュなヘッダーにより実現されます、`AbsoluteLayout`子とは、4 つ`BoxView`要素と`Label`、すべては、特定の場所とサイズを割り当てます。

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

XAML ファイルで、`AbsoluteLayout`が続く、`Label`で説明するテキストを書式設定、`AbsoluteLayout`します。

両方を囲むことで、テキスト文字列に下線を追加することができます、`Label`と`BoxView`で、`StackLayout`を持つその`HorizontalOptions`以外の値が何かに設定`Fill`します。 幅、`StackLayout`の幅は、準拠するもの、 `Label`、そこにその幅を課す、`BoxView`します。 `BoxView`明示的な高さのみが割り当てられます。

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

この手法を使用して、長いテキスト文字列または段落内の個々 の単語に下線を付けることはできません。

使用することも、`BoxView`を HTML のように`hr`(水平方向の規則) の要素。 幅で自動的に、`BoxView`ここでは、親コンテナーによって決定されます、 `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

両方を囲むことで、テキストの段落の 1 つにある垂直線を描画する最後に、`BoxView`と`Label`を横`StackLayout`します。 ここでの高さ、`BoxView`の高さと同じ`StackLayout`の高さが適用される`Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```

<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>色のカラー BoxView を一覧表示します。

`BoxView`色を表示するため便利です。 このプログラムを使用して、`ListView`すべて静的な読み取り専用のパブリック フィールド、Xamarin.Forms を一覧表示する`Color`構造体。

[![ListView 色](boxview-images/listviewcolors-small.png "ListView 色")](boxview-images/listviewcolors-large.png#lightbox "ListView の色")

[ **ListViewColors** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors)プログラムには、という名前のクラスが含まれています。`NamedColor`します。 静的コンス トラクターのすべてのフィールドにアクセスするリフレクションを使用して、`Color`構造体し、作成、`NamedColor`それぞれのオブジェクト。 静的に格納される`All`プロパティ。

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

XAML ファイルでは、プログラムのビジュアルがについて説明します。 `ItemsSource`のプロパティ、 `ListView` 、静的に設定されている`NamedColor.All`プロパティ、つまり、`ListView`すべてを表示します。`NamedColor`オブジェクト。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

`NamedColor`オブジェクトが書式設定される、`ViewCell`オブジェクトのデータ テンプレートとして設定されている、`ListView`します。 このテンプレートに含まれて、`BoxView`が`Color`プロパティにバインドする、`Color`のプロパティ、`NamedColor`オブジェクト。

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>サブクラス化 BoxView でライフ サイクルのゲームをプレイ

ゲームの人生はオートマトンちなんで John Conway により考案およびのページで普及*Scientific American* 1970 年代にします。 Wikipedia の記事で概要が提供される[Conway のゲームの人生](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)します。

Xamarin.Forms [ **GameOfLife** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)プログラムという名前のクラスを定義する`LifeCell`から派生した`BoxView`します。 このクラスは、個々 のセルのライフ サイクルのゲームのロジックをカプセル化します。

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell` 次の 3 つのプロパティを追加します`BoxView`:`Col`と`Row`プロパティは、グリッド内のセルの位置を格納し、`IsAlive`プロパティの状態を示します。 `IsAlive`プロパティも設定、`Color`のプロパティ、`BoxView`を黒、セルが有効でない場合、セルが生きてと白の場合。

`LifeCell` また、インストール、`TapGestureRecognizer`をタップすることによって、セルの状態を切り替えることを許可します。 クラスは、変換、`Tapped`イベントに独自のジェスチャ認識エンジンから`Tapped`イベント。

**GameOfLife**プログラムも含まれています、 `LifeGrid` 、ゲームのロジックの大部分をカプセル化するクラスと`MainPage`プログラムのビジュアルを処理するクラスです。 ゲームのルールを示すオーバーレイが含まれます。 数百のいくつかを示すアクションのプログラムを次に示します`LifeCell`ページ上のオブジェクト。

[![ライフ サイクルのゲーム](boxview-images/gameoflife-small.png "人生ゲーム")](boxview-images/gameoflife-large.png#lightbox "ライフ サイクルのゲーム")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>デジタル時計を作成します。

[ **DotMatrixClock** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)プログラム作成 210`BoxView`昔ながらの 5 ~ 7 でドット マトリックス ディスプレイのドットをシミュレートする要素。 縦または横モードでの時間を読み取ることができますが、ランドス ケープが大きい。

[![ドット マトリックス クロック](boxview-images/dotmatrixclock-small.png "ドット マトリックス クロック")](boxview-images/dotmatrixclock-large.png#lightbox "ドット マトリックス クロック")

XAML ファイルをインスタンス化は少々、`AbsoluteLayout`時計のために使用します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

他のすべては、分離コード ファイルで発生します。 ドット マトリックスの表示ロジックは 10 桁とコロンのそれぞれに対応する点を記述する複数の配列の定義によって大幅に簡略化します。

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1},
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0},
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0},
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0},
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0},
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1},
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

これらのフィールドの最後の 3 次元配列に`BoxView`6 桁のドットのパターンを格納するための要素。

コンス トラクターをすべて作成、 `BoxView` 、桁の数字とコロン、および初期化の要素、`Color`のプロパティ、`BoxView`コロン要素。

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

このプログラムは、相対位置とサイズ変更機能の`AbsoluteLayout`します。 幅と高さをそれぞれの`BoxView`水平および垂直のドット数で割った値 1 の 85% を具体的には、小数部の値に設定されます。 位置は、小数部の値にも設定されます。

すべての位置とサイズはの合計サイズを基準とするため、 `AbsoluteLayout`、`SizeChanged`ページのハンドラーが設定のみが必要、`HeightRequest`の`AbsoluteLayout`:

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }

    ···

}
```

幅、`AbsoluteLayout`はページの幅に収まらないほど長くなるため、自動的に設定します。

最後のコードで、`MainPage`クラスがタイマーのコールバックを処理し、各桁の点の色します。 このロジックのプログラムの最も簡単な部分を作成、分離コード ファイルの先頭に多次元配列の定義に役立ちます。

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```

<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>アナログ時計の作成

明確なアプリケーションのドット マトリックス クロックが見えるかもしれません`BoxView`が`BoxView`要素は、アナログ時計の実現も。

[![BoxView クロック](boxview-images/boxviewclock-small.png "BoxView クロック")](boxview-images/boxviewclock-large.png#lightbox "BoxView クロック")

内のすべてのビジュアル、 [ **BoxViewClock** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)プログラムの子である、`AbsoluteLayout`します。 使用してこれらの要素のサイズは、`LayoutBounds`添付プロパティ、および回転を使用して、`Rotation`プロパティ。

3 つ`BoxView`時計の針の要素は、XAML ファイル内でインスタンス化がいない配置またはサイズします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">

        <BoxView x:Name="hourHand"
                 Color="Black" />

        <BoxView x:Name="minuteHand"
                 Color="Black" />

        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

分離コード ファイルのコンス トラクターは、60 をインスタンス化`BoxView`時計の外周部に目盛りの要素。

```csharp
public partial class MainPage : ContentPage
{

    ···

    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }

    ···

}
```

サイズとすべての配置、`BoxView`で要素が発生した、`SizeChanged`のハンドラー、`AbsoluteLayout`します。 クラスの内部の小さな構造体と呼ばれる`HandParams`クロックの合計サイズを基準と 3 つのハンドのそれぞれのサイズについて説明します。

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);

    ···

 }
```

`SizeChanged` Center との半径を決定するハンドラー、 `AbsoluteLayout`、60 の位置し、サイズを変更し、`BoxView`目盛りとして使用される要素。 `for`ループの最後を設定して、`Rotation`これらの各プロパティ`BoxView`要素。 最後に、`SizeChanged`ハンドラー、`LayoutHand`サイズし、位置のクロックの 3 つの手をメソッドが呼び出されます。

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }

    ···

}
```

`LayoutHand`メソッドは、サイズを変更し、12時 00分まで直接ポイントする各ハンドを配置します。 メソッドの最後に、`AnchorY`プロパティが、クロックの中心に対応する位置に設定します。 これは、回転の中心を示します。

タイマーのコールバック関数では、手が回転します。

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

2番目の手の扱いは少し異なります。アニメーションイージング関数は、動きが smooth ではなく機械に見えるようにするために適用されます。 刻みごとに、2 番目の手札を少し戻るをプルし、送信先を所定します。 この少しのコードは、動きのリアリティをたくさん追加します。

## <a name="conclusion"></a>まとめ

`BoxView`としてするが、最初で単純に見えますが、きわめて用途が広くなりますグラフィックスやベクター グラフィックスをのみで通常使用できるビジュアルほぼを再現できる説明しました。 高度なグラフィックスを参照してください。 [Xamarin.Forms で SkiaSharp を使用して](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)します。

## <a name="related-links"></a>関連リンク

- [基本的な BoxView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)
- [文字の装飾 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)
- [ListView の色 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)
- [ゲームの有効期間 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)
- [ドット マトリックス クロック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)
- [BoxView クロック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)
- [BoxView](xref:Xamarin.Forms.BoxView)
