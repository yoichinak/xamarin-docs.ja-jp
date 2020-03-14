---
title: Xamarin CollectionView の選択
description: 既定では、CollectionView の選択は無効になっています。 単一または複数選択を有効にすることができます。
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a4cc237ef738edeccf66f1a91a010e4831c1c72f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305913"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin CollectionView の選択

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、項目の選択を制御する次のプロパティを定義します。

- 選択モード[`SelectionMode`](xref:Xamarin.Forms.SelectionMode)型の[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)。
- リスト内で選択されている項目 `object`型の[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)。 このプロパティには `TwoWay`の既定のバインディングモードがあり、項目が選択されていない場合は `null` 値が設定されます。
- リスト内で選択されている項目 `IList<object>`型の[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)。 このプロパティには `OneWay`の既定のバインディングモードがあり、項目が選択されていない場合は `null` 値が設定されます。
- `ICommand`型の[`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)。選択した項目が変更されたときに実行されます。
- `object`型の[`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)。 `SelectionChangedCommand`に渡されるパラメーターです。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトによって支持されています。つまり、プロパティがデータ バインディングの対象になる場合があります。

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView)選択は無効になっています。 ただし、この動作は、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティの値を[`SelectionMode`](xref:Xamarin.Forms.SelectionMode)列挙型のメンバーのいずれかに設定することによって変更できます。

- `None` –項目を選択できないことを示します。 これが既定値です。
- `Single` –選択した項目を強調表示して、1つの項目を選択できることを示します。
- `Multiple` –選択した項目を強調表示して、複数の項目を選択できることを示します。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、ユーザーが一覧から項目を選択したか、アプリケーションがプロパティを設定したことが原因で、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティが変更されたときに発生する[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントを定義します。 また、このイベントは、 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティが変更されたときにも発生します。 `SelectionChanged` イベントに付随する[`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs)オブジェクトには、両方とも `IReadOnlyList<object>`型の2つのプロパティがあります。

- `PreviousSelection` –選択項目が変更される前に選択された項目の一覧。
- `CurrentSelection` –選択内容の変更後に選択された項目の一覧。

さらに、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、1つの変更通知のみを実行しながら、選択した項目の一覧を使用して[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティを更新する `UpdateSelectedItems` メソッドがあります。

## <a name="single-selection"></a>単一選択

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティが `Single`に設定されている場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の1つの項目を選択できます。 項目を選択すると、[ [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) ] プロパティが選択した項目の値に設定されます。 このプロパティが変更されると、(`ICommand`に渡される[`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)の値を使用して) [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)が実行され、 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントが発生します。

次の XAML の例は、単一の項目選択に応答できる[`CollectionView`](xref:Xamarin.Forms.CollectionView)を示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

このコード例では、 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントが発生したときに `OnCollectionViewSelectionChanged` イベントハンドラーが実行され、前に選択した項目を取得するイベントハンドラーと、現在選択されている項目を取得します。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントは、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティを変更した結果として発生する変更によって発生する可能性があります。

次のスクリーンショットは、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)での1つの項目の選択を示しています。

