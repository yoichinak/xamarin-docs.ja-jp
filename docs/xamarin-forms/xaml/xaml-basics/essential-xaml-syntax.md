---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 788df5f27066d0d8d1f672d82e94a06ddf5e0916
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139816"
---
# <a name="part-2-essential-xaml-syntax"></a>第 2 部 基本的な XAML 構文

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML は、ほとんどがオブジェクトのインスタンス化と初期化を行うように設計されています。ただし、多くの場合、プロパティは、XML 文字列として簡単に表すことができない複合オブジェクトに設定する必要があります。また、1つのクラスで定義されるプロパティを子クラスに設定する必要がある場合もあります。この2つのニーズには、プロパティ要素と添付プロパティの基本的な XAML 構文機能が必要です。_

## <a name="property-elements"></a>Property 要素

XAML では、通常、クラスのプロパティは XML 属性として設定されます。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

ただし、XAML でプロパティを設定する別の方法があります。 で別の操作を実行するには `TextColor` 、まず既存の設定を削除し `TextColor` ます。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

空の要素 `Label` タグを開始タグと終了タグに分割して開きます。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

これらのタグ内には、クラス名とプロパティ名で構成される開始タグと終了タグをピリオドで区切って追加します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

次のように、これらの新しいタグのコンテンツとしてプロパティ値を設定します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

このプロパティを指定する2つの方法は機能的には同じですが、プロパティを 2 `TextColor` 回設定することになるため、同じプロパティに対して2つの方法を使用しないでください。これは、あいまいになる可能性があるためです。

この新しい構文では、便利な用語をいくつか紹介します。

- `Label`は*object 要素*です。 これは、 Xamarin.Forms XML 要素として表現されるオブジェクトです。
- `Text`、 `VerticalOptions` 、 `FontAttributes` および `FontSize` は、*プロパティ属性*です。 これらは、 Xamarin.Forms XML 属性として表現されるプロパティです。
- この最後のスニペットで `TextColor` は、が*プロパティ要素*になりました。 これは Xamarin.Forms プロパティですが、XML 要素になりました。

プロパティ要素の定義は、最初は XML 構文の違反であるように思われますが、そうではありません。 この期間は XML で特別な意味を持ちません。 XML デコーダーには、 `Label.TextColor` は単に通常の子要素です。

ただし、XAML では、この構文は非常に特殊です。 プロパティ要素の規則の1つは、タグに他の何も表示されないことです `Label.TextColor` 。 プロパティの値は、プロパティ要素の開始タグと終了タグの間のコンテンツとして常に定義されます。

複数のプロパティで、プロパティ要素構文を使用できます。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

または、すべてのプロパティに対してプロパティ要素構文を使用することもできます。

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

最初に、プロパティ要素の構文は、比較的単純なものに対しては不要な長い置換のように見えます。これらの例では、このようなケースを考えています。

ただし、プロパティの値が複雑すぎて単純な文字列として表現できない場合は、プロパティ要素の構文が不可欠になります。 プロパティ要素タグ内では、別のオブジェクトをインスタンス化し、そのプロパティを設定できます。 たとえば、プロパティの設定を使用して、などのプロパティを値に明示的に設定でき `VerticalOptions` `LayoutOptions` ます。

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

もう1つの例: には、 `Grid` とという名前の2つのプロパティがあり `RowDefinitions` `ColumnDefinitions` ます。 これらの2つのプロパティの型は `RowDefinitionCollection` と `ColumnDefinitionCollection` で、 `RowDefinition` オブジェクトとオブジェクトのコレクションです `ColumnDefinition` 。 これらのコレクションを設定するには、プロパティ要素構文を使用する必要があります。

次に示すのは、クラスの XAML ファイルの先頭です `GridDemoPage` 。コレクションとコレクションの property 要素タグが示されてい `RowDefinitions` `ColumnDefinitions` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

自動サイズのセル、ピクセル幅と高さのセル、およびスターの設定を定義するための省略構文に注意してください。

## <a name="attached-properties"></a>添付プロパティ

では、 `Grid` とのコレクションに対して、 `RowDefinitions` `ColumnDefinitions` 行と列を定義するためのプロパティ要素が必要であることがわかりました。 ただし、の各子が存在する行と列をプログラマが示す方法もあり `Grid` ます。

の各子のタグ内で、 `Grid` 次の属性を使用して、その子の行と列を指定します。

- `Grid.Row`
- `Grid.Column`

これらの属性の既定値は0です。 また、子が次の属性を持つ複数の行または列にまたがっているかどうかを示すこともできます。

- `Grid.RowSpan`
- `Grid.ColumnSpan`

これらの2つの属性の既定値は1です。

