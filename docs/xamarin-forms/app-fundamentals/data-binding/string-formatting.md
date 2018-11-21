---
title: Xamarin.Forms 文字列の書式設定
description: この記事では、Xamarin.FOrms のデータ バインディングを使用して書式設定し、オブジェクトを文字列として表示する方法について説明します。 これは、バインドの StringFormat をプレース ホルダーと標準 .NET 書式設定文字列に設定して実現されます。
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 8efd93204b848113e0ed95c8066a5506eb517ac6
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52170950"
---
# <a name="xamarinforms-string-formatting"></a>Xamarin.Forms 文字列の書式設定

データ バインドを使用して、オブジェクトまたは値の文字列形式を表示する便利な場合があります。 使用するなど、`Label`の現在の値を表示する、`Slider`します。 このデータ バインドで、 `Slider` 、ソースし、ターゲットが、`Text`のプロパティ、`Label`します。

コードで文字列を表示するときに最も強力なツールは、静的な[ `String.Format` ](xref:System.String.Format(System.String,System.Object))メソッド。 書式指定文字列は、さまざまな種類のオブジェクトに固有のコードを書式設定と書式設定される値とその他のテキストを含めることができます。 参照してください、 [.NET 型の書式設定](/dotnet/standard/base-types/formatting-types/)文字列の書式設定の詳細については資料。

## <a name="the-stringformat-property"></a>StringFormat プロパティ

この機能は、データ バインドに引き継がれます: 設定する、 [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat)プロパティの`Binding`(または[ `StringFormat` ](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat)のプロパティ、`Binding`マークアップ拡張機能) を標準 .NET が 1 つのプレース ホルダーと文字列の書式設定:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

書式指定文字列は、XAML パーサーが中かっこを別の XAML マークアップ拡張機能として扱うことを回避するための単一引用符 (アポストロフィ) 文字で区切られますことに注意してください。 含まない単一引用符文字は浮動小数点値への呼び出しで表示する使用するのと同じ文字列を文字列をそれ以外の場合、`String.Format`します。 書式指定`F2`と小数点以下 2 桁で表示される値。

`StringFormat`プロパティときにのみ意味ターゲット プロパティの型は`string`、バインディング モードおよび`OneWay`または`TwoWay`します。 双方向のバインディングを`StringFormat`はのみ、ソースからターゲットに渡す値に適用されます。

次の記事でわかる、[バインド パス](binding-path.md)、データ バインドは非常に複雑な複雑になることができます。 これらのデータ バインディングをデバッグするときに追加できます、`Label`で XAML ファイルに、`StringFormat`いくつかの中間結果を表示します。 場合でも、役に立つオブジェクトの型の表示にのみ使用するとします。

**文字列の書式設定**ページのいくつかの使用を示しています、`StringFormat`プロパティ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

バインド、`Slider`と`TimePicker`に特定の形式の仕様の使用方法を示して`double`と`TimeSpan`データ型。 `StringFormat`からテキストを表示する、`Entry`ビューは、使用量の書式指定文字列では、二重引用符を指定する方法を示します、 `&quot;` HTML エンティティ。

XAML ファイルで次のセクションは、`StackLayout`で、`BindingContext`に設定、 `x:Static` 、静的なを参照するマークアップ拡張機能`DateTime.Now`プロパティ。 最初のバインドには、プロパティがありません。

```xaml
<Label Text="{Binding}" />
```

だけが表示されます、`DateTime`の値、`BindingContext`で既定の書式設定します。 2 番目のバインドが表示されます、`Ticks`プロパティの`DateTime`が表示されるその他の 2 つのバインディング、`DateTime`自体と特定の書式設定します。 この点に気付く`StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

書式設定文字列の左または右中かっこを表示する必要がある場合は、それらのペアを使用だけです。

最後のセクションの設定、`BindingContext`の値に`Math.PI`され、既定の書式設定、数値の書式設定の 2 つのさまざまな種類と表示されます。

実行中のプログラムを次に示します。

[![文字列の書式設定](string-formatting-images/stringformatting-small.png "文字列の書式設定")](string-formatting-images/stringformatting-large.png#lightbox "文字列の書式設定")

## <a name="viewmodels-and-string-formatting"></a>ビューモデル、および文字列の書式設定

使用すると、`Label`と`StringFormat`ViewModel の対象でもあるビューの値を表示する、ビューからバインドを定義することができますか、`Label`またはに ViewModel から、`Label`します。 一般に、2 番目のアプローチはビューと ViewModel 間のバインドが動作しているを検証するために最適です。

この方法を示した、**より良い色セレクター**として同じ ViewModel を使用して、サンプル、**単純なカラー セレクター**プログラムに示すように、 [**バインド モード**](binding-mode.md)記事。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

現在の 3 つのペアがある`Slider`と`Label`ソースでプロパティを同じにバインドされている要素、`HslColorViewModel`オブジェクト。 唯一の違いは`Label`が、`StringFormat`プロパティを表示する各`Slider`値。

[![色セレクターのより](string-formatting-images/bettercolorselector-small.png "色セレクターのより")](string-formatting-images/bettercolorselector-large.png#lightbox "色セレクターの強化")

かもしれません。 RGB (赤、緑、青) 値を従来の 2 桁の 16 進数形式の方法で表示できます。 これらの整数値がから直接使用できない、`Color`構造体。 1 つのソリューションは、ビューモデル内での色要素の整数値を計算し、プロパティとして公開することです。 フォーマットする可能性がありますを使用して、`X2`仕様を書式設定します。

別のアプローチがより一般的な: 記述することができます、*バインディング値コンバーター*後の記事で説明したよう[**値コンバーターのバインディング**](converters.md)します。

ただし、次の記事を紹介します[**バインド パス**](binding-path.md)さらに詳細、およびサブプロパティとコレクション内の項目参照の使用方法を示します。


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
