---
title: ScrollView
description: この記事では、ScrollView クラスを使用して、1つの画面に収めることができないレイアウトと、キーボード用のコンテンツがあるレイアウトを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 8d523c6da6ca7feaf6894123822f789f37455865
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696850"
---
# <a name="xamarinforms-scrollview"></a>ScrollView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView)にはレイアウトが含まれており、スクリーンオフでスクロールできます。 また `ScrollView` は、キーボードが表示されているときに、画面の可視部分にビューを自動的に移動できるようにするためにも使用されます。

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>目的

[`ScrollView`](xref:Xamarin.Forms.ScrollView)を使用すると、より大きなビューが小さい電話でも表示されるようにすることができます。 たとえば、iPhone 6s で動作するレイアウトは、iPhone 4s でクリップすることができます。 @No__t_0 を使用すると、レイアウトのクリップ部分を小さい画面に表示できます。

## <a name="usage"></a>使用方法

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)オブジェクトを入れ子にすることはできません。 また、`ScrollView`s は、`ListView` や `WebView` などのスクロールを提供する他のコントロールと入れ子にすることはできません。

[`ScrollView`](xref:Xamarin.Forms.ScrollView)は `Content` プロパティを公開します。これは、1つのビューまたはレイアウトに設定できます。 この例では、サイズが非常に大きい boxView と、その後に `Entry` があるレイアウトを考えてみます。

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

ユーザーが下へスクロールする前に、`BoxView` のみが表示されます。

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

ユーザーが `Entry` にテキストを入力し始めると、画面に表示されるようにビューがスクロールします。

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>プロパティ

[`ScrollView`](xref:Xamarin.Forms.ScrollView) は次の特性を定義します。

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty)コンテンツのサイズを表す[`Size`](xref:Xamarin.Forms.Size)値を取得します。
- `ScrollView` のスクロール方向を表す[`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation)列挙値を[`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)取得または設定します。
- 現在の X スクロール位置を表す `double` を取得[`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty)します。
- 現在の Y スクロール位置を表す `double` を取得[`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty)します。
- 水平スクロールバーを表示するタイミングを表す[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)値を[`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty)取得または設定します。
- 垂直スクロールバーを表示するタイミングを表す[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)値を[`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty)取得または設定します。

> [!NOTE]
> [@No__t_1](xref:Xamarin.Forms.ScrollView.OrientationProperty)プロパティを `Neither` に設定すると、スクロールを無効にすることができます。

## <a name="methods"></a>メソッド

[`ScrollView`](xref:Xamarin.Forms.ScrollView)には、`ScrollToAsync` メソッドが用意されています。このメソッドを使用すると、座標を使用してビューをスクロールしたり、表示する必要のある特定のビューを指定したりできます。

座標を使用する場合は、`x` および `y` 座標と、スクロールをアニメーション化するかどうかを示すブール値を指定します。

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> [@No__t_2](xref:Xamarin.Forms.ScrollView.OrientationProperty)プロパティが `Neither` に設定されている場合、`ScrollToAsync` メソッドはスクロールしません。

@No__t_0 列挙体は、特定の要素にスクロールするときに、要素が表示されるビュー内の場所を指定します。

- [**中央**&ndash;] ビューの表示部分の中央に要素をスクロールします。
- **End** &ndash; は、要素をビューの表示部分の最後までスクロールします。
- **Makevisible** &ndash; 要素をスクロールして、ビュー内に表示されるようにします。
- **開始**&ndash;、要素をビューの表示部分の先頭までスクロールします。

@No__t_0 プロパティは、ビューをどのようにスクロールするかを指定します。 @No__t_0 に設定すると、コンテンツをすぐに表示に移動するのではなく、滑らかなアニメーションが使用されます。

## <a name="events"></a>イベント

[`ScrollView`](xref:Xamarin.Forms.ScrollView)は、1つのイベント、`Scrolled` を定義します。 `Scrolled` は、ビューのスクロールが終了したときに発生します。 @No__t_0 のイベントハンドラーは、`ScrollX` プロパティと `ScrollY` プロパティを持つ `ScrolledEventArgs` を受け取ります。 次の例では、`ScrollView` の現在のスクロール位置でラベルを更新する方法を示します。

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

スクロールの位置は、リストの末尾でスクロールするときのバウンス効果のため、負の値になることがあります。

## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble の例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
