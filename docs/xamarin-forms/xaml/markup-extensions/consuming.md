---
title: XAML マークアップ拡張の使用
description: この記事では、 Xamarin.Forms xaml マークアップ拡張機能を使用して、さまざまなソースから要素属性を設定できるようにすることで、xaml のパワーと柔軟性を向上させる方法について説明します。
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e1429c3f39e37dc552d7f6ca8767058e5aec853b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84903111"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML マークアップ拡張の使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML マークアップ拡張機能は、さまざまなソースから要素属性を設定できるようにすることで、XAML のパワーと柔軟性を向上させるのに役立ちます。 Xaml のマークアップ拡張機能のいくつかは、XAML 2009 仕様に含まれています。 これらは、慣例として名前空間プレフィックスを持つ XAML ファイルに表示され、 `x` 一般的にこのプレフィックスで参照されます。 この記事では、次のマークアップ拡張機能について説明します。

- [`x:Static`](#xstatic-markup-extension)–静的プロパティ、フィールド、または列挙型のメンバーを参照します。
- [`x:Reference`](#xreference-markup-extension)–ページの名前付き要素を参照します。
- [`x:Type`](#xtype-markup-extension)–属性をオブジェクトに設定 `System.Type` します。
- [`x:Array`](#xarray-markup-extension)–特定の型のオブジェクトの配列を構築します。
- [`x:Null`](#xnull-markup-extension)–属性を値に設定 `null` します。
- [`OnPlatform`](#onplatform-markup-extension)–プラットフォームごとに UI の外観をカスタマイズします。
- [`OnIdiom`](#onidiom-markup-extension)–アプリケーションが実行されているデバイスの表現に基づいて UI の外観をカスタマイズします。
- [`DataTemplate`](#datatemplate-markup-extension)–型をに変換 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) します。
- [`FontImage`](#fontimage-markup-extension)–を表示できる任意のビューにフォントアイコンを表示 `ImageSource` します。
- [`AppThemeBinding`](#appthemebinding-markup-extension)–現在のシステムテーマに基づいてリソースを使用します。

追加の XAML マークアップ拡張機能は、従来は他の XAML 実装によってサポートされており、でもサポートされてい Xamarin.Forms ます。 これらの詳細については、他の記事で詳しく説明します。

- `StaticResource`-リソースディクショナリからオブジェクトを参照します。詳細については、「[**リソースディクショナリ**](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。
- `DynamicResource`-リソースディクショナリ内のオブジェクトの変更に応答します。詳細については、「[**動的スタイル**](~/xamarin-forms/user-interface/styles/dynamic.md)」を参照してください。
- `Binding`-「[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)」で説明されているように、2つのオブジェクトのプロパティ間のリンクを確立します。
- `TemplateBinding`-コントロールテンプレートからデータバインディングを実行します。これについては、「 [** Xamarin.Forms コントロールテンプレート**](~/xamarin-forms/app-fundamentals/templates/control-template.md)」をご覧ください。
- `RelativeSource`-バインドソースを、バインディングターゲットの位置に対して相対的に設定します。これについては、「[相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」を参照してください。

レイアウトでは、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) カスタムマークアップ拡張機能を使用し [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) ます。 このマークアップ拡張機能については、記事[**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)を参照してください。

## <a name="xstatic-markup-extension"></a>x:Static マークアップ拡張機能

`x:Static`マークアップ拡張機能はクラスによってサポートされてい [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) ます。 クラスには、 [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) `string` パブリック定数、静的プロパティ、静的フィールド、または列挙型のメンバーの名前に設定する、type という名前の単一のプロパティがあります。

を使用する一般的な方法の1つとし `x:Static` て、最初に定数または静的変数を含むクラスを定義し `AppConstants` ます。たとえば、[**マークアップ拡張**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)プログラムでは、この小さなクラスを使用します。

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

[ **X:Static Demo** ] ページでは、マークアップ拡張機能を使用するいくつかの方法を示し `x:Static` ます。 最も詳細な方法では、 `StaticExtension` プロパティ要素タグ間でクラスをインスタンス化し `Label.FontSize` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
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

また、XAML パーサーでは、 `StaticExtension` クラスを次のように省略でき `x:Static` ます。

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

これはさらに簡略化できますが、変更には新しい構文がいくつか導入されています。これは、 `StaticExtension` クラスとメンバーの設定を中かっこで囲むことで構成されます。 結果として得られる式は、属性に直接設定され `FontSize` ます。

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

中かっこ内に引用符が*ない*ことに注意してください。 `Member`のプロパティ `StaticExtension` が XML 属性ではなくなりました。 代わりに、マークアップ拡張機能の式の一部になります。

`x:StaticExtension`オブジェクト要素として使用するときに、省略可能であるように `x:Static` 、中かっこ内の式で省略することもできます。

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension`クラスには、 `ContentProperty` プロパティを参照する属性があり `Member` ます。これは、このプロパティをクラスの既定のコンテンツプロパティとしてマークします。 中かっこで囲まれた XAML マークアップ拡張機能では、 `Member=` 式の一部を削除できます。

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

これは、 `x:Static` マークアップ拡張機能の最も一般的な形式です。

**静的なデモ**ページには、他の2つの例が含まれています。 XAML ファイルのルートタグには、.NET 名前空間の XML 名前空間宣言が含まれてい `System` ます。

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

これにより、 `Label` フォントサイズを静的フィールドに設定でき `Math.PI` ます。 その結果、テキストは小さくなり、 `Scale` プロパティは次のように設定され `Math.E` ます。

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

最後の例では、値が表示され `Device.RuntimePlatform` ます。 `Environment.NewLine`静的プロパティは、2つのオブジェクトの間に改行文字を挿入するために使用され `Span` ます。

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

## <a name="xreference-markup-extension"></a>x:Reference のマークアップ拡張機能

`x:Reference`マークアップ拡張機能はクラスによってサポートされてい [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) ます。 クラスには、という名前の単一のプロパティがあります。このプロパティには、 [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) という名前が付けられた `string` ページ上の要素の名前を設定し `x:Name` ます。 この `Name` プロパティはのコンテンツプロパティであるため、中 `ReferenceExtension` `Name=` かっこで囲まれている場合は必要ありません `x:Reference` 。

`x:Reference`マークアップ拡張機能は、データバインディングで排他的に使用されます。詳細については、「[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

**X:Reference Demo**ページでは、データバインディングを使用したの2つの使用方法が示されています `x:Reference` 。最初に、オブジェクトのプロパティを設定するために使用します。2つ目は、 `Source` `Binding` `BindingContext` 2 つのデータバインディングのプロパティを設定するために使用されます。

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

どちらの式でも、 `x:Reference` クラス名の省略版が使用され、 `ReferenceExtension` 式の一部が削除され `Name=` ます。 最初の例では、マークアップ `x:Reference` 拡張機能が `Binding` マークアップ拡張機能に埋め込まれています。 `Source`との `StringFormat` 設定はコンマで区切られていることに注意してください。 実行中のプログラムを次に示します。

[![x:Reference のデモ](consuming-images/referencedemo-small.png "x:Reference のデモ")](consuming-images/referencedemo-large.png#lightbox "x:Reference のデモ")

## <a name="xtype-markup-extension"></a>x:Type マークアップ拡張機能

`x:Type`マークアップ拡張機能は、C# キーワードに相当する XAML [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) です。 クラスによってサポートされてい [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) ます。このクラスは、 `string` クラスまたは構造体の名前に設定されている型の1つのプロパティを定義します。 `x:Type`マークアップ拡張機能は、 [`System.Type`](xref:System.Type) そのクラスまたは構造体のオブジェクトを返します。 `TypeName`はの content プロパティである `TypeExtension` ため、中 `TypeName=` `x:Type` かっこで囲まれている場合は必要ありません。

内 Xamarin.Forms には、型の引数を持ついくつかのプロパティがあり `Type` ます。 たとえば、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) のプロパティ `Style` や、ジェネリッククラスの引数を指定するために使用される[x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#specifying-a-generic-type-argument)属性などがあります。 ただし、XAML パーサーは `typeof` この操作を自動的に実行し `x:Type` ます。この場合、マークアップ拡張機能は使用されません。

`x:Type`*が*必要な場所の1つは、 `x:Array` マークアップ拡張機能を使用することです。これについては、次の[セクション](#xarray-markup-extension)で説明します。

`x:Type`マークアップ拡張機能は、各メニュー項目が特定の型のオブジェクトに対応するメニューを構築する場合にも便利です。 `Type`オブジェクトを各メニュー項目に関連付けて、メニュー項目を選択したときにオブジェクトをインスタンス化することができます。

これは `MainPage` 、**マークアップ拡張機能**プログラムののナビゲーションメニューの動作です。 **Mainpage.xaml**ファイルには、 `TableView` `TextCell` プログラム内の特定のページに対応するそれぞれのが含まれています。

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

各 `CommandParameter` プロパティは `x:Type` 、他のページの1つを参照するマークアップ拡張機能に設定されます。 プロパティは、 `Command` という名前のプロパティにバインドされ `NavigateCommand` ます。 このプロパティは、分離コードファイルで定義されてい `MainPage` ます。

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

`NavigateCommand`プロパティは、 `Command` `Type` &mdash; の値を型とする引数を指定して execute コマンドを実装するオブジェクトです `CommandParameter` 。 このメソッドは、を使用して `Activator.CreateInstance` ページをインスタンス化し、そのページに移動します。 コンストラクターは、ページのをそれ自体に設定することによって終了し `BindingContext` ます。これにより、が動作するようになり `Binding` `Command` ます。 この種類のコードの詳細については、[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関する記事と、特に[**コマンド**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)の使用に関する記事を参照してください。

**X:Type Demo**ページでは、同様の手法を使用して要素をインスタンス化 Xamarin.Forms し、に追加し `StackLayout` ます。 XAML ファイルは、最初は3つの要素で構成され、 `Button` `Command` プロパティはに設定さ `Binding` れ、 `CommandParameter` プロパティは3つのビューの種類に設定され Xamarin.Forms ます。

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

分離コードファイルでは、プロパティを定義し、初期化し `CreateCommand` ます。

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

が押されたときに実行されるメソッドは、 `Button` 引数の新しいインスタンスを作成し、その `VerticalOptions` プロパティを設定して、に追加し `StackLayout` ます。 次に、3つの要素が、 `Button` 動的に作成されたビューとページを共有します。

[![x:Type のデモ](consuming-images/typedemo-small.png "x:Type のデモ")](consuming-images/typedemo-large.png#lightbox "x:Type のデモ")

## <a name="xarray-markup-extension"></a>x:Array のマークアップ拡張機能

`x:Array`マークアップ拡張機能を使用すると、マークアップで配列を定義できます。 これは、次の [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) 2 つのプロパティを定義するクラスによってサポートされています。

- `Type`型の `Type` 。配列内の要素の型を示します。
- `Items`型の `IList` 。これは、項目自体のコレクションです。 これは、の content プロパティです `ArrayExtension` 。

`x:Array`マークアップ拡張機能自体は、中かっこで囲まれていません。 代わりに、 `x:Array` 開始タグと終了タグによって項目のリストが区切られます。 プロパティを `Type` `x:Type` マークアップ拡張機能に設定します。

**X:Array Demo**ページでは、を使用し `x:Array` て、プロパティを `ListView` 配列に設定することによってに項目を追加する方法を示してい `ItemsSource` ます。

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

は、 `ViewCell` `BoxView` 各カラーエントリに対して単純なを作成します。

[![x:Array のデモ](consuming-images/arraydemo-small.png "x:Array のデモ")](consuming-images/arraydemo-large.png#lightbox "x:Array のデモ")

この配列内の個々の項目を指定するには、いくつかの方法があり `Color` ます。 `x:Static`マークアップ拡張機能を使用できます。

```xaml
<x:Static Member="Color.Blue" />
```

または、を使用して、 `StaticResource` リソースディクショナリから色を取得することもできます。

```xaml
<StaticResource Key="myColor" />
```

この記事の最後には、新しいカラー値も作成するカスタム XAML マークアップ拡張機能が表示されます。

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

文字列や数値などの共通型の配列を定義する場合は、[**コンストラクター引数の引き渡し**](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments)に関する記事に記載されているタグを使用して、値を区切ります。

## <a name="xnull-markup-extension"></a>x:Null マークアップ拡張機能

`x:Null`マークアップ拡張機能はクラスによってサポートされてい [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) ます。 これにはプロパティはなく、単純に C# のキーワードに相当する XAML です [`null`](/dotnet/csharp/language-reference/keywords/null/) 。

`x:Null`マークアップ拡張機能はほとんど必要ありませんが、ほとんど使用されていませんが、必要な場合は、存在していることを確認してください。

**X:Null Demo**ページには、便利な場合があるシナリオが示されてい `x:Null` ます。 プロパティをプラットフォームに依存する `Style` `Label` `Setter` ファミリ名に設定するを含むの暗黙的なを定義するとし `FontFamily` ます。

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

次に、いずれかの要素について、既定値として使用するを除き、暗黙的なのすべてのプロパティ設定を使用することを確認し `Label` `Style` `FontFamily` ます。 この目的に対して別のを定義することもでき `Style` ますが、より簡単な方法は、 `FontFamily` `Label` `x:Null` 中心に示すように、特定ののプロパティをに設定することです `Label` 。

実行中のプログラムを次に示します。

[![x:Null のデモ](consuming-images/nulldemo-small.png "x:Null のデモ")](consuming-images/nulldemo-large.png#lightbox "x:Null のデモ")

4つの要素には `Label` セリフフォントがありますが、中央に `Label` は既定の sans serif フォントがあります。

## <a name="onplatform-markup-extension"></a>OnPlatform マークアップ拡張機能

`OnPlatform` マークアップ拡張では、プラットフォームごとに UI の外観をカスタマイズできます。 クラスおよびクラスと同じ機能を提供し [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) ますが、より簡潔な表現を使用します。

`OnPlatform`マークアップ拡張機能は、 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 次のプロパティを定義するクラスによってサポートされています。

- `Default`型の `object` 。プラットフォームを表すプロパティに適用される既定値に設定します。
- `Android`型の `object` 。 Android で適用される値に設定します。
- `GTK`型の `object` 。 GTK プラットフォームに適用される値に設定します。
- `iOS`型の `object` 。 iOS で適用される値に設定します。
- `macOS`型の `object` 。 macOS に適用される値に設定します。
- `Tizen`型の `object` 。 Tizen プラットフォームで適用される値に設定します。
- `UWP`型の `object` 。ユニバーサル Windows プラットフォームに適用される値に設定します。
- `WPF`型の `object` 。 Windows Presentation Foundation プラットフォームに適用される値に設定します。
- `Converter`型の `IValueConverter` 。実装に設定でき `IValueConverter` ます。
- `ConverterParameter`型の `object` 。実装に渡す値に設定でき `IValueConverter` ます。

> [!NOTE]
> XAML パーサーでは、 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) クラスをとして省略でき `OnPlatform` ます。

プロパティは、 `Default` の content プロパティです `OnPlatformExtension` 。 そのため、中かっこで囲まれた XAML マークアップ式では、 `Default=` 最初の引数であることを示す式の一部を削除できます。 プロパティが設定されていない場合 `Default` 、 [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) マークアップ拡張機能がを対象としている場合は、プロパティの値が既定値になり [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。

> [!IMPORTANT]
> XAML パーサーでは、適切な型の値がマークアップ拡張機能を使用するプロパティに提供されることを想定して `OnPlatform` います。 型変換が必要な場合、 `OnPlatform` マークアップ拡張機能は、によって提供される既定のコンバーターを使用してこれを実行しようとし Xamarin.Forms ます。 ただし、既定のコンバーターでは実行できない型変換がいくつかあり、このような場合は、 `Converter` プロパティを実装に設定する必要があり `IValueConverter` ます。

「 **Onplatform Demo** 」ページでは、 `OnPlatform` マークアップ拡張機能の使用方法を示しています。

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

この例では、3つ `OnPlatform` のすべての式でクラス名の省略版が使用されて `OnPlatformExtension` います。 3つ `OnPlatform` のマークアップ拡張機能は、の [`Color`](xref:Xamarin.Forms.BoxView.Color) 、 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 、 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) の [`BoxView`](xref:Xamarin.Forms.BoxView) 各プロパティを、iOS、Android、UWP の異なる値に設定します。 また、マークアップ拡張機能は、指定されていないプラットフォームでこれらのプロパティの既定値を提供し、 `Default=` 式の一部を削除します。 設定されているマークアップ拡張プロパティは、コンマで区切られていることに注意してください。

実行中のプログラムを次に示します。

[![OnPlatform デモ](consuming-images/onplatformdemo-small.png "OnPlatform デモ")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform デモ")

## <a name="onidiom-markup-extension"></a>OnIdiom のマークアップ拡張機能

`OnIdiom`マークアップ拡張機能を使用すると、アプリケーションが実行されているデバイスの表現に基づいて UI の外観をカスタマイズできます。 これは、 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 次のプロパティを定義するクラスによってサポートされています。

- `Default`型の `object` 。デバイスの表現方法を表すプロパティに適用される既定値に設定します。
- `Phone`型の `object` 。電話に適用される値に設定します。
- `Tablet`型の `object` 。タブレットで適用される値に設定します。
- `Desktop`型の `object` 。デスクトッププラットフォームに適用される値に設定します。
- `TV`型の `object` 。 TV プラットフォームに適用される値に設定します。
- `Watch`型の `object` 。 Watch プラットフォームで適用される値に設定します。
- `Converter`型の `IValueConverter` 。実装に設定でき `IValueConverter` ます。
- `ConverterParameter`型の `object` 。実装に渡す値に設定でき `IValueConverter` ます。

> [!NOTE]
> XAML パーサーでは、 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) クラスをとして省略でき `OnIdiom` ます。

プロパティは、 `Default` の content プロパティです `OnIdiomExtension` 。 そのため、中かっこで囲まれた XAML マークアップ式では、 `Default=` 最初の引数であることを示す式の一部を削除できます。

> [!IMPORTANT]
> XAML パーサーでは、適切な型の値がマークアップ拡張機能を使用するプロパティに提供されることを想定して `OnIdiom` います。 型変換が必要な場合、 `OnIdiom` マークアップ拡張機能は、によって提供される既定のコンバーターを使用してこれを実行しようとし Xamarin.Forms ます。 ただし、既定のコンバーターでは実行できない型変換がいくつかあり、このような場合は、 `Converter` プロパティを実装に設定する必要があり `IValueConverter` ます。

**Onidiom のデモ**ページは、 `OnIdiom` マークアップ拡張機能の使用方法を示しています。

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

この例では、3つ `OnIdiom` のすべての式でクラス名の省略版が使用されて `OnIdiomExtension` います。 3つ `OnIdiom` のマークアップ拡張機能は、の [`Color`](xref:Xamarin.Forms.BoxView.Color) 、 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 、および [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) の [`BoxView`](xref:Xamarin.Forms.BoxView) 各プロパティを、電話、タブレット、およびデスクトップの各表現形式の異なる値に設定します。 また、マークアップ拡張機能は、指定されていない表現でこれらのプロパティの既定値を提供し、 `Default=` 式の一部を削除します。 設定されているマークアップ拡張プロパティは、コンマで区切られていることに注意してください。

実行中のプログラムを次に示します。

[![OnIdiom のデモ](consuming-images/onidiomdemo-small.png "OnIdiom のデモ")](consuming-images/onidiomdemo-large.png#lightbox "OnIdiom のデモ")

## <a name="datatemplate-markup-extension"></a>System.windows.datatemplate> のマークアップ拡張機能

`DataTemplate`マークアップ拡張機能を使用すると、型をに変換でき [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 これは、型のプロパティを定義するクラスによってサポートされてい `DataTemplateExtension` `TypeName` `string` ます。これは、に変換される型の名前に設定され `DataTemplate` ます。 プロパティは、 `TypeName` の content プロパティです `DataTemplateExtension` 。 そのため、中かっこで囲まれた XAML マークアップ式では、式の `TypeName=` 部分を削除できます。

> [!NOTE]
> XAML パーサーでは、`DataTemplateExtension` クラスを `DataTemplate` に短縮できます。

このマークアップ拡張機能の一般的な使用方法は、次の例に示すように、シェルアプリケーション内にあります。

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

この例で `MonkeysPage` は、がからに変換され [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。これは、プロパティの値として設定され `ShellContent.ContentTemplate` ます。 これにより `MonkeysPage` 、は、アプリケーションの起動時ではなく、ページへの移動時にのみ作成されます。

シェルアプリケーションの詳細については、「 [ Xamarin.Forms shell](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

## <a name="fontimage-markup-extension"></a>FontImage のマークアップ拡張機能

`FontImage`マークアップ拡張機能を使用すると、を表示できる任意のビューにフォントアイコンを表示でき `ImageSource` ます。 クラスと同じ機能を提供し `FontImageSource` ますが、より簡潔な表現を使用します。

`FontImage` マークアップ拡張機能は、次のプロパティを定義する `FontImageExtension` クラスによってサポートされています。

- `FontFamily`型の `string` 場合は、フォントアイコンが属するフォントファミリ。
- `Glyph`型の `string` 場合は、フォントアイコンの unicode 文字値。
- `Color`型の [`Color`](xref:Xamarin.Forms.Color) 場合は、フォントアイコンを表示するときに使用する色。
- `Size`型の `double` 。表示されるフォントアイコンのサイズ (デバイスに依存しない単位)。 既定値は 30 です。 また、このプロパティは、名前付きフォントサイズに設定できます。

> [!NOTE]
> XAML パーサーでは、`FontImageExtension` クラスを `FontImage` に短縮できます。

プロパティは、 `Glyph` の content プロパティです `FontImageExtension` 。 そのため、中かっこで囲まれた XAML マークアップ式では、 `Glyph=` 最初の引数であることを示す式の一部を削除できます。

**FontImage Demo**ページは、マークアップ拡張機能の使用方法を示してい `FontImage` ます。

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

この例では、クラス名の省略版を使用して、 `FontImageExtension` の Ionicons フォントファミリから XBox アイコンを表示し [`Image`](xref:Xamarin.Forms.Image) ます。 また、この式では、マークアップ拡張機能を使用して、 `OnPlatform` `FontFamily` IOS と Android で異なるプロパティ値を指定します。 また、 `Glyph=` 式の一部が削除され、設定されているマークアップ拡張機能プロパティはコンマで区切られます。 アイコンの unicode 文字はであるのに対し `\uf30c` 、XAML でエスケープする必要があるため、になり `&#xf30c;` ます。

実行中のプログラムを次に示します。

[![FontImage マークアップ拡張機能のスクリーンショット](consuming-images/fontimagedemo.png "FontImage のデモ")](consuming-images/fontimagedemo-large.png#lightbox "FontImage のデモ")

オブジェクトのフォントアイコンデータを指定してフォントアイコンを表示する方法の詳細について `FontImageSource` は、「[フォント](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)アイコンの表示」を参照してください。

## <a name="appthemebinding-markup-extension"></a>AppThemeBinding のマークアップ拡張機能

`AppThemeBinding`マークアップ拡張機能を使用すると、現在のシステムテーマに基づいて、イメージや色など、消費するリソースを指定できます。

> [!IMPORTANT]
> `AppThemeBinding`マークアップ拡張機能には、オペレーティングシステムの最小要件があります。 詳細については、「 [ Xamarin.Forms アプリケーションでのシステムテーマの変更への応答](~/xamarin-forms/user-interface/theming/system-theme-changes.md)」を参照してください。

`AppThemeBinding` マークアップ拡張機能は、次のプロパティを定義する `AppThemeBindingExtension` クラスによってサポートされています。

- `Default`型の `object` 。既定で使用されるリソースに設定します。
- `Light`型の。これは、 `object` デバイスが明るいテーマを使用するときに使用されるリソースに設定します。
- `Dark`型の。これは、 `object` デバイスがダークテーマを使用するときに使用されるリソースに設定します。
- `Value``object`マークアップ拡張機能によって現在使用されているリソースを返す型の。

> [!NOTE]
> XAML パーサーでは、`AppThemeBindingExtension` クラスを `AppBindingTheme` に短縮できます。

プロパティは、 `Default` の content プロパティです `AppThemeBindingExtension` 。 そのため、中かっこで囲まれた XAML マークアップ式では、 `Default=` 最初の引数であることを示す式の一部を削除できます。

**AppThemeBinding Demo**ページは、マークアップ拡張機能の使用方法を示してい `AppThemeBinding` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.AppThemeBindingDemoPage"
             Title="AppThemeBinding Demo">
    <ContentPage.Resources>

        <Style x:Key="labelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{AppThemeBinding Black, Light=Blue, Dark=Teal}" />
        </Style>

    </ContentPage.Resources>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{AppThemeBinding Light=Green, Dark=Red}" />
        <Label Text="This text is black by default, blue in light mode, and teal in dark mode."
               Style="{StaticResource labelStyle}" />
    </StackLayout>
</ContentPage>
```

この例では、デバイスが明るいテーマを使用している場合、最初のテキストの色 [`Label`](xref:Xamarin.Forms.Label) が緑色に設定されています。また、デバイスがダークテーマを使用している場合は赤に設定されます。 2番目の `Label` [`TextColor`](xref:Xamarin.Forms.Label.TextColor) プロパティは、を通じて設定さ [`Style`](xref:Xamarin.Forms.Style) れます。 これに `Style` より、のテキストの色が `Label` 既定で黒に、デバイスが明るいテーマを使用している場合は青に、デバイスがダークテーマを使用している場合は青緑に設定されます。

実行中のプログラムを次に示します。

![AppThemeBinding のデモ](consuming-images/appthemebindingdemo.png "AppThemeBinding のデモ")

## <a name="define-markup-extensions"></a>マークアップ拡張機能の定義

では使用できない XAML マークアップ拡張機能が必要な場合は、独自のマークアップ拡張機能を Xamarin.Forms [作成](creating.md)できます。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [本の XAML マークアップ拡張機能の章 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的スタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [アプリケーションのシステムテーマの変更に応答する Xamarin.Forms](~/xamarin-forms/user-interface/theming/system-theme-changes.md)
