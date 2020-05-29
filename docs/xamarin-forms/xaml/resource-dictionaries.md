---
title: Xamarin.Formsリソースディクショナリ
description: Xamarin.FormsXAML リソースは、アプリケーション全体で共有および再利用できるオブジェクトです Xamarin.Forms 。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: ''
ms.openlocfilehash: a1c7cfd4a0f3549b11ac51dc13b40da552f6b758
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139413"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.Formsリソースディクショナリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は、アプリケーションによって使用されるリソースのリポジトリです Xamarin.Forms 。 に格納されている一般的なリソースには、 `ResourceDictionary` [スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)、[データテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)、色、およびコンバーターがあります。

XAML で [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `StaticResource` は、またはマークアップ拡張機能を使用して、に格納されているリソースを参照し、要素に適用でき `DynamicResource` ます。 C# では、リソースをで定義して `ResourceDictionary` から、文字列ベースのインデクサーを使用して要素に適用することもできます。 ただし、 `ResourceDictionary` C# では、共有オブジェクトをフィールドまたはプロパティとして格納し、最初にディクショナリから取得しなくても直接アクセスできるため、C# でを使用する利点はほとんどありません。

## <a name="create-resources-in-xaml"></a>XAML でリソースを作成する

すべての [`VisualElement`](xref:Xamarin.Forms.VisualElement) 派生オブジェクトには、プロパティがあり [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) ます。これは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) リソースを格納できるです。 同様に、 [`Application`](xref:Xamarin.Forms.Application) 派生オブジェクトにはプロパティがあり、 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) これは [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) リソースを格納できるです。

アプリケーションには Xamarin.Forms 、から派生したクラスのみが含まれ [`Application`](xref:Xamarin.Forms.Application) ますが、多くの場合 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 、ページ、レイアウト、コントロールなど、から派生した多くのクラスが使用されます。 これらのオブジェクトの `Resources` プロパティは、リソースを含むに設定でき [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 リソースを使用できる場所の特定の影響を選択し `ResourceDictionary` ます。

- やなどのビューにアタッチされているのリソースは、 `ResourceDictionary` [`Button`](xref:Xamarin.Forms.Button) [`Label`](xref:Xamarin.Forms.Label) その特定のオブジェクトにのみ適用できます。
- やなどのレイアウトにアタッチされたのリソースは、レイアウト `ResourceDictionary` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Grid`](xref:Xamarin.Forms.Grid) とそのレイアウトのすべての子に適用できます。
- `ResourceDictionary`ページレベルで定義されているのリソースは、ページおよびそのすべての子に適用できます。
- アプリケーション `ResourceDictionary` レベルで定義されているのリソースは、アプリケーション全体で適用できます。

暗黙的なスタイルを除き、リソースディクショナリ内の各リソースには、属性で定義された一意の文字列キーが必要 `x:Key` です。

次の XAML は、アプリケーションレベルで定義されている app.xaml ファイルのリソースを示して `ResourceDictionary` **い**ます。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.App">
    <Application.Resources>

        <Thickness x:Key="PageMargin">20</Thickness>

        <!-- Colors -->
        <Color x:Key="AppBackgroundColor">AliceBlue</Color>
        <Color x:Key="NavigationBarColor">#1976D2</Color>
        <Color x:Key="NavigationBarTextColor">White</Color>
        <Color x:Key="NormalTextColor">Black</Color>

        <!-- Implicit styles -->
        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{StaticResource NavigationBarColor}" />
            <Setter Property="BarTextColor"
                    Value="{StaticResource NavigationBarTextColor}" />
        </Style>

        <Style TargetType="{x:Type ContentPage}"
               ApplyToDerivedTypes="True">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>

    </Application.Resources>
</Application>
```

この例では、リソースディクショナリによって、 [`Thickness`](xref:Xamarin.Forms.Thickness) リソース、複数の [`Color`](xref:Xamarin.Forms.Color) リソース、および2つの暗黙的なリソースが定義されて [`Style`](xref:Xamarin.Forms.Style) います。 クラスの詳細については `App` 、「 [ Xamarin.Forms App class](~/xamarin-forms/app-fundamentals/application-class.md)」を参照してください。

> [!NOTE]
> また、明示的なタグの間にすべてのリソースを配置することもでき `ResourceDictionary` ます。 ただし、 Xamarin.Forms 3.0 以降、 `ResourceDictionary` タグは必要ありません。 代わりに、 `ResourceDictionary` オブジェクトが自動的に作成され、リソースをプロパティ要素タグの間に直接挿入でき `Resources` ます。

## <a name="consume-resources-in-xaml"></a>XAML でリソースを使用する

各リソースには、属性を使用して指定されたキーがあり `x:Key` ます。これは、のディクショナリキーになり `ResourceDictionary` ます。 キーは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) またはマークアップ拡張機能を使用してからリソースを参照するために使用され [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) ます。

