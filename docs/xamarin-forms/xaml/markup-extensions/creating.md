---
title: XAML マークアップ拡張機能の作成
description: この記事では、独自のカスタム Xamarin.Forms XAML マークアップ拡張を定義する方法について説明します。 XAML マークアップ拡張機能は、IMarkupExtension または IMarkupExtension を実装するクラスを<T>インターフェイス。
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: e69d4b9dcf93c095804c5ac46527c03049580d1c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178109"
---
# <a name="creating-xaml-markup-extensions"></a>XAML マークアップ拡張の作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)

プログラムのレベルでは、XAML マークアップ拡張機能が実装するクラス、 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension)または[ `IMarkupExtension<T>` ](xref:Xamarin.Forms.Xaml.IMarkupExtension`1)インターフェイス。 次で説明する標準のマークアップ拡張機能のソース コードを調べることができます、 [ **Markupextension**ディレクトリ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)Xamarin.Forms の GitHub リポジトリの。

派生することによって、独自のカスタム XAML マークアップ拡張を定義することも`IMarkupExtension`または`IMarkupExtension<T>`します。 マークアップ拡張機能が特定の型の値を取得する場合は、一般的な形式を使用します。 これは、Xamarin.Forms のマークアップ拡張機能のいくつかの場合です。

- `TypeExtension` を派生します。 `IMarkupExtension<Type>`
- `ArrayExtension` を派生します。 `IMarkupExtension<Array>`
- `DynamicResourceExtension` を派生します。 `IMarkupExtension<DynamicResource>`
- `BindingExtension` を派生します。 `IMarkupExtension<BindingBase>`
- `ConstraintExpression` を派生します。 `IMarkupExtension<Constraint>`

2 つ`IMarkupExtension`インターフェイスは、それぞれ 1 つのメソッドを定義という`ProvideValue`:

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

`IMarkupExtension<T>`から派生した`IMarkupExtension`が含まれています、`new`キーワードを`ProvideValue`、両方が含まれている`ProvideValue`メソッド。

非常に多くの場合、XAML マークアップ拡張機能は、戻り値に影響を与えるプロパティを定義します。 (明らかな例外`NullExtension`を`ProvideValue`を単純に返します`null`)。`ProvideValue`メソッドが 1 つの引数型の`IServiceProvider`は、この記事の後半で説明します。

## <a name="a-markup-extension-for-specifying-color"></a>色を指定するためのマークアップ拡張機能

次の XAML マークアップ拡張機能を使用すると、構築、`Color`色合い、鮮やかさ、および明るさのコンポーネントを使用する値します。 1 に初期化される、アルファ コンポーネントを含む、色の 4 つのコンポーネントの 4 つのプロパティを定義します。 クラスの派生元`IMarkupExtension<Color>`を示すために、`Color`値を返します。

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

`IMarkupExtension<T>`から派生`IMarkupExtension`、クラスには、2 つ含める必要があります`ProvideValue`メソッド、1 つを返す`Color`を返すもう`object`、2 番目のメソッドは、最初のメソッドを呼び出すことができますだけですが。

**HSL カラー デモ**ページの表示方法のさまざまな`HslColorExtension`の色を指定する XAML ファイルに含めることができます、 `BoxView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

その場合に注意してください`HslColorExtension`XML タグには、4 つのプロパティは、属性として設定されますが、4 つのプロパティが引用符なしのコンマで区切られた中かっこの間が表示されたら、します。 既定値`H`、 `S`、および`L`は 0、および既定値の`A`1 は既定値に設定したい場合、これらのプロパティを省略できます。 最後の例では、例、明るさが 0、黒で結果は、通常、これがアルファ チャネル 0.5、ため、透過的半分が表示されます、ページの白い背景に対してグレー。

[![HSL カラー デモ](creating-images/hslcolordemo-small.png "HSL カラー デモ")](creating-images/hslcolordemo-large.png#lightbox "HSL の色のデモ")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>ビットマップにアクセスするためのマークアップ拡張機能

引数に`ProvideValue`を実装するオブジェクトには、 [ `IServiceProvider` ](xref:System.IServiceProvider)インターフェイスは、.NET で定義されている`System`名前空間。 このインターフェイスは、1 つのメンバーであるという名前のメソッドを持って`GetService`で、`Type`引数。

`ImageResourceExtension`次に示すクラスの用途の 1 つを示しています。`IServiceProvider`と`GetService`を取得する、`IXmlLineInfoProvider`が特定のエラーが検出されたことを示す行や文字の情報を提供するオブジェクト。 この場合、例外が発生時に、`Source`プロパティが設定されていません。

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;
        return ImageSource.FromResource(assemblyName + "." + Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` XAML ファイルは、.NET Standard ライブラリ プロジェクト内の埋め込みリソースとして格納されているイメージ ファイルにアクセスする必要がある場合に役立ちます。 使用して、`Source`プロパティを呼び出す静的`ImageSource.FromResource`メソッド。 このメソッドでは、完全修飾リソース名、アセンブリ名、フォルダー名とピリオドで区切られたファイル名で構成される必要があります。 2 番目の引数、`ImageSource.FromResource`メソッド、アセンブリ名を提供し、のみが UWP のリリースのビルドに必要な。 いずれにせよ、`ImageSource.FromResource`イメージはライブラリでもない限り、この XAML リソース拡張機能に外部ライブラリの一部をすることはできませんが、ビットマップを格納しているアセンブリから呼び出す必要があります。 (を参照してください、 [**埋め込み画像**](~/xamarin-forms/user-interface/images.md#embedded-images)埋め込みリソースとして格納されているビットマップへのアクセスの詳細については資料)。

`ImageResourceExtension`が必要です、`Source`プロパティを設定する、`Source`プロパティは、クラスの content プロパティとして属性で示されます。 つまり、`Source=`中かっこで式の一部を省略できます。 **イメージ リソースのデモ** ページで、`Image`要素は、フォルダー名とピリオドで区切られたファイル名を使用して 2 つのイメージを取得します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

実行中のプログラムを次に示します。

[![イメージのリソースのデモ](creating-images/imageresourcedemo-small.png "イメージ リソース デモ")](creating-images/imageresourcedemo-large.png#lightbox "イメージ リソースのデモ")

## <a name="service-providers"></a>サービス プロバイダー

使用して、`IServiceProvider`引数`ProvideValue`、XAML マークアップ拡張機能は、使用しているされて、XAML ファイルに関する役立つ情報へのアクセスを取得できます。 使用する、`IServiceProvider`引数が正常にする必要がある特定のコンテキストで使用可能なサービスの種類を把握します。 この機能を理解する最善の方法は、既存の XAML マークアップ拡張機能でのソース コードを調べることで、 [ **Markupextension**フォルダー](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)の Xamarin.Forms GitHub リポジトリ。 いくつかの種類のサービスが Xamarin.Forms の内部にあることに注意します。

一部の XAML マークアップ拡張機能では、このサービスが便利な可能性があります。

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget`インターフェイスは、2 つのプロパティを定義します。`TargetObject`と`TargetProperty`します。 この情報を取得するときに、`ImageResourceExtension`クラス、`TargetObject`は、`Image`と`TargetProperty`は、`BindableProperty`オブジェクト、`Source`プロパティの`Image`します。 これは XAML マークアップ拡張機能が設定されているプロパティです。

`GetService`呼び出しの引数を持つ`typeof(IProvideValueTarget)`型のオブジェクトを実際に返す`SimpleValueTargetProvider`、定義されている、`Xamarin.Forms.Xaml.Internals`名前空間。 戻り値をキャストする場合`GetService`、その型にアクセスすることも、`ParentObjects`を含む配列であるプロパティ、`Image`要素、`Grid`親、および`ImageResourceDemoPage`の親、 `Grid`。

## <a name="conclusion"></a>まとめ

XAML マークアップ拡張機能では、さまざまなソースからの属性を設定する機能を拡張することによって、XAML で重要な役割を果たします。 さらに、必要な既存の XAML マークアップ拡張機能が提供されない場合、記述することも、独自です。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms book から XAML マークアップ拡張機能の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
