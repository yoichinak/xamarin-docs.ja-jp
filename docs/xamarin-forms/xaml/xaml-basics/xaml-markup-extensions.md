---
title: 第 3 部 XAML マークアップ拡張
description: XAML マークアップ拡張機能は、他のソースから間接的に参照されるオブジェクトまたは値にプロパティを設定できるようにする、XAML の重要な機能を構成します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 03/27/2018
ms.openlocfilehash: 1321f08cba4bf4cf23759ebb179a603b544ffc2e
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696647"
---
# <a name="part-3-xaml-markup-extensions"></a>第 3 部 XAML マークアップ拡張

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML マークアップ拡張機能は、他のソースから間接的に参照されるオブジェクトまたは値にプロパティを設定できるようにする、XAML の重要な機能を構成します。XAML マークアップ拡張機能は、オブジェクトを共有する場合や、アプリケーション全体で使用される定数を参照する場合に特に重要ですが、データバインディングで最も多くのユーティリティを見つけることができます。_

## <a name="xaml-markup-extensions"></a>XAML マークアップ拡張

一般に、XAML を使用して、オブジェクトのプロパティを明示的な値に設定します。たとえば、文字列、数値、列挙体のメンバー、またはバックグラウンドの背後にある値に変換される文字列などです。

ただし、場合によっては、プロパティは他の場所で定義された値を参照する必要があります。また、実行時にコードによって少し処理が必要になる場合もあります。 このため、XAML*マークアップ拡張機能*を使用できます。

これらの XAML マークアップ拡張機能は、XML の拡張機能ではありません。 XAML は完全に有効な XML です。 これらは、`IMarkupExtension` を実装するクラスのコードによってサポートされるため、"extensions" と呼ばれています。 独自のカスタムマークアップ拡張機能を作成できます。

多くの場合、XAML マークアップ拡張機能は、中かっこ {と} で区切られた属性設定として表示されるため、XAML ファイルですぐに認識できるようになりますが、マークアップ拡張機能は、通常の要素としてマークアップに表示されることがあります。

## <a name="shared-resources"></a>共有リソース

一部の XAML ページには、プロパティが同じ値に設定された複数のビューが含まれています。 たとえば、これらの `Button` オブジェクトのプロパティ設定の多くは同じです。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

これらのプロパティのいずれかを変更する必要がある場合は、3回ではなく、変更を1回だけ行うことをお勧めします。 これがコードである場合、定数と静的な読み取り専用オブジェクトを使用して、このような値の一貫性を保ち、変更しやすくすることができます。

XAML では、このような値またはオブジェクトを*リソースディクショナリ*に格納するという一般的なソリューションが1つあります。 @No__t_0 クラスは、型 `ResourceDictionary` の `Resources` という名前のプロパティを定義します。これは、型の `string` キーと `object` 型の値を持つディクショナリです。 このディクショナリにオブジェクトを格納してから、マークアップからそれらを参照することができます。

ページでリソースディクショナリを使用するには、`Resources` のプロパティ要素タグのペアを含めます。 ページの先頭に配置すると最も便利です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

@No__t_0 タグを明示的に含める必要もあります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

