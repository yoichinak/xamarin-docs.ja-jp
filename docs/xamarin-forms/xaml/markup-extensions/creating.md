---
title: XAML マークアップ拡張機能の作成
description: 独自カスタム XAML のマークアップ拡張機能を定義します。
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 1a484aa4a19473c5a4f60b3d7bab78af7a20eecd
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848253"
---
# <a name="creating-xaml-markup-extensions"></a>XAML マークアップ拡張機能の作成

プログラムのレベルでは、XAML マークアップ拡張機能が実装するクラス、 [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/)または[ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/)インターフェイスです。 以下に説明する標準のマークアップ拡張機能のソース コードを調べることができます、 [ **Markupextension**ディレクトリ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)Xamarin.Forms GitHub リポジトリのです。 

派生することによって独自カスタム XAML のマークアップ拡張機能を定義することも`IMarkupExtension`または`IMarkupExtension<T>`です。 マークアップ拡張機能が特定の種類の値を取得する場合は、汎用フォームを使用します。 Xamarin.Forms のマークアップ拡張機能のいくつかの場合と、次に示します。

- `TypeExtension` を派生します。 `IMarkupExtension<Type>`
- `ArrayExtension` を派生します。 `IMarkupExtension<Array>`
- `DynamicResourceExtension` を派生します。 `IMarkupExtension<DynamicResource>`
- `BindingExtension` を派生します。 `IMarkupExtension<BindingBase>`
- `ConstraintExpression` を派生します。 `IMarkupExtension<Constraint>`

2 つ`IMarkupExtension`インターフェイスが、それぞれ 1 つのメソッドを定義する名前付き`ProvideValue`: 

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

`IMarkupExtension<T>`から派生した`IMarkupExtension`が含まれています、`new`キーワードを`ProvideValue`、両方が含まれている`ProvideValue`メソッドです。

非常に多くの場合は、XAML マークアップ拡張機能は、戻り値に影響を与えるプロパティを定義します。 (明らかな例外`NullExtension`、いる`ProvideValue`を単純に返します`null`)。`ProvideValue`メソッドが型の単一の引数を持つ`IServiceProvider`をこの記事の後半で説明されます。

## <a name="a-markup-extension-for-specifying-color"></a>色を指定するためのマークアップ拡張機能

次の XAML マークアップ拡張機能では、構築することができます、`Color`色合い、鮮やかさ、および明るさのコンポーネントを使用する値します。 1 に初期化される、アルファ コンポーネントを含む、色の 4 つのコンポーネントの 4 つのプロパティを定義します。 クラスの派生元`IMarkupExtension<Color>`を示すために、`Color`戻り値。

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

`IMarkupExtension<T>`から派生した`IMarkupExtension`、クラスには、2 つ含める必要があります`ProvideValue`メソッド、返す`Color`もう 1 つを返す`object`、2 番目のメソッドは、最初のメソッドを呼び出すだけことができますが、します。

**HSL の色のデモ**ページの表示方法のさまざまな`HslColorExtension`の色を指定する XAML ファイルに表示されることができます、 `BoxView`:

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

場合に`HslColorExtension`XML タグには、4 つのプロパティは、属性として設定されます。 ただし 4 つのプロパティが引用符で囲まずにコンマで区切られます中かっこの間にが表示されたら、します。 に対して、既定値`H`、 `S`、および`L`は 0、および既定値の`A`1 の場合は既定値に設定したい場合は、これらのプロパティを省略できます。 最後の例は、ここで明るさ黒での結果、通常、0 がアルファ チャネルが 0.5、ため、透過的半分が表示されます例を示しています。 ページの背景色は白に対して灰色。

