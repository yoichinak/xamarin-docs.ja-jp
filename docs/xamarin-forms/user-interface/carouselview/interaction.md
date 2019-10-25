---
title: CarouselView の相互作用
description: CarouselView に現在表示されているアイテムには、CurrentItem プロパティと Position プロパティを使用してアクセスできます。
ms.prod: xamarin
ms.assetid: 854D97E5-D119-4BE2-AE7C-BD428792C992
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: fd85876b38c21c7ff1ea85d7a61c1449395d56f5
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749789"
---
# <a name="xamarinforms-carouselview-interaction"></a>CarouselView の相互作用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、ユーザーの操作を制御する次のプロパティを定義します。

- 現在表示されている項目 `object` 型の `CurrentItem`。 このプロパティには `TwoWay` の既定のバインディングモードがあり、表示するデータがない場合は `null` 値が設定されます。
- `ICommand` 型の `CurrentItemChangedCommand`。現在の項目が変更されたときに実行されます。
- `CurrentItemChangedCommandParameter`: `object` 型、`CurrentItemChangedCommand`に渡されるパラメーターです。
- `CarouselView` がコンテンツの境界でバウンスするかどうかを指定する `bool` 型の `IsBounceEnabled`。 既定値は `true`です。
- `bool` 型の `IsSwipeEnabled`。スワイプジェスチャによって表示される項目を変更するかどうかを決定します。 既定値は `true`です。
- 基になるコレクションの現在の項目のインデックス `int` 型の `Position`。 このプロパティには `TwoWay` の既定のバインディングモードがあり、表示するデータがない場合は0の値が設定されます。
- `ICommand` 型の `PositionChangedCommand`。位置が変更されたときに実行されます。
- `PositionChangedCommandParameter`: `object` 型、`PositionChangedCommand`に渡されるパラメーターです。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、ユーザーのスクロールによって、またはアプリケーションによってプロパティが設定されたときに、`CurrentItem` プロパティが変更されたときに発生する `CurrentItemChanged` イベントを定義します。 `CurrentItemChanged` イベントに付随する `CurrentItemChangedEventArgs` オブジェクトには、両方とも `object` 型の2つのプロパティがあります。

- `PreviousItem` –プロパティが変更された後の前の項目。
- `CurrentItem` –プロパティが変更された後の現在の項目。

また[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、ユーザーのスクロールによって、またはアプリケーションがプロパティを設定するときに、`Position` プロパティが変更されたときに発生する `PositionChanged` イベントも定義します。 `PositionChanged` イベントに付随する `PositionChangedEventArgs` オブジェクトには、両方とも `int` 型の2つのプロパティがあります。

- `PreviousPosition` –プロパティが変更された後の直前の位置。
- `CurrentPosition` –プロパティが変更された後の現在の位置。

## <a name="respond-to-the-current-item-changing"></a>現在の項目の変更に応答する

現在表示されている項目が変更されると、`CurrentItem` プロパティが項目の値に設定されます。 このプロパティが変更されると、`ICommand` に渡される `CurrentItemChangedCommandParameter` の値を使用して `CurrentItemChangedCommand` が実行されます。 次に、`Position` のプロパティが更新され、`CurrentItemChanged` イベントが発生します。

> [!IMPORTANT]
> `Position` プロパティは、`CurrentItem` プロパティが変更されたときに変更されます。 これにより、`PositionChangedCommand` が実行され、`PositionChanged` イベントが発生します。

### <a name="event"></a>event

次の XAML の例は、イベントハンドラーを使用して、変更中の現在の項目に応答する[`CarouselView`](xref:Xamarin.Forms.CarouselView)を示しています。

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

この例では、`CurrentItemChanged` イベントの発生時に `OnCurrentItemChanged` イベントハンドラーが実行され、前の項目と現在の項目がイベントハンドラーによって公開されます。

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

### <a name="command"></a>コマンド

次の XAML の例は、現在の項目の変更に応答するためにコマンドを使用する[`CarouselView`](xref:Xamarin.Forms.CarouselView)を示しています。

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

この例では、`CurrentItemChangedCommand` プロパティを `ItemChangedCommand` プロパティにバインドし、`CurrentItem` プロパティ値を引数として渡します。 `ItemChangedCommand` は、必要に応じて、現在の項目の変更に応答できます。

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

この例では、`ItemChangedCommand` は、前の項目と現在の項目を格納するオブジェクトを更新します。

## <a name="respond-to-the-position-changing"></a>位置の変更に応答する

現在表示されている項目が変更されると、`Position` プロパティは、基になるコレクションの現在の項目のインデックスに設定されます。 このプロパティが変更されると、`ICommand` に渡される `PositionChangedCommandParameter` の値を使用して `PositionChangedCommand` が実行されます。 その後、`PositionChanged` イベントが発生します。 `Position` プロパティがプログラムによって変更されている場合、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)は `Position` 値に対応する項目にスクロールされます。

