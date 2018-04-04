---
title: BoxView
description: 装飾、グラフィックス、および相互作用の色付きの四角形を使用します。
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 5ef55f9c4a747ef73d674fada71c3a92d0cf846a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="boxview"></a>BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 指定した幅、高さ、および色の単純な四角形を表示します。 使用することができます`BoxView`装飾、基本的なグラフィックス、およびタッチを介してユーザーと対話するためです。

Xamarin.Forms で組み込みのベクトル グラフィックス システムでは、があるないため、`BoxView`補正するために役立ちます。 この記事の使用中で説明されているサンプル プログラムのいくつか`BoxView`グラフィックのレンダリングにします。 `BoxView`サイズを特定の幅と太さの線のようにして角度を使用して、回転、`Rotation`プロパティです。

`BoxView`単純なグラフィックスを模倣、調査することができます[xamarin.forms を使用して SkiaSharp](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)のより高度なグラフィックス要件です。

この記事では、次のトピックについて説明します。

- **[BoxView 色とサイズを設定](#colorandsize)** &ndash;設定、`BoxView`プロパティです。
- **[レンダリング テキスト装飾](#textdecorations)** &ndash;を使用して、`BoxView`の線を表示します。
- **[BoxView で色を一覧表示する](#listingcolors)** &ndash;で、システム カラーをすべて表示、`ListView`です。
- **[サブクラス化 BoxView してゲームの寿命を再生](#subclassing)** &ndash;有名なオートマトンを実装します。
- **[デジタル時計を作成する](#digitalclock)** &ndash;ドット マトリックスの表示をシミュレートします。
- **[アナログ時計を作成する](#analogclock)** &ndash;変換し、アニメーション化`BoxView`要素。

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>設定 BoxView 色とサイズ

ほとんどの場合、次の 3 つのプロパティの設定`BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) 色を設定します。
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 幅を設定する、`BoxView`デバイスに依存しない単位。
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 高さを設定する、`BoxView`です。

`Color`プロパティの型は`Color`; プロパティは、いずれかに設定できます`Color`アルファベット順の範囲の色の名前付きの 141 静的読み取り専用フィールドを含む値、`AliceBlue`に`YellowGreen`です。

`WidthRequest`と`HeightRequest`プロパティは、場合のみ、役割を果たす、`BoxView`は*制約のない*レイアウトでします。 これは、レイアウト コンテナーを子 's、たとえば、する場合のサイズを知る必要がある場合の場合、`BoxView`で自動サイズ設定するセルの子である、`Grid`レイアウトです。 A`BoxView`もない制約されたときにその`HorizontalOptions`と`VerticalOptions`プロパティが以外の値に設定されます`LayoutOptions.Fill`です。 場合、 `BoxView` 、制約がありませんが、`WidthRequest`と`HeightRequest`プロパティが設定されていない、し、幅または高さが 40 単位、またはモバイル デバイスで約 1/4 インチの既定値に設定されます。

`WidthRequest`と`HeightRequest`プロパティが無視される場合、`BoxView`は*制約付き*レイアウトをレイアウト コンテナーの場合、自分のサイズに基づいてに課す、`BoxView`です。 

A `BoxView` 1 つのディメンション内で制限して、他の制約します。 たとえば場合、`BoxView`垂直方向の子である`StackLayout`の垂直方向、`BoxView`が制約し、水平方向に制限が一般にします。 水平方向の例外がありますが、: 場合、`BoxView`がその`HorizontalOptions`以外の何かに設定するプロパティ`LayoutOptions.Fill`、水平方向にも制約付きはできません。 ことも、`StackLayout`自体に制約なしの水平方向、その場合、`BoxView`も制約付きの水平方向にできません。

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)サンプルを表示、1 インチの正方形制約なし`BoxView`そのページの中央に。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center" 
             HorizontalOptions="Center" />

</ContentPage>
```

結果を次に示します。

[![基本的な BoxView](boxview-images/basicboxview-small.png "基本 BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

場合、`VerticalOptions`と`HorizontalOptions`プロパティはから削除、`BoxView`タグまたはに設定されている`Fill`、`BoxView`ページのサイズによって制限が、ページ全体に拡張されます。

A`BoxView`の子であることができますも、`AbsoluteLayout`です。 その場合、両方の場所のサイズと、`BoxView`使用して設定された、`LayoutBounds`添付バインド可能なプロパティです。 `AbsoluteLayout`記事で説明した[ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md)です。

次のサンプル プログラムでこれらすべてのケースの例が表示されます。

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>レンダリング テキスト装飾

使用することができます、`BoxView`を水平方向および垂直の線の形式で、ページのいくつかの単純な装飾を追加します。 [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)サンプルを示します。 定義されているすべてのプログラムのビジュアルは、 **MainPage.xaml**をいくつかを含むファイル`Label`と`BoxView`内の要素、`StackLayout`次に示します。

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

子であるすべてのマークアップに続く、`StackLayout`です。 装飾用の複数の型は、このマークアップ`BoxView`で使用される要素、`Label`要素。

[![文字飾り](boxview-images/textdecoration-small.png "文字飾り")](boxview-images/textdecoration-large.png#lightbox "テキスト装飾")

ページの上部にあるスタイリッシュ ヘッダーが得られます、`AbsoluteLayout`子は、4 つ`BoxView`要素と`Label`、すべては割り当ての特定の場所とサイズ。

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

XAML ファイルで、`AbsoluteLayout`が続く、`Label`で説明するテキストを書式設定、`AbsoluteLayout`です。

それを囲む両方によってテキスト文字列に下線を追加することができます、`Label`と`BoxView`で、`StackLayout`を持つその`HorizontalOptions`以外の値が何かに設定`Fill`です。 幅、`StackLayout`の幅に従いますが、 `Label`、しに幅を課す、`BoxView`です。 `BoxView`明示的な高さのみが割り当てられます。

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

この手法を使用して、長いテキスト文字列または段落内の個々 の単語に下線を付けることはできません。

使用することも、`BoxView`を HTML のように`hr`(水平線) 要素です。 自動的の幅、`BoxView`このケースでは、親コンテナーによって決定されます、 `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

それを囲む両方によって、テキストの段落の一方の側で垂直線を描画する最後に、`BoxView`と`Label`水平方向に`StackLayout`です。 この場合の高さ、`BoxView`の高さと同じ`StackLayout`、これはの高さを受ける、 `Label`: 

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>BoxView で色を一覧表示します。

`BoxView`色を表示するため便利です。 このプログラムを使用して、`ListView`パブリック静的な読み取り専用のフィールドはすべて、Xamarin.Forms を一覧表示する`Color`構造体。

[![ListView 色](boxview-images/listviewcolors-small.png "ListView 色")](boxview-images/listviewcolors-large.png#lightbox "ListView の色")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/)プログラムには、という名前のクラスが含まれています。`NamedColor`です。 静的コンス トラクターのすべてのフィールドにアクセスするリフレクションを使用して、`Color`構造体を作成、`NamedColor`それぞれのオブジェクト。 これらは、静的に格納されている`All`プロパティ。

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

XAML ファイルでは、プログラムのビジュアルがについて説明します。 `ItemsSource`のプロパティ、 `ListView` 、静的に設定されている`NamedColor.All`プロパティ、つまり、 `ListView` 、すべての個々 の表示`NamedColor`オブジェクト。 

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

`NamedColor`オブジェクトが書式設定される、`ViewCell`のデータ テンプレートとして設定されているオブジェクト、`ListView`です。 このテンプレートに含まれる、`BoxView`が`Color`プロパティにバインドされる、`Color`のプロパティ、`NamedColor`オブジェクト。

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>サブクラス化 BoxView によってライフのゲームで遊ぶ

ゲームの寿命は数学的 John Conway により考案およびのページで普及したオートマトン*科学 American* 1970 年代にします。 概要は、Wikipedia の記事「によって提供される[Conway のゲームの寿命](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)です。

Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/)プログラムという名前のクラスを定義する`LifeCell`から派生した`BoxView`です。 このクラスは、ゲームの有効期間内の各セルのロジックをカプセル化します。

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

`LifeCell` 次の 3 つ以上のプロパティを追加`BoxView`:`Col`と`Row`プロパティが、グリッド内のセルの位置を格納および`IsAlive`プロパティの状態を示します。 `IsAlive`プロパティも設定、`Color`のプロパティ、`BoxView`黒、セルが有効でない場合、セルがアライブと白の場合にします。

`LifeCell` また、インストール、`TapGestureRecognizer`をタップすることによって、セルの状態を切り替えることを許可します。 クラスの変換、`Tapped`独自にジェスチャ レコグナイザーからイベント`Tapped`イベント。

**GameOfLife**プログラムも含まれています、`LifeGrid`ほとんどのゲームのロジックをカプセル化するクラスと`MainPage`プログラムのビジュアルを処理するクラスです。 これらには、ゲームのルールを示すオーバーレイが含まれます。 数百のいくつかを示すアクションで、プログラムを次に示します`LifeCell`ページ上のオブジェクト。

[![有効期間のゲーム](boxview-images/gameoflife-small.png "人生ゲーム")](boxview-images/gameoflife-large.png#lightbox "人生ゲーム")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>デジタル時計を作成します。

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/)プログラム作成 210`BoxView`昔ながらの 5 ~ 7 をドット マトリックス ディスプレイのドットをシミュレートする要素。 縦方向または横モードでの時間を読み取ることができますが、ランドス ケープが大きい。

[![ドット マトリックス クロック](boxview-images/dotmatrixclock-small.png "ドット マトリックス クロック")](boxview-images/dotmatrixclock-large.png#lightbox "ドット マトリックス クロック")

XAML ファイルをインスタンス化がよりは少し多く、`AbsoluteLayout`時計のために使用します。

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

他のすべては、分離コード ファイル内に発生します。 ドット マトリックス表示ロジックは 10 桁とコロンのそれぞれに対応するドットを記述する複数の配列の定義によって大幅に簡略化します。

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

これらのフィールドの最後の 3 次元配列に`BoxView`6 桁のドット パターンを格納するための要素。

コンス トラクターをすべて作成、 `BoxView` 、数字、コロン、およびも初期化用の要素、`Color`のプロパティ、`BoxView`コロンの要素。

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

このプログラムは、相対位置とサイズ変更機能の`AbsoluteLayout`します。 幅と高さをそれぞれの`BoxView`水平および垂直方向のドット数で割った値 1 の 85% を具体的には、小数部の値に設定されます。 位置は、小数部の値にも設定されます。 

すべての位置とサイズの合計サイズの基準としたがあるため、 `AbsoluteLayout`、 `SizeChanged` 、ページのハンドラーを設定のみ必要があります、`HeightRequest`の`AbsoluteLayout`:

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

幅、`AbsoluteLayout`のため、ページの幅全体に広がることが自動的に設定します。

最終的なコード、`MainPage`クラスは、タイマー コールバックを処理し、各桁のピリオドの色します。 プログラムの最も簡単な部分にこのロジックを作成、分離コード ファイルの先頭に多次元配列の定義に役立ちます。

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

ドット マトリックス クロックがのアプリケーションが明らかになるように思えます`BoxView`が`BoxView`要素では、アナログ時計を実現できます。

[![BoxView クロック](boxview-images/boxviewclock-small.png "BoxView クロック")](boxview-images/boxviewclock-large.png#lightbox "BoxView クロック")

内のすべてのビジュアル、 [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/)プログラムの子である、`AbsoluteLayout`です。 使用してこれらの要素のサイズは、`LayoutBounds`添付プロパティ、および回転を使用して、`Rotation`プロパティです。 

3 つ`BoxView`時計の針の要素の XAML ファイルでインスタンス化が、配置されたりしないサイズします。

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

分離コード ファイルのコンス トラクターは、60 をインスタンス化`BoxView`目盛りは、クロックの円周上での要素。

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

サイズとすべての配置、`BoxView`で要素が発生した、`SizeChanged`のハンドラーは、`AbsoluteLayout`です。 少し構造体、クラスの内部と呼ばれる`HandParams`クロックの合計サイズの基準とした 3 つのハンドのそれぞれのサイズを説明します。

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

`SizeChanged`ハンドラー center との半径を決定する、 `AbsoluteLayout`、およびサイズを設定し、され、60 が置か`BoxView`目盛りとして使用される要素。 `for`ループが終了を設定して、`Rotation`これらの各プロパティ`BoxView`要素。 最後に、`SizeChanged`ハンドラー、`LayoutHand`サイズおよび位置情報、クロックの 3 つの手をメソッドが呼び出されます。

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

`LayoutHand`メソッドがサイズを設定し、12時 00分位置まで直接指す各ハンドを配置します。 メソッドの最後に、`AnchorY`プロパティは、クロックの中央に対応する位置に設定します。 これは、回転の中心を示します。

手は、タイマー コールバック関数に回転します。

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

秒針は少し異なる方法で扱われます: 滑らかなのではなく、機械的ないように見える場合、移動するイージング機能アニメーションを適用します。 各ティック秒針少し戻るをプルし、送信先を所定します。 少しのコードは、移動のよりリアルに多く追加します。

## <a name="conclusion"></a>まとめ

`BoxView`するとしては最初に、単純に見えるかもしれませんこれまで見てきたことができます、非常に多様なグラフィックスおよびベクター グラフィックスをのみで通常使用できる再現ビジュアルほぼことができます。 高度なグラフィックスを参照してください[xamarin.forms を使用して SkiaSharp](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)です。


## <a name="related-links"></a>関連リンク

- [基本的な BoxView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [文字飾り (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [色のリスト ボックス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [ゲームの有効期間 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [ドット マトリックス クロック (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView クロック (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)
