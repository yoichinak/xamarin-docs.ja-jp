---
title: ScrollView
description: "使用して ScrollView 提示のレイアウトを 1 つの画面上に収まるし、キーボードの領域を確保するコンテンツがあることはできません。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: c305075d37a25bfe828f16d4e69955437a591f9a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="scrollview"></a>ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) レイアウトが含まれていますでき、その画面外にスクロールします。 `ScrollView` キーボードが表示されている場合、画面の表示部分に自動的に移動するビューを許可するも使用されます。

[ ![](scroll-view-images/layouts-sml.png "Xamarin.Forms レイアウト")](scroll-view-images/layouts.png "Xamarin.Forms レイアウト")

この記事の内容について説明します。

- **[目的](#Purpose)** &ndash;目的は、`ScrollView`を使用する場合とします。
- **[使用状況](#Usage)** &ndash;の使用方法`ScrollView`実際にします。
- **[プロパティ](#Properties)** &ndash;パブリック プロパティを読み取りおよび変更できます。
- **[メソッド](#Methods)** &ndash;パブリック メソッド ビューをスクロールするために呼び出すことができます。
- **[イベント](#Events)** &ndash;ビューの状態の変化をリッスンするために使用するイベントです。

## <a name="purpose"></a>目的

`ScrollView` 小規模な携帯電話で大規模なビューが表示されるようにするために使用します。 たとえば、iPhone 6 s で動作するレイアウトは、iPhone 4 秒で切れる可能性があります。 使用して、`ScrollView`小さい画面に表示するレイアウトのクリップの一部になります。

## <a name="usage"></a>使用法

> [!NOTE]
> **注**: `ScrollView`s 入れ子にしてはいけない。 さらに、 `ScrollView`s と同様に、スクロールを提供する他のコントロールで入れ子にする必要がありますいない`ListView`と`WebView`です。

`ScrollView` 公開、`Content`プロパティ 1 つのビューやレイアウトを設定することができます。 続けて、非常に大きな boxView とレイアウトの例を考えてみます、 `Entry`:

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
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,   HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

ユーザーが、唯一下へスクロールする前に、`BoxView`が表示されます。

![](scroll-view-images/scroll-start.png "ScrollView に BoxView")

テキストを入力するユーザーが開始したとき、`Entry`画面に表示して、ビューをスクロールします。

![](scroll-view-images/scroll-end.png "ScrollView 内のエントリ")

## <a name="properties"></a>プロパティ

ScrollView には、次のプロパティがあります。

- **コンテンツ**&ndash;を取得または設定に表示するビュー、`ScrollView`です。
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash;読み取り専用、幅と高さコンポーネントを持つがコンテンツのサイズを取得します。 これは、バインド可能なプロパティ
- **[印刷の向き](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)** &ndash;これは、 [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)、これは、列挙型を設定できる`Horizontal`、 `Vertical`、または`Both`です。
- **ScrollX** &ndash;読み取り専用 X ディメンション内の現在のスクロール位置を取得します。
- **ScrollY** &ndash;読み取り専用 Y ディメンション内の現在のスクロール位置を取得します。

## <a name="methods"></a>メソッド

`ScrollView` 提供、`ScrollToAsync`ビューをスクロールするために使用するメソッドの座標を使用するか、値を表示できるようにする特定のビューを指定します。

座標を使用する場合を指定、`x`と`y`スクロールをアニメーション化するかどうかを示すブール値と共にの座標。

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

特定の要素にスクロールする場合、`ScrollToPosition`ビューで、要素が表示される場所の列挙を指定します。

- **Center** &ndash;ビューの表示部分の中央に要素をスクロールします。
- **終了**&ndash;ビューの表示部分の末尾に要素をスクロールします。
- **MakeVisible** &ndash;ビュー内で表示されるように要素をスクロールします。
- **開始**&ndash;ビューの表示部分の先頭に要素をスクロールします。

`IsAnimated`プロパティは、ビューのスクロール方法を指定します。 ときに滑らかなアニメーションを true に設定する適用されます、ビューに、コンテンツを瞬時に移動するのではなく。

## <a name="events"></a>イベント

`ScrollView` 1 つのイベントを公開`Scrolled`です。 `Scrolled` ビューのスクロールが完了すると発生します。 イベント ハンドラーの`Scrolled`受け取る`ScrolledEventArgs`を持つ、`ScrollX`と`ScrollY`プロパティです。 次の現在のスクロール位置のラベルを更新する方法を示します、 `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

スクロール位置を負の場合、リストの末尾にスクロールする場合は、バウンス影響が原因とする可能性がある注意してください。


## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
