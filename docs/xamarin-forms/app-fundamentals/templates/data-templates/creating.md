---
title: Xamarin.Forms の DataTemplate の作成
description: ResourceDictionary、またはカスタムの型または適切な Xamarin.Forms セルの種類により、データ テンプレートにインラインでを作成できます。 この記事では、各手法について説明します。
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994616"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Xamarin.Forms の DataTemplate の作成

_ResourceDictionary、またはカスタムの型または適切な Xamarin.Forms セルの種類により、データ テンプレートにインラインでを作成できます。この記事では、各手法について説明します。_

一般的な使用シナリオを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)内のオブジェクトのコレクションからデータを表示する、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 内の各セルのデータの外観、 [ `ListView` ](xref:Xamarin.Forms.ListView)を設定して管理することができます、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)プロパティを[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。 これを実現するために使用できる手法を数多くあります。

- [インラインのデータ テンプレートを作成する](#inline)します。
- [DataTemplate を作成する型を持つ](#type)します。
- [リソースとしての DataTemplate を作成する](#resource)します。

使用されている手法に関係なくその結果、内の各セルの外観、 [ `ListView` ](xref:Xamarin.Forms.ListView)によって定義されます、 [ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)の次のスクリーン ショットに示しますように。

![](creating-images/data-template-appearance.png "DataTemplate で ListView")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>インラインのデータ テンプレートを作成します。

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)インラインにプロパティを設定することができます[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。 データ テンプレートを他の場所を再利用する必要がない場合は、適切なコントロールの直接の子として配置されている 1 つは、インライン テンプレートを使用してください。 指定した要素、`DataTemplate`の XAML コードの例を次に示すように、各セルの外観を定義します。

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

インラインの子[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)で、またはから派生して、入力する必要があります[ `ViewCell`](xref:Xamarin.Forms.ViewCell)します。 内のレイアウト、`ViewCell`をここで管理される、 [ `Grid`](xref:Xamarin.Forms.Grid)します。 `Grid` 3 種類を含む[ `Label` ](xref:Xamarin.Forms.Label)そのバインドのインスタンス、 [ `Text` ](xref:Xamarin.Forms.Label.Text)それぞれの適切なプロパティをプロパティ`Person`コレクション内のオブジェクト。

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

C# のインライン[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を指定するコンス トラクター オーバー ロードを使用して作成、`Func`引数。

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>型を持つ、DataTemplate を作成します。

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)プロパティを設定することも、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)セルの種類から作成されました。 このアプローチの利点は、アプリケーション全体で複数のデータ テンプレートで、セルの種類によって定義された外観を再利用できることです。 次の XAML コードは、この方法の例を示します。

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

ここでは、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)プロパティに設定されて、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)セルの外観を定義するカスタム型から作成されました。 カスタムの型が型から派生する必要があります[ `ViewCell`](xref:Xamarin.Forms.ViewCell)次のコード例のように。

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

内で、 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)、レイアウトがここで管理される、 [ `Grid`](xref:Xamarin.Forms.Grid)します。 `Grid` 3 種類を含む[ `Label` ](xref:Xamarin.Forms.Label)そのバインドのインスタンス、 [ `Text` ](xref:Xamarin.Forms.Label.Text)それぞれの適切なプロパティをプロパティ`Person`コレクション内のオブジェクト。

同等の c# コードは次の例で示すように。

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

C# で、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)セルの種類を引数として指定するコンス トラクター オーバー ロードを使用して作成されます。 セルの種類は、型から派生する必要があります[ `ViewCell`](xref:Xamarin.Forms.ViewCell)次のコード例のように。

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
> Xamarin.Forms は単純なデータを表示するために使用するセルの種類も含まれることに注意してください。 [ `ListView` ](xref:Xamarin.Forms.ListView)セル。 詳細については、次を参照してください。[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)します。

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>リソースとしての DataTemplate の作成

再利用可能なオブジェクトとしてデータ テンプレートを作成することも、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 これは、一意の各宣言指定することにより実現されます`x:Key`、属性のわかりやすいキーで利用可能になる、`ResourceDictionary`次の XAML コード例のように。

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)に割り当てられている、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)プロパティを使用して、`StaticResource`マークアップ拡張機能。 注意してください、`DataTemplate`ページの定義は[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、コントロール レベルまたはアプリケーション レベルで定義することもできます。

次のコード例では、c# では、同等のページを示します。

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)に追加されます、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を使用して、 [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object))指定する、メソッド、`Key`するために使用される文字列参照、`DataTemplate`それを取得するときにします。

## <a name="summary"></a>まとめ

この記事がやカスタムの型からインラインでのデータ テンプレートを作成する方法を説明した、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 データ テンプレートを他の場所を再利用する必要がない場合、インライン テンプレートを使用する必要があります。 または、データ テンプレートは、カスタムの型、または制御レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで再利用できます。


## <a name="related-links"></a>関連リンク

- [セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
