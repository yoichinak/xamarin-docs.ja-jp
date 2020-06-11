---
title: " Xamarin.Forms stacklayout" description: "StackLayout は、1次元スタック内の子ビューを水平方向または垂直方向に整理します。"
ms. 製品: xamarin ms. assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 05/11/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-stacklayout"></a>Xamarin.FormsStackLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)

[![Xamarin.FormsStackLayout](stacklayout-images/layouts.png "[!ファンド.NO LOC (Xamarin. Forms)] StackLayout")](stacklayout-images/layouts-large.png#lightbox "[!ファンド.NO LOC (Xamarin. Forms)] StackLayout")

は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 1 次元のスタック内の子ビューを水平方向または垂直方向に整理します。 既定では、は `StackLayout` 垂直方向に配置されます。 また、は、 `StackLayout` 他の子レイアウトを含む親レイアウトとして使用できます。

[`StackLayout`](xref:Xamarin.Forms.StackLayout)クラスは、次のプロパティを定義します。

- [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)型のは、 [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) 子ビューが配置される方向を表します。 このプロパティの既定値は `Vertical` です。
- [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)型のは、 `double` 各子ビュー間のスペースの大きさを示します。 このプロパティの既定値は、デバイスに依存しない6つの単位です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、プロパティは、データバインディングのターゲットとスタイルを設定できます。

クラスは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) `Layout<T>` 型のプロパティを定義するクラスから派生 `Children` `IList<T>` します。 `Children`プロパティは `ContentProperty` クラスのである `Layout<T>` ため、XAML から明示的に設定する必要はありません。

> [!TIP]
> レイアウトの最適なパフォーマンスを得るには、「[レイアウトのパフォーマンスを最適化](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)する」のガイドラインに従ってください。

## <a name="vertical-orientation"></a>垂直方向

次の XAML は、異なる子ビューを含む垂直方向のを作成する方法を示して [`StackLayout`](xref:Xamarin.Forms.StackLayout) います。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.VerticalStackLayoutPage"
             Title="Vertical StackLayout demo">
    <StackLayout Margin="20">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

この例では、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) オブジェクトとオブジェクトを含む垂直方向のを作成し [`Label`](xref:Xamarin.Forms.Label) [`BoxView`](xref:Xamarin.Forms.BoxView) ます。 既定では、子ビューの間には、デバイスに依存しない6つの領域があります。