これで、さまざまな型のオブジェクトと値をリソースディクショナリに追加できるようになりました。 これらの型はインスタンス化可能である必要があります。 たとえば、抽象クラスにすることはできません。 これらの型には、パラメーターなしのパブリックコンストラクターも必要です。 各項目には、`x:Key` 属性で指定されたディクショナリキーが必要です。 (例:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

これらの2つの項目は `LayoutOptions` 構造体型の値であり、それぞれに一意のキーと1つまたは2つのプロパティが設定されています。 コードとマークアップでは、`LayoutOptions` の静的フィールドを使用する方が一般的ですが、ここではプロパティを設定する方が便利です。

次に、これらのボタンの `HorizontalOptions` と `VerticalOptions` のプロパティをこれらのリソースに設定する必要があります。これは、`StaticResource` XAML マークアップ拡張機能を使用して行います。

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

@No__t_0 マークアップ拡張機能は、常に中かっこで区切られ、ディクショナリキーが含まれます。

名前 `StaticResource` `DynamicResource` と区別されます。これは、Xamarin. Forms もサポートします。 `DynamicResource` は、実行時に変更される可能性がある値に関連付けられているディクショナリキーに対して、`StaticResource` は、ページ上の要素が構築されると、ディクショナリの要素に1回だけアクセスします。

@No__t_0 プロパティの場合、ディクショナリに double を格納する必要があります。 XAML では、`x:Double` や `x:Int32` などの一般的なデータ型のタグを簡単に定義できます。

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

3行に配置する必要はありません。 この回転角度のこのディクショナリエントリは、1行だけを取得します。

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

これら2つのリソースは、`LayoutOptions` 値と同じ方法で参照できます。

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

@No__t_0 型のリソースについては、これらの型の属性を直接割り当てるときに使用するのと同じ文字列表現を使用できます。 型コンバーターは、リソースの作成時に呼び出されます。 @No__t_0 型のリソースを次に示します。

```xaml
<Color x:Key="textColor">Red</Color>
```

多くの場合、プログラムは、`Large` などの `NamedSize` 列挙体のメンバーに `FontSize` プロパティを設定します。 @No__t_0 クラスはバックグラウンドで動作し、`Device.GetNamedSized` メソッドを使用してプラットフォームに依存する値に変換します。 ただし、フォントサイズリソースを定義する場合は、次に示すように、数値を使用することをお勧めします。 `x:Double` の型として表示されます。

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

これで、`Text` を除くすべてのプロパティがリソース設定によって定義されるようになりました。

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

リソースディクショナリ内で `OnPlatform` を使用して、プラットフォームのさまざまな値を定義することもできます。 さまざまなテキストの色に対して、`OnPlatform` オブジェクトをリソースディクショナリに含める方法を次に示します。

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

@No__t_0 は、ジェネリッククラスであるため、ディクショナリ内のオブジェクトであり、`x:TypeArguments` 属性であるため、`x:Key` 両方の属性を取得することに注意してください。 オブジェクトが初期化されると、`iOS`、`Android`、および `UWP` 属性が `Color` 値に変換されます。

次に、6つの共有値にアクセスする3つのボタンを持つ最終的な完全な XAML ファイルを示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:Double x:Key="fontSize">24</x:Double>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

スクリーンショットでは、一貫性のあるスタイルとプラットフォームに依存するスタイルを確認します。

[![Styled コントロール](xaml-markup-extensions-images/sharedresources.png)](xaml-markup-extensions-images/sharedresources-large.png#lightbox)

ページの上部に `Resources` コレクションを定義するのが最も一般的ですが、`Resources` プロパティは `VisualElement` によって定義されており、ページの他の要素に `Resources` コレクションを持つことができます。 たとえば、次の例では、`StackLayout` に1つを追加してみます。

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

ボタンのテキストの色が青になっていることがわかります。 基本的に、XAML パーサーは、`StaticResource` マークアップ拡張機能を検出するたびに、ビジュアルツリーを検索し、そのキーを含む最初に見つかった `ResourceDictionary` を使用します。

リソースディクショナリに格納されている最も一般的なオブジェクトの種類の1つは、プロパティ設定のコレクションを定義する Xamarin. Forms `Style` です。 スタイルについては、記事の[スタイル](~/xamarin-forms/user-interface/styles/index.md)をご覧ください。

場合によっては、XAML を初めて使用する開発者は、`Label` や `Button` などのビジュアル要素を `ResourceDictionary` に配置できるという疑問が生じることがあります。 確かに可能ですが、あまり意味がありません。 @No__t_0 の目的は、オブジェクトを共有することです。 ビジュアル要素を共有することはできません。 1つのページで同じインスタンスを2回表示することはできません。

## <a name="the-xstatic-markup-extension"></a>X:Static マークアップ拡張機能

名前の類似性にかかわらず、`x:Static` と `StaticResource` は大きく異なります。 `StaticResource` はリソースディクショナリからオブジェクトを返し、`x:Static` は次のいずれかにアクセスします。

- パブリック静的フィールド
- パブリック静的プロパティ
- パブリック定数フィールド
- 列挙体のメンバー。

@No__t_0 マークアップ拡張機能は、リソースディクショナリを定義する XAML 実装によってサポートされていますが、`x:Static` は `x` のプレフィックスによって明らかになる XAML の組み込み部分です。

次に、`x:Static` が静的フィールドと列挙型のメンバーを明示的に参照する方法を示すいくつかの例を示します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

これまでのところ、これは非常に優れていません。 ただし、`x:Static` マークアップ拡張機能は、静的フィールドまたは独自のコードからプロパティを参照することもできます。 たとえば、アプリケーション全体で複数のページで使用する静的フィールドを含む `AppConstants` クラスを次に示します。

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;

                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

XAML ファイルでこのクラスの静的フィールドを参照するには、このファイルが配置されている XAML ファイル内でを示す何らかの方法が必要です。 これを行うには、XML 名前空間宣言を使用します。

標準の Xamarin. Forms XAML テンプレートの一部として作成された XAML ファイルには、次の2つの XML 名前空間宣言が含まれています。1つは、Xamarin. Forms クラスにアクセスするためのもので、もう1つは XAML に固有のタグと属性を参照するためです。

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

他のクラスにアクセスするには、追加の XML 名前空間宣言が必要です。 追加の各 XML 名前空間宣言は、新しいプレフィックスを定義します。 @No__t_0 など、共有アプリケーション .NET Standard ライブラリに対してローカルなクラスにアクセスするには、多くの場合、XAML プログラマが `local` プレフィックスを使用します。 名前空間宣言は、CLR (共通言語ランタイム) 名前空間名を示す必要があります。これは、 C# `namespace` 定義または `using` ディレクティブに表示される名前で、.net 名前空間名とも呼ばれます。

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

また、.NET Standard ライブラリが参照する任意のアセンブリの .NET 名前空間の XML 名前空間宣言を定義することもできます。 たとえば、次に示すのは、 **mscorlib**アセンブリにある標準 .net `System` 名前空間の `sys` プレフィックスです。これは、"Microsoft Common Object runtime library" をそこした後、"多言語の標準共通オブジェクトランタイムライブラリ" を意味します。 これは別のアセンブリなので、アセンブリ名も指定する必要があります。この例では、 **mscorlib**です。

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

キーワード `clr-namespace` の後に、コロンと .NET 名前空間の名前、セミコロン、キーワード `assembly`、等号、およびアセンブリ名が続くことに注意してください。

はい。コロンは `clr-namespace` の後に続きますが、等号 (=) は `assembly` の後に続きます。 構文は、この方法で意図的に定義されています。ほとんどの XML 名前空間宣言は、`http` などの URI スキーム名を開始する URI を参照します。これには常にコロンが続きます。 この文字列の `clr-namespace` 部分は、その規則を模倣することを目的としています。

これらの名前空間宣言は、どちらも**StaticConstantsPage**サンプルに含まれています。 @No__t_0 のディメンションは `Math.PI` と `Math.E` に設定されていますが、100の係数でスケーリングされています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

画面を基準とした結果 `BoxView` のサイズは、プラットフォームに依存します。

[x:Static マークアップ拡張機能を使用した ![Controls](xaml-markup-extensions-images/staticconstants.png)](xaml-markup-extensions-images/staticconstants-large.png#lightbox)

## <a name="other-standard-markup-extensions"></a>その他の標準のマークアップ拡張機能

いくつかのマークアップ拡張機能は XAML に固有であり、Xamarin. Forms XAML ファイルでサポートされています。 これらの一部はあまり頻繁に使用されませんが、必要な場合には重要です。

- プロパティに既定で `null` 以外の値が設定されていても `null` に設定する場合は、`{x:Null}` マークアップ拡張機能に設定します。
- プロパティが `Type` 型の場合は、マークアップ拡張機能 `{x:Type someClass}` を使用して、`Type` オブジェクトに割り当てることができます。
- XAML では、`x:Array` マークアップ拡張機能を使用して、配列を定義できます。 このマークアップ拡張機能には、配列内の要素の型を示す `Type` という名前の必須の属性があります。
- @No__t_0 マークアップ拡張機能については、[パート4で説明します。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。
- @No__t_0 マークアップ拡張機能については、「[相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」を参照してください。

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression マークアップ拡張機能

マークアップ拡張機能はプロパティを持つことができますが、XML 属性のようには設定されません。 マークアップ拡張機能では、プロパティの設定はコンマで区切られ、中かっこ内に引用符は表示されません。

これは、`RelativeLayout` クラスで使用される `ConstraintExpression` という名前の Xamarin. Forms マークアップ拡張機能と共に使用できます。 子ビューの位置またはサイズを定数として指定することも、親または他の名前付きビューに対して相対的に指定することもできます。 @No__t_0 の構文を使用すると、別のビューのプロパティと `Constant` を `Factor` て、ビューの位置またはサイズを設定できます。 コードを必要とするよりも複雑なものがあります。

次に例を示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

このサンプルから実行する必要のある最も重要な教訓は、マークアップ拡張機能の構文です。マークアップ拡張機能の中かっこの中に引用符を含めないでください。 XAML ファイルにマークアップ拡張機能を入力する場合は、プロパティの値を引用符で囲むのが自然です。 このようにしてください。

次のプログラムが実行されています。

[制約を使用した ![Relative レイアウト](xaml-markup-extensions-images/relativelayout.png)](xaml-markup-extensions-images/relativelayout-large.png#lightbox)

## <a name="summary"></a>まとめ

ここに示す XAML マークアップ拡張機能は、XAML ファイルの重要なサポートを提供します。 しかし、多くの場合、最も重要な XAML マークアップ拡張機能は `Binding` です。これについては、このシリーズの第4部で説明し[ます。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [パート1。XAML を使用したはじめに](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第2部。必須の XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [パート4。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [パート5。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
