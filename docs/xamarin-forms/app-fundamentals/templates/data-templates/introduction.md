---
title: Xamarin.Forms データ テンプレートの概要
description: Xamarin.Forms のデータ テンプレートを使うと、サポートされているコントロール上のデータの表現方法を定義することができます。 この記事では、データ テンプレートの概要について説明し、これが必要である理由について調べます。
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3558352c9f43b8e301492077806bbb611e9b58cf
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929312"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms データ テンプレートの概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Xamarin.Forms のデータ テンプレートを使うと、サポートされているコントロール上のデータの表現方法を定義できます。この記事では、データ テンプレートの概要について説明し、これが必要である理由について調べます。_

`Person` オブジェクトのコレクションを表示する [`ListView`](xref:Xamarin.Forms.ListView) について検討します。 次のコード例は、`Person` クラスの定義を示しています。

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` クラスでは、`Name`、`Age`、および `Location` のプロパティを定義します。これらのプロパティは、`Person` オブジェクトの作成時に設定できます。 次の XAML コード例に示すように、[`ListView`](xref:Xamarin.Forms.ListView) は `Person` オブジェクトのコレクションを表示するために使用されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

項目を XAML の [`ListView`](xref:Xamarin.Forms.ListView) に追加するには、`Person` インスタンスの配列の [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを初期化します。

> [!NOTE]
> `x:Array` 要素には、配列内の項目の型を示す `Type` 属性が必要です。

C# での同等のページを次のコード例に示します。この例では、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを `Person` インスタンスの `List` に初期化します。

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

コレクション内のオブジェクトを表示するときに、[`ListView`](xref:Xamarin.Forms.ListView) によって `ToString` が呼び出されます。 次のスクリーンショットに示すように、`Person.ToString` のオーバーライドがないため、`ToString` から各オブジェクトの型名が返されます。

![データ テンプレートを使用しない ListView](introduction-images/no-data-template.png)

次のコード例に示すように、意味のあるデータを表示するために `Person` オブジェクトによって `ToString` メソッドをオーバーライドできます。

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

この結果、次のスクリーンショットに示すように、[`ListView`](xref:Xamarin.Forms.ListView) でコレクション内の各オブジェクトの `Person.Name` プロパティ値が表示されます。

![データ テンプレートを使用する ListView](introduction-images/override-tostring.png)

`Person.ToString` のオーバーライドから、`Name`、`Age`、および `Location` プロパティで構成される書式設定された文字列が返される可能性があります。 ただし、このアプローチでは、各データ項目の外観に対して制限されたコントロールのみが提供されます。 柔軟性を高めるために、データの外観を定義する [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を作成することができます。

## <a name="creating-a-datatemplate"></a>DataTemplate を作成する

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) はデータの外観を指定するために使用され、通常はデータを表示するためにデータ バインディングが使用されます。 一般的な使用シナリオは、[`ListView`](xref:Xamarin.Forms.ListView) でオブジェクトのコレクションのデータを表示する場合です。 たとえば、`ListView` が `Person` オブジェクトのコレクションにバインドされている場合、`ListView.ItemTemplate` プロパティは `ListView` 内の各 `Person` オブジェクトの外観を定義する `DataTemplate` に設定されます。 `DataTemplate` には、各 `Person` オブジェクトのプロパティ値にバインドされる要素が含まれます。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、次のプロパティの値として使用できます。

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)。これは [`ListView`](xref:Xamarin.Forms.ListView) によって継承されます。
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1)。これは [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)、[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) によって継承されます。

> [!NOTE]
> [`TableView`](xref:Xamarin.Forms.TableView) は [`Cell`](xref:Xamarin.Forms.Cell) オブジェクトを利用しますが、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は使用しない点に注意してください。 これは、データ バインディングは常に `Cell` オブジェクトに直接設定されるためです。

前述のプロパティの直接の子として配置された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、*インライン テンプレート*と呼ばれます。 また、`DataTemplate` は、コントロールレベル、ページレベル、またはアプリケーションレベルのリソースとして定義できます。 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を定義する場所の選択は、使用できる場所に影響があります。

- コントロール レベルで定義された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、コントロールにのみ適用できます。
- ページ レベルで定義された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、ページ上の複数の有効なコントロールに適用できます。
- アプリケーション レベルで定義された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、アプリケーション全体の有効なコントロールに適用できます。

`x:Key` 属性を共有する場合、ビュー階層で下位にあるデータ テンプレートは、上位の定義済みコントロール テンプレートよりも優先されます。 たとえば、アプリケーションレベルのデータ テンプレートはページレベルのデータ テンプレートでオーバーライドされ、ページレベルのデータ テンプレートはコントロールレベルのデータ テンプレートまたはインライン データ テンプレートによってオーバーライドされます。

## <a name="related-links"></a>関連リンク

- [セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [データ テンプレート (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
