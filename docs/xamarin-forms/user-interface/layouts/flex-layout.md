---
title: Xamarin. フォーム FlexLayout
description: 子ビューのコレクションを積み重ねる場合やラッピングする場合は、FlexLayout を使用します。
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 75249966c6506bc33ea06c7cfa9c398bd7eb8045
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029484"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin. フォーム FlexLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_子ビューのコレクションを積み重ねる場合やラッピングする場合は、FlexLayout を使用します。_

Xamarin [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)は、Xamarin. forms バージョン3.0 で新しく追加されたものです。 これは、CSS の[柔軟なボックスレイアウトモジュール](https://www.w3.org/TR/css-flexbox-1/)に基づいています。_フレックスレイアウト_または_フレックスボックス_と呼ばれます。これは、レイアウト内に子を配置するための柔軟なオプションが多数含まれているためです。

`FlexLayout` は、その子を積み重ねて水平方向および垂直方向に整列させることができるという点で、Xamarin. Forms [`StackLayout`](~/xamarin-forms/user-interface/layouts/stack-layout.md)に似ています。 ただし、`FlexLayout` は、1つの行または列に収まりきらない場合に、子をラップすることもできます。また、さまざまな画面サイズに合わせて向きや配置を行うためのオプションも多数用意されています。

`FlexLayout` は[`Layout<View>`](xref:Xamarin.Forms.Layout`1)から派生し、型 `IList<View>`の[`Children`](xref:Xamarin.Forms.Layout`1.Children)プロパティを継承します。

`FlexLayout` は、6つのパブリックバインド可能プロパティと、子要素のサイズ、向き、および配置に影響を与える、アタッチ可能な5つのバインド可能なプロパティを定義します。 (アタッチ可能なバインド可能なプロパティの詳細については、「 **[アタッチさ](~/xamarin-forms/xaml/attached-properties.md)** れたプロパティ」を参照してください)。これらのプロパティの詳細については、バインド可能な **[プロパティ](#bindable-properties)** について詳しく説明し、アタッチ可能なバインド可能な **[プロパティ](#attached-properties)** について詳しく説明します。 ただし、この記事では、これらのプロパティの多くをより簡単に説明する `FlexLayout` の **[一般的な使用シナリオ](#common-scenarios)** について説明します。 記事の最後に、`FlexLayout` と[CSS スタイルシート](~/xamarin-forms/user-interface/styles/css/index.md)を組み合わせる方法について説明します。

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>一般的な使用シナリオ

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルプログラムには、`FlexLayout` の一般的な使用方法を示すいくつかのページが含まれており、そのプロパティを試してみることができます。

### <a name="using-flexlayout-for-a-simple-stack"></a>単純なスタックに FlexLayout を使用する

**[単純なスタック]** ページには、`FlexLayout` が `StackLayout` に代わるものの、より単純なマークアップを使用する方法が示されています。 このサンプルの内容はすべて、XAML ページで定義されています。 `FlexLayout` には、次の4つの子が含まれます。

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

IOS、Android、ユニバーサル Windows プラットフォームで実行されているページは次のとおりです。

[![[単純なスタック] ページ](flex-layout-images/SimpleStack.png "[単純なスタック] ページ")](flex-layout-images/SimpleStack-Large.png#lightbox)

**Simplestackpage .xaml**ファイルには、`FlexLayout` の3つのプロパティが表示されます。

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)プロパティは、 [`FlexDirection`](xref:Xamarin.Forms.FlexDirection)列挙値に設定されます。 既定値は、`Row` です。 プロパティを `Column` に設定すると、`FlexLayout` の子が1つの項目の列に配置されます。

    `FlexLayout` 内の項目が列に配置されている場合、`FlexLayout` は _、垂直方向の主軸と_横方向の_交差軸_を持つことになります。

- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは[`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems)型で、交差軸上での項目の位置を指定します。 `Center` オプションを使用すると、各項目が水平方向に中央揃えになります。

    このタスクの `FlexLayout` ではなく `StackLayout` を使用していた場合は、各項目の `HorizontalOptions` プロパティを `Center`に割り当てることで、すべての項目を中央揃えにします。 `HorizontalOptions` プロパティは `FlexLayout`の子に対しては機能しませんが、単一の `AlignItems` プロパティは同じ目的を達成します。 必要に応じて、`AlignSelf` 添付されたバインド可能なプロパティを使用して、個々の項目の `AlignItems` プロパティをオーバーライドできます。

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    この変更により、読み取り順序が左から右になると、この1つの `Label` が `FlexLayout` の左端に配置されます。

- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティは[`FlexJustify`](xref:Xamarin.Forms.FlexJustify)型で、項目をメイン軸にどのように配置するかを指定します。 `SpaceEvenly` オプションを指定すると、すべてのアイテムと、最初のアイテムの上、および最後のアイテムの下にあるすべての領域が均等に割り当てられます。

    `StackLayout`を使用する場合は、同様の効果を得るために、各項目の `VerticalOptions` プロパティを `CenterAndExpand` に割り当てる必要があります。 ただし `CenterAndExpand` オプションでは、最初の項目の前と最後の項目の後に、各項目の間に2倍の領域が割り当てられます。 `FlexLayout` の `JustifyContent` プロパティを `SpaceAround`に設定することにより、`VerticalOptions` の `CenterAndExpand` オプションを模倣することができます。

これらの `FlexLayout` のプロパティについては、後の「バインド可能な **[プロパティ](#bindable-properties)** 」で詳しく説明します。

### <a name="using-flexlayout-for-wrapping-items"></a>項目のラップに FlexLayout を使用する

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** のサンプルの **[写真の折り返し]** ページでは、`FlexLayout` がその子を追加の行または列にラップする方法を示します。 XAML ファイルによって `FlexLayout` がインスタンス化され、次の2つのプロパティが割り当てられます。

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

この `FlexLayout` の [`Direction`] プロパティが設定されていないため、`Row`の既定の設定があります。これは、子が行に配置され、メイン軸が水平になることを意味します。

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは[`FlexWrap`](xref:Xamarin.Forms.FlexWrap)列挙型です。 項目が多すぎて行に収まらない場合は、このプロパティを設定すると、項目が次の行に折り返されます。

`FlexLayout` が `ScrollView`の子であることに注意してください。 ページに収める行が多すぎる場合、`ScrollView` には `Vertical` の既定の `Orientation` プロパティがあり、垂直スクロールが許可されます。

`JustifyContent` プロパティは、各項目が同じサイズの空白で囲まれるように、メイン軸 (水平軸) に残っている領域を割り当てます。

分離コードファイルは、サンプル写真のコレクションにアクセスし、`FlexLayout`の `Children` コレクションに追加します。

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

次のプログラムは、上から下へと徐々にスクロールして実行されています。

[![[写真の折り返し] ページ](flex-layout-images/PhotoWrapping.png "[写真の折り返し] ページ")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>FlexLayout を使用したページレイアウト

Web デザインには、[_究極_](https://en.wikipedia.org/wiki/Holy_grail_(web_design))と呼ばれる標準的なレイアウトがあります。これは、非常に望ましいレイアウト形式ですが、多くの場合、完璧求めるでは実現が困難です。 レイアウトは、ページの上部にあるヘッダーと下部のフッターで構成され、両方ともページ全体の幅に拡張されます。 ページの中央を使用するのはメインコンテンツですが、多くの場合、コンテンツの左側には列形式のメニューがあり、右側には_補助的な情報 (領域と_も呼ばれます) があります。 [「CSS フレキシブルボックスレイアウト仕様」のセクション 5.4.1](https://www.w3.org/TR/css-flexbox-1/#order-accessibility)では、フレックスボックスを使用して、聖究極レイアウトを実現する方法について説明しています。

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの**究極レイアウト**ページは、別ので入れ子になった1つの `FlexLayout` を使用したこのレイアウトの単純な実装を示しています。 このページは縦モードの携帯電話向けに設計されているため、コンテンツエリアの左右の領域は50ピクセル幅のみです。

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

次のように実行されています。

[![究極レイアウトページ](flex-layout-images/HolyGrailLayout.png "究極レイアウトページ")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

ナビゲーション領域と残り領域は、左右の `BoxView` でレンダリングされます。

XAML ファイルの最初の `FlexLayout` には垂直方向のメイン軸があり、列に配置された3つの子が含まれています。 これらは、ヘッダー、ページの本文、およびフッターです。 入れ子になった `FlexLayout` には、3つの子が行内に配置された水平方向のメイン軸があります。

このプログラムでは、アタッチ可能な3つのバインド可能なプロパティが示されています。

- `Order` アタッチされたバインド可能なプロパティは、最初の `BoxView`に設定されます。 このプロパティは整数で、既定値は0です。 このプロパティを使用して、レイアウトの順序を変更できます。 通常、開発者は、ナビゲーション項目および項目の前に、ページの内容をマークアップで表示することを希望しています。 最初の `BoxView` の `Order` プロパティを他の兄弟よりも小さい値に設定すると、行の最初の項目として表示されます。 同様に、`Order` プロパティを兄弟よりも大きい値に設定することにより、項目が最後に表示されるようにすることができます。

- `Basis` アタッチ可能なバインド可能なプロパティは、2つの `BoxView` 項目に対して設定され、幅は50ピクセルになります。 このプロパティの型は `FlexBasis`であり、`Auto`という名前の静的なプロパティ `FlexBasis` 型 (既定値) を定義する構造体です。 `Basis` を使用して、主軸上で項目が占める領域を示すピクセルサイズまたはパーセンテージを指定できます。 これは、後続のすべてのレイアウトの基礎となる項目サイズを指定するため、_ベース_として呼び出されます。

- `Grow` プロパティは、入れ子になった `Layout` と、コンテンツを表す `Label` 子に設定されます。 このプロパティの型は `float` で、既定値は0です。 正の値に設定すると、メイン軸に沿った残りのすべての領域が、その項目と `Grow`の正の値を持つ兄弟に割り当てられます。 値に比例して割り当てられます。これは、`Grid`でのスター指定と同様です。

    最初の `Grow` 添付プロパティは、入れ子になった `FlexLayout`に設定されます。これは、この `FlexLayout` が、外側の `FlexLayout`内の未使用のすべての垂直方向の領域を占有することを示します。 2番目の `Grow` 添付プロパティは、コンテンツを表す `Label` に対して設定されます。これは、このコンテンツが内部 `FlexLayout`内の未使用のすべての領域を占有することを示します。

    また、類似した `Shrink` 添付可能なバインド可能なプロパティもあります。これは、子のサイズが `FlexLayout` のサイズを超えても、折り返しが望ましくない場合に使用できます。

### <a name="catalog-items-with-flexlayout"></a>FlexLayout を使用したカタログアイテム

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **[カタログ項目]** ページは、 [CSS フレックスレイアウトボックス仕様のセクション1.1 の例 1](https://www.w3.org//TR/css-flexbox-1/#overview)と似ていますが、水平スクロール可能な一連の画像と3つの猿の説明が表示される点が異なります。:

[![[カタログアイテム] ページ](flex-layout-images/CatalogItems.png "[カタログアイテム] ページ")](flex-layout-images/CatalogItems-Large.png#lightbox)

3つの各猿は、明示的な高さと幅が指定され、さらに大きい `FlexLayout`の子でもある `Frame` に含まれる `FlexLayout` です。 この XAML ファイルでは、`FlexLayout` の子のほとんどのプロパティがスタイルで指定されていますが、そのうち1つだけが暗黙的なスタイルです。

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

`Image` の暗黙的なスタイルには、`Flexlayout`のアタッチ可能な2つのバインド可能なプロパティの設定が含まれます。

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

&ndash;1 の `Order` 設定によって、子コレクション内の位置に関係なく、入れ子になった各 `FlexLayout` ビューに `Image` 要素が最初に表示されます。 `Center` の `AlignSelf` プロパティによって、`Image` が `FlexLayout`内で中央揃えになります。 これにより、`AlignItems` プロパティの設定がオーバーライドされます。このプロパティには既定値 `Stretch`が設定されています。つまり、`Label` と `Button` の子は `FlexLayout`の完全な幅に拡大されます。

3つの `FlexLayout` ビューのそれぞれには、`Button`の前に空白の `Label` がありますが、`Grow` の設定は1です。 これは、すべての余分な垂直空間がこの空の `Label`に割り当てられることを意味します。これにより、`Button` が最終的に一番下にプッシュされます。

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>バインド可能なプロパティの詳細

`FlexLayout`の一般的なアプリケーションをいくつか紹介したので、`FlexLayout` のプロパティをより詳細に調べることができます。
`FlexLayout` は、コードまたは XAML で `FlexLayout` 自体に設定して向きと配置を制御する、6つのバインド可能なプロパティを定義します。 (これらのプロパティの1つである[`Position`](xref:Xamarin.Forms.FlexLayout.Position)については、この記事では説明しません)。

**[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプル の **Experiment** ページ を使って、残り 5 つのバインド可能なプロパティを試すことができます。 このページでは、`FlexLayout` の子を追加または削除したり、5つのバインド可能なプロパティの組み合わせを設定したりできます。 `FlexLayout` のすべての子は、さまざまな色とサイズのビュー `Label` ます。 `Text` プロパティは `Children` コレクション内の位置に対応する数値に設定されます。

プログラムが起動すると、5つの `Picker` ビューにこれら5つの `FlexLayout` プロパティの既定値が表示されます。 画面の下部に `FlexLayout` には、次の3つの子があります。

実験ページ![[:既定の](flex-layout-images/ExperimentDefault.png "実験ページ-既定")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

各 `Label` ビューには、`FlexLayout`内でその `Label` に割り当てられた領域を示す灰色の背景が表示されます。 `FlexLayout` 自体の背景は Alice Blue です。 左右の余白を除いて、ページの一番下の領域全体を占めます。

<a name="direction" />

### <a name="the-direction-property"></a>Direction プロパティ

[`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)プロパティは、次の4つのメンバーを持つ列挙体[`FlexDirection`](xref:Xamarin.Forms.FlexDirection)型です。

- `Column`
- `ColumnReverse` (XAML の "列反転")
- `Row`、既定値
- `RowReverse` (XAML の "行反転")

XAML では、小文字、大文字、または混合ケースで列挙メンバー名を使用して、このプロパティの値を指定できます。または、CSS インジケーターと同じかっこで囲まれた2つの追加文字列を使用することもできます。 ("列逆" と "行反転" の文字列は、XAML パーサーによって使用される[`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter)クラスで定義されています)。

(左から右)、`Row` 方向、`Column` 方向、`ColumnReverse` 方向を示す**実験**ページを次に示します。

実験ページ![[:方向](flex-layout-images/ExperimentDirection.png "実験ページの方向")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

`Reverse` オプションの場合、項目は右側または下部から開始されることに注意してください。

<a name="wrap" />

### <a name="the-wrap-property"></a>Wrap プロパティ

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは、次の3つのメンバーを持つ列挙体[`FlexWrap`](xref:Xamarin.Forms.FlexWrap)型です。

- `NoWrap`、既定値
- `Wrap`
- `Reverse` (XAML の "ラップ逆")

左から右に、これらの画面には、12個の子の `NoWrap`、`Wrap`、および `Reverse` のオプションが表示されます。

実験ページ![[:](flex-layout-images/ExperimentWrap.png "実験ページの折り返し")](flex-layout-images/ExperimentWrap-Large.png#lightbox) をラップする

`Wrap` プロパティが `NoWrap` に設定されていて、メイン軸が (このプログラムのように) 制約されていて、メインの軸がすべての子を収めるのに十分ではない場合、`FlexLayout` は項目を小さくしようとします。iOS のスクリーンショットで示すように。 [`Shrink`](#shrink)アタッチされたバインド可能なプロパティを使用して、項目の shrinkness を制御できます。

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>ジャスト Ifycontent プロパティ

[`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティは、次の6つのメンバーを持つ列挙体[`FlexJustify`](xref:Xamarin.Forms.FlexJustify)型です。

- `Start` (XAML の "フレックス開始")、既定値
- `Center`
- `End` (XAML の "flex-end")
- `SpaceBetween` (XAML の "スペース間")
- `SpaceAround` (XAML の "領域の周囲")
- `SpaceEvenly`

このプロパティは、次の例の横軸であるメイン軸上のアイテムの間隔を指定します。

実験ページ![[:コンテンツ](flex-layout-images/ExperimentJustifyContent.png "実験ページ-コンテンツのジャスティフィケーション")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox) を正当化する

3つのすべてのスクリーンショットでは、`Wrap` プロパティは `Wrap`に設定されています。 `Start` の既定値は、前の Android スクリーンショットに示されています。 IOS のスクリーンショットは、`Center` オプションを示しています。すべての項目が中央に移動されます。 Word で始まる他の3つのオプションは、項目によって占有されていない余分な領域を割り当て `Space` ます。 `SpaceBetween` は、項目間のスペースを均等に割り当てます。`SpaceAround` では、各項目の周囲に均等なスペースが配置されます。一方、`SpaceEvenly` では、各項目の間、最初の項目の前、および行の最後の項目の後に同じスペースが配置されます。

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems プロパティ

[`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは、次の4つのメンバーを持つ列挙体[`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems)型です。

- `Stretch`、既定値
- `Center`
- `Start` (XAML の "フレックス開始")
- `End` (XAML の "flex-end")

これは2つのプロパティ (もう1つは[`AlignContent`](#align-content)) の1つであり、交差軸上での子の位置を示します。 各行内では、次の3つのスクリーンショットに示すように、子が拡大されます (前のスクリーンショットに示したように)。または、各項目の開始、中央、または末尾に配置されます。

実験ページ![[:項目を](flex-layout-images/ExperimentAlignItems.png "実験ページ-項目の整列")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox) に揃える

IOS のスクリーンショットでは、すべての子の上部が揃っています。 Android のスクリーンショットでは、最も高い子に基づいてアイテムが垂直方向に中央揃えで配置されています。 UWP スクリーンショットでは、すべての項目の下部がアラインされています。

個々の項目については、`AlignItems` 設定は、 [`AlignSelf`](#align-self)アタッチされたバインド可能なプロパティを使用してオーバーライドできます。

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent プロパティ

[`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent)プロパティは、次の7つのメンバーを持つ列挙体[`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent)型です。

- `Stretch`、既定値
- `Center`
- `Start` (XAML の "フレックス開始")
- `End` (XAML の "flex-end")
- `SpaceBetween` (XAML の "スペース間")
- `SpaceAround` (XAML の "領域の周囲")
- `SpaceEvenly`

`AlignItems`と同様に、`AlignContent` プロパティは交差軸でも子を配置しますが、行または列全体に影響します。

実験ページ![[:コンテンツ](flex-layout-images/ExperimentAlignContent.png "実験ページ-コンテンツの整列")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox) の整列

IOS のスクリーンショットでは、両方の行が一番上にあります。Android のスクリーンショットでは、中央に配置されています。UWP のスクリーンショットでは、一番下にあります。 行は、さまざまな方法で間隔を指定することもできます。

実験ページ![[:コンテンツの整列 2](flex-layout-images/ExperimentAlignContent2.png "実験ページ-コンテンツの整列2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

行または列が1つしかない場合、`AlignContent` は効果がありません。

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>アタッチ可能なバインド可能なプロパティの詳細

`FlexLayout` は、アタッチ可能な5つのバインド可能なプロパティを定義します。 これらのプロパティは、`FlexLayout` の子に設定され、その特定の子にのみ関連します。

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf プロパティ

アタッチ可能なバインド可能な[`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty)プロパティは、次の5つのメンバーを持つ列挙体[`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent)型です。

- `Auto`、既定値
- `Stretch`
- `Center`
- `Start` (XAML の "フレックス開始")
- `End` (XAML の "flex-end")

`FlexLayout`の個々の子について、このプロパティ設定は `FlexLayout` 自体に設定されている[`AlignItems`](#align-items)プロパティよりも優先されます。 `Auto` の既定の設定は、`AlignItems` の設定を使用することを意味します。

`label` (または例) という名前の `Label` 要素の場合は、次のようなコードで `AlignSelf` プロパティを設定できます。

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

`Label`の `FlexLayout` 親への参照がないことに注意してください。 XAML では、次のようにプロパティを設定します。

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order プロパティ

[`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty)プロパティの型は `int`です。 既定値は 0 です。

`Order` プロパティを使用すると、`FlexLayout` の子が配置される順序を変更できます。 通常、`FlexLayout` の子は、`Children` コレクションに表示される順序と同じ順序で配置されます。 この順序をオーバーライドするには、1つまたは複数の子の `Order` アタッチ可能なバインド可能なプロパティを0以外の整数値に設定します。 `FlexLayout` は、各子の `Order` プロパティの設定に基づいて子を配置しますが、同じ `Order` 設定を持つ子は `Children` コレクションに出現する順序で配置されます。

### <a name="the-basis-property"></a>Basis プロパティ

[`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty)アタッチされたバインド可能なプロパティは、メイン軸上の `FlexLayout` の子に割り当てられている領域の量を示します。 `Basis` プロパティによって指定されるサイズは、親 `FlexLayout`の主軸に沿ったサイズです。 したがって、`Basis` は、子が行に配置されたときの子の幅、または子が列に配置されるときの高さを示します。

`Basis` プロパティは、構造体[`FlexBasis`](xref:Xamarin.Forms.FlexBasis)型です。 サイズは、デバイスに依存しない単位で指定することも、`FlexLayout`のサイズに対する割合として指定することもできます。 `Basis` プロパティの既定値は `FlexBasis.Auto`静的プロパティです。これは、子の要求された幅または高さが使用されることを意味します。

コードでは、`label` という名前の `Label` の `Basis` プロパティを、次のように40デバイスに依存しない単位に設定できます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

`FlexBasis` コンストラクターの2番目の引数は `isRelative` という名前で、サイズが相対 (`true`) と絶対 (`false`) のどちらであるかを示します。 引数には `false`の既定値があるため、次のコードを使用することもできます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

`float` から `FlexBasis` への暗黙的な変換が定義されているため、さらに単純化できます。

```csharp
FlexLayout.SetBasis(label, 40);
```

次のように、サイズを `FlexLayout` の親の25% に設定できます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

この小数部の値は、0 ~ 1 の範囲で指定する必要があります。

XAML では、デバイスに依存しない単位でサイズの数値を使用できます。

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

または、0 ~ 100% の範囲でパーセントを指定することもできます。

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **[Basis 実験]** ページでは、`Basis` プロパティを試してみることができます。 ページには、背景色と前景色が交互に表示される5つの `Label` 要素のラップされた列が表示されます。 2つの `Slider` 要素を使用して、2番目と4番目の `Label`の `Basis` 値を指定できます。

[![[ベース実験] ページ](flex-layout-images/BasisExperiment.png "[ベース実験] ページ")](flex-layout-images/BasisExperiment-Large.png#lightbox)

左側の iOS のスクリーンショットは、デバイスに依存しない単位で高さが指定されている2つの `Label` 要素を示しています。 Android の画面には、`FlexLayout`の合計高さの一部である高さが表示されます。 `Basis` が100% に設定されている場合、子は `FlexLayout`の高さになり、次の列に折り返され、その列の高さ全体が表示されます。これは、UWP スクリーンショットで示すようになります。5つの子が1つの行に配置されているように見えますが、実際には5つの列に配置されています。

### <a name="the-grow-property"></a>"拡大" プロパティ

アタッチ可能なバインド可能な[`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty)プロパティの型は `int`です。 既定値は0で、値は0以上である必要があります。

`Grow` プロパティは、`Wrap` プロパティが `NoWrap` に設定され、子の行の合計幅が `FlexLayout`の幅よりも小さい場合、または子の列の高さが `FlexLayout`よりも短い場合に、ロールを再生します。 `Grow` プロパティは、子の間に残されているスペースを割り当てる方法を示します。

**[実験の拡大]** ページでは、交互の色の5つの `Label` 要素が1列に配置され、2つの `Slider` 要素を使用して2番目と4番目の `Label`の `Grow` プロパティを調整できます。 左側にある iOS のスクリーンショットは、既定の `Grow` プロパティを0で示しています。

[![[実験の拡大] ページ](flex-layout-images/GrowExperiment.png "[実験の拡大] ページ")](flex-layout-images/GrowExperiment-Large.png#lightbox)

1つの子に正の `Grow` 値が指定されている場合、その子は、Android のスクリーンショットに示すように、残りのすべての領域を占有します。 この領域は、2つ以上の子の間に割り当てることもできます。 UWP スクリーンショットでは、2番目の `Label` の [`Grow`] プロパティが0.5 に設定されていますが、4番目の `Label` の [`Grow`] プロパティは1.5 で、4番目の `Label` は2番目の `Label`の残りの領域の3倍になります。

子ビューがその領域をどのように使用するかは、特定の子の種類によって異なります。 `Label`の場合、プロパティ `HorizontalTextAlignment` および `VerticalTextAlignment`を使用して、`Label` の合計領域内にテキストを配置できます。

<a name="shrink" />

### <a name="the-shrink-property"></a>Shrink プロパティ

アタッチ可能なバインド可能な[`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty)プロパティの型は `int`です。 既定値は1で、値は0以上である必要があります。

`Shrink` プロパティは、`Wrap` プロパティが `NoWrap` に設定されていて、子の行の集計幅が `FlexLayout`の幅よりも大きい場合、または子の1つの列の集計の高さが、の高さを超えている場合に、ロールを再生します。`FlexLayout`。 通常、`FlexLayout` は、これらの子のサイズを constricting して表示します。 `Shrink` プロパティでは、どの子に優先順位が割り当てられているかを示すことができます。

**[実験の縮小]** ページでは、`FlexLayout` の幅よりも多くの領域を必要とする5つの `Label` 子を持つ1行の `FlexLayout` が作成されます。 左側の iOS のスクリーンショットには、すべての `Label` 要素が既定値1で表示されています。

[![[縮小実験] ページ](flex-layout-images/ShrinkExperiment.png "[縮小実験] ページ")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android のスクリーンショットでは、2番目の `Label` の `Shrink` 値は0に設定され、その `Label` は完全な幅で表示されます。 また、4番目の `Label` には1より大きい `Shrink` 値が割り当てられ、圧縮されています。 UWP スクリーンショットでは、可能であれば、`Label` の要素の両方に0の `Shrink` 値を指定して、それらを完全なサイズで表示できるようにしています。

`Grow` と `Shrink` の両方の値を設定して、集計された子のサイズが `FlexLayout`のサイズよりも小さいか、または大きい場合もあります。

## <a name="css-styling-with-flexlayout"></a>FlexLayout を使用した CSS スタイル

`FlexLayout`との接続では、Xamarin. Forms 3.0 で導入された[CSS スタイル](~/xamarin-forms/user-interface/styles/css/index.md)設定機能を使用できます。 **[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの **[css catalog items]** ページでは、 **[カタログアイテム]** ページのレイアウトが複製されますが、多くのスタイルの css スタイルシートがあります。

[![[CSS カタログアイテム] ページ](flex-layout-images/CssCatalogItems.png "[CSS カタログアイテム] ページ")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

元の**CatalogItemsPage**ファイルの `Resources` セクションには、15 `Setter` オブジェクトを持つ5つの `Style` 定義があります。 **CssCatalogItemsPage**ファイルでは、4つの `Setter` オブジェクトだけを持つ2つの `Style` 定義に縮小されています。 これらのスタイルは、現在、Xamarin の CSS スタイル機能がサポートしていないプロパティの CSS スタイルシートを補完します。

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

CSS スタイルシートは、`Resources` セクションの最初の行で参照されています。

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

また、3つの各項目の2つの要素に `StyleClass` 設定が含まれていることにも注意してください。

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

これらは、 **Catalogitemsstyles. css**スタイルシートのセレクターを参照します。

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

ここでは、いくつかの `FlexLayout` アタッチされたバインド可能なプロパティを参照します。 `label.empty` セレクターに `flex-grow` 属性が表示されます。この属性は、空の `Label` をスタイルとして、`Button`の上に空白を入力します。 `image` セレクターには、`order` 属性と `align-self` 属性が含まれており、どちらも `FlexLayout` アタッチされたバインド可能なプロパティに対応しています。

`FlexLayout` にプロパティを直接設定できることがわかりました。また、`FlexLayout`の子に対して、アタッチ可能なバインド可能なプロパティを設定することもできます。 また、従来の XAML ベースのスタイルまたは CSS スタイルを使用して、これらのプロパティを間接的に設定することもできます。 重要なのは、これらのプロパティについて理解し、理解しておくことです。 これらのプロパティにより、`FlexLayout` 真に柔軟になります。

## <a name="flexlayout-with-xamarinuniversity"></a>Xamarin 大学を使用した FlexLayout

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin. Forms 3.0 のフレックスレイアウトビデオ**

## <a name="related-links"></a>関連リンク

- [FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
