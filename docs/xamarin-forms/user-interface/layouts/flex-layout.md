---
title: Xamarin.Forms FlexLayout
description: FlexLayout を使用して、スタックまたは子ビューのコレクションをラップします。
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 7585138cd6c33c2a5dc537ba28101a84e1c4b7ae
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

_FlexLayout を使用して、スタックまたは子ビューのコレクションをラップします。_

Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) Xamarin.Forms バージョン 3.0 の新機能はします。 これは CSS に基づいて[フレキシブル ボックス レイアウト モジュール](http://www.w3.org/TR/css-flexbox-1/)とよく呼ばれる、_レイアウトをフレックス_または_フレックス ボックス_、いわゆるを子を整列する多くの柔軟なオプションが含まれています内でのレイアウトです。

`FlexLayout` Xamarin.Forms に似ています[ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md)ことで配置できる子水平方向および垂直方向にスタックにします。 ただし、`FlexLayout`は 1 つの行または列に収まるようが多すぎますがある場合は、その子をラップすることも、向き、配置、およびさまざまな画面サイズに合わせて調整の多くのオプションがあります。

`FlexLayout` 派生した[ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)を継承し、 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/)型のプロパティ`IList<View>`です。

`FlexLayout` 6 つのパブリックなバインド可能なプロパティと、サイズ、向き、およびその子要素の配置に影響する 5 つの接続されているバインド可能なプロパティを定義します。 (接続されているバインド可能なプロパティに慣れていない場合は、記事を参照してください**[アタッチされるプロパティ](~/xamarin-forms/xaml/attached-properties.md)**。)。これらのプロパティに以下のセクションで詳しく説明されている**[バインド可能なプロパティの詳細](#bindable-properties)** と**[詳細に接続されているバインド可能なプロパティ](#attached-properties)**. この記事がいくつかのセクションで始まるただし、 **[一般的な使用シナリオ](#common-scenarios)** の`FlexLayout`より非公式のこれらのプロパティの多くを説明します。 記事の最後に、方向に結合する方法が分かります`FlexLayout`で[CSS スタイル シート](~/xamarin-forms/user-interface/styles/css/index.md)です。

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>一般的な使用シナリオ

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプル プログラムを含むページがいくつかの一般的な用途の demonstate`FlexLayout`とそのプロパティをテストすることができます。

### <a name="using-flexlayout-for-a-simple-stack"></a>単純なスタックの FlexLayout を使用します。

**単純なスタック**番組をどのようにページ`FlexLayout`代わりに使用できる、`StackLayout`が単純なマークアップを含むです。 このサンプルのすべての内容は、XAML ページに定義されます。 `FlexLayout` 4 つの子が含まれています。

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

IOS、Android、およびユニバーサル Windows プラットフォームで実行されているそのページを次に示します。

[![単純なページをスタックする](flex-layout-images/SimpleStack.png "単純なページをスタックします。")](flex-layout-images/SimpleStack-Large.png#lightbox)

3 つのプロパティの`FlexLayout`が表示されます、 **SimpleStackPage.xaml**ファイル。

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction)の値に設定されて、 [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection)列挙します。 既定値は、`Row` です。 プロパティを設定`Column`の子をにより、`FlexLayout`項目の 1 つの列に配置します。

    ときの項目を`FlexLayout`は、列に並べ、`FlexLayout`垂直方向と表現されます_主軸_と水平方向_軸クロス_です。

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティの型は[ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems)し、交差軸上のアイテムを整列する方法を指定します。 `Center`オプションは、各アイテムの水平方向に中央揃えにします。

    使用していた場合、`StackLayout`ではなく、 `FlexLayout` 、このタスクは、割り当てることによってすべての項目を中央揃え.、`HorizontalOptions`する各項目のプロパティ`Center`です。 `HorizontalOptions`の子のプロパティが機能しません、 `FlexLayout`、1 つが、`AlignItems`プロパティは、同じ目標を達成します。 使用することができますをする必要がある場合、`AlignSelf`添付バインド可能なプロパティを上書きする、`AlignItems`個々 のアイテムのプロパティ。

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    この変更によって`Label`がの左の端に配置されている、`FlexLayout`読み取り順序が左から右への場合。

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティの型は[ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)、メインの軸上のアイテムを配置する方法を指定します。 `SpaceEvenly`オプションはすべて残された垂直方向のスペースすべての項目間で均等にし、最初の項目では、上下にある最後の項目を割り当てます。

    使用していた場合、`StackLayout`は割り当てる必要があります、`VerticalOptions`する各項目のプロパティ`CenterAndExpand`と同様の効果を実現するためにします。 `CenterAndExpand`オプションは、各項目よりも、最初の項目の前後の最後の項目の間の 2 倍の空き領域を割り当てるとします。 模倣する、`CenterAndExpand`オプション`VerticalOptions`を設定して、`JustifyContent`プロパティの`FlexLayout`に`SpaceAround`です。

これら`FlexLayout`プロパティは、セクションで詳しく説明**[バインド可能なプロパティの詳細](#bindable-properties)** 以下です。

### <a name="using-flexlayout-for-wrapping-items"></a>項目をラップする FlexLayout を使用します。

**フォト ラッピング**のページ、 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルではどのように`FlexLayout`追加の行または列には、その子を折り返すことができます。 XAML ファイルのインスタンスを作成、`FlexLayout`し、その 2 つのプロパティを割り当てます。

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

`Direction`このプロパティ`FlexLayout`が設定されていないので、既定の設定の`Row`子は行に配置され、メインの軸は水平方向ことを意味します。

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap)列挙型のプロパティは、 [ `FlexWrap`](xref:Xamarin.Forms.FlexWrap)です。 行に収まらない項目が多すぎますがある場合は、このプロパティの設定は次の行をラップするアイテムをさせます。

注意して、`FlexLayout`の子である、`ScrollView`です。 ページに収まるように多くの行がある場合、 `ScrollView` 、既定値を持つ`Orientation`プロパティ`Vertical`垂直スクロールできるようにします。

`JustifyContent`プロパティが各項目は、一定の空白で囲まれているように、メイン軸 (水平軸) 上の未使用の領域を割り当てます。

分離コード ファイルは、サンプルの写真のコレクションにアクセスする、追加して、`Children`のコレクション、 `FlexLayout`:

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
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
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
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
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

次の 3 つのプラットフォームでは、下に上から段階的にスクロールされる基準で実行されているプログラムを次に示します。

[![写真の折り返しページ](flex-layout-images/PhotoWrapping.png "写真の折り返し ページ")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>FlexLayout とページ レイアウト

標準レイアウトと呼ばれる web デザインでは、 [_至高_](https://en.wikipedia.org/wiki/Holy_grail_(web_design))は、非常に望ましいことが完璧で実現する多くの場合、ハード レイアウト形式になっているためです。 レイアウトは、ページの上部にあるヘッダーとフッター下部で、両方のページの幅全体に拡張で構成されます。 コンテンツと補助情報の左側に単票形式メニューで多くの場合は、メインのコンテンツは、ページの中央を使用していた (とも呼ばれる、_確保しておく_領域)、右にあります。 [CSS のフレキシブル ボックスのレイアウトの仕様のセクション 5.4.1](http://www.w3.org/TR/css-flexbox-1/#order-accessibility)フレックス ボックス至高レイアウトを実現する方法について説明します。

**至高レイアウト**のページ、 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルは、このレイアウトを使用して 1 つの簡単な実装を示しています。`FlexLayout`入れ子の状態にします。 このページが縦向きモードの電話に設計されているため、コンテンツ領域の右側および左側の領域がのみ幅 50 ピクセルに。

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

ここで、次の 3 つのプラットフォームで実行されています。

[![至高レイアウト ページ](flex-layout-images/HolyGrailLayout.png "至高レイアウト ページ")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

ナビゲーションおよび aside 分野をレンダリングする際、`BoxView`左側および右側にします。

最初の`FlexLayout`xaml ファイルにメイン縦軸があり、列に配置する 3 つの子が含まれています。 これらは、ヘッダー、ページ、およびフッターの本文です。 入れ子になった`FlexLayout`メイン横軸を持つ行に配置された 3 つの子です。

このプログラムでは、次の 3 つの接続されているバインド可能なプロパティがについて説明します。

- `Order`接続されているバインド可能なプロパティが設定を最初に`BoxView`です。 このプロパティは、既定値 0 は整数です。 レイアウトの順序を変更するのには、このプロパティを使用することができます。 一般に開発者は、ナビゲーション項目の前にマークアップに表示されるページのコンテンツを優先し、項目を確保します。 設定、`Order`を最初にプロパティ`BoxView`値にその他の兄弟よりも小さいと、その行の最初の項目として表示します。 同様に、することができます、項目が表示されるように最後を設定して、`Order`プロパティをその兄弟より大きい値にします。

- `Basis`接続されているバインド可能なプロパティが 2 つの`BoxView`50 ピクセルの幅を付与する項目。 このプロパティの型は`FlexBasis`、型の静的プロパティを定義する構造体`FlexBasis`という名前`Auto`、既定値です。 使用することができます`Basis`ピクセルのサイズまたは領域の量を示すパーセンテージを指定するアイテムが主軸を占有します。 呼び出された、_単位_後続のすべてのレイアウトの基盤となる項目のサイズを指定するためです。

- `Grow`プロパティが設定されて、入れ子になったで`Layout`および、`Label`コンテンツを表す子。 このプロパティの型は`float`あり、既定値は 0 です。 正の値に設定すると、メインの軸に沿ったの残りの領域をすべてとに割り当てられるそのアイテムへの正の値を持つ兄弟`Grow`です。 星型の仕様のようなものの値に、容量が比例的に割り当て、`Grid`です。

    最初の`Grow`添付プロパティの設定で入れ子になった`FlexLayout`を示す、この`FlexLayout`外側内、すべての未使用垂直領域を占有するように、`FlexLayout`です。 2 番目`Grow`添付プロパティが設定されて、`Label`このコンテンツが内部で使用されていないすべての左右の間隔を占有することを示す、内容を表す`FlexLayout`です。

    同様`Shrink`添付子のサイズが、サイズを超える場合に使用できるバインド可能なプロパティ、`FlexLayout`ラッピングが必要でないが、します。

### <a name="catalog-items-with-flexlayout"></a>FlexLayout でカタログ アイテム

**カタログ アイテム** ページで、 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルはのような[CSS フレックス レイアウト ボックス仕様の1.1のセクションの例1](http://www.w3.org/TR/css-flexbox-1/#overview)、水平方向にスクロール可能な一連の画像と 3 つ猿の説明が表示される点が異なります。

[![カタログ アイテム ページ](flex-layout-images/CatalogItems.png "カタログ アイテム ページ")](flex-layout-images/CatalogItems-Large.png#lightbox)

3 つの猿のそれぞれが、`FlexLayout`に含まれている、`Frame`明示的な高さと幅を指定してより容量の大きい子はまた`FlexLayout`です。 この XAML ファイルのプロパティのほとんどで、`FlexLayout`子は、1 つのうちは暗黙的なスタイルを除くすべてのスタイルで指定します。

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

暗黙的なスタイルを`Image`の 2 つの接続されているバインド可能なプロパティの設定が含まれています`Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order`の設定&ndash;1 原因、 `Image` 、入れ子になったのそれぞれに最初に表示される要素`FlexLayout`子コレクション内の位置に関係なくビュー。 `AlignSelf`プロパティ`Center`により、`Image`内で中央揃えにする、`FlexLayout`です。 設定が上書きされます。、`AlignItems`を既定値を持つプロパティの`Stretch`つまりを、`Label`と`Button`子のサイズの幅全体に拡大は、`FlexLayout`です。

内で、3 つ`FlexLayout`は空白を表示します。`Label`の前に、 `Button`、がある、 `Grow` 1 を設定します。 これには空白にすべての余分な縦方向の領域が割り当てられていることを意味`Label`、効果的にプッシュする、`Button`最下位に達しています。

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>詳細にバインド可能なプロパティ

一般的なアプリケーションを確認した`FlexLayout`、プロパティの`FlexLayout`さらに詳しく調査できます。 
`FlexLayout` 設定した 6 つのバインド可能なプロパティを定義、`FlexLayout`自体であるため、コードまたはコントロール orientatin と配置に、XAML のいずれか。 (これらのプロパティのいずれかの[ `Position` ](xref:Xamarin.Forms.FlexLayout.Position)、この記事では説明しません)。

使用してバインド可能なプロパティの残りの 5 つを試すことができます、**実験**のページ、 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルです。 このページでは、追加または子要素を削除することができます、`FlexLayout`し、5 つのバインド可能なプロパティの組み合わせを設定します。 すべての子、`FlexLayout`は`Label`さまざまな色、およびサイズのビューで、`Text`プロパティ内での位置に対応する数値を設定、`Children`コレクション。

プログラムを再起動すると、5 つ`Picker`ビューは、これら 5 つの既定値を表示します。`FlexLayout`プロパティです。 `FlexLayout`画面の下部に 3 つの子が含まれています。

[![実験ページ: 既定の](flex-layout-images/ExperimentDefault.png "実験 ページで、既定値")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

各、`Label`に割り当てられた領域を示すグレーの背景を持っているビュー`Label`内で、`FlexLayout`です。 背景、`FlexLayout`自体は、Alice 青。 左と右に小さな余白を除く、ページの全体の下の領域を占有します。

<a name="direction" />

### <a name="the-direction-property"></a>Direction プロパティ

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction)プロパティの型は[ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection)、4 つのメンバーを列挙します。

- `Column`
- `ColumnReverse` (または「列リバース」XAML 内)
- `Row`、既定値
- `RowReverse` (または「行リバース」XAML 内)

XAML では、大文字、小文字で列挙メンバーの名前を使用してこのプロパティの値を指定することができます。 またはできます CSS インジケーターと同じでは、かっこで囲まれた 2 つの別の文字列を使用して、大文字小文字が混在したりします。 (「列リバース」と「行リバース」の文字列がで定義されている、 [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML パーサーで使用されるクラスです)。

ここでは、**実験**(左から右へ) を表示するページ、`Row`方向、`Column`方向、および`ColumnReverse`方向。

[![実験ページ: 方向](flex-layout-images/ExperimentDirection.png "実験ページ - の方向")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

ことに注意して、`Reverse`右端または下端にあるオプション 項目を起動します。

<a name="wrap" />

### <a name="the-wrap-property"></a>ラップ プロパティ

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティの型は[ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap)、3 つのメンバーを列挙します。

- `NoWrap`、既定値
- `Wrap`
- `Reverse` (または「ラップ リバース」XAML 内)

左から右に、これらの画面を表示する、 `NoWrap`、`Wrap`と`Reverse`12 の子のオプション。

[![実験ページ: ラップ](flex-layout-images/ExperimentWrap.png "実験ページ - ラップ")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

ときに、`Wrap`プロパティに設定されている`NoWrap`主軸が (このプログラム) のように制約されていると、メインの軸には、幅または高さが十分で、すべての子に合わせて、`FlexLayout`小さく、アイテム、iOS スクリーン ショットとしてしようとしています。について説明します。 使用して、項目の shrinkness を制御することができます、 [ `Shrink` ](#shrink)バインド可能なプロパティを添付します。

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent プロパティ

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティの型は[ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)、6 つのメンバーを列挙します。

- `Start` (または"フレックス start"XAML 内)、既定値
- `Center`
- `End` (または"フレックス end"XAML 内)
- `SpaceBetween` (または「領域の間」XAML 内)
- `SpaceAround` (または領域-周辺""XAML 内)
- `SpaceEvenly`

このプロパティは、メイン軸では、この例では水平軸であるアイテムを配置する方法を指定します。

[![実験ページ: コンテンツを正当化](flex-layout-images/ExperimentJustifyContent.png "実験ページ - コンテンツを正当化")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

次の 3 つのすべてのスクリーン ショットでは、`Wrap`プロパティに設定されている`Wrap`です。 `Start`既定値は、Android の前のスクリーン ショットに表示されます。 IOS スクリーン ショットをここでは、`Center`オプション: すべての項目は、中央に移動されます。 次の 3 つの他のオプションという単語で始まる`Space`アイテムによって使用されていない余分なスペースを割り当てます。 `SpaceBetween` 項目の間で均等に領域を割り当てます`SpaceAround` puts が各アイテムの周囲のスペースを等しく中`SpaceEvenly`と同じ領域の各項目の間と、最初の項目の前後に行の最後の項目を配置します。

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems プロパティ

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティの型は[ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems)、4 つのメンバーを列挙します。

- `Stretch`、既定値
- `Center`
- `Start` (または"フレックス start"XAML 内)
- `End` (または"フレックス end"XAML 内)

これは 2 つのプロパティの 1 つ (もう[ `AlignContent` ](#align-content)) の交差軸上の子を整列する方法を示すです。 行ごとに、子は、(前のスクリーン ショットに示す) に拡張されていて、内または次の 3 つのスクリーン ショットに示すように、開始、center、または各項目の末尾に合わせて調整。

[![実験ページ: アイテムの配置](flex-layout-images/ExperimentAlignItems.png "実験ページ - アイテムの配置")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

IOS のスクリーン ショットですべての子の上部を揃えます。 Android スクリーン ショットでは、項目は垂直方向に中央揃え、最も高い子に基づいています。 UWP スクリーン ショットでは、すべてのアイテムの下部が揃えられます。

すべての個々 の項目について、`AlignItems`で設定を上書きすることができます、 [ `AlignSelf` ](#align-self)添付バインド可能なプロパティ。

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent プロパティ

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent)プロパティの型は[ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent)、7 つのメンバーを持つ列挙します。

- `Stretch`、既定値
- `Center`
- `Start` (または"フレックス start"XAML 内)
- `End` (または"フレックス end"XAML 内)
- `SpaceBetween` (または「領域の間」XAML 内)
- `SpaceAround` (または領域-周辺""XAML 内)
- `SpaceEvenly`

同様に`AlignItems`、`AlignContent`プロパティも、交差軸上の子の配置が、行または列全体に影響を与えます。

[![実験ページ: コンテンツの配置](flex-layout-images/ExperimentAlignContent.png "実験ページ - コンテンツの配置")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

両方の行は iOS screnshot 上部です。Android のスクリーン ショットを務めるセンターです。あり UWP スクリーン ショットでは、下部です。 行は、さまざまな方法で等間隔に配置することができますも。

[![実験ページ: 配置コンテンツ 2](flex-layout-images/ExperimentAlignContent2.png "実験ページ - コンテンツの 2 の整列")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent`のみ 1 つの行または列がある場合に影響を与えません。

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>詳細に接続されているバインド可能なプロパティ

`FlexLayout` 5 つの接続されているバインド可能なプロパティを定義します。 子でこれらのプロパティを設定、`FlexLayout`し、その特定の子にのみ関連します。

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf プロパティ

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty)接続されているバインド可能なプロパティの型は[ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent)、5 つのメンバーを列挙します。

- `Auto`、既定値
- `Stretch`
- `Center`
- `Start` (または"フレックス start"XAML 内)
- `End` (または"フレックス end"XAML 内)

任意の個別の子の`FlexLayout`、このプロパティの設定の上書き、 [ `AlignItems` ](#align-items)プロパティの設定、`FlexLayout`自体です。 既定の設定の`Auto`を使用することを意味、`AlignItems`設定します。

`Label`という名前の要素`label`(例) を設定することができます、`AlignSelf`このようなコード内のプロパティ。

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

参照がないことに注意してください、`FlexLayout`の親、`Label`です。 XAML では、次のようにプロパティを設定します。

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order プロパティ

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty)プロパティの型は`int`します。 既定値は 0 です。

`Order`プロパティでは、順序を変更することができますの子、`FlexLayout`配置されます。 子では通常、`FlexLayout`並べたに出現する順序は、`Children`コレクション。 設定してこの順序を上書きすることができます、`Order`バインド可能なプロパティを 0 以外の整数値を 1 つ以上の子要素にアタッチします。 `FlexLayout`の設定に基づいてその子を整列、`Order`プロパティを各子要素が同じを持つ子`Order`設定に出現する順序に並べ、`Children`コレクション。

### <a name="the-basis-property"></a>基準のプロパティ

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty)接続されているバインド可能なプロパティの子に割り当てられている領域の量を示します、`FlexLayout`メインの軸にします。 サイズ指定によって、`Basis`プロパティは、親のメインの軸に沿ったサイズ`FlexLayout`です。 したがって、`Basis`子が列に配置されたときに、行、または高さに子が配置されたときに、子の幅を示します。

`Basis`プロパティの型は[ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis)、構造体。 いずれかのデバイスに依存しない単位またはサイズの割合として、サイズを指定できます、`FlexLayout`です。 既定値、`Basis`プロパティは、静的プロパティ`FlexBasis.Auto`子の幅または高さの使用を要求したことを意味します。

コードでは、設定することができます、`Basis`プロパティを`Label`という`label`を次のように 40 のデバイスに依存しない単位。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

2 番目の引数、`FlexBasis`コンス トラクターの名前は`isRelative`サイズは相対値かどうかを示します (`true`) または絶対 (`false`)。 引数が、既定値は`false`ので、次のコードを使用することもできます。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

暗黙的な変換`float`に`FlexBasis`が定義されている場合は、さらに簡略化できるようにします。

```csharp
FlexLayout.SetBasis(label, 40);
```

25% にサイズを設定することができます、`FlexLayout`次のように親。

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

この小数部の値は、0 ~ 1 の範囲でなければなりません。

XAML では、デバイスに依存しない単位のサイズの数値を使用できます。

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

または、範囲 0 ~ 100% の割合を指定できます。

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**ごとの実験**のページ、 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルを試すことができます、`Basis`プロパティです。 ページには、ラップされた 5 つの列が表示されます。`Label`要素が背景と前景の色を交互に使用します。 2 つ`Slider`要素を指定できます`Basis`、2 番目と 4 番目の値`Label`:。

[![ベース ページの実験](flex-layout-images/BasisExperiment.png "ベース ページのテスト")](flex-layout-images/BasisExperiment-Large.png#lightbox)

左側にある iOS のスクリーン ショットは、2 つを示しています。`Label`デバイス非依存単位の高さを指定された要素。 全体の高さの割合の高さを指定されている Android の画面を表示、`FlexLayout`です。 場合、`Basis`子の高さに 100% に設定されて、`FlexLayout`とは次の列にラップし、UWP スクリーン ショットに示すように、その列の全体の高さを占めます 5 つの子が行に配置された場合のように表示されます。、5 つの列で実際に配置されますが、します。

### <a name="the-grow-property"></a>拡張プロパティ

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty)接続されているバインド可能なプロパティの型は`int`します。 既定値は 0、および値が 0 以上にする必要があります。

`Grow`プロパティがときときに役割を果たす、`Wrap`プロパティに設定されている`NoWrap`あり、子の行の幅未満の幅の合計、 `FlexLayout`、または子の列がより短い高さ、`FlexLayout`です。 `Grow`プロパティが子の間で残された領域を割り当てる方法を示します。

**実験の拡張**5 つのページ`Label`列と 2 つの要素の色を交互に並べた`Slider`要素は、調整することができます、 `Grow` 、2 番目と 4 番目のプロパティ`Label`です。 一番左にある iOS のスクリーン ショットは、既定値を示しています。 `Grow` 0 のプロパティ。

[![拡大実験ページ](flex-layout-images/GrowExperiment.png "拡大実験 ページ")](flex-layout-images/GrowExperiment-Large.png#lightbox)

任意の 1 つの子には、正の値を指定した場合`Grow`値、Android のスクリーン ショットに示すように、その子が残りのすべての領域がします。 この領域は、次の 2 つ以上の子の間で割り当てることもできます。 UWP スクリーン ショットでは、 `Grow` 2 番目のプロパティ`Label`0.5 に設定されているときに、 `Grow` 4 番目のプロパティ`Label`1.5 では、4 つ目は、これは、 `Label` 3 倍の 2 つ目のとして残された領域の`Label`.

子ビューがその領域を使用する方法は、子の特定の種類によって異なります。 `Label`の合計領域内でテキストを配置することができます、`Label`プロパティを使用して`HorizontalTextAlignment`と`VerticalTextAlignment`です。

<a name="shrink" />

### <a name="the-shrink-property"></a>圧縮プロパティ

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty)接続されているバインド可能なプロパティの型は`int`します。 既定値は 1 であり、値が 0 以上にする必要があります。

`Shrink`プロパティは、役割を果たすときに、`Wrap`プロパティに設定されている`NoWrap`子の行の集計の幅がの幅を超えると、`FlexLayout`子の 1 つの列の集計の高さがより大きいか、高さ、`FlexLayout`です。 通常、 `FlexLayout` constricting のサイズによってこれらの子が表示されます。 `Shrink`プロパティでく子が優先度を指定で、フル サイズで表示されています。

**圧縮実験**ページを作成、 `FlexLayout` 5 つの単一行を持つ`Label`をより多くの領域を必要とする子、`FlexLayout`幅。 左側にある iOS スクリーン ショットはすべて、`Label`既定値は 1 を持つ要素。

[![圧縮は、ページを試す](flex-layout-images/ShrinkExperiment.png "圧縮 ページを試してみる")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android のスクリーン ショット、 `Shrink` 、2 番目の値`Label`に 0 に設定されている`Label`の幅全体に表示されます。 また、4 番目`Label`が与えられます、 `Shrink` 、1 よりも大きい値し、が圧縮されます。 UWP スクリーン ショットは、両方を示しています`Label`要素が指定されている、`Shrink`許可するように、フル サイズで表示される場合は、その 0 の値が可能です。

両方を設定することができます、`Grow`と`Shrink`状況の子の集計サイズ可能性がありますものサイズよりも大きいか小さいに対応する値、`FlexLayout`です。

## <a name="css-styling-with-flexlayout"></a>CSS スタイル FlexLayout を

使用することができます、 [CSS スタイル](~/xamarin-forms/user-interface/styles/css/index.md)で Xamarin.Forms 3.0 で導入された機能`FlexLayout`します。 **CSS カタログ アイテム**のページ、 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルのレイアウトの重複、**カタログ アイテム** ページで、ですが、CSSさまざまなスタイルのスタイル シート:

[![CSS のカタログ アイテム ページ](flex-layout-images/CssCatalogItems.png "CSS のカタログ項目ページ")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

元**CatalogItemsPage.xaml**ファイルには 5 つ`Style`内の定義、`Resources`セクション 15 で`Setter`オブジェクト。 **CssCatalogItemsPage.xaml**がまで減少した 2 つのファイル`Style`あり、4 つだけ定義`Setter`オブジェクト。 これらのスタイルは、Xamarin.Forms CSS スタイル処理の機能は、現在サポートされていないプロパティの CSS スタイル シートを補完します。

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

最初の行で CSS スタイル シートが参照されている、`Resources`セクション。

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

次の 3 つの項目ごとに 2 つの要素を含めることにも注意してください`StyleClass`設定。

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

これらのセレクターを参照してください、 **CatalogItemsStyles.css**スタイル シート。

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

いくつか`FlexLayout`接続されているバインド可能なプロパティがここで参照します。 `label.empty`セレクターが表示されます、`flex-grow`属性は、空のスタイルを`Label`上記空白領域の一部を提供する、`Button`です。 `image`セレクターを含む、`order`属性および`align-self`に対応する属性`FlexLayout`バインド可能なプロパティを添付します。

直接プロパティを設定するにはことを確認した、`FlexLayout`の子に接続されているバインド可能なプロパティを設定して、`FlexLayout`です。 または、これらのプロパティを間接的に使用して XAML ベースの従来のスタイルまたは CSS スタイルを設定することができます。 調べることや、これらのプロパティを理解はどのようなことが重要です。 これらのプロパティは、新機能により、`FlexLayout`真に柔軟です。 

## <a name="flexlayout-with-xamarinuniversity"></a>Xamarin.University で FlexLayout

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 は、レイアウトをでフレックス[Xamarin 大学](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
