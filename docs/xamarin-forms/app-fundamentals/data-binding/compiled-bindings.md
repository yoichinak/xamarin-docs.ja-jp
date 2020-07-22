---
title: 'title: "Xamarin.Formsコンパイル済みのバインド" の説明:"この記事では、コンパイル済みのバインドを使用して、Xamarin.Forms アプリケーションでのデータ バインディングのパフォーマンスを向上させる方法について説明します。"'
description: 'ms.prod: xamarin ms.assetid:ABE6B7F7-875E-4402-A1D2-845CE374402B ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:09/18/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 094691796fed9653f2a2e468ccb1c33d1a408a49
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571559"
---
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms のコンパイル済みのバインド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

_コンパイル済みのバインドは、従来のバインドより迅速に解決されるため、Xamarin.Forms アプリケーションでのデータ バインディングのパフォーマンスが向上します。_

データ バインディングには、以下の主な 2 つの問題があります。

1. バインディング式はコンパイル時に検証されません。 代わりに、バインドは実行時に解決されます。 そのため、アプリケーションが予期したとおりに動作しない場合やエラーメッセージが表示される場合、実行時まで無効なバインドが検出されません。
1. これらはコスト効率が高くありません。 バインドは実行時に汎用オブジェクト検査 (リフレクション) を使用して解決され、これを行うオーバーヘッドはプラットフォームによって異なります。

コンパイル済みのバインドでは、実行時ではなく、コンパイル時にバインド式を解決することで、Xamarin.Forms アプリケーションでのデータ バインディングのパフォーマンスが向上します。 さらに、このバインド式のコンパイル時の検証では、無効なバインドがビルド エラーとして報告されるため、開発者のトラブルシューティング エクスペリエンスを向上させることができます。

コンパイル済みのバインドを使用するためのプロセスでは、次のことを行います。

1. XAML のコンパイルを有効にします。 XAML のコンパイルの詳細については、[XAML のコンパイル](~/xamarin-forms/xaml/xamlc.md)に関するページを参照してください。
1. [`VisualElement`](xref:Xamarin.Forms.VisualElement) の `x:DataType` 属性を、`VisualElement` とその子がバインドされるオブジェクトの型に設定します。

> [!NOTE]
> [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) が設定されているのと同じレベルで、ビュー階層に `x:DataType` 属性を設定することをお勧めします。 ただし、この属性は、ビュー階層の任意の場所で再定義できます。

コンパイルされたバインドを使用するには、`x:DataType` 属性を文字列リテラル、または `x:Type` マークアップ拡張機能を使用する型に設定する必要があります。 XAML のコンパイル時に、無効なバインド式はすべてビルド エラーとして報告されます。 しかし、XAML コンパイラでは、最初に見つかった無効なバインド式についてのみ、ビルド エラーが報告されます。 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) が XAML またはコードで設定されているかどうかに関係なく、`VisualElement` またはその子で定義されている有効なバインド式はすべてコンパイルされます。 バインド式をコンパイルすると、コンパイル済みのコードが生成され、*ソース* のプロパティから値が取得されて、マークアップで指定されている*ターゲット* のプロパティに設定されます。 さらに、バインド式に応じて、生成されるコードでは、*ソース* プロパティの値の変更が監視され、*ターゲット* プロパティが更新される場合があります。また、*ターゲット* から*ソース* に変更がプッシュ バックされる場合があります。

> [!IMPORTANT]
> コンパイル済みのバインドは現在、[`Source`](xref:Xamarin.Forms.Binding.Source) プロパティを定義するどのバインド式でも無効になっています。 これは、コンパイル時に解決できない、`x:Reference` マークアップ拡張を使用して、`Source` プロパティが常に設定されるためです。

## <a name="use-compiled-bindings"></a>コンパイル済みのバインドを使用する

**[Compiled Color Selector] (コンパイル済みのカラー セレクター)** ページでは、Xamarin.Forms ビューと viewmodel プロパティの間でのコンパイル済みのバインドの使用について説明します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorSelectorPage"
             Title="Compiled Color Selector">
    ...
    <StackLayout x:DataType="local:HslColorViewModel">
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>
        <BoxView Color="{Binding Color}"
                 ... />
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

