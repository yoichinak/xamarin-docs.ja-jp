---
title: リソース ディクショナリ
description: XAML リソースは、2 回以上使用できるオブジェクトの定義です。 ResourceDictionary は、リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用できます。 この記事では、作成し、ResourceDictionary を使用する方法、リソース ディクショナリをマージする方法について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: aa3ae9fed67b6cd7521e5c59edcb54f05cc6b7c5
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2018
---
# <a name="resource-dictionaries"></a>リソース ディクショナリ

_XAML リソースは、2 回以上使用できるオブジェクトの定義です。ResourceDictionary は、リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用できます。この記事では、作成し、ResourceDictionary を使用する方法、リソース ディクショナリをマージする方法について説明します。_

## <a name="overview"></a>概要

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Xamarin.Forms アプリケーションによって使用されているリソースのリポジトリがします。 格納されている標準的なリソース、`ResourceDictionary`含める[スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)、[データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)色、およびコンバーター。

XAML では、リソースが定義されている、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を取得してを使用して要素に適用される、`StaticResource`マークアップ拡張機能です。 C# の場合は、リソースが定義されている、`ResourceDictionary`を取得して、文字列ベースのインデクサーを使用して要素に適用します。 ただし、使用する利点のほとんどは、 `ResourceDictionary` 、C# の場合は、リソースとして簡単に直接に代入できます視覚要素のプロパティを最初から取得することがなく、`ResourceDictionary`です。

## <a name="creating-and-consuming-a-resourcedictionary"></a>ResourceDictionary の作成と

リソースを定義することができます、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)に接続されている、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 、ページやコントロールのコレクションまたは、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/)コレクションアプリケーションです。 定義する場所を選択する、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)インパクトを使用できます。

- 内のリソース、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)定義で、コントロール レベルのみを適用できますコントロールとその子。
- 内のリソース、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)定義で、ページ レベルのみを適用できますページおよびに自身の子にします。
- 内のリソース、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)定義されているアプリケーションでは、アプリケーション全体でレベルが適用することができます。

次の XAML コードの例は、アプリケーション レベルで定義されているリソースを示しています[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):。

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

これは、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)定義 3 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)リソースと[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)リソース。 XAML の作成の詳細については`App`クラスを参照してください[App クラス](~/xamarin-forms/app-fundamentals/application-class.md)です。

各リソースには、指定したキーを使用して、`x:Key`属性は、ことのわかりやすいキーを`ResourceDictionary`です。 リソースを取得するキーが使用される、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)によって、`StaticResource`マークアップ拡張機能、コントロールで定義されているその他のリソースのレベルを表示する次の XAML コードの例に示すよう`ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

最初の[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスを取得し、使用、`LabelPageHeadingStyle`アプリケーション レベルで定義されているリソース[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)と 2 番目`Label`インスタンス取得して、利用、`LabelNormalStyle`制御レベルで定義されているリソース`ResourceDictionary`です。 同様に、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)インスタンスを取得し、使用、`NormalTextColor`アプリケーション レベルで定義されているリソース`ResourceDictionary`、および`MediumBoldText`制御レベルで定義されているリソース`ResourceDictionary`です。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary リソースを消費")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary リソースを消費")

> [!NOTE]
> 1 つのページに固有のリソースは、アプリケーション レベルのリソース ディクショナリ、そのため、ページで必要なときに、リソースの代わりにアプリケーションの起動時に解析されますに含めるべきではありません。 詳細については、次を参照してください。[アプリケーション リソース ディクショナリのサイズを小さく](~/xamarin-forms/deploy-test/performance.md)です。

## <a name="overriding-resources"></a>リソースをオーバーライドします。

ときに[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)のリソースを共有`x:Key`属性値は、ビューの階層内で下に定義されているリソースはよりも優先をそれ以上定義されているものです。 たとえば、設定、`PageBackgroundColor`リソースを`Blue`アプリケーションでレベルがページ レベルで上書きされている`PageBackgroundColor`に設定されたリソース`Yellow`です。 ページ レベルでは同様に、`PageBackgroundColor`リソースは制御レベルによって上書きされます`PageBackgroundColor`リソース。 この優先順位は次の XAML コードの例について説明します。

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

元の`PageBackgroundColor`と`NormalTextColor`アプリケーション レベルで定義されているインスタンスはによってオーバーライドされる、`PageBackgroundColor`と`NormalTextColor`ページ レベルで定義されているインスタンス。 そのため、ページの背景色青になり、次のスクリーン ショットで示したように、ページ上のテキストは、黄色になります。

[![](resource-dictionaries-images/overridding-screenshots-sml.png "ResourceDictionary リソースをオーバーライドする")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary リソースをオーバーライドします。")

しかし、なおのバック グラウンド バー、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)がまだ黄色のため、 [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)の値に設定されて、`PageBackgroundColor`アプリケーションで定義されているリソースレベル[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。

## <a name="merged-resource-dictionaries"></a>マージされたリソース ディクショナリ

マージされたリソース ディクショナリは、1 つ以上を組み合わせた[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)に別のインスタンス。 これを実現するには、`ResourceDictionary.MergedDictionaries`アプリケーション、ページ、または制御レベルにマージされる 1 つまたは複数のリソース ディクショナリにプロパティ`ResourceDictionary`です。

> [!IMPORTANT]
> [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)型自体には、 [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) 、1 つのマージに使用できるプロパティ`ResourceDictionary`を別に、 `ResourceDictionary` の値として指定された`MergedWith`現在にマージしているプロパティ`ResourceDictionary`インスタンス。 経由でマージするときに、`MergedWith`プロパティ、現在のすべてのリソース`ResourceDictionary`その共有`x:Key`属性値内のリソースを`ResourceDictionary`マージするのには置き換えられます。 ただし、`MergedWith`プロパティは、Xamarin.Forms の将来のリリースで廃止される予定です。 したがって、使用する推奨が、`MergedDictionaries`プロパティをマージする`ResourceDictionary`インスタンス。

XAML コード例を次に、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)という`MyResourceDictionary`を含む 1 つのリソース。

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` 任意のアプリケーション、ページ、または制御レベルにマージできます`ResourceDictionary`です。 次の XAML コードの例がページ レベルにマージしていることを示しています`ResourceDictionary`を使用して、`MergedDictionaries`プロパティ。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

マージされた場合に[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)のリソースを同じ共有`x:Key`Xamarin.Forms 属性値は、次のリソースの優先順位を使用します。

1. リソース ディクショナリへのローカル リソース。
1. 使用してマージされたリソース ディクショナリに含まれるリソース、 [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/)プロパティです。
1. 使用してマージされたリソース ディクショナリに含まれるリソース、`MergedDictionaries`に登録されている順序でのコレクション、`MergedDictionaries`プロパティです。

> [!NOTE]
> リソース ディクショナリの検索は、負荷の高い作業アプリケーションが複数含まれている場合の大規模なリソース ディクショナリ。 そのため、アプリケーション内の各ページだけのページに、不要な検索を回避するには、該当するリソース ディクショナリを使用することを確認します。

## <a name="summary"></a>まとめ

この記事の内容を作成および使用する方法を説明した、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、方法およびリソース ディクショナリをマージします。 A`ResourceDictionary`リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用を許可します。


## <a name="related-links"></a>関連リンク

- [リソース ディクショナリ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
