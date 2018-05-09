---
title: リソース ディクショナリ
description: XAML リソースは、共有および Xamarin.Forms アプリケーション全体で再利用できるオブジェクトです。
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/02/2018
ms.openlocfilehash: ee3e4c984072fc019fe3719aab650a44d3899911
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="resource-dictionaries"></a>リソース ディクショナリ

_XAML リソースは、共有および Xamarin.Forms アプリケーション全体で再利用できるオブジェクトの定義です。_

これらのリソース オブジェクトは、リソース ディクショナリに格納されます。 この記事では、作成し、リソース ディクショナリを使用する方法、リソース ディクショナリをマージする方法について説明します。

## <a name="overview"></a>概要

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Xamarin.Forms アプリケーションによって使用されているリソースのリポジトリがします。 格納されている標準的なリソース、`ResourceDictionary`含める[スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)、[データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)色、およびコンバーター。

XAML では、リソースに格納されている、`ResourceDictionary`されるを取得しを使用して要素に適用される、`StaticResource`マークアップ拡張機能です。 C# の場合は、リソース定義することもに、`ResourceDictionary`を取得して、文字列ベースのインデクサーを使用して要素に適用します。 ただし、使用する利点のほとんどは、 `ResourceDictionary` C# の場合は、共有オブジェクトは単にフィールドまたはプロパティとして格納されているし、しない直接アクセスと最初に元に戻すディクショナリ。

## <a name="creating-and-consuming-a-resourcedictionary"></a>ResourceDictionary の作成と

リソースがで定義されている、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 、次のいずれかに設定されている`Resources`プロパティ。

