---
title: リソース ディクショナリ
description: XAML リソースは、共有し、Xamarin.Forms アプリケーション全体で再利用が可能なオブジェクトです。
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2020
ms.custom: video
ms.openlocfilehash: f5266049e1498d902864185a61cba6d3730f9594
ms.sourcegitcommit: 2836f2003a5b745b042ee6003a3d6a11b9139e44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618869"
---
# <a name="resource-dictionaries"></a>リソース ディクショナリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_XAML リソースは、Xamarin. フォームアプリケーション全体で共有および再利用できるオブジェクトの定義です。これらのリソースオブジェクトは、リソースディクショナリに格納されます。_

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は、Xamarin. フォームアプリケーションで使用されるリソースのリポジトリです。 `ResourceDictionary` に格納されている一般的なリソースには、[スタイル](~/xamarin-forms/user-interface/styles/index.md)、[コントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)、[データテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)、色、およびコンバーターがあります。

XAML では、`ResourceDictionary` に格納されているリソースを取得して、`StaticResource` マークアップ拡張機能を使用して要素に適用できます。 でC#は、リソースを `ResourceDictionary` で定義してから、文字列ベースのインデクサーを使用して取得し、要素に適用することもできます。 ただし、でC#は `ResourceDictionary` を使用する利点はほとんどありません。共有オブジェクトは単にフィールドまたはプロパティとして格納でき、最初にディクショナリから取得しなくても直接アクセスできます。

## <a name="create-and-consume-a-resourcedictionary"></a>ResourceDictionary の作成と使用

リソースは[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)で定義され、次のいずれかの `Resources` プロパティに設定されます。

- から派生した任意のクラスの[`Resources`](xref:Xamarin.Forms.Application.Resources)プロパティ[`Application`](xref:Xamarin.Forms.Application)
- から派生した任意のクラスの[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)プロパティ[`VisualElement`](xref:Xamarin.Forms.Application)

Xamarin. Forms プログラムには `Application` から派生したクラスが1つだけ含まれていますが、多くの場合、ページ、レイアウト、コントロールなど、`VisualElement`から派生した多くのクラスを使用します。 これらのオブジェクトのいずれでも、`Resources` プロパティを `ResourceDictionary`に設定できます。 特定の `ResourceDictionary` をどこに配置するかを選択すると、リソースをどこで使用できるかが決まります。

- `Button` や `Label` などのビューにアタッチされている `ResourceDictionary` 内のリソースは、その特定のオブジェクトにのみ適用できます。そのため、これはあまり便利ではありません。
- `StackLayout` や `Grid` などのレイアウトにアタッチされている `ResourceDictionary` 内のリソースは、レイアウトとそのレイアウトのすべての子に適用できます。
- ページレベルで定義されている `ResourceDictionary` 内のリソースは、ページおよびそのすべての子に適用できます。
- アプリケーションレベルで定義されている `ResourceDictionary` 内のリソースは、アプリケーション全体で適用できます。

次の XAML は、標準の Xamarin. Forms プログラムの一部として作成された**app.xaml**ファイル内の `ResourceDictionary` アプリケーションレベルで定義されたリソースを示しています。

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

この `ResourceDictionary` では、3つの[`Color`](xref:Xamarin.Forms.Color)リソースと[`Style`](xref:Xamarin.Forms.Style)リソースを定義します。 `App` クラスの詳細については、「 [App クラス](~/xamarin-forms/app-fundamentals/application-class.md)」を参照してください。

Xamarin. Forms 3.0 以降では、明示的な `ResourceDictionary` タグは必要ありません。 `ResourceDictionary` オブジェクトは自動的に作成され、`Resources` のプロパティ要素タグの間に直接リソースを挿入できます。

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

各リソースには、`x:Key` 属性を使用して指定されたキーがあります。これは、`ResourceDictionary`のディクショナリキーになります。 キーは、`StackLayout`内で定義されている追加のリソースを示す次の XAML コード例に示すように、 [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)マークアップ拡張機能によって `ResourceDictionary` からリソースを取得するために使用されます。

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

