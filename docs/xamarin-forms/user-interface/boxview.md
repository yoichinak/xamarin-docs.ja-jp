---
title: Xamarin.FormsBoxView
description: この記事では、アプリケーションでの装飾、グラフィックス、および対話に色付きの四角形を使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3f4788c0201d2d286ff4de9b29ba6385d323a3b0
ms.sourcegitcommit: c3329ab25d377907d8804cdd5e26dc84a274f39c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88130943"
---
# <a name="no-locxamarinforms-boxview"></a>Xamarin.FormsBoxView

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)

[`BoxView`](xref:Xamarin.Forms.BoxView)指定した幅、高さ、および色の単純な四角形を描画します。 `BoxView`装飾、基本的なグラフィックス、およびタッチによるユーザーとの対話には、を使用できます。

に Xamarin.Forms はベクターグラフィックスシステムが組み込まれていないため、は `BoxView` 補正に役立ちます。 この記事に記載されているサンプルプログラムの一部は、 `BoxView` グラフィックスのレンダリングに使用されます。 は、 `BoxView` 特定の幅と太さの線に似たサイズにすることができ、プロパティを使用して任意の角度で回転でき `Rotation` ます。

`BoxView`は単純なグラフィックスを模倣できますが、より高度なグラフィックス要件については、 [ Xamarin.Forms の SkiaSharp を使用して](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)調査することをお勧めします。

## <a name="setting-boxview-color-and-size"></a>BoxView の色とサイズの設定

通常は、の次のプロパティを設定し `BoxView` ます。

- [`Color`](xref:Xamarin.Forms.BoxView.Color)色を設定する場合は。
- [`CornerRadius`](xref:Xamarin.Forms.BoxView.CornerRadius)角の半径を設定する場合は。
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)`BoxView`デバイスに依存しない単位での幅を設定する場合は。
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)の高さを設定する場合は `BoxView` 。

`Color`プロパティは型です。プロパティは任意の値に設定できます。これには、からまでの `Color` `Color` アルファベット順の名前付きの色の静的読み取り専用フィールド141が含まれ `AliceBlue` `YellowGreen` ます。

`CornerRadius`プロパティは型です。プロパティには、 [`CornerRadius`](xref:Xamarin.Forms.CornerRadius) 1 つの均一な `double` 角の半径値、またはの左上、 `CornerRadius` 右上、左下、右下に適用される4つの値で定義された構造体を設定でき `double` `BoxView` ます。

`WidthRequest`プロパティと `HeightRequest` プロパティは、 `BoxView` がレイアウトで*制約*を持たない場合にのみ、ロールを再生します。 これは、レイアウトコンテナーが子のサイズを知る必要がある場合です。たとえば、 `BoxView` がレイアウト内の自動サイズのセルの子である場合です `Grid` 。 `BoxView` `HorizontalOptions` および `VerticalOptions` プロパティが以外の値に設定されている場合は、も制約され `LayoutOptions.Fill` ません。 に `BoxView` 制約はあり `WidthRequest` ませんが、 `HeightRequest` プロパティとプロパティが設定されていない場合、幅または高さは40単位の既定値、またはモバイルデバイスでは約1/4 インチに設定されます。

`WidthRequest` `HeightRequest` がレイアウトで制限されている場合、プロパティとプロパティは無視されます。この場合、 `BoxView` レイアウトコンテナーはに独自のサイズを指定し*constrained* `BoxView` ます。

`BoxView` では、一方の寸法を制約ありにし、他の寸法を制約なしにすることができます。 たとえば、 `BoxView` が垂直の子である場合、の `StackLayout` 垂直ディメンションは `BoxView` 制約されません。また、水平ディメンションは一般に制約されます。 ただし、その水平ディメンションには例外があります。の `BoxView` プロパティが以外の値に設定されている場合、 `HorizontalOptions` `LayoutOptions.Fill` 水平ディメンションも制約されません。 また、には制約の `StackLayout` ない水平ディメンションがある場合もあります。この場合、は `BoxView` 水平方向に制約されません。

[**Basicboxview**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)サンプルでは、ページの中央に制約のない1インチの正方形が表示され `BoxView` ます。

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

結果は次のとおりです。

