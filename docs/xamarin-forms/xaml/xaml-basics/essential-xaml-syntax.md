---
title: 第 2 部です。 重要な XAML 構文
description: この記事では、プロパティ要素の重要な XAML 構文機能を説明し、添付プロパティ。
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 7526349c1b4b61495af95dfc200a5055cea5650e
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529297"
---
# <a name="part-2-essential-xaml-syntax"></a>第 2 部です。 重要な XAML 構文

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML は、ほとんどの場合、インスタンス化して、オブジェクトの初期化用です。多くの場合、XML の文字列として簡単に表すことができない複雑なオブジェクトにプロパティを設定する必要があり、子クラスの 1 つのクラスによって定義されたプロパティを設定する必要がある場合があります。これら 2 つのニーズには、プロパティ要素および添付プロパティの基本の XAML 構文の機能が必要です。_

## <a name="property-elements"></a>プロパティ要素

、XAML でクラスのプロパティは通常、XML 属性として設定します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

ただし、XAML でプロパティを設定する別の方法はあります。 この方法を試すに`TextColor`、削除、既存`TextColor`設定。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

空要素を開いて`Label`タグと終了タグに分けることで。

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

これら 2 つの方法を指定する、`TextColor`プロパティは機能的に同等は効果的にするプロパティの設定、2 回、およびあいまいな場合がありますされるため、同じプロパティを 2 つの方法を使用しないでください。

この新しい構文では、いくつかの便利な用語を導入できます。

- `Label` *オブジェクト要素*します。 これは Xamarin.Forms オブジェクトが XML 要素として表されます。
- `Text`、 `VerticalOptions`、`FontAttributes`と`FontSize`は*プロパティ属性*します。 これらは Xamarin.Forms プロパティを XML 属性として表されます。
- その最終的なスニペットで`TextColor`になりますが、*プロパティ要素*します。 Xamarin.Forms プロパティですが、XML 要素ではようになりました。


最初の要素があるプロパティの定義を XML の構文の違反となると思われるはありません。 XML では、期間の特別な意味がありません。 XML デコーダーでは、`Label.TextColor`通常の子要素だけです。

XAML、ただし、この構文は非常に特殊です。 他に何も表示できることは、プロパティ要素の規則のいずれか、`Label.TextColor`タグ。 プロパティの値は常に、プロパティ要素の開始と終了タグの間のコンテンツとして定義します。

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

ただし、プロパティ要素構文になりますプロパティの値が複雑すぎるため、単純な文字列として表すことが不可欠です。 プロパティ要素タグ内では、別のオブジェクトをインスタンス化し、そのプロパティを設定できます。 たとえば、明示的に設定できますプロパティなど`VerticalOptions`を`LayoutOptions`プロパティの設定の値。

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

もう1つの例を次に示します。に`Grid`は、とと`RowDefinitions` `ColumnDefinitions`いう名前の2つのプロパティがあります。 これら 2 つのプロパティが型`RowDefinitionCollection`と`ColumnDefinitionCollection`のコレクションである`RowDefinition`と`ColumnDefinition`オブジェクト。 プロパティ要素構文を使用して、これらのコレクションを設定する必要があります。

ここでは、XAML ファイルの先頭を`GridDemoPage`クラス、プロパティ要素タグを表示、`RowDefinitions`と`ColumnDefinitions`コレクション。

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

上記を`Grid`のプロパティ要素が必要です、`RowDefinitions`と`ColumnDefinitions`行と列を定義するコレクション。 ただし、必要もありますを行と列を示すために、プログラマにとっての何らかの方法でそれぞれの子の`Grid`が存在します。

それぞれの子のタグ内で、`Grid`行と、次の属性を使用してその子の列を指定します。

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

`Grid.Row`と`Grid.Column`0 の設定は必要ありませんが、わかりやすくするための目的で一般的に含まれます。

次のような見た目に示します。