最初の[`Label`](xref:Xamarin.Forms.Label)インスタンスは、アプリケーションレベル `ResourceDictionary`で定義されている `LabelPageHeadingStyle` リソースを取得して使用します。2つ目の `Label` インスタンスは、コントロールレベル `LabelNormalStyle` に定義されている `ResourceDictionary`リソースを取得して使用します。 同様に、 [`Button`](xref:Xamarin.Forms.Button)インスタンスは、アプリケーションレベル `ResourceDictionary`で定義されている `NormalTextColor` リソースと、コントロールレベル `ResourceDictionary`で定義されている `MediumBoldText` リソースを取得して使用します。 これで、次のスクリーンショットのような結果になります。

[ResourceDictionary リソースを消費している ![](resource-dictionaries-images/screenshots-sml.png)](resource-dictionaries-images/screenshots.png#lightbox)

> [!NOTE]
> 1 つのページに固有のリソースは、アプリケーション レベルのリソース ディクショナリをそのため、ページで必要なときに、リソースの代わりに、アプリケーションの起動時に解析されますに含めることはできません。 詳細については、「[アプリケーションリソースディクショナリのサイズを減らす](~/xamarin-forms/deploy-test/performance.md)」を参照してください。

## <a name="override-resources"></a>リソースの上書き

`ResourceDictionary` リソースが `x:Key` 属性値を共有すると、ビュー階層で定義されているリソースよりも低いリソースが定義されているリソースよりも優先されます。 たとえば、`PageBackgroundColor` リソースをアプリケーションレベルで `Blue` に設定すると、`Yellow`に設定されたページレベル `PageBackgroundColor` リソースが上書きされます。 同様に、ページレベルの `PageBackgroundColor` リソースは、コントロールレベル `PageBackgroundColor` リソースによってオーバーライドされます。 この優先順位は次の XAML コード例について説明します。

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

アプリケーションレベルで定義されている元の `PageBackgroundColor` インスタンスと `NormalTextColor` インスタンスは、ページレベルで定義されている `PageBackgroundColor` および `NormalTextColor` インスタンスによってオーバーライドされます。 したがって、ページの背景色が青と、次のスクリーン ショットに示すよう、ページ上のテキストが、黄色になります。

[ResourceDictionary リソースのオーバーライド ![](resource-dictionaries-images/overridding-screenshots-sml.png)](resource-dictionaries-images/overridding-screenshots.png#lightbox)

ただし、 [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)プロパティは、アプリケーションレベル `ResourceDictionary`で定義されている `PageBackgroundColor` リソースの値に設定されているため、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)の背景バーは黄色のままになっていることに注意してください。

次に、`ResourceDictionary` の優先順位について検討するもう1つの方法を示します。 XAML パーサーが `StaticResource`を検出すると、見つかった最初の一致を使用して、ビジュアルツリー内を移動することで一致するキーが検索されます。 この検索がページで終了しても、キーがまだ見つからない場合は、XAML パーサーによって、`App` オブジェクトにアタッチされている `ResourceDictionary` が検索されます。 でも、キーが存在しない場合は、例外が発生します。

## <a name="stand-alone-resource-dictionaries"></a>スタンドアロンリソースディクショナリ

`ResourceDictionary` から派生したクラスは、独立したスタンドアロンファイルにも存在できます。 (正確には、`ResourceDictionary` から派生したクラスには、通常、ファイルの_ペア_が必要です。これは、リソースが XAML ファイルで定義されていても、`InitializeComponent` 呼び出しを伴う分離コードファイルが必要なためです)。結果ファイルは、アプリケーション間で共有できます。

このようなファイルを作成するには、新しい**コンテンツビュー**または**コンテンツページ**アイテムをプロジェクトに追加します (ただし、 C#ファイルが含まれている**コンテンツビュー**や**コンテンツページ**は除く)。 XAML ファイルとC#ファイルの両方で、基底クラスの名前を `ContentView` または `ContentPage` から `ResourceDictionary`に変更します。 XAML ファイルでは、基底クラスの名前は、最上位要素です。

次の XAML の例は、`MyResourceDictionary`という名前の[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を示しています。

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

この `ResourceDictionary` には、`DataTemplate`型のオブジェクトである1つのリソースが含まれています。

`MyResourceDictionary` インスタンス化するには、たとえば `ContentPage`内の `Resources` のプロパティ要素タグのペアの間に配置します。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

`MyResourceDictionary` のインスタンスは、`ContentPage` オブジェクトの `Resources` プロパティに設定されます。

ただし、この方法にはいくつかの制限があります。 `ContentPage` の `Resources` プロパティは、この1つの `ResourceDictionary`のみを参照します。 ほとんどの場合、他の `ResourceDictionary` インスタンスとその他のリソースも含めるオプションが必要です。

このタスクでは、マージされたリソース ディクショナリが必要です。

## <a name="merged-resource-dictionaries"></a>結合されたリソース ディクショナリ

マージされたリソースディクショナリは、1つ以上の[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)オブジェクトを別の `ResourceDictionary`に結合します。

### <a name="merge-local-resource-dictionaries"></a>ローカルリソースディクショナリのマージ

ローカル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を別の `ResourceDictionary` にマージするには、 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)プロパティに、リソースを含む XAML ファイルのファイル名を設定します。

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

この構文では、`MyResourceDictionary` クラスはインスタンス化されません。 代わりに、XAML ファイルを参照します。 このため、 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)プロパティを設定するときに、分離コードファイル (**MyResourceDictionary.xaml.cs**) は必要なく、`x:Class` 属性は**myresourcedictionary .xaml**ファイルのルートタグから削除できます。 また、この方法を使用してリソースディクショナリをマージする場合、Xamarin. Forms は[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を自動的にインスタンス化するため、外側の `ResourceDictionary` タグは必要ありません。

> [!IMPORTANT]
> [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)プロパティは、XAML からのみ設定できます。

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>他のアセンブリからリソースディクショナリをマージする

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は、`ResourceDictionary`の[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)プロパティに追加することで、別の `ResourceDictionary` にマージすることもできます。 この手法では、リソースディクショナリが配置されているアセンブリに関係なく、リソースディクショナリをマージできます。

> [!IMPORTANT]
> [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)クラスは、 [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith)プロパティも定義します。 ただし、このプロパティは非推奨とされているため、使用できなくなりました。

次のコード例は、ページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)の[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)コレクションに追加される `MyResourceDictionary` を示しています。

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

次の例では、同じアセンブリに存在する `MyResourceDictionary`のインスタンスを[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加しています。 さらに、他のアセンブリ、 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)プロパティ要素タグ内の他の `ResourceDictionary` オブジェクト、およびこれらのタグの外部にある他のリソースからリソースディクショナリを追加することもできます。

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

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を外部アセンブリに配置する場合は、ビルドアクションが**EmbeddedResource**に設定されていることを確認します。 また、分離コードファイルがあることを確認します。

> [!IMPORTANT]
> [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)には `MergedDictionaries` プロパティ要素タグが1つしか存在できませんが、必要な数の `ResourceDictionary` オブジェクトを含めることができます。

結合された[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)リソースが同じ `x:Key` 属性値を共有する場合、Xamarin は次のリソース優先順位を使用します。

1. リソース ディクショナリのローカル リソース。
1. `MergedDictionaries` コレクションによってマージされたリソースディクショナリに含まれているリソースは、逆の順序で、`MergedDictionaries` プロパティに一覧表示されます。

> [!NOTE]
> アプリケーションが複数含まれている場合に、負荷の高いタスク リソース ディクショナリを検索することができますサイズの大きいリソース ディクショナリ。 そのため、不要な検索を避けるため、アプリケーション内の各ページだけが、ページに適切なリソース ディクショナリを使用するを確認する必要があります。

## <a name="related-links"></a>関連リンク

- [リソースディクショナリ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
