---
title: Xamarin. Forms のバインド可能なレイアウト
description: バインド可能なレイアウトでは、レイアウトクラスが項目のコレクションにバインドすることによってコンテンツを生成できます。また、各項目の外観を System.windows.datatemplate> で設定することもできます。
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: a824c892d21df9264b772bed09a4aef893f3b949
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68647902"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Xamarin. Forms のバインド可能なレイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

バインド可能なレイアウトでは、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスから派生する任意のレイアウトクラスを使用して、項目のコレクションにバインドすることによってコンテンツを生成できます。また、各項目の外観を[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で設定することもできます。 バインド可能なレイアウトは `BindableLayout` クラスによって提供され、次の添付プロパティを公開します。

- `ItemsSource` –レイアウトによって表示される `IEnumerable` 項目のコレクションを指定します。
- `ItemTemplate` –レイアウトによって表示される項目のコレクション内の各項目に適用する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を指定します。
- `ItemTemplateSelector` –実行時に項目の[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択するために使用される[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を指定します。

これらのプロパティは、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)、 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)、 [`Grid`](xref:Xamarin.Forms.Grid)、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)、および[`StackLayout`](xref:Xamarin.Forms.StackLayout)クラスにアタッチできます。これらのクラスはすべて、 [1](xref:Xamarin.Forms.Layout`1)クラスから派生します。

> [!NOTE]
> @No__t_1 と `ItemTemplateSelector` の両方のプロパティが設定されている場合は、`ItemTemplate` プロパティが優先されます。

@No__t_0 クラスは、レイアウトの子要素が追加される[`Children`](xref:Xamarin.Forms.Layout`1.Children)コレクションを公開します。 @No__t_0 プロパティが項目のコレクションに設定され、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)派生クラスに関連付けられている場合、レイアウトによって表示されるように、コレクション内の各項目が `Layout<T>.Children` コレクションに追加されます。 @No__t_0 派生クラスは、基になるコレクションが変更されたときに子ビューを更新します。 Xamarin のレイアウトサイクルの詳細については、「[カスタムレイアウトを作成する](~/xamarin-forms/user-interface/layouts/custom.md)」を参照してください。

バインド可能なレイアウトは、表示される項目のコレクションが小さく、スクロールと選択が不要な場合にのみ使用してください。 [@No__t_1](xref:Xamarin.Forms.ScrollView)にバインド可能なレイアウトをラップすることによってスクロールできますが、バインド可能なレイアウトに UI 仮想化が存在しない場合は推奨されません。 スクロールが必要な場合は、 [`ListView`](xref:Xamarin.Forms.ListView)や[`CollectionView`](xref:Xamarin.Forms.CollectionView)などの UI 仮想化を含む、スクロール可能なビューを使用する必要があります。 この推奨事項に従わないと、パフォーマンスの問題が発生する可能性があります。

> [!IMPORTANT]
>[@No__t_1](xref:Xamarin.Forms.Layout`1)クラスから派生した任意のレイアウトクラスにバインド可能なレイアウトをアタッチすることは技術的に可能ですが、特に、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)、 [`Grid`](xref:Xamarin.Forms.Grid)、および[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)クラスでは、これを行うことは実用的ではありません。 たとえば、バインド可能なレイアウトを使用して[`Grid`](xref:Xamarin.Forms.Grid)のデータのコレクションを表示するシナリオを考えてみます。コレクション内の各項目は、複数のプロパティを含むオブジェクトです。 @No__t_0 の各行は、コレクションのオブジェクトを表示する必要があります。また、`Grid` 内の各列には、オブジェクトのプロパティの1つが表示されます。 バインド可能なレイアウトの[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)には1つのオブジェクトのみを含めることができるため、そのオブジェクトは、特定の `Grid` 列にオブジェクトのプロパティの1つを表示する複数のビューを含むレイアウトクラスである必要があります。 このシナリオはバインド可能なレイアウトと realised ことができますが、バインドされたコレクション内の各項目の子 `Grid` を含む親 `Grid` になります。これは、`Grid` レイアウトの非常に非効率的で問題のある使用です。

## <a name="populating-a-bindable-layout-with-data"></a>バインド可能なレイアウトのデータへの読み込み

バインド可能なレイアウトには、その `ItemsSource` プロパティを `IEnumerable` を実装する任意のコレクションに設定し、それを[`Layout<T>`](xref:Xamarin.Forms.Layout`1)派生クラスにアタッチすることによって、データが格納されます。

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

これに相当する C# コードを次に示します。

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

@No__t_0 添付プロパティがレイアウトに設定されていても、`BindableLayout.ItemTemplate` 添付プロパティが設定されていない場合、`IEnumerable` コレクション内のすべての項目は、`BindableLayout` クラスによって作成された[`Label`](xref:Xamarin.Forms.Label)によって表示されます。

## <a name="defining-item-appearance"></a>項目の外観の定義

バインド可能なレイアウトの各項目の外観を定義するには、`BindableLayout.ItemTemplate` 添付プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

これに相当する C# コードを次に示します。

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

この例では、`TopFollowers` コレクション内のすべての項目が、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で定義されている `CircleImage` ビューによって表示されます。

![System.windows.datatemplate> を使用したバインド可能なレイアウト](bindable-layouts-images/top-followers.png "データテンプレートを使用したバインド可能なレイアウト")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choosing-item-appearance-at-runtime"></a>実行時の項目の外観の選択

バインド可能なレイアウトの各項目の外観は、項目の値に基づいて実行時に選択できます。そのためには、`BindableLayout.ItemTemplateSelector` 添付プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に設定します。

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

これに相当する C# コードを次に示します。

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

サンプルアプリケーションで使用されている[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を次の例に示します。

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

@No__t_0 クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `DefaultTemplate` と `XamarinFormsTemplate` を定義します。 @No__t_0 メソッドは `XamarinFormsTemplate` を返します。これにより、項目が "Xamarin. Forms" と等しい場合に、赤い赤の項目が横のハートで表示されます。 項目が "Xamarin. Forms" と等しくない場合、`OnSelectTemplate` メソッドは、 [`Label`](xref:Xamarin.Forms.Label)の既定の色を使用して項目を表示する `DefaultTemplate` を返します。

![DataTemplateSelector を使用したバインド可能なレイアウト](bindable-layouts-images/favorite-tech.png "データテンプレートセレクターを使用したバインド可能なレイアウト")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [バインド可能なレイアウトのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
