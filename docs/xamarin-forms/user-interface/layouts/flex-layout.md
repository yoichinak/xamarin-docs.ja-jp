---
title: Xamarin.Forms FlexLayout
description: FlexLayout を使用して、子ビューのコレクションの配置と折り返しを行う。
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 187befd88c115133a92aa90a711438e7754518d5
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648801"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_FlexLayout を使用して、子ビューのコレクションの配置と折り返しを行う。_

Xamarin.Forms [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) は Xamarin.Forms 3.0 の新機能です。 これは CSS [フレキシブル ボックス レイアウト モジュール](http://www.w3.org/TR/css-flexbox-1/) に基づいており、通常 _flex layout_ や _flex-box_ として知られています。これはレイアウトに子を整列するための多くの柔軟なオプションが含まれているためそう呼ばれています。

`FlexLayout` は スタックに水平および垂直方向に配置することができる Xamarin.Forms の [`StackLayout`](~/xamarin-forms/user-interface/layouts/stack-layout.md) に似ています。 ただし、`FlexLayout` は、1行または1列に多すぎて収められない場合に子を折り返す機能もあります。また配置方向や配置属性やさまざまな画面サイズへ対応するための多くのオプションもあります。

`FlexLayout` は [`Layout<View>`](xref:Xamarin.Forms.Layout`1) から派生し、`IList<View>` 型の [`Children`](xref:Xamarin.Forms.Layout`1.Children) プロパティを継承しています。

`FlexLayout` は、6 つのパブリックなバインド可能なプロパティと、サイズ・向き・およびその子要素の配置に影響する 5 つのバインド可能な添付プロパティが定義されています。 (バインド可能な添付プロパティについてご不明な点があれば、 **[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)** の記事を参照してください。)これらのプロパティは、以下の **[添付プロパティの詳細](#bindable-properties)** と **[バインド可能な添付プロパティの詳細](#attached-properties)** のセクションで詳しく説明されています。 しかし、この記事はこれらのプロパティの多くをより簡略に説明する `FlexLayout` の **[一般的な使用シナリオ](#common-scenarios)** のセクションから始まります。 この記事の最後に、`FlexLayout` と [CSS スタイルシート](~/xamarin-forms/user-interface/styles/css/index.md) を組み合わせる方法を参照できます。

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>一般的な使用シナリオ

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプル プログラムにはの一般的な使用方法を説明するいくつかのページが含まれています`FlexLayout`し、そのプロパティを実験することができます。

### <a name="using-flexlayout-for-a-simple-stack"></a>単純なスタックで FlexLayout を使用する

**Simple Stack** のページは、`FlexLayout` を `StackLayout` の代わりにどのように、よりシンプルなマークアップで使用できるかを示します。 このサンプルは全てXAMLで定義されています。 この `FlexLayout` は 4 つの子が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

iOS、Android、および Universal Windows Platform で実行したページを次に示します。

[![Simple Stack ページ](flex-layout-images/SimpleStack.png "Simple Stack ページ")](flex-layout-images/SimpleStack-Large.png#lightbox)

`FlexLayout`SimpleStackPage.xaml**ファイルに** の 3 つのプロパティが示されています。

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)プロパティは、[`FlexDirection`](xref:Xamarin.Forms.FlexDirection)列挙型の値が設定されます。 既定値は `Row` です。 このプロパティを `Column` に設定すると、 `FlexLayout` の子はアイテムを 1 つの列に配置します。

    `FlexLayout` のアイテムが1列で配置されたとき、 `FlexLayout` は垂直の _主軸_ と水平の _交差軸_ を持つとされます。

- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは、[`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 型で、交差軸上にアイテムを配置する方法を指定します。 `Center`オプションは、各アイテムを水平方向に中央揃えにします。

    もしこのタスクに `FlexLayout` ではなく `StackLayout` を使用していたら、各アイテムの `HorizontalOptions` を `Center` に設定することで、全てのアイテムを中央揃えにする必要があったでしょう。 `HorizontalOptions` プロパティは、 `FlexLayout` の子では動作しませんが、 1つの `AlignItems` プロパティだけで同じ目標を達成できます。 必要であれば、 `AlignSelf` の添付されたバインド可能なプロパティを使って、個別のアイテムの `AlignItems` プロパティを上書きできます。

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    この変更によって、 この `Label` は、読み順が左から右の場合、`FlexLayout` の左の端に配置されます。

- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティは、[`FlexJustify`](xref:Xamarin.Forms.FlexJustify) 型で、主軸上にアイテムを配置する方法を指定します。 `SpaceEvenly`オプションは、全ての垂直の余白を全てのアイテム間と最初のアイテムの上と最後のアイテムの下とで均等に割り当てます。

    もし `StackLayout` を使用していたら、同様の効果を実現するために、各アイテムの `VerticalOptions` プロパティに `CenterAndExpand` を設定する必要があったでしょう。 しかし`CenterAndExpand` オプションは、最初のアイテムの前と最後のアイテムの後の余白の2倍の余白を各アイテム間に割り当ててしまいます。 `FlexLayout` の `JustifyContent` プロパティを `SpaceAround` に設定することで、 `VerticalOptions` の `CenterAndExpand` オプションを再現できます。

これらの `FlexLayout` プロパティは、下にある **[バインド可能なプロパティの詳細](#bindable-properties)** セクションでより詳しく説明します。

### <a name="using-flexlayout-for-wrapping-items"></a>FlexLayout を使ってアイテムを折り返す

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **Photo Wrapping** のページでは、`FlexLayout`がどのように行または列に追加された子を折り返すことができるかを説明します。 次のXAML ファイルでは `FlexLayout` をインスタンス化し、2 つのプロパティを割り当てます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

この `FlexLayout` の `Direction` プロパティは未設定なので、規定の設定 `Row` になり、子が行に配置されて主軸が水平方向になることを意味します。

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは、 [`FlexWrap`](xref:Xamarin.Forms.FlexWrap)列挙型です。 アイテムが多くて行に収まらない場合は、このプロパティの設定によってアイテムを次の行に折り返すことができます。

`FlexLayout` は `ScrollView` の子であることに注意してください。 ページに収まらないほどの多くの行がある場合、 `ScrollView` は、`Vertical` という既定の `Orientation` プロパティを持ち、 垂直スクロールできるようになります。

`JustifyContent`プロパティは、各アイテムが同じ量の空白によって囲まれるように、主軸（水平方向軸）の余った領域を割り当てます。

次の分離コード ファイルでは、サンプル写真のコレクションにアクセスし、 `FlexLayout` の `Children` コレクションに追加します。

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

下にスクロールされる上から実行されている、段階的にプログラムを次に示します。

[![Photo Wrapping ページ](flex-layout-images/PhotoWrapping.png "Photo Wrapping ページ")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>FlexLayout を使ったページレイアウト

非常に望ましいレイアウト形式ですが、完全に実現することがしばしば困難であることから、 [_聖杯_](https://en.wikipedia.org/wiki/Holy_grail_(web_design))と呼ばれる webデザインの標準的なレイアウトがあります。 そのレイアウトは、ページの上部にあるヘッダーと下部にあるフッターで構成され、そのどちらもページの幅全体に及びます。 ページの中央を占めるものは、メインコンテンツですが、しばしばコンテンツの左側に縦型のメニューと右側に補助的な情報（_aside_ エリア とも呼ばれる）があります。 [Section 5.4.1 of the CSS Flexible Box Layout specification 5.4.1](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) で 聖杯レイアウトを flex box を使って実現する方法について説明されています。

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプル の **Holy Grail Layout** のページは、`FlexLayout` の入れ子を使った聖杯レイアウトの簡単な実装を示しています。 このページは縦向きモードの電話用にデザインされているため、コンテンツ領域の右側および左側の領域は50ピクセル幅です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

ここでは、実行します。

[![Holy Grail Layout ページ](flex-layout-images/HolyGrailLayout.png "Holy Grail Layout ページ")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

ナビゲーションと aside エリアは、左側と右側に `BoxView` を使ってレンダリングしています。

XAML ファイルの最初の `FlexLayout` は垂直の主軸を持ち、列に配置された3つの子を含んでいます。 それらは ヘッダー、ページ本体、フッターとなります。 入れ子の `FlexLayout` は水平の主軸と行に配置された3つの子を持ちます。

このプログラムでは、次の 3 つのバインド可能な添付プロパティについて説明します。

- `Order`のバインド可能な添付プロパティは、最初の `BoxView` にセットされています。 このプロパティは、既定値 0 を持つ整数型です。 このプロパティを使ってレイアウトの順序を変更することができます。 一般的に開発者は、ナビゲーション項目と aside 項目より前にページのコンテンツがマークアップに表示されることを好みます。 最初の `BoxView` の `Order` プロパティに他の兄弟要素より小さい値を設定すると、その行の最初のアイテムとして表示することができます。 同様に、`Order` プロパティに兄弟要素より大きい値を設定することでアイテムを最後に表示させることができます。

- `Basis`添付プロパティは、2 つの `BoxView` に 50 ピクセル幅を与えるために設定されています。 このプロパティは、 `FlexBasis` 型で、 `Auto` という名前の `FlexBasis` 型の静的プロパティが定義されている構造体で、`Auto` が既定値となります。 `Basis` を使って、ピクセルのサイズまたは主軸上でそのアイテムが占める領域の量を示すパーセンテージを指定することができます。 _basis_ と呼ばれる理由は、全ての後続のレイアウトの基本となるアイテムのサイズを指定するためです。

- `Grow`プロパティは、 入れ子の `Layout` と コンテンツを表す子の `Label` に設定されています。 このプロパティは `float` 型で、既定値は 0 です。 正の値を設定すると、主軸上にある残りの全ての領域が、そのアイテムと正の値の `Grow` を持つ兄弟要素に割り当てられます。その領域は `Grow` の値の比率にしたがって割り当てられます。 これは `Grid` の星形のプロパティにやや似ています。

    最初の `Grow` 添付プロパティは、 入れ子の `FlexLayout` 上に設定されており、この `FlexLayout` が外側の `FlexLayout` 内の未使用の垂直の領域全てを使用することを示しています。 2つめの `Grow` 添付プロパティは、コンテンツを表す `Label` 上に設定されており、このコンテンツが 内部の `FlexLayout` の未使用の水平の領域全てを使用することを示します。

    同様に、子のサイズの合計が (折り返ししない) `FlexLayout` のサイズを超える場合に使うことができる `Shrink` のバインド可能な添付プロパティもあります。

### <a name="catalog-items-with-flexlayout"></a>FlexLayout を使ったカタログアイテム

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **Catalog Items** ページは、水平スクロールによって一連の3匹のサルの写真と説明が表示される点以外は、[CSS フレックス レイアウト ボックス仕様の1.1のセクションの例1](http://www.w3.org/TR/css-flexbox-1/#overview) に似ています。

[![Catalog Items ページ](flex-layout-images/CatalogItems.png "Catalog Items ページ")](flex-layout-images/CatalogItems-Large.png#lightbox)

3匹のそれぞれのサルは、明示的に高さと幅を与えられた `Frame` に含まれた `FlexLayout` の中に表示します。またそれは、より大きな `FlexLayout` の子となっています。 この XAML ファイルでは、`FlexLayout` の子のプロパティのほとんどは、スタイルで指定されており、そのうちの一つ以外は暗黙的なスタイルが使われています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

`Image` のための暗黙的なスタイルは、`Flexlayout` の2つのバインド可能な添付プロパティの設定を含みます。

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order`の設定の &ndash;1 は、`Image` 要素を子コレクション内の位置に関係なく、それぞれの入れ子の `FlexLayout` で最初に表示させるようにします。 `AlignSelf`プロパティの `Center` は、`Image` を `FlexLayout` 内で中央揃えにします。 このプロパティは `AlignItems` プロパティの設定を上書きします。`AlignItems` プロパティは規定値として `Stretch` を持つので、`Label` と `Button` は `FlexLayout` の全幅に引き延ばされるということになります。

3 つの各 `FlexLayout` の中には、空白の `Label` が `Button` の前にあり、それには `Grow` に 1 が設定されています。 これは全ての余分な垂直の領域がこの空白の `Label` に割り当てられることを意味します。これによって `Button` を最下部に効果的に押し出すことができます。

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>バインド可能なプロパティの詳細

`FlexLayout` のいくつかの一般的なアプリケーションを見てきた今なら、`FlexLayout` のプロパティをより詳しく調査できるはずです。
`FlexLayout` 設定された 6 つのバインド可能なプロパティを定義、`FlexLayout`自体であるため、コードまたは XAML でコントロールの向きを配置します。 （これらのプロパティの1つに [`Position`](xref:Xamarin.Forms.FlexLayout.Position) がありますが、これはこの記事では説明しません。）

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプル の **Experiment** ページ を使って、残り 5 つのバインド可能なプロパティを試すことができます。 このページは、`FlexLayout` から子を追加・削除したり、5 つのバインド可能なプロパティの組み合わせを設定したりすることができます。 `FlexLayout` の全ての子は、様々な色やサイズの `Label` view で、`Text` プロパティには `Children` コレクションの位置に対応する番号がセットされます。

プログラムを起動すると、5 つの `Picker` view に、これら 5 つの `FlexLayout` のプロパティの規定値が表示されます。 画面下部の `FlexLayout` は 3 つの子を含んでいます。

[![実験ページ:既定](flex-layout-images/ExperimentDefault.png "の実験ページ-既定")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

各 `Label` view は、 `FlexLayout` 内で `Label` に割り当てられた領域を示すグレーの背景を持っています。 `FlexLayout` 自身の背景はアリスブルーです。 それはページ下部の左右の少量の余白を除いた領域全体を占めます。

<a name="direction" />

### <a name="the-direction-property"></a>Direction プロパティ

[`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) プロパティは 4 つのメンバーを列挙する [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) 型です。

- `Column`
- `ColumnReverse` ( XAML 内では "cloumn-reverse" も可 )
- `Row` 既定値
- `RowReverse` ( XAML 内では "row-reverse" も可 )

XAML では、このプロパティの値を大文字や小文字、またはそれらの混在で列挙メンバー名を指定できます。また、CSSの指定方法と同様の括弧内に示す2つの追加された文字列も使用可能です。 （"column-reverse" と "row-reverse" は [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) クラスに定義され、XAML パーサーによって使用されます。

ここに、 (左から右へ順番に) `Row` direction、`Column` direction、および`ColumnReverse` direction で表示した **Experiment** ページ を示します。

[![実験ページ:(flex-layout-images/ExperimentDirection.png "実験ページの方向")]](flex-layout-images/ExperimentDirection-Large.png#lightbox)

`Reverse` オプションに注意してください。これはアイテムが右または下から始まるようになります。

<a name="wrap" />

### <a name="the-wrap-property"></a>Wrap プロパティ

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは 3 つのメンバーを列挙する [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) 型です。

- `NoWrap` 既定値
- `Wrap`
- `Reverse` ( XAML 内では "wrap-reverse" も可 )

左から順番に、12 の子を持つ `NoWrap` 、`Wrap`、`Reverse` オプションでの画面表示です。

[![実験ページ:](flex-layout-images/ExperimentWrap.png "実験ページの折り返しを")折り返す](flex-layout-images/ExperimentWrap-Large.png#lightbox)

`Wrap` プロパティに `NoWrap` が設定されていて、主軸が (このプログラム) のように制約されていると、主軸は全ての子を合わせるほどの十分な高さや幅がありません。`FlexLayout` は、iOSのデモのスクリーンショットのように、アイテムを小さくさせることを試みます。 [`Shrink`](#shrink) 添付プロパティを使って、これらのアイテムの縮小を制御することができます。

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent プロパティ

[`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティは 6 つのメンバーを列挙する [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) 型です。

- `Start` ( XAML では "flex-start" も可 )、既定値
- `Center`
- `End` ( XAML では "flex-end" も可 )
- `SpaceBetween` ( XAML では "space-between" も可 )
- `SpaceAround` ( XAML では "space-around" も可 )
- `SpaceEvenly`

このプロパティは、主軸上でのアイテムを配置方法を指定します。この例では主軸は水平軸です。

[![実験ページ:コンテンツの調整(flex-layout-images/ExperimentJustifyContent.png "実験ページのコンテンツのジャスティフィケーション")]](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

3 つのすべてのスクリーン ショットでは、`Wrap` プロパティには `Wrap` が設定されています。 規定値である `Start` は、Android のスクリーンショットで示しています。 iOS のスクリーンショットには `Center` オプション （すべてのアイテムが中央に移動）が示されています。 `Space` という単語で始まるその他3つのオプションは、アイテムに使用されなかった余白を割り当てます。 `SpaceBetween` はアイテムの間に余白を均等に割り当て、`SpaceAround` は各アイテムの両端に均等に余白を置きます。一方、`SpaceEvenly` は各アイテムの間と行の最初のアイテムの前と最後のアイテムの後に均等に余白を置きます。

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems プロパティ

[`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは、4 つのメンバーを列挙する [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 型です。

- `Stretch` 既定値
- `Center`
- `Start` ( XAML では "flex-start" も可 )
- `End` ( XAML では "flex-end" も可 )

これは、 交差軸上に子を整列する方法を示す 2 つのプロパティの 1 つ（もう 1 つは[`AlignContent`](#align-content)）です。 各行内で（上のスクリーンショットで示すように）子は引き伸ばされますが、以下の3つのスクリーンショットのように、各アイテムの開始・中央・終了位置に揃えられます。

[![実験ページ:項目](flex-layout-images/ExperimentAlignItems.png "の整列実験ページの")整列](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

iOS のスクリーンショットでは、すべての子は上揃えになっています。 Android のスクリーンショットでは、全てのアイテムはもっとも高い子に合わせて垂直方向に中央揃えになっています。 UWP のスクリーンショットでは、すべてのアイテムは下揃えになっています。

すべての個々のアイテムは、[`AlignSelf`](#align-self) 添付プロパティを使って `AlignItems` の設定を上書きすることができます。

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent プロパティ

[`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent)プロパティは、7 つのメンバーを列挙する [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent) 型です。

- `Stretch` 既定値
- `Center`
- `Start` ( XAML では "flex-start" も可 )
- `End` ( XAML では "flex-end" も可 )
- `SpaceBetween` ( XAML では "space-between" も可 )
- `SpaceAround` ( XAML では "space-around" も可 )
- `SpaceEvenly`

`AlignItems` と同様に、`AlignContent` プロパティも交差軸上の子を整列しますが、これは行または列全体に影響を与えます。

[![実験ページ:コンテンツの配置(flex-layout-images/ExperimentAlignContent.png "実験ページのコンテンツの整列")]](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

両方の行は、iOS のスクリーン ショット。 上部にあります。center; にあればの Android のスクリーン ショット下部にある UWP のスクリーン ショットになるとします。 行自体もさまざまな方法で配置することができます。

[![実験ページ:コンテンツの整列]2(flex-layout-images/ExperimentAlignContent2.png "実験ページの配置 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` は1行または1列のみの場合は影響を与えません。

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>バインド可能な添付プロパティの詳細

`FlexLayout` は 5 つの添付プロパティが定義されています。これらのプロパティは `Flexlayout` の子に設定され、その特定の子のみに影響します。 子でこれらのプロパティを設定、`FlexLayout`し、その特定の子にのみ関連します。

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf プロパティ

[`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) 添付プロパティは、 5 つのメンバーを列挙する [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent) 型です。

- `Auto` 既定値
- `Stretch`
- `Center`
- `Start` ( XAML では "flex-start" も可 )
- `End` ( XAML では "flex-end" も可 )

`FlexLayout` の任意の個別の子のために、このプロパティの設定は `FlexLayout` 自身の [`AlignItems`](#align-items) プロパティ設定を上書きします。 規定の設定である `Auto` は `AlignItems` の設定を使用することを意味します。

例えば `Label` という名前の `label` 要素には、以下のようにコードで `AlignSelf` プロパティをセットすることができます。

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

`Label` の親である `FlexLayout` への参照がないことに注意してください。 XAML では、このようなプロパティを設定します。

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order プロパティ

[`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty) プロパティは `int` 型で、 既定値は 0 です。

`Order` プロパティは、`FlexLayout` の子が配置される順番を変更することができます。 通常、`FlexLayout` の子は `Children` コレクションの出現順と同じ順番で配置されます。 この順番は、1つ以上の子の `Order` 添付プロパティに 0 以外の数値を設定することで上書きすることができます。 `FlexLayout` はその場合、それぞれの子の `Order` プロパティの設定に基づいて自身の子を配置します。ただし、同じ `Order` 設定を持つ子は、`Children` コレクションの出現順に配置されます。

### <a name="the-basis-property"></a>Basis プロパティ

[`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty) 添付プロパティは、主軸上で `FlexLayout` の子に割り当てられている領域の量を示します。 指定したサイズ、`Basis`プロパティは、メインの親の軸に沿ったサイズ`FlexLayout`します。 したがって、`Basis` は、行に子が配置された時の子の幅、または列に子が配置された時の高さを示します。

`Basis`プロパティは、[`FlexBasis`](xref:Xamarin.Forms.FlexBasis) 型の構造体です。 サイズはデバイスに依存しない単位、または `FlexLayout` のサイズの割合のどちらかで指定することができます。 `Basis` プロパティの規定値は、静的プロパティの `FlexBasis.Auto` で、これは子に要求した幅や高さが使用されることを意味します。

コードでは、以下のように `label` という名前の `Label` の `Basis` プロパティに 40 デバイス非依存単位を設定することができます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

`FlexBasis` コンストラクタの 2 番目の引数は `isRelative` という名前で、相対サイズ（`true`）にするか絶対サイズ（`false`）にするかどうかを示します。 この引数は規定値として `false` を持っています。したがって次のコードのように書くこともできます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

`float` から `FlexBasis` への暗黙的な変換が定義されているので、より簡略的に記述できます。

```csharp
FlexLayout.SetBasis(label, 40);
```

以下のように、親の `FlexLayout` の 25% にサイズを設定できます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

この小数値は 0 ~ 1 の範囲でなければなりません。

XAML では、デバイス非依存単位のサイズの数値を使用できます。

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

また 0 ~ 100% の範囲も指定できます。

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **Basis Experiment** のページでは、`Basis` プロパティを試すことができます。 このページは交互に異なる背景色と前景色を持つ 5 つの `Label` 要素の折り返しする列が表示されます。 2 つの `Slider` 要素は 2 番目と 4 番目の `Label` に `Basis` 値を指定します。

[![The Basis Experiment Page](flex-layout-images/BasisExperiment.png "The Basis Experiment Page")](flex-layout-images/BasisExperiment-Large.png#lightbox)

左の iOS のスクリーンショットは、2 つの `Label` 要素がデバイス非依存単位で高さが与えられていること示しています。 Android のスクリーンショットは、それらの要素が `FlexLayout` の 合計の高さに対する割合での高さが与えられていることを示しています。 が 100% に設定されている場合、子はの高さ`FlexLayout`になり、次の列に折り返され、UWP スクリーンショットに示すように、その列の高さ全体が占められます。 `Basis`5つの子が1つの行に配置されているように見えますが、実際には5つの列に配置されています。

### <a name="the-grow-property"></a>Grow プロパティ

[`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty)添付プロパティは `int` 型です。 規定値は 0 でこの値は 0以上でなければなりません。

`Grow`プロパティは、役割を果たすときに、`Wrap`プロパティに設定されて`NoWrap`子の行には、幅より小さい、幅の合計、 `FlexLayout`、または子の列がよりも短い高さ、 `FlexLayout`。 `Grow` プロパティは子に余った領域を分配する方法を示します。

**Grow Experiment** ページでは、交互に異なる色の 5 つの `Label`要素が列に配置されています。そして 2 つの `Slider` で 2 番目と 4 番目の `Label` の `Grow` プロパティを調整できます。 一番左の iOS のスクリーンショットは、`Grow` プロパティが 0 であるデフォルトの状態を示しています。

[![The Grow Experiment Page](flex-layout-images/GrowExperiment.png "The Grow Experiment Page")](flex-layout-images/GrowExperiment-Large.png#lightbox)

ある子要素に正の `Grow` 値が設定されている場合、Android のスクリーンショットで示すように、その子要素は残りの全ての領域を取得します。 この領域は2つ以上の子の間で割り当てることもできます。 UWP のスクリーンショットでは、2 番目の `Label` の `Grow` プロパティに 0.5 を、4 番目の `Label` の `Grow` プロパティに 1.5 を設定しています。それによって 4 番目の `Label` に 2 番目の `Label` の 3 倍の領域が残りの領域から与えられます。

子ビューがその領域をどのように使用するかは、子の特定の型によって異なります。 `Label` では、テキストは `HorizontalTextAlignment` と `VerticalTextAlignment` のプロパティを使って `Label` の合計領域内に配置することができます。

<a name="shrink" />

### <a name="the-shrink-property"></a>Shrink プロパティ

[`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty)添付プロパティは `int` 型です。 規定値は 1 でその値は 0 以上でなければなりません。

`Shrink` プロパティは、`Wrap` プロパティに `NoWrap` が設定されていて、1行の子の合計幅が `FlexLayout` の幅よりも大きくなる場合、または1列の子の合計の高さが `FlexLayout` の高さより大きくなる場合に機能します。 通常、`FlexLayout` は子のサイズを収縮させることで子を表示しようとします。 `Shrink`プロパティは、子がフルサイズで表示されることに優先度を与えることができます。

**Shrink Experiment** ページは、`FlexLayout` の幅より大きい領域を要求する 5 つの `Label` の子要素を1行で持つ `FlexLayout` を生成します。 左の iOS のスクリーンショットでは、全て規定値 1 の `Label` 要素を表示しています。

[![The Shrink Experiment Page](flex-layout-images/ShrinkExperiment.png "The Shrink Experiment Page")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android のスクリーン ショットで、`Shrink`値、2 番目の`Label`0 に設定されている`Label`がその幅いっぱいに表示されます。 また、4 番目の `Label` には `Shrink` に 1 より大きい値が与えられ、それは収縮しています。 UWP のスクリーンショットは、両方の `Label` 要素に `Shrink` 値 0 が与えられ、可能であれば、それらをフルサイズで表示できることを示しています。

両方を設定することができます、`Grow`と`Shrink`値、子の集計サイズなる可能性がのサイズよりも大きいか小さい場合に備えて、`FlexLayout`します。

## <a name="css-styling-with-flexlayout"></a>FlexLayout を使った CSS スタイル

`FlexLayout` に関連して Xamarin.Forms 3.0 に導入された [CSS スタイル](~/xamarin-forms/user-interface/styles/css/index.md) を使うことができます。 **[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **CSS Catalog Items** ページは、**Catalog Items** ページのレイアウトを複写したものですが、多くのスタイルに CSS スタイルシートを使っています。

[![The CSS Catalog Items Page](flex-layout-images/CssCatalogItems.png "The CSS Catalog Items Page")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

オリジナルの **CatalogItemsPage.xaml** ファイルには 5 つの `Style` があり、自身の `Resources` セクション内に 15 の `Setter` オブジェクトが定義されています。 **CssCatalogItemsPage.xaml** ファイルでは、4 つだけの `Setter` オブジェクトが定義された 2 つの `Style` に減少しています。 これらのスタイルは、Xamarin.Forms CSS スタイル機能が現在サポートしていないプロパティのために、その CSS スタイルシートを補っています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

`Resources` セクションの最初の行で CSS スタイルシートを参照しています。

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

3 つの各アイテム内の 2 つの要素に `StyleClass` 設定が含まれていることにも注目してください。

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

これらは **CatalogItemsStyles.css** スタイルシートのセレクタを参照しています。

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

様々な `FlexLayout` の添付プロパティがここを参照しています。 `label.empty`セレクタでは、`flex-grow` 属性が確認できますが、これは `Button` の上の空白を埋めるために空の `Label` をスタイルしています。 `image`セレクタは、`order`属性と `align-self` 属性を含んでおり、その両方が `FlexLayout` の添付プロパティに対応しています。

これまで `FlexLayout` 上に直接プロパティを設定できることと、`FlexLayout` の子に添付プロパティを設定できること、 またこれらのプロパティに従来の XAML に基づくスタイルや CSS スタイルを使って間接的に設定できることを見てきました 重要なことはこれらのプロパティを知って理解することです。 これらのプロパティは `FlexLayout` を真に柔軟にさせます。

## <a name="flexlayout-with-xamarinuniversity"></a>Xamarin.University で FlexLayout

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin. Forms 3.0 のフレックスレイアウトビデオ**

## <a name="related-links"></a>関連リンク

- [FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
