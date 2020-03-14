---
title: パート 3. XAML マークアップ拡張
description: XAML マークアップ拡張機能は、オブジェクトまたはその他のソースから直接参照されている値に設定するプロパティを XAML で重要な機能を構成します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 03/27/2018
ms.openlocfilehash: 89e2026ff16a9614234d6ee4bfa4df620cf58b56
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305901"
---
# <a name="part-3-xaml-markup-extensions"></a>パート 3. XAML マークアップ拡張

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML マークアップ拡張機能は、他のソースから間接的に参照されるオブジェクトまたは値にプロパティを設定できるようにする、XAML の重要な機能を構成します。XAML マークアップ拡張機能は、オブジェクトを共有する場合や、アプリケーション全体で使用される定数を参照する場合に特に重要ですが、データバインディングで最も多くのユーティリティを見つけることができます。_

## <a name="xaml-markup-extensions"></a>XAML マークアップ拡張

一般に、XAML を使用して、文字列、数値、列挙体のメンバー、またはバック グラウンドでの値に変換される文字列などの明示的な値をオブジェクトのプロパティを設定します。

場合によっては、ただし、プロパティは、それ以外の場合、どこかに定義された値に代わりに参照する必要があります。 または実行時にコードをほとんど処理が必要な場合があります。 このため、XAML*マークアップ拡張機能*を使用できます。

これらの XAML マークアップ拡張機能は、XML の拡張機能ではされません。 XAML は、完全に有効な XML です。 これらは、`IMarkupExtension`を実装するクラスのコードによってサポートされるため、"extensions" と呼ばれています。 独自のカスタム マークアップ拡張機能を記述することができます。

多くの場合、XAML マークアップ拡張機能は XAML ファイルですぐに認識できるため、中かっこで区切られた属性の設定として表示されます。 {および} が、従来型の要素としてマークアップでマークアップ拡張機能の表示場合があります。

## <a name="shared-resources"></a>共有リソース

一部の XAML ページには、同じ値に設定されたプロパティを持ついくつかのビューが含まれます。 たとえば、これらの `Button` オブジェクトのプロパティ設定の多くは同じです。

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

XAML では、このような値またはオブジェクトを*リソースディクショナリ*に格納するという一般的なソリューションが1つあります。 `VisualElement` クラスは、型 `ResourceDictionary`の `Resources` という名前のプロパティを定義します。これは、型の `string` キーと `object`型の値を持つディクショナリです。 このディクショナリにオブジェクトを配置し、それらを XAML ですべてのマークアップから参照できます。

ページでリソースディクショナリを使用するには、`Resources` のプロパティ要素タグのペアを含めます。 これらのページの上部に配置すると便利です。

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

`ResourceDictionary` タグを明示的に含める必要もあります。

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

今すぐオブジェクトとさまざまな種類の値は、リソース ディクショナリに追加できます。 これらの型をインスタンス化可能にする必要があります。 抽象クラスをたとえばすることはできません。 これらの型は、パブリック コンス トラクターも必要です。 各項目には、`x:Key` 属性で指定されたディクショナリキーが必要です。 例 :

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

これらの2つの項目は `LayoutOptions`構造体型の値であり、それぞれに一意のキーと1つまたは2つのプロパティが設定されています。 コードとマークアップでは、`LayoutOptions`の静的フィールドを使用する方が一般的ですが、ここではプロパティを設定する方が便利です。

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

`StaticResource` マークアップ拡張機能は、常に中かっこで区切られ、ディクショナリキーが含まれます。

名前 `StaticResource` `DynamicResource`と区別されます。これは、Xamarin. Forms もサポートします。 `DynamicResource` は、実行時に変更される可能性がある値に関連付けられているディクショナリキーに対して、`StaticResource` は、ページ上の要素が構築されると、ディクショナリの要素に1回だけアクセスします。

`BorderWidth` プロパティの場合、ディクショナリに double を格納する必要があります。 XAML では、`x:Double` や `x:Int32`などの一般的なデータ型のタグを簡単に定義できます。

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

