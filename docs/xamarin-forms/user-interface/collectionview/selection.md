---
title: Xamarin.Forms CollectionView の選択
description: 既定では、CollectionView の選択は無効になっています。 ただし、1つまたは複数の選択を有効にすることができます。
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fd4ac89b60cdac8509e5c057c698ef79f7e6e969
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375344"
---
# <a name="no-locxamarinforms-collectionview-selection"></a>Xamarin.Forms CollectionView の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目の選択を制御する次のプロパティを定義します。

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)選択モードである、型の [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) 。
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)型の、 `object` リスト内の選択された項目。 このプロパティの既定のバインディングモードは `TwoWay` で、 `null` 項目が選択されていない場合は値が設定されます。
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)型の、 `IList<object>` リスト内の選択された項目。 このプロパティの既定のバインディングモードは `OneWay` で、項目が選択されていない場合は値が設定され `null` ます。
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)型の `ICommand` 。選択した項目が変更されたときに実行されます。
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)型の `object` 。これは、に渡されるパラメーターです `SelectionChangedCommand` 。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

既定で [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、選択は無効になっています。 ただし、この動作は、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) プロパティ値を列挙体のメンバーの1つに設定することによって変更でき [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) ます。

- `None` –項目を選択できないことを示します。 これが既定値です。
- `Single` –1つの項目を選択し、選択した項目を強調表示することを示します。
- `Multiple` –選択された項目が強調表示されている複数の項目を選択できることを示します。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) ユーザーが一覧から項目を選択したか、アプリケーションがプロパティを設定したときに、プロパティが変更されたときに発生するイベントを定義します。 また、このイベントは、プロパティが変更されたときにも発生し [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) ます。 [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs)イベントに付随するオブジェクトに `SelectionChanged` は、次の2種類のプロパティがあり `IReadOnlyList<object>` ます。

- `PreviousSelection` –選択項目が変更される前に選択された項目の一覧。
- `CurrentSelection` –選択の変更後に選択された項目の一覧。

さらに、には、選択した [`CollectionView`](xref:Xamarin.Forms.CollectionView) `UpdateSelectedItems` 項目のリストを使用してプロパティを更新するメソッドがありますが、 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 1 つの変更通知のみを実行します。

## <a name="single-selection"></a>単一選択

プロパティがに設定されている場合は、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `Single` 内の1つの項目を [`CollectionView`](xref:Xamarin.Forms.CollectionView) 選択できます。 項目を選択すると、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) プロパティは選択した項目の値に設定されます。 このプロパティが変更されると、 [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) が実行され (に渡されるの値によって [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) )、イベントが発生し `ICommand` [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) ます。

次の XAML の例は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 単一の項目の選択に応答できるを示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

