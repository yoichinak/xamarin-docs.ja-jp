---
title: " Xamarin.Forms ScrollView" description: " Xamarin.Forms ScrollView は、そのコンテンツをスクロールできるレイアウトです。"
ms. 製品: xamarin ms. assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 05/27/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-scrollview"></a>Xamarin.FormsScrollView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)

[![Xamarin.FormsScrollView](scrollview-images/layouts.png "[!ファンド.NO LOC (Xamarin. Forms)] ScrollView")](scrollview-images/layouts-large.png#lightbox "[!ファンド.NO LOC (Xamarin. Forms)] ScrollView")

[`ScrollView`](xref:Xamarin.Forms.ScrollView)は、コンテンツをスクロールできるレイアウトです。 `ScrollView`クラスはクラスから派生 [`Layout`](xref:Xamarin.Forms.Layout) し、既定でコンテンツを垂直方向にスクロールします。 には `ScrollView` 1 つの子のみを指定できますが、これは他のレイアウトでもかまいません。

> [!WARNING]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)オブジェクトを入れ子にすることはできません。 また、 `ScrollView` オブジェクトは、、、などのスクロールを提供する他のコントロールで入れ子にすることはできません [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView) [`WebView`](xref:Xamarin.Forms.WebView) 。

[`ScrollView`](xref:Xamarin.Forms.ScrollView)では、次のプロパティが定義されています。

- [`Content`](xref:Xamarin.Forms.ScrollView.Content)型のは、 [`View`](xref:Xamarin.Forms.View) に表示されるコンテンツを表し [`ScrollView`](xref:Xamarin.Forms.ScrollView) ます。
- [`ContentSize`](xref:Xamarin.Forms.ScrollView)型のは、 [`Size`](xref:Xamarin.Forms.Size) コンテンツのサイズを表します。 これは、読み取り専用プロパティです。
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)型のは、 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) 水平スクロールバーが表示されるタイミングを表します。
- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation)型のは、の [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) スクロール方向を表し [`ScrollView`](xref:Xamarin.Forms.ScrollView) ます。 このプロパティの既定値は `Vertical` です。
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollX)型のは、 `double` 現在の X スクロール位置を示します。 この読み取り専用プロパティの既定値は0です。
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollY)型のは、 `double` 現在の Y スクロール位置を示します。 この読み取り専用プロパティの既定値は0です。
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)型のは、 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) 垂直スクロールバーが表示されるタイミングを表します。

これらのプロパティは、プロパティを除き、オブジェクトによってバックアップされます [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) [`Content`](xref:Xamarin.Forms.ScrollView.Content) 。これは、データバインディングのターゲットとスタイルを持つことができることを意味します。

[`Content`](xref:Xamarin.Forms.ScrollView.Content)プロパティは [`ContentProperty`](xref:Xamarin.Forms.ContentPropertyAttribute) クラスのである [`ScrollView`](xref:Xamarin.Forms.ScrollView) ため、XAML から明示的に設定する必要はありません。

> [!TIP]
> レイアウトの最適なパフォーマンスを得るには、「[レイアウトのパフォーマンスを最適化](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)する」のガイドラインに従ってください。

## <a name="scrollview-as-a-root-layout"></a>ルートレイアウトとしての ScrollView

は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 1 つの子のみを持つことができ、他のレイアウトにすることができます。 したがって、は、ページのルートレイアウトとして一般的に使用 `ScrollView` されます。 子コンテンツをスクロールするために、は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) コンテンツの高さとその高さの差を計算します。 この違いは、がコンテンツを `ScrollView` スクロールできる量です。

は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 多くの場合、の子になり `ScrollView` ます。 このシナリオでは、は、の `ScrollView` `StackLayout` 子の高さの合計の高さになります。 次に、は、 `ScrollView` コンテンツをスクロールできる量を決定できます。 の詳細については `StackLayout` 、「 [ Xamarin.Forms stacklayout](stacklayout.md)」を参照してください。