`Color`型のリソースについては、これらの型の属性を直接割り当てるときに使用するのと同じ文字列表現を使用できます。 型コンバーターは、リソースが作成されるときに呼び出されます。 `Color`型のリソースを次に示します。

```xaml
<Color x:Key="textColor">Red</Color>
```

多くの場合、プログラムは、`Large`などの `NamedSize` 列挙体のメンバーに `FontSize` プロパティを設定します。 `FontSizeConverter` クラスはバックグラウンドで動作し、`Device.GetNamedSized` メソッドを使用してプラットフォームに依存する値に変換します。 ただし、フォントサイズリソースを定義する場合は、次に示すように、数値を使用することをお勧めします。 `x:Double` の型として表示されます。

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

`OnPlatform` は、ジェネリッククラスであるため、ディクショナリ内のオブジェクトであり、`x:TypeArguments` 属性であるため、`x:Key` 両方の属性を取得することに注意してください。 オブジェクトが初期化されると、`iOS`、`Android`、および `UWP` 属性が `Color` 値に変換されます。

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

スクリーン ショットは、一貫性のあるスタイル、およびプラットフォームに依存するスタイル設定を確認します。

[![スタイルコントロール](xaml-markup-extensions-images/sharedresources.png)](xaml-markup-extensions-images/sharedresources-large.png#lightbox)

ページの上部に `Resources` コレクションを定義するのが最も一般的ですが、`Resources` プロパティは `VisualElement`によって定義されており、ページの他の要素に `Resources` コレクションを持つことができます。 たとえば、次の例では、`StackLayout` に1つを追加してみます。

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

ボタンのテキストの色が青になったことがわかるでしょう。 基本的に、XAML パーサーは、`StaticResource` マークアップ拡張機能を検出するたびに、ビジュアルツリーを検索し、そのキーを含む最初に見つかった `ResourceDictionary` を使用します。

リソースディクショナリに格納されている最も一般的なオブジェクトの種類の1つは、プロパティ設定のコレクションを定義する Xamarin. Forms `Style`です。 スタイルについては、記事の[スタイル](~/xamarin-forms/user-interface/styles/index.md)をご覧ください。

場合によっては、XAML を初めて使用する開発者は、`Label` や `Button` などのビジュアル要素を `ResourceDictionary`に配置できるという疑問が生じることがあります。 間違いなくことはできますが、あまり意味がをなさない。 `ResourceDictionary` の目的は、オブジェクトを共有することです。 視覚的要素を共有することはできません。 1 ページに 2 回、同じインスタンスは使用できません。

## <a name="the-xstatic-markup-extension"></a>X:static マークアップ拡張機能

名前の類似性にかかわらず、`x:Static` と `StaticResource` は大きく異なります。 `StaticResource` はリソースディクショナリからオブジェクトを返し、`x:Static` は次のいずれかにアクセスします。

- パブリックな静的フィールド
- パブリック静的プロパティ
- パブリック定数フィールド
- 列挙体のメンバー。

`StaticResource` マークアップ拡張機能は、リソースディクショナリを定義する XAML 実装によってサポートされていますが、`x:Static` は `x` のプレフィックスによって明らかになる XAML の組み込み部分です。

次に、`x:Static` が静的フィールドと列挙型のメンバーを明示的に参照する方法を示すいくつかの例を示します。

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

ここまでは、これはそれほどたいしたものではありません。 ただし、`x:Static` マークアップ拡張機能は、静的フィールドまたは独自のコードからプロパティを参照することもできます。 たとえば、アプリケーション全体で複数のページで使用する静的フィールドを含む `AppConstants` クラスを次に示します。

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

その他のクラスにアクセスする XML 名前空間宣言を追加する必要があります。 各追加の XML 名前空間宣言は、新しいプレフィックスを定義します。 `AppConstants`など、共有アプリケーション .NET Standard ライブラリに対してローカルなクラスにアクセスするには、多くの場合、XAML プログラマが `local`プレフィックスを使用します。 名前空間宣言は、CLR (共通言語ランタイム) 名前空間名を示す必要があります。これは、 C# `namespace` 定義または `using` ディレクティブに表示される名前で、.net 名前空間名とも呼ばれます。

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

.NET Standard ライブラリを参照するアセンブリでの .NET 名前空間の XML 名前空間宣言を定義することもできます。 たとえば、次に示すのは、 **netstandard.library**アセンブリにある標準の .net `System` 名前空間の `sys` プレフィックスです。 これは別のアセンブリなので、アセンブリ名も指定する必要があります。この場合、 **netstandard.library**:

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

キーワード `clr-namespace` の後に、コロンと .NET 名前空間の名前、セミコロン、キーワード `assembly`、等号、およびアセンブリ名が続くことに注意してください。

はい。コロンは `clr-namespace` の後に続きますが、等号 (=) は `assembly`の後に続きます。 構文は、この方法で意図的に定義されています。ほとんどの XML 名前空間宣言は、`http`などの URI スキーム名を開始する URI を参照します。これには常にコロンが続きます。 この文字列の `clr-namespace` 部分は、その規則を模倣することを目的としています。

これらの名前空間宣言は、どちらも**StaticConstantsPage**サンプルに含まれています。 `BoxView` のディメンションは `Math.PI` と `Math.E`に設定されていますが、100の係数でスケーリングされています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
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

[x:Static マークアップ拡張機能を使用してコントロールを ![する](xaml-markup-extensions-images/staticconstants.png)](xaml-markup-extensions-images/staticconstants-large.png#lightbox)

## <a name="other-standard-markup-extensions"></a>他の標準のマークアップ拡張機能

いくつかのマークアップ拡張機能では、XAML に固有し、Xamarin.Forms XAML ファイルでサポートされています。 これらのいくつか非常に多くの場合は使用されませんが、必要なときに不可欠です。

- プロパティに既定で `null` 以外の値が設定されていても `null`に設定する場合は、`{x:Null}` マークアップ拡張機能に設定します。
- プロパティが `Type`型の場合は、マークアップ拡張機能 `{x:Type someClass}`を使用して、`Type` オブジェクトに割り当てることができます。
- XAML では、`x:Array` マークアップ拡張機能を使用して、配列を定義できます。 このマークアップ拡張機能には、配列内の要素の型を示す `Type` という名前の必須の属性があります。
- `Binding` マークアップ拡張機能については、[パート4で説明します。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。
- `RelativeSource` マークアップ拡張機能については、「[相対バインド](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)」を参照してください。

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression マークアップ拡張機能

マークアップ拡張機能では、プロパティを使用できますが、XML 属性のように設定されていません。 マークアップ拡張機能では、プロパティの設定がコンマで区切られ、中かっこ内の引用符が表示されません。

これは、`RelativeLayout` クラスで使用される `ConstraintExpression`という名前の Xamarin. Forms マークアップ拡張機能と共に使用できます。 定数の場合、または親であるかその他の名前付きビューに対する相対位置または子ビューのサイズを指定できます。 `ConstraintExpression` の構文を使用すると、別のビューのプロパティと `Constant`を `Factor` て、ビューの位置またはサイズを設定できます。 複雑なコードが必要です。

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

[制約を使用した ![相対レイアウト](xaml-markup-extensions-images/relativelayout.png)](xaml-markup-extensions-images/relativelayout-large.png#lightbox)

## <a name="summary"></a>要約

ここで示すように XAML マークアップ拡張機能は、XAML ファイルの重要なサポートを提供します。 しかし、多くの場合、最も重要な XAML マークアップ拡張機能は `Binding`です。これについては、このシリーズの第4部で説明し[ます。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [パート1。XAML を使用したはじめに](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第2部。必須の XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [パート4。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [パート5。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
