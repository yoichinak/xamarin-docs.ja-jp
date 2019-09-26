---
title: Xamarin CollectionView の選択
description: 既定では、CollectionView の選択は無効になっています。 単一または複数選択を有効にすることができます。
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f1a3e8bb8959588e64339f70268370440f356be9
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "68738973"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin CollectionView の選択

![](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)項目の選択を制御する次のプロパティを定義します。

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)選択モードで[`SelectionMode`](xref:Xamarin.Forms.SelectionMode)ある、型の。
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)型`object`の、リスト内の選択された項目。 このプロパティの既定の`TwoWay`バインディングモードはで、項目が選択されていない場合は`null`値が設定されます。
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)型`IList<object>`の、リスト内の選択された項目。 このプロパティの既定の`OneWay`バインディングモードはで、項目が選択されていない場合は`null`値が設定されます。
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)型`ICommand`の。選択した項目が変更されたときに実行されます。
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)型`object`の。これは、 `SelectionChangedCommand`に渡されるパラメーターです。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、選択は無効になっています。 ただし、この動作は、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティ値を[`SelectionMode`](xref:Xamarin.Forms.SelectionMode)列挙体のメンバーの1つに設定することによって変更できます。

- `None`–項目を選択できないことを示します。 これが既定値です。
- `Single`–1つの項目を選択し、選択した項目を強調表示することを示します。
- `Multiple`–選択された項目が強調表示されている複数の項目を選択できることを示します。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ユーザーが[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)一覧から項目を選択し[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)たか、アプリケーションがプロパティを設定したときに、プロパティが変更されたときに発生するイベントを定義します。 また、このイベントは、 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティが変更されたときにも発生します。 `IReadOnlyList<object>`イベント[`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) に`SelectionChanged`付随するオブジェクトには、次の2種類のプロパティがあります。

- `PreviousSelection`–選択項目が変更される前に選択された項目の一覧。
- `CurrentSelection`–選択の変更後に選択された項目の一覧。

## <a name="single-selection"></a>単一選択

プロパティが[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)に`Single`設定されている場合は、内[`CollectionView`](xref:Xamarin.Forms.CollectionView)の1つの項目を選択できます。 項目を選択[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)すると、プロパティは選択した項目の値に設定されます。 このプロパティ[`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)が変更[`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)されると、が実行され (に`ICommand`渡されるの値によって) [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 、イベントが発生します。

次の XAML の例は[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、単一の項目の選択に応答できるを示しています。

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

このコード例`OnCollectionViewSelectionChanged`では、イベントが発生し[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)たときにイベントハンドラーが実行され、前に選択した項目を取得するイベントハンドラーと、現在選択されている項目を取得します。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> イベント[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)は、プロパティを[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)変更した結果として発生する変更によって発生する可能性があります。

次のスクリーンショットは、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)での単一項目の選択を示しています。

[![IOS と Android での単一選択による CollectionView 縦の一覧のスクリーンショット](selection-images/single-selection.png "1 つの選択項目を含む CollectionView 縦の一覧")](selection-images/single-selection-large.png#lightbox "1つの選択項目を含む CollectionView 縦の一覧")

## <a name="multiple-selection"></a>複数選択

プロパティが[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)に`Multiple`設定されている場合は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の複数の項目を選択できます。 項目を選択[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)すると、プロパティは選択した項目に設定されます。 このプロパティ[`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)が変更[`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)されると、が実行され (に`ICommand`渡されるの値によって) [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 、イベントが発生します。

次の XAML の例は[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、複数の項目の選択に応答できるを示しています。

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

このコード例`OnCollectionViewSelectionChanged`では、イベントの[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)発生時にイベントハンドラーが実行され、以前に選択された項目を取得するイベントハンドラーと、現在選択されている項目を取得します。

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> イベント[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)は、プロパティを[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)変更した結果として発生する変更によって発生する可能性があります。

次のスクリーンショットは、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)での複数の項目の選択を示しています。

[![IOS と Android での複数選択がある CollectionView 縦の一覧のスクリーンショット](selection-images/multiple-selection.png "複数選択を含む CollectionView の一覧")](selection-images/multiple-selection-large.png#lightbox "複数選択を含む CollectionView の一覧")

## <a name="single-pre-selection"></a>1つの事前選択

プロパティがに`Single`設定されている場合、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティを[`CollectionView`](xref:Xamarin.Forms.CollectionView)項目に設定することによって、内の1つの項目を事前選択できます。 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 次の XAML の例は`CollectionView` 、1つの項目を事前に選択するを示しています。

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
> プロパティには、の既定の`TwoWay`バインディングモードがあります。 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)

[ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティ データにバインド、`SelectedMonkey`プロパティの種類は、接続されているビュー モデルの`Monkey`します。 既定`TwoWay`では、ユーザーが選択した項目を変更した場合に、選択`Monkey`したオブジェクト`SelectedMonkey`にプロパティの値が設定されるように、バインドが使用されます。 プロパティは`MonkeysViewModel`クラスで定義され、 `Monkeys`コレクションの4番目の項目に設定されます。 `SelectedMonkey`

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

そのため、が[`CollectionView`](xref:Xamarin.Forms.CollectionView)表示されたら、一覧の4番目の項目が事前に選択されています。

[![IOS と Android での1つの事前選択を含む CollectionView の一覧のスクリーンショット](selection-images/single-pre-selection.png "1 つの事前選択を含む CollectionView 縦の一覧")](selection-images/single-pre-selection-large.png#lightbox "1つの事前選択を含む CollectionView 縦の一覧")

## <a name="multiple-pre-selection"></a>複数の事前選択

プロパティが[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)に`Multiple`設定されている場合は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の複数の項目を事前に選択できます。 次の XAML の例は`CollectionView` 、複数の項目の事前選択を有効にするを示しています。

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
> プロパティには、の既定の`OneWay`バインディングモードがあります。 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)

[ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティ データにバインド、`SelectedMonkeys`プロパティの種類は、接続されているビュー モデルの`ObservableCollection<object>`します。 プロパティは`MonkeysViewModel`クラスで定義され、 `Monkeys`コレクション内の2番目、4番目、および5番目の項目に設定されます。 `SelectedMonkeys`

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

したがって、が[`CollectionView`](xref:Xamarin.Forms.CollectionView)表示された場合、一覧の2番目、4番目、および5番目の項目が事前に選択されています。

[![IOS と Android での複数の事前選択を含む CollectionView の一覧のスクリーンショット](selection-images/multiple-pre-selection.png "複数の事前選択を含む CollectionView 縦の一覧")](selection-images/multiple-pre-selection-large.png#lightbox "複数の事前選択を含む CollectionView 縦の一覧")

## <a name="clearing-selections"></a>選択のクリア

プロパティ[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) `null`と[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)プロパティをクリアするには、プロパティまたはバインド先のオブジェクトをに設定します。

## <a name="change-selected-item-color"></a>選択した項目の色の変更

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、を使用して、 `CollectionView`内の選択した項目に対するビジュアル変更を開始できます。 `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) 一般的なユースケース`VisualState`では、次の XAML の例に示すように、選択した項目の背景色を変更します。

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
> を[`Style`](xref:Xamarin.Forms.Style) [`TargetType`](xref:Xamarin.Forms.Style.TargetType) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)含むは、`ItemTemplate`のルート要素の型であるプロパティ値を持つ必要があります。これは、プロパティ値として設定されます。`VisualState` `Selected`