`StaticResource`マークアップ拡張機能は、では、 `DynamicResource` ディクショナリキーを使用してリソースディクショナリから値を参照するという点で、マークアップ拡張機能に似ています。 ただし、 `StaticResource` マークアップ拡張機能は、単一の辞書参照を実行しますが、 `DynamicResource` マークアップ拡張機能はディクショナリキーへのリンクを保持します。 そのため、キーに関連付けられているディクショナリエントリが置換された場合、変更はビジュアル要素に適用されます。 これにより、アプリケーションでランタイムリソースの変更を行うことができます。 マークアップ拡張機能の詳細については、「 [XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。

次の XAML の例は、リソースを使用する方法と、で追加のリソースを定義する方法を示してい [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.HomePage"
             Title="Home Page">
    <StackLayout Margin="{StaticResource PageMargin}">
        <StackLayout.Resources>
            <!-- Implicit style -->
            <Style TargetType="Button">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </StackLayout.Resources>

        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries." />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked" />
    </StackLayout>
</ContentPage>
```

この例では、 [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトは、アプリケーションレベルのリソースディクショナリに定義されている暗黙的なスタイルを使用します。 オブジェクトは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) `PageMargin` アプリケーションレベルのリソースディクショナリに定義されているリソースを使用します。一方、オブジェクトは、 [`Button`](xref:Xamarin.Forms.Button) リソースディクショナリで定義されている暗黙的なスタイルを使用し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 この結果、次のスクリーンショットに示すような外観が表示されます。

[![ResourceDictionary リソースの消費](resource-dictionaries-images/consuming.png "ResourceDictionary リソースの消費")](resource-dictionaries-images/consuming-large.png#lightbox "ResourceDictionary リソースの消費")

> [!IMPORTANT]
> 1つのページに固有のリソースをアプリケーションレベルのリソースディクショナリに含めないでください。そのようなリソースは、ページで要求されるときではなく、アプリケーションの起動時に解析されます。 詳細については、「[アプリケーションリソースディクショナリのサイズを減らす](~/xamarin-forms/deploy-test/performance.md)」を参照してください。

## <a name="resource-lookup-behavior"></a>リソース参照の動作

[`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)またはマークアップ拡張機能を使用してリソースを参照すると、次の検索プロセスが実行され [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) ます。

- 要求されたキーは、プロパティを設定する要素について、リソースディクショナリ (存在する場合) で確認されます。 要求されたキーが見つかった場合は、その値が返され、参照プロセスが終了します。
- 一致するものが見つからない場合は、参照プロセスによってビジュアルツリーが上位に検索され、各親要素のリソースディクショナリがチェックされます。 要求されたキーが見つかった場合は、その値が返され、参照プロセスが終了します。 それ以外の場合、ルート要素に到達するまでプロセスは上位に進みます。
- ルート要素で一致するものが見つからない場合は、アプリケーションレベルのリソースディクショナリが検査されます。
- 一致するものがまだ見つからない場合は、 `XamlParseException` がスローされます。

そのため、XAML パーサーは `StaticResource` またはマークアップ拡張機能を検出すると、 `DynamicResource` 見つかった最初の一致を使用して、ビジュアルツリー内を移動することで一致するキーを検索します。 この検索がページで終了しても、キーがまだ見つからない場合、XAML パーサーはオブジェクトにアタッチされているを検索し `ResourceDictionary` `App` ます。 キーがまだ見つからない場合は、例外がスローされます。

## <a name="override-resources"></a>リソースの上書き

リソースがキーを共有する場合、ビジュアルツリーの下位に定義されているリソースは、より高いレベルで定義されているリソースよりも優先されます。 たとえば、 `AppBackgroundColor` リソースをアプリケーションレベルでに設定すると、 `AliceBlue` に設定されたページレベルリソースによってオーバーライドされ `AppBackgroundColor` `Teal` ます。 同様に、ページレベル `AppBackgroundColor` リソースは制御レベルリソースによってオーバーライドされ `AppBackgroundColor` ます。

## <a name="stand-alone-resource-dictionaries"></a>スタンドアロンリソースディクショナリ

から派生したクラスは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 独立したスタンドアロンファイルにすることもできます。 結果ファイルは、アプリケーション間で共有できます。

このようなファイルを作成するには、新しい**コンテンツビュー**または**コンテンツページ**アイテムをプロジェクトに追加します (ただし、C# ファイルのみを含む**コンテンツビュー**や**コンテンツページ**は追加しません)。 分離コードファイルを削除し、XAML ファイルで基底クラスの名前を [`ContentView`](xref:Xamarin.Forms.ContentView) またはからに変更し [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 また、 `x:Class` ファイルのルートタグから属性を削除します。

次の XAML の例は [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 、 **myresourcedictionary**という名前のを示しています。

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}"
                       TextColor="{StaticResource NormalTextColor}"
                       FontAttributes="Bold" />
                <Label Grid.Column="1"
                       Text="{Binding Age}"
                       TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2"
                       Text="{Binding Location}"
                       TextColor="{StaticResource NormalTextColor}"
                       HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

この例では、に [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 1 つのリソースが含まれています。これは、型のオブジェクトです [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。 **Myresourcedictionary**は、別のリソースディクショナリにマージすることによって使用できます。

## <a name="merged-resource-dictionaries"></a>結合されたリソース ディクショナリ

マージされたリソースディクショナリは、1つまたは複数 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) のオブジェクトを別のオブジェクトに結合 `ResourceDictionary` します。

### <a name="merge-local-resource-dictionaries"></a>ローカルリソースディクショナリのマージ

ローカル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ファイルを別のファイルにマージするには、 `ResourceDictionary` `ResourceDictionary` [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) プロパティがリソースを含む XAML ファイルのファイル名に設定されたオブジェクトを作成します。

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

この構文は、クラスをインスタンス化しません `MyResourceDictionary` 。 代わりに、XAML ファイルを参照します。 そのため、プロパティを設定するときに、 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) 分離コードファイルは必要ありません。また、属性は、 `x:Class` **myresourcedictionary .xaml**ファイルのルートタグから削除できます。

> [!IMPORTANT]
> プロパティは、 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) XAML からのみ設定できます。

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>他のアセンブリからリソースディクショナリをマージする

は [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `ResourceDictionary` 、のプロパティに追加することで、別のにマージすることもでき [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) `ResourceDictionary` ます。 この手法では、リソースディクショナリが配置されているアセンブリに関係なく、リソースディクショナリをマージできます。 外部アセンブリからリソースディクショナリをマージするには、が [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ビルドアクションを**EmbeddedResource**に設定し、分離コードファイルを持つようにし、 `x:Class` ファイルのルートタグに属性を定義する必要があります。

> [!WARNING]
> クラスは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) プロパティも定義し [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) ます。 ただし、このプロパティは非推奨とされているため、使用できなくなりました。

次のコード例は、ページレベルのコレクションに追加される2つのリソースディクショナリを示してい [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。

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

この例では、同じアセンブリからのリソースディクショナリと外部アセンブリのリソースディクショナリがページレベルリソースディクショナリにマージされます。 さらに、 `ResourceDictionary` [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) プロパティ要素タグやその他のリソース内の他のオブジェクトを追加することもできます。

> [!IMPORTANT]
> にはプロパティ要素タグを1つだけ含めることができ `MergedDictionaries` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ますが、必要に応じてその中にオブジェクトをいくつでも含めることができ `ResourceDictionary` ます。

マージさ [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) れたリソース `x:Key` が同一の属性値を共有する場合、は Xamarin.Forms 次のリソース優先順位を使用します。

1. リソースディクショナリに対してローカルのリソース。
1. コレクションを使用してマージされたリソースディクショナリに含まれるリソースは、 `MergedDictionaries` 逆の順序でプロパティに表示され `MergedDictionaries` ます。

> [!NOTE]
> アプリケーションに複数の大きなリソースディクショナリが含まれている場合、リソースディクショナリの検索には多くの計算作業が必要になることがあります。 そのため、不要な検索を回避するには、アプリケーションの各ページが、ページに適したリソースディクショナリのみを使用するようにしてください。

## <a name="related-links"></a>関連リンク

- [リソースディクショナリ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Formsスタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
