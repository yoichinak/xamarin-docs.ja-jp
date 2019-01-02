---
title: Xamarin.Forms DataTemplateSelector の作成
description: この記事では、DataTemplateSelector を作成および使用する方法について説明します。これを使用して、データバインド プロパティの値に基づいて、実行時に DataTemplate を選択することができます。
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 75ea30dd510021d7755814e19728bfe60f765645
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057288"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Xamarin.Forms DataTemplateSelector の作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)

_DataTemplateSelector を使用して、データバインド プロパティの値に基づいて、実行時に DataTemplate を選択できます。これにより、複数の DataTemplates を同じ種類のオブジェクトに適用し、特定のオブジェクトの外観をカスタマイズできます。この記事では、DataTemplateSelector を作成して使用する方法を示します。_

データ テンプレート セレクターによって、オブジェクトのコレクションにバインドされる [`ListView`](xref:Xamarin.Forms.ListView) などのシナリオが可能になります。この場合、`ListView` 内の各オブジェクトの外観は、特定の [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を返すデータ テンプレート セレクターによって実行時に選択できます。

## <a name="creating-a-datatemplateselector"></a>DataTemplateSelector の作成

データ テンプレート セレクターを実装するには、[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) から継承するクラスを作成します。 次のコード例に示すように、`OnSelectTemplate` メソッドがオーバーライドされ、特定の [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) が返されます。

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

`OnSelectTemplate` メソッドからは、`DateOfBirth` プロパティの値に基づいて適切なテンプレートが返されます。 返されるテンプレートは、`PersonDataTemplateSelector` の使用時に設定される `ValidTemplate` プロパティまたは `InvalidTemplate` プロパティの値です。

データ テンプレート セレクター クラスのインスタンスは、[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) などの Xamarin.Forms コントロール プロパティに割り当てることができます。 有効なプロパティの一覧については、[DataTemplate の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md)に関するページを参照してください。

### <a name="limitations"></a>制限事項

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) インスタンスには次の制限事項があります。

- 複数回クエリが実行された場合、同じデータに対して `DataTemplateSelector` サブクラスからは常に同じテンプレートが返される必要があります。
- `DataTemplateSelector` サブクラスから別の `DataTemplateSelector` サブクラスを返すことはできません。
- 呼び出しごとに、`DataTemplateSelector` サブクラスから新しいインスタンスの `DataTemplate` を返すことはできません。 代わりに、同じインスタンスを返す必要があります。 そうしないと、メモリ リークが発生し、仮想化が無効になります。
- Android では、`ListView` ごとに使用できるデータ テンプレートは 20 種類以下です。

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML での DataTemplateSelector の使用

XAML で `PersonDataTemplateSelector` をインスタンス化するには、次のコード例のようにリソースとして宣言します。

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

このページ レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) には、2 つの [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) インスタンスと 1 つの `PersonDataTemplateSelector` インスタンスが定義されています。 `PersonDataTemplateSelector` インスタンスでは、`StaticResource` マークアップ拡張を使用して、`ValidTemplate` および `InvalidTemplate` プロパティが適切な `DataTemplate` インスタンスに設定されます。 リソースはページの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に定義されていますが、コントロール レベルまたはアプリケーション レベルで定義することもできる点に注意してください。

`PersonDataTemplateSelector` インスタンスを使用するには、次のコード例に示すように、[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティに割り当てます。

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

実行時に、基となるコレクション内の項目ごとに、[`ListView`](xref:Xamarin.Forms.ListView) から `PersonDataTemplateSelector.OnSelectTemplate` メソッドが呼び出されます。このとき、呼び出しでデータ オブジェクトが `item` パラメーターとして渡されます。 このメソッドから返された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) はそのオブジェクトに適用されます。

次のスクリーンショットは、[`ListView`](xref:Xamarin.Forms.ListView) によって、基となるコレクション内の各オブジェクトに対して `PersonDataTemplateSelector` が適用された結果を示しています。

![](selector-images/data-template-selector.png "データ テンプレート セレクターを使用する ListView")

`DateOfBirth` プロパティ値が 1980 以上のすべての `Person` オブジェクトは緑色で表示され、その他のオブジェクトは赤色で表示されます。

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C&num; での DataTemplateSelector の使用

次のコード例に示すように、C# では、`PersonDataTemplateSelector` をインスタンス化して [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティに割り当てることができます。

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

`PersonDataTemplateSelector` インスタンスでは、`SetupDataTemplates` メソッドで作成された適切な [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) インスタンスに `ValidTemplate` および `InvalidTemplate` プロパティが設定されます。 実行時に、基となるコレクション内の項目ごとに、[`ListView`](xref:Xamarin.Forms.ListView) から `PersonDataTemplateSelector.OnSelectTemplate` メソッドが呼び出されます。このとき、呼び出しでデータ オブジェクトが `item` パラメーターとして渡されます。 このメソッドから返された `DataTemplate` はそのオブジェクトに適用されます。

## <a name="summary"></a>まとめ

この記事では、[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) を作成して使用する方法について説明しました。 `DataTemplateSelector` を使用すると、データバインド プロパティの値に基づいて実行時に [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を選択できます。 これにより、複数の `DataTemplate` インスタンスを同じ種類のオブジェクトに適用し、特定のオブジェクトの外観をカスタマイズできます。


## <a name="related-links"></a>関連リンク

- [データ テンプレート セレクター (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