[![](essential-xaml-syntax-images/griddemo.png "グリッド レイアウト")](essential-xaml-syntax-images/griddemo-large.png#lightbox "グリッド レイアウト")

構文からのみ判断これら`Grid.Row`、 `Grid.Column`、`Grid.RowSpan`と`Grid.ColumnSpan`の静的フィールドまたはプロパティに属性が表示されます`Grid`、が十分な大きさで`Grid`という名前が定義されていません。`Row`、 `Column`、 `RowSpan`、または`ColumnSpan`します。

代わりに、`Grid`という 4 つのバインド可能なプロパティを定義します。 `RowProperty`、 `ColumnProperty`、 `RowSpanProperty`、および`ColumnSpanProperty`します。 これらは特殊な種類と呼ばれるバインド可能なプロパティの*添付プロパティ*します。 によって定義されている、`Grid`クラスは、の子で、設定、`Grid`します。

コードでは、プロパティがアタッチされているこれらを使用する場合、`Grid`クラスという名前の静的メソッドを提供する`SetRow`、`GetColumn`となります。 子の属性として、XAML でこれらの添付プロパティの設定が、`Grid`単純なプロパティ名を使用します。

添付プロパティは常に認識可能な XAML ファイルで、クラスとピリオドで区切ったプロパティ名の両方を含む属性として。 呼び出される*添付プロパティ*1 つのクラスで定義されているため (この場合、 `Grid`) 他のオブジェクトにアタッチされています (このケースでは、子で、 `Grid`)。 レイアウト中に、`Grid`それぞれの子を配置する場所を把握するこれらの添付プロパティの値を問い合わせることができます。

`AbsoluteLayout`クラスという 2 つの添付プロパティを定義する`LayoutBounds`と`LayoutFlags`します。 ここでは、比例して配置を使用して実現チェッカー ボード パターンのサイズ変更機能`AbsoluteLayout`:

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

[![](essential-xaml-syntax-images/absolutedemo-large.png "絶対レイアウト")](essential-xaml-syntax-images/absolutedemo-large.png#lightbox "絶対レイアウト")

このようなものの XAML を使用しての知恵を質問可能性があります。 確かに、繰り返しおよびの規制、`LayoutBounds`四角形が、その可能性がありますより適切に含まれているコードを示します。

正当な問題にならなければ、これは確かに、ユーザー インターフェイスを定義するときに、コードとマークアップの使用を分散の問題はありません。 一部のビジュアルを XAML で定義し、分離コード ファイルのコンス トラクターを使用して、ループ内で生成されるいくつかの多くのビジュアルを追加する簡単です。

## <a name="content-properties"></a>コンテンツのプロパティ

前の例で、 `StackLayout`、 `Grid`、および`AbsoluteLayout`オブジェクトに設定されます、`Content`のプロパティ、 `ContentPage`、内の項目では実際には、これらのレイアウトの子、`Children`コレクション。 まだこれら`Content`と`Children`プロパティではなく、XAML ファイル。

確かに含めることができます、`Content`と`Children`プロパティなどで、プロパティ要素として、 **XamlPlusCode**サンプル。

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

実際の質問は次のとおりです。これらのプロパティ要素が XAML ファイルで必須では*ない*のはなぜですか。

XAML で使用するために、Xamarin.Forms で定義された要素が実行を許可されている 1 つのプロパティのフラグが設定された、`ContentProperty`クラスの属性。 検索する場合、`ContentPage`クラス オンライン Xamarin.Forms ドキュメントでは、この属性を確認します。

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

つまり、`Content`プロパティ要素タグは必要ありません。 開始と終了の間に表示される任意の XML コンテンツ`ContentPage`タグに割り当てられると見なされます、`Content`プロパティ。

 `StackLayout`、 `Grid`、 `AbsoluteLayout`、および`RelativeLayout`からすべての派生`Layout<View>`、を参照する場合と`Layout<T>`Xamarin.Forms ドキュメントでは、別表示されます`ContentProperty`属性。

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

コンテンツのレイアウトを自動的に追加できるようにする、`Children`せずコレクション明示的な`Children`プロパティ要素タグ。

その他のクラスもある`ContentProperty`属性の定義。 たとえば、コンテンツ プロパティの`Label`は`Text`します。 他の API のドキュメントを確認します。

## <a name="platform-differences-with-onplatform"></a>OnPlatform とプラットフォームの違い

シングル ページ アプリケーションで、設定する一般的な`Padding`iOS のステータス バーの上書きを回避するために、ページのプロパティ。 コードでは、使用することができます、`Device.RuntimePlatform`この目的のプロパティ。

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

XAML では、クラス[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)と[`On`](xref:Xamarin.Forms.On)クラスを使用して同様の操作を行うこともできます。 最初のプロパティ要素を含める、`Padding`ページの上部付近のプロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

これらのタグ内で含める、`OnPlatform`タグ。 `OnPlatform` ジェネリック クラスです。 この場合は、ジェネリック型引数を指定する必要がある`Thickness`の型である`Padding`プロパティ。 さいわいと呼ばれるジェネリック引数を定義するには、具体的には XAML 属性が`x:TypeArguments`します。 これを設定するプロパティの型と一致する必要があります。

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

`OnPlatform` という名前のプロパティを持つ`Platforms`つまり、`IList`の`On`オブジェクト。 そのプロパティのプロパティ要素タグを使用します。

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

ここで追加`On`要素。 1 つずつ設定、`Platform`プロパティおよび`Value`プロパティのマークアップを`Thickness`プロパティ。

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

このマークアップを簡略化できます。 Content プロパティ`OnPlatform`は`Platforms`ので、これらのプロパティ要素タグを削除することができます。

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

`Platform`プロパティの`On`の種類は`IList<string>`ので、値が同じ場合は、複数のプラットフォームを含めることができます。

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

Android、UWP は、の既定値に設定されているため`Padding`タグを削除できます。

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

プラットフォーム依存を設定する標準的な方法は、この`Padding`XAML プロパティ。 場合、`Value`設定が 1 つの文字列によって表されることはできませんが、プロパティ要素を定義できます。

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
> マーク`OnPlatform`アップ拡張機能を XAML で使用して、プラットフォームごとに UI の外観をカスタマイズすることもできます。 クラス`OnPlatform` および`On`クラスと同じ機能を提供しますが、より簡潔な表現を使用します。 詳細については、次を参照してください。 [OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)します。

## <a name="summary"></a>Summary

プロパティ要素と添付プロパティの場合は、基本的な XAML 構文の多くが確立されました。 ただし、場合によってオブジェクトに間接的な方法で、たとえば、リソース ディクショナリからプロパティを設定する必要があります。 この方法については、次の部分の一部で[3。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)します。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第 5 部MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
