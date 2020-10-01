---
title: デュアル画面デバイスに関する Xamarin.Forms の機能
description: このガイドでは、Xamarin.Forms の DualScreenInfo クラスを使用して Surface Duo や Surface Neo などのデュアル画面デバイスのアプリ エクスペリエンスを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4e6bbd40fb80c2884013647ced3c3660ab4af738
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562679"
---
# <a name="no-locxamarinforms-dualscreeninfo-helper-class"></a>Xamarin.Forms DualScreenInfo ヘルパー クラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

`DualScreenInfo` で、表示されるペイン、その大きさ、デバイスがどのようなものか、ヒンジの角度などを決定できるようになります。

## <a name="configure-dualscreeninfo"></a>DualScreenInfo を構成する

次の手順に従って、アプリにデュアル画面レイアウトを作成します。

1. [概要](index.md)手順に従って NuGet を追加し、Android `MainActivity` クラスを構成します。
1. クラス ファイルに `using Xamarin.Forms.DualScreen;` を追加します。
1. アプリで `DualScreenInfo.Current` クラスを使用します。

## <a name="properties"></a>プロパティ

- 2 画面にまたがっている場合は、`SpanningBounds` が表示可能な各領域の境界を示す 2 つの四角形を返します。 ウィンドウが複数にまたがっていない場合は、空の配列が返されます。
- `HingeBounds` は画面上のヒンジの位置を示します。
- `IsLandscape` はデバイスが横向きであるかどうかを示します。 アプリケーションが複数にまたがっている場合、ネイティブの向きの API では向きが正しく報告されないため、これは便利です。
- `SpanMode` はレイアウトが縦長、横長、または単一のペイン モードのどれかを示します。

また、`PropertyChanged` イベントはプロパティが変更されたときに発生し、`HingeAngleChanged` イベントはヒンジ角度が変更されたときに発生します。

## <a name="poll-hinge-angle-on-android-and-uwp"></a>Android および UWP でのヒンジ角度のポーリング

次のメソッドは、Android および UWP プラットフォーム プロジェクトから `DualScreenInfo` にアクセスする場合に使用できます。

- `GetHingeAngleAsync` はデバイスのヒンジの現在の角度を取得します。 シミュレーターを使用する場合は、圧力センサーを変更することで HingeAngle を設定できます。

このメソッドは、Android および UWP のカスタム レンダラーから呼び出すことができます。 次のコードは、Android のカスタム レンダラーの例を示しています。

```csharp
public class HingeAngleLabelRenderer : Xamarin.Forms.Platform.Android.FastRenderers.LabelRenderer
{
    System.Timers.Timer _hingeTimer;
    public HingeAngleLabelRenderer(Context context) : base(context)
    {
    }

    async void OnTimerElapsed(object sender, System.Timers.ElapsedEventArgs e)
    {
        if (_hingeTimer == null)
            return;

        _hingeTimer.Stop();
        var hingeAngle = await DualScreenInfo.Current.GetHingeAngleAsync();

        Device.BeginInvokeOnMainThread(() =>
        {
            if (_hingeTimer != null)
                Element.Text = hingeAngle.ToString();
        });

        if (_hingeTimer != null)
            _hingeTimer.Start();
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
    {
        base.OnElementChanged(e);

        if (_hingeTimer == null)
        {
            _hingeTimer = new System.Timers.Timer(100);
            _hingeTimer.Elapsed += OnTimerElapsed;
            _hingeTimer.Start();
        }
    }

    protected override void Dispose(bool disposing)
    {
        if (_hingeTimer != null)
        {
            _hingeTimer.Elapsed -= OnTimerElapsed;
            _hingeTimer.Stop();
            _hingeTimer = null;
        }

        base.Dispose(disposing);
    }
}
```

## <a name="access-dualscreeninfo-in-your-application-window"></a>アプリケーション ウィンドウの DualScreenInfo にアクセスする

次のコードは、アプリケーション ウィンドウの `DualScreenInfo` にアクセスする方法を示しています。

```csharp
DualScreenInfo currentWindow = DualScreenInfo.Current;

// Retrieve absolute position of the hinge on the screen
var hingeBounds = currentWindow.HingeBounds;

// check if app window is spanned across two screens
if(currentWindow.SpanMode == TwoPaneViewMode.SinglePane)
{
    // window is only on one screen
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Tall)
{
    // window is spanned across two screens and oriented top-bottom
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented side-by-side
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-layouts"></a>レイアウトに DualScreenInfo を適用する

`DualScreenInfo` クラスには、レイアウトを取得し、デバイスを基準としたレイアウトに関する情報を 2 つの画面に表示するコンストラクターがあります。

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}" />
        <ColumnDefinition Width="{Binding Column2Width}" />
        <ColumnDefinition Width="{Binding Column3Width}" />
    </Grid.ColumnDefinitions>
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="End"
           Text="I should be on the left side of the hinge" />
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="Start"
           Grid.Column="2"
           Text="I should be on the right side of the hinge" />
</Grid>
```

```csharp
public partial class GridUsingDualScreenInfo : ContentPage
{
    public DualScreenInfo DualScreenInfo { get; }
    public double Column1Width { get; set; }
    public double Column2Width { get; set; }
    public double Column3Width { get; set; }

    public GridUsingDualScreenInfo()
    {
        InitializeComponent();
        DualScreenInfo = new DualScreenInfo(grid);
        BindingContext = this;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        DualScreenInfo.PropertyChanged += OnInfoPropertyChanged;
        UpdateColumns();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        DualScreenInfo.PropertyChanged -= OnInfoPropertyChanged;
    }

    void UpdateColumns()
    {
        // Check if grid is on two screens
        if (DualScreenInfo.SpanningBounds.Length > 0)
        {
            // set the width of the first column to the width of the layout
            // that's on the left screen
            Column1Width = DualScreenInfo.SpanningBounds[0].Width;

            // set the middle column to the width of the hinge
            Column2Width = DualScreenInfo.HingeBounds.Width;

            // set the width of the third column to the width of the layout
            // that's on the right screen
            Column3Width = DualScreenInfo.SpanningBounds[1].Width;
        }
        else
        {
            Column1Width = 100;
            Column2Width = 0;
            Column3Width = 100;
        }

        OnPropertyChanged(nameof(Column1Width));
        OnPropertyChanged(nameof(Column2Width));
        OnPropertyChanged(nameof(Column3Width));

    }

    void OnInfoPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
    {
        UpdateColumns();
    }
}
```

次のスクリーンショットは、結果のレイアウトを示しています。

![2 画面へのグリッドの配置](dual-screen-info-images/grid-on-two-screens.png)

## <a name="related-links"></a>関連リンク

- [DualScreen (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)