> [!CAUTION]
> 垂直方向では、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) プロパティを `VerticalOptions` 、、またはに設定しないで `Start` `Center` `End` ください。 これにより、は、 `ScrollView` 必要なだけの高さになるようになります。これは、ゼロになる可能性があります。 Xamarin.Formsはこの不測に対して保護されていますが、発生させたくないことを示すコードを避けることをお勧めします。

次の XAML の例では、が [`ScrollView`](xref:Xamarin.Forms.ScrollView) ページのルートレイアウトとして使用されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ScrollViewDemos"
             x:Class="ScrollViewDemos.Views.ColorListPage"
             Title="ScrollView demo">
    <ScrollView>
        <StackLayout BindableLayout.ItemsSource="{x:Static local:NamedColor.All}">
            <BindableLayout.ItemTemplate>
                <DataTemplate>
                    <StackLayout Orientation="Horizontal">
                        <BoxView Color="{Binding Color}"
                                 HeightRequest="32"
                                 WidthRequest="32"
                                 VerticalOptions="Center" />
                        <Label Text="{Binding FriendlyName}"
                               FontSize="24"
                               VerticalOptions="Center" />
                    </StackLayout>
                </DataTemplate>
            </BindableLayout.ItemTemplate>
        </StackLayout>
    </ScrollView>
</ContentPage>
```

この例では、の [`ScrollView`](xref:Xamarin.Forms.ScrollView) コンテンツが、バインド可能 [`StackLayout`](xref:Xamarin.Forms.StackLayout) なレイアウトを使用し [`Color`](xref:Xamarin.Forms.Color) てで定義されたフィールドを表示するに設定されてい Xamarin.Forms ます。 既定では、は `ScrollView` 垂直方向にスクロールします。これにより、さらに多くの内容が示されます。

[![ルート ScrollView レイアウトのスクリーンショット](scrollview-images/root-layout.png "ルート ScrollView のレイアウト")](scrollview-images/root-layout-large.png#lightbox "ルート ScrollView のレイアウト")

これに相当する C# コードを次に示します。

```csharp
public class ColorListPageCode : ContentPage
{
    public ColorListPageCode()
    {
        DataTemplate dataTemplate = new DataTemplate(() =>
        {
            BoxView boxView = new BoxView
            {
                HeightRequest = 32,
                WidthRequest = 32,
                VerticalOptions = LayoutOptions.Center
            };
            boxView.SetBinding(BoxView.ColorProperty, "Color");

            Label label = new Label
            {
                FontSize = 24,
                VerticalOptions = LayoutOptions.Center
            };
            label.SetBinding(Label.TextProperty, "FriendlyName");

            StackLayout horizontalStackLayout = new StackLayout
            {
                Orientation = StackOrientation.Horizontal,
                Children = { boxView, label }
            };
            return horizontalStackLayout;
        });

        StackLayout stackLayout = new StackLayout();
        BindableLayout.SetItemsSource(stackLayout, NamedColor.All);
        BindableLayout.SetItemTemplate(stackLayout, dataTemplate);

        ScrollView scrollView = new ScrollView { Content = stackLayout };

        Title = "ScrollView demo";
        Content = scrollView;
    }
}
```

バインド可能なレイアウトの詳細については、「 [」の Xamarin.Forms 「バインド](bindable-layouts.md)可能なレイアウト」を参照してください。

## <a name="scrollview-as-a-child-layout"></a>子レイアウトとしての ScrollView

は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 別の親レイアウトに対する子レイアウトにすることができます。

は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 多くの場合、の子になり [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 では、 `ScrollView` コンテンツの高さと独自の高さの差を計算するために、特定の高さが必要です。これは、がコンテンツをスクロールできる量とは異なり `ScrollView` ます。 `ScrollView`がの子である場合 `StackLayout` 、特定の高さは取得されません。 は、 `StackLayout` `ScrollView` を可能な限り短くすることを望んでいます。これは、コンテンツの高さまたは0のいずれかになり `ScrollView` ます。 このシナリオを処理するには、 `VerticalOptions` のプロパティを `ScrollView` に設定する必要があり `FillAndExpand` ます。 これにより、は `StackLayout` `ScrollView` 他の子に必要のない余分な領域をすべて提供し、には `ScrollView` 特定の高さが設定されます。

次の XAML の例では、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) に対する子レイアウトとしてが使用されてい [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ScrollViewDemos.Views.BlackCatPage"
             Title="ScrollView as a child layout demo">
    <StackLayout Margin="20">
        <Label Text="THE BLACK CAT by Edgar Allan Poe"
               FontSize="Medium"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
        <ScrollView VerticalOptions="FillAndExpand">
            <StackLayout>
                <Label Text="FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects." />
                <!-- More Label objects go here -->
            </StackLayout>
        </ScrollView>
    </StackLayout>
</ContentPage>
```

