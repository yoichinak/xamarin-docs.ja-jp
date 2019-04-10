---
title: Xamarin.Forms CollectionView 選択モードを設定します。
description: 既定では、CollectionView 選択は無効です。 ただし、1 つまたは複数選択を有効にすることができます。
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/18/2019
ms.openlocfilehash: 441afb9348a85de61d35574bb9121c7de713a897
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329953"
---
# <a name="set-collectionview-selection-mode"></a>CollectionView 選択モードを設定します。

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView`は現在プレビュー段階で、その計画的な機能の一部が不足しています。 さらに、実装が完了すると、API を変更することがあります。

`CollectionView` 項目の選択を制御する次のプロパティを定義します。

- `SelectionMode`、型の`SelectionMode`、選択モード。
- `SelectedItem`、型の`object`、一覧で選択された項目。 このプロパティは、`null`項目が選択されていないときの値します。
- `SelectionChangedCommand`、型の`ICommand`、選択した項目が変更されたときに実行されます。
- `SelectionChangedCommandParameter`、型の`object`、に渡されるパラメーターは、`SelectionChangedCommand`します。

これらすべてのプロパティに支えは[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。

既定では、`CollectionView`選択が無効になっています。 この動作を変更して、設定して、ただし、`SelectionMode`プロパティの値のいずれかを`SelectionMode`列挙型メンバー。

- `None` – 項目を選択できないことを示します。 これが既定値です。
- `Single` –、強調表示されている選択項目を 1 つの項目を選択できることを示します。
- `Multiple` – 複数の項目選択できるである、選択したアイテムを強調表示されていることを示します。

`CollectionView` 定義、`SelectionChanged`ときに発生するイベント、`SelectedItem`プロパティの変更、いずれかの理由、ユーザーまたはアプリケーションが、プロパティを設定すると、一覧から項目を選択します。 `SelectionChangedEventArgs`に付属しているオブジェクト、`SelectionChanged`イベントには 2 つのプロパティが両方の種類の`IReadOnlyList<object>`:

- `PreviousSelection` – 選択範囲が変更される前に、選択された項目の一覧。
- `CurrentSelection` – 選択の変更後に、選択された項目の一覧。

## <a name="single-selection"></a>単一の選択

ときに、`SelectionMode`プロパティに設定されて`Single`、1 つの項目で、`CollectionView`を選択できます。 項目が選択されているときに、`SelectedItem`プロパティは、選択した項目の値に設定されます。 このプロパティが変更されたときに、`SelectionChangedCommand`が実行される (の値を持つ、`SelectionChangedCommandParameter`に渡される、 `ICommand`)、および`SelectionChanged`イベントが発生します。

次の XAML の例は、`CollectionView`単一アイテムの選択に応答することができます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

このコード例では、`OnCollectionViewSelectionChanged`イベント ハンドラーが実行されるときに、`SelectionChanged`を以前に選択したアイテム、および現在の選択項目を取得するイベント ハンドラーのイベントの起動。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (previousSelectedItems.FirstOrDefault() as Monkey)?.Name;
    string current = (currentSelectedItems.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> `SelectionChanged`変更した結果として発生した変更がイベントを発生させる、`SelectionMode`プロパティ。

次のスクリーン ショットで 1 つの項目の選択、 `CollectionView`:

[![IOS と Android で、1 つを選択した CollectionView 垂直方向の一覧のスクリーン ショット](selection-images/single-selection.png "CollectionView 垂直方向に 1 つを選択した一覧")](selection-images/single-selection-large.png#lightbox "CollectionView で単一の垂直方向の一覧選択")

## <a name="pre-selection"></a>前の選択

ときに、`SelectionMode`プロパティに設定されて`Single`、1 つの項目で、`CollectionView`設定であらかじめ選択されて、`SelectedItem`プロパティ項目を。 次の XAML の例は、`CollectionView`事前に 1 つの項目を選択します。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
    ...
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

`SelectedItem`プロパティ データにバインド、`SelectedMonkey`プロパティの種類は、接続されているビュー モデルの`Monkey`します。 A`TwoWay`バインドを使用する場合に、ユーザーが、選択した項目の値を変更するため、`SelectedMonkey`プロパティの設定に、選択した`Monkey`オブジェクト。 `SelectedMonkey`でプロパティが定義されている、`MonkeysViewModel`クラスし、の 4 番目の項目に設定されている、`Monkeys`コレクション。

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

そのため、ときに、`CollectionView`が表示されたら、一覧の 4 番目の項目が事前選択されています。

[![IOS と Android での 1 つの事前選択で CollectionView 垂直方向の一覧のスクリーン ショット](selection-images/single-pre-selection.png "CollectionView 垂直方向に一覧で 1 つの事前選択")](selection-images/single-pre-selection-large.png#lightbox "CollectionView 垂直方向の一覧1 つ前の選択")

## <a name="change-selected-item-color"></a>選択した項目の色を変更します。

`CollectionView` `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState)で選択されたアイテムを視覚的な変更の開始に使用できる、`CollectionView`します。 このユース ケースを共通の`VisualState`の次の XAML の例に示すは、選択した項目の背景色を変更することです。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> [ `Style` ](xref:Xamarin.Forms.Style)を格納している、 `Selected` `VisualState`必要があります、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティ値のルート要素の型が、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)、として設定されています、`ItemTemplate`プロパティの値。

この例で、 [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティの値に設定されて`Grid`ためのルート要素、`ItemTemplate`は、 [ `Grid`](xref:Xamarin.Forms.Grid)します。 `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState)内の項目のときに、ことを指定します、`CollectionView`が選択されている、 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)に設定されますアイテムの`LightSkyBlue`:

[![IOS と Android での単一選択のカスタム色で CollectionView 垂直方向の一覧のスクリーン ショット](selection-images/single-selection-color.png "単一選択のカスタム色で縦方向のリストを CollectionView") ] (selection-images/single-selection-color-large.png#lightbox "単一選択のカスタム色で縦方向のリストを CollectionView")

表示状態の詳細については、[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)を参照してください。

## <a name="disable-selection"></a>選択範囲を無効にします。

`CollectionView` 既定では、選択範囲が無効です。 ただし場合、`CollectionView`が有効にすると、選択範囲を設定して無効にすることができます、`SelectionMode`プロパティを`None`:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

ときに、`SelectionMode`プロパティに設定されて`None`、項目を`CollectionView`選択することはできません、`SelectedItem`プロパティが引き続き`null`、および`SelectionChanged`イベントは発生しません。

> [!NOTE]
> 項目が選択されている場合、`SelectionMode`からプロパティを変更`Single`に`None`、`SelectedItem`プロパティに設定する`null`と`SelectionChanged`空のときに発生するイベント`CurrentSelection`プロパティ。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
