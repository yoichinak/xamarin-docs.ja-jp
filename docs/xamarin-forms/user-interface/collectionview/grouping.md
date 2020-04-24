---
title: CollectionView グループ化
description: CollectionView では、IsGrouped 化プロパティを true に設定することによって、正しくグループ化されたデータを表示できます。
ms.prod: xamarin
ms.assetid: 7E494245-FDBD-49D6-B7FA-CEF976EB59BB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 8360123b01f36bde084b4dc315109e6bdaef2207
ms.sourcegitcommit: 99aa05bd9b5e3f66d134066b860f41b54fa2d850
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103283"
---
# <a name="xamarinforms-collectionview-grouping"></a>CollectionView グループ化

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

多くの場合、頻繁にスクロールするリストに表示すると、大きなデータセットが扱いにくくなる可能性があります。 このシナリオでは、データをグループにまとめると、データの移動が容易になるため、ユーザーエクスペリエンスが向上します。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)グループ化されたデータの表示をサポートし、表示方法を制御する次のプロパティを定義します。

- `IsGrouped`型`bool`のは、基になるデータをグループに表示するかどうかを示します。 このプロパティの既定値は `false` です。
- `GroupHeaderTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。各グループのヘッダーに使用するテンプレート。
- `GroupFooterTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。各グループのフッターに使用するテンプレート。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

次のスクリーンショットは[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、グループ化されたデータの表示を示しています。

[![CollectionView のグループ化されたデータのスクリーンショット (iOS と Android)](grouping-images/grouped-data.png "グループ化されたデータを含む CollectionView")](grouping-images/grouped-data-large.png#lightbox "グループ化されたデータを含む CollectionView")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="group-data"></a>データのグループ化

データは、表示する前にグループ化する必要があります。 これを行うには、グループの一覧を作成します。各グループは項目のリストです。 グループの一覧は`IEnumerable<T>`コレクションである必要があり`T`ます。ここで、は2つのデータを定義します。

- グループ名。
- グループ`IEnumerable`に属する項目を定義するコレクション。

このため、データをグループ化するプロセスは次のようになります。

- 1つの項目をモデル化する型を作成します。
- 項目の1つのグループをモデル化する型を作成します。
- `IEnumerable<T>`コレクションを作成します`T` 。は、単一の項目グループをモデル化する型です。 このコレクションは、グループ化されたデータを格納するグループのコレクションです。
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

このコードは、コレクション内`Animals`の各項目が`AnimalGroup`オブジェクトであるという名前のコレクションを定義します。 各`AnimalGroup`オブジェクトは、名前と、グループ`List<Animal>`内の`Animal`オブジェクトを定義するコレクションで構成されます。

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
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
    },
    new Animal
    {
        Name = "Capuchin Monkey",
        Location = "Central & South America",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
    },
    new Animal
    {
        Name = "Blue Monkey",
        Location = "Central and East Africa",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
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

該当の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

の[`CollectionView`](xref:Xamarin.Forms.CollectionView)各項目の外観は、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティをに設定することによっ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)て定義されます。 詳細については、「[アイテムの外観を定義](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)する」を参照してください。

> [!NOTE]
> 既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、グループのヘッダーとフッターにグループ名が表示されます。 この動作は、グループヘッダーとグループフッターをカスタマイズすることによって変更できます。

## <a name="customize-the-group-header"></a>グループヘッダーをカスタマイズする

各グループヘッダーの外観は、 `CollectionView.GroupHeaderTemplate`プロパティをに設定することによっ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)てカスタマイズできます。

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

この例では、各グループヘッダーは、グループ[`Label`](xref:Xamarin.Forms.Label)名を表示するに設定され、その他の外観プロパティが設定されています。 次のスクリーンショットは、カスタマイズされたグループヘッダーを示しています。

[![IOS と Android での CollectionView のカスタマイズされたグループヘッダーのスクリーンショット](grouping-images/customized-header.png "カスタマイズされたグループヘッダーを含む CollectionView")](grouping-images/customized-header-large.png#lightbox "カスタマイズされたグループヘッダーを含む CollectionView")

## <a name="customize-the-group-footer"></a>グループフッターをカスタマイズする

各グループフッターの外観は、 `CollectionView.GroupFooterTemplate`プロパティをに設定することによっ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)てカスタマイズできます。

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

この例では、各グループフッターは、グループ[`Label`](xref:Xamarin.Forms.Label)内の項目数を表示するに設定されています。 次のスクリーンショットは、カスタマイズされたグループフッターを示しています。

[![IOS と Android での CollectionView のカスタマイズされたグループフッターのスクリーンショット](grouping-images/customized-footer.png "カスタマイズされたグループフッターを含む CollectionView")](grouping-images/customized-footer-large.png#lightbox "カスタマイズされたグループフッターを含む CollectionView")

## <a name="empty-groups"></a>空のグループ

に[`CollectionView`](xref:Xamarin.Forms.CollectionView)グループ化されたデータが表示されると、空のグループが表示されます。 このようなグループは、グループのヘッダーとフッターと共に表示され、グループが空であることを示します。 次のスクリーンショットは、空のグループを示しています。

[![IOS と Android の CollectionView の空のグループのスクリーンショット](grouping-images/empty-group.png "空のグループを含む CollectionView")](grouping-images/empty-group-large.png#lightbox "空のグループを含む CollectionView")

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

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