[![垂直方向の StackLayout のスクリーンショット](stacklayout-images/vertical.png "垂直方向の StackLayout")](stacklayout-images/vertical-large.png#lightbox "垂直方向の StackLayout")

これに相当する C# コードを次に示します。

```csharp
public class VerticalStackLayoutPageCS : ContentPage
{
    public VerticalStackLayoutPageCS()
    {
        Title = "Vertical StackLayout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

> [!NOTE]
> プロパティの値は、 [`Margin`](xref:Xamarin.Forms.View.Margin) 要素とそれに隣接する要素との距離を表します。 詳細については「[Margin and Padding](margin-and-padding.md)」 (余白とスペース) を参照してください。

## <a name="horizontal-orientation"></a>水平方向

次の XAML は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) プロパティをに設定することによって、水平方向のを作成する方法を示してい [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) `Horizontal` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.HorizontalStackLayoutPage"
             Title="Horizontal StackLayout demo">
    <StackLayout Margin="20"
                 Orientation="Horizontal"
                 HorizontalOptions="Center">
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

次の例では [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`BoxView`](xref:Xamarin.Forms.BoxView) 、子ビューの間に、デバイスに依存しない6つの領域を含む水平方向のオブジェクトを作成します。

[![水平方向の StackLayout のスクリーンショット](stacklayout-images/horizontal.png "水平方向の StackLayout")](stacklayout-images/horizontal-large.png#lightbox "水平方向の StackLayout")

これに相当する C# コードを次に示します。

```csharp
public HorizontalStackLayoutPageCS()
{
    Title = "Horizontal StackLayout demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Orientation = StackOrientation.Horizontal,
        HorizontalOptions = LayoutOptions.Center,
        Children =
        {
            new BoxView { Color = Color.Red },
            new BoxView { Color = Color.Yellow },
            new BoxView { Color = Color.Blue },
            new BoxView { Color = Color.Green },
            new BoxView { Color = Color.Orange },
            new BoxView { Color = Color.Purple }
        }
    };
}
```

## <a name="space-between-child-views"></a>子ビュー間のスペース

の子ビュー間の間隔は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) プロパティを値に設定することによって変更でき `double` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.StackLayoutSpacingPage"
             Title="StackLayout Spacing demo">
    <StackLayout Margin="20"
                 Spacing="0">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

この例では、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label) オブジェクトとオブジェクトの間にスペースがない、とのオブジェクトを含む縦書きを作成し [`BoxView`](xref:Xamarin.Forms.BoxView) ます。

[![空白を含まない StackLayout のスクリーンショット](stacklayout-images/spacing.png "StackLayout (スペースなし)")](stacklayout-images/spacing-large.png#lightbox "StackLayout (スペースなし)")

> [!TIP]
> [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)子ビューが重なり合うように、プロパティを負の値に設定できます。

これに相当する C# コードを次に示します。

```csharp
public class StackLayoutSpacingPageCS : ContentPage
{
    public StackLayoutSpacingPageCS()
    {
        Title = "StackLayout Spacing demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Spacing = 0,
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

## <a name="position-and-size-of-child-views"></a>子ビューの位置とサイズ

内の子ビューのサイズと位置は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 子ビューの値およびプロパティの値 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) と、それらのプロパティとプロパティの値によって異なり [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) ます。 垂直方向の [`StackLayout`](xref:Xamarin.Forms.StackLayout) 子ビューは、サイズが明示的に設定されていない場合に、使用可能な幅を埋めるように展開されます。 同様に、水平方向の `StackLayout` 子ビューは、そのサイズが明示的に設定されていない場合に、使用可能な高さを埋めるように展開されます。

と [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) その子ビューのプロパティおよびプロパティは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 2 つのレイアウト設定をカプセル化する構造体のフィールドに設定できます。

- *配置*では、親レイアウト内の子ビューの位置とサイズを決定します。
- [*展開*] は、子ビューが使用可能な場合に余分なスペースを使用する必要があるかどうかを示します。

> [!TIP]
> [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 必要がある場合を除き、のプロパティとプロパティを設定しないで [`StackLayout`](xref:Xamarin.Forms.StackLayout) ください。 既定値の `LayoutOptions.Fill` と `LayoutOptions.FillAndExpand` で、レイアウトの最適化が最大になります。 これらのプロパティを変更すると、既定値に戻す場合でも、コストがかかり、メモリが消費されます。

### <a name="alignment"></a>アラインメント

次の XAML の例では、の各子ビューに配置設定を設定し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.AlignmentPage"
             Title="Alignment demo">
    <StackLayout Margin="20">
        <Label Text="Start"
               BackgroundColor="Gray"
               HorizontalOptions="Start" />
        <Label Text="Center"
               BackgroundColor="Gray"
               HorizontalOptions="Center" />
        <Label Text="End"
               BackgroundColor="Gray"
               HorizontalOptions="End" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               HorizontalOptions="Fill" />
    </StackLayout>
</ContentPage>
```

この例では、オブジェクトに配置設定を設定して [`Label`](xref:Xamarin.Forms.Label) 、内での位置を制御し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)、、 [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) [`End`](xref:Xamarin.Forms.LayoutOptions.End) 、およびの [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 各フィールドは、親内のオブジェクトの配置を定義するために使用され [`Label`](xref:Xamarin.Forms.Label) `StackLayout` ます。

[![アラインメントオプションが設定される StackLayout のスクリーンショット](stacklayout-images/alignment.png "アラインメントオプション付き StackLayout")](stacklayout-images/alignment-large.png#lightbox "アラインメントオプション付き StackLayout")

[`StackLayout`](xref:Xamarin.Forms.StackLayout) は、`StackLayout` の方向とは反対方向にある子ビューの配置設定のみに従います。 したがって、垂直方向の `StackLayout` 内の [`Label`](xref:Xamarin.Forms.Label) 子ビューは、それらの [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティを次の配置フィールドの 1 つに設定します。

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)。をの [`Label`](xref:Xamarin.Forms.Label) 左辺に配置し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の中央に配置します。
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)。をの [`Label`](xref:Xamarin.Forms.Label) 右側に配置し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)。これにより、[`Label`](xref:Xamarin.Forms.Label) によって [`StackLayout`](xref:Xamarin.Forms.StackLayout) の幅が埋まるようになります。

これに相当する C# コードを次に示します。

```csharp
public class AlignmentPageCS : ContentPage
{
    public AlignmentPageCS()
    {
        Title = "Alignment demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
                new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
                new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
                new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
            }
        };
    }
}
```

### <a name="expansion"></a>正規の表記

次の XAML の例では、の各に拡張設定を設定し [`Label`](xref:Xamarin.Forms.Label) [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.ExpansionPage"
             Title="Expansion demo">
    <StackLayout Margin="20">
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Start"
               BackgroundColor="Gray"
               VerticalOptions="StartAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Center"
               BackgroundColor="Gray"
               VerticalOptions="CenterAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="End"
               BackgroundColor="Gray"
               VerticalOptions="EndAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               VerticalOptions="FillAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
    </StackLayout>
</ContentPage>
```

