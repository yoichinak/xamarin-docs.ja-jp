---
title: Xamarin.Forms DataTemplate の作成
description: データ テンプレートは、ResourceDictionary 内でインラインで作成したり、またはカスタム型や適切な Xamarin.Forms のセルの種類から作成したりできます。 この記事では、各手法について説明します。
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 399f411acd497b9d55ca81f670556430fe5f5503
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "70771287"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Xamarin.Forms DataTemplate の作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_データ テンプレートは、ResourceDictionary 内でインラインで作成したり、またはカスタム型や適切な Xamarin.Forms のセルの種類から作成したりできます。この記事では、各手法について説明します。_

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) の一般的な使用シナリオは、[`ListView`](xref:Xamarin.Forms.ListView) でオブジェクトのコレクションのデータを表示することです。 [`ListView`](xref:Xamarin.Forms.ListView) の各セルのデータの外観は、[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティを [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定することで管理できます。 これを実現するために利用できる手法がいくつかあります。

- [インライン DataTemplate を作成する](#inline)。
- [Type を使用して DataTemplate を作成する](#type)。
- [リソースとして DataTemplate を作成する](#resource)。

使用する手法に関係なく、次のスクリーンショットに示すように、結果として [`ListView`](xref:Xamarin.Forms.ListView) の各セルの外観は [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) で定義されます。

![](creating-images/data-template-appearance.png "ListView with a DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>インライン DataTemplate を作成する

[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティはインライン [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定できます。 インライン テンプレートは、適切なコントロール プロパティの直接の子として配置されるものであり、データ テンプレートを別の場所で再利用する必要がない場合に使用するようにします。 次の XAML コード例のように、`DataTemplate` に指定された要素で各セルの外観を定義します。

```xaml
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
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

インライン [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) の子は、種類が [`Cell`](xref:Xamarin.Forms.Cell) か、そこから派生している必要があります。 この例では、`Cell` から派生した [`ViewCell`](xref:Xamarin.Forms.ViewCell) が使われます。 `ViewCell` 内のレイアウトは [`Grid`](xref:Xamarin.Forms.Grid) で管理されています。 `Grid` には 3 つの [`Label`](xref:Xamarin.Forms.Label) インスタンスが含まれており、その [`Text`](xref:Xamarin.Forms.Label.Text) プロパティをコレクション内の各 `Person` オブジェクトの適切なプロパティにバインドしています。

これと同じ C# コードの例は次のとおりです。

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

C# の場合、インライン [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、`Func` 引数を指定するコンストラクター オーバーロードを使用して作成されます。

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Type を使用して DataTemplate を作成する

[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティは、セルの種類から作成された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に設定することもできます。 このアプローチの利点は、アプリケーション全体で、セルの種類に定義された外観を複数のデータ テンプレートから再利用できることです。 このアプローチの例を次の XAML コードに示します。

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
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

ここでは、セルの外観を定義するカスタムの種類から作成された [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティが設定されています。 次のコード例に示すように、カスタムの種類は、種類 [`ViewCell`](xref:Xamarin.Forms.ViewCell) から派生している必要があります。

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

[`ViewCell`](xref:Xamarin.Forms.ViewCell) 内では、レイアウトは [`Grid`](xref:Xamarin.Forms.Grid) によってここで管理されます。 `Grid` には 3 つの [`Label`](xref:Xamarin.Forms.Label) インスタンスが含まれており、その [`Text`](xref:Xamarin.Forms.Label.Text) プロパティをコレクション内の各 `Person` オブジェクトの適切なプロパティにバインドしています。

C# での同等のコード例を次に示します。

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

C# の場合、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、引数としてセルの種類を指定するコンストラクター オーバーロードを使用して作成されます。 次のコード例に示すように、セルの種類は、種類 [`ViewCell`](xref:Xamarin.Forms.ViewCell) から派生している必要があります。

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Xamarin.Forms には、[`ListView`](xref:Xamarin.Forms.ListView) セルに単純なデータを表示するために使用できるセルの種類も含まれています。 詳細については、[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)に関するページを参照してください。

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>リソースとして DataTemplate を作成する

データ テンプレートは、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) で再利用可能なオブジェクトとして作成することもできます。 これを実現するには、次の XAML コード例に示すように、`ResourceDictionary` 内に説明的なキーを提供する一意の `x:Key` 属性を各宣言に指定します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

`StaticResource` マークアップ拡張を使用して、[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) が [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) プロパティに割り当てられます。 `DataTemplate` はページの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) で定義されていますが、コントロール レベルまたはアプリケーション レベルで定義することもできる点に注意してください。

C# での同等のページのコード例を次に示します。

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) は、[`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) メソッドを使用して [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加されます。これで、取得時に `DataTemplate` を参照するために使用される `Key` 文字列が指定されます。

## <a name="summary"></a>まとめ

この記事では、インライン、カスタムの種類、または [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) でデータ テンプレートを作成する方法について説明しました。 データ テンプレートを他の場所で再利用する必要がない場合は、インライン テンプレートを使用する必要があります。 または、データ テンプレートをカスタム型として定義することで、あるいは制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで、それを再利用できます。

## <a name="related-links"></a>関連リンク

- [セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [データ テンプレート (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