この例では、2つのオブジェクトがあり [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 1つ目 `StackLayout` はルートレイアウトオブジェクトで、 [`Label`](xref:Xamarin.Forms.Label) オブジェクトとが [`ScrollView`](xref:Xamarin.Forms.ScrollView) 子要素として使用されます。 は、 `ScrollView` を `StackLayout` コンテンツとして持ち、には複数のオブジェクトが含まれてい `StackLayout` `Label` ます。 このように配置することで、最初 `Label` は常に画面上に表示され、他のオブジェクトによって表示されるテキストはスクロールできるようになり `Label` ます。

[![子 ScrollView レイアウトのスクリーンショット](scrollview-images/child-layout.png "子 ScrollView のレイアウト")](scrollview-images/child-layout-large.png#lightbox "子 ScrollView のレイアウト")

これに相当する C# コードを次に示します。

```csharp
public class BlackCatPageCS : ContentPage
{
    public BlackCatPageCS()
    {
        Label titleLabel = new Label
        {
            Text = "THE BLACK CAT by Edgar Allan Poe",
            // More properties set here to define the Label appearance
        };

        ScrollView scrollView = new ScrollView
        {
            VerticalOptions = LayoutOptions.FillAndExpand,
            Content = new StackLayout
            {
                Children =
                {
                    new Label
                    {
                        Text = "FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects.",
                    },
                    // More Label objects go here
                }
            }
        };

        Title = "ScrollView as a child layout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { titleLabel, scrollView }
        };
    }
}
```

## <a name="orientation"></a>方向

[`ScrollView`](xref:Xamarin.Forms.ScrollView)には、 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) のスクロール方向を表すプロパティがあり `ScrollView` ます。 このプロパティの型は [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) で、次のメンバーを定義します。

- `Vertical`が垂直方向にスクロールすることを示し `ScrollView` ます。 このメンバーは、プロパティの既定値です [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 。
- `Horizontal`が水平にスクロールすることを示し `ScrollView` ます。
- `Both`が水平方向および垂直方向にスクロールすることを示し `ScrollView` ます。
- `Neither`が `ScrollView` スクロールしないことを示します。

> [!TIP]
> プロパティをに設定すると、スクロールを無効にすることができ [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `Neither` ます。

## <a name="detect-scrolling"></a>スクロールの検出

[`ScrollView`](xref:Xamarin.Forms.ScrollView)スクロールが [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) 発生したことを示すために発生するイベントを定義します。 [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs)イベントに付随するオブジェクト `Scrolled` には `ScrollX` 、 `ScrollY` 型とプロパティの両方があり `double` ます。

> [!IMPORTANT]
> `ScrolledEventArgs.ScrollX`プロパティとプロパティには、 `ScrolledEventArgs.ScrollY` の先頭にスクロールするときに発生するバウンス効果があるため、負の値を指定できます [`ScrollView`](xref:Xamarin.Forms.ScrollView) 。

次の XAML の例は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) イベントのイベントハンドラーを設定するを示してい [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) ます。

```xaml
<ScrollView Scrolled="OnScrollViewScrolled">
        ...
</ScrollView>
```

これに相当する C# コードを次に示します。

```csharp
ScrollView scrollView = new ScrollView();
scrollView.Scrolled += OnScrollViewScrolled;
```

この例では、イベント `OnScrollViewScrolled` の発生時にイベントハンドラーが実行され [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) ます。

```csharp
void OnScrollViewScrolled(object sender, ScrolledEventArgs e)
{
    Console.WriteLine($"ScrollX: {e.ScrollX}, ScrollY: {e.ScrollY}");
}
```

この例では、イベントハンドラーは、 `OnScrollViewScrolled` [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs) イベントに付随するオブジェクトの値を出力します。

> [!NOTE]
> イベントは、 [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) ユーザーが開始したスクロールと、プログラムによるスクロールのために発生します。

## <a name="scroll-programmatically"></a>プログラムによるスクロール

[`ScrollView`](xref:Xamarin.Forms.ScrollView)を [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) 非同期的にスクロールする2つのメソッドを定義し `ScrollView` ます。 オーバーロードの1つは、内の指定された位置までスクロール `ScrollView` し、もう一方は指定された要素をビューにスクロールします。 どちらのオーバーロードにも、スクロールをアニメーション化するかどうかを示すために使用できる追加の引数があります。

> [!IMPORTANT]
> [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) プロパティがに設定されている場合、メソッドはスクロール `Neither` しません。

### <a name="scroll-a-position-into-view"></a>位置をビューにスクロールする

内の位置は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) `double` `x` 引数と引数を受け取るメソッドを使用してにスクロールでき `y` ます。 という名前の垂直方向のオブジェクトがあるとし `ScrollView` `scrollView` ます。次の例では、の上部から、デバイスに依存しない150の単位までスクロールする方法を示してい `ScrollView` ます。

```csharp
await scrollView.ScrollToAsync(0, 150, true);
```

の3番目の引数 [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) は `animated` 引数で、プログラムを使用してをスクロールするときにスクロールアニメーションを表示するかどうかを決定し [`ScrollView`](xref:Xamarin.Forms.ScrollView) ます。

### <a name="scroll-an-element-into-view"></a>要素をスクロールして表示します

内の要素は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) 引数と引数を受け取るメソッドを使用して、ビューにスクロールでき [`Element`](xref:Xamarin.Forms.Element) [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) ます。 という名前のとという名前のを指定した場合、 `ScrollView` `scrollView` [`Label`](xref:Xamarin.Forms.Label) `label` 次の例では、要素をスクロールして表示する方法を示しています。

