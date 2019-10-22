---
title: XAML マークアップ拡張の使用
description: この記事では、さまざまなソースから要素属性を設定できるようにすることで、Xamarin の XAML マークアップ拡張機能を使用して、XAML のパワーと柔軟性を向上させる方法について説明します。
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/27/2019
ms.openlocfilehash: a8698975d2609599e1404fbb9c87c617a54f23d7
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696348"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML マークアップ拡張の使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML マークアップ拡張機能は、さまざまなソースから要素属性を設定できるようにすることで、XAML のパワーと柔軟性を向上させるのに役立ちます。 Xaml のマークアップ拡張機能のいくつかは、XAML 2009 仕様に含まれています。 これらは、通常の `x` 名前空間プレフィックスを持つ XAML ファイルに表示され、一般的にこのプレフィックスで参照されます。 この記事では、次のマークアップ拡張機能について説明します。

- [`x:Static`](#static) –静的プロパティ、フィールド、または列挙型のメンバーを参照します。
- [`x:Reference`](#reference) –ページの名前付き要素を参照します。
- [`x:Type`](#type) –属性を `System.Type` オブジェクトに設定します。
- [`x:Array`](#array) –特定の型のオブジェクトの配列を構築します。
- [`x:Null`](#null) –属性を `null` の値に設定します。
- [`OnPlatform`](#onplatform) –プラットフォームごとに UI の外観をカスタマイズします。
- [`OnIdiom`](#onidiom) –アプリケーションが実行されているデバイスの表現に基づいて UI の外観をカスタマイズします。
- [`DataTemplate`](#datatemplate-markup-extension) -型を[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に変換します。
- [`FontImage`](#fontimage-markup-extension) -`ImageSource` を表示できる任意のビューにフォントアイコンを表示します。

追加の XAML マークアップ拡張機能は、従来は他の XAML 実装によってサポートされており、Xamarin. Forms でもサポートされています。 これらの詳細については、他の記事で詳しく説明します。

- リソースディクショナリからオブジェクトを参照するには、記事「[**リソースディクショナリ**](~/xamarin-forms/xaml/resource-dictionaries.md)」で説明されているように `StaticResource` します。
- `DynamicResource`-リソースディクショナリ内のオブジェクトの変更に応答します。詳細については、「[**動的スタイル**](~/xamarin-forms/user-interface/styles/dynamic.md)」を参照してください。
- `Binding`-「[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)」で説明されているように、2つのオブジェクトのプロパティ間のリンクを確立します。
- `TemplateBinding`-「[**コントロールテンプレートからのバインド**](~/xamarin-forms/app-fundamentals/templates/control-templates/template-binding.md)」で説明されているように、コントロールテンプレートからデータバインディングを実行します。
- `RelativeSource`-「[相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」の記事で説明されているように、バインディングターゲットの位置に対して相対的なバインディングソースを設定します。

[@No__t_1](xref:Xamarin.Forms.RelativeLayout)レイアウトでは、 [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)カスタムマークアップ拡張機能を使用します。 このマークアップ拡張機能については、記事[**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)を参照してください。

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static マークアップ拡張機能

@No__t_0 マークアップ拡張機能は、 [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)クラスでサポートされています。 クラスには、パブリック定数、静的プロパティ、静的フィールド、または列挙型のメンバーの名前に設定する `string` 型の[`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member)という名前の単一のプロパティがあります。

@No__t_0 を使用する一般的な方法の1つとして、最初に定数または静的変数を持つクラスを定義します。たとえば、次のような小さな `AppConstants` クラスを[**マークアップ拡張**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)プログラムに定義します。

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**[X:Static Demo]** ページでは、`x:Static` マークアップ拡張機能を使用するいくつかの方法を示します。 最も詳細な方法では、`Label.FontSize` のプロパティ要素タグ間で `StaticExtension` クラスをインスタンス化します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

XAML パーサーでは、`StaticExtension` クラスを `x:Static` として省略することもできます。

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

これをさらに簡略化することもできますが、変更には新しい構文がいくつか導入されています。これは、`StaticExtension` クラスとメンバー設定を中かっこで囲むことで構成されます。 結果として得られる式は、`FontSize` 属性に直接設定されます。

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

中かっこ内に引用符が*ない*ことに注意してください。 @No__t_1 の `Member` プロパティは、XML 属性ではなくなりました。 代わりに、マークアップ拡張機能の式の一部になります。

オブジェクト要素として使用するときに `x:Static` `x:StaticExtension` を省略できるように、中かっこ内の式で省略することもできます。

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

@No__t_0 クラスには、プロパティ `Member` を参照する `ContentProperty` 属性があります。この属性は、このプロパティをクラスの既定のコンテンツプロパティとしてマークします。 中かっこで囲まれた XAML マークアップ拡張機能では、式の `Member=` 部分を削除できます。

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

これは、`x:Static` マークアップ拡張機能の最も一般的な形式です。

**静的なデモ**ページには、他の2つの例が含まれています。 XAML ファイルのルートタグには、.NET `System` 名前空間の XML 名前空間宣言が含まれています。

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

これにより、`Label` のフォントサイズを静的フィールド `Math.PI` に設定できます。 その結果、テキストが小さくなるので、`Scale` プロパティは `Math.E` に設定されます。

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

最後の例では、`Device.RuntimePlatform` 値が表示されます。 @No__t_0 の静的プロパティは、2つの `Span` オブジェクトの間に改行文字を挿入するために使用されます。

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

次に、の実行例を示します。

[![x:Static デモ](consuming-images/staticdemo-small.png "x:Static デモ")](consuming-images/staticdemo-large.png#lightbox "x:Static デモ")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference のマークアップ拡張機能

@No__t_0 マークアップ拡張機能は、 [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)クラスでサポートされています。 クラスには、`string` 型の[`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name)という名前の1つのプロパティがあり、`x:Name` を持つ名前が指定されたページ上の要素の名前に設定します。 この `Name` プロパティは `ReferenceExtension` の content プロパティであるため、中かっこで囲まれた `x:Reference` の場合 `Name=` は必要ありません。

@No__t_0 マークアップ拡張機能は、データバインディングで排他的に使用されます。詳細については、「[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

**X:Reference Demo**ページには、データバインディングを使用した `x:Reference` の2つの使用方法が示されています。最初の例では、`Binding` オブジェクトの `Source` プロパティを設定します。2つ目は、2つのデータバインディングの `BindingContext` プロパティを設定するために使用されます。:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

どちらの `x:Reference` 式でも `ReferenceExtension` クラス名の省略形が使用され、式の `Name=` 部分は削除されます。 最初の例では、`x:Reference` マークアップ拡張機能が `Binding` マークアップ拡張機能に埋め込まれています。 @No__t_0 と `StringFormat` の設定がコンマで区切られていることに注意してください。 実行中のプログラムを次に示します。

[![x:Reference のデモ](consuming-images/referencedemo-small.png "x:Reference のデモ")](consuming-images/referencedemo-large.png#lightbox "x:Reference のデモ")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type マークアップ拡張機能

@No__t_0 マークアップ拡張機能は、 C# [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/)キーワードに相当する XAML です。 これは[`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)クラスによってサポートされています。このクラスは、クラスまたは構造体の名前に設定されている `string` 型の[`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName)という名前のプロパティを定義します。 @No__t_0 マークアップ拡張機能は、そのクラスまたは構造体の[`System.Type`](xref:System.Type)オブジェクトを返します。 `TypeName` は `TypeExtension` の content プロパティであるため、中かっこで囲まれた `x:Type` の場合、`TypeName=` は必要ありません。

Xamarin. フォーム内には、`Type` 型の引数を持ついくつかのプロパティがあります。 例として、`Style` の[`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティや、ジェネリッククラスの引数を指定するために使用される[x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments)属性があります。 ただし、XAML パーサーは、`typeof` 操作を自動的に実行します。このような場合、`x:Type` マークアップ拡張機能は使用されません。

@No__t_0*が*必要な場所の1つは、`x:Array` マークアップ拡張機能を使用することです。これについては、[次のセクション](#array)で説明します。

@No__t_0 マークアップ拡張機能は、各メニュー項目が特定の型のオブジェクトに対応するメニューを構築する場合にも便利です。 @No__t_0 オブジェクトを各メニュー項目に関連付けて、メニュー項目を選択したときにオブジェクトをインスタンス化できます。

これは、**マークアップ拡張機能**プログラムの `MainPage` のナビゲーションメニューの動作です。 **Mainpage.xaml**ファイルには、プログラム内の特定のページに対応する各 `TextCell` の `TableView` が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

**マークアップ拡張機能**の開始メインページは次のとおりです。

[![メインページ](consuming-images/mainpage-small.png "メインページ")](consuming-images/mainpage-large.png#lightbox "メインページ")

各 `CommandParameter` プロパティは、他のページの1つを参照する `x:Type` マークアップ拡張機能に設定されます。 @No__t_0 プロパティは `NavigateCommand` という名前のプロパティにバインドされます。 このプロパティは、`MainPage` 分離コードファイルで定義されています。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

@No__t_0 プロパティは、`CommandParameter` の値 &mdash; 型 `Type` の引数を使用して execute コマンドを実装する `Command` オブジェクトです。 メソッドは、`Activator.CreateInstance` を使用してページをインスタンス化し、そのページに移動します。 コンストラクターは、ページの `BindingContext` をそれ自体に設定することによって終了します。これにより、`Command` 上の `Binding` が機能するようになります。 この種類のコードの詳細については、[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関する記事と、特に[**コマンド**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)の使用に関する記事を参照してください。

**X:Type Demo**ページでは、同様の手法を使用して、Xamarin の要素をインスタンス化し、それを `StackLayout` に追加します。 XAML ファイルは、最初は3つの `Button` 要素で構成され、`Command` プロパティは `Binding` に設定され、`CommandParameter` プロパティは次の3つの Xamarin ビューの種類に設定されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

分離コードファイルは、`CreateCommand` プロパティを定義し、初期化します。

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

@No__t_0 が押されたときに実行されるメソッドは、引数の新しいインスタンスを作成し、その `VerticalOptions` プロパティを設定して、`StackLayout` に追加します。 次に、3つの `Button` 要素は、動的に作成されたビューとページを共有します。

[![x:Type のデモ](consuming-images/typedemo-small.png "x:Type のデモ")](consuming-images/typedemo-large.png#lightbox "x:Type のデモ")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array のマークアップ拡張機能

@No__t_0 マークアップ拡張機能を使用すると、マークアップで配列を定義できます。 これは、次の2つのプロパティを定義する[`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)クラスによってサポートされています。

- 配列内の要素の型を示す `Type` 型の `Type`。
- `IList` 型の `Items`。これは、項目自体のコレクションです。 これは `ArrayExtension` の content プロパティです。

@No__t_0 マークアップ拡張機能自体は、中かっこで囲まれていません。 代わりに、開始タグと終了タグを `x:Array` して、項目の一覧を区切ります。 @No__t_0 プロパティを `x:Type` マークアップ拡張機能に設定します。

**X:Array Demo**ページでは、`ItemsSource` プロパティを配列に設定することによって、`x:Array` を使用して `ListView` に項目を追加する方法を示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

@No__t_0 は、各カラーエントリに対して単純な `BoxView` を作成します。

[![x:Array のデモ](consuming-images/arraydemo-small.png "x:Array のデモ")](consuming-images/arraydemo-large.png#lightbox "x:Array のデモ")

この配列内の個々の `Color` 項目を指定するには、いくつかの方法があります。 @No__t_0 マークアップ拡張機能を使用できます。

```xaml
<x:Static Member="Color.Blue" />
```

または、`StaticResource` を使用して、リソースディクショナリから色を取得することもできます。

```xaml
<StaticResource Key="myColor" />
```

この記事の最後には、新しいカラー値も作成するカスタム XAML マークアップ拡張機能が表示されます。

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

文字列や数値などの共通型の配列を定義する場合は、[**コンストラクター引数の引き渡し**](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)に関する記事に記載されているタグを使用して、値を区切ります。

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null マークアップ拡張機能

@No__t_0 マークアップ拡張機能は、 [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)クラスでサポートされています。 これにはプロパティはなく、単にC# [`null`](/dotnet/csharp/language-reference/keywords/null/)キーワードに相当する XAML です。

@No__t_0 マークアップ拡張機能はほとんど必要ありませんが、ほとんど使用されませんが、必要な場合は、存在していることを確認してください。

**X:Null Demo**ページは、`x:Null` が便利な場合の1つのシナリオを示しています。 @No__t_3 プロパティをプラットフォームに依存するファミリ名に設定する `Setter` を含む `Label` の暗黙的な `Style` を定義するとします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

次に、`Label` の要素の1つについて、暗黙的な `Style` のすべてのプロパティ設定 (既定値にする `FontFamily` を除く) を必要としていることがわかります。 この目的に対して別の `Style` を定義することもできますが、より簡単な方法は、中央の `Label` に示すように、特定の `Label` の `FontFamily` プロパティを `x:Null` に設定することです。

実行中のプログラムを次に示します。

[![x:Null のデモ](consuming-images/nulldemo-small.png "x:Null のデモ")](consuming-images/nulldemo-large.png#lightbox "x:Null のデモ")

@No__t_0 の要素の4つにはセリフフォントがありますが、中央の `Label` には既定の sans serif フォントがあります。

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform マークアップ拡張機能

@No__t_0 マークアップ拡張機能を使用すると、プラットフォームごとに UI の外観をカスタマイズできます。 [@No__t_1](xref:Xamarin.Forms.OnPlatform`1)クラスと[`On`](xref:Xamarin.Forms.On)クラスと同じ機能を提供しますが、より簡潔に表現できます。

@No__t_0 マークアップ拡張機能は、次のプロパティを定義する[`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension)クラスによってサポートされています。

- `object` 型の `Default`。プラットフォームを表すプロパティに適用される既定値に設定します。
- Android に適用する値に設定した `object` 型の `Android`。
- `object` 型の `GTK`。 GTK プラットフォームに適用される値に設定します。
- iOS に適用する値に設定した `object` 型の `iOS`。
- macOS に適用される値に設定する `object` 型の `macOS`。
- Tizen プラットフォームで適用される値に設定する `object` 型の `Tizen`。
- ユニバーサル Windows プラットフォームに適用される値に設定する `object` 型の `UWP`。
- Windows Presentation Foundation プラットフォームに適用される値に設定する `object` 型の `WPF`。
- `IValueConverter` の実装に設定する `IValueConverter` 型の `Converter`。
- `object` 型の `ConverterParameter`。 `IValueConverter` の実装に渡す値に設定します。

> [!NOTE]
> XAML パーサーでは、 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension)クラスを `OnPlatform` として省略できます。

@No__t_0 プロパティは `OnPlatformExtension` の content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式の場合は、最初の引数であることを示す式の `Default=` 部分を削除できます。 @No__t_0 プロパティが設定されていない場合は、マークアップ拡張機能が[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)を対象としている場合は、 [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue)プロパティ値が既定値になります。

> [!IMPORTANT]
> XAML パーサーでは、正しい型の値が、`OnPlatform` マークアップ拡張機能を使用するプロパティに提供されることを想定しています。 型変換が必要な場合、`OnPlatform` マークアップ拡張機能は、Xamarin. Forms によって提供される既定のコンバーターを使用してこれを実行しようとします。 ただし、既定のコンバーターでは実行できない型変換がいくつかあります。そのような場合は、`Converter` プロパティを `IValueConverter` の実装に設定する必要があります。

**Onplatform のデモ**ページでは、`OnPlatform` マークアップ拡張機能の使用方法を示しています。

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

この例では、3つのすべての `OnPlatform` 式は、`OnPlatformExtension` クラス名の省略されたバージョンを使用します。 3つの `OnPlatform` マークアップ拡張機能は、 [`BoxView`](xref:Xamarin.Forms.BoxView)の[`Color`](xref:Xamarin.Forms.BoxView.Color)、 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)、および[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティを、iOS、Android、UWP の異なる値に設定します。 また、マークアップ拡張機能は、指定されていないプラットフォームでこれらのプロパティの既定値を提供し、式の `Default=` 部分を削除します。 設定されているマークアップ拡張プロパティは、コンマで区切られていることに注意してください。

実行中のプログラムを次に示します。

[![OnPlatform デモ](consuming-images/onplatformdemo-small.png "OnPlatform デモ")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform デモ")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom のマークアップ拡張機能

@No__t_0 マークアップ拡張機能を使用すると、アプリケーションが実行されているデバイスの表現に基づいて UI の外観をカスタマイズできます。 これは、次のプロパティを定義する[`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension)クラスによってサポートされています。

- `object` 型の `Default`。デバイスの表現方法を表すプロパティに適用される既定値に設定します。
- 電話に適用される値に設定する `object` 型の `Phone`。
- タブレットで適用される値に設定する `object` 型の `Tablet`。
- `object` 型の `Desktop` は、デスクトッププラットフォームに適用される値に設定します。
- `object` 型の `TV`。 TV プラットフォームに適用される値に設定します。
- Watch プラットフォームに適用される値に設定する `object` 型の `Watch`。
- `IValueConverter` の実装に設定する `IValueConverter` 型の `Converter`。
- `object` 型の `ConverterParameter`。 `IValueConverter` の実装に渡す値に設定します。

> [!NOTE]
> XAML パーサーでは、 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension)クラスを `OnIdiom` として省略できます。

@No__t_0 プロパティは `OnIdiomExtension` の content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式の場合は、最初の引数であることを示す式の `Default=` 部分を削除できます。

> [!IMPORTANT]
> XAML パーサーでは、正しい型の値が、`OnIdiom` マークアップ拡張機能を使用するプロパティに提供されることを想定しています。 型変換が必要な場合、`OnIdiom` マークアップ拡張機能は、Xamarin. Forms によって提供される既定のコンバーターを使用してこれを実行しようとします。 ただし、既定のコンバーターでは実行できない型変換がいくつかあります。そのような場合は、`Converter` プロパティを `IValueConverter` の実装に設定する必要があります。

**Onidiom のデモ**ページでは、`OnIdiom` マークアップ拡張機能の使用方法を示しています。

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

この例では、3つのすべての `OnIdiom` 式は、`OnIdiomExtension` クラス名の省略されたバージョンを使用します。 3つの `OnIdiom` マークアップ拡張機能は、 [`BoxView`](xref:Xamarin.Forms.BoxView)の[`Color`](xref:Xamarin.Forms.BoxView.Color)、 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)、および[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティを、電話、タブレット、およびデスクトップの各表現の異なる値に設定します。 また、マークアップ拡張機能は、指定されていない表現でこれらのプロパティの既定値を提供し、式の `Default=` 部分を除去します。 設定されているマークアップ拡張プロパティは、コンマで区切られていることに注意してください。

実行中のプログラムを次に示します。

[![OnIdiom のデモ](consuming-images/onidiomdemo-small.png "OnIdiom のデモ")](consuming-images/onidiomdemo-large.png#lightbox "OnIdiom のデモ")

## <a name="datatemplate-markup-extension"></a>System.windows.datatemplate> のマークアップ拡張機能

@No__t_0 マークアップ拡張機能を使用すると、型を[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に変換できます。 これは `DataTemplateExtension` クラスによってサポートされています。このクラスは `string` 型の `TypeName` プロパティを定義します。これは、`DataTemplate` に変換される型の名前に設定されます。 @No__t_0 プロパティは `DataTemplateExtension` の content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では、式の `TypeName=` 部分を削除できます。

> [!NOTE]
> XAML パーサーでは、`DataTemplateExtension` クラスを `DataTemplate` として省略できます。

このマークアップ拡張機能の一般的な使用方法は、次の例に示すように、シェルアプリケーション内にあります。

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

この例では、`MonkeysPage` が[`ContentPage`](xref:Xamarin.Forms.ContentPage)から[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に変換されます。これは、`ShellContent.ContentTemplate` プロパティの値として設定されます。 これにより、アプリケーションの起動時ではなく、ページへの移動が発生した場合にのみ `MonkeysPage` が作成されるようになります。

シェルアプリケーションの詳細については、「 [Xamarin. フォームシェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

## <a name="fontimage-markup-extension"></a>FontImage のマークアップ拡張機能

@No__t_0 マークアップ拡張機能を使用すると、`ImageSource` を表示できる任意のビューにフォントアイコンを表示できます。 @No__t_0 クラスと同じ機能を提供しますが、より簡潔に表現できます。

@No__t_0 マークアップ拡張機能は、次のプロパティを定義する `FontImageExtension` クラスによってサポートされています。

- フォントアイコンが属するフォントファミリ `string` 型の `FontFamily`。
- `string` 型の `Glyph`、フォントアイコンの unicode 文字の値です。
- `Color` 型の `Color`、フォントアイコンを表示するときに使用する色です。
- `double` 型の `Size`。表示されるフォントアイコンのサイズ (デバイスに依存しない単位) です。

> [!NOTE]
> XAML パーサーでは、`FontImageExtension` クラスを `FontImage` として省略できます。

@No__t_0 プロパティは `FontImageExtension` の content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式の場合は、最初の引数であることを示す式の `Glyph=` 部分を削除できます。

**FontImage Demo**ページでは、`FontImage` マークアップ拡張機能の使用方法を示しています。

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

この例では、`FontImageExtension` クラス名の省略されたバージョンを使用して、 [`Image`](xref:Xamarin.Forms.Image)の Ionicons フォントファミリから XBox アイコンを表示します。 また、この式では、`OnPlatform` マークアップ拡張機能を使用して、iOS と Android で異なる `FontFamily` プロパティ値を指定します。 また、式の `Glyph=` 部分が削除され、設定されているマークアップ拡張機能のプロパティはコンマで区切られます。 アイコンの unicode 文字は `\uf30c` ますが、XAML でエスケープする必要があるため、`&#xf30c;` になります。

実行中のプログラムを次に示します。

[![FontImage マークアップ拡張機能のスクリーンショット](consuming-images/fontimagedemo.png "FontImage のデモ")](consuming-images/fontimagedemo-large.png#lightbox "FontImage のデモ")

@No__t_0 オブジェクトのフォントアイコンデータを指定してフォントアイコンを表示する方法の詳細については、「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="define-your-own-markup-extensions"></a>独自のマークアップ拡張機能を定義する

Xamarin. Forms では使用できない XAML マークアップ拡張機能が必要な場合は、[独自に作成](creating.md)することができます。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Xamarin. Forms book の章の XAML マークアップ拡張機能](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. フォームシェル](~/xamarin-forms/app-fundamentals/shell/index.md)。
