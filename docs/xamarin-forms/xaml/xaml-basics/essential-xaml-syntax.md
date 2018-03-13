---
title: "第 2 部: 重要な XAML 構文"
description: "XAML は、ほとんどの場合、インスタンス化して、オブジェクトの初期化用です。 多くの場合、XML 文字列として簡単に表現できない複雑なオブジェクトにプロパティを設定する必要があり、子クラスの 1 つのクラスによって定義されたプロパティを設定する必要があります。 これら 2 つのニーズは、プロパティ要素と添付プロパティの不可欠な XAML 構文の機能が必要です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 77ed7c49a901a877d822c2274263bcb8dbe19ac6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="part-2-essential-xaml-syntax"></a>第 2 部: 重要な XAML 構文

_XAML は、ほとんどの場合、インスタンス化して、オブジェクトの初期化用です。多くの場合、XML 文字列として簡単に表現できない複雑なオブジェクトにプロパティを設定する必要があり、子クラスの 1 つのクラスによって定義されたプロパティを設定する必要があります。これら 2 つのニーズは、プロパティ要素と添付プロパティの不可欠な XAML 構文の機能が必要です。_

## <a name="property-elements"></a>プロパティ要素

XAML では、クラスのプロパティは通常、XML 属性として設定します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

ただし、XAML のプロパティを設定する別の方法があります。 この代替手段を持つを試みる`TextColor`、最初に既存の削除`TextColor`設定。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

空要素を開いている`Label`タグと終了タグに分けることで。

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

これら 2 つの方法を指定する、`TextColor`プロパティは機能的に等価ですは効果的に設定するプロパティを 2 回をあいまいになる可能性がありますので、同じプロパティの 2 つの方法を使用しません。

この新しい構文では、いくつかの便利な用語を導入することができます。

-  `Label` *オブジェクト要素*です。 これは、XML 要素として表される Xamarin.Forms オブジェクトです。
-  `Text`、 `VerticalOptions`、`FontAttributes`と`FontSize`は*プロパティ属性*です。 これらは Xamarin.Forms プロパティを XML 属性として表されます。
-  その最終的なスニペット`TextColor`になった、*プロパティ要素*です。 Xamarin.Forms プロパティが XML 要素であるようになりました。


まず、要素があるプロパティの定義は XML 構文の違反となるようがではありません。 期間は、XML では特別な意味を持ちません。 XML デコーダーに`Label.TextColor`単に通常の子要素です。

XAML では、ただし、この構文は非常に特殊なです。 プロパティ要素の規則の 1 つが、それ以外のもので表示できること、`Label.TextColor`タグ。 プロパティの値は常に、プロパティ要素の開始と終了タグ間のコンテンツとして定義します。

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

最初は、プロパティ要素構文のように思わ不要なあまりに長く話し交換用の比較的非常に単純なものとこれらの例でそのような問題はありません。

ただし、プロパティ要素構文は、プロパティの値が複雑すぎるため、単純な文字列として表現する場合に不可欠ななります。 プロパティ要素タグ内では、別のオブジェクトをインスタンス化し、そのプロパティを設定できます。 たとえば、ことが明示的に設定するプロパティなど`VerticalOptions`を`LayoutOptions`プロパティの設定の値。

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

別の例:`Grid`という 2 つのプロパティを持つ`RowDefinitions`と`ColumnDefinitions`です。 これら 2 つのプロパティが型`RowDefinitionCollection`と`ColumnDefinitionCollection`のコレクションは`RowDefinition`と`ColumnDefinition`オブジェクト。 これらのコレクションを設定するプロパティ要素構文を使用する必要があります。

XAML ファイルの先頭をここでは、`GridDemoPage`クラスのプロパティ要素タグを示す、`RowDefinitions`と`ColumnDefinitions`コレクション。

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

自動サイズ設定のセルをピクセル幅と高さ、星の設定を定義するための省略構文に注意してください。

## <a name="attached-properties"></a>アタッチされるプロパティ

説明したこと、`Grid`のプロパティ要素が必要です、`RowDefinitions`と`ColumnDefinitions`行や列を定義するコレクション。 ただし、必要もあります行と列を示すためにプログラマにとっての何らかの方法でそれぞれの子の`Grid`が存在します。

それぞれの子のタグ内で、`Grid`行と、次の属性を使用してその子の列を指定します。

-  `Grid.Row`
-  `Grid.Column`

これらの属性の既定値は 0 です。 子が 1 つ以上の行またはこれらの属性を持つ列にまたがるかどうかを示していることができます。

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

これら 2 つの属性は、1 の既定値を設定します。

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

`Grid.Row`と`Grid.Column`0 の設定は必要ありませんが、わかりやすくするための目的で一般に含まれます。

どのように 3 つすべてのプラットフォームで次に示します。

