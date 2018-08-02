---
title: XAML マークアップ拡張機能の使用
description: この記事では、Xamarin.Forms XAML マークアップ拡張機能を使用してさまざまなソースから設定する要素の属性を許可することで、電源と XAML の柔軟性を向上させる方法について説明します。
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: a630d7c2acb95b7551c9f5f870078a0efcfc075c
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393673"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML マークアップ拡張機能の使用

XAML マークアップ拡張機能は、さまざまなソースから設定する要素の属性を許可することで、電源と XAML の柔軟性の向上に役立ちます。 いくつかの XAML マークアップ拡張機能は、XAML 2009 の仕様の一部です。 これらをよく使用される XAML ファイルには表示`x`名前空間プレフィックス、およびは一般的に呼ばれるこのプレフィックスを持つ。 以下のセクションで以下のとおりです。

- [`x:Static`](#static) &ndash; 静的プロパティ、フィールド、または列挙型メンバーを参照します。
- [`x:Reference`](#reference) &ndash; ページ上の要素をという名前の参照。
- [`x:Type`](#type) &ndash; 属性を設定、`System.Type`オブジェクト。
- [`x:Array`](#array) &ndash; 特定の種類のオブジェクトの配列を構築します。
- [`x:Null`](#null) &ndash; 属性を設定、`null`値。

XAML マークアップ拡張機能を追加では、従来の他の XAML 実装によってサポートされていて、Xamarin.Forms でもサポートされます。 これらは、他の記事で詳しく説明されます。

- `StaticResource` &ndash; 記事の説明に従って、リソース ディクショナリからオブジェクトを参照[**リソース ディクショナリ**](~/xamarin-forms/xaml/resource-dictionaries.md)します。
- `DynamicResource` &ndash; 記事の説明に従って、リソース ディクショナリ内のオブジェクトの変更に応答[**動的なスタイル**](~/xamarin-forms/user-interface/styles/dynamic.md)します。
- `Binding` &ndash; 2 つのオブジェクトのプロパティ間のリンクを確立、記事の説明に従って[**データ バインディングの**](~/xamarin-forms/app-fundamentals/data-binding/index.md)します。
- `TemplateBinding` &ndash; この記事で説明したようにコントロール テンプレートからデータ バインディングを実行します[**からコントロール テンプレートのバインド**](/guides/xamarin-forms/application-fundamentals/templates/control-templates/template-binding/)します。

[ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout)レイアウト カスタム マークアップ拡張機能を利用[ `ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)します。 このマークアップ拡張機能が、情報の記事で説明されている[ **[相対レイアウト]**](~/xamarin-forms/user-interface/layouts/relative-layout.md)します。

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static のマークアップ拡張機能

`x:Static`によってマークアップ拡張機能がサポートされている、 [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension)クラス。 クラスがという名前の単一プロパティ[ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member)型の`string`パブリック定数、静的プロパティ、静的フィールド、または列挙型のメンバーの名前に設定することです。

使用する一般的な方法の 1 つ`x:Static`などいくつかの定数または静的変数を持つクラスを定義して、この小さな`AppConstants`クラス、 [ **Markupextension** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)プログラム。

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X:static デモ**ページを使用するいくつかの方法を示します、`x:Static`マークアップ拡張機能。 最も詳細なアプローチをインスタンス化、`StaticExtension`間クラス`Label.FontSize`プロパティ要素タグ。

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

XAML パーサーができます、`StaticExtension`のように短縮するにはクラス`x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

これさらに、簡略化することができますが、変更がいくつかの新しい構文が導入されています。 配置することで構成されます、`StaticExtension`クラスとメンバーの中かっこで囲んで設定します。 結果として得られる式の設定に直接、`FontSize`属性。

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

あることに注意してください*ありません*中かっこ内の引用符。 `Member`プロパティの`StaticExtension`XML 属性ではなくなりました。 代わりに、マークアップ拡張機能の式の一部です。

省略することができますよう`x:StaticExtension`に`x:Static`、オブジェクト要素として使用するときに省略することできますも、中かっこ内の式。

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension`クラスには、`ContentProperty`属性プロパティを参照する`Member`クラスの既定の content プロパティとしては、このプロパティがマークされます。 中かっこで表された XAML マークアップ拡張機能を除外できます、`Member=`式の一部。

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

これは、最も一般的な形式の`x:Static`マークアップ拡張機能。

**静的デモ**ページには、その他の 2 つの例が含まれています。 XAML ファイルのルート タグには、.NET 用の XML 名前空間宣言が含まれています`System`名前空間。

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

これにより、`Label`フォント サイズを静的フィールドに設定する`Math.PI`します。 比較的小さなテキストは、結果をそのため、`Scale`プロパティに設定されて`Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

最後の例が表示されます、`Device.RuntimePlatform`値。 `Environment.NewLine`静的プロパティは、2 つの間、改行文字を挿入するために使用`Span`オブジェクト。

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

3 つすべてのプラットフォームで実行されているサンプルを次に示します。

[![X:static デモ](consuming-images/staticdemo-small.png "X:static デモ")](consuming-images/staticdemo-large.png#lightbox "X:static デモ")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference のマークアップ拡張機能

`x:Reference`によってマークアップ拡張機能がサポートされている、 [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension)クラス。 クラスがという名前の単一プロパティ[ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name)型の`string`が付いた名前が与えられているページ上の要素の名前に設定すること`x:Name`します。 これは、`Name`プロパティは、コンテンツのプロパティの`ReferenceExtension`ため、`Name=`必要でない場合に`x:Reference`中かっこが表示されます。

`x:Reference`マークアップ拡張機能がデータ バインディングは、この記事で詳しく説明するのみで使用される[**データ バインディングの**](~/xamarin-forms/app-fundamentals/data-binding/index.md)します。

**X:reference デモ**ページの 2 つの使用を示しています`x:Reference`データ バインディングを持つ最初の設定に使用されている、`Source`のプロパティ、`Binding`オブジェクト、および 2 番目の設定に使用されている、 `BindingContext` 。2 つのデータ バインディングのプロパティ:

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

両方`x:Reference`式の短縮形を使用して、`ReferenceExtension`クラス名と、削除、`Name=`式の一部です。 最初の例では、`x:Reference`にマークアップ拡張機能が埋め込まれた、`Binding`マークアップ拡張機能。 なお、`Source`と`StringFormat`設定は、コンマで区切られます。 3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![X:reference デモ](consuming-images/referencedemo-small.png "X:reference デモ")](consuming-images/referencedemo-large.png#lightbox "X:reference デモ")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type マークアップ拡張機能

`x:Type`マークアップ拡張機能は、XAML と同等の c# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/)キーワード。 サポートされている、 [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension)という名前の 1 つのプロパティを定義するクラスを[ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName)型の`string`クラスまたは構造体の名前に設定されています。 `x:Type`マークアップ拡張機能を返します、 [ `System.Type` ](xref:System.Type)そのクラスまたは構造体のオブジェクト。 `TypeName` コンテンツ プロパティ`TypeExtension`ため、`TypeName=`必要でない場合に`x:Type`中かっこが表示されます。

Xamarin.Forms 内では、いくつかのプロパティ型の引数を持つ`Type`します。 例としては、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティの`Style`、および[X:typearguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments)属性をジェネリック クラスの引数を指定するために使用します。 ただし、XAML パーサーの実行、`typeof`操作自動的には、および`x:Type`マークアップ拡張機能は、このような場合は使用されません。

1 つの場所、 `x:Type` *は*では、必要な`x:Array`マークアップ拡張機能では説明されている、[次のセクション](#array)。

`x:Type`マークアップ拡張機能は、各メニュー項目が特定の種類のオブジェクトに対応するメニューを作成するときにも役立ちます。 関連付けることができます、`Type`オブジェクトの各メニュー項目とメニュー項目が選択されているときに、オブジェクトをインスタンス化します。

これは、どのナビゲーション メニューを`MainPage`で、**マークアップ拡張機能**動作をプログラムします。 **MainPage.xaml**ファイルが含まれています、`TableView`各`TextCell`プログラムで特定のページに対応します。

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

先頭のメイン ページを次に示します**マークアップ拡張機能**:

[![メイン ページ](consuming-images/mainpage-small.png "メイン ページ")](consuming-images/mainpage-large.png#lightbox "メイン ページ")

各`CommandParameter`プロパティに設定されて、`x:Type`いずれかの他のページを参照するマークアップ拡張機能。 `Command`プロパティという名前のプロパティにバインドする`NavigateCommand`します。 このプロパティが定義されている、`MainPage`分離コード ファイル。

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

`NavigateCommand`プロパティは、`Command`型の引数と execute コマンドを実装するオブジェクト`Type`&mdash;の値`CommandParameter`します。 メソッドを使用して`Activator.CreateInstance`ページをインスタンス化するに移動するとします。 コンス トラクターは、最後に設定して、`BindingContext`これにより、ページ自体への`Binding`で`Command`させる。 参照してください、 [**データ バインディングの**](~/xamarin-forms/app-fundamentals/data-binding/index.md)記事と、特に[ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)この種のコードの詳細については資料。

**X:type デモ**ページに追加して Xamarin.Forms の要素をインスタンス化する同様の手法を使用して、`StackLayout`します。 XAML ファイルには、3 つの最初に`Button`を持つ要素が`Command`プロパティに設定、`Binding`と`CommandParameter`プロパティが 3 つの Xamarin.Forms のビューの種類に設定。

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

分離コード ファイルを定義し、初期化、`CreateCommand`プロパティ。

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

メソッドはときに実行を`Button`が押された引数の新しいインスタンスを作成、設定、`VerticalOptions`プロパティに追加します、`StackLayout`します。 3 つ`Button`し、要素は動的に作成されたビューと、ページを共有します。

[![X:type デモ](consuming-images/typedemo-small.png "X:type デモ")](consuming-images/typedemo-large.png#lightbox "X:type のデモ")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array のマークアップ拡張機能

`x:Array`マークアップ拡張機能では、マークアップで配列を定義することができます。 サポートされている、 [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension)クラスは、2 つのプロパティを定義します。

- `Type` 型の`Type`配列内の要素の型を示します。
- `Items` 型の`IList`、項目自体のコレクションです。 これは、コンテンツのプロパティの`ArrayExtension`します。

`x:Array`中かっこでマークアップ拡張機能自体は表示されません。 代わりに、`x:Array`開始と終了タグが項目の一覧を区切ります。 設定、`Type`プロパティを`x:Type`マークアップ拡張機能。

**X:array デモ**ページが使用する方法を示します`x:Array`に項目を追加、`ListView`を設定して、`ItemsSource`配列へのプロパティ。

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

`ViewCell` 、単純な作成`BoxView`色エントリごとに。

[![X:array デモ](consuming-images/arraydemo-small.png "X:array デモ")](consuming-images/arraydemo-large.png#lightbox "X:array のデモ")

個人を指定するいくつかの方法がある`Color`この配列内の項目。 使用することができます、`x:Static`マークアップ拡張機能。

```xaml
<x:Static Member="Color.Blue" />
```

または、使用することができます`StaticResource`リソース ディクショナリから色を取得します。

```xaml
<StaticResource Key="myColor" />
```

この記事の最後に、方向にも、新しい色の値を作成するカスタム XAML マークアップ拡張が表示されます。

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

文字列や数値などの一般的な型の配列を定義するときに、記載タグを使用して、 [**コンス トラクターの引数を渡す**](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)値を区切るための記事。

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null のマークアップ拡張機能

`x:Null`によってマークアップ拡張機能がサポートされている、 [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension)クラス。 プロパティは持たず、c# の XAML と同じだけ[ `null` ](/dotnet/csharp/language-reference/keywords/null/)キーワード。

`x:Null`ことの必要性が見つかった場合は、ある場合が存在する、よかったが、マークアップ拡張機能をほとんど必要し、めったに使用します。

**X:null デモ**ページは、1 つのシナリオを示しています。 ときに`x:Null`便利な場合があります。 暗黙的な定義することをとします`Style`の`Label`を含む、`Setter`設定を`FontFamily`プラットフォームに依存するファミリ名のプロパティ。

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

いずれかのことがわかり、`Label`要素、暗黙ですべてのプロパティ設定する`Style`を除き、`FontFamily`既定値を指定します。 別に定義することが`Style`目的のために、簡単な方法には単に設定することです、 `FontFamily` 、特定のプロパティ`Label`に`x:Null`センターで示すように、`Label`します。

次の 3 つのプラットフォームで実行されるプログラムを次に示します。

[![X:null デモ](consuming-images/nulldemo-small.png "X:null デモ")](consuming-images/nulldemo-large.png#lightbox "X:null デモ")

その 4 つの通知、`Label`要素中心がセリフ フォントのある`Label`が既定の sans-serif フォント。

## <a name="define-your-own-markup-extensions"></a>独自のマークアップ拡張機能を定義します。

Xamarin.Forms で使用可能でない XAML マークアップ拡張機能の必要性が発生した場合は、[独自に作成](creating.md)です。


## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms book から XAML マークアップ拡張機能の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
