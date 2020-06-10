---
title: " Xamarin.Forms flexlayout" の説明: "子ビューのコレクションを積み重ねる場合またはラッピングする場合に FlexLayout を使用します。"
ms. 製品: xamarin ms. assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8: xamu-ビデオ作成者: davidbritch ミリ秒: dabritch ms. date: 05/07/2018 no loc: [ Xamarin.Forms ,、] を指定します。 Xamarin.Essentials
---

# <a name="the-xamarinforms-flexlayout"></a>Xamarin.FormsFlexlayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_子ビューのコレクションを積み重ねる場合やラッピングする場合は、FlexLayout を使用します。_

は、 Xamarin.Forms [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) Xamarin.Forms バージョン3.0 の新バージョンです。 これは、CSS の[柔軟なボックスレイアウトモジュール](https://www.w3.org/TR/css-flexbox-1/)に基づいています。_フレックスレイアウト_または_フレックスボックス_と呼ばれます。これは、レイアウト内に子を配置するための柔軟なオプションが多数含まれているためです。

`FlexLayout`はと似ていますが、 Xamarin.Forms [`StackLayout`](~/xamarin-forms/user-interface/layouts/stacklayout.md) スタック内で子を水平方向および垂直方向に整列させることができます。 ただし、では、 `FlexLayout` 1 つの行または列に収まりきらない場合に、子をラップすることもできます。また、さまざまな画面サイズに合わせて向きや配置を行うためのオプションも多数用意されています。

`FlexLayout`[`Layout<View>`](xref:Xamarin.Forms.Layout`1)は、から派生し、 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 型のプロパティを継承し `IList<View>` ます。

`FlexLayout`6つのパブリックバインド可能プロパティと、子要素のサイズ、向き、および配置に影響を与える、アタッチ可能な5つのバインド可能なプロパティを定義します。 (アタッチ可能なバインド可能なプロパティの詳細については、「**[アタッチさ](~/xamarin-forms/xaml/attached-properties.md)** れたプロパティ」を参照してください)。これらのプロパティの詳細については、バインド可能な**[プロパティ](#the-bindable-properties-in-detail)** について詳しく説明し、アタッチ可能なバインド可能な**[プロパティ](#the-attached-bindable-properties-in-detail)** について詳しく説明します。 ただし、この記事では、これらのプロパティの多くをより簡単に説明するいくつかの**[一般的な使用シナリオ](#common-usage-scenarios)** について `FlexLayout` 説明します。 記事の最後に、 `FlexLayout` [CSS スタイルシート](~/xamarin-forms/user-interface/styles/css/index.md)と組み合わせる方法について説明します。

## <a name="common-usage-scenarios"></a>一般的な利用シナリオ

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルプログラムには、の一般的な使用方法を示すいくつかのページが含まれて `FlexLayout` おり、そのプロパティを試してみることができます。

### <a name="using-flexlayout-for-a-simple-stack"></a>単純なスタックに FlexLayout を使用する

[**単純なスタック**] ページでは `FlexLayout` 、を、より単純なマークアップでに置き換える方法を示して `StackLayout` います。 このサンプルの内容はすべて、XAML ページで定義されています。 には、 `FlexLayout` 次の4つの子が含まれます。

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

`FlexLayout` **Simplestackpage .xaml**ファイルには、次の3つのプロパティが表示されます。

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)プロパティは、列挙体の値に設定され [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) ます。 既定値は、`Row` です。 プロパティをに設定する `Column` と、の子が `FlexLayout` 項目の1つの列に配置されます。

    内の項目 `FlexLayout` が列に配置されている場合、は、 `FlexLayout` 垂直方向の_メイン軸_と横方向の_交差軸_を持つと言います。

- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは型で、 [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 交差軸に項目をどのように揃えるかを指定します。 `Center`オプションを指定すると、各項目が水平方向に中央揃えで配置されます。

    このタスクでではなくを使用していた場合は、 `StackLayout` `FlexLayout` `HorizontalOptions` 各項目のプロパティをに割り当てて、すべての項目を中央に配置し `Center` ます。 `HorizontalOptions`プロパティはの子に対しては機能しません `FlexLayout` が、1つのプロパティが `AlignItems` 同じ目的を達成します。 必要に応じて、アタッチ可能なバインド可能プロパティを使用して、 `AlignSelf` `AlignItems` 個々の項目のプロパティをオーバーライドできます。

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    この変更により、 `Label` `FlexLayout` 読み取り順序が左から右になると、この1つがの左端に配置されます。

- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティは型 [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) で、項目をメイン軸にどのように配置するかを指定します。 オプションを指定すると、すべてのアイテムが、最初のアイテムと最後のアイテムの下のすべてのアイテムに均等に割り当てられ `SpaceEvenly` ます。

    を使用していた場合は `StackLayout` 、 `VerticalOptions` 同様の効果を得るために、各項目のプロパティをに割り当てる必要があり `CenterAndExpand` ます。 ただし、このオプションでは、 `CenterAndExpand` 最初の項目の前と最後の項目の後に、各項目の間に2倍の領域が割り当てられます。 の `CenterAndExpand` `VerticalOptions` プロパティをに設定すると、のオプションを模倣でき `JustifyContent` `FlexLayout` `SpaceAround` ます。

これらのプロパティの詳細に `FlexLayout` ついては、後の「バインド可能な**[プロパティ](#the-bindable-properties-in-detail)**」で詳しく説明します。

### <a name="using-flexlayout-for-wrapping-items"></a>項目のラップに FlexLayout を使用する

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** のサンプルの [**写真の折り返し**] ページでは、 `FlexLayout` がその子を追加の行または列にラップする方法を示しています。 XAML ファイルによってがインスタンス化され、 `FlexLayout` その2つのプロパティが割り当てられます。

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

こののプロパティは設定されていないので、既定の設定であるになり `Direction` `FlexLayout` `Row` ます。つまり、子が行に配置され、メイン軸が水平になります。

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは列挙型 [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) です。 項目が多すぎて行に収まらない場合は、このプロパティを設定すると、項目が次の行に折り返されます。

がの子であることに注意して `FlexLayout` `ScrollView` ください。 ページに収まりきらない行が多い場合、には `ScrollView` の既定のプロパティが設定され、 `Orientation` `Vertical` 垂直スクロールが可能になります。

プロパティは、 `JustifyContent` 各項目が同じサイズの空白で囲まれるように、メイン軸 (水平軸) に残っている領域を割り当てます。

分離コードファイルは、サンプル写真のコレクションにアクセスし `Children` 、のコレクションに追加し `FlexLayout` ます。

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

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの**究極レイアウト**ページでは、 `FlexLayout` 別ので入れ子になっているものを使用して、このレイアウトの単純な実装を示しています。 このページは縦モードの携帯電話向けに設計されているため、コンテンツエリアの左右の領域は50ピクセル幅のみです。

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

ナビゲーション領域と残り領域は、を使用して左右にレンダリングされ `BoxView` ます。

XAML ファイルの最初のは、 `FlexLayout` 垂直方向のメイン軸を持ち、列に配置された3つの子を含みます。 これらは、ヘッダー、ページの本文、およびフッターです。 入れ子になったに `FlexLayout` は、3つの子が行内に配置された水平方向のメイン軸があります。

このプログラムでは、アタッチ可能な3つのバインド可能なプロパティが示されています。

- `Order`アタッチ可能なバインド可能なプロパティは、最初のに設定され `BoxView` ます。 このプロパティは整数で、既定値は0です。 このプロパティを使用して、レイアウトの順序を変更できます。 通常、開発者は、ナビゲーション項目および項目の前に、ページの内容をマークアップで表示することを希望しています。 `Order`最初ののプロパティを `BoxView` 他の兄弟よりも小さい値に設定すると、行の最初の項目として表示されます。 同様に、 `Order` プロパティを兄弟よりも大きい値に設定することによって、項目が最後に表示されるようにすることができます。

- 添付可能なバインド可能な `Basis` プロパティは、2つの項目に対して `BoxView` 50 ピクセルの幅を与えるように設定されています。 このプロパティは、 `FlexBasis` という型の静的プロパティを定義する構造体であり、 `FlexBasis` `Auto` 既定値です。 を使用すると、 `Basis` メイン軸上で項目が占める領域を示すピクセルサイズまたはパーセンテージを指定できます。 これは、後続のすべてのレイアウトの基礎となる項目サイズを指定するため、_ベース_として呼び出されます。

- `Grow`プロパティは、入れ子になっていると、コンテンツを表す子に設定され `Layout` `Label` ます。 このプロパティの型は `float` で、既定値は0です。 正の値に設定すると、メイン軸に沿った残りのすべての領域が、その項目との正の値を持つ兄弟に割り当てられ `Grow` ます。 領域は、のスター指定と同様に、値に比例して割り当てられ `Grid` ます。

    最初の `Grow` 添付プロパティは、入れ子になったに設定され `FlexLayout` `FlexLayout` ます。これは、外側の内の未使用のすべての垂直方向の領域を占有することを示し `FlexLayout` ます。 2番目の `Grow` 添付プロパティは、コンテンツを表すに設定されます `Label` 。これは、このコンテンツが内部のすべての未使用の領域を占有することを示し `FlexLayout` ます。

    また、同様のアタッチ可能なバインド可能なプロパティもあり `Shrink` ます。これは、子のサイズがのサイズを超えても、折り返しが不要な場合に使用でき `FlexLayout` ます。

### <a name="catalog-items-with-flexlayout"></a>FlexLayout を使用したカタログアイテム

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの [**カタログ項目**] ページは、 [CSS フレックスレイアウトボックスの仕様のセクション1.1 の例 1](https://www.w3.org//TR/css-flexbox-1/#overview)と似ています。ただし、水平スクロール可能な一連の画像と3つの猿の説明が表示されます。

[![[カタログアイテム] ページ](flex-layout-images/CatalogItems.png "[カタログアイテム] ページ")](flex-layout-images/CatalogItems-Large.png#lightbox)

3つの各猿は、 `FlexLayout` に含まれるに含まれており、 `Frame` 明示的な高さと幅が指定され、さらにの子でも `FlexLayout` あります。 この XAML ファイルでは、子のほとんどのプロパティがスタイルで指定されていますが、そのうち `FlexLayout` の1つだけが暗黙的なスタイルです。

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

の暗黙的なスタイルには、 `Image` 次の2つのアタッチ可能なバインド可能なプロパティの設定が含まれ `Flexlayout` ます。

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order`1 に設定すると、 &ndash; `Image` 子コレクション内の位置に関係なく、入れ子になった各ビューに要素が最初に表示され `FlexLayout` ます。 `AlignSelf`のプロパティに `Center` より、が内の `Image` 中央に配置され `FlexLayout` ます。 これにより、プロパティの設定がオーバーライドされます。これは、 `AlignItems` 既定値が `Stretch` であることを意味し `Label` ます。つまり、との子は、の `Button` 完全な幅に拡大され `FlexLayout` ます。

3つの各ビュー内では、はの `FlexLayout` 前に空白に `Label` `Button` `Grow` なりますが、設定は1になります。 これは、すべての余分な垂直空間がこの空白に割り当てられることを意味します。これにより `Label` 、が実質的 `Button` に一番下にプッシュされます。

## <a name="the-bindable-properties-in-detail"></a>バインド可能なプロパティの詳細

一般的なアプリケーションをいくつか紹介したので `FlexLayout` 、のプロパティを `FlexLayout` より詳細に調べることができます。
`FlexLayout``FlexLayout`向きと配置を制御するために、コードまたは XAML で自身に設定する、6つのバインド可能なプロパティを定義します。 (これらのプロパティの1つ [`Position`](xref:Xamarin.Forms.FlexLayout.Position) は、この記事では説明されていません)。

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** のサンプルの**実験**ページを使用して、残りの5つのバインド可能なプロパティを試すことができます。 このページでは、との間で子を追加または削除し、バインド可能な `FlexLayout` 5 つのプロパティの組み合わせを設定できます。 のすべての子は `FlexLayout` 、 `Label` さまざまな色とサイズのビューです `Text` 。プロパティは、コレクション内の位置に対応する数値に設定され `Children` ます。

プログラムが起動すると、5つ `Picker` のビューにこれら5つのプロパティの既定値が表示さ `FlexLayout` れます。 `FlexLayout`画面の下部には、次の3つの子があります。

[![実験ページ: 既定](flex-layout-images/ExperimentDefault.png "実験ページ-既定")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

各ビューには `Label` 、内に割り当てられた領域を示す灰色の背景が表示され `Label` `FlexLayout` ます。 自身の背景 `FlexLayout` は Alice Blue です。 左右の余白を除いて、ページの一番下の領域全体を占めます。

### <a name="the-direction-property"></a>Direction プロパティ

[`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)プロパティは [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) 、4つのメンバーを持つ列挙体である型です。

- `Column`
- `ColumnReverse`(XAML の "列反転")
- `Row` (既定値)
- `RowReverse`(XAML の "行の逆")

XAML では、小文字、大文字、または混合ケースで列挙メンバー名を使用して、このプロパティの値を指定できます。または、CSS インジケーターと同じかっこで囲まれた2つの追加文字列を使用することもできます。 ("列逆" と "行反転" の文字列は、 [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML パーサーによって使用されるクラスで定義されています)。

(左から右)、方向、方向、方向の各**実験**ページを次に示し `Row` `Column` `ColumnReverse` ます。

[![実験ページ: 方向](flex-layout-images/ExperimentDirection.png "実験ページの方向")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

オプションの場合、 `Reverse` 項目は右側または下部から開始されることに注意してください。

### <a name="the-wrap-property"></a>Wrap プロパティ

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) 、次の3つのメンバーを持つ列挙体である型です。

- `NoWrap` (既定値)
- `Wrap`
- `Reverse`(XAML の "ラップ反転")

左から右に、これらの画面に `NoWrap` は `Wrap` 、 `Reverse` 12 個の子のオプションとオプションが表示されます。

[![実験ページ: Wrap](flex-layout-images/ExperimentWrap.png "実験ページの折り返し")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

`Wrap`プロパティがに設定されていて、 `NoWrap` メイン軸が (このプログラムのように) 制約されていて、メインの軸がすべての子を収めるのに十分ではない場合は、 `FlexLayout` iOS のスクリーンショットに示すように、項目のサイズを小さくします。 アタッチ可能なバインド可能なプロパティを使用して、項目の shrinkness を制御でき [`Shrink`](#the-shrink-property) ます。

### <a name="the-justifycontent-property"></a>ジャスト Ifycontent プロパティ

[`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティは [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) 、次の6つのメンバーを持つ列挙体である型です。

- `Start`(XAML の "フレックス開始")、既定値
- `Center`
- `End`(XAML の "flex-end")
- `SpaceBetween`(XAML の "スペース間")
- `SpaceAround`(XAML の "スペース")
- `SpaceEvenly`

このプロパティは、次の例の横軸であるメイン軸上のアイテムの間隔を指定します。

[![実験ページ: コンテンツのジャスティフィケーション](flex-layout-images/ExperimentJustifyContent.png "実験ページ-コンテンツのジャスティフィケーション")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

3つのすべてのスクリーンショットで `Wrap` は、プロパティはに設定されて `Wrap` います。 `Start`既定値は、前の Android スクリーンショットに示されています。 IOS のスクリーンショットは、次のオプションを示してい `Center` ます。すべての項目が中央に移動されます。 単語で始まる他の3つのオプションでは、 `Space` 項目によって占有されていない余分な領域が割り当てられます。 `SpaceBetween`項目間にスペースを均等に割り当てます。`SpaceAround`は各項目の周囲に均等なスペースを置き、は `SpaceEvenly` 各項目の前、最初の項目の前、および行の最後の項目の後に同じ間隔を置きます。

### <a name="the-alignitems-property"></a>AlignItems プロパティ

[`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 、4つのメンバーを持つ列挙体である型です。

- `Stretch` (既定値)
- `Center`
- `Start`(XAML の "フレックス開始")
- `End`(XAML の "flex-end")

これは2つのプロパティ (もう1つは) の1つであり [`AlignContent`](#the-aligncontent-property) 、交差軸上での子のアライン方法を示します。 各行内では、次の3つのスクリーンショットに示すように、子が拡大されます (前のスクリーンショットに示したように)。または、各項目の開始、中央、または末尾に配置されます。

[![実験ページ: 項目の整列](flex-layout-images/ExperimentAlignItems.png "実験ページ-項目の整列")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

IOS のスクリーンショットでは、すべての子の上部が揃っています。 Android のスクリーンショットでは、最も高い子に基づいてアイテムが垂直方向に中央揃えで配置されています。 UWP スクリーンショットでは、すべての項目の下部がアラインされています。

個々の項目については、この設定は、 `AlignItems` アタッチ可能なバインド可能なプロパティでオーバーライドでき [`AlignSelf`](#the-alignself-property) ます。

### <a name="the-aligncontent-property"></a>AlignContent プロパティ

[`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent)プロパティは [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent) 、7つのメンバーを持つ列挙体である型です。

- `Stretch` (既定値)
- `Center`
- `Start`(XAML の "フレックス開始")
- `End`(XAML の "flex-end")
- `SpaceBetween`(XAML の "スペース間")
- `SpaceAround`(XAML の "スペース")
- `SpaceEvenly`

と同様 `AlignItems` に、 `AlignContent` プロパティは交差軸でも子を配置しますが、行または列全体に影響します。

[![実験ページ: コンテンツの整列](flex-layout-images/ExperimentAlignContent.png "実験ページ-コンテンツの整列")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

IOS のスクリーンショットでは、両方の行が一番上にあります。Android のスクリーンショットでは、中央に配置されています。UWP のスクリーンショットでは、一番下にあります。 行は、さまざまな方法で間隔を指定することもできます。

[![実験ページ: コンテンツの整列2](flex-layout-images/ExperimentAlignContent2.png "実験ページ-コンテンツの整列2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent`行または列が1つしかない場合、は効果がありません。

## <a name="the-attached-bindable-properties-in-detail"></a>アタッチ可能なバインド可能なプロパティの詳細

`FlexLayout`アタッチ可能な5つのバインド可能なプロパティを定義します。 これらのプロパティは、の子に対して設定され、 `FlexLayout` その特定の子にのみ関係します。

### <a name="the-alignself-property"></a>AlignSelf プロパティ

アタッチ可能なバインド可能な [`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) プロパティは [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent) 、次の5つのメンバーを持つ列挙体である型です。

- `Auto` (既定値)
- `Stretch`
- `Center`
- `Start`(XAML の "フレックス開始")
- `End`(XAML の "flex-end")

の個々の子については、このプロパティ設定によって、 `FlexLayout` 自体に設定されているプロパティがオーバーライドされ [`AlignItems`](#the-alignitems-property) `FlexLayout` ます。 の既定の設定は、 `Auto` 設定を使用することを意味し `AlignItems` ます。

`Label`という名前の要素 `label` (または例) では、次の `AlignSelf` ようなコードでプロパティを設定できます。

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

の親への参照がないことに注意 `FlexLayout` `Label` してください。 XAML では、次のようにプロパティを設定します。

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order プロパティ

[`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty)プロパティの型は `int` です。 既定値は 0 です。

`Order`プロパティを使用すると、の子が配置される順序を変更でき `FlexLayout` ます。 通常、の子は、 `FlexLayout` コレクション内に出現する順序と同じ順序で配置され `Children` ます。 この順序をオーバーライドするには、 `Order` 1 つまたは複数の子で、アタッチ可能なバインド可能なプロパティを0以外の整数値に設定します。 は、 `FlexLayout` 各子のプロパティの設定に基づいて子を配置し `Order` ますが、同じ設定を持つ子 `Order` は、コレクション内に出現する順序で配置され `Children` ます。

### <a name="the-basis-property"></a>Basis プロパティ

アタッチ可能なバインド可能プロパティは、 [`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty) メイン軸上のの子に割り当てられている領域の量を示し `FlexLayout` ます。 プロパティで指定されるサイズ `Basis` は、親のメイン軸に沿ったサイズです `FlexLayout` 。 したがって、子が `Basis` 行に配置されている場合は子の幅、子が列に配置される場合は高さを示します。

`Basis`プロパティは [`FlexBasis`](xref:Xamarin.Forms.FlexBasis) 、構造体である型です。 サイズは、デバイスに依存しない単位で指定することも、のサイズの割合として指定することもでき `FlexLayout` ます。 プロパティの既定値 `Basis` は静的なプロパティです `FlexBasis.Auto` 。これは、子の要求された幅または高さが使用されることを意味します。

コードでは、 `Basis` という名前ののプロパティを、次の `Label` ように `label` 40 デバイスに依存しない単位に設定できます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

コンストラクターの2番目の引数に `FlexBasis` はという名前が付けられ、 `isRelative` サイズが相対 () と絶対 () のどちらであるかが示され `true` `false` ます。 引数の既定値はである `false` ため、次のコードを使用することもできます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

からへの暗黙の型変換が定義されている `float` `FlexBasis` ため、さらに簡略化できます。

```csharp
FlexLayout.SetBasis(label, 40);
```

次のように、サイズを親の25% に設定でき `FlexLayout` ます。

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

**[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの**基本的な実験**ページでは、プロパティを試してみることができ `Basis` ます。 ページには、 `Label` 背景色と前景色が交互に表示される5つの要素からなる、ラップされた列が表示されます。 2つ `Slider` の要素を使用して `Basis` 、2番目と4番目の値を指定でき `Label` ます。

[![[ベース実験] ページ](flex-layout-images/BasisExperiment.png "[ベース実験] ページ")](flex-layout-images/BasisExperiment-Large.png#lightbox)

左側の iOS のスクリーンショットは、 `Label` デバイスに依存しない単位で高さが指定されている2つの要素を示しています。 Android の画面には、の高さの割合で表示される高さが示されてい `FlexLayout` ます。 が100% に設定されている場合、子はの `Basis` 高さに `FlexLayout` なり、次の列にラップされ、その列の高さ全体を占めます。これは、UWP スクリーンショットのように、5つの子が行に配置されているように見えますが、実際には5つの列に配置されています。

### <a name="the-grow-property"></a>"拡大" プロパティ

アタッチ可能なバインド可能な [`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty) プロパティの型は `int` です。 既定値は0で、値は0以上である必要があります。

プロパティ `Grow` `Wrap` がに設定され、 `NoWrap` 子の行の合計幅がの幅よりも小さい場合、 `FlexLayout` または子の列の高さがよりも短い場合、プロパティはロールを再生し `FlexLayout` ます。 プロパティは、 `Grow` 子の間に残されているスペースを割り当てる方法を示します。

[**実験の拡大**] ページでは、 `Label` 交互の色の5つの要素が1つの列に配置され、2つの要素を使用して `Slider` `Grow` 2 番目と4番目のプロパティを調整でき `Label` ます。 一番左にある iOS スクリーンショットは、既定のプロパティである0を示してい `Grow` ます。

[![[実験の拡大] ページ](flex-layout-images/GrowExperiment.png "[実験の拡大] ページ")](flex-layout-images/GrowExperiment-Large.png#lightbox)

1つの子に正の値が指定されている場合 `Grow` 、その子は、Android のスクリーンショットに示すように、残りのすべての領域を占有します。 この領域は、2つ以上の子の間に割り当てることもできます。 UWP スクリーンショットでは、 `Grow` 2 番目のプロパティは0.5 に設定されていますが、4番目 `Label` のプロパティは1.5 で、残りの `Grow` `Label` `Label` 領域の2倍が2番目に `Label` なります。

子ビューがその領域をどのように使用するかは、特定の子の種類によって異なります。 では、 `Label` `Label` プロパティとを使用して、の合計領域内にテキストを配置できます `HorizontalTextAlignment` `VerticalTextAlignment` 。

### <a name="the-shrink-property"></a>Shrink プロパティ

アタッチ可能なバインド可能な [`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) プロパティの型は `int` です。 既定値は1で、値は0以上である必要があります。

プロパティ `Shrink` `Wrap` がに設定され、 `NoWrap` 子の行の集計幅がの幅より大きい場合、 `FlexLayout` または子の1つの列の集計の高さがの高さを超える場合、プロパティはロールを再生し `FlexLayout` ます。 通常、では、 `FlexLayout` これらの子のサイズが constricting に表示されます。 プロパティは、 `Shrink` どの子に優先順位が与えられているかを示します。

[**実験の縮小**] ページでは、 `FlexLayout` `Label` 幅よりも多くの領域を必要とする5つの子の単一行を持つが作成され `FlexLayout` ます。 左側の iOS のスクリーンショットには、 `Label` 既定値が1のすべての要素が表示されています。

[![[縮小実験] ページ](flex-layout-images/ShrinkExperiment.png "[縮小実験] ページ")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android のスクリーンショットで `Shrink` は、2番目の値が0に設定されており、それが `Label` `Label` 完全な幅で表示されています。 また、4番目のに `Label` は `Shrink` 1 より大きい値が割り当てられ、圧縮されています。 UWP スクリーンショットで `Label` は、可能であれ `Shrink` ば、両方の要素の値が0であることが示されています。

との両方の値を設定して、 `Grow` `Shrink` 子の集計サイズがのサイズよりも小さいか、または大きい場合がある状況に対応することができ `FlexLayout` ます。

## <a name="css-styling-with-flexlayout"></a>FlexLayout を使用した CSS スタイル

3.0 で導入された[CSS スタイル](~/xamarin-forms/user-interface/styles/css/index.md)機能は、 Xamarin.Forms との接続に使用でき `FlexLayout` ます。 **[Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** サンプルの [ **css catalog items** ] ページでは、[**カタログアイテム**] ページのレイアウトが複製されますが、多くのスタイルの css スタイルシートがあります。

[![[CSS カタログアイテム] ページ](flex-layout-images/CssCatalogItems.png "[CSS カタログアイテム] ページ")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

元の**CatalogItemsPage**ファイルのセクションには、15個の `Style` オブジェクトを含む5つの定義があり `Resources` `Setter` ます。 **CssCatalogItemsPage**ファイルでは、4つのオブジェクトだけを持つ2つの定義に縮小されてい `Style` `Setter` ます。 これらのスタイルは、css スタイル機能で現在サポートされていないプロパティの CSS スタイルシートを補完し Xamarin.Forms ます。

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

CSS スタイルシートは、セクションの最初の行で参照され `Resources` ます。

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

また、3つの各項目の2つの要素に設定が含まれていることに注目して `StyleClass` ください。

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

アタッチ可能ないくつか `FlexLayout` のバインド可能なプロパティをここで参照します。 セレクターに `label.empty` 属性が表示され `flex-grow` ます。この属性は `Label` 、の上に空白領域を提供するために空のをスタイル指定し `Button` ます。 セレクターには `image` 属性と属性が含まれており `order` `align-self` 、どちらもアタッチ可能なバインド可能なプロパティに対応してい `FlexLayout` ます。

ここでは、にプロパティを直接設定でき `FlexLayout` ます。また、の子に対して、アタッチ可能なバインド可能なプロパティを設定することもでき `FlexLayout` ます。 また、従来の XAML ベースのスタイルまたは CSS スタイルを使用して、これらのプロパティを間接的に設定することもできます。 重要なのは、これらのプロパティについて理解し、理解しておくことです。 これらのプロパティを使用すると、真に柔軟になり `FlexLayout` ます。

## <a name="flexlayout-with-xamarinuniversity"></a>Xamarin 大学を使用した FlexLayout

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms3.0 フレックスレイアウトビデオ**

## <a name="related-links"></a>関連リンク

- [FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
