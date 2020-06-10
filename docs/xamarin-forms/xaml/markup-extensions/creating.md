---
title: "XAML マークアップ拡張機能の作成" の説明: "この記事では、独自のカスタム Xamarin.Forms XAML マークアップ拡張機能を定義する方法について説明します。 XAML マークアップ拡張機能は、IMarkupExtension インターフェイスまたは IMarkupExtension インターフェイスを実装するクラスです <T> 。 "
ms. 製品: xamarin ms. assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 01/05/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="creating-xaml-markup-extensions"></a>XAML マークアップ拡張の作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

プログラムレベルでは、XAML マークアップ拡張機能は、 [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) インターフェイスまたはインターフェイスを実装するクラスです [`IMarkupExtension<T>`](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) 。 GitHub リポジトリの[**マークアップ拡張**ディレクトリ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)で、以下に説明する標準のマークアップ拡張機能のソースコードを調べることができ Xamarin.Forms ます。

またはから派生することで、独自のカスタム XAML マークアップ拡張機能を定義することもでき `IMarkupExtension` `IMarkupExtension<T>` ます。 マークアップ拡張機能が特定の型の値を取得する場合は、汎用フォームを使用します。 これは、いくつかのマークアップ拡張機能がある場合に当てはまり Xamarin.Forms ます。

- `TypeExtension` は、`IMarkupExtension<Type>` から派生します
- `ArrayExtension` は、`IMarkupExtension<Array>` から派生します
- `DynamicResourceExtension` は、`IMarkupExtension<DynamicResource>` から派生します
- `BindingExtension` は、`IMarkupExtension<BindingBase>` から派生します
- `ConstraintExpression` は、`IMarkupExtension<Constraint>` から派生します

2つの `IMarkupExtension` インターフェイスは、それぞれ1つのメソッドを定義し `ProvideValue` ます。

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

は `IMarkupExtension<T>` から派生 `IMarkupExtension` し、にキーワードが含まれているため `new` `ProvideValue` 、両方のメソッドが含まれてい `ProvideValue` ます。

多くの場合、XAML マークアップ拡張機能は、戻り値に寄与するプロパティを定義します。 (明らかな例外はで `NullExtension` 、は `ProvideValue` 単にを返し `null` ます)。`ProvideValue`このメソッドには、 `IServiceProvider` この記事の後半で説明する型の引数が1つあります。

## <a name="a-markup-extension-for-specifying-color"></a>色を指定するためのマークアップ拡張機能

次の XAML マークアップ拡張機能を使用すると、 `Color` 色合い、鮮やかさ、および輝度のコンポーネントを使用して値を構築できます。 ここでは、1に初期化されるアルファコンポーネントを含む、色の4つのコンポーネントの4つのプロパティを定義します。 クラスはから派生 `IMarkupExtension<Color>` し、 `Color` 戻り値を示します。

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

は `IMarkupExtension<T>` から派生するため `IMarkupExtension` 、クラスにはを返すメソッドと、を返すメソッドの2つが含まれている必要があり `ProvideValue` `Color` `object` ますが、2番目のメソッドは単に最初のメソッドを呼び出すことができます。

[ **HSL 色のデモ**] ページには、 `HslColorExtension` XAML ファイルに表示されるの色を指定するさまざまな方法が示されてい `BoxView` ます。

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

`HslColorExtension`が XML タグの場合、4つのプロパティは属性として設定されますが、中かっこの間に出現すると、4つのプロパティは引用符なしでコンマで区切られます。 `H`、、およびの既定値 `S` `L` は0で、の既定値 `A` は1であるため、既定値に設定する場合は、これらのプロパティを省略できます。 最後の例では、明度が0の例を示しています。通常は黒が使用されますが、アルファチャネルは0.5 であるため、半透明で、ページの白い背景に対して灰色で表示されます。

