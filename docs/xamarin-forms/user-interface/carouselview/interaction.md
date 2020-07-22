---
title: Xamarin.FormsCarouselView の相互作用
description: CarouselView に現在表示されているアイテムには、CurrentItem プロパティと Position プロパティを使用してアクセスできます。
ms.prod: xamarin
ms.assetid: 854D97E5-D119-4BE2-AE7C-BD428792C992
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/11/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 57c501c0f789ce448d8381cbbccb46666cf06305
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137411"
---
# <a name="xamarinforms-carouselview-interaction"></a>Xamarin.FormsCarouselView の相互作用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)ユーザーの操作を制御する次のプロパティを定義します。

- `CurrentItem`型の、 `object` 現在表示されている項目。 このプロパティの既定のバインディングモードは `TwoWay` で、 `null` 表示するデータがない場合は値が設定されます。
- `CurrentItemChangedCommand``ICommand`現在の項目が変更されたときに実行される、型の。
- `CurrentItemChangedCommandParameter`: `object` 型、`CurrentItemChangedCommand` に渡されるパラメーター。
- `IsBounceEnabled`が `bool` `CarouselView` コンテンツ境界でバウンスするかどうかを指定する型の。 既定値は `true` です。
- `IsSwipeEnabled`の型の `bool` 。スワイプジェスチャによって表示項目が変更されるかどうかを決定します。 既定値は `true` です。
- `Position`型の、 `int` 基になるコレクション内の現在の項目のインデックス。 このプロパティの既定のバインディングモードは `TwoWay` であり、表示するデータがない場合は0の値が設定されます。
- `PositionChangedCommand``ICommand`位置が変更されたときに実行される、型の。
- `PositionChangedCommandParameter`: `object` 型、`PositionChangedCommand` に渡されるパラメーター。
- `VisibleViews`型の `ObservableCollection<View>` 。現在表示されている項目のオブジェクトを格納する読み取り専用のプロパティ。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)`CurrentItemChanged` `CurrentItem` ユーザーのスクロールによって、またはアプリケーションによってプロパティが設定されたときに、プロパティが変更されたときに発生するイベントを定義します。 `CurrentItemChangedEventArgs`イベントに付随するオブジェクトに `CurrentItemChanged` は、次の2種類のプロパティがあり `object` ます。

- `PreviousItem`–プロパティが変更された後の前の項目。
- `CurrentItem`–プロパティが変更された後の現在の項目。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)`PositionChanged` `Position` は、ユーザーのスクロールによって、またはアプリケーションがプロパティを設定するときに、プロパティが変更されたときに発生するイベントも定義します。 `PositionChangedEventArgs`イベントに付随するオブジェクトに `PositionChanged` は、次の2種類のプロパティがあり `int` ます。

- `PreviousPosition`–プロパティが変更された後の前の位置。
- `CurrentPosition`–プロパティが変更された後の現在の位置。

## <a name="respond-to-the-current-item-changing"></a>現在の項目の変更に応答する

現在表示されている項目が変更されると、 `CurrentItem` プロパティは項目の値に設定されます。 このプロパティが変更されると、 `CurrentItemChangedCommand` に渡されるの値を使用してが実行され `CurrentItemChangedCommandParameter` `ICommand` ます。 次に、 `Position` プロパティが更新され、 `CurrentItemChanged` イベントが発生します。

> [!IMPORTANT]
> プロパティが変更さ `Position` れると、プロパティが変更され `CurrentItem` ます。 これにより、が実行され、イベントが発生し `PositionChangedCommand` `PositionChanged` ます。

### <a name="event"></a>イベント

次の XAML の例は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) イベントハンドラーを使用して、変更中の現在の項目に応答するを示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChanged="OnCurrentItemChanged">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.CurrentItemChanged += OnCurrentItemChanged;
```

この例では、イベント `OnCurrentItemChanged` の発生時にイベントハンドラーが実行され `CurrentItemChanged` ます。

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

この例では、 `OnCurrentItemChanged` イベントハンドラーが前の項目と現在の項目を公開しています。

[![以前の項目と現在の項目を含む CarouselView のスクリーンショット (iOS と Android)](interaction-images/current-item-events.png "現在と以前の項目を含む CarouselView")](interaction-images/current-item-events-large.png#lightbox "現在と以前の項目を含む CarouselView")

### <a name="command"></a>コマンド

次の XAML の例は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) コマンドを使用して、変更中の現在の項目に応答するを示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChangedCommand="{Binding ItemChangedCommand}"
              CurrentItemChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=CurrentItem}">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandProperty, "ItemChangedCommand");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandParameterProperty, new Binding("CurrentItem", source: RelativeBindingSource.Self));
```

