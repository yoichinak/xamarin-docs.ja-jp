---
title: パート 3 です。 XAML マークアップ拡張機能
description: XAML マークアップ拡張機能は、オブジェクトやその他のソースから直接参照されている値を設定するプロパティを許可する XAML で重要な機能を構成します。 XAML マークアップ拡張機能は、オブジェクトを共有し、アプリケーション全体で使用される定数を参照してにとって特に重要ですが、データ バインドで、最大のユーティリティを見ることができます。
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: cd881b79945c2b9c10e9bb1bc85fce98acb71026
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="part-3-xaml-markup-extensions"></a>パート 3 です。 XAML マークアップ拡張機能

_XAML マークアップ拡張機能は、オブジェクトやその他のソースから直接参照されている値を設定するプロパティを許可する XAML で重要な機能を構成します。XAML マークアップ拡張機能は、オブジェクトを共有し、アプリケーション全体で使用される定数を参照してにとって特に重要ですが、データ バインドで、最大のユーティリティを見ることができます。_

## <a name="xaml-markup-extensions"></a>XAML マークアップ拡張機能

一般に、XAML を使用して、文字列、数値、列挙体のメンバー、またはバック グラウンドでの値に変換される文字列などの明示的な値にオブジェクトのプロパティを設定します。

場合によっては、ただし、プロパティはそれ以外の場合、どこかに定義された値を代わりに参照する必要があります。 または実行時にコードでほとんど処理が必要な場合があります。 このため、XAML*マークアップ拡張機能*を利用できます。

これらの XAML マークアップ拡張機能は、XML の拡張機能ではありません。 XAML は、完全に有効な XML です。 呼ばれる"extensions"を実装するクラスのコードでのバックアップがあるため`IMarkupExtension`です。 独自のカスタム マークアップ拡張機能を記述することができます。

多くの場合、XAML マークアップ拡張機能は XAML ファイル内で即座に認識できるため、中かっこで区切られた属性の設定として表示されます: {、} が従来型の要素としてマークアップにマークアップ拡張機能の表示場合があります。

## <a name="shared-resources"></a>共有リソース

一部の XAML ページには、同じ値に設定されたプロパティを持つ複数のビューが含まれます。 たとえば、多くのこれらのプロパティ設定`Button`オブジェクトが等しい。

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

変更するこれらのプロパティのいずれかの場合は、3 回ではなく、1 回だけ変更を行いたい場合があります。 これがコードの場合、するも可能性がありますを使用定数と静的な読み取り専用のオブジェクトに一貫性があり簡単に変更してそのような値を保つためです。

XAML では、1 つの一般的なソリューションは、このような値を格納するオブジェクトまたはで、*リソース ディクショナリ*です。 `VisualElement`クラスという名前のプロパティを定義する`Resources`型の`ResourceDictionary`、これは、型のキーを持つディクショナリ`string`と型の値`object`です。 XAML で、すべてのマークアップからそれらを参照して、このディクショナリにオブジェクトを配置することができます。

をページ上のリソース ディクショナリを使用するのには、1 組の`Resources`プロパティ要素タグ。 これらのページの上部に配置すると便利です。

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

明示的に指定する必要も`ResourceDictionary`タグ。

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

これでオブジェクトとさまざまな種類の値は、リソース ディクショナリに追加できます。 これらの型をインスタンス化可能にする必要があります。 たとえば、抽象クラスにできません。 これらの型は、パブリック パラメーターなしのコンス トラクターも必要です。 各項目で指定された辞書のキーが必要です、`x:Key`属性。 例えば:

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

これら 2 つの項目は構造体型の値は、`LayoutOptions`とには、一意のキーと 1 つまたは 2 つのプロパティを設定します。 コードとマークアップではの静的フィールドを使用して一般的な`LayoutOptions`、ここでは、プロパティを設定する方が便利ですが。

設定する必要があるので、`HorizontalOptions`と`VerticalOptions`これらのリソースでは、これらのボタンのプロパティが終了して、 `StaticResource` XAML マークアップ拡張機能。

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource`マークアップ拡張機能は、常に、中かっこで区切られます、ディクショナリのキーが含まれています。

名前`StaticResource`から区別`DynamicResource`Xamarin.Forms もサポートします。 `DynamicResource` 実行時に変わる可能性がある値に関連付けられた辞書キーは、中に`StaticResource`ページ上の要素を構築するときは 1 回で、ディクショナリから要素にアクセスします。

`BorderWidth`プロパティは、double 型の値をディクショナリに格納するために必要です。 XAML のような一般的なデータ型のタグを容易を定義`x:Double`と`x:Int32`:

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

次の 3 つの行に配置する必要はありません。 この回転角度の場合は、このディクショナリ エントリは、1 行上のみ受け取る。

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

これら 2 つのリソースと同じ方法で参照できる、`LayoutOptions`値。

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

種類のリソースに対する`Color`、これらの型の属性を直接割り当てるときに使用する同じ文字列形式を使用することができます。 リソースの作成時に、型コンバーターが呼び出されます。 型のリソースを次に示します`Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

