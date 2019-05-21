---
title: リソース ディクショナリ
description: XAML リソースは、共有し、Xamarin.Forms アプリケーション全体で再利用が可能なオブジェクトです。
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 8b5a610cbd87ca9e80272b6be8e726e4fd323dd0
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925611"
---
# <a name="resource-dictionaries"></a>リソース ディクショナリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/XAML/ResourceDictionaries/)

_XAML リソースは、共有し、Xamarin.Forms アプリケーション全体で再利用が可能なオブジェクトの定義です。_

これらのリソース オブジェクトは、リソース ディクショナリに格納されます。 この記事では、リソース ディクショナリをマージする方法とを作成し、リソース ディクショナリを使用する方法について説明します。

## <a name="overview"></a>概要

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Xamarin.Forms アプリケーションで使用されるリソースのリポジトリです。 一般的なリソースが含まれている、`ResourceDictionary`含める[スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)、[データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)色、およびコンバーター。

XAML に格納されているリソースで、`ResourceDictionary`取得して使用して要素に適用できるし、`StaticResource`マークアップ拡張機能。 C# の場合は、リソース定義することもに、`ResourceDictionary`を取得して、文字列ベースのインデクサーを使用して要素に適用します。 ただし、使用する利点のほとんどは、`ResourceDictionary`で c# の場合は、共有オブジェクトは単にフィールドまたはプロパティとして格納されているし、しない直接アクセスに最初から取得ディクショナリ。

## <a name="creating-and-consuming-a-resourcedictionary"></a>作成および ResourceDictionary を使用します。

リソースが定義されている、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 、次のいずれかに設定されている`Resources`プロパティ。

- [ `Resources` ](xref:Xamarin.Forms.Application.Resources)から派生したクラスのプロパティ [`Application`](xref:Xamarin.Forms.Application)
- [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)から派生したクラスのプロパティ [`VisualElement`](xref:Xamarin.Forms.Application)

Xamarin.Forms のプログラムにはから派生したクラス 1 つだけにはが含まれています`Application`多くの場合を使用して、多くのクラスから派生するが、 `VisualElement`(ページ、レイアウト、コントロールなど)。 これらのオブジェクトのいずれかがその`Resources`プロパティに設定、`ResourceDictionary`します。 特定の配置場所を選択する`ResourceDictionary`影響は、リソースを使用できます。

- 内のリソースを`ResourceDictionary`など、ビューに接続される`Button`または`Label`のためこれは非常に便利なのみに、その特定のオブジェクトに適用できます。
- 内のリソースを`ResourceDictionary`などのレイアウトにアタッチされている`StackLayout`または`Grid`レイアウトとそのレイアウトのすべての子に適用できます。
- 内のリソースを`ResourceDictionary`定義されているページとそのすべての子ページにレベルは適用できます。
- 内のリソースを`ResourceDictionary`定義されているアプリケーションで、アプリケーション全体でレベルを適用できます。

次の XAML には、アプリケーション レベルで定義されているリソースが表示されます。`ResourceDictionary`で、 **App.xaml**標準 Xamarin.Forms プログラムの一部として作成されたファイル。

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

これは、`ResourceDictionary`定義 3 [ `Color` ](xref:Xamarin.Forms.Color)リソースと[ `Style` ](xref:Xamarin.Forms.Style)リソース。 詳細については、`App`クラスを参照してください[App クラス](~/xamarin-forms/app-fundamentals/application-class.md)します。

明示的な Xamarin.Forms 3.0 以降では`ResourceDictionary`タグは必要ありません。 `ResourceDictionary`オブジェクトが自動的に作成され、間で直接、リソースを挿入することができます、`Resources`プロパティ要素タグ。

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

各リソースを使用して指定されているキーには、`x:Key`属性には、ディクショナリ キーになりますが、`ResourceDictionary`します。 リソースを取得するキーが使用される、`ResourceDictionary`によって、 [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension)マークアップ拡張機能、内で定義された追加のリソースを示す次の XAML コードの例のように、 `StackLayout`:

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

