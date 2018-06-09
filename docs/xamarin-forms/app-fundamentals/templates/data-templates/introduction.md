---
title: Xamarin.Forms データ テンプレートの概要
description: Xamarin.Forms データ テンプレートでは、サポートされているコントロールのデータの表示を定義する機能を提供します。 この記事では、必要とされる理由を調べて、データ テンプレートに紹介します。
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: e54c7d3ea01c59a20561b69c6e790747567d92f0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240178"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms データ テンプレートの概要

_Xamarin.Forms データ テンプレートでは、サポートされているコントロールのデータの表示を定義する機能を提供します。この記事では、必要とされる理由を調べて、データ テンプレートに紹介します。_

検討してください、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)のコレクションを表示する`Person`オブジェクト。 次のコード例は、の定義を示しています、`Person`クラス。

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person`クラスを定義`Name`、 `Age`、および`Location`プロパティで、ときに設定可能、`Person`オブジェクトを作成します。 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)のコレクションを表示するために使用`Person`オブジェクトを次の XAML コードの例で示すようにします。

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

項目を追加、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)初期化することによって XAML で、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/)プロパティの配列から`Person`インスタンス。

> [!NOTE]
> なお、`x:Array`要素が必要です、`Type`配列内の項目の種類を示す属性です。

次のコードの例は、初期化に、同等の c# ページが表示、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/)プロパティを`List`の`Person`インスタンス。

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

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)呼び出し`ToString`コレクション内のオブジェクトを表示するときにします。 あるためありません`Person.ToString`を上書きする`ToString`次のスクリーン ショットに示すように、各オブジェクトの型名を返します。

![](introduction-images/no-data-template.png "データ テンプレートを使用せず ListView")

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

これは、結果、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を表示する、`Person.Name`次のスクリーン ショットに示すように、コレクション内の各オブジェクトのプロパティの値。

![](introduction-images/override-tostring.png "データ テンプレートを含む ListView")

`Person.ToString`オーバーライドで構成される書式設定された文字列を返す可能性があります、 `Name`、 `Age`、および`Location`プロパティです。 ただし、この方法は、データの各項目の外観の管理は制限を提供します。 柔軟性が高まります、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)作成できるデータの外観を定義します。

## <a name="creating-a-datatemplate"></a>DataTemplate を作成します。

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)は、データの外観を指定に使用され、通常のデータ バインディングを使用して、データを表示します。 一般的な使用シナリオは内のオブジェクトのコレクションからデータを表示するときに、 [ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)です。 たとえば、`ListView`のコレクションにバインドされます`Person`オブジェクト、`ListView.ItemTemplate`プロパティ設定されます、`DataTemplate`それぞれの外観を定義する`Person`内のオブジェクト、`ListView`です。 `DataTemplate`の各プロパティの値にバインド要素を含む`Person`オブジェクト。 データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)次のプロパティの値として使用できます。

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)、これがによって継承[ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)です。
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/)、これがによって継承[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)、および[ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)です。

> [!NOTE]
> [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)の使用は、 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)オブジェクトを使用していない、 [ `DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)です。 これは、データ バインディングは、常に直接設定ため`Cell`オブジェクト。

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)上に示したプロパティの直接の子は、として認識されているが配置されます、*インライン テンプレート*です。 または、`DataTemplate`制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することができます。 定義する場所を選択する、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)インパクトを使用できます。

- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)定義されているコントロールのレベルのみに適用できますコントロール。
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)定義レベルは、ページで、ページ上の複数の有効なコントロールを適用することができます。
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)アプリケーション レベルで定義されているアプリケーション全体で有効に適用されるコントロールを指定できます。

データ テンプレート ビュー階層の下位に定義されているもの以上を共有する場合よりも優先`x:Key`属性。 たとえば、アプリケーション レベルのデータ テンプレートは、ページ レベルのデータ テンプレートによって上書きされますおよびページ レベルのデータ テンプレートは、コントロール レベルのデータ テンプレートまたはインライン データ テンプレートによって上書きされます。


## <a name="related-links"></a>関連リンク

- [セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
