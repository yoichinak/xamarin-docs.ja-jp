---
title: Xamarin.Forms のバインドをコンパイルします。
description: この記事では、コンパイル済みのバインドを使用して、Xamarin.Forms アプリケーションでデータ バインディングのパフォーマンスを向上する方法について説明します。
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
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms のバインドをコンパイルします。

_コンパイル済みのバインドは、Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させるため、クラシックのバインドよりも迅速に解決されます。_

データ バインドでは、2 つの主な問題があります。

1. バインディング式のコンパイル時の検証はありません。 代わりに、バインドは、実行時に解決されます。 そのため、無効なバインドが実行時まで検出されないときに、アプリケーションが期待どおりに動作しないか、エラー メッセージが表示されます。
1. これらは、コスト効率がありません。 汎用オブジェクト検査 (リフレクション) を使用して実行時にバインディングが解決され、これを行うオーバーヘッドがプラットフォームによって異なります。

コンパイル済みのバインドは、ランタイムではなく、コンパイル時に連結式を解決することで、Xamarin.Forms アプリケーションでのデータ バインディングのパフォーマンスを向上します。 さらに、このバインディング式のコンパイル時の検証には、トラブルシューティング エクスペリエンスが正しくないバインドがビルド エラーとして報告されますのでより優れた開発者ができるようにします。

コンパイル済みのバインドを使用するためのプロセスでは。

1. XAML のコンパイルを有効にします。 XAML のコンパイルの詳細については、次を参照してください。 [XAML コンパイル](~/xamarin-forms/xaml/xamlc.md)します。
1. 設定、`x:DataType`属性を[ `VisualElement` ](xref:Xamarin.Forms.VisualElement)オブジェクトの型にする、`VisualElement`とその子にバインドされます。 この属性を再表示階層内の任意の場所で定義できるに注意してください。

> [!NOTE]
> 設定することをお勧め、`x:DataType`としてビュー階層内で同じレベルの属性、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)設定されます。

XAML のコンパイル時に、無効なバインド式はビルド エラーとして報告されます。 ただし、XAML コンパイラは、ビルド エラーが発生した最初の無効なバインド式ののみを報告します。 定義されている任意の有効なバインド式、`VisualElement`したりその子には、コンパイル済みかどうかに関係なく、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) XAML またはコードで設定されます。 プロパティから値を取得するコンパイル済みのコードを生成するバインディング式をコンパイルする、*ソース*、上のプロパティに設定し、*ターゲット*マークアップで指定されています。 さらに、バインド式では、によって生成されたコードがで変更を確認の値、*ソース*プロパティと更新、*ターゲット*プロパティ、および、から変更をプッシュすることがあります*ターゲット*に戻す、*ソース*します。

> [!IMPORTANT]
> バインディング式を定義するため、コンパイル済みのバインドが現在無効になって、 [ `Source` ](xref:Xamarin.Forms.Binding.Source)プロパティ。 これは、ため、`Source`常にプロパティを使用して、`x:Reference`マークアップ拡張機能、コンパイル時に解決することはできません。

## <a name="using-compiled-bindings"></a>コンパイル済みのバインドを使用してください。

**色セレクターのコンパイル** ページでは、Xamarin.Forms のビューとビューモデルのプロパティのコンパイル済みバインディングを使用する方法を示します。

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

ルート[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)をインスタンス化、`HslColorViewModel`し、初期化、`Color`プロパティ要素タグ内のプロパティ、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティ。 このルート`StackLayout`も定義、`x:DataType`バインド式ルートにあることを示す、ViewModel 型として属性`StackLayout`ビュー階層がコンパイルされます。 これは、ビルド エラーが存在しない ViewModel のプロパティにバインドするバインディング式のいずれかを変更することで確認できます。

> [!IMPORTANT]
> `x:DataType`属性は、任意の時点でビュー階層を再定義されていることができます。

[ `BoxView` ](xref:Xamarin.Forms.BoxView)、 [ `Label` ](xref:Xamarin.Forms.Label)要素、および[ `Slider` ](xref:Xamarin.Forms.Slider)ビューからのバインディング コンテキストを継承する、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout). これらのビューは、ソース、ViewModel のプロパティを参照するすべてのバインドのターゲットです。 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color)プロパティ、および[ `Label.Text` ](xref:Xamarin.Forms.Label.Text)プロパティ、データ バインドは`OneWay`– ビューモデルのプロパティから、ビューのプロパティが設定されます。 ただし、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)プロパティで使用する`TwoWay`バインドします。 これにより、各`Slider`、ViewModel からおよび各から設定するビューモデルに設定する`Slider`します。

アプリケーションが最初に実行時に、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)、 [ `Label` ](xref:Xamarin.Forms.Label)要素、および[ `Slider` ](xref:Xamarin.Forms.Slider)要素に基づいてビューモデルからすべての設定は、初期`Color`プロパティは、ViewModel がインスタンス化されたときに設定します。 これは、次のスクリーン ショットに示されます。

[![コンパイルのカラー セレクター](compiled-bindings-images/compiledcolorselector-small.png "色セレクターのコンパイル")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "色セレクターのコンパイル")

スライダーを操作する場合に、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)と[ `Label` ](xref:Xamarin.Forms.Label)要素がそれに応じて更新されます。