このコード例では、イベント `OnCollectionViewSelectionChanged` が発生したときにイベントハンドラーが実行され、前に選択した項目を [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 取得するイベントハンドラーと、現在選択されている項目を取得します。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントは、プロパティを変更した結果として発生する変更によって発生する可能性があり [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) ます。

次のスクリーンショットは、での単一項目の選択を示してい [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。

[![IOS と Android での単一選択による CollectionView 縦の一覧のスクリーンショット](selection-images/single-selection.png "1つの選択項目を含む CollectionView 縦の一覧")](selection-images/single-selection-large.png#lightbox "1つの選択項目を含む CollectionView 縦の一覧")

## <a name="multiple-selection"></a>複数選択

プロパティがに設定されている場合は [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `Multiple` 、内の複数の項目を [`CollectionView`](xref:Xamarin.Forms.CollectionView) 選択できます。 項目を選択すると、 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) プロパティは選択した項目に設定されます。 このプロパティが変更されると、 [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) が実行され (に渡されるの値によって [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) )、イベントが発生し `ICommand` [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) ます。

次の XAML の例は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 複数の項目の選択に応答できるを示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

このコード例では、イベントの発生時にイベントハンドラーが実行され、 `OnCollectionViewSelectionChanged` 以前に選択された項目を取得するイベントハンドラーと、 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 現在選択されている項目を取得します。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントは、プロパティを変更した結果として発生する変更によって発生する可能性があり [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) ます。

次のスクリーンショットは、での複数の項目の選択を示してい [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。

[![IOS と Android での複数選択がある CollectionView 縦の一覧のスクリーンショット](selection-images/multiple-selection.png "複数選択を含む CollectionView の一覧")](selection-images/multiple-selection-large.png#lightbox "複数選択を含む CollectionView の一覧")

## <a name="single-pre-selection"></a>1つの事前選択

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティがに設定され `Single` ている場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) プロパティを項目に設定することによって、内の1つの項目を事前選択できます。 次の XAML の例は、 `CollectionView` 1 つの項目を事前に選択するを示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> プロパティには、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) の既定のバインディングモードがあり `TwoWay` ます。

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティデータは、 `SelectedMonkey` 接続されたビューモデルのプロパティにバインドされます。これは型 `Monkey` です。 既定では、ユーザーが選択した項目を変更した場合に、選択した `TwoWay` オブジェクトにプロパティの値が設定されるように、バインドが使用され `SelectedMonkey` `Monkey` ます。 `SelectedMonkey`プロパティはクラスで定義され、 `MonkeysViewModel` コレクションの4番目の項目に設定され `Monkeys` ます。

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

そのため、が [`CollectionView`](xref:Xamarin.Forms.CollectionView) 表示されたら、一覧の4番目の項目が事前に選択されています。

[![IOS と Android での1つの事前選択を含む CollectionView の一覧のスクリーンショット](selection-images/single-pre-selection.png "1つの事前選択を含む CollectionView 縦の一覧")](selection-images/single-pre-selection-large.png#lightbox "1つの事前選択を含む CollectionView 縦の一覧")

## <a name="multiple-pre-selection"></a>複数の事前選択

プロパティがに設定されている場合は [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `Multiple` 、内の複数の項目を [`CollectionView`](xref:Xamarin.Forms.CollectionView) 事前に選択できます。 次の XAML の例は、 `CollectionView` 複数の項目の事前選択を有効にするを示しています。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> プロパティには、 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) の既定のバインディングモードがあり `OneWay` ます。

[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティデータは、 `SelectedMonkeys` 接続されたビューモデルのプロパティにバインドされます。これは型 `ObservableCollection<object>` です。 `SelectedMonkeys`プロパティはクラスで定義され、 `MonkeysViewModel` コレクション内の2番目、4番目、および5番目の項目に設定され `Monkeys` ます。

```csharp
namespace CollectionViewDemos.ViewModels
{
    public class MonkeysViewModel : INotifyPropertyChanged
    {
        ...
        ObservableCollection<object> selectedMonkeys;
        public ObservableCollection<object> SelectedMonkeys
        {
            get
            {
                return selectedMonkeys;
            }
            set
            {
                if (selectedMonkeys != value)
                {
                    selectedMonkeys = value;
                }
            }
        }

        public MonkeysViewModel()
        {
            ...
            SelectedMonkeys = new ObservableCollection<object>()
            {
                Monkeys[1], Monkeys[3], Monkeys[4]
            };
        }
        ...
    }
}
```

したがって、が表示された場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 一覧の2番目、4番目、および5番目の項目が事前に選択されています。

[![IOS と Android での複数の事前選択を含む CollectionView の一覧のスクリーンショット](selection-images/multiple-pre-selection.png "複数の事前選択を含む CollectionView 縦の一覧")](selection-images/multiple-pre-selection-large.png#lightbox "複数の事前選択を含む CollectionView 縦の一覧")

## <a name="clear-selections"></a>選択範囲のクリア

プロパティとプロパティをクリアするには、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) プロパティまたはバインド先のオブジェクトをに設定し `null` ます。

## <a name="change-selected-item-color"></a>選択した項目の色の変更

[`CollectionView`](xref:Xamarin.Forms.CollectionView) では、を使用して `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) 、内の選択した項目に対するビジュアル変更を開始でき `CollectionView` ます。 一般的なユースケースで `VisualState` は、次の XAML の例に示すように、選択した項目の背景色を変更します。

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
> を含むは、の [`Style`](xref:Xamarin.Forms.Style) `Selected` `VisualState` [`TargetType`](xref:Xamarin.Forms.Style.TargetType) ルート要素の型であるプロパティ値を持つ必要があり [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。これは、プロパティ値として設定され `ItemTemplate` ます。

この例では、 [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) `Grid` のルート要素がであるため、プロパティ値はに設定されてい [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`Grid`](xref:Xamarin.Forms.Grid) ます。 の項目が選択されて `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) いる場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 項目のは次のように設定されることを指定し `LightSkyBlue` ます。

[![IOS と Android でのカスタム単一選択の色を使用した CollectionView 縦の一覧のスクリーンショット](selection-images/single-selection-color.png "カスタムの単一選択の色を持つ CollectionView 縦の一覧")](selection-images/single-selection-color-large.png#lightbox "カスタムの単一選択の色を持つ CollectionView 縦の一覧")

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

## <a name="disable-selection"></a>選択の無効化

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 既定では、選択は無効になっています。 ただし、の選択が有効になっている場合は、 `CollectionView` プロパティをに設定することによって無効にすることができ [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `None` ます。

```xaml
<CollectionView ...
                SelectionMode="None" />
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

プロパティがに設定されている場合、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `None` の項目は選択できず、プロパティはその [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) まま残り、 `null` イベントは発生し [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) ません。

> [!NOTE]
> 項目が選択され、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) プロパティがからに変更されると `Single` `None` 、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) プロパティがに設定され、空のプロパティを使用して `null` [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) イベントが発生し `CurrentSelection` ます。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)