[![](essential-xaml-syntax-images/griddemo.png "グリッド レイアウト")](essential-xaml-syntax-images/griddemo-large.png#lightbox "グリッド レイアウト")

内部、構文の情報だけを頼りこれら`Grid.Row`、 `Grid.Column`、 `Grid.RowSpan`、および`Grid.ColumnSpan`の静的フィールドまたはプロパティである属性が表示される`Grid`が興味深いことに、`Grid`というものが定義されていません`Row`、 `Column`、 `RowSpan`、または`ColumnSpan`です。

代わりに、`Grid`という 4 つのバインド可能なプロパティを定義`RowProperty`、 `ColumnProperty`、 `RowSpanProperty`、および`ColumnSpanProperty`です。 これらは、特別な種類と呼ばれるバインド可能なプロパティの*アタッチされるプロパティ*です。 定義されている、`Grid`の子のセットがクラス、`Grid`です。

アタッチされるコードでは、プロパティを使用する場合、`Grid`クラスは、という名前の静的メソッドを提供`SetRow`、`GetColumn`などのようにします。 子の属性として XAML では、これらの添付プロパティの設定が、`Grid`単純なプロパティ名を使用します。

アタッチされるプロパティは常に認識可能な XAML ファイルで、クラスとピリオドで区切ったプロパティ名の両方を含む属性として。 呼び出された*添付プロパティ*1 つのクラスで定義されているため (この場合、 `Grid`) が他のオブジェクトにアタッチされて (この場合の子、 `Grid`)。 レイアウト時に、`Grid`をそれぞれの子を配置する場所を知るこれらの添付プロパティの値を問い合わせることができます。

`AbsoluteLayout`クラスという 2 つの添付プロパティを定義する`LayoutBounds`と`LayoutFlags`です。 ここでは、比例して配置を使用して実現チェッカー ボード パターンのサイズ変更機能`AbsoluteLayout`:

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

次に示します。

[![](essential-xaml-syntax-images/absolutedemo-large.png "絶対レイアウト")](essential-xaml-syntax-images/absolutedemo-large.png#lightbox "絶対レイアウト")

次のように、なんらかの XAML を使用しての知識を質問可能性があります。 確かに、繰り返しおよびの規制、`LayoutBounds`四角形は、その可能性がありますよりで実現コードを提案します。

正当な問題にならなければ、確実に、独自のユーザー インターフェイスを定義するときに、コードとマークアップの使用を分散の問題はありません。 XAML で定義の一部のビジュアルと分離コード ファイルのコンス トラクターを使用してループでより適切に生成される可能性があるよりのビジュアルを追加するには簡単です。

## <a name="content-properties"></a>コンテンツのプロパティ

前の例で、 `StackLayout`、 `Grid`、および`AbsoluteLayout`オブジェクトに設定されます、`Content`のプロパティ、 `ContentPage`、これらのレイアウトの子供は内の項目実際には、`Children`コレクション。 これらをまだ`Content`と`Children`プロパティがない XAML ファイルでします。

確実に含めることができます、`Content`と`Children`プロパティなどで、プロパティ要素として、 **XamlPlusCode**サンプル。

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

問題: がこれらのプロパティ要素はなぜ*いない*XAML ファイルで必要なしますか?

フラグが設定された 1 つのプロパティを持つ XAML で使用するために、Xamarin.Forms で定義された要素が許可されている、`ContentProperty`クラスの属性です。 検索する場合、`ContentPage`クラス Xamarin.Forms のオンライン ドキュメントのこの属性が表示されます。

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

つまり、`Content`プロパティ要素タグが必要ではありません。 開始と終了の間に表示される任意の XML コンテンツ`ContentPage`タグが割り当てられると見なされます、`Content`プロパティです。

 `StackLayout`、 `Grid`、 `AbsoluteLayout`、および`RelativeLayout`からすべて派生`Layout<View>`、を参照する場合と`Layout<T>`ドキュメントでは、Xamarin.Forms、わかります別`ContentProperty`属性。

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

コンテンツのレイアウトに自動的に追加することができます、`Children`せずコレクション明示的な`Children`プロパティ要素タグ。

その他のクラスも持つ`ContentProperty`定義の属性です。 コンテンツのプロパティなど、`Label`は`Text`します。 その他の API のドキュメントを確認してください。

## <a name="platform-differences-with-onplatform"></a>OnPlatform とプラットフォームの違い

単一ページ アプリケーションに共通する設定は、 `Padding` iOS ステータス バーが上書きされないように、ページのプロパティです。 コードでは、使用することができます、`Device.RuntimePlatform`この目的のためのプロパティ。

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

以下のように、XAML を使用して行うこともできます、`OnPlatform`と`On`クラスです。 最初のプロパティ要素を含める、`Padding`ページの上部付近のプロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

これらのタグ内で含める、`OnPlatform`タグ。 `OnPlatform` ジェネリック クラスです。 この場合、ジェネリック型引数を指定する必要がある`Thickness`の型は`Padding`プロパティです。 さいわいと呼ばれるジェネリック引数を定義するには、具体的には、XAML の属性がある`x:TypeArguments`です。 これは、設定するプロパティの型と一致する必要があります。

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

`OnPlatform` という名前のプロパティを持つ`Platforms`されている、`IList`の`On`オブジェクト。 そのプロパティのプロパティ要素タグを使用します。

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

ここで追加`On`要素。 各 onem 設定、`Platform`プロパティおよび`Value`プロパティのマークアップを`Thickness`プロパティ。

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

このマークアップを簡略化できます。 コンテンツ プロパティ`OnPlatform`は`Platforms`のため、これらのプロパティ要素タグを削除できます。

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

`Platform`のプロパティ`On`の種類は`IList<string>`ので、値が同じ場合は、複数のプラットフォームを含めることができます。

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

Android および Windows の既定値に設定されているため`Padding`タグを削除することができます。

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

これは、プラットフォームに依存するを設定する標準的な方法`Padding`XAML でのプロパティです。 場合、`Value`設定は、1 つの文字列で表すことはできません、プロパティ要素を定義できます。

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

## <a name="summary"></a>まとめ

プロパティ要素と添付プロパティの場合は、ほとんどの基本的な XAML 構文が確立されました。 ただし、場合によってリソース ディクショナリから、間接的な方法で、たとえば、オブジェクトにプロパティを設定する必要があります。 この方法は、次の部分では、一部の説明[3 です。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)します。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第 5 部MVVM へのデータ バインディング](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
