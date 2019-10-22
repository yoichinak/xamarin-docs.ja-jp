---
title: CollectionView グループ化
description: CollectionView では、IsGrouped 化プロパティを true に設定することによって、正しくグループ化されたデータを表示できます。
ms.prod: xamarin
ms.assetid: 7E494245-FDBD-49D6-B7FA-CEF976EB59BB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 0df3b082d6a3a4ebd64627082b2ac56dd0836e81
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696747"
---
# <a name="xamarinforms-collectionview-grouping"></a>CollectionView グループ化

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos)

多くの場合、頻繁にスクロールするリストに表示すると、大きなデータセットが扱いにくくなる可能性があります。 このシナリオでは、データをグループにまとめると、データの移動が容易になるため、ユーザーエクスペリエンスが向上します。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、グループ化されたデータの表示をサポートし、次のプロパティを定義します。

- `bool` 型の `IsGrouped` は、基になるデータをグループに表示するかどうかを示します。 このプロパティの既定値は `false` です。
- 各グループのヘッダーに使用するテンプレート[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の `GroupHeaderTemplate`。
- 各グループのフッターに使用するテンプレート[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の `GroupFooterTemplate`。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

次のスクリーンショットは、グループ化されたデータを表示する[`CollectionView`](xref:Xamarin.Forms.CollectionView)を示しています。

[![CollectionView のグループ化されたデータのスクリーンショット (iOS と Android)](grouping-images/grouped-data.png "グループ化されたデータを含む CollectionView")](grouping-images/grouped-data-large.png#lightbox "グループ化されたデータを含む CollectionView")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="group-data"></a>データのグループ化

データは、表示する前にグループ化する必要があります。 これを行うには、グループの一覧を作成します。各グループは項目のリストです。 グループの一覧は `IEnumerable<T>` コレクションである必要があります。ここで `T` は2つのデータを定義します。

- グループ名。
- グループに属する項目を定義する `IEnumerable` コレクション。

このため、データをグループ化するプロセスは次のようになります。

- 1つの項目をモデル化する型を作成します。
- 項目の1つのグループをモデル化する型を作成します。
- @No__t_0 コレクションを作成します。 `T` は、1つの項目グループをモデル化する型です。 このコレクションは、グループ化されたデータを格納するグループのコレクションです。
- @No__t_0 コレクションにデータを追加します。

### <a name="example"></a>例

データをグループ化する場合の最初の手順は、1つの項目をモデル化する型を作成することです。 次の例は、サンプルアプリケーションの `Animal` クラスを示しています。

```csharp
public class Animal
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

@No__t_0 クラスは、1つの項目をモデル化します。 その後、項目のグループをモデル化する型を作成できます。 次の例は、サンプルアプリケーションの `AnimalGroup` クラスを示しています。

```csharp
public class AnimalGroup : List<Animal>
{
    public string Name { get; private set; }

    public AnimalGroup(string name, List<Animal> animals) : base(animals)
    {
        Name = name;
    }
}
```

@No__t_0 クラスは `List<T>` クラスから継承され、グループ名を表す `Name` プロパティを追加します。

グループの `IEnumerable<T>` コレクションを作成できます。

```csharp
public List<AnimalGroup> Animals { get; private set; } = new List<AnimalGroup>();
```

このコードは `Animals` という名前のコレクションを定義します。コレクション内の各項目は `AnimalGroup` オブジェクトです。 各 `AnimalGroup` オブジェクトは、名前と、グループ内の `Animal` オブジェクトを定義する `List<Animal>` コレクションで構成されます。

次に、グループ化されたデータを `Animals` コレクションに追加できます。

```csharp
Animals.Add(new AnimalGroup("Bears", new List<Animal>
{
    new Animal
    {
        Name = "American Black Bear",
        Location = "North America",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/0/08/01_Schwarzbär.jpg"
    },
    new Animal
    {
        Name = "Asian Black Bear",
        Location = "Asia",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG/180px-Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG"
    },
    // ...
}));

Animals.Add(new AnimalGroup("Monkeys", new List<Animal>
{
    new Animal
    {
        Name = "Baboon",
        Location = "Africa & Asia",
        Details = "Details about the monkey go here.",
        ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
    },
    new Animal
    {
        Name = "Capuchin Monkey",
        Location = "Central & South America",
        Details = "Details about the monkey go here.",
        ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
    },
    new Animal
    {
        Name = "Blue Monkey",
        Location = "Central and East Africa",
        Details = "Details about the monkey go here.",
        ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
    },
    // ...
}));
```

このコードでは、`Animals` コレクションに2つのグループを作成します。 最初の `AnimalGroup` には `Bears` という名前が付けられ、詳細の `List<Animal>` コレクションが含まれています。 2番目の `AnimalGroup` には `Monkeys` という名前が付けられ、[サルの詳細] の `List<Animal>` コレクションが含まれています。

## <a name="display-grouped-data"></a>グループ化されたデータの表示

データが正しくグループ化されている場合は、[`IsGrouped`] プロパティを `true` に設定すると、グループ化されたデータが[`CollectionView`](xref:Xamarin.Forms.CollectionView)に表示されます。

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                ...
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

[@No__t_1](xref:Xamarin.Forms.CollectionView)内の各項目の外観は、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義されます。 詳細については、「[アイテムの外観を定義](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)する」を参照してください。

> [!NOTE]
> 既定では、グループのヘッダーとフッターにグループ名が表示さ[`CollectionView`](xref:Xamarin.Forms.CollectionView)ます。 この動作は、グループヘッダーとグループフッターをカスタマイズすることによって変更できます。

## <a name="customize-the-group-header"></a>グループヘッダーをカスタマイズする

各グループヘッダーの外観をカスタマイズするには、`CollectionView.GroupHeaderTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupHeaderTemplate>
        <DataTemplate>
            <Label Text="{Binding Name}"
                   BackgroundColor="LightGray"
                   FontSize="Large"
                   FontAttributes="Bold" />
        </DataTemplate>
    </CollectionView.GroupHeaderTemplate>
</CollectionView>
```

この例では、各グループヘッダーは、グループ名を表示する[`Label`](xref:Xamarin.Forms.Label)に設定されており、その他の外観プロパティが設定されています。 次のスクリーンショットは、カスタマイズされたグループヘッダーを示しています。

[![IOS と Android での CollectionView のカスタマイズされたグループヘッダーのスクリーンショット](grouping-images/customized-header.png "カスタマイズされたグループヘッダーを含む CollectionView")](grouping-images/customized-header-large.png#lightbox "カスタマイズされたグループヘッダーを含む CollectionView")

## <a name="customize-the-group-footer"></a>グループフッターをカスタマイズする

各グループフッターの外観をカスタマイズするには、[`CollectionView.GroupFooterTemplate`] プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定します。

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupFooterTemplate>
        <DataTemplate>
            <Label Text="{Binding Count, StringFormat='Total animals: {0:D}'}"
                   Margin="0,0,0,10" />
        </DataTemplate>
    </CollectionView.GroupFooterTemplate>
</CollectionView>
```

この例では、各グループフッターは、グループ内の項目数を表示する[`Label`](xref:Xamarin.Forms.Label)に設定されています。 次のスクリーンショットは、カスタマイズされたグループフッターを示しています。

[![IOS と Android での CollectionView のカスタマイズされたグループフッターのスクリーンショット](grouping-images/customized-footer.png "カスタマイズされたグループフッターを含む CollectionView")](grouping-images/customized-footer-large.png#lightbox "カスタマイズされたグループフッターを含む CollectionView")

## <a name="empty-groups"></a>空のグループ

[@No__t_1](xref:Xamarin.Forms.CollectionView)にグループ化されたデータが表示されると、空のグループが表示されます。 このようなグループは、グループのヘッダーとフッターと共に表示され、グループが空であることを示します。 次のスクリーンショットは、空のグループを示しています。

[![IOS と Android の CollectionView の空のグループのスクリーンショット](grouping-images/empty-group.png "空のグループを含む CollectionView")](grouping-images/empty-group-large.png#lightbox "空のグループを含む CollectionView")

> [!NOTE]
> IOS 10 以降では、空のグループのグループヘッダーとフッターは、すべて `CollectionView` の先頭に表示される場合があります。

## <a name="group-without-templates"></a>テンプレートのないグループ

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定しなくても、適切にグループ化されたデータを表示できます。

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true" />
```

このシナリオでは、1つの項目をモデル化する型の `ToString` メソッドと、1つの項目グループをモデル化する型をオーバーライドすることによって、意味のあるデータを表示できます。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