完全な GridDemoPage .xaml ファイルを次に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row` `Grid.Column` 0 のおよびの設定は必須ではありませんが、一般的にはわかりやすくするために含まれています。

次のようになります。

[![Grid レイアウト](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

構文から審査だけでは、これらの、、 `Grid.Row` `Grid.Column` `Grid.RowSpan` 、および `Grid.ColumnSpan` 属性はの静的なフィールドまたはプロパティであるように見えますが、興味深いことに、、、 `Grid` `Grid` `Row` `Column` `RowSpan` 、またはという名前の何も定義しません `ColumnSpan` 。

代わりに、は、、、 `Grid` 、およびという4つのバインド可能なプロパティを定義し `RowProperty` `ColumnProperty` `RowSpanProperty` `ColumnSpanProperty` ます。 これらは、*添付プロパティ*と呼ばれる特別な種類のバインド可能プロパティです。 これらはクラスによって定義され `Grid` ますが、の子に設定され `Grid` ます。

これらの添付プロパティをコードで使用する場合、クラスには、、など `Grid` という名前の静的メソッドが用意されて `SetRow` `GetColumn` います。 ただし、XAML では、これらの添付プロパティは、 `Grid` 単純なプロパティ名を使用して、の子の属性として設定されます。

添付プロパティは、クラスとプロパティ名の両方をピリオドで区切った属性として、常に XAML ファイルで認識できます。 これらは、 *attached properties* 1 つのクラス (この場合は) によって定義されてい `Grid` ますが、他のオブジェクト (この場合はの子) にアタッチされているため、添付プロパティと呼ばれ `Grid` ます。 レイアウト中に、は、 `Grid` これらの添付プロパティの値を調べて、各子の配置場所を確認できます。

`AbsoluteLayout`クラスは、とという2つの添付プロパティを定義し `LayoutBounds` `LayoutFlags` ます。 次に、の比例配置とサイズ調整の機能を使用して実現されるチェッカーボードパターンを示し `AbsoluteLayout` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

ここでは次のようになります。

[![絶対レイアウト](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

このような場合は、XAML を使用する知識について質問することがあります。 もちろん、四角形の繰り返しと定期的を使用すると、 `LayoutBounds` コードでより適切に実現できることがわかります。

これは正当な懸念事項であり、ユーザーインターフェイスを定義するときに、コードとマークアップの使用のバランスを調整しても問題はありません。 XAML でいくつかのビジュアルを定義し、分離コードファイルのコンストラクターを使用して、ループで生成される可能性のあるいくつかのビジュアルを追加するのは簡単です。

## <a name="content-properties"></a>コンテンツのプロパティ

前の例では、 `StackLayout` 、 `Grid` 、およびの `AbsoluteLayout` 各オブジェクトはのプロパティに設定され `Content` `ContentPage` ており、これらのレイアウトの子は実際にはコレクション内の項目です `Children` 。 ただし、これらの `Content` `Children` プロパティとプロパティは、XAML ファイルには存在しません。

`Content` `Children` **XamlPlusCode**サンプルのように、プロパティとプロパティをプロパティ要素として含めることができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

実際の質問は、これらのプロパティ要素が XAML ファイルに必要*ない*のはなぜですか。

XAML で使用するためにで定義された要素 Xamarin.Forms は、クラスの属性でフラグが設定された1つのプロパティを持つことができ `ContentProperty` ます。 オンラインドキュメントでクラスを検索すると、次の `ContentPage` Xamarin.Forms 属性が表示されます。

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

これは、 `Content` プロパティ要素タグが必要ないことを意味します。 開始タグと終了タグの間に表示される XML コンテンツ `ContentPage` は、プロパティに割り当てられるものと見なされ `Content` ます。

 `StackLayout`、 `Grid` 、 `AbsoluteLayout` 、およびは `RelativeLayout` すべてから派生 `Layout<View>` します。ドキュメントを参照すると、 `Layout<T>` 別の属性が表示され Xamarin.Forms `ContentProperty` ます。

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

これにより、 `Children` 明示的なプロパティ要素タグを使用せずに、レイアウトの内容をコレクションに自動的に追加でき `Children` ます。

他のクラスには `ContentProperty` 、属性の定義もあります。 たとえば、の content プロパティ `Label` は `Text` です。 他の API ドキュメントを確認してください。

## <a name="platform-differences-with-onplatform"></a>OnPlatform とのプラットフォームの違い

シングルページアプリケーションでは、 `Padding` iOS ステータスバーが上書きされないように、ページのプロパティを設定するのが一般的です。 コードでは、 `Device.RuntimePlatform` この目的のためにプロパティを使用できます。

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

XAML では、クラスとクラスを使用して同様の操作を行うこともでき [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) ます。 最初に、ページの上部付近のプロパティのプロパティ要素を含め `Padding` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

これらのタグ内には、タグを含め `OnPlatform` ます。 `OnPlatform`はジェネリッククラスです。 ジェネリック型引数 (この場合は、プロパティの型) を指定する必要があり `Thickness` `Padding` ます。 幸いにも、という汎用的な引数を定義するための XAML 属性があり `x:TypeArguments` ます。 これは、設定するプロパティの型と一致している必要があります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform`には、オブジェクトのであるという名前のプロパティがあり `Platforms` `IList` `On` ます。 プロパティのプロパティ要素タグを使用します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

次に `On` 、要素を追加します。 それぞれに対して、プロパティを設定し、プロパティをプロパティの `Platform` `Value` マークアップに設定し `Thickness` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

このマークアップは簡略化できます。 の content プロパティ `OnPlatform` は `Platforms` であるため、これらのプロパティ要素タグは削除できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`Platform`のプロパティ `On` は型である `IList<string>` ため、値が同じである場合は、複数のプラットフォームを含めることができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Android と UWP はの既定値に設定され `Padding` ているため、このタグは削除できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

これは、XAML でプラットフォームに依存するプロパティを設定するための標準的な方法です `Padding` 。 設定を `Value` 1 つの文字列で表すことができない場合は、次のようにプロパティ要素を定義できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

> [!NOTE]
> `OnPlatform`マークアップ拡張機能を XAML で使用して、プラットフォームごとに UI の外観をカスタマイズすることもできます。 クラスおよびクラスと同じ機能を提供し `OnPlatform` `On` ますが、より簡潔な表現を使用します。 詳細については、「 [Onplatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)」を参照してください。

## <a name="summary"></a>[概要]

プロパティ要素と添付プロパティを使用すると、基本的な XAML 構文の多くが確立されています。 ただし、リソースディクショナリなどの間接的な方法で、オブジェクトにプロパティを設定することが必要になる場合があります。 この方法については、第3部で説明し[ます。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [パート1。XAML を使用したはじめに](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [パート3。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [パート4。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [パート5。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