この例では、プロパティはプロパティ `CurrentItemChangedCommand` にバインドされ `ItemChangedCommand` 、プロパティ `CurrentItem` 値が引数として渡されます。 次に、 `ItemChangedCommand` 必要に応じて、現在の項目の変更に応答できます。

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

この例では、は、 `ItemChangedCommand` 前の項目と現在の項目を格納するオブジェクトを更新します。

## <a name="respond-to-the-position-changing"></a>位置の変更に応答する

現在表示されている項目が変更されると、プロパティは、 `Position` 基になるコレクションの現在の項目のインデックスに設定されます。 このプロパティが変更されると、 `PositionChangedCommand` に渡されるの値を使用してが実行され `PositionChangedCommandParameter` `ICommand` ます。 次に、 `PositionChanged` イベントが発生します。 `Position`プロパティがプログラムによって変更されている場合、は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 値に対応する項目にスクロールされ `Position` ます。

> [!NOTE]
> プロパティを `Position` 0 に設定すると、基になるコレクションの最初の項目が表示されます。

### <a name="event"></a>イベント

次の XAML の例は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) イベントハンドラーを使用してプロパティの変更に応答するを示してい `Position` ます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"              
              PositionChanged="OnPositionChanged">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.PositionChanged += OnPositionChanged;
```

この例では、イベント `OnPositionChanged` の発生時にイベントハンドラーが実行され `PositionChanged` ます。

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

この例では、 `OnCurrentItemChanged` イベントハンドラーが前の位置と現在の位置を公開しています。

[![以前の位置と現在の位置を含む CarouselView のスクリーンショット (iOS と Android)](interaction-images/current-position-events.png "現在と以前の位置を含む CarouselView")](interaction-images/current-position-events-large.png#lightbox "現在と以前の位置を含む CarouselView")

### <a name="command"></a>コマンド

次の XAML の例は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) コマンドを使用してプロパティの変更に応答するを示してい `Position` ます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PositionChangedCommand="{Binding PositionChangedCommand}"
              PositionChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=Position}">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionChangedCommandProperty, "PositionChangedCommand");
carouselView.SetBinding(CarouselView.PositionChangedCommandParameterProperty, new Binding("Position", source: RelativeBindingSource.Self));
```

この例では、プロパティはプロパティ `PositionChangedCommand` にバインドされ `PositionChangedCommand` 、プロパティ `Position` 値が引数として渡されます。 次に、は、 `PositionChangedCommand` 必要に応じて、変更位置に応答できます。

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

この例では、は、 `PositionChangedCommand` 前の位置と現在の位置を格納するオブジェクトを更新します。

## <a name="preset-the-current-item"></a>現在の項目を事前設定する

の現在の項目は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) `CurrentItem` プロパティを項目に設定することによってプログラムで設定できます。 次の XAML の例は、現在の項目を事前に選択するを示してい `CarouselView` ます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItem="{Binding CurrentItem}">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemProperty, "CurrentItem");
```

> [!NOTE]
> プロパティには、 `CurrentItem` の既定のバインディングモードがあり `TwoWay` ます。

`CarouselView.CurrentItem`プロパティデータは、 `CurrentItem` 接続されたビューモデルのプロパティにバインドされます。これは型 `Monkey` です。 既定では、バインディングを使用して、 `TwoWay` ユーザーが現在の項目を変更した場合に、プロパティの値が `CurrentItem` 現在のオブジェクトに設定されるようにし `Monkey` ます。 `CurrentItem`プロパティは、クラスで定義されてい `MonkeysViewModel` ます。

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    public Monkey CurrentItem { get; set; }

    public MonkeysViewModel()
    {
        // ...
        CurrentItem = Monkeys.Skip(3).FirstOrDefault();
        OnPropertyChanged("CurrentItem");
    }
}
```

この例では、 `CurrentItem` プロパティはコレクションの4番目の項目に設定されてい `Monkeys` ます。

