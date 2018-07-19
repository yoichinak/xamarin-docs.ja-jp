---
title: Xamarin.Forms DataTemplateSelector を作成します。
description: この記事では、作成し、データ バインド プロパティの値に基づいて実行時に、DataTemplate を選択するために使用できる、DataTemplateSelector を使用する方法を示します。
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994518"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Xamarin.Forms DataTemplateSelector を作成します。

_データ バインド プロパティの値に基づいて実行時に、DataTemplate を選択する、DataTemplateSelector を使用できます。これにより、特定のオブジェクトの外観をカスタマイズする、オブジェクトの同じ型に適用される複数の Datatemplate ができます。この記事では、作成し、DataTemplateSelector を使用する方法を示します。_

データ テンプレート セレクターの行うシナリオを[ `ListView` ](xref:Xamarin.Forms.ListView) 、オブジェクトのコレクションへのバインドを内の各オブジェクトの外観、`ListView`データ テンプレート セレクターを返すことによって実行時に選択できます、特定[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。

## <a name="creating-a-datatemplateselector"></a>DataTemplateSelector を作成します。

継承するクラスを作成して、データ テンプレート セレクターが実装されている[ `DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)します。 `OnSelectTemplate`を返す特定のメソッドをオーバーライドして、 [ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)次のコード例のように。

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

`OnSelectTemplate`メソッドの値に基づいて適切なテンプレートを返します、`DateOfBirth`プロパティ。 値を返すテンプレートである、`ValidTemplate`プロパティまたは`InvalidTemplate`プロパティを使用するときに設定されている、`PersonDataTemplateSelector`します。

データ テンプレート セレクターのクラスのインスタンスはなど、Xamarin.Forms コントロール プロパティに割り当てられますし[ `ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)します。 有効なプロパティの一覧は、次を参照してください。 [DataTemplate を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md)します。

### <a name="limitations"></a>制限事項

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) インスタンスには、次の制限があります。

- `DataTemplateSelector`複数回のクエリを実行する場合、サブクラスは、同じテンプレートを同じデータを返す常に必要があります。
- `DataTemplateSelector`サブクラス別返す必要がありますいない`DataTemplateSelector`サブクラスです。
- `DataTemplateSelector`サブクラスはの新しいインスタンスを返す必要がありますいないを`DataTemplate`呼び出しのたびにします。 代わりに、同じインスタンスが返される必要があります。 そのためにはエラーでは、メモリ リークが作成され、仮想化を無効になります。
- Android では、可能性があるあたり最大 20 個の異なるデータ テンプレート`ListView`します。

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML での DataTemplateSelector の使用

XAML、`PersonDataTemplateSelector`次のコード例に示すように、リソースとして宣言することによってインスタンス化することができます。

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

このページ レベル[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 2 つ定義する[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)インスタンスと`PersonDataTemplateSelector`インスタンス。 `PersonDataTemplateSelector`セットのインスタンス、`ValidTemplate`と`InvalidTemplate`プロパティを適切な`DataTemplate`インスタンスを使用して、`StaticResource`マークアップ拡張機能。 リソースは、ページので定義されていることに注目[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、コントロール レベルまたはアプリケーション レベルで定義することもできます。

`PersonDataTemplateSelector`に割り当てることでインスタンスが使用される、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)プロパティは、次のコード例に示すようにします。

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

実行時に、 [ `ListView` ](xref:Xamarin.Forms.ListView)呼び出し、`PersonDataTemplateSelector.OnSelectTemplate`メソッド呼び出しとしてデータ オブジェクトを渡すことで、基になるコレクション内の項目ごと、`item`パラメーター。 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)によって返される、メソッドはそのオブジェクトに適用されます。

次のスクリーン ショットの結果を表示する、 [ `ListView` ](xref:Xamarin.Forms.ListView)適用、`PersonDataTemplateSelector`基になるコレクション内の各オブジェクトに。

![](selector-images/data-template-selector.png "データ テンプレート セレクターを ListView")

すべて`Person`を持つオブジェクトを`DateOfBirth`1980年以上のプロパティの値が赤で表示されている残りのオブジェクトを緑色で表示します。

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C での DataTemplateSelector の使用&num;

C# で、`PersonDataTemplateSelector`インスタンスが作成されに割り当てられていることができます、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)プロパティは、次のコード例に示すようにします。

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

`PersonDataTemplateSelector`セットのインスタンス、`ValidTemplate`と`InvalidTemplate`プロパティを適切な[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)で作成されたインスタンス、`SetupDataTemplates`メソッド。 実行時に、 [ `ListView` ](xref:Xamarin.Forms.ListView)呼び出し、`PersonDataTemplateSelector.OnSelectTemplate`メソッド呼び出しとしてデータ オブジェクトを渡すことで、基になるコレクション内の項目ごと、`item`パラメーター。 `DataTemplate`によって返される、メソッドはそのオブジェクトに適用されます。

## <a name="summary"></a>まとめ

この記事を作成および使用する方法を示しましたが、 [ `DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)します。 A`DataTemplateSelector`選択に使用できる、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)データ バインド プロパティの値に基づいて実行時にします。 これにより、複数`DataTemplate`インスタンス オブジェクト、特定のオブジェクトの外観をカスタマイズするのと同じ型に適用します。


## <a name="related-links"></a>関連リンク

- [データ テンプレート セレクター (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
