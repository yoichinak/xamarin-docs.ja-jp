---
title: Xamarin.Forms ScrollView
description: この記事では、Xamarin.Forms ScrollView クラスを使用して、1 つだけの画面上に一致することはできませんし、キーボードのためのコンテンツがあるが、レイアウトを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: bb10cda7c9899f176861ceee712cc876984c56ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488271"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) レイアウトを含み、オフスクリーン スクロールを有効になります。 `ScrollView` キーボードを表示するときに、画面の表示部分に自動的に移動するビューを許可するも使用されます。

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>目的

[`ScrollView`](xref:Xamarin.Forms.ScrollView)を使用すると、より大きなビューが小さい電話でも表示されるようにすることができます。 たとえば、iPhone 6 s で動作するレイアウトを iPhone 4 秒で切れる可能性があります。 使用して、`ScrollView`できるよう、小さい画面に表示されるレイアウトのクリップの一部になります。

## <a name="usage"></a>使用状況

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)オブジェクトを入れ子にすることはできません。 さらに、 `ScrollView`s が他のコントロールなどスクロールなどを提供すると入れ子にする必要がありますできません`ListView`と`WebView`します。

[`ScrollView`](xref:Xamarin.Forms.ScrollView)は `Content` プロパティを公開します。これは、1つのビューまたはレイアウトに設定できます。 この例の後に、非常に大きな boxView のレイアウトを検討してください、 `Entry`:

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
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

ユーザーがスクロール ダウンしているだけ前に、`BoxView`が表示されます。

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

ユーザーが画面にテキストを入力すると、`Entry`画面に表示されるようにする、ビューをスクロールします。

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>[プロパティ]

[`ScrollView`](xref:Xamarin.Forms.ScrollView) は次の特性を定義します。

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) 取得、 [ `Size` ](xref:Xamarin.Forms.Size)コンテンツのサイズを表す値です。
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) 取得または設定します、 [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation)のスクロール方向を表す列挙値、`ScrollView`します。
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) 取得、`double`現在スクロール位置を表します。
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) 取得、 `double` Y の現在のスクロール位置を表します。
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) 取得または設定します、 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility)水平スクロール バーが表示されるときを表す値です。
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) 取得または設定します、 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility)垂直スクロール バーが表示されるときを表す値です。

> [!NOTE]
> [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)プロパティを `Neither`に設定すると、スクロールを無効にすることができます。

## <a name="methods"></a>メソッド

[`ScrollView`](xref:Xamarin.Forms.ScrollView)には、`ScrollToAsync` メソッドが用意されています。このメソッドを使用すると、座標を使用してビューをスクロールしたり、表示する必要のある特定のビューを指定したりできます。

座標を使用する場合は、指定、`x`と`y`と共にスクロールをアニメーション化するかどうかを示すブール値の座標。

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)プロパティが `Neither`に設定されている場合、`ScrollToAsync` メソッドはスクロールしません。

`ScrollToPosition` 列挙体は、特定の要素にスクロールするときに、要素が表示されるビュー内の場所を指定します。

- **Center** &ndash;ビューの表示部分の中央に要素をスクロールします。
- **終了**&ndash;ビューの表示部分の末尾に要素をスクロールします。
- **MakeVisible** &ndash;ビュー内で表示されるように要素をスクロールします。
- **開始**&ndash;ビューの表示部分の先頭に要素をスクロールします。

`IsAnimated`プロパティは、ビューのスクロール方法を指定します。 `true`に設定すると、コンテンツをすぐに表示に移動するのではなく、滑らかなアニメーションが使用されます。

## <a name="events"></a>Events

[`ScrollView`](xref:Xamarin.Forms.ScrollView)は、1つのイベント、`Scrolled`を定義します。 `Scrolled` スクロール、表示が完了したときに発生します。 イベント ハンドラー`Scrolled`は`ScrolledEventArgs`を持つ、`ScrollX`と`ScrollY`プロパティ。 次の現在のスクロール位置にラベルを更新する方法を示します、 `ScrollView`:

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
