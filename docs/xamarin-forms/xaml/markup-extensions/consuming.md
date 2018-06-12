---
title: XAML マークアップ拡張機能の使用
description: この記事では、Xamarin.Forms XAML マークアップ拡張機能を使用してさまざまなソースから設定する要素の属性を許可することで、電源と XAML の柔軟性を向上させる方法について説明します。
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 278677d45f997ac446c2a20967dc3501179bf8da
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245938"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML マークアップ拡張機能の使用

XAML マークアップ拡張機能は、さまざまなソースから設定する要素の属性を許可することで、電源と XAML の柔軟性の向上に役立ちます。 いくつかの XAML マークアップ拡張機能は、XAML 2009 の仕様の一部です。 これらに、一般的な XAML ファイルには表示`x`名前空間プレフィックス、およびは一般にこのプレフィックスを持つ。 以下のセクションに次のとおりです。

- [`x:Static`](#static) &ndash; 静的プロパティ、フィールド、または列挙型のメンバーを参照します。
- [`x:Reference`](#reference) &ndash; ページ上の要素をという名前の参照。
- [`x:Type`](#type) &ndash; 属性を設定、`System.Type`オブジェクト。
- [`x:Array`](#array) &ndash; 特定の種類のオブジェクトの配列を構築します。
- [`x:Null`](#null) &ndash; 属性を設定、`null`値。

他の 3 つの XAML マークアップ拡張機能では、その他の XAML 実装ではサポートされていましたし、Xamarin.Forms ではサポートされてもします。 これらは、他の記事で詳しく説明します。

- `StaticResource` &ndash; アーティクルの説明に従って、リソース ディクショナリからオブジェクトを参照[**リソース ディクショナリ**](~/xamarin-forms/xaml/resource-dictionaries.md)です。
- `DynamicResource` &ndash; アーティクルの説明に従って、リソース ディクショナリ内のオブジェクトの変更に応答[**動的スタイル**](~/xamarin-forms/user-interface/styles/dynamic.md)です。
- `Binding` &ndash; アーティクルの説明に従って、2 つのオブジェクトのプロパティの間のリンクを確立[**データ バインディングの**](~/xamarin-forms/app-fundamentals/data-binding/index.md)です。
- `TemplateBinding` &ndash; 記事に説明したように、コントロール テンプレートからデータのバインドを実行 **[コントロール テンプレートからバインド]**/ガイド/xamarin-フォーム/アプリケーションの基礎/テンプレート/コントロールのテンプレート/テンプレートのバインド/)

[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)レイアウト カスタム マークアップ拡張機能を利用[ `ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)です。 このマークアップ拡張機能は、資料に記載されて[ **[相対レイアウト]**](~/xamarin-forms/user-interface/layouts/relative-layout.md)です。

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static のマークアップ拡張機能

`x:Static`によってマークアップ拡張機能がサポートされている、 [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)クラスです。 クラスには、という名前の単一プロパティ[ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/)型の`string`パブリックの定数、静的プロパティ、静的フィールド、または列挙型のメンバーの名前に設定することです。

1 つの一般的な方法を使用する`x:Static`最初など一部の定数または静的変数を持つクラスを定義するのには、この小さな`AppConstants`クラス内で、 [ **Markupextension** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)プログラム。

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X:static デモ**ページを使用するいくつかの方法を示しています、`x:Static`マークアップ拡張機能です。 最も詳細なアプローチをインスタンス化、`StaticExtension`間クラス`Label.FontSize`プロパティ要素タグ。

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

XAML パーサーも許可、`StaticExtension`クラスとしての省略形を`x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

この、さらに簡略化できますが、変更、新しい構文が一部: 配置することで構成されて、`StaticExtension`クラスとメンバーの中かっこで設定します。 結果の式が設定に直接、`FontSize`属性。

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

あることに注意してください*ありません*中かっこ内の引用符。 `Member`プロパティ`StaticExtension`XML 属性ではなくなりました。 代わりに、マークアップ拡張機能の式の一部です。

省略することができます、同じ`x:StaticExtension`に`x:Static`、オブジェクト要素として使用するときにも簡略化できますが、中かっこ内の式の。

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension`クラスには、`ContentProperty`属性プロパティを参照している`Member`クラスの既定のコンテンツ プロパティとしてこのプロパティをマークします。 XAML のマークアップ拡張機能が中かっこで表される、除去することができます、`Member=`式の一部。

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

これは、最も一般的な形式の`x:Static`マークアップ拡張機能です。

**静的デモ**ページには、その他の 2 つの例が含まれています。 XAML ファイルのルート タグには、.NET 用の XML 名前空間宣言が含まれています`System`名前空間。

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

これにより、`Label`フォント サイズを静的フィールドを設定する`Math.PI`です。 比較的小さなテキストは、の結果生成されたため、`Scale`プロパティに設定されている`Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

最後の例を表示、`Device.RuntimePlatform`値。 `Environment.NewLine`静的プロパティは、2 つの間に改行文字を挿入するために使用`Span`オブジェクト。

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

`x:Reference`によってマークアップ拡張機能がサポートされている、 [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)クラスです。 クラスには、という名前の単一プロパティ[ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/)型の`string`が付いた名前が与えられているページの要素の名前に設定すること`x:Name`です。 これは、`Name`プロパティのコンテンツ プロパティは、`ReferenceExtension`ため、`Name=`は不要な場合に`x:Reference`中かっこで表示されます。

`x:Reference`専用の記事で詳しく説明されているデータ バインディングは、マークアップ拡張機能が使用される[**データ バインディングの**](~/xamarin-forms/app-fundamentals/data-binding/index.md)です。

**X:reference デモ**ページの 2 つの用途には表示`x:Reference`データ バインディングを持つ最初の設定に使用されている、`Source`のプロパティ、`Binding`オブジェクト、および設定に使用されている 2 つ目、 `BindingContext`2 つのデータ バインディングのプロパティ:

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

両方`x:Reference`式の短縮形を使用して、`ReferenceExtension`クラス名と、削除、`Name=`式の一部です。 最初の例では、`x:Reference`にマークアップ拡張機能が埋め込まれている、`Binding`マークアップ拡張機能です。 注意して、`Source`と`StringFormat`設定は、コンマで区切られます。 3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![X:reference デモ](consuming-images/referencedemo-small.png "X:reference デモ")](consuming-images/referencedemo-large.png#lightbox "X:reference デモ")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type マークアップ拡張機能

`x:Type`マークアップ拡張機能が XAML、c# の該当するショートカットは、 [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/)キーワード。 サポートされている、 [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)という名前のプロパティを定義するクラス[ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/)型の`string`クラスまたは構造体の名前に設定されています。 `x:Type`マークアップ拡張機能を返します、 [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/)そのクラスまたは構造体のオブジェクト。 `TypeName` コンテンツ プロパティは、`TypeExtension`ため、`TypeName=`は不要な場合に`x:Type`が中かっこが表示されます。

型の引数を持つ複数のプロパティがある、Xamarin.Forms 内で`Type`です。 例としては、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)プロパティ`Style`、および[X:typearguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments)ジェネリック クラスの引数を指定するための属性です。 ただし、XAML パーサーを実行、`typeof`操作自動的に、および`x:Type`マークアップ拡張機能は、このような場合は使用されません。

1 つの場所、 `x:Type` *は*では、必要な`x:Array`記載されているマークアップ拡張機能、[次のセクション](#array)です。

`x:Type`マークアップ拡張機能は、メニューが、特定の型のオブジェクトに対応する各メニュー項目を作成するときにも役立ちます。 関連付けることができます、`Type`各メニュー項目が表示されたオブジェクトし、メニュー項目が選択されているときに、オブジェクトをインスタンス化します。

これは、どのナビゲーション メニューを`MainPage`で、**マークアップ拡張機能**プログラムの動作です。 **MainPage.xaml**ファイルが含まれています、`TableView`各`TextCell`プログラムで特定のページに対応します。

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

開くメイン ページを次に示します**マークアップ拡張機能**:

[![メイン ページ](consuming-images/mainpage-small.png "メイン ページ")](consuming-images/mainpage-large.png#lightbox "メイン ページ")

各`CommandParameter`プロパティに設定されている、`x:Type`他のページの 1 つを参照するマークアップ拡張機能です。 `Command`プロパティがという名前のプロパティにバインドされる`NavigateCommand`です。 このプロパティが定義されている、`MainPage`分離コード ファイル。

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

`NavigateCommand`プロパティは、`Command`型の引数と execute コマンドを実装するオブジェクト`Type`&mdash;の値`CommandParameter`です。 メソッドを使用して`Activator.CreateInstance`ページのインスタンスを作成してに移動するとします。 コンス トラクターが終了を設定して、`BindingContext`これにより、それ自体に、ページの`Binding`で`Command`動作をします。 参照してください、 [**データ バインディングの**](~/xamarin-forms/app-fundamentals/data-binding/index.md)記事および特に[ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)この種のコードの詳細については資料です。

**X:type デモ**Xamarin.Forms 要素のインスタンスを作成してに追加するページは同様の手法を使用して、`StackLayout`です。 XAML ファイルには、3 つの最初に`Button`を持つ要素が`Command`プロパティに設定、`Binding`と`CommandParameter`Xamarin.Forms の 3 つのビューの種類に設定されているプロパティ。

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

メソッドは実行すると実行、`Button`が押された引数の新しいインスタンスを作成、設定、`VerticalOptions`プロパティに追加し、`StackLayout`です。 3 つ`Button`要素が動的に作成されたビューとし、ページを共有します。

[![X:type デモ](consuming-images/typedemo-small.png "X:type デモ")](consuming-images/typedemo-large.png#lightbox "X:type デモ")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array のマークアップ拡張機能

`x:Array`マークアップ拡張機能では、マークアップで配列を定義することができます。 サポートされている、 [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)クラスは、次の 2 つのプロパティを定義します。

- `Type` 型の`Type`配列内の要素の型を示します。
- `Items` 型の`IList`、項目自体のコレクションです。 これは、コンテンツの`ArrayExtension`します。

`x:Array`自体のマークアップ拡張機能は、中かっこで表示されません。 代わりに、`x:Array`開始タグと終了タグが項目の一覧を区切ります。 設定、`Type`プロパティを`x:Type`マークアップ拡張機能です。

**X:array デモ**ページが使用する方法を示します`x:Array`に項目を追加する、`ListView`を設定して、`ItemsSource`配列へのプロパティ。

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

`ViewCell` 、単純なが作成`BoxView`カラー エントリごとに。

[![X:array デモ](consuming-images/arraydemo-small.png "X:array デモ")](consuming-images/arraydemo-large.png#lightbox "X:array デモ")

個人を指定するいくつかの方法があります`Color`配列内の項目。 使用することができます、`x:Static`マークアップ拡張機能。

```xaml
<x:Static Member="Color.Blue" />
```

または、使用することができます`StaticResource`をリソース ディクショナリから色を取得します。

```xaml
<StaticResource Key="myColor" />
```

この記事の最後に、方向にも新しい色の値を作成するカスタム XAML マークアップ拡張が表示されます。

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

文字列や数値などの一般的な型の配列を定義するときにに一覧表示、タグを使用して、 [**コンス トラクターの引数を渡す**](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)値を区切るための記事です。

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null のマークアップ拡張機能

`x:Null`によってマークアップ拡張機能がサポートされている、 [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)クラスです。 C# XAML と同じだけをプロパティを持たず[ `null` ](/dotnet/csharp/language-reference/keywords/null/)キーワード。

`x:Null`場合はその必要性を見つけるにはできるようになりますが存在することが、マークアップ拡張機能をほとんど必要し使用頻度の低い。

**X:null デモ**ページは、1 つのシナリオを示しています。 ときに`x:Null`便利な場合があります。 暗黙的なを定義することをとします`Style`の`Label`が含まれている、`Setter`が設定された、`FontFamily`プロパティをプラットフォームに依存するファミリ名。

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

いずれかのことを検出し、`Label`要素、暗黙のすべてのプロパティ設定をする`Style`を除き、`FontFamily`既定値を指定します。 別に定義することが`Style`が、その目的、簡単な方法は単に設定する、 `FontFamily` 、特定のプロパティ`Label`に`x:Null`中央に示すように、`Label`です。

3 つのプラットフォームで実行されているプログラムを次に示します。

[![X:null デモ](consuming-images/nulldemo-small.png "X:null デモ")](consuming-images/nulldemo-large.png#lightbox "X:null デモ")

通知、4、`Label`要素は、中央がセリフ フォントをある`Label`既定 sans-serif フォントがします。

## <a name="define-your-own-markup-extensions"></a>独自のマークアップ拡張機能を定義します。

場合は、Xamarin.Forms では使用できませんの XAML マークアップ拡張機能の必要性を検出する[独自に作成](creating.md)です。


## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 帳から XAML マークアップ拡張機能の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