この例では、オブジェクトに拡張設定を設定し [`Label`](xref:Xamarin.Forms.Label) て、内でのサイズを制御し [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)、、 [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand) [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) 、およびの [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) 各フィールドは、配置の設定を定義するために使用されます。また、が `Label` 親内で使用可能な場合は、より多くの領域を使用するかどうかを指定し `StackLayout` ます。

[![拡張オプションが設定される StackLayout のスクリーンショット](stacklayout-images/expansion.png "StackLayout と拡張オプション")](stacklayout-images/expansion-large.png#lightbox "StackLayout と拡張オプション")

[`StackLayout`](xref:Xamarin.Forms.StackLayout) は子ビューをその方向にのみ展開できます。 したがって、垂直方向の `StackLayout` は、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティをいずれかの展開フィールドに設定する [`Label`](xref:Xamarin.Forms.Label) 子ビューを展開できます。 つまり、垂直方向の配置では、各 `Label` が `StackLayout` 内で同じ量のスペースを占有します。 ただし、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) に設定する最後の `Label` のみ、サイズが異なります。

> [!TIP]
> を使用する場合 [`StackLayout`](xref:Xamarin.Forms.StackLayout) は、子ビューが1つだけに設定されていることを確認して [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) ください。 このプロパティにより、指定された子は、`StackLayout` がそれに与えられる最大の領域を占有します。このような計算を複数回実行することは無駄です。

これに相当する C# コードを次に示します。

```csharp
public ExpansionPageCS()
{
    Title = "Expansion demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children =
        {
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
        }
    };
}
```

> [!IMPORTANT]
> [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内のすべてのスペースが使用されている場合、展開設定は無効になります。

配置と展開の詳細については、「 [」 Xamarin.Forms の「レイアウトオプション](layout-options.md)」を参照してください。

## <a name="nested-stacklayout-objects"></a>入れ子になった StackLayout オブジェクト

は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 入れ子になった子 `StackLayout` オブジェクトまたはその他の子レイアウトを含む親レイアウトとして使用できます。

次の XAML は、オブジェクトの入れ子の例を示してい [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.CombinedStackLayoutPage"
             Title="Combined StackLayouts demo">
    <StackLayout Margin="20">
        ...
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Red" />
                <Label Text="Red"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Yellow" />
                <Label Text="Yellow"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Blue" />
                <Label Text="Blue"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        ...
    </StackLayout>
</ContentPage>
```

この例では、親に、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) オブジェクト内の入れ子になったオブジェクトが含まれてい `StackLayout` [`Frame`](xref:Xamarin.Forms.Frame) ます。 親は `StackLayout` 垂直方向で、子 `StackLayout` オブジェクトは水平方向に配置されます。

[![入れ子になった StackLayout オブジェクトのスクリーンショット](stacklayout-images/combined.png "入れ子になった StackLayouts")](stacklayout-images/combined-large.png#lightbox "入れ子になった StackLayouts")

> [!IMPORTANT]
> [`StackLayout`](xref:Xamarin.Forms.StackLayout)オブジェクトやその他のレイアウトの入れ子を深くするほど、入れ子になったレイアウトの方がパフォーマンスに影響します。 詳細については、「[適切なレイアウトを選択する](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout)」を参照してください。

これに相当する C# コードを次に示します。

```csharp
public class CombinedStackLayoutPageCS : ContentPage
{
    public CombinedStackLayoutPageCS()
    {
        Title = "Combined StackLayouts demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Red },
                            new Label { Text = "Red", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Yellow },
                            new Label { Text = "Yellow", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Blue },
                            new Label { Text = "Blue", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                // ...
            }
        };
    }
}
```

## <a name="related-links"></a>関連リンク

- [StackLayout のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)
- [レイアウトオプションXamarin.Forms](layout-options.md)
- [レイアウトの選択 Xamarin.Forms](choose-layout.md)
- [アプリのパフォーマンスを向上させる Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
