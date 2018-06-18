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
ms.locfileid: "33921837"
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

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプル の **Experiment** ページ を使って、残り 5 つのバインド可能なプロパティを試すことができます。 このページは、`FlexLayout` から子を追加・削除したり、5 つのバインド可能なプロパティの組み合わせを設定したりすることができます。 `FlexLayout` の全ての子は、様々な色やサイズの `Label` view で、`Text` プロパティには `Children` コレクションの位置に対応する番号がセットされます。

プログラムを起動すると、5 つの `Picker` view に、これら 5 つの `FlexLayout` のプロパティの規定値が表示されます。 画面下部の `FlexLayout` は 3 つの子を含んでいます。

[![The Experiment Page: Default](flex-layout-images/ExperimentDefault.png "The Experiment Page - Default")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

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

[![The Experiment Page: Direction](flex-layout-images/ExperimentDirection.png "The Experiment Page - Direction")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

`Reverse` オプションに注意してください。これはアイテムが右または下から始まるようになります。

<a name="wrap" />

### <a name="the-wrap-property"></a>Wrap プロパティ

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)プロパティは 3 つのメンバーを列挙する [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) 型です。

- `NoWrap` 既定値
- `Wrap`
- `Reverse` ( XAML 内では "wrap-reverse" も可 )

左から順番に、12 の子を持つ `NoWrap` 、`Wrap`、`Reverse` オプションでの画面表示です。

[![The Experiment Page: Wrap](flex-layout-images/ExperimentWrap.png "The Experiment Page - Wrap")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

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

[![The Experiment Page: Justify Content](flex-layout-images/ExperimentJustifyContent.png "The Experiment Page - Justify Content")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

3 つのすべてのスクリーン ショットでは、`Wrap` プロパティには `Wrap` が設定されています。 規定値である `Start` は、Android のスクリーンショットで示しています。 iOS のスクリーンショットには `Center` オプション （すべてのアイテムが中央に移動）が示されています。 `Space` という単語で始まるその他3つのオプションは、アイテムに使用されなかった余白を割り当てます。 `SpaceBetween` はアイテムの間に余白を均等に割り当て、`SpaceAround` は各アイテムの両端に均等に余白を置きます。一方、`SpaceEvenly` は各アイテムの間と行の最初のアイテムの前と最後のアイテムの後に均等に余白を置きます。

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems プロパティ

[`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティは、4 つのメンバーを列挙する [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 型です。

- `Stretch`、既定値
- `Center`
- `Start` ( XAML では "flex-start" も可 )
- `End` ( XAML では "flex-end" も可 )

これは、 交差軸上に子を整列する方法を示す 2 つのプロパティの 1 つ（もう 1 つは[`AlignContent`](#align-content)）です。 各行内で（上のスクリーンショットで示すように）子は引き伸ばされますが、以下の3つのスクリーンショットのように、各アイテムの開始・中央・終了位置に揃えられます。

[![The Experiment Page: Align Items](flex-layout-images/ExperimentAlignItems.png "The Experiment Page - Align Items")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

iOS のスクリーンショットでは、すべての子は上揃えになっています。 Android のスクリーンショットでは、全てのアイテムはもっとも高い子に合わせて垂直方向に中央揃えになっています。 UWP のスクリーンショットでは、すべてのアイテムは下揃えになっています。

すべての個々のアイテムは、[`AlignSelf`](#align-self) 添付プロパティを使って `AlignItems` の設定を上書きすることができます。

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent プロパティ

[`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent)プロパティは、7 つのメンバーを列挙する [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent) 型です。

- `Stretch`、既定値
- `Center`
- `Start` ( XAML では "flex-start" も可 )
- `End` ( XAML では "flex-end" も可 )
- `SpaceBetween` ( XAML では "space-between" も可 )
- `SpaceAround` ( XAML では "space-around" も可 )
- `SpaceEvenly`

`AlignItems` と同様に、`AlignContent` プロパティも交差軸上の子を整列しますが、これは行または列全体に影響を与えます。

[![The Experiment Page: Align Content](flex-layout-images/ExperimentAlignContent.png "The Experiment Page - Align Content")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

iOS のスクリーンショットでは、すべての行は上部にあります。Android のスクリーンショットでは、中央にあります。そして UWP のスクリーンショットでは、下部にあります。 行自体もさまざまな方法で配置することができます。

[![The Experiment Page:  Align Content 2](flex-layout-images/ExperimentAlignContent2.png "The Experiment Page - Align Content 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` は1行または1列のみの場合は影響を与えません。

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>バインド可能な添付プロパティの詳細

`FlexLayout` は 5 つの添付プロパティが定義されています。これらのプロパティは `Flexlayout` の子に設定され、その特定の子のみに影響します。 子でこれらのプロパティを設定、`FlexLayout`し、その特定の子にのみ関連します。

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf プロパティ

[`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) 添付プロパティは、 5 つのメンバーを列挙する [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent) 型です。

- `Auto`、既定値
- `Stretch`
- `Center`
- `Start` ( XAML では "flex-start" も可 )
- `End` ( XAML では "flex-end" も可 )

`FlexLayout` の任意の個別の子のために、このプロパティの設定は `FlexLayout` 自身の [`AlignItems`](#align-items) プロパティ設定を上書きします。 規定の設定である `Auto` は `AlignItems` の設定を使用することを意味します。

例えば `Label` という名前の `label` 要素には、以下のようにコードで `AlignSelf` プロパティをセットすることができます。

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

`Label` の親である `FlexLayout` への参照がないことに注意してください。 XAML では、次のようにプロパティを設定します。

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order プロパティ

[`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty) プロパティは `int` 型で、 既定値は 0 です。

`Order` プロパティは、`FlexLayout` の子が配置される順番を変更することができます。 通常、`FlexLayout` の子は `Children` コレクションの出現順と同じ順番で配置されます。 この順番は、1つ以上の子の `Order` 添付プロパティに 0 以外の数値を設定することで上書きすることができます。 `FlexLayout` はその場合、それぞれの子の `Order` プロパティの設定に基づいて自身の子を配置します。ただし、同じ `Order` 設定を持つ子は、`Children` コレクションの出現順に配置されます。

### <a name="the-basis-property"></a>Basis プロパティ

[`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty) 添付プロパティは、主軸上で `FlexLayout` の子に割り当てられている領域の量を示します。 `Basis` プロパティによって指定されたサイズは、親の `FlexLayout` の主軸に沿ったサイズです。 したがって、`Basis` は、行に子が配置された時の子の幅、または列に子が配置された時の高さを示します。

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

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルの **Basis Experiment** のページでは、`Basis` プロパティを試すことができます。 このページは交互に異なる背景色と前景色を持つ 5 つの `Label` 要素の折り返しする列が表示されます。 2 つの `Slider` 要素は 2 番目と 4 番目の `Label` に `Basis` 値を指定します。

[![The Basis Experiment Page](flex-layout-images/BasisExperiment.png "The Basis Experiment Page")](flex-layout-images/BasisExperiment-Large.png#lightbox)

左の iOS のスクリーンショットは、2 つの `Label` 要素がデバイス非依存単位で高さが与えられていること示しています。 Android のスクリーンショットは、それらの要素が `FlexLayout` の 合計の高さに対する割合での高さが与えられていることを示しています。 `Basis` に 100% がセットされた場合、UWP のスクリーンショットで示すように、子は `FlexLayout` の高さになり、次の列に折り返され、その列の全体の高さを占有します。さも 5 つの子が行に配置されたかのように見えますが、それらは実際は 5 つの列に配置されています。

### <a name="the-grow-property"></a>Grow プロパティ

[`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty)添付プロパティは `int` 型です。 規定値は 0 でこの値は 0以上でなければなりません。

`Grow` プロパティは、`Wrap` プロパティに `NoWrap` が設定されていて、1行の子の合計幅が `FlexLayout` の幅より小さい場合、または1列の子の合計の高さが `FlexLayout` の高さより低い場合に機能します。 `Grow` プロパティは子に余った領域を分配する方法を示します。

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

Android のスクリーンショットでは、2 番目の `Label` の `Shrink` 値に 0 が設定され、その `Label` はフルサイズで表示されています。 また、4 番目の `Label` には `Shrink` に 1 より大きい値が与えられ、それは収縮しています。 UWP のスクリーンショットは、両方の `Label` 要素に `Shrink` 値 0 が与えられ、可能であれば、それらをフルサイズで表示できることを示しています。

子の合計サイズが `FlexLayout` のサイズより小さくなったり大きくなったりすることがあるような状況に対応するために、`Grow` と `Shrink` 値を両方設定することができます。

## <a name="css-styling-with-flexlayout"></a>FlexLayout を使った CSS スタイル

`FlexLayout` に関連して Xamarin.Forms 3.0 に導入された [CSS スタイル](~/xamarin-forms/user-interface/styles/css/index.md) を使うことができます。 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** サンプルの **CSS Catalog Items** ページは、**Catalog Items** ページのレイアウトを複写したものですが、多くのスタイルに CSS スタイルシートを使っています。

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

**Xamarin.Forms 3.0 Flex Layout, by [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
