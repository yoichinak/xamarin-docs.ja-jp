---
title: パート 2. 基本的な XAML 構文
description: この記事では、プロパティ要素の重要な XAML 構文機能を説明し、添付プロパティ。
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: f79a07a04eddeea1441f7938fdef210a37fb920a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306573"
---
# <a name="part-2-essential-xaml-syntax"></a>パート 2. 基本的な XAML 構文

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML は、ほとんどがオブジェクトのインスタンス化と初期化を行うように設計されています。ただし、多くの場合、プロパティは、XML 文字列として簡単に表すことができない複合オブジェクトに設定する必要があります。また、1つのクラスで定義されるプロパティを子クラスに設定する必要がある場合もあります。この2つのニーズには、プロパティ要素と添付プロパティの基本的な XAML 構文機能が必要です。_

## <a name="property-elements"></a>プロパティ要素

、XAML でクラスのプロパティは通常、XML 属性として設定します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

ただし、XAML でプロパティを設定する別の方法はあります。 `TextColor`でこの方法を使用するには、まず既存の `TextColor` 設定を削除します。

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

これらのタグ内には、クラス名とピリオドで区切ったプロパティ名で構成されると終了タグを追加します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

次のように、これらの新しいタグの内容として、プロパティ値を設定します。

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

`TextColor` プロパティを指定する2つの方法は機能的に同等ですが、同じプロパティに対して2つの方法を使用しないでください。これは実質的にプロパティを2回設定し、あいまいになる可能性があるためです。

この新しい構文では、いくつかの便利な用語を導入できます。

- `Label` は*オブジェクト要素*です。 これは Xamarin.Forms オブジェクトが XML 要素として表されます。
- `Text`、`VerticalOptions`、`FontAttributes`、および `FontSize` は*プロパティ属性*です。 これらは Xamarin.Forms プロパティを XML 属性として表されます。
- この最後のスニペットでは、`TextColor` が*プロパティ要素*になりました。 Xamarin.Forms プロパティですが、XML 要素ではようになりました。

最初の要素があるプロパティの定義を XML の構文の違反となると思われるはありません。 XML では、期間の特別な意味がありません。 XML デコーダーでは、`Label.TextColor` は単に通常の子要素です。

XAML、ただし、この構文は非常に特殊です。 Property 要素のルールの1つに、`Label.TextColor` タグに他のものが何も表示されないことがあります。 プロパティの値は常に、プロパティ要素の開始と終了タグの間のコンテンツとして定義します。

1 つ以上のプロパティでは、プロパティ要素構文を使用できます。

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

または、すべてのプロパティのプロパティ要素構文を使用することができます。

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

最初は、プロパティ要素構文は非常に比較的単純なものの不要な取り戻す交換用と思えるかもしれませんし、これらの例では確実にします。

ただし、プロパティ要素構文になりますプロパティの値が複雑すぎるため、単純な文字列として表すことが不可欠です。 プロパティ要素タグ内では、別のオブジェクトをインスタンス化し、そのプロパティを設定できます。 たとえば、プロパティの設定を使用して、`VerticalOptions` などのプロパティを `LayoutOptions` 値に明示的に設定できます。

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

別の例: `Grid` に `RowDefinitions` と `ColumnDefinitions`という名前の2つのプロパティがあります。 これらの2つのプロパティは `RowDefinitionCollection` と `ColumnDefinitionCollection`型で、`RowDefinition` および `ColumnDefinition` オブジェクトのコレクションです。 プロパティ要素構文を使用して、これらのコレクションを設定する必要があります。

`GridDemoPage` クラスの XAML ファイルの先頭を次に示します。 `RowDefinitions` コレクションと `ColumnDefinitions` コレクションの property 要素タグが示されています。

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

自動サイズ設定のセル、セルのピクセル幅と高さを星の設定を定義するための省略構文に注意してください。

## <a name="attached-properties"></a>アタッチされるプロパティ

`Grid` には、行と列を定義するために、`RowDefinitions` コレクションと `ColumnDefinitions` コレクションのプロパティ要素が必要であることがわかりました。 ただし、プログラマが `Grid` の各子要素が存在する行と列を示す方法も必要です。

`Grid` の各子のタグ内で、次の属性を使用してその子の行と列を指定します。

- `Grid.Row`
- `Grid.Column`

これらの属性の既定値は、0 です。 子が 1 つ以上の行または列でこれらの属性にまたがるかどうかを示すこともできます。

- `Grid.RowSpan`
- `Grid.ColumnSpan`

これら 2 つの属性は、1 の既定値を指定します。

完全な GridDemoPage.xaml ファイルを次に示します。

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

0の `Grid.Row` と `Grid.Column` の設定は必須ではありませんが、一般的にわかりやすくするために含まれています。

次のような見た目に示します。