[![HSL の色のデモ](creating-images/hslcolordemo-small.png "HSL の色のデモ")](creating-images/hslcolordemo-large.png#lightbox "HSL の色のデモ")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>ビットマップにアクセスするためのマークアップ拡張機能

の引数は、 `ProvideValue` [`IServiceProvider`](xref:System.IServiceProvider) .net 名前空間で定義されているインターフェイスを実装するオブジェクトです `System` 。 このインターフェイスには、引数を持つという名前のメソッドを持つ1つのメンバーがあり `GetService` `Type` ます。

`ImageResourceExtension`次に示すクラスは、との1つの使用方法を示して `IServiceProvider` `GetService` います。これは、 `IXmlLineInfoProvider` 特定のエラーが検出された場所を示す行と文字の情報を提供できるオブジェクトを取得するためです。 この場合、 `Source` プロパティが設定されていないときに例外が発生します。

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

`ImageResourceExtension`は、XAML ファイルが、.NET Standard ライブラリプロジェクトに埋め込まれたリソースとして格納されているイメージファイルにアクセスする必要がある場合に便利です。 このメソッドは、プロパティを使用して `Source` 静的メソッドを呼び出し `ImageSource.FromResource` ます。 このメソッドには完全修飾リソース名が必要です。これは、アセンブリ名、フォルダー名、およびピリオドで区切られたファイル名で構成されます。 メソッドの2番目の引数は `ImageSource.FromResource` アセンブリ名を提供し、UWP のリリースビルドにのみ必要です。 に関係なく、は `ImageSource.FromResource` ビットマップを含むアセンブリから呼び出す必要があります。つまり、この XAML リソース拡張機能を外部ライブラリに含めることはできません。ただし、そのライブラリにも画像が含まれている必要はありません。 (埋め込みリソースとして格納されているビットマップへのアクセスの詳細については、[**埋め込み画像**](~/xamarin-forms/user-interface/images.md#embedded-images)に関する記事を参照してください)。

ではプロパティが設定されている `ImageResourceExtension` 必要があり `Source` ますが、 `Source` プロパティは、クラスの content プロパティとして属性で示されています。 これは、 `Source=` 中かっこ内の式の一部を省略できることを意味します。 **イメージリソースのデモ**ページでは、 `Image` 要素はフォルダー名とファイル名をピリオドで区切って2つのイメージをフェッチします。

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

[![イメージリソースのデモ](creating-images/imageresourcedemo-small.png "イメージリソースのデモ")](creating-images/imageresourcedemo-large.png#lightbox "イメージリソースのデモ")

## <a name="service-providers"></a>サービス プロバイダー

の引数を使用 `IServiceProvider` する `ProvideValue` と、xaml マークアップ拡張機能は、使用されている xaml ファイルに関する有用な情報にアクセスできます。 ただし、引数を正常に使用するに `IServiceProvider` は、特定のコンテキストで使用できるサービスの種類を把握しておく必要があります。 この機能を理解する最善の方法は、GitHub のリポジトリの[**マークアップ拡張**フォルダー](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)にある既存の XAML マークアップ拡張機能のソースコードを調べることです Xamarin.Forms 。 一部の種類のサービスは内部にあることに注意 Xamarin.Forms してください。

一部の XAML マークアップ拡張機能では、このサービスが役に立つ場合があります。

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

インターフェイスは、 `IProvideValueTarget` とという2つのプロパティを定義し `TargetObject` `TargetProperty` ます。 この情報がクラスで取得されると、はで、は `ImageResourceExtension` `TargetObject` `Image` `TargetProperty` `BindableProperty` `Source` のプロパティのオブジェクトです `Image` 。 これは、XAML マークアップ拡張機能が設定されているプロパティです。

`GetService`引数を指定した呼び出しは、実際には `typeof(IProvideValueTarget)` `SimpleValueTargetProvider` 、名前空間で定義されている型のオブジェクトを返し `Xamarin.Forms.Xaml.Internals` ます。 の戻り値をその型にキャストした場合は、の `GetService` `ParentObjects` `Image` 要素、 `Grid` 親、および `ImageResourceDemoPage` の親を含む配列であるプロパティにもアクセスできます `Grid` 。

## <a name="conclusion"></a>まとめ

XAML マークアップ拡張機能は、さまざまなソースから属性を設定する機能を拡張することによって、XAML の重要な役割を果たします。 また、既存の XAML マークアップ拡張機能が必要なものを正確に提供していない場合は、独自の拡張機能を作成することもできます。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [本の XAML マークアップ拡張機能の章 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