多くの場合、プログラムのセット、`FontSize`プロパティのメンバーを`NamedSize`など列挙`Large`です。 `FontSizeConverter`クラスを使用して、プラットフォームに依存する値に変換するシーンの背後にある works、`Device.GetNamedSized`メソッドです。 ただし、フォント サイズのリソースを定義するときにほうが効果的に示すように、数値の値を使用してを図って、ここに`x:Double`型。

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

これですべてのプロパティを除く`Text`リソースの設定によって定義されます。

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

使用することも`OnPlatform`プラットフォーム用の別の値を定義するリソース ディクショナリ内で。 ここでは、方法、`OnPlatform`オブジェクトが別のテキストの色をリソース ディクショナリの一部にすることができます。

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

注意して`OnPlatform`両方を取得、`x:Key`属性ディクショナリ内のオブジェクトは、および`x:TypeArguments`属性のジェネリック クラスであるためです。 `iOS`、 `Android`、および`UWP`属性に変換されます`Color`値は、オブジェクトが初期化されたときにします。

共有 6 つの値にアクセスする次の 3 つのボタンを持つ最後の完全な XAML ファイルを次に示します。

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

            <x:String x:Key="fontSize">Large</x:String>
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

スクリーン ショットでは、一貫したスタイル、およびプラットフォームに依存するスタイル設定を確認します。

[![](xaml-markup-extensions-images/sharedresources.png "スタイルのコントロールを")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "スタイルのコントロール")

定義する最も一般的な`Resources`点に注意して、ページの上部にあるコレクションを`Resources`によってプロパティが定義されている`VisualElement`、持つことができます`Resources`ページ上の他の要素のコレクション。 たとえば、を 1 つを追加してみてください、`StackLayout`この例では。

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

ボタンのテキストの色が青になっているを紹介しています。 基本的には、ときに、XAML パーサーが検出した、`StaticResource`マークアップ拡張機能、ビジュアル ツリーを検索し、1 つを使用して、`ResourceDictionary`そのキーを含むを検出します。

