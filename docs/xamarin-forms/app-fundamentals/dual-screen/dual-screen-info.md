---
title: Xamarin.Forms の DualScreenInfo
description: このガイドでは、Xamarin.Forms の DualScreenInfo を使用して Surface Duo や Surface Neo などのデュアル画面デバイスのアプリ エクスペリエンスを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: cae704b7d6300a9dc5eb456f0dec8989813c6581
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145616"
---
# <a name="xamarinforms-dualscreeninfo"></a>Xamarin.Forms の DualScreenInfo

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)

_DualScreenInfo を使用して、レイアウトの変更と、デュアル画面との現在のリレーションシップを検出して通知することができます。_

## <a name="properties"></a>プロパティ

- `SpanningBounds` 2 画面にまたがっている場合は、表示可能な各領域の境界を示す 2 つの四角形を返します。 ウィンドウが複数にまたがっていない場合は、空の配列が返されます。
- `HingeBounds` 画面上のヒンジの位置。
- `IsLandscape` はデバイスが横向きであるかどうかを示します。 アプリケーションが複数にまたがっている場合、ネイティブの向きの API では向きが正しく報告されないため、これは便利です。
- `PropertyChanged` はプロパティが変更されたときに発生します。
- `SpanMode` はレイアウトが縦長、横長、または単一のペイン モードのどれかを示します。

## <a name="android-only-property"></a>Android のみのプロパティ

このプロパティは、Android プラットフォーム プロジェクトから DualScreenInfo にアクセスする場合にのみ使用できます。
このプロパティはカスタム レンダラーから使用できます。

- `GetHingeAngleAsync` はデバイスのヒンジの現在の角度を取得します。 シミュレーターを使用する場合は、圧力センサーを変更することで HingeAngle を設定できます。

## <a name="android-custom-renderer-for-polling-hinge-angle"></a>ポーリングのヒンジ角度用の Android カスタム レンダラー

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

## <a name="access-dualscreeninfo-for-application-window"></a>アプリケーション ウィンドウの DualScreenInfo にアクセスする

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
    // window is spanned across two screens and oriented as landscape
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented as portrait
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-your-own-layouts"></a>独自のレイアウトに DualScreenInfo を適用する

DualScreenInfo には、レイアウトを取得し、デバイスを基準としたレイアウトに関する情報を 2 つの画面に表示するコンストラクターがあります。

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}"></ColumnDefinition>
        <ColumnDefinition Width="{Binding Column2Width}"></ColumnDefinition>
        <ColumnDefinition Width="{Binding Column3Width}"></ColumnDefinition>
    </Grid.ColumnDefinitions>

    <Label FontSize="Large" VerticalOptions="Center" HorizontalOptions="End" Text="I should be on the left side of the hinge"></Label>
    <Label FontSize="Large" VerticalOptions="Center" HorizontalOptions="Start" Grid.Column="2" Text="I should be on the right side of the hinge"></Label>
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

![](dual-screen-info-images/grid-on-two-screens.png "Positioning Grid on Two Screens")

## <a name="related-links"></a>関連リンク

- [DualScreen (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)
