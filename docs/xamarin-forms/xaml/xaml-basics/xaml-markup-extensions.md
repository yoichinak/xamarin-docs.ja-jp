---
title: 第 3 部です。 XAML マークアップ拡張機能
description: XAML マークアップ拡張機能は、オブジェクトまたはその他のソースから直接参照されている値に設定するプロパティを XAML で重要な機能を構成します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 3/27/2018
ms.openlocfilehash: bffddfdb67238287b868b01edad88bc8d43e5bb1
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563837"
---
# <a name="part-3-xaml-markup-extensions"></a>第 3 部です。 XAML マークアップ拡張機能

_XAML マークアップ拡張機能は、オブジェクトまたはその他のソースから直接参照されている値に設定するプロパティを XAML で重要な機能を構成します。XAML マークアップ拡張機能は、オブジェクトの共有と、アプリケーション全体で使用される定数を参照するにとって特に重要ですが、データ バインドで、最大のユーティリティを検索します。_

## <a name="xaml-markup-extensions"></a>XAML マークアップ拡張機能

一般に、XAML を使用して、文字列、数値、列挙体のメンバー、またはバック グラウンドでの値に変換される文字列などの明示的な値をオブジェクトのプロパティを設定します。

場合によっては、ただし、プロパティは、それ以外の場合、どこかに定義された値に代わりに参照する必要があります。 または実行時にコードをほとんど処理が必要な場合があります。 このため、XAML*マークアップ拡張機能*利用できます。

これらの XAML マークアップ拡張機能は、XML の拡張機能ではされません。 XAML は、完全に有効な XML です。 "Extensions"と呼ばれているは、実装するクラス内のコードでのバックアップがあるため`IMarkupExtension`します。 独自のカスタム マークアップ拡張機能を記述することができます。

多くの場合、XAML マークアップ拡張機能は XAML ファイルですぐに認識できるため、中かっこで区切られた属性の設定として表示されます。 {および} が、従来型の要素としてマークアップでマークアップ拡張機能の表示場合があります。

## <a name="shared-resources"></a>共有リソース

一部の XAML ページには、同じ値に設定されたプロパティを持ついくつかのビューが含まれます。 たとえば、これらのプロパティの設定の多く`Button`オブジェクトが等しい。

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

を変更するこれらのプロパティのいずれかの必要がある場合は、3 回ではなく、1 回だけ変更できます。 これがコードの場合、するを使用する可能性定数と静的な読み取り専用オブジェクト一貫して簡単に変更してこのような値を保つためにします。

1 つの一般的なソリューション、XAML では、このような値を格納するまたは内のオブジェクトを*リソース ディクショナリ*します。 `VisualElement`クラスという名前のプロパティを定義する`Resources`型の`ResourceDictionary`、型のキーを持つディクショナリである`string`型の値と`object`します。 このディクショナリにオブジェクトを配置し、それらを XAML ですべてのマークアップから参照できます。

ページ上をリソース ディクショナリを使用するには、ペアをインクルード`Resources`プロパティ要素タグ。 これらのページの上部に配置すると便利です。

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

今すぐオブジェクトとさまざまな種類の値は、リソース ディクショナリに追加できます。 これらの型をインスタンス化可能にする必要があります。 抽象クラスをたとえばすることはできません。 これらの型は、パブリック コンス トラクターも必要です。 各項目で指定されたディクショナリのキーが必要です、`x:Key`属性。 例えば:

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

これら 2 つの項目は、構造型の値`LayoutOptions`とには、一意のキーと 1 つまたは 2 つのプロパティを設定します。 コードとマークアップではの静的フィールドを使用して一般的な`LayoutOptions`、ここでプロパティを設定する方が便利ですが。

設定する必要があるので、`HorizontalOptions`と`VerticalOptions`これらのリソースでは、これらのボタンのプロパティと、そのために、 `StaticResource` XAML マークアップ拡張機能。

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

名前`StaticResource`から区別`DynamicResource`Xamarin.Forms もサポートします。 `DynamicResource` dictionary のキーに関連付けられた実行中に変わる可能性がある値の中に`StaticResource`ページ上の要素を構築するときに 1 回では、ディクショナリから要素にアクセスします。

`BorderWidth`プロパティは、double 型の値をディクショナリに格納するために必要です。 XAML は便利なことのような一般的なデータ型のタグを定義します`x:Double`と`x:Int32`:

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