> [!NOTE]
> `Position` プロパティを0に設定すると、基になるコレクションの最初の項目が表示されます。

### <a name="event"></a>event

次の XAML の例は、イベントハンドラーを使用して `Position` プロパティの変更に応答する[`CarouselView`](xref:Xamarin.Forms.CarouselView)を示しています。

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

この例では、`PositionChanged` イベントの発生時に `OnPositionChanged` イベントハンドラーが実行され、前の位置と現在の位置を公開するイベントハンドラーがあります。

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

### <a name="command"></a>コマンド

次の XAML の例は、コマンドを使用して `Position` プロパティの変更に応答する[`CarouselView`](xref:Xamarin.Forms.CarouselView)を示しています。

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

この例では、`PositionChangedCommand` プロパティを `PositionChangedCommand` プロパティにバインドし、`Position` プロパティ値を引数として渡します。 `PositionChangedCommand` は、必要に応じて、変更された位置に応答できます。

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

この例では、`PositionChangedCommand` は、前の位置と現在の位置を格納するオブジェクトを更新します。

## <a name="preset-the-current-item"></a>現在の項目を事前設定する

[`CarouselView`](xref:Xamarin.Forms.CarouselView)内の現在の項目は、`CurrentItem` プロパティを項目に設定することによって、プログラムで設定できます。 次の XAML の例は、現在の項目を事前に選択している `CarouselView` を示しています。

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
> `CurrentItem` プロパティには、`TwoWay` の既定のバインディングモードがあります。

`CarouselView.CurrentItem` のプロパティデータは、接続されたビューモデルの `CurrentItem` プロパティにバインドされます。これは `Monkey` 型です。 既定では、ユーザーが現在の項目を変更した場合に、`CurrentItem` プロパティの値が現在の `Monkey` オブジェクトに設定されるように `TwoWay` バインドが使用されます。 `CurrentItem` プロパティは `MonkeysViewModel` クラスで定義され、`Monkeys` コレクションの4番目の項目に設定されます。

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

## <a name="preset-the-position"></a>位置の事前設定

[`CarouselView`](xref:Xamarin.Forms.CarouselView)表示される項目は、`Position` プロパティを基になるコレクション内の項目のインデックスに設定することによって、プログラムで設定できます。 次の XAML の例は、表示される項目を設定する `CarouselView` を示しています。

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
> `Position` プロパティには、`TwoWay` の既定のバインディングモードがあります。

`CarouselView.Position` のプロパティデータは、接続されたビューモデルの `Position` プロパティにバインドされます。これは `int` 型です。 既定では、ユーザーが[`CarouselView`](xref:Xamarin.Forms.CarouselView)をスクロールしたときに、`Position` プロパティの値が表示されている項目のインデックスに設定されるように、`TwoWay` バインドが使用されます。 `Position` プロパティは `MonkeysViewModel` クラスで定義され、`Monkeys` コレクションの4番目の項目に設定されます。

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

## <a name="clear-the-current-item"></a>現在の項目をクリアします

`CurrentItem` プロパティは、それを設定するか、バインド先のオブジェクトを `null` に設定することによってクリアできます。

## <a name="disable-bounce"></a>バウンスを無効にする

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)コンテンツの境界で項目をバウンスします。 これは、`IsBounceEnabled` プロパティを `false` に設定することによって無効にすることができます。

## <a name="disable-swipe-interaction"></a>スワイプ操作を無効にする

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)を使用すると、ユーザーはスワイプジェスチャを使用して項目間を移動できます。 このスワイプ操作は、`IsSwipeEnabled` プロパティを `false` に設定することによって無効にすることができます。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