この色セレクターの詳細については、次を参照してください。[ビューモデル、およびプロパティ変更通知](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications)します。

## <a name="using-compiled-bindings-in-a-datatemplate"></a>DataTemplate でコンパイルされたバインディングの使用

内のバインディングを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)テンプレート化されているオブジェクトのコンテキストで解釈されます。 そのためを使用してコンパイルされたときにバインディングを`DataTemplate`、`DataTemplate`そのデータを使用してオブジェクトの型を宣言する必要があります、`x:DataType`属性。

**色の一覧をコンパイル**ページは、コンパイル済みのバインドでの使用方法を示します、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

[ `ListView.ItemsSource` ](xref:Xamarin.Forms.ListView)プロパティが、静的な`NamedColor.All`プロパティ。 `NamedColor`クラスで、すべての静的パブリック フィールドを列挙するために .NET リフレクションを使用して、 [ `Color` ](xref:Xamarin.Forms.Color)構造、および静的なからアクセス可能なコレクションの名前と共に保存する`All`プロパティ。 そのため、`ListView`はのすべてで埋め、`NamedColor`インスタンス。 各項目に対して、 `ListView`、品目のバインディング コンテキストに設定されて、`NamedColor`オブジェクト。 [ `BoxView` ](xref:Xamarin.Forms.BoxView)と[ `Label` ](xref:Xamarin.Forms.Label)内の要素、 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)にバインドされて`NamedColor`プロパティ。

なお、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を定義、`x:DataType`属性を`NamedColor`ことを示す型内の式をバインド、`DataTemplate`ビュー階層がコンパイルされます。 存在しないにバインドするバインディング式のいずれかを変更することで確認できるよう`NamedColor`プロパティで、ビルド エラーが発生します。

アプリケーションが最初に実行時に、 [ `ListView` ](xref:Xamarin.Forms.ListView)には、`NamedColor`インスタンス。 内の項目のときに、`ListView`が選択されている、 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color)プロパティで選択された項目の色に設定されて、 `ListView`:

[![色の一覧をコンパイル](compiled-bindings-images/compiledcolorlist-small.png "色の一覧をコンパイルする]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

その他の項目を選択すると、 [ `ListView` ](xref:Xamarin.Forms.BoxView)の色を更新、 [ `BoxView`](xref:Xamarin.Forms.BoxView)します。

## <a name="combining-compiled-bindings-with-classic-bindings"></a>結合とクラシックのバインドのバインドをコンパイルします。

バインディング式が階層の表示のコンパイルのみを`x:DataType`に属性が定義されています。 逆に、すべてのビューに階層を`x:DataType`属性が定義されていない従来のバインドを使用します。 コンパイル済みのバインドと、ページ上のクラシックのバインドを結合することはそのためです。 など、前のセクション内のビュー、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)コンパイル済みのバインドを使用中に、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)で選択した色に設定されている、 [ `ListView`](xref:Xamarin.Forms.ListView)はありません。

注意が必要な構造化`x:DataType`属性にそのため、ページのコンパイルとクラシックのバインドを使用して発生する可能性です。 または、`x:DataType`属性が任意の時点へのビュー階層内で再定義できる`null`を使用して、`x:Null`マークアップ拡張機能。 これを行うには、ビュー階層内の任意のバインディング式がクラシックのバインドを使用することを示します。 *混合バインド*ページは、この方法を示します。

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

ルート[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)設定、`x:DataType`属性を`HslColorViewModel`ことを示す型ルートで式をバインド`StackLayout`ビュー階層がコンパイルされます。 ただし、内部`StackLayout`再定義する、`x:DataType`属性を`null`で、`x:Null`マークアップ式。 内部内でバインド式ではそのため、`StackLayout`クラシックのバインドを使用します。 のみ、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)、ルート内`StackLayout`階層、コンパイルを使用してバインドを表示します。

詳細については、`x:Null`マークアップの式を参照してください[X:null マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#null)します。

## <a name="performance"></a>パフォーマンス

コンパイル済みのバインドには、データ バインディングのさまざまなパフォーマンスの特典でのパフォーマンスが向上します。 単体テストの調査によってわかりました。

- プロパティ変更通知を使用するコンパイル済みのバインド (つまり、 `OneWay`、 `OneWayToSource`、または`TwoWay`バインド) 約 8 回クラシック バインドよりも高速に解決されます。
- コンパイル済みのバインド プロパティの変更通知を使用しない (つまり、`OneTime`バインド) が約 20 回クラシック バインドよりも高速に解決します。
- 設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティを使用するコンパイル済みのバインドでは、変更通知 (つまり、 `OneWay`、 `OneWayToSource`、または`TwoWay`バインド) は約 5 倍高速化、を設定するよりも`BindingContext`クラシック バインドします。
- 設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティを使用しないコンパイル済みのバインドでは、変更通知 (つまり、`OneTime`バインド) 設定よりも 7 倍高速化は、約、`BindingContext`クラシック バインドします。

これらのパフォーマンスの違いは、使用されているオペレーティング システムとアプリケーションが実行されているデバイスのバージョンが使用されているプラットフォームに依存する、モバイル デバイスで拡大することができます。

## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
