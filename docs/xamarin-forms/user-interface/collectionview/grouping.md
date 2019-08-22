---
title: CollectionView グループ化
description: CollectionView では、IsGrouped 化プロパティを true に設定することによって、正しくグループ化されたデータを表示できます。
ms.prod: xamarin
ms.assetid: 7E494245-FDBD-49D6-B7FA-CEF976EB59BB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 8fd37999428c2813bbf96de3bcbd6ebd1fe0879d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69894029"
---
# <a name="xamarinforms-collectionview-grouping"></a>CollectionView グループ化

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos)

多くの場合、頻繁にスクロールするリストに表示すると、大きなデータセットが扱いにくくなる可能性があります。 このシナリオでは、データをグループにまとめると、データの移動が容易になるため、ユーザーエクスペリエンスが向上します。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)グループ化されたデータの表示をサポートし、表示方法を制御する次のプロパティを定義します。

- `IsGrouped`型`bool`のは、基になるデータをグループに表示するかどうかを示します。 このプロパティの既定値は `false` です。
- `GroupHeaderTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。各グループのヘッダーに使用するテンプレート。
- `GroupFooterTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。各グループのフッターに使用するテンプレート。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

> [!IMPORTANT]
> でのデータ[`CollectionView`](xref:Xamarin.Forms.CollectionView)のグループ化は、現在、iOS でのみサポートされています。

## <a name="group-data"></a>データのグループ化

データは、表示する前にグループ化する必要があります。 これを行うには、グループの一覧を作成します。各グループは項目のリストです。 グループの一覧は`IEnumerable<T>`コレクションである必要があります。ここ`T`で、は2つのデータを定義します。

- グループ名。
- グループに属する項目を定義するコレクション。`IEnumerable`

このため、データをグループ化するプロセスは次のようになります。

- 1つの項目をモデル化する型を作成します。
- 項目の1つのグループをモデル化する型を作成します。
- コレクションを作成します。は、単一の項目グループをモデル化する型です。`T` `IEnumerable<T>` このコレクションは、グループ化されたデータを格納するグループのコレクションです。
- `IEnumerable<T>`コレクションにデータを追加します。

### <a name="example"></a>例

データをグループ化する場合の最初の手順は、1つの項目をモデル化する型を作成することです。 次の例は、 `Animal`サンプルアプリケーションのクラスを示しています。

```csharp
public class Animal
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

クラス`Animal`は、1つの項目をモデル化します。 その後、項目のグループをモデル化する型を作成できます。 次の例は、 `AnimalGroup`サンプルアプリケーションのクラスを示しています。

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

クラス`AnimalGroup`は、 `List<T>`クラスから継承され、 `Name`グループ名を表すプロパティを追加します。

次`IEnumerable<T>`に、グループのコレクションを作成できます。

```csharp
public List<AnimalGroup> Animals { get; private set; } = new List<AnimalGroup>();
```

このコード`Animals` `AnimalGroup`は、コレクション内の各項目がオブジェクトであるという名前のコレクションを定義します。 各`AnimalGroup`オブジェクトは、名前`List<Animal>`と、グループ内の`Animal`オブジェクトを定義するコレクションで構成されます。

次に、 `Animals`グループ化されたデータをコレクションに追加できます。

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

このコードにより、 `Animals`コレクションに2つのグループが作成されます。 1つ`AnimalGroup`目は`Bears`という名前で`List<Animal>` 、詳細なコレクションが含まれています。 2番`AnimalGroup`目の`Monkeys`はという名前`List<Animal>`で、サルの詳細のコレクションが含まれています。

## <a name="display-grouped-data"></a>グループ化されたデータの表示

[`CollectionView`](xref:Xamarin.Forms.CollectionView)データが正しくグループ化されている場合は、 `IsGrouped`プロパティをに`true`設定して、グループ化されたデータを表示します。

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

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

の[`CollectionView`](xref:Xamarin.Forms.CollectionView)各項目の外観は、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティをに設定する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)ことによって定義されます。 詳細については、「[アイテムの外観を定義](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)する」を参照してください。

> [!NOTE]
> 既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、グループのヘッダーとフッターにグループ名が表示されます。 この動作は、グループヘッダーとグループフッターをカスタマイズすることによって変更できます。

## <a name="customize-the-group-header"></a>グループヘッダーをカスタマイズする

各グループヘッダーの外観は、 `CollectionView.GroupHeaderTemplate`プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによってカスタマイズできます。

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

この例では、各グループヘッダーは、グループ[`Label`](xref:Xamarin.Forms.Label)名を表示するに設定され、その他の外観プロパティが設定されています。

## <a name="customize-the-group-footer"></a>グループフッターをカスタマイズする

各グループフッターの外観は、 `CollectionView.GroupFooterTemplate`プロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)をに設定することによってカスタマイズできます。

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

この例では、各グループフッターは、グループ[`Label`](xref:Xamarin.Forms.Label)内の項目数を表示するに設定されています。

## <a name="empty-groups"></a>空のグループ

に[`CollectionView`](xref:Xamarin.Forms.CollectionView)グループ化されたデータが表示されると、空のグループが表示されます。 このようなグループは、グループのヘッダーとフッターと共に表示され、グループが空であることを示します。

> [!NOTE]
> IOS 10 以降では、 `CollectionView`空のグループのグループヘッダーとグループフッターがの先頭に表示されることがあります。

## <a name="group-without-templates"></a>テンプレートのないグループ

[`CollectionView`](xref:Xamarin.Forms.CollectionView)[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティをに設定せずに、正しくグループ化[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)されたデータを表示できます。

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true" />
```

このシナリオでは、1つの項目をモデル化`ToString`する型のメソッドをオーバーライドすることによって意味のあるデータを表示し、単一の項目グループをモデル化する型を表示できます。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