最初の[ `Label` ](xref:Xamarin.Forms.Label)インスタンスを取得し、使用、`LabelPageHeadingStyle`アプリケーション レベルで定義されているリソース`ResourceDictionary`、2 つ目`Label`インスタンスの取得および使用、 `LabelNormalStyle`制御レベルで定義されているリソース`ResourceDictionary`します。 同様に、 [ `Button` ](xref:Xamarin.Forms.Button)インスタンスを取得し、使用、`NormalTextColor`アプリケーション レベルで定義されているリソース`ResourceDictionary`、および`MediumBoldText`制御レベルで定義されているリソース`ResourceDictionary`します。 次のスクリーン ショットに示すように外観が発生します。

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary リソースを消費して")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary リソースを消費")

> [!NOTE]
> 1 つのページに固有のリソースは、アプリケーション レベルのリソース ディクショナリをそのため、ページで必要なときに、リソースの代わりに、アプリケーションの起動時に解析されますに含めることはできません。 詳細については、次を参照してください。[アプリケーション リソース ディクショナリのサイズを減らす](~/xamarin-forms/deploy-test/performance.md)します。

## <a name="overriding-resources"></a>リソースのオーバーライド

ときに`ResourceDictionary`のリソースを共有`x:Key`属性値は、ビューの階層内で下に定義されているリソースは優先を以上定義されています。 たとえば、設定、`PageBackgroundColor`リソースを`Blue`アプリケーションでレベルがページ レベルで上書きされている`PageBackgroundColor`リソースに設定`Yellow`。 ページ レベルでは同様に、`PageBackgroundColor`制御レベルによってリソースが上書きされます`PageBackgroundColor`リソース。 この優先順位は次の XAML コード例について説明します。

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

元の`PageBackgroundColor`と`NormalTextColor`によって、アプリケーション レベルで定義されている、インスタンスが上書きされます、`PageBackgroundColor`と`NormalTextColor`ページ レベルで定義されているインスタンス。 したがって、ページの背景色が青と、次のスクリーン ショットに示すよう、ページ上のテキストが、黄色になります。

[![](resource-dictionaries-images/overridding-screenshots-sml.png "上書きする ResourceDictionary リソース")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary リソースをオーバーライドします。")

ただし、注意のバック グラウンド バー、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)黄色のまま、ため、 [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)の値に設定されて、`PageBackgroundColor`アプリケーションで定義されているリソースレベル`ResourceDictionary`します。

別の方法について考える`ResourceDictionary`よりも優先されます。XAML パーサーが検出した場合、 `StaticResource`、ビジュアル ツリーを介したを結ぶこと、一致するキーを検索、見つかった最初の一致を使用します。 XAML パーサーが検索ページでこの検索を終了し、キーもまだ検出された場合、`ResourceDictionary`にアタッチされている、`App`オブジェクト。 でも、キーが存在しない場合は、例外が発生します。

## <a name="stand-alone-resource-dictionaries"></a>スタンドアロンのリソース ディクショナリ

派生したクラス`ResourceDictionary`別のスタンドアロン ファイルにもできます。 (から派生したクラスより正確に`ResourceDictionary`通常必要がある、_ペア_ファイルの XAML ファイルの分離コード ファイル内のリソースが定義されているため、`InitializeComponent`呼び出しも必要です)。結果のファイルは、アプリケーション間で共有できます。

このようなファイルを作成するには、追加、新しい**コンテンツ ビュー**または**コンテンツ ページ**項目をプロジェクト (ではなく、**コンテンツ ビュー**または**コンテンツ ページ**でc# ファイルのみ)。 XAML ファイルと c# ファイルの両方でから基底クラスの名前を変更`ContentView`または`ContentPage`に`ResourceDictionary`します。 XAML ファイルでは、基底クラスの名前は、最上位要素です。

次の XAML の例は、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)という`MyResourceDictionary`:

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