ルートの [`StackLayout`](xref:Xamarin.Forms.StackLayout) では、`HslColorViewModel` をインスタンス化し、[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティのためのプロパティ要素タグ内の `Color` プロパティを初期化します。 また、このルートの `StackLayout` では `x:DataType` 属性を viewmodel 型として定義し、ルートの `StackLayout` ビュー階層内のバインド式がすべてコンパイルされることを示します。 これは、ビルド エラーとなる、存在しない viewmodel プロパティにバインドするためにバインド式のいずれかを変更することで、確認できます。 この例では、`x:DataType` 属性を文字列リテラルに設定しますが、`x:Type` マークアップ拡張機能を使用して型に設定することもできます。 `x:Type` マークアップ拡張機能の詳細については、「[x:Type マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension)」を参照してください。

> [!IMPORTANT]
> `x:DataType` 属性は、ビュー階層の任意の時点で再定義できます。

[`BoxView`](xref:Xamarin.Forms.BoxView)、[`Label`](xref:Xamarin.Forms.Label) 要素、および [`Slider`](xref:Xamarin.Forms.Slider) ビューでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) からバインド コンテキストを継承します。 これらのビューはすべてバインディング ターゲットであり、viewmodel のソース プロパティを参照します。 [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) プロパティ、および [`Label.Text`](xref:Xamarin.Forms.Label.Text) プロパティの場合、データ バインディングは `OneWay` となります。ビューのプロパティは viewmodel のプロパティから設定されます。 しかし、[`Slider.Value`](xref:Xamarin.Forms.Slider.Value) プロパティでは `TwoWay` バインドが使用されます。 これにより、各 `Slider` は viewmodel から設定できるようになり、また、viewmodel を各 `Slider` から設定できるようになります。

アプリケーションの最初の実行時に、[`BoxView`](xref:Xamarin.Forms.BoxView)、[`Label`](xref:Xamarin.Forms.Label) 要素、および [`Slider`](xref:Xamarin.Forms.Slider) 要素はすべて、viewmodel がインスタンス化されたときに設定された初期の `Color` プロパティに基づいて、viewmodel から設定されます。 これを次のスクリーンショットに示します。

[![コンパイルされたカラー セレクター](compiled-bindings-images/compiledcolorselector-small.png "コンパイルされたカラー セレクター")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "コンパイルされたカラー セレクター")

スライダーの操作に応じて、[`BoxView`](xref:Xamarin.Forms.BoxView) および [`Label`](xref:Xamarin.Forms.Label) 要素が更新されます。

このカラー セレクターの詳細については、「[ViewModel とプロパティ変更通知](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications)」を参照してください。

## <a name="use-compiled-bindings-in-a-datatemplate"></a>DataTemplate でコンパイル済みのバインドを使用する

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) でのバインドは、テンプレート化されているオブジェクトのコンテキストで解釈されます。 そのため、`DataTemplate` でコンパイル済みのバインドを使用するときには、`DataTemplate` で `x:DataType` 属性を使用して、そのデータ オブジェクトの型を宣言する必要があります。

**[Compiled Color List]\(コンパイル済みのカラー リスト\)** ページでは、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) でのコンパイル済みのバインドの使用について説明します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorListPage"
             Title="Compiled Color List">
    <Grid>
        ...
        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  ... >
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:NamedColor">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     ... />
                            <Label Text="{Binding FriendlyName}"
                                   ... />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <!-- The BoxView doesn't use compiled bindings -->
        <BoxView Color="{Binding Source={x:Reference colorListView}, Path=SelectedItem.Color}"
                 ... />
    </Grid>
</ContentPage>
```

[`ListView.ItemsSource`](xref:Xamarin.Forms.ListView) プロパティは、静的な `NamedColor.All` プロパティに設定されます。 `NamedColor` クラスでは .NET リフレクションを使用して、[`Color`](xref:Xamarin.Forms.Color) 構造体のすべての静的なパブリック フィールドを列挙し、静的な `All` プロパティからアクセス可能なコレクションにその名前と共に格納します。 そのため、`ListView` には `NamedColor` インスタンスがすべて取り込まれます。 `ListView` の各項目については、その項目のバインド コンテキストが `NamedColor` オブジェクトに設定されます。 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 内の [`BoxView`](xref:Xamarin.Forms.BoxView) および [`Label`](xref:Xamarin.Forms.Label) 要素は、`NamedColor` プロパティにバインドされます。

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) では、`NamedColor` 型になるように `x:DataType` 属性が定義され、`DataTemplate` ビュー階層内のバインド式がすべてコンパイルされることが示されることに注意してください。 これは、ビルド エラーとなる、存在しない `NamedColor` プロパティにバインドするためにバインド式のいずれかを変更することで、確認できます。  この例では、`x:DataType` 属性を文字列リテラルに設定しますが、`x:Type` マークアップ拡張機能を使用して型に設定することもできます。 `x:Type` マークアップ拡張機能の詳細については、「[x:Type マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension)」を参照してください。

アプリケーションの最初の実行時に、[`ListView`](xref:Xamarin.Forms.ListView) には `NamedColor` インスタンスが取り込まれます。 `ListView` 内の項目が選択されると、[`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) プロパティは、`ListView` 内の選択された項目の色に設定されます。

