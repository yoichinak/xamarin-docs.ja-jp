---
title: リソース ディクショナリ
description: XAML リソースは、共有し、Xamarin.Forms アプリケーション全体で再利用が可能なオブジェクトです。
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2019
ms.custom: video
ms.openlocfilehash: 7c0fffbe626a740c15d85b1277c5158a5e564a15
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228077"
---
# <a name="resource-dictionaries"></a>リソース ディクショナリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_XAML リソースは、Xamarin. フォームアプリケーション全体で共有および再利用できるオブジェクトの定義です。これらのリソースオブジェクトは、リソースディクショナリに格納されます。_

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Xamarin.Forms アプリケーションで使用されるリソースのリポジトリです。 一般的なリソースが含まれている、`ResourceDictionary`含める[スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)、[データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)色、およびコンバーター。

XAML に格納されているリソースで、`ResourceDictionary`取得して使用して要素に適用できるし、`StaticResource`マークアップ拡張機能。 C# の場合は、リソース定義することもに、`ResourceDictionary`を取得して、文字列ベースのインデクサーを使用して要素に適用します。 ただし、使用する利点のほとんどは、`ResourceDictionary`で c# の場合は、共有オブジェクトは単にフィールドまたはプロパティとして格納されているし、しない直接アクセスに最初から取得ディクショナリ。

## <a name="create-and-consume-a-resourcedictionary"></a>ResourceDictionary の作成と使用

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

[![ResourceDictionary リソースの消費](resource-dictionaries-images/screenshots-sml.png)](resource-dictionaries-images/screenshots.png#lightbox)

> [!NOTE]
> 1 つのページに固有のリソースは、アプリケーション レベルのリソース ディクショナリをそのため、ページで必要なときに、リソースの代わりに、アプリケーションの起動時に解析されますに含めることはできません。 詳細については、次を参照してください。[アプリケーション リソース ディクショナリのサイズを減らす](~/xamarin-forms/deploy-test/performance.md)します。

## <a name="override-resources"></a>リソースの上書き

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

[![ResourceDictionary リソースの上書き](resource-dictionaries-images/overridding-screenshots-sml.png)](resource-dictionaries-images/overridding-screenshots.png#lightbox)

ただし、注意のバック グラウンド バー、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)黄色のまま、ため、 [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)の値に設定されて、`PageBackgroundColor`アプリケーションで定義されているリソースレベル`ResourceDictionary`します。

`ResourceDictionary`優先順位を考慮するもう1つの方法を次に示します。XAML パーサーは、を`StaticResource`検出すると、見つかった最初の一致を使用して、ビジュアルツリーの上に移動することで、一致するキーを検索します。 XAML パーサーが検索ページでこの検索を終了し、キーもまだ検出された場合、`ResourceDictionary`にアタッチされている、`App`オブジェクト。 でも、キーが存在しない場合は、例外が発生します。

## <a name="stand-alone-resource-dictionaries"></a>スタンドアロンリソースディクショナリ

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

ただし、この方法にはいくつかの制限があります。のプロパティは`Resources` 、この1つ`ResourceDictionary`だけを参照します。`ContentPage` などの他のオプションが必要なほとんどの場合、`ResourceDictionary`インスタンスとおそらく他のリソースもします。

このタスクでは、マージされたリソース ディクショナリが必要です。

## <a name="merged-resource-dictionaries"></a>マージされたリソースディクショナリ

マージされたリソースディクショナリは[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 、1つ`ResourceDictionary`または複数のオブジェクトを別のオブジェクトに結合します。

### <a name="merge-local-resource-dictionaries"></a>ローカルリソースディクショナリのマージ

ローカル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を別`ResourceDictionary`のにマージするには、 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)プロパティにリソースを含む XAML ファイルのファイル名を設定します。

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

この構文は、クラスを`MyResourceDictionary`インスタンス化しません。 代わりに、XAML ファイルを参照します。 そのため、 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)プロパティを設定するときに、分離コードファイル (**MyResourceDictionary.xaml.cs**) は必要なく、属性は`x:Class` **myresourcedictionary .xaml**ファイルのルートタグから削除できます。 さらに、この方法を使用してリソースディクショナリをマージする場合、Xamarin に[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)よってが自動的`ResourceDictionary`にインスタンス化されるため、外側のタグは必要ありません。

> [!IMPORTANT]
> プロパティ[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)は、XAML からのみ設定できます。

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>他のアセンブリからリソースディクショナリをマージする

は、 `ResourceDictionary` [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) のプロパティに追加すること`ResourceDictionary`で、別のにマージすることもできます。 この手法では、リソースディクショナリが配置されているアセンブリに関係なく、リソースディクショナリをマージできます。

> [!IMPORTANT]
> クラス[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は、 [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith)プロパティも定義します。 ただし、このプロパティは非推奨とされているため、使用できなくなりました。

次のコード例は`MyResourceDictionary` 、ページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)の[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)コレクションに追加されることを示しています。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo">
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

次の例は、同じ`MyResourceDictionary`アセンブリに存在するのインスタンスを[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加しています。 さらに、他のアセンブリ、 `ResourceDictionary` [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)プロパティ要素タグ内の他のオブジェクト、その他のリソース (タグ以外) からリソースディクショナリを追加することもできます。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo"
             xmlns:theme="clr-namespace:MyThemes;assembly=MyThemes">
    <ContentPage.Resources>
        <ResourceDictionary>
            <!-- Add more resources here -->
            <ResourceDictionary.MergedDictionaries>
                <!-- Add more resource dictionaries here -->
                <local:MyResourceDictionary />
                <theme:LightTheme />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Add more resources here -->
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

> [!IMPORTANT]
> にはプロパティ要素タグ`MergedDictionaries`を1つだけ含めることができますが、そこに`ResourceDictionary`必要な数のオブジェクトを含めることができます。 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)

マージする際に[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)リソースが同じ共有`x:Key`属性値は、Xamarin.Forms は、次のリソースの優先順位を使用します。

1. リソース ディクショナリのローカル リソース。
1. 使用してマージされたリソース ディクショナリに含まれるリソース、`MergedDictionaries`に登録されている逆の順序で、コレクション、`MergedDictionaries`プロパティ。

> [!NOTE]
> アプリケーションが複数含まれている場合に、負荷の高いタスク リソース ディクショナリを検索することができますサイズの大きいリソース ディクショナリ。 そのため、不要な検索を避けるため、アプリケーション内の各ページだけが、ページに適切なリソース ディクショナリを使用するを確認する必要があります。

## <a name="related-links"></a>関連リンク

- [リソース ディクショナリ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