[![IOS と Android での単一選択による CollectionView 縦の一覧のスクリーンショット](selection-images/single-selection.png "1つの選択項目を含む CollectionView 縦の一覧")](selection-images/single-selection-large.png#lightbox "1つの選択項目を含む CollectionView 縦の一覧")

## <a name="multiple-selection"></a>複数選択

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティが `Multiple`に設定されている場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の複数の項目を選択できます。 項目を選択すると、[ [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) ] プロパティが選択した項目に設定されます。 このプロパティが変更されると、(`ICommand`に渡される[`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)の値を使用して) [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)が実行され、 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントが発生します。

次の XAML の例は、複数の項目の選択に応答できる[`CollectionView`](xref:Xamarin.Forms.CollectionView)を示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

このコード例では、 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントが発生したときに `OnCollectionViewSelectionChanged` イベントハンドラーが実行され、以前に選択された項目を取得するイベントハンドラーと、現在選択されている項目を取得します。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントは、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティを変更した結果として発生する変更によって発生する可能性があります。

次のスクリーンショットは、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)での複数の項目の選択を示しています。

[![IOS と Android での複数選択がある CollectionView 縦の一覧のスクリーンショット](selection-images/multiple-selection.png "複数選択を含む CollectionView の一覧")](selection-images/multiple-selection-large.png#lightbox "複数選択を含む CollectionView の一覧")

## <a name="single-pre-selection"></a>1つの事前選択

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティが `Single`に設定されている場合、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティを項目に設定することによって、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の1つの項目を事前に選択できます。 次の XAML の例は、1つの項目を事前に選択する `CollectionView` を示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティには、`TwoWay`の既定のバインディングモードがあります。

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)のプロパティデータは、接続されたビューモデルの `SelectedMonkey` プロパティにバインドされます。これは `Monkey`型です。 既定では、ユーザーが選択した項目を変更した場合に、`SelectedMonkey` プロパティの値が選択した `Monkey` オブジェクトに設定されるように、`TwoWay` バインドが使用されます。 `SelectedMonkey` プロパティは `MonkeysViewModel` クラスで定義され、`Monkeys` コレクションの4番目の項目に設定されます。

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

そのため、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)が表示されたら、一覧の4番目の項目が事前に選択されています。

[![IOS と Android での1つの事前選択を含む CollectionView の一覧のスクリーンショット](selection-images/single-pre-selection.png "1つの事前選択を含む CollectionView 縦の一覧")](selection-images/single-pre-selection-large.png#lightbox "1つの事前選択を含む CollectionView 縦の一覧")

## <a name="multiple-pre-selection"></a>複数の事前選択

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティが `Multiple`に設定されている場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の複数の項目を事前に選択できます。 次の XAML の例は、複数の項目の事前選択を有効にする `CollectionView` を示しています。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティには、`OneWay`の既定のバインディングモードがあります。

[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)のプロパティデータは、接続されたビューモデルの `SelectedMonkeys` プロパティにバインドされます。これは `ObservableCollection<object>`型です。 `SelectedMonkeys` プロパティは `MonkeysViewModel` クラスで定義され、`Monkeys` コレクションの2番目、4番目、および5番目の項目に設定されます。

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

このため、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)が表示された場合、一覧の2番目、4番目、および5番目の項目が事前に選択されています。

[![IOS と Android での複数の事前選択を含む CollectionView の一覧のスクリーンショット](selection-images/multiple-pre-selection.png "複数の事前選択を含む CollectionView 縦の一覧")](selection-images/multiple-pre-selection-large.png#lightbox "複数の事前選択を含む CollectionView 縦の一覧")

## <a name="clear-selections"></a>選択範囲のクリア

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティと[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティは、これらのプロパティまたはバインド先のオブジェクトを `null`に設定することによってクリアできます。

## <a name="change-selected-item-color"></a>選択した項目の色の変更

[`CollectionView`](xref:Xamarin.Forms.CollectionView)には、`CollectionView`内の選択した項目に対する視覚的な変更を開始するために使用できる `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState)があります。 この `VisualState` の一般的なユースケースは、選択した項目の背景色を変更することです。これを次の XAML の例に示します。

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
> `Selected` `VisualState` を含む[`Style`](xref:Xamarin.Forms.Style)には、`DataTemplate`プロパティ値として設定されている[`ItemTemplate`](xref:Xamarin.Forms.DataTemplate)のルート要素の型である[`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティ値が必要です。

この例では、 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)のルート要素が[`Grid`](xref:Xamarin.Forms.Grid)であるため、 [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティ値は `Grid` に設定されています。 `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState)では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の項目が選択されると、その項目の[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)が `LightSkyBlue`に設定されることを指定します。

[![IOS と Android でのカスタム単一選択の色を使用した CollectionView 縦の一覧のスクリーンショット](selection-images/single-selection-color.png "カスタムの単一選択の色を持つ CollectionView 縦の一覧")](selection-images/single-selection-color-large.png#lightbox "カスタムの単一選択の色を持つ CollectionView 縦の一覧")

表示状態の詳細については、「 [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="disable-selection"></a>選択の無効化

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView)選択は無効になっています。 ただし、`CollectionView` で選択が有効になっている場合は、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティを `None`に設定することによって無効にすることができます。

```xaml
<CollectionView ...
                SelectionMode="None" />
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティが `None`に設定されている場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の項目は選択できません。 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティは `null`のままで、 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントは発生しません。

> [!NOTE]
> 項目が選択され、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティが `Single` から `None`に変更されると、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティが `null` に設定され、空の`SelectionChanged`プロパティを使用して[`CurrentSelection`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)イベントが発生します。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin Forms State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
