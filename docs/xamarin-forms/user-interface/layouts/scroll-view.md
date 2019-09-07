---
title: Xamarin.Forms ScrollView
description: この記事では、Xamarin.Forms ScrollView クラスを使用して、1 つだけの画面上に一致することはできませんし、キーボードのためのコンテンツがあるが、レイアウトを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 131b9dc64db1ea0dba3982bd71459c2a281743bc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770286"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) レイアウトを含み、オフスクリーン スクロールを有効になります。 `ScrollView` キーボードを表示するときに、画面の表示部分に自動的に移動するビューを許可するも使用されます。

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms のレイアウト")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms のレイアウト")

この記事では次の内容について説明します。

- **[目的](#purpose)** &ndash;の目的は、`ScrollView`使われているとします。
- **[使用状況](#usage)** &ndash;を使用する`ScrollView`実際にします。
- **[プロパティ](#properties)** &ndash;パブリック プロパティを読み取り、変更することができます。
- **[メソッド](#methods)** &ndash;パブリック メソッド ビューをスクロールするために呼び出すことができます。
- **[イベント](#events)** &ndash;ビューの状態の変化をリッスンするために使用できるイベント。

## <a name="purpose"></a>目的

`ScrollView` 小さい携帯電話で適切に拡大ビューが表示されるようにするために使用します。 たとえば、iPhone 6 s で動作するレイアウトを iPhone 4 秒で切れる可能性があります。 使用して、`ScrollView`できるよう、小さい画面に表示されるレイアウトのクリップの一部になります。

## <a name="usage"></a>使用法

> [!NOTE]
> `ScrollView`s は入れ子にする必要がありますできません。 さらに、 `ScrollView`s が他のコントロールなどスクロールなどを提供すると入れ子にする必要がありますできません`ListView`と`WebView`します。

`ScrollView` 公開、`Content`プロパティの 1 つのビューやレイアウトを設定することができます。 この例の後に、非常に大きな boxView のレイアウトを検討してください、 `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

C# の場合:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

ユーザーがスクロール ダウンしているだけ前に、`BoxView`が表示されます。

![](scroll-view-images/scroll-start.png "ScrollView で BoxView")

ユーザーが画面にテキストを入力すると、`Entry`画面に表示されるようにする、ビューをスクロールします。

![](scroll-view-images/scroll-end.png "ScrollView 内のエントリ")

## <a name="properties"></a>プロパティ

`ScrollView` 次のプロパティを定義します。

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) 取得、 [ `Size` ](xref:Xamarin.Forms.Size)コンテンツのサイズを表す値です。
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) 取得または設定します、 [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation)のスクロール方向を表す列挙値、`ScrollView`します。
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) 取得、`double`現在スクロール位置を表します。
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) 取得、 `double` Y の現在のスクロール位置を表します。
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) 取得または設定します、 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility)水平スクロール バーが表示されるときを表す値です。
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) 取得または設定します、 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility)垂直スクロール バーが表示されるときを表す値です。

## <a name="methods"></a>メソッド

`ScrollView` 提供、`ScrollToAsync`メソッドは、ビューをスクロールするために使用する座標を使用するか、特定のビューを表示するようにする必要があります値を指定します。

座標を使用する場合は、指定、`x`と`y`と共にスクロールをアニメーション化するかどうかを示すブール値の座標。

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

特定の要素にスクロールする場合、`ScrollToPosition`ビューで、要素が表示される場所の列挙を指定します。

- **Center** &ndash;ビューの表示部分の中央に要素をスクロールします。
- **終了**&ndash;ビューの表示部分の末尾に要素をスクロールします。
- **MakeVisible** &ndash;ビュー内で表示されるように要素をスクロールします。
- **開始**&ndash;ビューの表示部分の先頭に要素をスクロールします。

`IsAnimated`プロパティは、ビューのスクロール方法を指定します。 ときに滑らかなアニメーションを true に設定される、瞬時にコンテンツをビューに移動するのではなく。

## <a name="events"></a>イベント

`ScrollView` 1 つのイベントを定義します。`Scrolled`します。 `Scrolled` スクロール、表示が完了したときに発生します。 イベント ハンドラー`Scrolled`は`ScrolledEventArgs`を持つ、`ScrollX`と`ScrollY`プロパティ。 次の現在のスクロール位置にラベルを更新する方法を示します、 `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

負の場合、一覧の最後にスクロールする場合、バウンド効果のためのスクロール位置である可能性がありますに注意してください。

## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