- [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/)から派生したクラスのプロパティ [`Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)
- [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)任意のクラスの派生元となるプロパティ['VisualElement'](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)

Xamarin.Forms プログラムには、1 つだけから派生するクラスが含まれています。`Application`多くの場合、使用する多くのクラスから派生するが、 `VisualElement`(ページ、レイアウト、およびコントロールなど)。 これらのオブジェクトがその`Resources`プロパティに設定、`ResourceDictionary`です。 特定の配置場所を選択する`ResourceDictionary`から受ける影響のリソースを使用できます。

- 内のリソース、`ResourceDictionary`ように、ビューに接続されている`Button`または`Label`ので、これは非常に便利ではないのみに特定のオブジェクトに適用できます。
- 内のリソース、`ResourceDictionary`などのレイアウトにアタッチされている`StackLayout`または`Grid`レイアウトやそのレイアウトのすべての子に適用できます。 
- 内のリソース、`ResourceDictionary`定義ページで、ページとそのすべての子レベルを適用できます。
- 内のリソース、`ResourceDictionary`定義されているアプリケーションでは、アプリケーション全体でレベルが適用することができます。

次の XAML は、アプリケーション レベルで定義されているリソースを示しています。`ResourceDictionary`で、 **App.xaml**標準 Xamarin.Forms プログラムの一部として作成されたファイル。

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

これは、`ResourceDictionary`定義 3 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)リソースと[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)リソース。 詳細については、`App`クラスを参照してください[App クラス](~/xamarin-forms/app-fundamentals/application-class.md)です。

明示的な Xamarin.Forms 3.0 以降では`ResourceDictionary`タグは必要ありません。 `ResourceDictionary`オブジェクトが自動的に作成され、リソースの間で直接挿入することができます、`Resources`プロパティ要素タグ。

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

各リソースには、指定したキーを使用して、`x:Key`属性は、ディクショナリのキーになります、`ResourceDictionary`です。 リソースを取得するキーが使用される、`ResourceDictionary`によって、 [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)内で定義されているその他のリソースを示す次の XAML コードの例で示したようには、マークアップ拡張機能、 `StackLayout`:

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

最初の[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスを取得し、使用、`LabelPageHeadingStyle`アプリケーション レベルで定義されているリソース`ResourceDictionary`と 2 番目`Label`インスタンスの取得および使用、 `LabelNormalStyle`制御レベルで定義されているリソース`ResourceDictionary`です。 同様に、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)インスタンスを取得し、使用、`NormalTextColor`アプリケーション レベルで定義されているリソース`ResourceDictionary`、および`MediumBoldText`制御レベルで定義されているリソース`ResourceDictionary`です。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary リソースを消費")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary リソースを消費")

> [!NOTE]
> 1 つのページに固有のリソースは、アプリケーション レベルのリソース ディクショナリ、そのため、ページで必要なときに、リソースの代わりにアプリケーションの起動時に解析されますに含めるべきではありません。 詳細については、次を参照してください。[アプリケーション リソース ディクショナリのサイズを小さく](~/xamarin-forms/deploy-test/performance.md)です。

## <a name="overriding-resources"></a>リソースをオーバーライドします。

ときに`ResourceDictionary`のリソースを共有`x:Key`属性値は、ビューの階層内で下に定義されているリソースはよりも優先をそれ以上定義されているものです。 たとえば、設定、`PageBackgroundColor`リソースを`Blue`アプリケーションでレベルがページ レベルで上書きされている`PageBackgroundColor`に設定されたリソース`Yellow`です。 ページ レベルでは同様に、`PageBackgroundColor`リソースは制御レベルによって上書きされます`PageBackgroundColor`リソース。 この優先順位は次の XAML コードの例について説明します。

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

しかし、なおのバック グラウンド バー、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)がまだ黄色のため、 [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)の値に設定されて、`PageBackgroundColor`アプリケーションで定義されているリソースレベル`ResourceDictionary`です。

について検討する別の方法を次に示します`ResourceDictionary`優先順位: ときに、XAML パーサーが検出した、 `StaticResource`、ビジュアル ツリーを通ってで一致するキーの検索、見つかった最初の一致を使用します。 場合は、ページでこの検索を終了し、キーもされてを見つけられない、XAML パーサーを検索、`ResourceDictionary`にアタッチされている、`App`オブジェクト。 キーが見つからない場合、例外が発生します。

## <a name="stand-alone-resource-dictionaries"></a>スタンドアロン リソース ディクショナリ

派生したクラス`ResourceDictionary`別のスタンドアロン ファイルにすることもできます。 (具体的から派生したクラス`ResourceDictionary`通常必要がある、_ペア_ファイルのリソースが XAML ファイルの分離コード ファイルで定義されているため、`InitializeComponent`呼び出しも必要です)。結果のファイルは、アプリケーション間で共有できます。

このようなファイルを作成するには、追加、新しい**コンテンツ ビュー**または**コンテンツ ページ**をプロジェクトに項目 (ではなく、**コンテンツ ビュー**または**コンテンツ ページ**とc# ファイルのみ)。 XAML ファイルと c# ファイルの両方で基本クラスの名前変更`ContentView`または`ContentPage`に`ResourceDictionary`です。 XAML ファイルでは、基本クラスの名前は、最上位要素です。

XAML 例を次に、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)という`MyResourceDictionary`:

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

これは、`ResourceDictionary`型のオブジェクトは、1 つのリソースを含む`DataTemplate`です。

インスタンス化`MyResourceDictionary`のペアの間に配置して`Resources`プロパティ要素タグなどで、 `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>  
```

インスタンス`MyResourceDictionary`に設定されている、`Resources`のプロパティ、`ContentPage`オブジェクト。

ただし、このアプローチにはいくつかの制限:`Resources`のプロパティ、`ContentPage`この 1 つだけ参照`ResourceDictionary`です。 ほとんどの場合、するなどその他のオプション`ResourceDictionary`インスタンスと、その他のリソースとします。

このタスクには、マージされたリソース ディクショナリが必要です。

## <a name="merged-resource-dictionaries"></a>マージされたリソース ディクショナリ

マージされたリソース ディクショナリは、1 つ以上を組み合わせた`ResourceDictionary`に別のインスタンス`ResourceDictionary`です。 これを行う XAML ファイルで設定して、 [ `MergedDictionaries` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedDictionaries/)アプリケーション、ページ、または制御レベルにマージされる 1 つまたは複数のリソース ディクショナリにプロパティ`ResourceDictionary`です。

> [!IMPORTANT]
> `ResourceDictionary` また、定義、 [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/)プロパティです。 このプロパティは使用しないでください。Xamarin.Forms 3.0 の時点で廃止されました。

インスタンスと`MyResourceDictionary`任意のアプリケーション、ページ、または制御レベルにマージできます`ResourceDictionary`です。 次の XAML コードの例がページ レベルにマージしていることを示しています`ResourceDictionary`を使用して、`MergedDictionaries`プロパティ。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

そのマークアップは、のインスタンスのみを示しています。`MyResourceDictionary`に追加されている、`ResourceDictionary`他のを参照することもできますが、`ResourceDictionary`インスタンス内で、`MergedDictionary`プロパティ要素タグ、およびそれらのタグの外部でのその他のリソース。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

1 つのみできます`MergedDictionaries`」の「、 `ResourceDictionary`、数だけを配置することができますが、`ResourceDictionary`そこでインスタンス化します。

マージされた場合に[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)のリソースを同じ共有`x:Key`Xamarin.Forms 属性値は、次のリソースの優先順位を使用します。

1. リソース ディクショナリへのローカル リソース。
1. マージされたリソース ディクショナリに含まれるリソースによって使用されていない[ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/)プロパティです。
1. 使用してマージされたリソース ディクショナリに含まれるリソース、`MergedDictionaries`に登録されている順序でのコレクション、`MergedDictionaries`プロパティです。

> [!NOTE]
> リソース ディクショナリの検索は、負荷の高い作業アプリケーションが複数含まれている場合の大規模なリソース ディクショナリ。 そのため、不要な検索を回避するのには、アプリケーション内の各ページだけが、ページに対応するリソース ディクショナリを使用するをするようにします。

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Xamarin.Forms 3.0 でのディクショナリのマージ

Xamarin.Forms 3.0 では、マージ プロセスで始まる`ResourceDictionaries`と比較的簡単かつ柔軟になりました。 `MergedDictionaries`プロパティ要素タグが必要ではありません。 代わりに、追加するリソース ディクショナリに間`ResourceDictionary`に新しいタグ[ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.Source/)プロパティが、リソースを持つ XAML ファイルのファイル名に設定します。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Xamarin.Forms 3.0 が自動的にインスタンス化するため、 `ResourceDictionary`、これらの 2 つは外部`ResourceDictionary`タグは必要なくなりました。

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Add more resources here -->

        <ResourceDictionary Source="MyResourceDictionary.xaml" />

        <!-- Add more resources here -->

    </ContentPage.Resources>
    ...
</ContentPage>
```

この新しい構文は_いない_をインスタンス化、`MyResourceDictionary`クラスです。 代わりに、XAML ファイルを参照します。 分離コード ファイルの理由を (**MyResourceDictionary.xaml.cs**) は必要なくなりました。 削除することも、`x:Class`のルート タグから属性を**MyResourceDictionary.xaml**ファイル。 

## <a name="summary"></a>まとめ

この記事の内容を作成および使用する方法を説明した、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、方法およびリソース ディクショナリをマージします。 A`ResourceDictionary`リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用を許可します。

## <a name="related-links"></a>関連リンク

- [リソース ディクショナリ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
