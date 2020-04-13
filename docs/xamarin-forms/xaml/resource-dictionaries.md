---
title: Xamarin.Forms リソース ディクショナリ
description: Xamarin.Forms XAML リソースは、Xamarin.Forms アプリケーション全体で共有および再利用できるオブジェクトです。
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.custom: video
ms.openlocfilehash: 8dd3c7f36ddd436a812927816a1326dbb7c48341
ms.sourcegitcommit: ee9e48e2ec643915f42a6c1641077970ae20cb17
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/01/2020
ms.locfileid: "80523217"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.Forms リソース ディクショナリ

[![サンプルの](~/media/shared/download.png)ダウンロード サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

A[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は、Xamarin.Forms アプリケーションで使用されるリソースのリポジトリです。 一般的なリソースには、[スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)、データ[テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)、色、およびコンバータ`ResourceDictionary`などがあります。

XAML では、 に格納されているリソース[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を参照し、 または`StaticResource``DynamicResource`マークアップ拡張機能を使用して要素に適用できます。 C# では、リソースを a で`ResourceDictionary`定義し、文字列ベースのインデクサーを使用して要素に参照および適用することもできます。 ただし、共有オブジェクトはフィールドまたはプロパティとして格納`ResourceDictionary`でき、最初に辞書から取得することなく直接アクセスできるため、C# で a を使用する利点はほとんどありません。

## <a name="create-resources-in-xaml"></a>XAML でリソースを作成する

すべての[`VisualElement`](xref:Xamarin.Forms.VisualElement)派生オブジェクトには[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)プロパティがあり、これはリソースを[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)含むことができるプロパティです。 同様に[`Application`](xref:Xamarin.Forms.Application)、派生オブジェクトには[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)プロパティがあり、これはリソースを[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)含むことができます。

Xamarin.Forms アプリケーションには、 から[`Application`](xref:Xamarin.Forms.Application)派生するクラスのみが含まれていますが、多くの場合、ページ、[`VisualElement`](xref:Xamarin.Forms.VisualElement)レイアウト、コントロールなど、 から派生する多くのクラスを使用します。 これらのオブジェクトのプロパティは、その`Resources`リソースを含む[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)リソースに設定できます。 リソースを使用できる特定の`ResourceDictionary`影響を与える場所を選択する:

- ビューにアタッチ`ResourceDictionary`されているリソース、[`Button`](xref:Xamarin.Forms.Button)または[`Label`](xref:Xamarin.Forms.Label)ビュー内のリソースは、その特定のオブジェクトにのみ適用できます。
- レイアウトに`ResourceDictionary`アタッチされているリソース(または[`StackLayout`](xref:Xamarin.Forms.StackLayout)[`Grid`](xref:Xamarin.Forms.Grid)など)は、レイアウトとそのレイアウトのすべての子に適用できます。
- ページ レベル`ResourceDictionary`で定義されているリソースは、ページとそのすべての子に適用できます。
- アプリケーション レベル`ResourceDictionary`で定義されたリソースは、アプリケーション全体に適用できます。

暗黙的なスタイルを除き、リソース ディクショナリ内の各リソースには、`x:Key`属性で定義された一意の文字列キーが必要です。

次の XAML は`ResourceDictionary`**、App.xaml**ファイル内のアプリケーション レベルで定義されたリソースを示しています。

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

この例では、リソース ディクショナリはリソース[`Thickness`](xref:Xamarin.Forms.Thickness)、複数[`Color`](xref:Xamarin.Forms.Color)のリソース、および 2[`Style`](xref:Xamarin.Forms.Style)つの暗黙のリソースを定義します。 クラスの`App`詳細については[、「Xamarin.Forms アプリ クラス](~/xamarin-forms/app-fundamentals/application-class.md)」を参照してください。

> [!NOTE]
> また、すべてのリソースを明示的`ResourceDictionary`なタグの間に配置することも有効です。 ただし、Xamarin.Forms 3.0`ResourceDictionary`以降ではタグは必要ありません。 代わりに、`ResourceDictionary`オブジェクトが自動的に作成され、プロパティ要素タグの間に直接`Resources`リソースを挿入できます。

## <a name="consume-resources-in-xaml"></a>XAML でリソースを消費する

各リソースには、 属性を`x:Key`使用して指定されたキーがあり、 でのディクショナリ`ResourceDictionary`キーになります。 キーは、 または[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)[`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)[`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)マークアップ拡張機能を使用してリソースを参照するために使用されます。

マークアップ`StaticResource`拡張機能は、どちらもディクショナリ`DynamicResource`キーを使用してリソース ディクショナリの値を参照するという点でマークアップ拡張機能に似ています。 ただし、マークアップ拡張機能`StaticResource`は単一のディクショナリ検索を実行します`DynamicResource`が、マークアップ拡張機能はディクショナリ キーへのリンクを保持します。 したがって、キーに関連付けられているディクショナリ エントリが置き換えられた場合、変更はビジュアル要素に適用されます。 これにより、ランタイム リソースの変更をアプリケーションで行うことができます。 マークアップ拡張機能の詳細については、「 [XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。

次の XAML の例では、リソースを消費し、追加のリソース[`StackLayout`](xref:Xamarin.Forms.StackLayout)を定義する方法を示しています。

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

この例では、オブジェクト[`ContentPage`](xref:Xamarin.Forms.ContentPage)は、アプリケーション レベルのリソース ディクショナリで定義された暗黙的なスタイルを使用します。 オブジェクト[`StackLayout`](xref:Xamarin.Forms.StackLayout)は、アプリケーション`PageMargin`レベルのリソース ディクショナリで定義されたリソースを消費[`Button`](xref:Xamarin.Forms.Button)し、オブジェクトはリソース ディクショナリで定義[`StackLayout`](xref:Xamarin.Forms.StackLayout)された暗黙的なスタイルを使用します。 この結果、次のスクリーンショットに示すような外観が表示されます。

[![リソースディクショナリ リソースの使用](resource-dictionaries-images/consuming.png "リソースディクショナリ リソースの使用")](resource-dictionaries-images/consuming-large.png#lightbox "リソースディクショナリ リソースの使用")

> [!IMPORTANT]
> 単一のページに固有のリソースは、アプリケーション レベルのリソース ディクショナリに含めるべきではないため、ページで必要とされる場合ではなく、アプリケーションの起動時に解析されます。 詳細については、「[アプリケーション リソース ディクショナリのサイズを縮小する](~/xamarin-forms/deploy-test/performance.md)」を参照してください。

## <a name="resource-lookup-behavior"></a>リソースの検索動作

リソースがまたは[`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)[`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)マークアップ拡張機能で参照されている場合、次の参照プロセスが発生します。

- 要求されたキーは、プロパティを設定する要素のリソース ディクショナリ (存在する場合) でチェックされます。 要求されたキーが見つかった場合、その値が返され、ルックアップ プロセスが終了します。
- 一致が見つからない場合、検索プロセスはビジュアル ツリーを上方向に検索し、各親要素のリソース ディクショナリをチェックします。 要求されたキーが見つかった場合、その値が返され、ルックアップ プロセスが終了します。 それ以外の場合、プロセスはルート要素に到達するまで上方向に続行されます。
- ルート要素で一致が見つからない場合は、アプリケーション レベルのリソース ディクショナリが調べられます。
- 一致が見つからない場合は、 が`XamlParseException`スローされます。

したがって、XAML パーサーは、 または`StaticResource``DynamicResource`マークアップ拡張機能を検出すると、最初に見つかった一致を使用してビジュアル ツリーを順に移動して、一致するキーを検索します。 この検索がページで終了してもキーが見つからない場合、XAML パーサーはオブジェクトにアタッチされているキー`ResourceDictionary`を`App`検索します。 キーがまだ見つからない場合は、例外がスローされます。

## <a name="override-resources"></a>リソースの上書き

リソースがキーを共有する場合、ビジュアル ツリーで下位に定義されたリソースは、上位に定義されたリソースよりも優先されます。 たとえば、アプリケーション レベル`AppBackgroundColor`でリソース`AliceBlue`を に設定すると、ページ レベル`AppBackgroundColor`のリソースが に`Teal`設定され、上書きされます。 同様に、ページ`AppBackgroundColor`レベルのリソースは、コントロール レベル`AppBackgroundColor`のリソースによってオーバーライドされます。

## <a name="stand-alone-resource-dictionaries"></a>スタンドアロン リソース ディクショナリ

派生[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)したクラスは、独立したスタンドアロン ファイルに含まれることもあります。 結果ファイルはアプリケーション間で共有できます。

このようなファイルを作成するには、新しい**コンテンツ ビュー**または**コンテンツ ページ**アイテムをプロジェクトに追加します (ただし、C# ファイルのみを含む**コンテンツ ビュー**またはコンテンツ**ページ**は追加しません)。 分離コード ファイルを削除し、XAML ファイルで基本クラスの名前を 変更[`ContentView`](xref:Xamarin.Forms.ContentView)するか[`ContentPage`](xref:Xamarin.Forms.ContentPage)、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)にします。 また、ファイルの`x:Class`ルート タグから属性を削除します。

次の XAML の[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)例は、名前付き**の MyResourceDictionary.xaml**を示しています。

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

この例では、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)型のオブジェクトである 1 つのリソースが含[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)まれています。 **MyResourceDictionary.xaml**は、別のリソース ディクショナリにマージすることで使用できます。

## <a name="merged-resource-dictionaries"></a>結合されたリソース ディクショナリ

マージされたリソース ディクショナリは、1[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)つまたは複数の`ResourceDictionary`オブジェクトを結合して別の オブジェクトにします。

### <a name="merge-local-resource-dictionaries"></a>ローカル リソース ディクショナリの結合

ローカル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)ファイルを別`ResourceDictionary`の`ResourceDictionary`ファイルにマージするには、リソースを[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)持つ XAML ファイルのファイル名にプロパティが設定されているオブジェクトを作成します。

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

この構文では、クラスは`MyResourceDictionary`インスタンス化されません。 代わりに、XAML ファイルを参照します。 そのため、プロパティを[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)設定するときに、分離コード ファイルは必要ありませんし、`x:Class`属性を**MyResourceDictionary.xaml**ファイルのルート タグから削除できます。

> [!IMPORTANT]
> プロパティ[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)は XAML からのみ設定できます。

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>他のアセンブリからリソース ディクショナリをマージする

A[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を のプロパティに`ResourceDictionary`追加して、別の[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)プロパティに`ResourceDictionary`マージすることもできます。 この方法では、リソース ディクショナリをマージできます。 外部アセンブリからリソース ディクショナリをマージするには、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)ビルド アクションを**EmbeddedResource**に設定し、分離コード ファイルを持ち、ファイルのルート`x:Class`タグに属性を定義する必要があります。

> [!WARNING]
> この[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)クラスは、プロパティ[`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith)も定義します。 ただし、このプロパティは非推奨となっており、使用する必要はありません。

次のコード例は、ページ レベル[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)のコレクションに追加される 2 つのリソース ディクショナリを示しています。

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

この例では、同じアセンブリのリソース ディクショナリと外部アセンブリのリソース ディクショナリがページ レベルのリソース ディクショナリにマージされます。 また、プロパティ要素タグ内の`ResourceDictionary`[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)他のオブジェクトや、それらのタグの外側にある他のリソースを追加することもできます。

> [!IMPORTANT]
> にできるプロパティ要素タグ`MergedDictionaries`[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は 1 つだけですが、必要な数`ResourceDictionary`だけオブジェクトをそこに配置できます。

マージされた[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)リソースが同`x:Key`じ属性値を共有する場合、Xamarin.Forms は次のリソース優先順位を使用します。

1. リソース ディクショナリに対してローカルなリソース。
1. `MergedDictionaries`コレクションを介してマージされたリソース ディクショナリに含まれるリソースは、プロパティに表示される逆の`MergedDictionaries`順序で返されます。

> [!NOTE]
> アプリケーションに複数の大きなリソース ディクショナリが含まれている場合、リソース ディクショナリの検索は、計算に負荷がかかるタスクになる可能性があります。 したがって、不要な検索を避けるため、アプリケーションの各ページで使用されるのは、そのページに適したリソース ディクショナリのみです。

## <a name="related-links"></a>関連リンク

- [リソース ディクショナリ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms のスタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