リソース ディクショナリに格納されているオブジェクトの最も一般的な種類の 1 つは、Xamarin.Forms`Style`プロパティの設定のコレクションを定義します。 スタイルは、記事で説明した[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。

XAML に慣れていない開発者疑問を視覚的要素をなどにすることができるかどうか`Label`または`Button`で、`ResourceDictionary`です。 確実に考えられますが、その無意味です。 目的、`ResourceDictionary`はオブジェクトを共有します。 視覚的要素を共有することはできません。 同じインスタンスは、1 ページに 2 回表示ことはできません。

## <a name="the-xstatic-markup-extension"></a>X:static マークアップ拡張機能

その名前の類似`x:Static`と`StaticResource`は大きく異なります。 `StaticResource` 中に、リソース ディクショナリからオブジェクトを返します`x:Static`次のいずれかにアクセスします。

- パブリックな静的フィールド
- パブリック静的プロパティ
- パブリック定数フィールド 
- 列挙体のメンバーです。 

`StaticResource`マークアップ拡張機能は、リソース ディクショナリを定義する XAML の実装でサポートされて中に`x:Static`として XAML での組み込みの一部である、`x`プレフィックスが表示されます。

ここでは、具体的に示す例をいくつか方法`x:Static`静的フィールドおよび列挙型メンバーを明示的に参照できます。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

これまでに非常に優れたではありません。 `x:Static`マークアップ拡張機能も参照できます静的フィールドまたはプロパティ、独自のコードから。 たとえば、ここでは、`AppConstants`アプリケーション全体で複数のページを使用するいくつかの静的フィールドを含むクラス。

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

XAML ファイルで、このクラスの静的フィールドを参照するには、このファイルが配置されている XAML ファイル内を示すために何らかの方法を必要があります。 XML 宣言付きの名前空間には、これを行います。

標準 Xamarin.Forms XAML テンプレートの一部として作成された XAML ファイルが 2 つの XML 名前空間宣言を含めることに注意してください: Xamarin.Forms クラスと別のタグおよび属性を XAML に固有の参照へのアクセスのいずれか。

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

その他のクラスにアクセスする XML 名前空間宣言を追加する必要があります。 各追加の XML 名前空間宣言では、新しいプレフィックスを定義します。 クラスにアクセスするために PCL を共有アプリケーションをローカルなど`AppConstants`、XAML のプログラマが多くの場合、プレフィックスを使用して`local`です。 名前空間の宣言は、CLR (共通言語ランタイム) の名前空間の名前とも呼ばれる .NET 名前空間の名前、これは、C# の場合に表示される名前を示す必要があります`namespace`定義または、`using`ディレクティブ。

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

PCL を参照する任意のアセンブリ内の .NET 名前空間の XML 名前空間宣言を定義することもできます。 たとえば、ここでは、`sys`標準の .NET のプレフィックス`System`である名前空間、 **mscorlib**アセンブリでは、"Microsoft 共通オブジェクト ランタイム ライブラリの"1 回縦が、"多言語標準を今すぐ意味共通オブジェクト ランタイム Library"です。 これは別のアセンブリであるため、必要がありますも、アセンブリ名を指定する例ではこの**mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

注意して、キーワード`clr-namespace`コロンと .NET 名前空間名、キーワード、セミコロンの後に続く`assembly`、等号 (=)、およびアセンブリ名。

はい、コロンを付ける`clr-namespace`が等号 (=) 従って`assembly`です。 構文はよう意図的に定義されたこの: 最も XML 名前空間宣言が参照など、URI スキーム名で始まる URI `http`、これは常に続けてコロンです。 `clr-namespace`その規則を模倣するためにこの文字列の一部が対象としています。

これら両方の名前空間宣言に含まれる、 **StaticConstantsPage**サンプルです。 注意して、`BoxView`に設定されているディメンション`Math.PI`と`Math.E`の 100 倍して、スケール。

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

結果のサイズ`BoxView`画面に対して相対的には、プラットフォームに依存します。

 [![](xaml-markup-extensions-images/staticconstants.png "X:static マークアップ拡張機能を使用して、コントロール")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "X:static マークアップ拡張機能を使用して、コントロール")

## <a name="other-standard-markup-extensions"></a>他の標準のマークアップ拡張機能

いくつかのマークアップ拡張機能では、XAML に固有し、Xamarin.Forms XAML ファイルでサポートされています。 これらのいくつかは非常に多くの場合は使用されませんが、必要なときに不可欠な。

-  プロパティがあるない場合`null`設定に必要な値は、既定で`null`には、設定、`{x:Null}`マークアップ拡張機能です。
-  プロパティが型の場合`Type`を割り当てることができます、`Type`オブジェクト、マークアップ拡張機能を使用して`{x:Type someClass}`です。
-  XAML を使用して配列を定義することができます、`x:Array`マークアップ拡張機能です。 このマークアップ拡張機能がという名前の必須属性を持つ`Type`を示す、配列内の要素の型。
- `Binding`マークアップ拡張機能は、後ほど[パート 4 です。データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)です。

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression マークアップ拡張機能

マークアップ拡張機能は、プロパティを持つことができますが、XML 属性のように設定されています。 マークアップ拡張機能でプロパティの設定は、コンマで区切るし、中かっこ内の引用符が表示されません。

これは、マークアップ拡張機能の Xamarin.Forms という名前を使用して説明できます`ConstraintExpression`で使用される、`RelativeLayout`クラスです。 定数、または親、またはその他の名前付きのビューに対しては、位置または子ビューのサイズを指定できます。 構文、`ConstraintExpression`によりを使用してビューのサイズまたは位置を設定する、`Factor`時間別のビューのプロパティおよび`Constant`です。 何もするよりも複雑には、コードが必要です。

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

おそらく、このサンプルから行う必要があります、最も重要なレッスンは、マークアップ拡張機能の構文: マークアップ拡張機能の中かっこ内で引用符が表示する必要がありますされません。 XAML ファイルで、マークアップ拡張機能を入力する場合は、プロパティの値を引用符で囲むする自然です。 我慢!

実行中のプログラムを次に示します。

[![](xaml-markup-extensions-images/relativelayout.png "制約を使用して、相対的なレイアウト")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "制約を使用して、相対的なレイアウト")

## <a name="summary"></a>まとめ

ここで示すように XAML マークアップ拡張機能は、XAML ファイルの重要なサポートを提供します。 最も重要な XAML のマークアップ拡張機能は、おそらくが`Binding`、このシリーズの次の部分で説明される[パート 4 です。データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)です。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第 5 部MVVM へのデータ バインディング](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