[![基本 BoxView](boxview-images/basicboxview-small.png "基本 BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

`VerticalOptions` `HorizontalOptions` プロパティとプロパティがタグから削除された場合 `BoxView` 、またはに設定されている場合 `Fill` 、は `BoxView` ページのサイズによって制限され、ページに収まるように拡大されます。

は、 `BoxView` の子にすることもでき `AbsoluteLayout` ます。 その場合、の位置とサイズの両方が、アタッチ可能なバインド可能な `BoxView` プロパティを使用して設定され `LayoutBounds` ます。 に `AbsoluteLayout` ついては、 [**AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolutelayout.md)の記事で説明されています。

これらのすべてのケースの例については、次のサンプルプログラムを参照してください。

## <a name="rendering-text-decorations"></a>文字装飾のレンダリング

を使用すると、 `BoxView` 水平方向と垂直方向の線の形式で、ページに単純な装飾を追加できます。 [**Textdecoration**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)のサンプルでは、これを示します。 すべてのプログラムのビジュアルは**mainpage.xaml**ファイルで定義されています。このファイルには、 `Label` `BoxView` 次に示すの要素と要素がいくつか含まれてい `StackLayout` ます。

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

後続のすべてのマークアップは、の子です `StackLayout` 。 このマークアップは `BoxView` 、要素で使用される複数の種類の装飾要素で構成されてい `Label` ます。

[![文字の装飾](boxview-images/textdecoration-small.png "文字の装飾")](boxview-images/textdecoration-large.png#lightbox "文字の装飾")

ページの上部にあるスタイリッシュなヘッダーは、 `AbsoluteLayout` 子が4つの `BoxView` 要素を持ち `Label` 、すべてに特定の場所とサイズが割り当てられているで実現されます。

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

XAML ファイルでは、の後に、が `AbsoluteLayout` 記述された書式付きテキストを持つが続き `Label` `AbsoluteLayout` ます。

`Label` `BoxView` `StackLayout` `HorizontalOptions` 値が以外の値に設定されているでとの両方を囲むことによって、テキスト文字列に下線を引くことができ `Fill` ます。 の幅は、の幅に `StackLayout` よって制御され、その幅がに `Label` `BoxView` なります。 に `BoxView` は、明示的な高さのみが割り当てられます。

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

この手法を使用して、長いテキスト文字列や段落内の個々の単語に下線を引くことはできません。

また、を使用して、 `BoxView` HTML `hr` (水平方向のルール) 要素のようにすることもできます。 の幅を `BoxView` 親コンテナーによって決定するだけです。この例では次のようになり `StackLayout` ます。

```xaml
<BoxView HeightRequest="3" />
```

最後に、との両方を水平方向に囲むことによって、テキストの段落の片側に垂直線を描画でき `BoxView` `Label` `StackLayout` ます。 この場合、の高さは、の高さ `BoxView` `StackLayout` によって制御されるの高さと同じになり `Label` ます。

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```

## <a name="listing-colors-with-boxview"></a>BoxView を使用した色の一覧表示

は、 `BoxView` 色を表示する場合に便利です。 このプログラムは、を使用して、 `ListView` 構造体のすべてのパブリックな静的な読み取り専用フィールドを一覧表示し Xamarin.Forms `Color` ます。

[![ListView の色](boxview-images/listviewcolors-small.png "ListView の色")](boxview-images/listviewcolors-large.png#lightbox "ListView の色")

[**ListViewColors**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors)プログラムには、という名前のクラスが含まれてい `NamedColor` ます。 静的コンストラクターは、リフレクションを使用して構造体のすべてのフィールドにアクセス `Color` し、 `NamedColor` それぞれに対してオブジェクトを作成します。 これらは静的プロパティに格納され `All` ます。

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

プログラムのビジュアルについては、「XAML ファイル」を参照してください。 `ItemsSource`のプロパティは、 `ListView` 静的プロパティに設定され `NamedColor.All` ます。これは、によってすべてのオブジェクトが表示されることを意味し `ListView` `NamedColor` ます。

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

`NamedColor`オブジェクトは `ViewCell` 、のデータテンプレートとして設定されたオブジェクトによって書式設定され `ListView` ます。 このテンプレートには `BoxView` 、 `Color` オブジェクトのプロパティにバインドされているプロパティを持つが含まれてい `Color` `NamedColor` ます。

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>BoxView をサブクラス化して、そのゲームをプレイする

人生の試合は、1970年代の*アメリカ合衆国*の Mathematician John conway とによって考案された携帯電話のオートマトンです。 お勧めするのは、Wikipedia の記事[Conway のゲーム](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)です。

この Xamarin.Forms プログラム[**は、**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)から派生するという名前のクラスを定義し `LifeCell` `BoxView` ます。 このクラスは、有効期間の試合における個々のセルのロジックをカプセル化します。

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

`LifeCell`次の3つのプロパティをに追加し `BoxView` ます。プロパティ `Col` とプロパティは、 `Row` グリッド内のセルの位置を格納し、 `IsAlive` プロパティはその状態を示します。 `IsAlive`また、セルが生きている場合、プロパティはのプロパティを black に設定し、 `Color` `BoxView` セルが生きていない場合は白に設定します。

`LifeCell`また、はをインストールし `TapGestureRecognizer` て、ユーザーがセルをタップして状態を切り替えることができるようにします。 クラスは、 `Tapped` ジェスチャ認識エンジンからイベントを独自のイベントに変換し `Tapped` ます。

また、game of **life**プログラムには、 `LifeGrid` ゲームのロジックの多くをカプセル化するクラスと、 `MainPage` プログラムのビジュアルを処理するクラスも含まれています。 これには、ゲームのルールを説明するオーバーレイが含まれます。 次に示すのは、ページに数百のオブジェクトを表示するアクションのプログラムです `LifeCell` 。

[![人生の試合](boxview-images/gameoflife-small.png "人生の試合")](boxview-images/gameoflife-large.png#lightbox "人生の試合")

## <a name="creating-a-digital-clock"></a>デジタル時計を作成する

[**DotMatrixClock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)プログラムは、210 `BoxView` の要素を作成して、旧形式のドットマトリックスディスプレイのドットをシミュレートします。 この時間は、縦モードまたは横モードで読むことができますが、横には大きくあります。

[![ドットマトリックスクロック](boxview-images/dotmatrixclock-small.png "ドットマトリックスクロック")](boxview-images/dotmatrixclock-large.png#lightbox "ドットマトリックスクロック")

XAML ファイルは、 `AbsoluteLayout` クロックに使用されるをインスタンス化するだけではありません。

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

その他はすべて、分離コードファイルで発生します。 ドットマトリックス表示ロジックは、10桁とコロンのそれぞれに対応するドットを記述するいくつかの配列の定義によって大幅に簡素化されています。

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

これらのフィールドは、 `BoxView` 6 桁のドットパターンを格納する要素の3次元配列で終わります。

コンストラクターは、 `BoxView` 数字とコロンのすべての要素を作成し、 `Color` コロンの要素のプロパティも初期化し `BoxView` ます。

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

このプログラムでは、の相対配置とサイズ変更の機能を使用し `AbsoluteLayout` ます。 各の幅と高さは、それぞれの `BoxView` 小数値に設定されます。具体的には、1の85% が水平と垂直のドット数で割った値になります。 位置は、小数部の値にも設定されます。

すべての位置とサイズはの合計サイズに対して相対的であるため、 `AbsoluteLayout` `SizeChanged` ページのハンドラーは、のを設定するだけで済み `HeightRequest` `AbsoluteLayout` ます。

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

の幅は、 `AbsoluteLayout` ページの幅に合わせて自動的に設定されます。

クラスの最後のコードは、 `MainPage` タイマーコールバックを処理し、各数字のドットの色を決定します。 分離コードファイルの先頭に多次元配列を定義すると、このロジックをプログラムの最も簡単な部分にすることができます。

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

## <a name="creating-an-analog-clock"></a>アナログクロックの作成

ドットマトリックスクロックは、のような明確なアプリケーションであるように見え `BoxView` ますが、 `BoxView` 要素もアナログクロックを認識することができます。

[![BoxView Clock](boxview-images/boxviewclock-small.png "BoxView Clock")](boxview-images/boxviewclock-large.png#lightbox "BoxView Clock")

[**Boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)プログラム内のすべてのビジュアルは、の子です `AbsoluteLayout` 。 これらの要素は、添付プロパティを使用してサイズを設定し、 `LayoutBounds` プロパティを使用して回転し `Rotation` ます。

時計の針の3つの `BoxView` 要素は、XAML ファイルでインスタンス化されますが、位置やサイズはありません。

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

分離コードファイルのコンストラクターは、 `BoxView` 時計の円周を囲む目盛りの60要素をインスタンス化します。

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

すべての要素のサイズと位置は、の `BoxView` ハンドラーで発生し `SizeChanged` `AbsoluteLayout` ます。 というクラスの部分的な構造は、 `HandParams` クロックの合計サイズに対する3人の各針のサイズを示しています。

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

ハンドラーは、 `SizeChanged` の中心と半径を決定 `AbsoluteLayout` し、 `BoxView` 目盛りとして使用する60要素のサイズと位置を設定します。 ループは、 `for` `Rotation` これらの各要素のプロパティを設定することによって終了し `BoxView` ます。 ハンドラーの最後に `SizeChanged` 、 `LayoutHand` メソッドが呼び出され、時計の3人の針のサイズと位置が変更されます。

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

メソッドは、 `LayoutHand` 各ハンドが12:00 の位置を指すようにサイズと位置を設定します。 メソッドの末尾で、 `AnchorY` プロパティは、クロックの中心に対応する位置に設定されます。 これは、回転の中心を示します。

ハンドはタイマーコールバック関数でローテーションされます。

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

2番目の手の扱いは少し異なります。アニメーションイージング関数を適用して、動きが smooth ではなく機械的に見えるようにします。 各目盛りでは、2番目の針は少し戻り、その変換先をオーバーします。 この少量のコードは、動きのリアリティを高めます。

## <a name="related-links"></a>関連リンク

- [基本 BoxView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)
- [文字の装飾 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)
- [ListView の色 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)
- [有効なゲーム (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)
- [ドットマトリックスクロック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)
- [BoxView Clock (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)
- [BoxView](xref:Xamarin.Forms.BoxView)
