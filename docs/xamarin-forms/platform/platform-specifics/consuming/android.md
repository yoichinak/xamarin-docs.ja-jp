---
title: "Android プラットフォーム仕様"
description: "プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。"
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

この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

Android では、Xamarin.Forms は次のようなプラットフォーム仕様が含まれています。

- ソフトキーボードの操作モードの設定。詳細については、「ソフトキーボードの入力モードの設定(#soft_input_mode)」を参照してください。
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)での高速スクロールの有効化。詳細については、「[ListView での高速スクロールの有効化(#fastscroll)」を参照してください。
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)でのページ間のスワイプ操作の有効化。詳細については、「TabbedPage でのページ間のスワイプ操作の有効化(#enable_swipe_paging)」を参照してください。
- 描画の順番を決定するための視覚要素の Z オーダーの制御。詳細については、「視覚要素の昇格の制御(#elevation)」を参照してください。
- AppCompat を使用するアプリケーションに対し、一時停止や再開時に [`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)や[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) といったぺージのライフサイクルのイベントをそれぞれ無効化する。詳細については、「Disappearing と Appearing のぺージライフサイクルイベントの無効化(#disable_lifecycle_events)」を参照してください。

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

`Application.On<Android>`メソッドは Android 上でのみ動作することを指定します。[`Application.UseWindowSoftInputModeAdjust`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、[`WindowSoftInputModeAdjust`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/)列挙型が提供する2つの値（[`Pan`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) / [`Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)）を使って、ソフトキーボードの入力エリアの操作方式を設定するために使用します。`Pan`は[`AdjustPan`](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/)調整オプションを使用しますが、それは入力コントローラがフォーカスを持つ時にウィンドウをリサイズしません。その代わり、ウィンドウのコンテンツはソフトキーボードによって現在のフォーカスが隠れないように移動します。`Resize`は[`AdjustResize`](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/)調整オプションを使用し、入力コントロールがフォーカスを持つ時にソフトキーボードのスペースを空けるためにリサイズします。

その結果、入力コントロールがフォーカスを持つ時のソフトキーボードの入力エリアの操作方式を設定することができます。

[![](android-images/pan-resize.png "ソフトキーボード操作モードのプラットフォーム仕様")](android-images/pan-resize-large.png#lightbox "ソフトキーボード操作モードのプラットフォーム仕様")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>ListView での高速スクロールの有効化

このプラットフォーム仕様は[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)のデータ間での高速なスクロールを有効にするために使われます。これはXAMLで`ListView.IsFastScrollEnabled`添付プロパティに`boolean`値を設定することで使用します。

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

`ListView.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。`ListView.SetIsFastScrollEnabled`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)のデータ間で高速なスクロールを可能にするために使われます。さらに、`SetIsFastScrollEnabled`メソッドは高速スクロールが有効かどうかを返すために `IsFastScrollEnabled`メソッドを呼ぶことで高速スクロールを切り替えるために使用することができます。

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

その結果、[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)のデータ間での高速スクロールが有効になり、スクロールのつまみのサイズが変更されます。

[![](android-images/fastscroll.png "ListView 高速スクロールのプラットフォーム仕様")](android-images/fastscroll-large.png#lightbox "ListView 高速スクロールのプラットフォーム仕様")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>TabbedPage でのページ間のスワイプ操作の有効化

このプラットフォーム仕様は[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)のぺージ間の水平ジェスチャーによるスワイプ移動を有効にするために使われます。これは XAML で[`TabbedPage.IsSwipePagingEnabled`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/)添付プロパティに`boolean` 値を設定することで使用されます。

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

`TabbedPage.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。[`TabbedPage.SetIsSwipePagingEnabled`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/)メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)のぺージ間のスワイプ移動を有効にするために使われます。`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`名前空間の`TabbedPage`クラスには、このプラットフォーム仕様を有効にする[`EnableSwipePaging`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/)メソッドと無効にする[`DisableSwipePaging`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/)メソッドもあります。[`TabbedPage.OffscreenPageLimit`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/)添付プロパティと[`SetOffscreenPageLimit`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/)メソッドは現在のぺージの両側でアイドル状態のままで保持すべきぺージ数を設定するために使用します。

その結果、[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)によって表示されたぺージ間のスワイプ遷移が有効になります。

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>視覚要素の昇格の制御

このプラットフォーム仕様は、ターゲットが API 21 以上のアプリケーションで視覚要素の昇格または Z オーダーを制御するために使われます。視覚要素の昇格は、高い Z の値を持つ視覚要素が低い Z の値をもつ視覚要素を塞ぐように、自身の描画順を決定します。これは XAML で `Elevation.Elevation` 添付プロパティに`boolean`値を設定して使用します。

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

`Button.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。`Elevation.SetElevation`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)名前空間に存在し、視覚要素の昇格に null 許容型の `float` 値を設定するために使われます。さらに、`Elevation.GetElevation`メソッドは、視覚要素の昇格値を取得するために使用できます。

その結果、視覚要素の昇格は、高いZ値の視覚要素が低いZ値の視覚要素を塞ぐように制御されます。それによって、この例では2番目の[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)は、より高い昇格値を持つため、[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)の上に表示されます。

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Disappearing と Appearing のぺージライフサイクルイベントの無効化


このプラットフォーム仕様は、AppCompat を使ったアプリケーションで、アプリケーションの一時停止と再開の[`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)のぺージイベントをそれぞれ無効にするために使用されます。さらに、これには一時停止時にソフトキーボードの操作方式に[`WindowSoftInputModeAdjust.Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)が設定されていて、ソフトキーボードが表示されていた場合に、再開時にソフトキーボードを表示するかどうかを制御する機能も含まれます。

> [!NOTE]
> これらのイベントは、そのイベントに依存するアプリケーションの既存の動作を保持するためにデフォルトで有効であることに注意してください。これらのイベントを無効にするとAppCompatのイベントサイクルを以前のAppCompatのイベントサイクルに合わせます。

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

`Application.Current.On<Android>`メソッドはこのプラットフォーム仕様が Android上でのみ動作することを指定します。[`Application.SendDisappearingEventOnPause`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)名前空間に存在し、アプリケーションがバックグラウンドに切り替わった時に[`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ぺージイベントの発生を有効化または無効化するために使用されます。[`Application.SendAppearingEventOnResume`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドは、アプリケーションがバックグラウンドから復帰した時に[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ページイベントの発生を有効化または無効化するために使用されます。[`Application.ShouldPreserveKeyboardOnResume`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/)メソッドは、一時停止時にソフトキーボードの操作方式に[`WindowSoftInputModeAdjust.Resize`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)が設定されていて、それが表示されている場合に、再開時にそれを表示するかどうかを制御するために使用されます。

その結果、[`Disappearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)と[`Appearing`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/)ぺージイベントはそれぞれアプリケーションの一時停止時と再開時に発生しなくなり、アプリケーションの一時停止時にソフトキーボードが表示されていた場合、再開時にもそれが表示されるようになります。

[![](android-images/keyboard-on-resume.png "ライフサイクル イベントのプラットフォーム仕様")](android-images/keyboard-on-resume-large.png#lightbox "ライフサイクル イベントのプラットフォーム仕様")

## <a name="summary"></a>まとめ

この記事では Xamarin.Forms に組み込まれている Android のプラットフォーム仕様の使い方を説明します。プラットフォーム仕様は、カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。


## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
