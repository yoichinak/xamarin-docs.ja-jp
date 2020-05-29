---
title: ' Xamarin.Forms ScrollView ' 説明: ' この記事では、ScrollView クラスを使用して、 Xamarin.Forms 1 つの画面に収めることができないレイアウトと、キーボードのためのスペースを作成する方法について説明します。 "
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-scrollview"></a>Xamarin.FormsScrollView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView)レイアウトが含まれており、スクリーンオフでスクロールできます。 `ScrollView`は、キーボードが表示されているときに、画面の可視部分にビューを自動的に移動できるようにするためにも使用されます。

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>目的

[`ScrollView`](xref:Xamarin.Forms.ScrollView)を使用すると、より大きなビューを小さい電話でも確実に表示できます。 たとえば、iPhone 6s で動作するレイアウトは、iPhone 4s でクリップすることができます。 を使用する `ScrollView` と、レイアウトのクリップ部分を小さい画面に表示できます。

## <a name="usage"></a>使用

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)オブジェクトを入れ子にすることはできません。 また、は、 `ScrollView` やなど、スクロールを提供する他のコントロールと入れ子にすることはできません `ListView` `WebView` 。

[`ScrollView`](xref:Xamarin.Forms.ScrollView)`Content`1 つのビューまたはレイアウトに設定できるプロパティを公開します。 この例では、サイズが非常に大きい boxView を使用し、その後に次のようなレイアウトを使用し `Entry` ます。

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

ユーザーが下へスクロールする前に、のみ `BoxView` が表示されます。

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

ユーザーがのテキストの入力を開始すると、 `Entry` 画面に表示されるようにビューがスクロールします。

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>プロパティ

[`ScrollView`](xref:Xamarin.Forms.ScrollView)では、次のプロパティが定義されています。

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty)[`Size`](xref:Xamarin.Forms.Size)コンテンツのサイズを表す値を取得します。
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)[`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation)のスクロール方向を表す列挙値を取得または設定し `ScrollView` ます。
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty)現在の `double` X スクロール位置を表すを取得します。
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty)現在の `double` Y スクロール位置を表すを取得します。
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty)[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)水平スクロールバーが表示されるタイミングを表す値を取得または設定します。
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty)[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)垂直スクロールバーが表示されるタイミングを表す値を取得または設定します。

> [!NOTE]
> プロパティをに設定すると、スクロールを無効にすることができ [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `Neither` ます。

## <a name="methods"></a>メソッド

[`ScrollView`](xref:Xamarin.Forms.ScrollView)メソッドを提供します。このメソッドを使用すると `ScrollToAsync` 、座標を使用して、または表示する必要がある特定のビューを指定することによって、ビューをスクロールできます。

座標を使用する場合は、との座標と、スクロールをアニメーション化する `x` `y` かどうかを示すブール値を指定します。

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> `ScrollToAsync` [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) プロパティがに設定されている場合、メソッドはスクロール `Neither` しません。

特定の要素にスクロールすると、 `ScrollToPosition` ビュー内の要素の表示位置が列挙体によって指定されます。

- **中央** &ndash;要素をビューの表示部分の中央にスクロールします。
- **終了** &ndash;要素をビューの可視部分の末尾までスクロールします。
- **Makevisible** &ndash;要素をスクロールして、ビュー内に表示されるようにします。
- **開始** &ndash;要素をビューの可視部分の先頭までスクロールします。

プロパティは、 `IsAnimated` ビューをどのようにスクロールするかを指定します。 に設定すると、 `true` コンテンツをすぐにビューに移動するのではなく、滑らかなアニメーションが使用されます。

## <a name="events"></a>events

[`ScrollView`](xref:Xamarin.Forms.ScrollView)1つのイベントだけを定義 `Scrolled` します。 `Scrolled`ビューのスクロールが終了したときに発生します。 のイベントハンドラーは `Scrolled` `ScrolledEventArgs` 、プロパティとプロパティを持つを受け取り `ScrollX` `ScrollY` ます。 次の例は、の現在のスクロール位置でラベルを更新する方法を示してい `ScrollView` ます。

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
