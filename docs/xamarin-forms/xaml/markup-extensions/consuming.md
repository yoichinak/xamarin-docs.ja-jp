---
title: XAML マークアップ拡張の使用
description: この記事では、さまざまなソースから要素属性を設定できるようにすることで、Xamarin の XAML マークアップ拡張機能を使用して、XAML のパワーと柔軟性を向上させる方法について説明します。
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/21/2020
ms.openlocfilehash: 7f46bc84543a1838241923ab91809bd01c706f44
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517112"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML マークアップ拡張の使用

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML マークアップ拡張機能は、さまざまなソースから要素属性を設定できるようにすることで、XAML のパワーと柔軟性を向上させるのに役立ちます。 Xaml のマークアップ拡張機能のいくつかは、XAML 2009 仕様に含まれています。 これらは、慣例`x`として名前空間プレフィックスを持つ XAML ファイルに表示され、一般的にこのプレフィックスで参照されます。 この記事では、次のマークアップ拡張機能について説明します。

- [`x:Static`](#static)–静的プロパティ、フィールド、または列挙型のメンバーを参照します。
- [`x:Reference`](#reference)–ページの名前付き要素を参照します。
- [`x:Type`](#type)–属性を`System.Type`オブジェクトに設定します。
- [`x:Array`](#array)–特定の型のオブジェクトの配列を構築します。
- [`x:Null`](#null)–属性を`null`値に設定します。
- [`OnPlatform`](#onplatform)–プラットフォームごとに UI の外観をカスタマイズします。
- [`OnIdiom`](#onidiom)–アプリケーションが実行されているデバイスの表現に基づいて UI の外観をカスタマイズします。
- [`DataTemplate`](#datatemplate-markup-extension)–型をに変換[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。
- [`FontImage`](#fontimage-markup-extension)–を表示できる任意のビューにフォントアイコンを表示`ImageSource`します。
- [`OnAppTheme`](#onapptheme-markup-extension)–現在のシステムテーマに基づいてリソースを使用します。

追加の XAML マークアップ拡張機能は、従来は他の XAML 実装によってサポートされており、Xamarin. Forms でもサポートされています。 これらの詳細については、他の記事で詳しく説明します。

- `StaticResource`-リソースディクショナリからオブジェクトを参照します。詳細については、「[**リソースディクショナリ**](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。
- `DynamicResource`-リソースディクショナリ内のオブジェクトの変更に応答します。詳細については、「[**動的スタイル**](~/xamarin-forms/user-interface/styles/dynamic.md)」を参照してください。
- `Binding`-「[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)」で説明されているように、2つのオブジェクトのプロパティ間のリンクを確立します。
- `TemplateBinding`-「 [**Xamarin. フォームコントロールテンプレート**](~/xamarin-forms/app-fundamentals/templates/control-template.md)」で説明されているように、コントロールテンプレートからデータバインディングを実行します。
- `RelativeSource`-バインドソースを、バインディングターゲットの位置に対して相対的に設定します。これについては、「[相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」を参照してください。

レイアウト[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)では、カスタムマークアップ拡張機能[`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)を使用します。 このマークアップ拡張機能については、記事[**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)を参照してください。

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static マークアップ拡張機能

`x:Static`マークアップ拡張機能は[`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)クラスによってサポートされています。 クラスには、パブリック定数、 [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member)静的プロパティ`string` 、静的フィールド、または列挙型のメンバーの名前に設定する、type という名前の単一のプロパティがあります。

を使用する一般的な`x:Static`方法の1つとして、最初に定数または静的変数を含む`AppConstants`クラスを定義します。たとえば、[**マークアップ拡張**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)プログラムでは、この小さなクラスを使用します。

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

[ **X:Static Demo** ] ページでは、 `x:Static`マークアップ拡張機能を使用するいくつかの方法を示します。 最も詳細な方法では`StaticExtension` 、プロパティ`Label.FontSize`要素タグ間でクラスをインスタンス化します。

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

また、XAML パーサーでは`StaticExtension` 、クラスを次の`x:Static`ように省略できます。

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

これはさらに簡略化できますが、変更には新しい構文がいくつか導入され`StaticExtension`ています。これは、クラスとメンバーの設定を中かっこで囲むことで構成されます。 結果として得られる式`FontSize`は、属性に直接設定されます。

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

中かっこ内に引用符が*ない*ことに注意してください。 の`Member` `StaticExtension`プロパティが XML 属性ではなくなりました。 代わりに、マークアップ拡張機能の式の一部になります。

オブジェクト要素として`x:StaticExtension`使用`x:Static`するときに、省略可能であるように、中かっこ内の式で省略することもできます。

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

クラス`StaticExtension`には、 `ContentProperty`プロパティ`Member`を参照する属性があります。これは、このプロパティをクラスの既定のコンテンツプロパティとしてマークします。 中かっこで囲まれ`Member=`た XAML マークアップ拡張機能では、式の一部を削除できます。

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

これは、 `x:Static`マークアップ拡張機能の最も一般的な形式です。

**静的なデモ**ページには、他の2つの例が含まれています。 XAML ファイルのルートタグには、.NET `System`名前空間の XML 名前空間宣言が含まれています。

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

これにより`Label` 、フォントサイズを静的フィールド`Math.PI`に設定できます。 その結果、テキストは小さくなり、 `Scale`プロパティは次のよう`Math.E`に設定されます。

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

最後の例では`Device.RuntimePlatform` 、値が表示されます。 `Environment.NewLine`静的プロパティは、2つ`Span`のオブジェクトの間に改行文字を挿入するために使用されます。

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

`x:Reference`マークアップ拡張機能は[`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)クラスによってサポートされています。 クラスには、という名前[`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name)の単一`string`のプロパティがあります。このプロパティには、という名前が付け`x:Name`られたページ上の要素の名前を設定します。 この`Name`プロパティはの`ReferenceExtension`コンテンツプロパティであるため`Name=` 、中かっこで`x:Reference`囲まれている場合は必要ありません。

`x:Reference`マークアップ拡張機能は、データバインディングで排他的に使用されます。詳細については、「[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

**X:Reference Demo**ページでは、データバインディング`x:Reference`を使用したの2つの使用方法が示され`Source`ています`Binding` 。最初に、オブジェクトのプロパティを設定するために`BindingContext`使用します。2つ目は、2つのデータバインディングのプロパティを設定するために使用されます。

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

どちら`x:Reference`の式でも、 `ReferenceExtension`クラス名の省略版が使用`Name=`され、式の一部が削除されます。 最初の例では、 `x:Reference`マークアップ拡張機能が`Binding`マークアップ拡張機能に埋め込まれています。 と`Source` `StringFormat`の設定はコンマで区切られていることに注意してください。 実行中のプログラムを次に示します。

[![x:Reference のデモ](consuming-images/referencedemo-small.png "x:Reference のデモ")](consuming-images/referencedemo-large.png#lightbox "x:Reference のデモ")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type マークアップ拡張機能

`x:Type`マークアップ拡張機能は、C# [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/)キーワードに相当する XAML です。 クラスによってサポート[`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)されています。この[`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName)クラスは`string` 、クラスまたは構造体の名前に設定されている型の1つのプロパティを定義します。 マーク`x:Type`アップ拡張機能は[`System.Type`](xref:System.Type) 、そのクラスまたは構造体のオブジェクトを返します。 `TypeName`はの`TypeExtension`content プロパティであるため`TypeName=` 、中かっこで`x:Type`囲まれている場合は必要ありません。

Xamarin. Forms 内には、型`Type`の引数を持つプロパティがいくつかあります。 たとえば、の[`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style`プロパティや、ジェネリッククラスの引数を指定するために使用される[x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments)属性などがあります。 ただし、XAML パーサーはこの`typeof`操作を自動的に実行し`x:Type`ます。この場合、マークアップ拡張機能は使用されません。

`x:Type` *が*必要な場所の1つは`x:Array` 、マークアップ拡張機能を使用することです。これについては、[次のセクション](#array)で説明します。

マーク`x:Type`アップ拡張機能は、各メニュー項目が特定の型のオブジェクトに対応するメニューを構築する場合にも便利です。 オブジェクトを`Type`各メニュー項目に関連付けて、メニュー項目を選択したときにオブジェクトをインスタンス化することができます。

これは、 `MainPage` **マークアップ拡張機能**プログラムののナビゲーションメニューの動作です。 **Mainpage.xaml**ファイルには、 `TableView`プログラム内の特定`TextCell`のページに対応するそれぞれのが含まれています。

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

各`CommandParameter`プロパティは、他の`x:Type`ページの1つを参照するマークアップ拡張機能に設定されます。 `Command`プロパティは、という名前`NavigateCommand`のプロパティにバインドされます。 このプロパティは、 `MainPage`分離コードファイルで定義されています。

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

`NavigateCommand`プロパティは、の`Command` `Type` &mdash; `CommandParameter`値を型とする引数を指定して execute コマンドを実装するオブジェクトです。 このメソッドは`Activator.CreateInstance` 、を使用してページをインスタンス化し、そのページに移動します。 コンストラクターは、ページ`BindingContext`のをそれ自体に設定することによって`Binding`終了`Command`します。これにより、が動作するようになります。 この種類のコードの詳細については、[**データバインディング**](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関する記事と、特に[**コマンド**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)の使用に関する記事を参照してください。

**X:Type Demo**ページでは、同様の手法を使用して、Xamarin の要素をインスタンス化`StackLayout`し、それをに追加します。 XAML `Button`ファイルは、 `Command` `Binding`最初は3つの要素で構成され、プロパティ`CommandParameter`はに設定され、プロパティは3つの Xamarin ビューの種類に設定されます。

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

分離コードファイルでは、 `CreateCommand`プロパティを定義し、初期化します。

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

`Button`が押されたときに実行されるメソッドは、引数の新しいインスタンスを作成し`VerticalOptions` 、そのプロパティを設定して`StackLayout`、に追加します。 次に`Button` 、3つの要素が、動的に作成されたビューとページを共有します。

[![x:Type のデモ](consuming-images/typedemo-small.png "x:Type のデモ")](consuming-images/typedemo-large.png#lightbox "x:Type のデモ")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array のマークアップ拡張機能

マーク`x:Array`アップ拡張機能を使用すると、マークアップで配列を定義できます。 これは、次の[`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) 2 つのプロパティを定義するクラスによってサポートされています。

- `Type`型`Type`の。配列内の要素の型を示します。
- `Items`型`IList`の。これは、項目自体のコレクションです。 これは、の`ArrayExtension`content プロパティです。

マーク`x:Array`アップ拡張機能自体は、中かっこで囲まれていません。 代わりに、 `x:Array`開始タグと終了タグによって項目のリストが区切られます。 `Type`プロパティを`x:Type`マークアップ拡張機能に設定します。

**X:Array Demo**ページでは、を使用`x:Array`して、 `ListView` `ItemsSource`プロパティを配列に設定することによってに項目を追加する方法を示しています。

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

は`ViewCell` 、各カラー `BoxView`エントリに対して単純なを作成します。

[![x:Array のデモ](consuming-images/arraydemo-small.png "x:Array のデモ")](consuming-images/arraydemo-large.png#lightbox "x:Array のデモ")

この配列内の個々`Color`の項目を指定するには、いくつかの方法があります。 `x:Static`マークアップ拡張機能を使用できます。

```xaml
<x:Static Member="Color.Blue" />
```

または、を使用`StaticResource`して、リソースディクショナリから色を取得することもできます。

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

`x:Null`マークアップ拡張機能は[`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)クラスによってサポートされています。 これにはプロパティはなく、単純に C# [`null`](/dotnet/csharp/language-reference/keywords/null/)のキーワードに相当する XAML です。

`x:Null`マークアップ拡張機能はほとんど必要ありませんが、ほとんど使用されていませんが、必要な場合は、存在していることを確認してください。

**X:Null Demo**ページには、便利な`x:Null`場合があるシナリオが示されています。 プロパティをプラットフォームに依存する`Style`ファミリ`Label`名に設定`Setter`するを含むの暗黙的なを定義するとします。 `FontFamily`

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

次に、いずれかの`Label`要素について、既定値として使用するを`Style`除き`FontFamily`、暗黙的なのすべてのプロパティ設定を使用することを確認します。 この目的に対し`Style`て別のを定義することもできますが、 `FontFamily`より簡単な方法`Label`は`x:Null`、中心`Label`に示すように、特定ののプロパティをに設定することです。

実行中のプログラムを次に示します。

[![x:Null のデモ](consuming-images/nulldemo-small.png "x:Null のデモ")](consuming-images/nulldemo-large.png#lightbox "x:Null のデモ")

4つの`Label`要素にはセリフフォントがありますが、中央`Label`には既定の sans serif フォントがあります。

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform マークアップ拡張機能

`OnPlatform` マークアップ拡張では、プラットフォームごとに UI の外観をカスタマイズできます。 クラス[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)および[`On`](xref:Xamarin.Forms.On)クラスと同じ機能を提供しますが、より簡潔な表現を使用します。

`OnPlatform`マークアップ拡張機能は、次[`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension)のプロパティを定義するクラスによってサポートされています。

- `Default`型`object`の。プラットフォームを表すプロパティに適用される既定値に設定します。
- `Android`型`object`の。 Android で適用される値に設定します。
- `GTK`型`object`の。 GTK プラットフォームに適用される値に設定します。
- `iOS`型`object`の。 iOS で適用される値に設定します。
- `macOS`型`object`の。 macOS に適用される値に設定します。
- `Tizen`型`object`の。 Tizen プラットフォームで適用される値に設定します。
- `UWP`型`object`の。ユニバーサル Windows プラットフォームに適用される値に設定します。
- `WPF`型`object`の。 Windows Presentation Foundation プラットフォームに適用される値に設定します。
- `Converter`型`IValueConverter`の。 `IValueConverter`実装に設定できます。
- `ConverterParameter`型`object`の。 `IValueConverter`実装に渡す値に設定できます。

> [!NOTE]
> XAML パーサーでは、 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension)クラスをとして`OnPlatform`省略できます。

`Default`プロパティは、の`OnPlatformExtension`content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では`Default=` 、最初の引数であることを示す式の一部を削除できます。 プロパティが`Default`設定されていない場合、マーク[`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue)アップ拡張機能がを[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)対象としている場合は、プロパティの値が既定値になります。

> [!IMPORTANT]
> XAML パーサーでは、適切な型の値が`OnPlatform`マークアップ拡張機能を使用するプロパティに提供されることを想定しています。 型変換が必要な場合、 `OnPlatform`マークアップ拡張機能は、Xamarin. Forms によって提供される既定のコンバーターを使用してこれを実行しようとします。 ただし、既定のコンバーターでは実行できない型変換がいくつかあり、このような`Converter`場合は、プロパティを`IValueConverter`実装に設定する必要があります。

「 **Onplatform Demo** 」ページでは、マーク`OnPlatform`アップ拡張機能の使用方法を示しています。

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

この例では、3 `OnPlatform`つのすべての式で`OnPlatformExtension`クラス名の省略版が使用されています。 3つ`OnPlatform`のマークアップ拡張[`Color`](xref:Xamarin.Forms.BoxView.Color)機能[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)は、 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)の、、 [`BoxView`](xref:Xamarin.Forms.BoxView)の各プロパティを、iOS、Android、UWP の異なる値に設定します。 また、マークアップ拡張機能は、指定されていないプラットフォームでこれらのプロパティの`Default=`既定値を提供し、式の一部を削除します。 設定されているマークアップ拡張プロパティは、コンマで区切られていることに注意してください。

実行中のプログラムを次に示します。

[![OnPlatform デモ](consuming-images/onplatformdemo-small.png "OnPlatform デモ")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform デモ")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom のマークアップ拡張機能

`OnIdiom`マークアップ拡張機能を使用すると、アプリケーションが実行されているデバイスの表現に基づいて UI の外観をカスタマイズできます。 これは、次の[`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension)プロパティを定義するクラスによってサポートされています。

- `Default`型`object`の。デバイスの表現方法を表すプロパティに適用される既定値に設定します。
- `Phone`型`object`の。電話に適用される値に設定します。
- `Tablet`型`object`の。タブレットで適用される値に設定します。
- `Desktop`型`object`の。デスクトッププラットフォームに適用される値に設定します。
- `TV`型`object`の。 TV プラットフォームに適用される値に設定します。
- `Watch`型`object`の。 Watch プラットフォームで適用される値に設定します。
- `Converter`型`IValueConverter`の。 `IValueConverter`実装に設定できます。
- `ConverterParameter`型`object`の。 `IValueConverter`実装に渡す値に設定できます。

> [!NOTE]
> XAML パーサーでは、 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension)クラスをとして`OnIdiom`省略できます。

`Default`プロパティは、の`OnIdiomExtension`content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では`Default=` 、最初の引数であることを示す式の一部を削除できます。

> [!IMPORTANT]
> XAML パーサーでは、適切な型の値が`OnIdiom`マークアップ拡張機能を使用するプロパティに提供されることを想定しています。 型変換が必要な場合、 `OnIdiom`マークアップ拡張機能は、Xamarin. Forms によって提供される既定のコンバーターを使用してこれを実行しようとします。 ただし、既定のコンバーターでは実行できない型変換がいくつかあり、このような`Converter`場合は、プロパティを`IValueConverter`実装に設定する必要があります。

**Onidiom のデモ**ページは、マークアップ拡張`OnIdiom`機能の使用方法を示しています。

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

この例では、3 `OnIdiom`つのすべての式で`OnIdiomExtension`クラス名の省略版が使用されています。 3つ`OnIdiom`のマークアップ拡張[`Color`](xref:Xamarin.Forms.BoxView.Color)機能[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)は、 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)の、、 [`BoxView`](xref:Xamarin.Forms.BoxView)およびの各プロパティを、電話、タブレット、およびデスクトップの各表現形式の異なる値に設定します。 また、マークアップ拡張機能は、指定されていない表現でこれらのプロパティの`Default=`既定値を提供し、式の一部を削除します。 設定されているマークアップ拡張プロパティは、コンマで区切られていることに注意してください。

実行中のプログラムを次に示します。

[![OnIdiom のデモ](consuming-images/onidiomdemo-small.png "OnIdiom のデモ")](consuming-images/onidiomdemo-large.png#lightbox "OnIdiom のデモ")

## <a name="datatemplate-markup-extension"></a>System.windows.datatemplate> のマークアップ拡張機能

`DataTemplate`マークアップ拡張機能を使用すると、型を[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に変換できます。 これ`DataTemplateExtension`は、型`TypeName` `string`のプロパティを定義するクラスによってサポートされています。これは、 `DataTemplate`に変換される型の名前に設定されます。 `TypeName`プロパティは、の`DataTemplateExtension`content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では、式の `TypeName=` 部分を削除できます。

> [!NOTE]
> XAML パーサーでは、`DataTemplateExtension` クラスを `DataTemplate` に短縮できます。

このマークアップ拡張機能の一般的な使用方法は、次の例に示すように、シェルアプリケーション内にあります。

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

この例では`MonkeysPage` 、がから[`ContentPage`](xref:Xamarin.Forms.ContentPage)に[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)変換されます。これは、 `ShellContent.ContentTemplate`プロパティの値として設定されます。 これにより`MonkeysPage` 、は、アプリケーションの起動時ではなく、ページへの移動時にのみ作成されます。

シェルアプリケーションの詳細については、「 [Xamarin. フォームシェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

## <a name="fontimage-markup-extension"></a>FontImage のマークアップ拡張機能

`FontImage`マークアップ拡張機能を使用すると、を表示できる任意のビューにフォント`ImageSource`アイコンを表示できます。 `FontImageSource`クラスと同じ機能を提供しますが、より簡潔な表現を使用します。

`FontImage` マークアップ拡張機能は、次のプロパティを定義する `FontImageExtension` クラスによってサポートされています。

- `FontFamily`型`string`の場合は、フォントアイコンが属するフォントファミリ。
- `Glyph`型`string`の場合は、フォントアイコンの unicode 文字値。
- `Color`型[`Color`](xref:Xamarin.Forms.Color)の場合は、フォントアイコンを表示するときに使用する色。
- `Size`型`double`の。表示されるフォントアイコンのサイズ (デバイスに依存しない単位)。 既定値は 30 です。 また、このプロパティは、名前付きフォントサイズに設定できます。

> [!NOTE]
> XAML パーサーでは、`FontImageExtension` クラスを `FontImage` に短縮できます。

`Glyph`プロパティは、の`FontImageExtension`content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では`Glyph=` 、最初の引数であることを示す式の一部を削除できます。

**FontImage Demo**ページは、マークアップ拡張機能`FontImage`の使用方法を示しています。

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

この例では、 `FontImageExtension`クラス名の省略版を使用して、の Ionicons フォントファミリから XBox アイコンを表示し[`Image`](xref:Xamarin.Forms.Image)ます。 また、この式で`OnPlatform`は、マークアップ拡張`FontFamily`機能を使用して、iOS と Android で異なるプロパティ値を指定します。 また、式の`Glyph=`一部が削除され、設定されているマークアップ拡張機能プロパティはコンマで区切られます。 アイコンの unicode 文字は`\uf30c`であるのに対し、XAML でエスケープする必要があるため、に`&#xf30c;`なります。

実行中のプログラムを次に示します。

[![FontImage マークアップ拡張機能のスクリーンショット](consuming-images/fontimagedemo.png "FontImage のデモ")](consuming-images/fontimagedemo-large.png#lightbox "FontImage のデモ")

オブジェクトのフォントアイコンデータを指定してフォントアイコンを表示する方法の詳細については、「フォントアイコンの表示」を参照してください。 [Display font icons](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) `FontImageSource`

## <a name="onapptheme-markup-extension"></a>OnAppTheme マークアップ拡張機能

`OnAppTheme`マークアップ拡張機能を使用すると、現在のシステムテーマに基づいて、イメージや色など、消費するリソースを指定できます。 `OnAppTheme<T>`クラスと同じ機能を提供しますが、より簡潔な表現を使用します。

> [!IMPORTANT]
> マーク`OnAppTheme`アップ拡張機能には、オペレーティングシステムの最小要件があります。 詳細については、「 [Xamarin. フォームアプリケーションでのシステムテーマの変更への応答](~/xamarin-forms/user-interface/theming/system-theme-changes.md)」を参照してください。

`OnAppTheme` マークアップ拡張機能は、次のプロパティを定義する `OnAppThemeExtension` クラスによってサポートされています。

- `Default`型`object`の。既定で使用されるリソースに設定します。
- `Light`型`object`の。これは、デバイスが明るいテーマを使用するときに使用されるリソースに設定します。
- `Dark`型`object`の。これは、デバイスがダークテーマを使用するときに使用されるリソースに設定します。
- `Value`マークアップ拡張`object`機能によって現在使用されているリソースを返す型の。
- `Converter`型`IValueConverter`の。 `IValueConverter`実装に設定できます。
- `ConverterParameter`型`object`の。 `IValueConverter`実装に渡す値に設定できます。

> [!NOTE]
> XAML パーサーでは、`OnAppThemeExtension` クラスを `OnAppTheme` に短縮できます。

`Default`プロパティは、の`OnAppThemeExtension`content プロパティです。 そのため、中かっこで囲まれた XAML マークアップ式では`Default=` 、最初の引数であることを示す式の一部を削除できます。

**Onapptheme デモ**ページは、マークアップ拡張機能`OnAppTheme`の使用方法を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.OnAppThemeDemoPage"
             Title="OnAppTheme Demo">
    <ContentPage.Resources>

        <Style x:Key="labelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{OnAppTheme Black, Light=Blue, Dark=Teal}" />
        </Style>

    </ContentPage.Resources>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Label Text="This text is black by default, blue in light mode, and teal in dark mode."
               Style="{DynamicResource labelStyle}" />
    </StackLayout>
</ContentPage>
```

この例では、デバイスが明るいテーマを[`Label`](xref:Xamarin.Forms.Label)使用している場合、最初のテキストの色が緑色に設定されています。また、デバイスがダークテーマを使用している場合は赤に設定されます。 2番`Label`目の[`TextColor`](xref:Xamarin.Forms.Label.TextColor)プロパティは、を[`Style`](xref:Xamarin.Forms.Style)通じて設定されます。 これ`Style`により、 `Label`のテキストの色が既定で黒に、デバイスが明るいテーマを使用している場合は青に、デバイスがダークテーマを使用している場合は青緑に設定されます。

> [!NOTE]
> マークアップ拡張機能を使用するは[`Style`](xref:Xamarin.Forms.Style) 、 `DynamicResource`マークアップ拡張機能を持つコントロールに適用する必要があります。これにより、システムテーマの変更時にアプリの UI が更新されます。 `OnAppTheme`

実行中のプログラムを次に示します。

![OnAppTheme デモ](consuming-images/onappthemedemo.png "OnAppTheme デモ")

## <a name="define-markup-extensions"></a>マークアップ拡張機能の定義

Xamarin. Forms では使用できない XAML マークアップ拡張機能が必要な場合は、[独自に作成](creating.md)することができます。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Xamarin. Forms book の章の XAML マークアップ拡張機能](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin. Forms アプリケーションでのシステムテーマの変更に応答する](~/xamarin-forms/user-interface/theming/system-theme-changes.md)