[グリッドレイアウトの ![](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

構文から審査だけでは、これらの `Grid.Row`、`Grid.Column`、`Grid.RowSpan`、および `Grid.ColumnSpan` の属性は `Grid`の静的フィールドまたはプロパティとして表示されますが、`Grid`、`Row`、`Column`、`RowSpan`という名前の何も定義されていません。`ColumnSpan`

代わりに、`Grid` は `RowProperty`、`ColumnProperty`、`RowSpanProperty`、`ColumnSpanProperty`という4つのバインド可能なプロパティを定義します。 これらは、*添付プロパティ*と呼ばれる特別な種類のバインド可能プロパティです。 これらは `Grid` クラスによって定義されていますが、`Grid`の子に設定されています。

これらの添付プロパティをコードで使用する場合、`Grid` クラスには、`SetRow`、`GetColumn`などという名前の静的メソッドが用意されています。 ただし、XAML では、これらの添付プロパティは、単純なプロパティ名を使用して、`Grid` の子の属性として設定されます。

添付プロパティは常に認識可能な XAML ファイルで、クラスとピリオドで区切ったプロパティ名の両方を含む属性として。 これらのプロパティは、1つのクラス (この場合は `Grid`) によって定義されていますが、他のオブジェクト (この場合は `Grid`の子) にアタッチされているため、*添付プロパティ*と呼ばれます。 レイアウト中、`Grid` は、これらの添付プロパティの値を調べて、各子を配置する場所を知ることができます。

`AbsoluteLayout` クラスは、`LayoutBounds` と `LayoutFlags`という2つの添付プロパティを定義します。 `AbsoluteLayout`の比例した配置とサイズ設定の機能を使用して実現されるチェッカーボードパターンを次に示します。

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

ここでは。

[絶対レイアウト ![](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

このようなものの XAML を使用しての知恵を質問可能性があります。 確かに、`LayoutBounds` の四角形の繰り返しと定期的により、コードでより適切に実現できることがわかります。

正当な問題にならなければ、これは確かに、ユーザー インターフェイスを定義するときに、コードとマークアップの使用を分散の問題はありません。 一部のビジュアルを XAML で定義し、分離コード ファイルのコンス トラクターを使用して、ループ内で生成されるいくつかの多くのビジュアルを追加する簡単です。

## <a name="content-properties"></a>コンテンツのプロパティ

前の例では、`StackLayout`、`Grid`、および `AbsoluteLayout` オブジェクトは `ContentPage`の `Content` プロパティに設定されており、これらのレイアウトの子は実際には `Children` コレクション内の項目です。 ただし、これらの `Content` および `Children` プロパティは、XAML ファイルには存在しません。

**XamlPlusCode**サンプルのように、`Content` と `Children` プロパティをプロパティ要素として含めることができます。

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

XAML で使用するために Xamarin. Forms で定義された要素は、クラスの `ContentProperty` 属性でフラグが設定された1つのプロパティを持つことができます。 オンラインの Xamarin. Forms ドキュメントで `ContentPage` クラスを参照すると、次の属性が表示されます。

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

これは、`Content` のプロパティ要素タグが必要ないことを意味します。 `ContentPage` の開始タグと終了タグの間に表示される XML コンテンツは、`Content` プロパティに割り当てられると想定されます。

 `StackLayout`、`Grid`、`AbsoluteLayout`、および `RelativeLayout` はすべて `Layout<View>`から派生しています。また、Xamarin. Forms ドキュメントで `Layout<T>` を検索すると、別の `ContentProperty` 属性が表示されます。

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

これにより、明示的に `Children` プロパティ要素タグを使用せずに、レイアウトの内容を `Children` コレクションに自動的に追加できます。

他のクラスには、`ContentProperty` 属性の定義もあります。 たとえば、`Label` の content プロパティは `Text`です。 他の API のドキュメントを確認します。

## <a name="platform-differences-with-onplatform"></a>OnPlatform とプラットフォームの違い

シングルページアプリケーションでは、iOS ステータスバーが上書きされないように、ページの `Padding` プロパティを設定するのが一般的です。 コードでは、この目的のために `Device.RuntimePlatform` プロパティを使用できます。

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

XAML では、 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスと[`On`](xref:Xamarin.Forms.On)クラスを使用して同様の操作を行うこともできます。 最初に、ページの上部付近の `Padding` プロパティのプロパティ要素を含めます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

これらのタグ内には、`OnPlatform` タグを含めます。 `OnPlatform` はジェネリッククラスです。 ジェネリック型引数を指定する必要があります。この例では、`Padding` プロパティの型である `Thickness`です。 幸いにも、`x:TypeArguments`と呼ばれる汎用的な引数を定義するための XAML 属性があります。 これを設定するプロパティの型と一致する必要があります。

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

`OnPlatform` には、`On` オブジェクトの `IList` である `Platforms` という名前のプロパティがあります。 そのプロパティのプロパティ要素タグを使用します。

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

ここで `On` 要素を追加します。 それぞれに対して、`Platform` プロパティを設定し、`Value` プロパティを `Thickness` プロパティのマークアップに設定します。

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

このマークアップを簡略化できます。 `OnPlatform` の content プロパティは `Platforms`ため、これらのプロパティ要素タグは削除できます。

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

`On` の `Platform` プロパティは `IList<string>`型であるため、値が同じ場合は複数のプラットフォームを含めることができます。

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

Android と UWP は `Padding`の既定値に設定されているため、このタグは削除できます。

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

これは、XAML でプラットフォームに依存する `Padding` プロパティを設定するための標準的な方法です。 `Value` 設定を1つの文字列で表すことができない場合は、次のようにプロパティ要素を定義できます。

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
> XAML では、`OnPlatform` マークアップ拡張機能を使用して、プラットフォームごとに UI の外観をカスタマイズすることもできます。 `OnPlatform` クラスと `On` クラスと同じ機能を提供しますが、より簡潔に表現できます。 詳細については、「 [Onplatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)」を参照してください。

## <a name="summary"></a>要約

プロパティ要素と添付プロパティの場合は、基本的な XAML 構文の多くが確立されました。 ただし、場合によってオブジェクトに間接的な方法で、たとえば、リソース ディクショナリからプロパティを設定する必要があります。 この方法については、第3部で説明し[ます。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [パート1。XAML を使用したはじめに](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [パート3。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [パート4。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [パート5。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