[![IOS と Android の事前設定された項目を含む CarouselView のスクリーンショット](interaction-images/preset-item.png "CarouselView と事前設定される項目")](interaction-images/preset-item-large.png#lightbox "CarouselView と事前設定される項目")

## <a name="preset-the-position"></a>位置の事前設定

表示される項目は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) プロパティを基に `Position` なるコレクションの項目のインデックスに設定することによって、プログラムで設定できます。 次の XAML の例は、 `CarouselView` 表示されている項目を設定するを示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              Position="{Binding Position}">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionProperty, "Position");
```

> [!NOTE]
> プロパティには、 `Position` の既定のバインディングモードがあり `TwoWay` ます。

`CarouselView.Position`プロパティデータは、 `Position` 接続されたビューモデルのプロパティにバインドされます。これは型 `int` です。 既定では、 `TwoWay` ユーザーがをスクロールすると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) プロパティの値が表示されている `Position` 項目のインデックスに設定されるように、バインディングが使用されます。 `Position`プロパティは、クラスで定義されてい `MonkeysViewModel` ます。

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public int Position { get; set; }

    public MonkeysViewModel()
    {
        // ...
        Position = 3;
        OnPropertyChanged("Position");
    }
}
```

この例では、 `Position` プロパティはコレクションの4番目の項目に設定されてい `Monkeys` ます。

[![IOS と Android における事前設定された位置の CarouselView のスクリーンショット](interaction-images/preset-position.png "CarouselView をプリセット位置として使用する")](interaction-images/preset-position-large.png#lightbox "CarouselView をプリセット位置として使用する")

## <a name="define-visual-states"></a>視覚的状態の定義

[`CarouselView`](xref:Xamarin.Forms.CarouselView)4つの表示状態を定義します。

- `CurrentItem`現在表示されている項目の表示状態を表します。
- `PreviousItem`以前に表示された項目の表示状態を表します。
- `NextItem`次の項目の表示状態を表します。
- `DefaultItem`項目の残りの部分の表示状態を表します。

これらの表示状態は、によって表示される項目に対する視覚的な変更を開始するために使用でき [`CarouselView`](xref:Xamarin.Forms.CarouselView) ます。

次の XAML の例は、、、、およびの各表示状態を定義する方法を示してい `CurrentItem` `PreviousItem` `NextItem` `DefaultItem` ます。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="CurrentItem">
                            <VisualState.Setters>
                                <Setter Property="Scale"
                                        Value="1.1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="PreviousItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="NextItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="DefaultItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.25" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <!-- Item template content -->
                <Frame HasShadow="true">
                    ...
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

この例では、 `CurrentItem` 表示状態は、によって表示される現在の項目の [`CarouselView`](xref:Xamarin.Forms.CarouselView) プロパティが、 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 既定値の1から1.1 に変更されることを指定します。 `PreviousItem`との `NextItem` 表示状態は、現在の項目を囲む項目が0.5 の値で表示されることを指定し [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) ます。 `DefaultItem`表示状態は、によって表示される項目の残りの部分が、 `CarouselView` 0.25 の値で表示されることを指定し `Opacity` ます。

> [!NOTE]
> または、プロパティ値 [`Style`](xref:Xamarin.Forms.Style) [`TargetType`](xref:Xamarin.Forms.Style.TargetType) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) として設定されているのルート要素の型であるプロパティ値を持つで、ビジュアル状態を定義することもでき `ItemTemplate` ます。

次のスクリーンショットは、、、およびの表示状態を示してい `CurrentItem` `PreviousItem` `NextItem` ます。

[![IOS と Android での表示状態を使用した CarouselView のスクリーンショット](interaction-images/visual-states.png "CarouselView の視覚的状態")](interaction-images/visual-states-large.png#lightbox "CarouselView の視覚的状態")

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

## <a name="clear-the-current-item"></a>現在の項目をクリアします

プロパティをクリアするには、 `CurrentItem` プロパティを設定するか、バインド先のオブジェクトをに設定し `null` ます。

## <a name="disable-bounce"></a>バウンスを無効にする

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) コンテンツの境界で項目がバウンスされます。 これは、プロパティをに設定することによって無効にすることができ `IsBounceEnabled` `false` ます。

## <a name="disable-swipe-interaction"></a>スワイプ操作を無効にする

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) ユーザーはスワイプジェスチャを使用して項目間を移動できます。 このスワイプ操作は、プロパティをに設定することによって無効にすることができ `IsSwipeEnabled` `false` ます。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
