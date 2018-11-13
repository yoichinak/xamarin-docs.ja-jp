---
title: Xamarin.Forms コンパイル済みバインディング。
description: この記事では、コンパイル済みバインディングを使用して、Xamarin.Forms アプリケーションでデータ バインディングのパフォーマンスを向上する方法について説明します。
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2018
ms.openlocfilehash: 0b350082c834076a1d69427644259087d64bf26a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111607"
---
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms コンパイル済みバインディング

_コンパイル済みバインディングは、従来のバインディングよりも迅速に解決するため、Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させます。_

データ バインディングには、2 つの主な問題があります。

1. バインディング式のコンパイル時の検証がありません。 その代わりに、バインディングは、実行時に解決されます。 そのため、無効なバインディングは、アプリケーションが期待どおりに動作しないか、エラーメッセージが現れる実行時まで検出されません。
2. これらは、コスト効率がよくありません。 バインディングは、汎用オブジェクト検査 (リフレクション) を使用して実行時に解決され、これを行うオーバーヘッドがプラットフォームによって異なります。

コンパイル済みバインディングは、実行時ではなく、コンパイル時にバインディング式を解決することで、Xamarin.Forms アプリケーションでのデータ バインディングのパフォーマンスを向上させます。 さらに、このバインディング式のコンパイル時の検証は、無効なバインディングがビルドエラーとして報告されるので、より優れた開発者のトラブルシューティングを可能にします。

コンパイル済みバインディングを使用するには:

1. XAML のコンパイルを有効にします。 XAML のコンパイルの詳細については、[XAML のコンパイル](~/xamarin-forms/xaml/xamlc.md) を参照してください。
2. `x:DataType` 属性を [`VisualElement`](xref:Xamarin.Forms.VisualElement) に設定して、 `VisualElement` 型のオブジェクトとその子要素をバインドします。この属性は、ビューの階層内のどの場所からでも再定義できることに注意してください。

> [!NOTE]
> [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) が設定されるビュー階層と同じレベルに `x:DataType` 属性を設定することを推奨します。

XAML のコンパイル時に、無効なバインディング式はビルド エラーとして報告されます。 ただし、XAML コンパイラは、発生したエラーの中で、最初の無効なバインディング式のビルド エラーのみを報告します。 `VisualElement` やその子要素に定義されている有効なバインディング式は、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) が XAML またはコードで設定されているかどうかに関係なく、コンパイルされます。 バインディング式のコンパイルは、*ソース* のプロパティから値を取得してコンパイル済みのコードを生成し、それをマークアップで指定された *ターゲット* のプロパティにセットします。 さらに、バインディング式によっては、生成されたコードは、 *ソース* プロパティの値の変更を観測し、*ターゲット* プロパティを更新することがあり、また *ターゲット* から *ソース* へ返す変更をプッシュすることがあります。

> [!IMPORTANT]
> コンパイル済みバインディングは、[ `Source` ](xref:Xamarin.Forms.Binding.Source)プロパティを定義しているバインディング式では現在無効になっています。これは、`Source` プロパティが、常に `x:Reference` マークアップ拡張を使ってセットされるためで、これはコンパイル時に解決することはできません。

## <a name="using-compiled-bindings"></a>コンパイル済みバインディングの使用

**Compiled Color Selector** ページでは、 Xamarin.Forms のView と ViewModel のプロパティ間にコンパイル済みバインディングを使用する方法を示しています。

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

ルートの[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)は、`HslColorViewModel` をインスタンス化し、[ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティのプロパティ要素タグ内で `Color` プロパティを初期化します。このルートの `StackLayout` には、 ViewModel の型として `x:DataType` 属性の定義もされており、これはルートの `StackLayout` のビュー階層内のバインディング式がコンパイルされることを示しています。 これは、バインディング式のいずれかを、存在しない ViewModel のプロパティにバインドすることによって確認でき、その結果は、ビルドエラーになります。

> [!IMPORTANT]
> `x:DataType` 属性は、ビュー階層のどの場所でも再定義できます。

[ `BoxView` ](xref:Xamarin.Forms.BoxView)、 [ `Label` ](xref:Xamarin.Forms.Label)要素、および[ `Slider` ](xref:Xamarin.Forms.Slider)ビューは、[ `StackLayout`](xref:Xamarin.Forms.StackLayout) からバインディング コンテキストを継承しています。 これらのビューはすべて、ViewModel の ソースプロパティを参照するターゲットをバインディングしています。 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color)プロパティ、および[ `Label.Text` ](xref:Xamarin.Forms.Label.Text)プロパティのデータバインディングは、`OneWay` （ViewModel のプロパティから View のプロパティにセットされます）です。 ただし、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)プロパティには、`TwoWay` バインディングが使用されています。これにより、各 `Slider` が ViewModel からセットされ、また各 `Slider` から ViewModel へセットされることを可能にします。

アプリケーションが最初に実行されるとき、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)、 [ `Label` ](xref:Xamarin.Forms.Label)要素、および[ `Slider` ](xref:Xamarin.Forms.Slider)要素は、ViewModel がインスタンス化された時にセットした初期の `Color` プロパティに基づいて、ViewModel からすべて設定されます。この様子は、次のスクリーンショットで示しています。

[![Compiled Color Selector](compiled-bindings-images/compiledcolorselector-small.png "Compiled Color Selector")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "Compiled Color Selector")

スライダーを操作すると、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)と[ `Label` ](xref:Xamarin.Forms.Label)要素は、それに応じて更新されます。

