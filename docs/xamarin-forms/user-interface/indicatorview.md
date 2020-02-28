---
title: IndicatorView
description: IndicatorView は、CarouselView 内の項目数と現在位置を表すインジケーターを表示するコントロールです。
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2020
ms.openlocfilehash: e76cf6e766a95994fa2862deb9eb73928f4769a2
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674532"
---
# <a name="xamarinforms-indicatorview"></a>IndicatorView

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView` は、`CarouselView`内の項目数と現在位置を表すインジケーターを表示するコントロールです。

[![IOS と Android での CarouselView と IndicatorView のスクリーンショット](indicatorview-images/circles.png "IndicatorView の円")](indicatorview-images/circles-large.png#lightbox "IndicatorView の円")

`IndicatorView` は、iOS および Android プラットフォームの Xamarin. Forms 4.4、ユニバーサル Windows プラットフォームの4.5 で使用できます。 ただし、現在は実験的で、`Forms.Init`を呼び出す前に、次のコード行を iOS の `AppDelegate` クラス、または Android の `MainActivity` クラスに追加することによってのみ使用できます。

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` は、次のプロパティを定義します。

- インジケーターの数 `int`型の `Count`。
- 型 `bool`の `HideSingle`は、1つだけ存在する場合にインジケーターを非表示にするかどうかを示します。 既定値は `true` です。
- インジケーターの色を `Color`型の `IndicatorColor`。
- インジケーターのサイズ `double`型の `IndicatorSize`。 既定値は6.0 です。
- `Layout<View>`型の `IndicatorLayout`は、`IndicatorView`を表示するために使用するレイアウトクラスを定義します。 このプロパティは、Xamarin. Forms によって設定されます。通常、開発者が設定する必要はありません。
- 各インジケーターの外観を定義するテンプレート `DataTemplate`型の `IndicatorTemplate`。
- 各インジケーターの形状 `IndicatorShape`型の `IndicatorsShape`。
- `IEnumerable`型の `ItemsSource`、インジケーターが表示されるコレクションです。 このプロパティは、`CarouselView.IndicatorView` プロパティが設定されている場合に自動的に設定されます。
- `int`型の `MaximumVisible`、表示されるインジケーターの最大数。 既定値は `int.MaxValue` です。
- 現在選択されているインジケーターインデックスの型 `int`の `Position`。 このプロパティは、`TwoWay` バインドを使用します。 このプロパティは、`CarouselView.IndicatorView` プロパティが設定されている場合に自動的に設定されます。
- `CarouselView`内の現在の項目を表すインジケーターの色 `Color`型の `SelectedIndicatorColor`。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

## <a name="create-an-indicatorview"></a>IndicatorView を作成する

次の例は、XAML で `IndicatorView` をインスタンス化する方法を示しています。

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

この例では、`IndicatorView` が `CarouselView`の下にレンダリングされ、`CarouselView`内の各項目のインジケーターが表示されます。 `IndicatorView` には、`CarouselView.IndicatorView` プロパティを `IndicatorView` オブジェクトに設定することによってデータが設定されます。 各インジケーターは薄い灰色の円であり、`CarouselView` の現在の項目を表すインジケーターは濃い灰色です。

> [!IMPORTANT]
> `CarouselView.IndicatorView` プロパティを設定すると、`IndicatorView.Position` プロパティのバインドが `CarouselView.Position` プロパティになり、`IndicatorView.ItemsSource` プロパティが `CarouselView.ItemsSource` プロパティにバインドされます。

## <a name="change-indicator-shape"></a>インジケーター形状の変更

`IndicatorView` クラスには、インジケーターの形状を決定する `IndicatorsShape` プロパティがあります。 このプロパティは、`IndicatorShape` 列挙型のメンバーのいずれかに設定できます。

- `Circle` は、インジケーター図形を円形にすることを指定します。 これは、`IndicatorView.IndicatorsShape` プロパティの既定値です。
- `Square` は、インジケーター図形が正方形であることを示します。

次の例は、正方形のインジケーターを使用するように構成された `IndicatorView` を示しています。

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>インジケーターサイズの変更

`IndicatorView` クラスには `double`型の `IndicatorSize` プロパティがあり、デバイスに依存しない単位でのインジケーターのサイズを決定します。 このプロパティの既定値は6.0 です。

次の例は、より大きなインジケーターを表示するように構成された `IndicatorView` を示しています。

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>表示されるインジケーターの数を制限する

`IndicatorView` クラスには `int`型の `MaximumVisible` プロパティがあり、表示されるインジケーターの最大数を決定します。

次の例は、最大6つのインジケーターを表示するように構成された `IndicatorView` を示しています。

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>インジケーターの外観を定義する

各インジケーターの外観は、`IndicatorView.IndicatorTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義できます。

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で指定された要素は、各インジケーターの外観を定義します。 この例では、各インジケーターは、`FontImage` マークアップ拡張機能を使用してフォントアイコンを表示する[`Image`](xref:Xamarin.Forms.Image)です。

次のスクリーンショットは、フォントアイコンを使用して表示されるインジケーターを示しています。

[![IOS と Android のテンプレート化された IndicatorView のスクリーンショット](indicatorview-images/templated.png "テンプレート化 IndicatorView")](indicatorview-images/templated-large.png#lightbox "テンプレート化 IndicatorView")

`FontImage` マークアップ拡張機能の詳細については、「 [FontImage markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)」を参照してください。

## <a name="related-links"></a>関連リンク

- [IndicatorView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage のマークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
