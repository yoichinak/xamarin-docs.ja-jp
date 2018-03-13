---
title: "Android プラットフォーム仕様"
description: "プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。 この記事では、Android プラットフォームの具体的な Xamarin.Forms に組み込まれているを使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: 739ee4ebeb3176d23ab1eb911baaab31a26252c4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-specifics"></a>Android プラットフォーム仕様

_プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。この記事では、Android プラットフォームの具体的な Xamarin.Forms に組み込まれているを使用する方法を示します。_

Android では、Xamarin.Forms には、次のプラットフォームの設定が含まれています。

- ソフト キーボードの動作モードを設定します。 詳細については、次を参照してください。[ソフト キーボード入力モードを設定](#soft_input_mode)です。
- 高速のスクロールを有効にする、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)詳細については、次を参照してください。 [ListView で高速スクロールを有効にすると](#fastscroll)です。
- 内のページ間の読み取りを有効にする、 [ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)です。 詳細については、次を参照してください。[を有効にするスワイプ間のページ、TabbedPage](#enable_swipe_paging)です。
- Z オーダーの視覚要素の描画順序を決定するを制御します。 詳細については、次を参照してください。[視覚要素の昇格を制御する](#elevation)です。
- 無効にすると、 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)一時停止のライフ サイクル イベントをページおよび AppCompat を使用するアプリケーションのそれぞれを再開します。 詳細については、次を参照してください。 [: 消えるおよびページ ライフ サイクル イベントの表示を無効にする](#disable_lifecycle_events)です。

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>ソフト キーボード入力モードを設定

このプラットフォームに固有のソフト キーボード入力領域の動作モードの設定に使用され、設定によって、XAML で使用される、 [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/)添付プロパティの値を[ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)列挙体。

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>`メソッドは、Android でのこのプラットフォームに固有の実行はのみを指定します。 [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)でソフト キーボード入力領域の動作モードを設定する、名前空間が使用される、 [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)2 つの値を指定する列挙体: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/)と[ `Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)です。 `Pan`値の使用、 [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/)調整オプションは、入力コントロールにフォーカスがある場合、ウィンドウのサイズを変更しません。 代わりに、ウィンドウの内容は方向のパンのソフト キーボードを使用して、現在のフォーカスされていない隠されるようにします。 `Resize`値の使用、 [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/)調整オプションは、入力コントロールに、ソフト キーボードを確保するために、フォーカスがある場合、ウィンドウのサイズを変更します。

結果は、ソフト キーボードが入力コントロールにフォーカスがある場合、動作モードを設定できる領域を入力します。

[![](android-images/pan-resize.png "ソフト キーボードがモードのプラットフォームに固有の動作")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>ListView で高速のスクロールを有効にします。

このプラットフォームに固有の使用内のデータを高速のスクロールを有効にする[ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)です。 使用される XAML で設定して、`ListView.IsFastScrollEnabled`添付プロパティを`boolean`値。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>`メソッドは、Android でのこのプラットフォームに固有の実行はのみを指定します。 `ListView.SetIsFastScrollEnabled`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)内のデータを高速のスクロールを有効にする、名前空間が使用される、 [ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)です。 さらに、`SetIsFastScrollEnabled`メソッドを呼び出すことによって高速のスクロールを切り替えを使用することができます、`IsFastScrollEnabled`を高速スクロールが有効になっているかどうかを返すメソッド。

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

結果は、スクロールするには高速にデータを[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を有効にする、スクロールのスクロール ボックスのサイズを変更します。

[![](android-images/fastscroll.png "ListView FastScroll プラットフォーム固有")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>スワイプ、TabbedPage 内のページ間の有効化

このプラットフォームに固有の使用でページ間の水平方向の指ジェスチャにスワイプを有効にする[ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)です。 使用される XAML で設定して、 [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/)添付プロパティを`boolean`値。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`メソッドは、Android でのこのプラットフォームに固有の実行はのみを指定します。 [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間を使用して、内のページ間の読み取りを有効にする[ `TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)です。 さらに、`TabbedPage`クラス内で、`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`名前空間にはまた、 [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/)をこのプラットフォームに固有で有効にするメソッドと[ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/)を無効にする方法このプラットフォームに固有です。 [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/)添付プロパティ、および[ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/)メソッドを使用する現在のページのどちら側にアイドル状態で保持するページの数を設定します。

結果は、その方向にスワイプするページングによって表示されるページを[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)を有効にします。

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>ビジュアル要素の昇格を制御します。

このプラットフォームに固有とは、昇格、またはアプリケーションのビジュアル要素の Z オーダーをターゲットとする API 21 を制御するために使用以上です。 視覚的要素の昇格では、Z 値が大きい他 Z 値が低いビジュアル要素を視覚的要素の描画順序を決定します。 使用される XAML で設定して、`Elevation.Elevation`添付プロパティを`boolean`値。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:Elevation.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>`メソッドは、Android でのこのプラットフォームに固有の実行はのみを指定します。 `Elevation.SetElevation`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間が null 値を許容するビジュアル要素の昇格を設定するため`float`です。 さらに、`Elevation.GetElevation`視覚的要素の昇格の値を取得するメソッドを使用できます。

結果は、Z 値を大きく視覚要素が Z 値が低いビジュアル要素を視覚的要素の昇格を制御できます。 この例ではそのため、2 番目[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)上に描画、 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)度の高い値があるため。

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>消えたと表示されているページのライフ サイクル イベントを無効にします。

このプラットフォームに固有の使用を無効に、 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)アプリケーションのページのイベントが一時停止および AppCompat を使用するアプリケーションのそれぞれを再開します。 ソフト キーボードが再開に表示されることが表示された場合、一時停止でソフト キーボードの動作モードに設定されているかどうかを制御する機能を掲載してさらに、 [ `WindowSoftInputModeAdjust.Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)です。

> [!NOTE]
> これらのイベントがイベントに依存するアプリケーションの既存の動作を保持するために既定で有効にことに注意してください。 プレ AppCompat イベント サイクルに合わせて AppCompat イベント サイクルは、これらのイベントを無効にします。

XAML で設定して使用できるこのプラットフォームに固有の[ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/)、 [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/)、および[ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) にアタッチされるプロパティ`boolean`値。

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>`メソッドは、Android でのこのプラットフォームに固有の実行はのみを指定します。 [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)有効にするにまたは起動処理を無効にする、名前空間が使用される、 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ページ イベントときに、アプリケーションバック グラウンドを入力します。 [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドを使用を有効にするにまたは起動処理を無効にする、 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ページ イベント、アプリケーションがバック グラウンドから再開するとします。 [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドを使用して制御で一時停止、表示された場合、再開にソフト キーボードが表示されるかどうかは、ソフト キーボードの動作モードに設定されている提供[ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

結果は、 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ソフト キーボードがの場合はそのページ イベントがアプリケーションの一時停止されません起動され、それぞれ、再開、ときに表示されるアプリケーション一時停止は表示することも、アプリケーションが再開されるとします。

[![](android-images/keyboard-on-resume.png "ライフ サイクル イベント プラットフォーム固有")](android-images/keyboard-on-resume-large.png#lightbox "イベントのプラットフォームに固有のライフ サイクル")

## <a name="summary"></a>まとめ

この記事では、Android プラットフォームの具体的な Xamarin.Forms に組み込まれているを使用する方法を示しました。 プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。


## <a name="related-links"></a>関連リンク

- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
