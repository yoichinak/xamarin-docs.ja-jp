---
title: DataTemplateSelector を作成します。
description: データ バインドされたプロパティの値に基づいて実行時にデータ テンプレートを選択する、DataTemplateSelector を使用できます。 これにより、特定のオブジェクトの外観をカスタマイズする、オブジェクトの同じ型に適用する複数の Datatemplate できます。 この記事では、作成し、DataTemplateSelector を使用する方法を示します。
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d87b9f3cf47e3b3efa42ad53cfba91bac1d07d4f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplateselector"></a>DataTemplateSelector を作成します。

_データ バインドされたプロパティの値に基づいて実行時にデータ テンプレートを選択する、DataTemplateSelector を使用できます。これにより、特定のオブジェクトの外観をカスタマイズする、オブジェクトの同じ型に適用する複数の Datatemplate できます。この記事では、作成し、DataTemplateSelector を使用する方法を示します。_

データ テンプレート セレクターを使用するシナリオなどと、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 、オブジェクトのコレクションへのバインド位置内の各オブジェクトの外観、`ListView`データ テンプレート セレクターを返すことによって実行時に選択されることができます、特定[ `DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)です。

## <a name="creating-a-datatemplateselector"></a>DataTemplateSelector を作成します。

継承するクラスを作成することでデータのテンプレート セレクターを実装[ `DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)です。 `OnSelectTemplate`メソッドをオーバーライドし、返す特定する[ `DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)次のコード例のように。

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

`OnSelectTemplate`メソッドの値に基づいて、適切なテンプレートが返されます、`DateOfBirth`プロパティです。 返されるテンプレートの値、`ValidTemplate`プロパティまたは`InvalidTemplate`プロパティを使用するときに設定されている、`PersonDataTemplateSelector`です。

データ テンプレート セレクター クラスのインスタンスに割り当てて、Xamarin.Forms コントロールのプロパティをなど[ `ListView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)です。 有効なプロパティの一覧は、次を参照してください。 [DataTemplate を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md)です。

### <a name="limitations"></a>制限事項

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) インスタンスには、次の制限があります。

- `DataTemplateSelector`複数回のクエリを実行する場合、サブクラスは、同じデータに対する同じテンプレートを返す常にする必要があります。
- `DataTemplateSelector`サブクラスを返さないでください別`DataTemplateSelector`サブクラスです。
- `DataTemplateSelector`サブクラスはの新しいインスタンスを返す必要がありますいないを`DataTemplate`呼び出すたびにします。 代わりに、同じインスタンスが返される必要があります。 そのためにはエラーでは、メモリ リークが作成され、仮想化を無効になります。
- Android でありますあたり最大 20 個の異なるデータ テンプレート`ListView`です。

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML で DataTemplateSelector の使用

XAML では、`PersonDataTemplateSelector`次のコード例に示すように、リソースとして宣言することによってインスタンス化されることができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

このページ レベル[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 2 つ定義[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)インスタンスおよび`PersonDataTemplateSelector`インスタンス。 `PersonDataTemplateSelector`インスタンスのセットをその`ValidTemplate`と`InvalidTemplate`を適切なプロパティ`DataTemplate`インスタンスを使用して、`StaticResource`マークアップ拡張機能です。 リソースの中に、ページので定義されている注[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、コントロール レベルまたはアプリケーション レベルで定義することもできます。

`PersonDataTemplateSelector`に割り当てることでインスタンスが消費される、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)プロパティ、次のコード例に示すようにします。

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

実行時に、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)呼び出し、`PersonDataTemplateSelector.OnSelectTemplate`としてデータ オブジェクトを渡すことの呼び出しで、基になるコレクション内の項目の各メソッド、`item`パラメーター。 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)によって返される、メソッドはそのオブジェクトに適用されます。

次のスクリーン ショットの結果を表示する、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を適用する、`PersonDataTemplateSelector`基になるコレクション内の各オブジェクトに。

![](selector-images/data-template-selector.png "データ テンプレート セレクターを含む ListView")

どの`Person`を持つオブジェクト、 `DateOfBirth` 1980年以上のプロパティの値が赤で表示されている残りのオブジェクトと緑に表示されます。

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C で DataTemplateSelector の使用&num;

C# の場合は、`PersonDataTemplateSelector`インスタンス化されに割り当てられていることができます、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)プロパティ、次のコード例に示すようにします。

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

`PersonDataTemplateSelector`インスタンスのセットをその`ValidTemplate`と`InvalidTemplate`プロパティを適切な[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)によって作成されたインスタンス、`SetupDataTemplates`メソッドです。 実行時に、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)呼び出し、`PersonDataTemplateSelector.OnSelectTemplate`としてデータ オブジェクトを渡すことの呼び出しで、基になるコレクション内の項目の各メソッド、`item`パラメーター。 `DataTemplate`によって返される、メソッドはそのオブジェクトに適用されます。

## <a name="summary"></a>まとめ

この記事は、作成および使用する方法を示しましたが、 [ `DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)です。 A`DataTemplateSelector`選択に使用できる、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)データ バインドされたプロパティの値に基づいて実行時にします。 これにより、複数`DataTemplate`インスタンスを特定のオブジェクトの外観をカスタマイズする、オブジェクトの同じ型に適用できます。


## <a name="related-links"></a>関連リンク

- [データ テンプレート セレクター (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