[![HSL の色のデモ](creating-images/hslcolordemo-small.png "HSL の色のデモ")](creating-images/hslcolordemo-large.png#lightbox "HSL の色のデモ")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>ビットマップにアクセスするためのマークアップ拡張機能

引数`ProvideValue`を実装するオブジェクトには、 [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) .NET で定義されているインターフェイス`System`名前空間。 このインターフェイスは、1 つのメンバーであるという名前のメソッド`GetService`で、`Type`引数。 

`ImageResourceExtension`次に示すクラスの用途の 1 つを示しています。`IServiceProvider`と`GetService`を取得する、`IXmlLineInfoProvider`特定のエラーが検出されましたを示す行や文字の情報を提供できるオブジェクト。 ここでは、例外が発生したときに、`Source`プロパティが設定されていません。

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

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` XAML ファイルが、標準的な .NET のライブラリ プロジェクト内の埋め込みリソースとして保存されたイメージ ファイルにアクセスする必要がある場合に便利です。 使用して、`Source`を呼び出す、静的なプロパティ`ImageSource.FromResource`メソッドです。 このメソッドには、アセンブリ名、フォルダー名、およびピリオドで区切られたファイル名から成る完全修飾リソース名が必要です。 `ImageResourceExtension`しない必要があります、アセンブリ名の部分にリフレクションを使用して、アセンブリ名を取得し、前に付加するため、`Source`プロパティです。 関係なく、`ImageSource.FromResource`をイメージにもそのライブラリしない限り、この XAML リソース拡張機能に外部ライブラリの一部をすることはできませんが、ビットマップを含むアセンブリから呼び出す必要があります。 (を参照してください、 [**埋め込み画像**](~/xamarin-forms/user-interface/images.md#embedded_images)埋め込みリソースとして格納されているビットマップへのアクセスの詳細については資料です)。 

`ImageResourceExtension`が必要です、`Source`プロパティを設定する、`Source`プロパティは、クラスのコンテンツのプロパティとして属性で示されます。 つまり、`Source=`中かっこで式の一部を省略することができます。 **イメージ リソース デモ** ページで、`Image`要素は、フォルダー名とピリオドで区切られたファイル名を使用して 2 つのイメージをフェッチします。

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

3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![イメージのリソースのデモ](creating-images/imageresourcedemo-small.png "イメージ リソース デモ")](creating-images/imageresourcedemo-large.png#lightbox "イメージのリソースのデモ")

## <a name="service-providers"></a>サービス プロバイダー

使用して、`IServiceProvider`引数`ProvideValue`、XAML マークアップ拡張機能は、使用しているされて XAML ファイルに関する有用な情報へのアクセスを取得できます。 使用して、`IServiceProvider`引数が正常に必要なサービスの種類は特定のコンテキストで使用できます。 既存の XAML マークアップ拡張機能でのソース コードを調べることでは、この機能を理解するために最適な方法、 [ **Markupextension**フォルダー](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) github Xamarin.Forms リポジトリにします。 サービスの種類によって Xamarin.Forms の内部にあることに注意してください。

一部の XAML マークアップ拡張機能では、このサービスが便利です可能性があります。

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget`インターフェイスが 2 つのプロパティを定義`TargetObject`と`TargetProperty`です。 この情報を取得するときに、`ImageResourceExtension`クラス、`TargetObject`は、`Image`と`TargetProperty`は、`BindableProperty`オブジェクトに対して、`Source`のプロパティ`Image`です。 これは、XAML マークアップ拡張機能が設定されているプロパティです。

`GetService`の引数による呼び出し`typeof(IProvideValueTarget)`型のオブジェクトを実際に返す`SimpleValueTargetProvider`で定義されている、`Xamarin.Forms.Xaml.Internals`名前空間。 戻り値をキャストする場合`GetService`その型にアクセスすることも、`ParentObjects`プロパティを格納する配列、`Image`要素、`Grid`親、および`ImageResourceDemoPage`の親、`Grid`です。

## <a name="conclusion"></a>まとめ

XAML マークアップ拡張機能は、さまざまなソースからの属性を設定する機能を拡張することによって、XAML で重要な役割を果たします。 さらに、ニーズに合った既存の XAML マークアップ拡張機能が提供されない場合も記述できます独自。 


## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 帳から XAML マークアップ拡張機能の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
