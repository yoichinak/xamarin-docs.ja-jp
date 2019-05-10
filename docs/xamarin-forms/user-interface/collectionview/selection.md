---
title: Xamarin.Forms CollectionView の選択
description: 既定では、CollectionView 選択は無効です。 単一または複数選択を有効にすることができます。
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 1ffed60253889491636fa105dd444ced9c2bedf5
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048218"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin.Forms CollectionView の選択

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` 項目の選択を制御する次のプロパティを定義します。

- `SelectionMode`、型の`SelectionMode`、選択モード。
- `SelectedItem`、型の`object`、一覧で選択された項目。 このプロパティは、`null`項目が選択されていないときの値します。
- `SelectedItems`、型の`IList<object>`、一覧で項目を選択します。 このプロパティは読み取り専用であり、`null`項目が選択されていないときの値します。
- `SelectionChangedCommand`、型の`ICommand`、選択した項目が変更されたときに実行されます。
- `SelectionChangedCommandParameter`、型の`object`、に渡されるパラメーターは、`SelectionChangedCommand`します。

これらすべてのプロパティに支えは[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。

既定では、`CollectionView` の選択は無効になっていますが、 この動作を変更して、設定して、ただし、`SelectionMode`プロパティの値のいずれかを`SelectionMode`列挙型メンバー。

- `None` – 項目を選択できないことを示します。 これが既定値です。
- `Single` –、強調表示されている選択項目を 1 つの項目を選択できることを示します。
- `Multiple` – 複数の項目選択できるである、選択したアイテムを強調表示されていることを示します。

`CollectionView` 定義、`SelectionChanged`ときに発生するイベント、`SelectedItem`プロパティの変更、いずれかの理由、ユーザーまたはアプリケーションが、プロパティを設定すると、一覧から項目を選択します。 さらに、このイベントもときに発生する、`SelectedItems`プロパティの変更。 `SelectionChangedEventArgs`に付属しているオブジェクト、`SelectionChanged`イベントには 2 つのプロパティが両方の種類の`IReadOnlyList<object>`:

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
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> `SelectionChanged`変更した結果として発生した変更がイベントを発生させる、`SelectionMode`プロパティ。

次のスクリーン ショットで 1 つの項目の選択、 `CollectionView`:

[![IOS と Android で、1 つを選択した CollectionView 垂直方向の一覧のスクリーン ショット](selection-images/single-selection.png "CollectionView 垂直方向に 1 つを選択した一覧")](selection-images/single-selection-large.png#lightbox "CollectionView で単一の垂直方向の一覧選択")

## <a name="multiple-selection"></a>複数の選択

ときに、`SelectionMode`プロパティに設定されて`Multiple`での複数の項目、`CollectionView`を選択できます。 項目が選択されているときに、`SelectedItems`プロパティは、選択した項目に設定されます。 このプロパティが変更されたときに、`SelectionChangedCommand`が実行される (の値を持つ、`SelectionChangedCommandParameter`に渡される、 `ICommand`)、および`SelectionChanged`イベントが発生します。

次の XAML の例は、`CollectionView`複数項目の選択に応答することができます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

このコード例では、`OnCollectionViewSelectionChanged`イベント ハンドラーが実行されるときに、`SelectionChanged`イベント ハンドラーが、以前に選択した項目では、および現在の選択した項目を取得するイベントの起動。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> `SelectionChanged`変更した結果として発生した変更がイベントを発生させる、`SelectionMode`プロパティ。

次のスクリーン ショットで複数の項目の選択、 `CollectionView`:

[![IOS と Android での複数選択 CollectionView 垂直方向の一覧のスクリーン ショット](selection-images/multiple-selection.png "CollectionView 複数を選択した垂直方向に一覧")](selection-images/multiple-selection-large.png#lightbox "CollectionView を垂直方向に一覧複数の選択")

## <a name="single-pre-selection"></a>1 つの事前選択

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

## <a name="multiple-pre-selection"></a>複数の事前選択

ときに、`SelectionMode`プロパティに設定されて`Multiple`での複数の項目、`CollectionView`事前に選択できます。 次の XAML の例は、`CollectionView`を複数の項目の前の選択が有効になります。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple">
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

複数の項目、`CollectionView`追加することによってあらかじめ選択されて、`SelectedItems`プロパティ。

```csharp
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(1).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(3).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(4).FirstOrDefault());
```

> [!NOTE]
> `SelectedItems`プロパティが読み取り専用、およびため双方向のデータが事前に項目を選択するバインディングを使用することはできません。

そのため、ときに、`CollectionView`が表示されたら、2 番目、4 番目に、リスト内の 5 番目の項目があらかじめ選択と。

[![IOS と Android での複数の事前選択 CollectionView 垂直方向の一覧のスクリーン ショット](selection-images/multiple-pre-selection.png "CollectionView 垂直方向に一覧で複数の事前選択")](selection-images/multiple-pre-selection-large.png#lightbox "CollectionView 垂直複数の事前選択されたリスト")

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

表示状態の詳細については、次を参照してください。 [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

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