この Color Selector の詳細については、[ビューモデル、およびプロパティ変更通知](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications) を参照してください。

## <a name="using-compiled-bindings-in-a-datatemplate"></a>DataTemplate でのコンパイル済みバインディングの使用

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 内のバインディングは、テンプレート化されたオブジェクトのコンテキストで解釈されます。そのため、`DataTemplate` 内でコンパイル済みバインディングを使う時、`DataTemplate` は `x:DataType` 属性を使ってデータオブジェクトの型を宣言する必要があります。

**Compiled Color List** ページは、[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 内でのコンパイル済みバインディングの使用方法を示します。:

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

[ `ListView.ItemsSource` ](xref:Xamarin.Forms.ListView) プロパティには、静的な `NamedColor.All` プロパティがセットされています。 `NamedColor` クラスは、.NET リフレクションを使用して、[ `Color` ](xref:Xamarin.Forms.Color) 構造体のすべての静的なパブリックフィールドを列挙して、静的な `All` プロパティからアクセスできるコレクションに、それらの名前を保存しています。そのため、`ListView` は、すべて `NamedColor` インスタンスで満たされています。 `ListView` の各アイテムのバインディングコンテキストには、 `NamedColor` オブジェクトがセットされています。[ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 内の [ `BoxView` ](xref:Xamarin.Forms.BoxView) と [ `Label` ](xref:Xamarin.Forms.Label) 要素は `NamedColor` プロパティにバインドされています。

なお、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) には、`x:DataType` 属性に `NamedColor` 型であることが定義されており、これは `DataTemplate` のビュー階層内のバインディング式がコンパイルされることを示しています。これは、いずれかのバインディング式を変更して、存在しない `NamedColor` のプロパティにバインドすることによって検証でき、その結果はビルドエラーとなります。

アプリケーションが最初に実行されたとき、 [ `ListView` ](xref:Xamarin.Forms.ListView) には、`NamedColor` インスタンスが入っています。 `ListView` 内のアイテムが選択されたとき、[ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) プロパティに `ListView` で選択されたアイテムの色がセットされます。:

[![Compiled Color List](compiled-bindings-images/compiledcolorlist-small.png "Compiled Color List]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

[ `ListView` ](xref:Xamarin.Forms.BoxView) 内の他のアイテムを選択すると、 [ `BoxView`](xref:Xamarin.Forms.BoxView) の色も更新されます。

## <a name="combining-compiled-bindings-with-classic-bindings"></a>コンパイル済みバインディングと従来のバインディングを組み合わせる

バインディング式は、 `x:DataType` 属性が定義されているビュー階層でのみコンパイルされます。逆に、`x:DataType` 属性が定義されていない階層のビューは、従来のバインディングが使用されます。したがって、ページ上でコンパイル済みバインディングと従来のバインディングを組み合わせることができます。 例えば、前のセクションの [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 内のビューでは、コンパイル済みバインディングが使われ、[ `ListView`](xref:Xamarin.Forms.ListView) で選択された色が設定される [ `BoxView` ](xref:Xamarin.Forms.BoxView) では、コンパイル済みバインディングは使用されていません。

そのため、`x:DataType` 属性の構造に注意すると、ページにコンパイル済みと従来のバインディングを使用できるようになります。 また、`x:DataType` 属性は、ビュー階層のどの位置からでも、`x:Null` マークアップ拡張を使って `null` で再定義することができます。これを行うことで、ビュー階層のどのバインディング式にも、従来のバインディングが使用できることを示しています。 *Mixed Binding* ページでは、この方法を示しています。

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

ルートの [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) には、`x:DataType` 属性に `HslColorViewModel` 型が設定されています。これは、ルートの `StackLayout` のビュー階層のどのバインディング式もコンパイルされることを示しています。 ただし、内部の `StackLayout` は `x:DataType` 属性に `x:Null` マークアップ式を使って `null` で再定義されています。 そのため、内部の `StackLayout` 内のバインディング式には、従来のバインディングが使用されます。 ルートの `StackLayout` の中の、[ `BoxView` ](xref:Xamarin.Forms.BoxView) だけに、コンパイル済みバインディングが使用されます。

`x:Null` マークアップ式の詳細については、[x:Null マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#null) を参照してください。

## <a name="performance"></a>パフォーマンス

コンパイル済みバインディングは、多様なパフォーマンスの利益をともなって、データバインディングのパフォーマンスを向上させます。単体テストによって以下のことが判明しています。

- プロパティ変更通知を使用するコンパイル済みバインディング ( `OneWay` 、 `OneWayToSource` 、または `TwoWay` バインディング) は、従来のバインディングより約 8 倍速く解決されます。
- プロパティ変更通知を使用しないコンパイル済みバインディング  ( `OneTime` バインディング) は、従来のバインディングより約 20 倍速く解決されます。
- プロパティ変更通知を使用するコンパイル済みバインディングの [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) の設定 ( `OneWay` 、 `OneWayToSource` 、または `TwoWay` バインディング) は、従来のバインディングの `BindingContext` の設定より約 5 倍高速です。
- プロパティ変更通知を使用しないコンパイル済みバインディングの [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) の設定 ( `OneTime` バインディング) は、従来のバインディングの `BindingContext` の設定より約 7 倍高速です。

これらのパフォーマンスの変化は、使用しているプラットフォーム、オペレーティング システムのバージョン、およびアプリケーションが実行されているデバイスによって、モバイルデバイス上で大きくなる可能性があります。

## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
