---
title: Android プラットフォーム仕様
description: この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。 プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: 8d7ec3f2f64fdb8be903fd13bd72bcf545265a3d
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546087"
---
# <a name="android-platform-specifics"></a>Android プラットフォーム仕様

_この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。_

Android では、Xamarin.Forms には、次のプラットフォームの設定が含まれています。

- ソフトキーボードの操作モードの設定。 詳細については、[ソフトキーボードの入力モードの設定](#soft_input_mode)を参照してください。
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)での高速スクロールの有効化。詳細については、[ListView での高速スクロールの有効化(#fastscroll)](#fastscroll)を参照してください。
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)でのページ間のスワイプ操作の有効化。 詳細については、[TabbedPage でのページ間のスワイプ操作の有効化](#enable_swipe_paging)を参照してください。
- 描画の順番を決定するための視覚要素の Z オーダーの制御。 詳細については、[視覚要素の昇格の制御(#elevation)](#elevation)を参照してください。
- AppCompat を使用するアプリケーションに対し、一時停止や再開時に [`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)や[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) といったぺージのライフサイクルのイベントをそれぞれ無効化する。 詳細については、[Disappearing と Appearing のぺージライフサイクルイベントの無効化](#disable_lifecycle_events)」を参照してください。
- 制御するかどうか、 [ `WebView` ](xref:Xamarin.Forms.WebView)混合コンテンツを表示できます。 詳細については、次を参照してください。[混在コンテンツ WebView に有効にすると](#webview-mixed-content)です。
- 入力方式のソフト キーボード用のエディター オプションの設定、 [ `Entry`](xref:Xamarin.Forms.Entry)です。 詳細については、次を参照してください。[設定エントリ入力方式エディター オプション](#entry-imeoptions)です。

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>ソフトキーボードの入力モードの設定

このプラットフォーム仕様はソフトキーボードの入力エリアのための操作方式を設定するために使われ、Xaml で[`Application.WindowSoftInputModeAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/)添付プロパティを[`WindowSoftInputModeAdjust`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)列挙型の値に設定して使用します。

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

`Application.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 [`Application.UseWindowSoftInputModeAdjust`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、[`WindowSoftInputModeAdjust`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)列挙型が提供する2つの値（[`Pan`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) /[`Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)）を使って、ソフトキーボードの入力エリアの操作方式を設定するために使用します。 `Pan`は[`AdjustPan`](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/)調整オプションを使用しますが、それは入力コントローラがフォーカスを持つ時にウィンドウをリサイズしません。 その代わり、ウィンドウのコンテンツはソフトキーボードによって現在のフォーカスが隠れないように移動します。 `Resize`は[`AdjustResize`](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/)調整オプションを使用し、入力コントロールがフォーカスを持つ時にソフトキーボードのスペースを空けるためにリサイズします。

その結果、入力コントロールがフォーカスを持つ時のソフトキーボードの入力エリアの操作方式を設定することができます。

[![](android-images/pan-resize.png "ソフトキーボード操作モードのプラットフォーム仕様")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>ListView での高速スクロールの有効化

このプラットフォーム仕様は[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)のデータ間での高速なスクロールを有効にするために使われます。 これはXAMLで`ListView.IsFastScrollEnabled`添付プロパティに`boolean`値を設定することで使用します。

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

`ListView.On<Android>`メソッドはこのプラットフォーム仕様が Android上でのみ動作することを指定します。 `ListView.SetIsFastScrollEnabled`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、アプリケーションがバックグラウンドに切り替わった時に[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)ぺージイベントの発生を有効化または無効化するために使用されます。 `SetIsFastScrollEnabled`メソッドは、アプリケーションがバックグラウンドから復帰した時に`IsFastScrollEnabled`ページイベントの発生を有効化または無効化するために使用されます。

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

メソッドは、一時停止時にソフトキーボードの操作方式に[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)が設定されていて、それが表示されている場合に、再開時にそれを表示するかどうかを制御するために使用されます。

[![](android-images/fastscroll.png "ListView 高速スクロールのプラットフォーム仕様")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>TabbedPage でのページ間のスワイプ操作の有効化

このプラットフォーム仕様は[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)のぺージ間の水平ジェスチャーによるスワイプ移動を有効にするために使われます。 これは XAML で[`TabbedPage.IsSwipePagingEnabled`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/)添付プロパティに`boolean` 値を設定することで使用されます。

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

`TabbedPage.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [`TabbedPage.SetIsSwipePagingEnabled`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/)メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)のぺージ間のスワイプ移動を有効にするために使われます。 `TabbedPage`名前空間の`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`クラスには、このプラットフォーム仕様を有効にする[`EnableSwipePaging`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/)メソッドと無効にする[`DisableSwipePaging`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/)メソッドもあります。 [`TabbedPage.OffscreenPageLimit`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/)添付プロパティと[`SetOffscreenPageLimit`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/)メソッドは現在のぺージの両側でアイドル状態のままで保持すべきぺージ数を設定するために使用します。

その結果、[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)によって表示されたぺージ間のスワイプ遷移が有効になります。

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>視覚要素の昇格の制御

このプラットフォーム仕様は、ターゲットが API 21 以上のアプリケーションで視覚要素の昇格または Z オーダーを制御するために使われます。 視覚要素の昇格は、高い Z の値を持つ視覚要素が低い Z の値をもつ視覚要素を塞ぐように、自身の描画順を決定します。 この機能は XAML で `VisualElement.Elevation` 添付プロパティを `boolean` 値に設定して使用します。

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
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
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

`Button.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 `VisualElement.SetElevation`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、視覚要素の昇格に null 許容型の `float` 値を設定するために使われます。 さらに、`VisualElement.GetElevation`メソッドは、視覚要素の昇格値を取得するために使用できます。

その結果、視覚要素の昇格は、高いZ値の視覚要素が低いZ値の視覚要素を塞ぐように制御されます。 それによって、この例では2番目の[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)は、より高い昇格値を持つため、[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)の上に表示されます。

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Disappearing と Appearing のぺージライフサイクルイベントの無効化

このプラットフォーム仕様は、AppCompat を使ったアプリケーションで、アプリケーションの一時停止と再開の[`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)のぺージイベントをそれぞれ無効にするために使用されます。 さらに、これには一時停止時にソフトキーボードの操作方式に[`WindowSoftInputModeAdjust.Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)が設定されていて、ソフトキーボードが表示されていた場合に、再開時にソフトキーボードを表示するかどうかを制御する機能も含まれます。

> [!NOTE]
> これらのイベントは、そのイベントに依存するアプリケーションの既存の動作を保持するためにデフォルトで有効であることに注意してください。 これらのイベントを無効にするとAppCompatのイベントサイクルを以前のAppCompatのイベントサイクルに合わせます。

このプラットフォーム仕様はXamlで[`Application.SendDisappearingEventOnPause`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/)、[`Application.SendAppearingEventOnResume`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/)、[`Application.ShouldPreserveKeyboardOnResume`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/)添付プロパティに`boolean`値を設定して使用します。

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

`Application.Current.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)有効にするにまたは起動処理を無効にする、名前空間が使用される、 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ページ イベントときに、アプリケーションバック グラウンドを入力します。 [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドを使用を有効にするにまたは起動処理を無効にする、 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ページ イベント、アプリケーションがバック グラウンドから再開するとします。 [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドを使用して制御で一時停止、表示された場合、再開にソフト キーボードが表示されるかどうかは、ソフト キーボードの動作モードに設定されている提供[ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

その結果、[`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ぺージイベントはそれぞれアプリケーションの一時停止時と再開時に発生しなくなり、アプリケーションの一時停止時にソフトキーボードが表示されていた場合、再開時にもそれが表示されるようになります。

[![](android-images/keyboard-on-resume.png "ライフサイクル イベントのプラットフォーム仕様")](android-images/keyboard-on-resume-large.png#lightbox "ライフサイクル イベントのプラットフォーム仕様")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>WebView に混在したコンテンツの有効化

このプラットフォームに固有のコントロールかどうか、 [ `WebView` ](xref:Xamarin.Forms.WebView)できます混在した表示アプリケーションでコンテンツを対象とする API 21 以上です。 混在したコンテンツは、HTTP 接続を介して (画像、オーディオ、ビデオ、スタイル シート、スクリプト) などのリソースを読み込みます HTTPS 接続経由で最初に読み込まれるコンテンツです。 これは、 XAML で[`WebView.MixedContentMode`](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty)添付プロパティを[`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間を使用して、混合コンテンツを表示できるかどうかと、 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)3 つの値を提供する列挙。

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTP 発生元からコンテンツを読み込む HTTPS origin を許可します。
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP 発生元からコンテンツを読み込むにはできません。
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView)アプローチでは最新のデバイスの web ブラウザーの互換性があることを試みます。 HTTPS origin によって読み込まれる一部の HTTP コンテンツを許可することがありますおよびその他の種類のコンテンツはブロックされます。 ブロックまたは許可されているコンテンツの種類は、各オペレーティング システムのリリースで変更できます。

結果は、指定した[ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)値に適用されます、 [ `WebView` ](xref:Xamarin.Forms.WebView)、混合コンテンツを表示できるかどうかを制御します。

[![WebView 混合コンテンツの処理のプラットフォーム固有](android-images/webview-mixedcontent.png "WebView 混合コンテンツの処理のプラットフォーム固有")](android-images/webview-mixedcontent-large.png#lightbox "WebView 混合プラットフォームに固有のコンテンツの処理")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>設定エントリ入力方式エディターのオプション

このプラットフォームに応じた入力方式エディター (IME) に対してオプションを設定のソフト キーボード、 [ `Entry`](xref:Xamarin.Forms.Entry)です。 これは、ソフト キーボード、およびとの対話の下で、ユーザーのアクション ボタンを設定が含まれています。、`Entry`です。 これは、 XAML で[`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty)添付プロパティを[`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)のソフト キーボードの入力方式アクション オプションの設定に名前空間が使用される、 [ `Entry` ](xref:Xamarin.Forms.Entry)、[ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)列挙体の次の値を指定します。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – 特定のアクションのキーは必要なく、され、基になるコントロールが、独自の場合を生成することはできることを示します。
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – アクション キーを作成していない使用可能なことを示します。
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – アクション キーは、操作を実行"go"、文字列のターゲットにユーザーを取得する型指定されたことを示します。
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) –「検索」操作を実行するアクション キー、入力したテキストの検索の結果をユーザーの取得のことを示します。
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – アクション キーが、ターゲットにテキストを提供する"send"操作を実行することを示します。
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – アクション キーがテキストを受け入れる次のフィールドにかかるため、ユーザー、[次へ] の操作を実行することを示します。
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – アクション キーがソフト キーボードを閉じる、「完了」の操作を実行することを示します。
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – アクション キーがテキストを受け入れる前のフィールドにかかるため、ユーザー、「前」の操作を実行することを示します。
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – アクションのオプションを選択するマスクです。
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – するスペリングは、ユーザーから学習でも、ユーザーが以前入力内容に基づく修正を提案を示します。
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI が全画面表示を移動する必要がありますいないことを示します。
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – 抽出されたテキストの UI が表示されませんかを示します。
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – カスタム動作の UI は表示されないことを示します。

結果は、指定した[ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)のソフト キーボードを適用する値、 [ `Entry` ](xref:Xamarin.Forms.Entry)、入力方式エディターのオプションを設定します。

[![エントリは、エディターのプラットフォームに固有のメソッドを入力](android-images/entry-imeoptions.png "エントリ エディターのプラットフォームに固有のメソッドの入力")](android-images/entry-imeoptions-large.png#lightbox "エントリ エディターのプラットフォームに固有のメソッドの入力")

## <a name="summary"></a>まとめ

プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
