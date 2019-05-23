---
title: Xamarin.Forms の CollectionView スクロール
description: ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、CollectionView は、プログラムで項目をスクロールして表示する 2 つの ScrollTo メソッドを定義します。
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: bd328c307ef5ad243569c294a7256ae9bdb3806a
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005265"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin.Forms の CollectionView スクロール

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 2 つ定義する[ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドは、項目をビューにスクロールします。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。 両方のオーバー ロードは、スクロールが完了すると、項目の正確な位置を示すために指定できる追加の引数とスクロールをアニメーション化するかどうかがあります。

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 定義、 [ `ScrollToRequested` ](xref:Xamarin.Forms.ItemsView.ScrollToRequested)ときに発生するイベントの 1 つ、 [ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されます。 [ `ScrollToRequestedEventArgs` ](xref:Xamarin.Forms.ScrollToRequestedEventArgs)に付属しているオブジェクト、`ScrollToRequested`イベントなど、多くのプロパティには、 `IsAnimated`、 `Index`、 `Item`、および`ScrollToPosition`します。 これらのプロパティで指定された引数から設定、`ScrollTo`メソッドの呼び出し。

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 この機能は、項目がスクロールが停止したときの位置に合わせるために、スナップと呼ばれます。 詳細については、次を参照してください。[のスナップ ポイント](#snap-points)します。

## <a name="scroll-an-item-at-an-index-into-view"></a>インデックスにある項目をビューにスクロールします。

最初の[ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドのオーバー ロードが指定したインデックス位置にある項目をビューにスクロールします。 指定された、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)という名前のオブジェクト`collectionView`、次の例では、ビューにインデックス 12 にある項目をスクロールする方法を示しています。

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>項目をビューにスクロールします。

2 番目の[ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドのオーバー ロードが、指定した項目をビューにスクロールします。 指定された、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)という名前のオブジェクト`collectionView`、次の例では、指定した項目をスクロールして表示する方法を示しています。

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>コントロールのスクロール位置

スクロール バーが完了した後の項目の正確な位置を指定できます、項目をビューにスクロールするとき、`position`の引数、 [ `ScrollTo` ](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッド。 この引数を受け入れる、 [ `ScrollToPosition` ](xref:Xamarin.Forms.ScrollToPosition)列挙型のメンバー。

### <a name="makevisible"></a>MakeVisible

[ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition)メンバーは、項目は、これが、ビューに表示されるまでスクロールする必要がありますを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

このコード例は、項目をスクロールして表示するために必要な最小限のスクロールが得られます。

[![項目に垂直方向の CollectionView 一覧のスクリーン ショットは、iOS と Android でのビューにスクロール](scrolling-images/scrolltoposition-makevisible.png "CollectionView スクロール項目に垂直方向に一覧")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "CollectionView を垂直方向に一覧スクロール項目")

> [!NOTE]
> [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition)場合、既定では、メンバーが使用されて、`position`を呼び出すときに、引数が指定されていない、`ScrollTo`メソッド。

### <a name="start"></a>[開始]

[ `ScrollToPosition.Start` ](xref:Xamarin.Forms.ScrollToPosition)ビューの先頭に、項目をスクロールする必要がありますをメンバーを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

このコード例は、ビューの先頭にスクロールされる項目が得られます。

[![項目に垂直方向の CollectionView 一覧のスクリーン ショットは、iOS と Android でのビューにスクロール](scrolling-images/scrolltoposition-start.png "CollectionView スクロール項目に垂直方向に一覧")](scrolling-images/scrolltoposition-start-large.png#lightbox "CollectionView を垂直方向に一覧スクロール項目")

### <a name="center"></a>中央揃え

[ `ScrollToPosition.Center` ](xref:Xamarin.Forms.ScrollToPosition)ビューの中心に、項目をスクロールする必要がありますをメンバーを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

このコード例は、ビューの中央にスクロールされる項目が得られます。

[![項目に垂直方向の CollectionView 一覧のスクリーン ショットは、iOS と Android でのビューにスクロール](scrolling-images/scrolltoposition-center.png "CollectionView スクロール項目に垂直方向に一覧")](scrolling-images/scrolltoposition-center-large.png#lightbox "CollectionView を垂直方向に一覧スクロール項目")

### <a name="end"></a>終了

[ `ScrollToPosition.End` ](xref:Xamarin.Forms.ScrollToPosition)ビューの末尾に項目をスクロールする必要がありますをメンバーを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

このコード例は、ビューの末尾にスクロールされる項目が得られます。

[![項目に垂直方向の CollectionView 一覧のスクリーン ショットは、iOS と Android でのビューにスクロール](scrolling-images/scrolltoposition-end.png "CollectionView スクロール項目に垂直方向に一覧")](scrolling-images/scrolltoposition-end-large.png#lightbox "CollectionView を垂直方向に一覧スクロール項目")

## <a name="disable-scroll-animation"></a>スクロール アニメーションを無効にします。

項目をビューにスクロールする場合は、スクロール アニメーションが表示されます。 ただし、このアニメーションを設定して無効にできます、`animate`の引数、`ScrollTo`メソッドを`false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>スナップ ポイント

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 項目が停止し、次のプロパティによって制御されます、スクロールするときに配置するスナップために、この機能が、スナップと呼ばれる、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout)クラス。

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)、型の[ `SnapPointsType` ](xref:Xamarin.Forms.SnapPointsType)、スクロールするとき、スナップ ポイントの動作を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)、型の[ `SnapPointsAlignment` ](xref:Xamarin.Forms.SnapPointsAlignment)、項目を含むのスナップ ポイントを配置する方法を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

> [!NOTE]
> スナップが発生したときに、方向で実行されます、運動量が少なくを生成します。

### <a name="snap-points-type"></a>スナップ ポイントの種類

[ `SnapPointsType` ](xref:Xamarin.Forms.SnapPointsType)列挙体は、次のメンバーを定義します。

- `None` スクロールにスナップしません項目を示します。
- `Mandatory` コンテンツ、スクロールは自然を停止する、慣性による処理の方向に最も近いスナップイン スナップが常にポイントします。
- `MandatorySingle` 同じ動作を示す`Mandatory`が、一度に 1 つの項目のみスクロールします。

既定では、 [ `SnapPointsType` ](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)プロパティに設定されて`SnapPointsType.None`、これにより、スクロールするにはスナップしません項目では、次のスクリーン ショットに示すようにします。

[![IOS と Android でのスナップ ポイントなし CollectionView 垂直方向の一覧のスクリーン ショット](scrolling-images/snappoints-none.png "CollectionView 垂直方向のリストのスナップ ポイントなし")](scrolling-images/snappoints-none-large.png#lightbox "スナップせず CollectionView 垂直方向の一覧ポイント")

### <a name="snap-points-alignment"></a>スナップ ポイントの配置

[ `SnapPointsAlignment` ](xref:Xamarin.Forms.SnapPointsAlignment)列挙体を定義`Start`、 `Center`、および`End`メンバー。

> [!IMPORTANT]
> 値、 [ `SnapPointsAlignment` ](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティは、のみ適用されるときに、 [ `SnapPointsType` ](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)プロパティに設定されて`Mandatory`、または`MandatorySingle`します。

#### <a name="start"></a>[開始]

`SnapPointsAlignment.Start`メンバーは、スナップ ポイントが項目のリーディング エッジに準拠していることを示します。

既定で、 [ `SnapPointsAlignment` ](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティに設定されて`SnapPointsAlignment.Start`します。 ただし、完全を期すため、次の XAML の例はこの列挙体メンバーを設定する方法を示します。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Start">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

ユーザー カードのスクロールを開始するには、ときに、最上位の項目は、ビューの上部に揃えています。

[![IOS と Android での開始のスナップ ポイントと CollectionView 垂直一覧のスクリーン ショット](scrolling-images/snappoints-start.png "CollectionView 開始のスナップ ポイントと垂直方向に一覧")](scrolling-images/snappoints-start-large.png#lightbox "CollectionView が開始すると垂直方向に一覧スナップ ポイント")

#### <a name="center"></a>中央揃え

`SnapPointsAlignment.Center`メンバーは、項目の中央揃えのスナップ ポイントが揃っていることを示します。 次の XAML の例では、この列挙体メンバーを設定する方法を示します。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Center">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

ときにユーザー カードのスクロールを開始するには、最上位の項目を中央揃え、ビューの上部にあるとなります。

[![Center スナップ ポイントでは、iOS と Android で CollectionView 垂直方向の一覧のスクリーン ショット](scrolling-images/snappoints-center.png "CollectionView スナップインの中心点の垂直方向に一覧")](scrolling-images/snappoints-center-large.png#lightbox "CollectionView を垂直方向に一覧center のスナップ ポイント")

#### <a name="end"></a>終了

`SnapPointsAlignment.End`メンバーでは、項目のトレーリング エッジのスナップ ポイントを整列することを示します。 次の XAML の例では、この列挙体メンバーを設定する方法を示します。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="End">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

ユーザー カードのスクロールを開始するには、ときに、最下位の項目は、ビューの下部に揃えられます。

[![IOS と Android での最後のスナップ ポイントと CollectionView 垂直一覧のスクリーン ショット](scrolling-images/snappoints-end.png "CollectionView 最後のスナップ ポイントと垂直方向に一覧")](scrolling-images/snappoints-end-large.png#lightbox "CollectionView エンド スナップインと垂直方向に一覧ポイント")

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