次の 3 つの行に配置する必要はありません。 この回転角の場合は、このディクショナリ エントリは、わずか 1 行上。

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

種類のリソースに対する`Color`、直接これらの型の属性を割り当てるときに使用する同じ文字列形式を使用することができます。 型コンバーターは、リソースが作成されるときに呼び出されます。 型のリソースを次に示します`Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

多くの場合、プログラムのセットを`FontSize`プロパティのメンバーを`NamedSize`など列挙`Large`します。 `FontSizeConverter`クラスを使用して、プラットフォームに依存する値に変換する背後のしくみ、`Device.GetNamedSized`メソッド。 ただし、フォント サイズのリソースを定義するときに行う方を示す数値を使用して、ここで、`x:Double`型。

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

注意して`OnPlatform`両方を取得、`x:Key`属性ディクショナリ内のオブジェクトは、および`x:TypeArguments`属性、ジェネリック クラスは、します。 `iOS`、 `Android`、および`UWP`属性に変換されます`Color`オブジェクトが初期化される値します。

3 つのボタンが 6 つの共有値へのアクセスを持つ最後の完全な XAML ファイルを次に示します。

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

スクリーン ショットは、一貫性のあるスタイル、およびプラットフォームに依存するスタイル設定を確認します。

[![](xaml-markup-extensions-images/sharedresources.png "スタイルのコントロールを")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "スタイルのコントロール")

定義する最も一般的です、 `Resources` 、ページの上部にあるコレクションに留意する、`Resources`によってプロパティが定義されている`VisualElement`、持つことが可能`Resources`ページ上の他の要素のコレクション。 たとえば、を 1 つを追加してみてください、`StackLayout`この例では。

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

ボタンのテキストの色が青になったことがわかるでしょう。 XAML パーサーが検出されるたびに、基本的に、`StaticResource`マークアップ拡張機能、ビジュアル ツリーを検索し、1 つ目を使用して、`ResourceDictionary`そのキーを格納しているが発生しました。

リソース ディクショナリに格納されたオブジェクトの最も一般的な種類の 1 つは、Xamarin.Forms`Style`プロパティの設定のコレクションを定義します。 スタイルは、情報の記事で説明した[スタイル](~/xamarin-forms/user-interface/styles/index.md)します。

初めて使用する XAML 開発者疑問ビジュアル要素をなど、配置できるかどうか`Label`または`Button`で、`ResourceDictionary`します。 間違いなくことはできますが、あまり意味がをなさない。 目的、`ResourceDictionary`オブジェクトを共有することです。 視覚的要素を共有することはできません。 1 ページに 2 回、同じインスタンスは使用できません。

## <a name="the-xstatic-markup-extension"></a>X:static マークアップ拡張機能

その名前の類似`x:Static`と`StaticResource`は大きく異なります。 `StaticResource` 中にリソース ディクショナリからオブジェクトを返します`x:Static`次のいずれかにアクセスします。

- パブリックな静的フィールド
- パブリック静的プロパティ
- パブリック定数フィールド
- 列挙体のメンバー。

`StaticResource`マークアップ拡張機能は、リソース ディクショナリを定義する XAML の実装でサポートされています。 中に`x:Static`として、XAML の組み込みの一部である、`x`プレフィックスが表示されます。

ここでは、いくつかの例を示す方法`x:Static`静的フィールドおよび列挙型メンバーを明示的に参照できます。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

ここまでは、これはそれほどたいしたものではありません。 `x:Static`マークアップ拡張機能も参照できます静的フィールドまたはプロパティ、独自のコードから。 たとえば、ここでは、`AppConstants`アプリケーション全体で複数のページを使用するいくつかの静的フィールドを含むクラス。

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

XAML ファイルでこのクラスの静的フィールドを参照するには、何らかの方法をこのファイルは、XAML ファイル内で示す必要があります。 これは、XML 名前空間宣言で行います。

標準の Xamarin.Forms XAML テンプレートの一部として作成された XAML ファイルに 2 つの XML 名前空間宣言が含まれていることを思い出してください。 Xamarin.Forms クラスと別のタグと XAML に固有の属性を参照するためにアクセスするための 1 つ。

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

その他のクラスにアクセスする XML 名前空間宣言を追加する必要があります。 各追加の XML 名前空間宣言は、新しいプレフィックスを定義します。 共有アプリケーションの .NET Standard ライブラリのローカル クラスへのアクセスなどに`AppConstants`、XAML のプログラマは多くの場合、プレフィックスを使用して`local`します。 名前空間の宣言は、CLR (共通言語ランタイム) の名前空間の名前とも呼ばれる .NET 名前空間の名前、これは、c# で表示される名前を示す必要があります`namespace`定義または、`using`ディレクティブ。

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

.NET Standard ライブラリを参照するアセンブリでの .NET 名前空間の XML 名前空間宣言を定義することもできます。 たとえば、次に示します、`sys`標準の .NET のプレフィックス`System`内にある名前空間、 **mscorlib**アセンブリでは、「Microsoft 共通オブジェクト ランタイム ライブラリ、」縦 1 回が、"多言語標準を今すぐ意味共通オブジェクト ランタイム Library"です。 これは別のアセンブリであるため、する必要がありますも指定するアセンブリ名では、ここで**mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

注意キーワード`clr-namespace`コロンと .NET 名前空間名は、セミコロン、キーワードの後に続く`assembly`、等号 (=) と、アセンブリ名。

はい、コロン`clr-namespace`等号が`assembly`します。 この意図定義された構文: 最も XML 名前空間宣言など、URI スキーム名を開始する URI を参照する`http`コロンが後に常にします。 `clr-namespace`その規則を模倣するためにこの文字列の一部が対象としています。

これら両方の名前空間宣言が含まれている、 **StaticConstantsPage**サンプル。 注意、`BoxView`ディメンションに設定されます`Math.PI`と`Math.E`100 の倍数ではスケール。

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

結果のサイズ`BoxView`画面を基準とは、プラットフォームに依存します。

 [![](xaml-markup-extensions-images/staticconstants.png "X:static マークアップ拡張機能を使用してコントロール")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "X:static マークアップ拡張機能を使用してコントロール")

## <a name="other-standard-markup-extensions"></a>他の標準のマークアップ拡張機能

いくつかのマークアップ拡張機能では、XAML に固有し、Xamarin.Forms XAML ファイルでサポートされています。 これらのいくつか非常に多くの場合は使用されませんが、必要なときに不可欠です。

-  プロパティがあるない場合`null`が既定値に設定する`null`に設定、`{x:Null}`マークアップ拡張機能。
-  プロパティが型の場合`Type`に割り当てることができます、`Type`オブジェクト マークアップ拡張機能を使用して`{x:Type someClass}`します。
-  XAML を使用して配列を定義することができます、`x:Array`マークアップ拡張機能。 このマークアップ拡張機能がという名前の必須属性`Type`配列内の要素の型を示します。
- `Binding`マークアップ拡張機能は、後ほど[パート 4 です。データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)します。

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression マークアップ拡張機能

マークアップ拡張機能では、プロパティを使用できますが、XML 属性のように設定されていません。 マークアップ拡張機能では、プロパティの設定がコンマで区切られ、中かっこ内の引用符が表示されません。

これは、マークアップ拡張機能の Xamarin.Forms という名前を使用して説明できます`ConstraintExpression`で使用される、`RelativeLayout`クラス。 定数の場合、または親であるかその他の名前付きビューに対する相対位置または子ビューのサイズを指定できます。 構文、`ConstraintExpression`位置またはを使用してビューのサイズを設定できます、`Factor`時間別のビューのプロパティおよび`Constant`します。 複雑なコードが必要です。

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

おそらく、マークアップ拡張機能の構文は、このサンプルから行う必要があります、最も重要なレッスンです。 引用符がマークアップ拡張機能の中かっこ内で表示する必要がありますされません。 XAML ファイルにマークアップ拡張機能を入力するときに、プロパティの値を引用符で囲みますする自然なです。 しないでください。

実行中のプログラムを次に示します。

[![](xaml-markup-extensions-images/relativelayout.png "制約を使用して、相対的なレイアウト")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "制約を使用して、相対的なレイアウト")

## <a name="summary"></a>まとめ

ここで示すように XAML マークアップ拡張機能は、XAML ファイルの重要なサポートを提供します。 おそらく最も重要な XAML マークアップ拡張機能ですが、 `Binding`、このシリーズの次の部分で説明される[パート 4 です。データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)します。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 1 部XAML の概要](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第 5 部MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