```csharp
await scrollView.ScrollToAsync(label, ScrollToPosition.End, true);
```

の3番目の引数 [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) は `animated` 引数で、プログラムを使用してをスクロールするときにスクロールアニメーションを表示するかどうかを決定し [`ScrollView`](xref:Xamarin.Forms.ScrollView) ます。

要素をビューにスクロールするとき、スクロールが完了した後の要素の正確な位置は、メソッドの2番目の引数を使用して設定でき `position` [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) ます。 この引数は、 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) 列挙型のメンバーを受け取ります。

- `MakeVisible`要素がに表示されるまでスクロールする必要があることを示し `ScrollView` ます。
- `Start`要素をの先頭までスクロールする必要があることを示し `ScrollView` ます。
- `Center`要素をの中央にスクロールする必要があることを示し `ScrollView` ます。
- `End`要素をの末尾までスクロールする必要があることを示し `ScrollView` ます。

## <a name="scroll-bar-visibility"></a>スクロールバーの表示

[`ScrollView`](xref:Xamarin.Forms.ScrollView)[`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)バインド可能 [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView) なプロパティによってサポートされるプロパティとプロパティを定義します。 これらのプロパティは、 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) 水平方向または垂直方向のスクロールバーを表示するかどうかを示す列挙値を取得または設定します。 `ScrollBarVisibility` 列挙体を使って、次のメンバーを定義できます。

- `Default`プラットフォームの既定のスクロールバーの動作を示し `HorizontalScrollBarVisibility` ます。は、プロパティとプロパティの既定値です `VerticalScrollBarVisibility` 。
- `Always`ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- `Never`コンテンツがビューに収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="related-links"></a>関連リンク

- [ScrollView デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)
- [Xamarin.FormsStackLayout](stacklayout.md)
- [バインド可能なレイアウトXamarin.Forms](bindable-layouts.md)
