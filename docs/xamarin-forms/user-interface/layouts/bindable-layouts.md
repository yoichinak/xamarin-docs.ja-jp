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
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647902"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Xamarin. Forms のバインド可能なレイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

バインド可能なレイアウトでは、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスから派生した任意のレイアウトクラスを使用して、項目のコレクションにバインドすることによってコンテンツを生成できます。また、を[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)使用して各項目の外観を設定することもできます。 バインド可能なレイアウトは、 `BindableLayout`クラスによって提供されます。このクラスは、次の添付プロパティを公開します。

- `ItemsSource`–レイアウトによって`IEnumerable`表示される項目のコレクションを指定します。
- `ItemTemplate`–レイアウトに[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)よって表示される項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`–実行時[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に項目のを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)選択するために使用されるを指定します。

これらの[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)プロパティは[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`StackLayout`](xref:Xamarin.Forms.StackLayout) 、クラスから派生する、、、、およびの各クラスにアタッチできます。 [`Grid`](xref:Xamarin.Forms.Grid) [`Layout<T>`](xref:Xamarin.Forms.Layout`1)

> [!NOTE]
> プロパティ`ItemTemplate` `ItemTemplate`とプロパティ`ItemTemplateSelector`の両方が設定されている場合は、プロパティが優先されます。

クラス`Layout<T>`は、レイアウト[`Children`](xref:Xamarin.Forms.Layout`1.Children)の子要素が追加されるコレクションを公開します。 プロパティが項目のコレクションに設定され、 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)から派生したクラスに関連付けられている場合、 `Layout<T>.Children`コレクション内の各項目は、レイアウトによって表示されるようにコレクションに追加されます。 `BinableLayout.ItemsSource` `Layout<T>`派生クラスは、基になるコレクションが変更されたときに、その子ビューを更新します。 Xamarin のレイアウトサイクルの詳細については、「[カスタムレイアウトを作成する](~/xamarin-forms/user-interface/layouts/custom.md)」を参照してください。

バインド可能なレイアウトは、表示される項目のコレクションが小さく、スクロールと選択が不要な場合にのみ使用してください。 では、バインド可能なレイアウトをラップすることに[`ScrollView`](xref:Xamarin.Forms.ScrollView)よってスクロールできますが、バインド可能なレイアウトに UI 仮想化が存在しない場合は推奨されません。 スクロールが必要な場合[`ListView`](xref:Xamarin.Forms.ListView)は、や[`CollectionView`](xref:Xamarin.Forms.CollectionView)などの UI 仮想化を含むスクロール可能なビューを使用する必要があります。 この推奨事項に従わないと、パフォーマンスの問題が発生する可能性があります。

> [!IMPORTANT]
>[`Layout<T>`](xref:Xamarin.Forms.Layout`1)クラスから派生した任意のレイアウトクラスにバインド可能なレイアウトをアタッチすることは技術的に可能ですが、特に[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)、 [`Grid`](xref:Xamarin.Forms.Grid)、および[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)の各クラスでは、これを行うことは実用的ではありません。 たとえば、バインド可能なレイアウトを[`Grid`](xref:Xamarin.Forms.Grid)使用してのデータのコレクションを表示するシナリオを考えてみます。コレクション内の各項目は、複数のプロパティを含むオブジェクトです。 の各行には、コレクションのオブジェクトが表示されます。各列に`Grid`は、オブジェクトのプロパティの1つが表示されます。 `Grid` バインド可能`Grid`なレイアウトのには1つのオブジェクトのみを含めることができるため、そのオブジェクトは、それぞれが特定の列にあるオブジェクトのプロパティの1つを表示する複数のビューを含むレイアウトクラスである必要があります。 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) このシナリオはバインド可能なレイアウトと realised ことができますが、 `Grid`バインドされ`Grid`たコレクション内の各項目の子を含む親になります。これは、 `Grid`レイアウトの非常に非効率的で問題のある使用です。

## <a name="populating-a-bindable-layout-with-data"></a>バインド可能なレイアウトのデータへの読み込み

バインド可能なレイアウトには、 `ItemsSource`プロパティをが実装`IEnumerable`されている任意のコレクションに設定[`Layout<T>`](xref:Xamarin.Forms.Layout`1)し、それを派生クラスにアタッチすることによって、データを設定します。

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

同等の C# コードに示します。

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

`IEnumerable` `BindableLayout.ItemTemplate` `BindableLayout` [`Label`](xref:Xamarin.Forms.Label)添付プロパティがレイアウトに設定されていても、添付プロパティが設定されていない場合、コレクション内のすべての項目は、クラスによって作成されたによって表示されます。 `BindableLayout.ItemsSource`

## <a name="defining-item-appearance"></a>項目の外観の定義

バインド可能な`BindableLayout.ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)レイアウトの各項目の外観は、添付プロパティをに設定することによって定義できます。

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

同等の C# コードに示します。

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

この例では、 `TopFollowers`コレクション内のすべての項目が、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で`CircleImage`定義されているビューによって表示されます。

![System.windows.datatemplate> を使用したバインド]可能なレイアウト(bindable-layouts-images/top-followers.png "データテンプレートを使用したバインド可能なレイアウト")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choosing-item-appearance-at-runtime"></a>実行時の項目の外観の選択

バインド可能な`BindableLayout.ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)レイアウトの各項目の外観は、添付プロパティをに設定することによって、項目の値に基づいて実行時に選択できます。

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

同等の C# コードに示します。

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

サンプル[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)アプリケーションで使用されているは、次の例のようになります。

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

クラス`TechItemTemplateSelector`は、さまざま`XamarinFormsTemplate`なデータテンプレートに設定されるプロパティと[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティを定義`DefaultTemplate`します。 メソッド`OnSelectTemplate`はを`XamarinFormsTemplate`返します。これにより、項目が "Xamarin. Forms" に等しい場合は、その横にハートが付いた濃い赤の項目が表示されます。 項目が "Xamarin. Forms" と等しくない場合、メソッド`OnSelectTemplate`はを返し`DefaultTemplate` [`Label`](xref:Xamarin.Forms.Label)ます。これは、の既定の色を使用して項目を表示します。

![DataTemplateSelector を使用したバインド]可能なレイアウト(bindable-layouts-images/favorite-tech.png "データテンプレートセレクターを使用したバインド可能なレイアウト")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [バインド可能なレイアウトのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