これは、`ResourceDictionary`型のオブジェクトは、1 つのリソースを含む`DataTemplate`します。

インスタンスを作成できる`MyResourceDictionary`のペアの間に配置して`Resources`プロパティ要素タグをたとえばで、 `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

インスタンス`MyResourceDictionary`に設定されている、`Resources`のプロパティ、`ContentPage`オブジェクト。

ただし、このアプローチでは、いくつかの制限があります。`Resources`のプロパティ、`ContentPage`参照のみ`ResourceDictionary`します。 などの他のオプションが必要なほとんどの場合、`ResourceDictionary`インスタンスとおそらく他のリソースもします。

このタスクでは、マージされたリソース ディクショナリが必要です。

## <a name="merged-resource-dictionaries"></a>マージされたリソース ディクショナリ

マージされたリソース ディクショナリは、1 つ以上を組み合わせた`ResourceDictionary`インスタンス別に`ResourceDictionary`します。 これを行う XAML ファイルで設定して、 [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)アプリケーション、ページ、または制御レベルにマージする 1 つまたは複数のリソース ディクショナリにプロパティ`ResourceDictionary`します。

> [!IMPORTANT]
> `ResourceDictionary` 定義、 [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith)プロパティ。 このプロパティを使用しないでください。Xamarin.Forms の 3.0 の時点で廃止されました。

インスタンス`MyResourceDictionary`任意のアプリケーション、ページ、または制御レベルにマージできるように`ResourceDictionary`します。 次の XAML コード例はページ レベルにマージしていることを示しています`ResourceDictionary`を使用して、`MergedDictionaries`プロパティ。

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

そのマークアップは、のインスタンスのみを示しています。`MyResourceDictionary`に追加される、`ResourceDictionary`他を参照することもできますが、`ResourceDictionary`内のインスタンスで、`MergedDictionary`プロパティ要素タグ、およびそれらのタグの外部でのその他のリソース。

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

1 つのみ`MergedDictionaries`セクション、 `ResourceDictionary`、同数に配置できますが、`ResourceDictionary`そこでインスタンス化します。

マージする際に[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)リソースが同じ共有`x:Key`属性値は、Xamarin.Forms は、次のリソースの優先順位を使用します。

1. リソース ディクショナリのローカル リソース。
1. マージされたリソース ディクショナリに含まれるリソース経由で非推奨とされる[ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith)プロパティ。
1. 使用してマージされたリソース ディクショナリに含まれるリソース、`MergedDictionaries`に登録されている逆の順序で、コレクション、`MergedDictionaries`プロパティ。

> [!NOTE]
> アプリケーションが複数含まれている場合に、負荷の高いタスク リソース ディクショナリを検索することができますサイズの大きいリソース ディクショナリ。 そのため、不要な検索を避けるため、アプリケーション内の各ページだけが、ページに適切なリソース ディクショナリを使用するを確認する必要があります。

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Xamarin.Forms 3.0 でのマージの辞書

Xamarin.Forms 3.0 では、マージ プロセスで始まる[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)インスタンスはやや簡単かつ柔軟になりました。 `MergedDictionaries`プロパティ要素タグが必要ではありません。 代わりに、追加するリソース ディクショナリに別`ResourceDictionary`新しいタグ[ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source)プロパティがリソースで XAML ファイルのファイル名に設定します。

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

この新しい構文は_いない_をインスタンス化、`MyResourceDictionary`クラス。 代わりに、XAML ファイルを参照します。 分離コード ファイルの理由を (**MyResourceDictionary.xaml.cs**) は必要なくなりました。 削除することも、`x:Class`のルート タグから属性を**MyResourceDictionary.xaml**ファイル。

## <a name="summary"></a>まとめ

この記事で作成および使用する方法を説明した、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、およびリソース ディクショナリを結合する方法。 A`ResourceDictionary`リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用を許可します。

## <a name="related-links"></a>関連リンク

- [リソース ディクショナリ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/ResourceDictionaries/)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