この例では、 [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType)の[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)ルート[`Grid`](xref:Xamarin.Forms.Grid)要素がで`Grid`あるため、プロパティ値はに設定されています。 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) `LightSkyBlue`の項目が選択されている場合、項目のは次のように設定されることを[指定します。`VisualState`](xref:Xamarin.Forms.VisualState) `Selected`

[![IOS と Android でのカスタム単一選択の色を使用した CollectionView 縦の一覧のスクリーンショット](selection-images/single-selection-color.png "カスタムの単一選択の色を持つ CollectionView 縦の一覧")](selection-images/single-selection-color-large.png#lightbox "カスタムの単一選択の色を持つ CollectionView 縦の一覧")

表示状態の詳細については、「 [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="disable-selection"></a>選択の無効化

[`CollectionView`](xref:Xamarin.Forms.CollectionView)既定では、選択は無効になっています。 ただし、の`CollectionView`選択が有効になっている場合は、 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)プロパティをに設定`None`することによって無効にすることができます。

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

`null` [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)プロパティがに`None`設定されている場合、の項目は選択できず、プロパティはそのまま残り、イベントは発生しません。 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)

> [!NOTE]
> 項目が[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)選択され、プロパティがから`Single`に`None`変更されると、 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)プロパティがに`null` [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)設定され、空`CurrentSelection`のプロパティを使用してイベントが発生します.

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin Forms State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
