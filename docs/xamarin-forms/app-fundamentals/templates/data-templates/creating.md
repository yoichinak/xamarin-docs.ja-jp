---
title: DataTemplate を作成します。
description: ResourceDictionary、またはカスタムの型または適切な Xamarin.Forms セルの種類からにより、データ テンプレートにインラインでを作成できます。 この記事では、各テクニックについて説明します。
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: cd4266fb8e7d68a9f93bd169ca70c6a0f516a357
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplate"></a>DataTemplate を作成します。

_ResourceDictionary、またはカスタムの型または適切な Xamarin.Forms セルの種類からにより、データ テンプレートにインラインでを作成できます。この記事では、各テクニックについて説明します。_

一般的な使用シナリオ、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)内のオブジェクトのコレクションからデータを表示する、 [ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)です。 内の各セルのデータの外観、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を設定して管理することができます、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)プロパティを[ `DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)です。 これを実現するために使用する方法の数があります。

- [インライン DataTemplate を作成する](#inline)です。
- [DataTemplate 型を作成する、](#type)です。
- [リソースとしてデータ テンプレートを作成する](#resource)です。

使用されている手法に関係なく、結果を内の各セルの外観、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)によって定義された、 [ `DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)次のスクリーン ショットに示すように、します。

![](creating-images/data-template-appearance.png "DataTemplate を含む ListView")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>インライン DataTemplate を作成します。

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)インラインにプロパティを設定することができます[ `DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)です。 データ テンプレートを他の場所を再利用する必要がない場合、1 つである適切なコントロールの直接の子として配置される、インライン テンプレートを使用してください。 指定した要素を`DataTemplate`の XAML コードの例を次に示すように、各セルの外観を定義します。

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

インラインの子[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)で、またはから派生して、入力する必要があります[ `ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)です。 レイアウト内、`ViewCell`でここで管理されて、 [ `Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)です。 `Grid` 3 種類を含む[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bind をインスタンス化、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)それぞれの適切なプロパティをプロパティ`Person`コレクション内のオブジェクト。

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

C# の場合、インライン[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)を指定するコンス トラクター オーバー ロードを使用して作成された、`Func`引数。

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>型を持つ DataTemplate を作成します。

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)プロパティを設定することも、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)セルの種類から作成します。 この方法の利点は、アプリケーション全体で複数のデータ テンプレートでのセルの型によって定義された外観を再利用できることです。 次の XAML コードは、この方法の例を示します。

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

ここでは、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)プロパティに設定されている、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)セルの外観を定義するカスタム型から作成します。 カスタムの型が型から派生する必要があります[ `ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)の次のコード例に示すようにします。

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

内で、 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)、レイアウトがここで管理されている、 [ `Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)です。 `Grid` 3 種類を含む[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bind をインスタンス化、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)それぞれの適切なプロパティをプロパティ`Person`コレクション内のオブジェクト。

次の例では同等の c# コードが表示されます。

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

C# で、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)セルの種類を引数として指定するコンス トラクター オーバー ロードを使用して作成します。 セルの種類は、型から派生する必要があります[ `ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)の次のコード例に示すようにします。

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
> Xamarin.Forms は単純なデータを表示するために使用するセルの種類も含まれることに注意してください[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)セル。 詳細については、次を参照してください。[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)です。

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>DataTemplate をリソースとして作成します。

再利用可能なオブジェクトとしてデータ テンプレートを作成することも、 [ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。 これは、各宣言を一意に付けることで実現`x:Key`のわかりやすいキーで利用可能になる属性、`ResourceDictionary`次の XAML コードの例のように。

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

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)に割り当てられている、 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)プロパティを使用して、`StaticResource`マークアップ拡張機能です。 注意してください、`DataTemplate`ページのでは定義されている[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、コントロール レベルまたはアプリケーション レベルで定義することもできます。

次のコード例では、C# の場合と同じページを示します。

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

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)に追加、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を使用して、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/)指定する、メソッド、`Key`するために使用される文字列参照、`DataTemplate`それを取得するときにします。

## <a name="summary"></a>まとめ

この記事は、カスタム型からまたはインラインでのデータ テンプレートを作成する方法を説明しましたが、 [ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。 データ テンプレートを他の場所を再利用する必要がない場合は、インライン テンプレートを使用してください。 代わりに、データ テンプレートは、カスタムの型、または、コントロール レベル、ページ レベル、またはアプリケーション レベルのリソースとして定義することで再利用できます。


## <a name="related-links"></a>関連リンク

- [セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [データ テンプレート (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