[![コンパイルされた色の一覧](compiled-bindings-images/compiledcolorlist-small.png "コンパイルされたカラー リスト]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "コンパイルされたカラー リスト")

[`ListView`](xref:Xamarin.Forms.BoxView) のその他の項目を選択すると、[`BoxView`](xref:Xamarin.Forms.BoxView) の色が更新されます。

## <a name="combine-compiled-bindings-with-classic-bindings"></a>コンパイルされたバインドと従来のバインドを結合する

バインド式は、`x:DataType` 属性が定義されているビュー階層でのみコンパイルされます。 逆に、`x:DataType` 属性が定義されていない階層内のすべてのビューでは、従来のバインドが使用されます。 そのため、ページ上でコンパイル済みのバインドと従来のバインドを結合することができます。 たとえば、前のセクションでは、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 内のビューでコンパイル済みのバインドが使用されますが、[`ListView`](xref:Xamarin.Forms.ListView) で選択された色に設定されている [`BoxView`](xref:Xamarin.Forms.BoxView) では使用されません。

したがって、`x:DataType` 属性を慎重に構成することで、ページでコンパイル済みのバインドと従来のバインドが使用される可能性があります。 また、ビュー階層の任意の時点で `x:Null` マークアップ拡張を使用して、`x:DataType` 属性を `null` に再定義することができます。 これを行うことで、ビュー階層内のすべてのバインド式で従来のバインドが使用されることが示されます。 *混合バインド* ページでこの方法を示します。

```xaml
<StackLayout x:DataType="local:HslColorViewModel">
    <StackLayout.BindingContext>
        <local:HslColorViewModel Color="Sienna" />
    </StackLayout.BindingContext>
    <BoxView Color="{Binding Color}"
             VerticalOptions="FillAndExpand" />
    <StackLayout x:DataType="{x:Null}"
                 Margin="10, 0">
        <Label Text="{Binding Name}" />
        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</StackLayout>   
```

ルートの [`StackLayout`](xref:Xamarin.Forms.StackLayout) では、`HslColorViewModel` 型になるように `x:DataType` 属性が設定され、ルートの `StackLayout` ビュー階層内のバインド式がすべてコンパイルされることが示されます。 しかし、内部の `StackLayout` では、`x:DataType` 属性が `x:Null` マークアップ式を使用して `null` に再定義されます。 したがって、内部の `StackLayout` 内のバインド式では、従来のバインドが使用されます。 ルートの `StackLayout` ビュー階層内の [`BoxView`](xref:Xamarin.Forms.BoxView) のみで、従来のバインドが使用されます。

`x:Null` マークアップ式の詳細については、「[x:Null のマークアップ拡張](~/xamarin-forms/xaml/markup-extensions/consuming.md#xnull-markup-extension)」を参照してください。

## <a name="performance"></a>パフォーマンス

コンパイル済みのバインドではデータ バインディングのパフォーマンスが向上しますが、パフォーマンスの利点はさまざまです。 単体テストでは次のことがわかります。

- プロパティ変更通知を使用するコンパイル済みのバインド (つまり、`OneWay`、`OneWayToSource`、または `TwoWay` バインド) は、従来のバインドより約 8 倍の速さで解決されます。
- プロパティ変更通知を使用しないコンパイル済みのバインド (つまり、`OneTime` バインド) は、従来のバインドより約 20 倍の速さで解決されます。
- プロパティ変更通知を使用するコンパイル済みのバインド (つまり、`OneWay`、`OneWayToSource`、または `TwoWay` バインド) で [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を設定すると、従来のバインドで `BindingContext` を設定する場合より約 5 倍の速さとなります。
- プロパティ変更通知を使用しないコンパイル済みのバインド (つまり、`OneTime` バインド) で [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を設定すると、従来のバインドで `BindingContext` を設定する場合より約 7 倍の速さとなります。

モバイル デバイスでのこれらのパフォーマンスの違いは、使用されているプラットフォーム、使用されているオペレーティング システムのバージョン、およびアプリケーションが実行されているデバイスによって拡大する場合があります。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
