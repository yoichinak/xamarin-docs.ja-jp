---
title: "文字列の書式設定"
description: "データ バインディングを使用して書式設定およびオブジェクトを文字列として表示するには"
ms.topic: article
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 6735e9c03bee981f048231b53539c3b239f64484
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="string-formatting"></a>文字列の書式設定

オブジェクトまたは値の文字列表現を表示するデータ バインドを使用すると便利な場合があります。 使用するなど、`Label`の現在の値を表示する、`Slider`です。 このデータ バインドで、 `Slider` 、ソースであり、ターゲットは、`Text`のプロパティ、`Label`です。

文字列を表示する、コードで、最も強力なツールは、静的な[ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/)メソッドです。 書式指定文字列は、さまざまな種類のオブジェクトに固有のコードを書式設定し、書式が設定される値とその他のテキストを含めることができます。 参照してください、 [.NET 型の書式設定](/dotnet/standard/base-types/formatting-types/)文字列の書式設定の詳細については資料です。

## <a name="the-stringformat-property"></a>StringFormat プロパティ

この機能は、データ バインディングに引き継がれます: を設定する、 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/)プロパティの`Binding`(または[ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/)のプロパティ、`Binding`マークアップ拡張機能) には標準 .NET が 1 つのプレース ホルダーで文字列の書式設定:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

書式指定文字列は、XAML パーサーは中かっこを別の XAML マークアップ拡張機能として扱うことを回避するための単一引用符 (アポストロフィ) 文字で区切られますことに注意してください。 含まない単一引用符文字は浮動小数点値への呼び出しで表示する使用する同じ文字列を文字列をそれ以外の場合、`String.Format`です。 書式指定`F2`小数点以下 2 桁で表示される値が発生します。

`StringFormat`プロパティ場合にだけターゲット プロパティの型は`string`、バインディング モードと`OneWay`または`TwoWay`です。 双方向のバインディングを`StringFormat`ソースからターゲットに渡す値に対してのみ適用されます。

次の記事で後ほど、[バインド パス](binding-path.md)、非常に複雑になり、複雑なデータ バインドになります。 これらのデータ バインディングをデバッグするときに追加できます、`Label`持つ XAML ファイルに、`StringFormat`いくつかの中間結果を表示します。 オブジェクトの種類を表示するだけで使用する場合でもすることができます。

**文字列の書式設定**ページの複数の用途を示しています、`StringFormat`プロパティ。

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

バインディング、`Slider`と`TimePicker`に特定の書式指定の使用を表示する`double`と`TimeSpan`データ型。 `StringFormat`からテキストを表示する、`Entry`ビューでの使用で書式指定文字列は二重引用符を指定する方法を示します、 `&quot;` HTML エンティティです。

XAML ファイルで次のセクションは、`StackLayout`で、`BindingContext`に設定、 `x:Static` 、静的なを参照するマークアップ拡張機能`DateTime.Now`プロパティです。 最初のバインドには、プロパティはありません。

```xaml
<Label Text="{Binding}" />
```

これを表示するだけ、`DateTime`の値、`BindingContext`で既定の書式設定します。 2 つ目のバインドを表示、`Ticks`のプロパティ`DateTime`他の 2 つのバインディングの表示中に、`DateTime`固有の書式設定には、自らです。 これを通知`StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

書式設定文字列に左または右中かっこを表示する場合は、それらのペアを使用します。

最後のセクションを設定、`BindingContext`の値に`Math.PI`し、それを既定の書式設定し、2 種類の数値の書式設定を表示します。

3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![書式設定文字列](string-formatting-images/stringformatting-small.png "書式設定文字列")](string-formatting-images/stringformatting-large.png#lightbox "文字列の書式設定")

## <a name="viewmodels-and-string-formatting"></a>ViewModels と文字列の書式設定

使用すると、`Label`と`StringFormat`、ViewModel の対象になっているビューの値を表示するをビューからバインドを定義することができますか、`Label`またはに ViewModel から、`Label`です。 一般に、2 番目の方法では最適なビューと ViewModel 間のバインドが動作していることを確認します。

この方法を示す、 **[カラー セレクター] 向上**サンプルは、同じ ViewModel としてを使用して、**単純なカラー セレクター**プログラムに示すように、 [**バインド モード**](binding-mode.md)資料。

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

ペアがあります。 今すぐ次の 3 つの`Slider`と`Label`を同じバインドされている要素のソースのプロパティ、`HslColorViewModel`オブジェクト。 唯一の違いは`Label`が、`StringFormat`プロパティを表示する各`Slider`値。

[![色セレクターのより](string-formatting-images/bettercolorselector-small.png "色セレクターのより")](string-formatting-images/bettercolorselector-large.png#lightbox "色セレクターの向上")

だろう、従来の 2 桁の 16 進数形式の RGB (赤、緑、青) 値の表示方法できます。 これらの整数値がない直接から、`Color`構造体。 1 つのソリューションは、ViewModel 内の色要素の整数値を計算し、プロパティとして公開することです。 フォーマットする可能性がありますを使用して、`X2`仕様を書式設定します。

別のアプローチがより一般的な: 記述することができます、*バインディング値コンバーター* 、以降の記事で説明したよう[**バインディング値コンバーター**](converters.md)です。

ただし、次の記事を検討、 [**バインド パス**](binding-path.md)さらに詳細、およびサブプロパティとコレクション内のアイテム参照を使用する方法を表示します。


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
