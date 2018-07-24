---
title: Xamarin.Forms データ テンプレートの概要
description: Xamarin.Forms データ テンプレートは、サポートされているコントロールのデータのプレゼンテーションを定義する機能を提供します。 この記事では、必要とされる理由を調べて、データ テンプレートの概要を提供します。
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 129ce7a04b93bfb3cb1b9a1639aee61cd56d09d5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998919"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms データ テンプレートの概要

_Xamarin.Forms データ テンプレートは、サポートされているコントロールのデータのプレゼンテーションを定義する機能を提供します。この記事では、必要とされる理由を調べて、データ テンプレートの概要を提供します。_

検討してください、 [ `ListView` ](xref:Xamarin.Forms.ListView)のコレクションを表示する`Person`オブジェクト。 次のコード例の定義を示しています、`Person`クラス。

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person`クラスを定義`Name`、`Age`と`Location`プロパティで、ときに設定できる、`Person`オブジェクトが作成されます。 [ `ListView` ](xref:Xamarin.Forms.ListView)のコレクションを表示するために使用`Person`オブジェクトの場合、次の XAML コード例に示すようにします。

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

項目を追加、 [ `ListView` ](xref:Xamarin.Forms.ListView)初期化することにより XAML で、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティの配列から`Person`インスタンス。

> [!NOTE]
> なお、`x:Array`要素が必要です、`Type`配列内の項目の種類を示す属性です。

初期化する次のコード例で、同等の c# のページが表示、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティを`List`の`Person`インスタンス。

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

[ `ListView` ](xref:Xamarin.Forms.ListView)呼び出し`ToString`コレクション内のオブジェクトを表示するときにします。 存在しない`Person.ToString`オーバーライド、`ToString`の次のスクリーン ショットに示すように、各オブジェクトの型名を返します。

![](introduction-images/no-data-template.png "データ テンプレートを使用せずに ListView")

`Person`オブジェクトを上書きできます、`ToString`メソッドの次のコード例に示すように、意味のあるデータを表示します。

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

これは、結果、 [ `ListView` ](xref:Xamarin.Forms.ListView)を表示する、`Person.Name`次のスクリーン ショットに示すように、コレクション内の各オブジェクトのプロパティの値。

![](introduction-images/override-tostring.png "データ テンプレートを使用して ListView")

`Person.ToString`上書きで構成される書式設定された文字列を返すことができます、 `Name`、 `Age`、および`Location`プロパティ。 ただし、このアプローチは、データの各アイテムの外観の管理は制限を提供します。 柔軟性を高めるため、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)作成できるデータの外観を定義します。

## <a name="creating-a-datatemplate"></a>DataTemplate を作成します。

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)データの外観を指定するために使用して、通常データ バインドを使用してデータを表示します。 その一般的な使用シナリオは、オブジェクトのコレクションからデータを表示するときに、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 たとえばときに、`ListView`のコレクションにバインドされて`Person`オブジェクト、`ListView.ItemTemplate`プロパティ設定されます、`DataTemplate`それぞれの外観を定義する`Person`オブジェクトに、`ListView`します。 `DataTemplate`の各プロパティの値をバインド要素を含む`Person`オブジェクト。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)次のプロパティの値として使用できます。

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)、継承によって[ `ListView`](xref:Xamarin.Forms.ListView)します。
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1)、継承によって[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)、および[ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。

> [!NOTE]
> [ `TableView` ](xref:Xamarin.Forms.TableView)使用[ `Cell` ](xref:Xamarin.Forms.Cell)オブジェクトを使用していない、 [ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。 これは、データ バインドは常に直接設定ため`Cell`オブジェクト。

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)上に示したプロパティの直接の子と呼ばれるように配置するが、*インライン テンプレート*します。 または、`DataTemplate`制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することができます。 定義する場所を選択する、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)に及ぼす影響を使用できます。

- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)定義されているコントロールのレベルのみに適用できますコントロール。
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)定義ページで、ページ上の複数の有効なコントロールにレベルを適用できます。
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)アプリケーション レベルで定義されているアプリケーション全体で有効に適用されるコントロールを指定できます。

データ テンプレートのビュー階層の下位に共有するときに、アップ以上定義されているよりも優先`x:Key`属性。 たとえば、ページ レベルのデータ テンプレートで、テンプレートをアプリケーション レベルのデータは上書きされ、ページ レベルのデータ テンプレートはコントロール レベルのデータ テンプレート、またはインライン データ テンプレートによって上書きされます。


## <a name="related-links"></a>関連リンク

- [セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
