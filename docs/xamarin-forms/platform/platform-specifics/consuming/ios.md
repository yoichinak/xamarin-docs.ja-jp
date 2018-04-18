---
title: iOS Platform-Specifics
description: Platform-specifics は Custom Renderers や Effects を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では Xamarin.Forms 組み込みの iOS の platform-specifics の使い方を説明します。
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 7826962cd3bf9595a63841e3f2d9fb377d1a0574
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="ios-platform-specifics"></a>iOS Platform-Specifics

_Platform-specifics は Custom Renderers や Effects を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では Xamarin.Forms 組み込みの iOS の platform-specifics の使い方を説明します。_

iOS では、Xamarin.Forms は次のような platform-specific が含まれています。

- [ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)への Blur サポート。 詳細は[ Blur の適用](#blur)を参照。
- ページタイトルをナビゲーションバーの Large Title として表示するかどうかの制御。詳細は[ Large Title の表示](#large_title)を参照。
- ページのコンテンツを全ての iOS デバイスで安全な画面の領域へ配置させる機能。詳細は[ Safe Area Layout Guide の有効化](#safe_area_layout)を参照。
- 半透明のナビゲーションバー。 詳細は[半透明の Navigation Bar の作成](#translucent_navigation_bar)を参照。
- [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)上のステータスバーのテキスト色をナビゲーションバーの明るさに合わせて調整するかどうかの制御。詳細は[ Status Bar の Text Color Mode の調整](#status_bar_color_mode)を参照。
- 入力した文字をフォントサイズを調整することで[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)内に収まるように合わせる機能。詳細は[ Entry の Font Size の調整](#adjust_font_size)を参照。
- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) で項目の選択が発生するタイミングの制御。詳細は[Picker の Item Selection の制御](#picker_update_mode)を参照。
- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) のステータスバーの可視性の設定。詳細は[ページのステータスバーの可視性の設定](#set_status_bar_visibility)を参照。
- [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) がタッチジェスチャを処理するか、自身のコンテンツに伝えるかの制御。 詳細は[ ScrollView 内のタッチ処理の遅延](#delay_content_touches)を参照。

<a name="blur" />

## <a name="applying-blur"></a>Blur の適用

この platform-specific は下に重なったコンテンツをぼかすために使われ、Xamlで、 [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/)添付プロパティに[ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)の列挙型を設定して使用します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
    <Image Source="monkeyface.png" />
    <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>`メソッドは、この platform-specific が iOS でのみ動作することを指定します。[ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間に存在し、[ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)列挙型が適用する4つの値（[ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/)、 [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/)、 [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/)、[ `Dark`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/)）を使って Blur 効果を適用するために使用されます。

結果として、指定された[ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)が[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)のインスタンスに適用され、下に重なっている[ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)をぼかします。

![](ios-images/blur-effect.png "Blur 効果 Platform-Specific")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Large Title の表示

この platform-specific は、iOS11 以上を使うデバイスで、ナビゲーションバー上で Large Title としてページタイトルを表示するために使われます。Large Title は左寄せで大きいフォントが使われます。そして画面の領域を効率的に使うために、ユーザーがコンテンツをスクロールした時に、標準のタイトルに移り変わります。しかし横向きの画面では、コンテンツのレイアウトを最適化するためにタイトルはナビゲーションバーの中央に戻ります。この機能は XAML で `NavigationPage.PrefersLargeTitles` 添付プロパティに `boolean` 値を設定して使用します。

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

または fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 `NavigationPage.SetPrefersLargeTitle` メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間に存在し、Large Title を有効にするかどうかを制御します。

[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) で Large Title が有効になると、ナビゲーションスタックにある全てのページは Large Title で表示されるようになります。この動作は個々のページ上で `Page.LargeTitleDisplay` 添付プロパティに `LargeTitleDisplayMode` 列挙型の値を指定することで上書きできます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

または、fluent API を使用して、c# からページの動作を上書きできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 `Page.SetLargeTitleDisplay` メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 名前空間に存在し、[ `Page`] (https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 上の Large Title の動作を  `LargeTitleDisplayMode` 列挙型が提供する3つ値を使って制御します。

- `Always` – 強制的にナビゲーションバーとフォントサイズに大きいフォーマットを使う
- `Automatic` – ナビゲーションスタックの前の要素と同じスタイル（ large か small ）を使う
- `Never` – 強制的に標準の小さいフォーマットのナビゲーションバーを使う 

さらに、`SetLargeTitleDisplay` メソッドは、現在の `LargeTitleDisplayMode` を返す `LargeTitleDisplay` メソッドを呼ぶことで、列挙値を切り替えて使うことができます。

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

その結果、指定した `LargeTitleDisplayMode` が [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) に適用され、 Large Title の動作を制御します。

![](ios-images/large-title.png "Blur Effect Platform-Specific")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Safe Area Layout Guide の有効化

この platform-specific は iOS11 以上を使う全てのデバイスでページコンテンツを安全な画面の領域に配置させるために使います。特に、iPhone X でデバイスの角丸やホームインジケータやセンサーハウジングによってコンテンツが切り抜かれないようにすることを保証します。これは XAML で `Page.UseSafeArea` 添付プロパティに `boolean` 値を設定して使用します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>`メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 `Page.SetUseSafeArea` メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 名前空間に存在し、Safe Area Layout Guide を有効にするかどうかを制御します。

その結果、ページコンテンツを全ての iPhone で画面の安全な領域に配置することができます。

[![](ios-images/safe-area-layout.png "Safe Area Layout Guide")](ios-images/safe-area-layout-large.png#lightbox "Safe Area Layout Guide")

> [!NOTE]
> Apple によって定義された Safe Area は、Xamarin.Forms では[ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/)プロパティを設定することで使用されます。既に設定されているこのプロパティの以前の全ての値は上書きされます。

Safe Area は[ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間の `Page.SafeAreaInsets` メソッドを使って、その [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)の値を変更することでカスタマイズが可能です。取得した値を必要に応じて、ページのコンストラクタや[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/)のオーバーライドで、 `Padding` プロパティに再割り当てすることで変更が可能です。

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>半透明の Navigation Bar の作成

この platform-specific はナビゲーションバーの透明度を変更するために使われます。これは XAML で [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/)添付プロパティに `boolean` 値を設定して使用します。

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` メソッドはこの platform-specific が iOS上 でのみ動作することを指定します。 [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/)メソッドは、[ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間に存在し、ナビゲーションバーを透過させるために使用されます。さらに、 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間の [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/)クラスには、ナビゲーションバーをデフォルトの状態に戻す[ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/)メソッドや[ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/)メソッドを呼ぶことで、ナビゲーションバーの透明度を切り替えることに使用できる[ `SetIsNavigationBarTranslucent`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) メソッドもあります。

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

その結果、ナビゲーション バーの透明度を変更することができます。

![](ios-images/translucent-navigation-bar.png "Translucent Navigation Bar Platform-Specific")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Status Bar の Text Color Mode の調整

この platform-specific は、[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) のステータスバーのテキスト色をナビゲーションバーの明るさに合うように調整するかどうかを制御します。。これは、 XAML で[ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/)添付プロパティに[ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)列挙型の値を指定して使用します。

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 名前空間に存在し、[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) のステータスバーのテキスト色をナビゲーションバーの明るさに合うように調整するかどうかを、[ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)列挙型が提供する2つの値を使って制御します。

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) – ステータスバーのテキスト色を調整しないことを示します。
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) – ステータスバーのテキスト色をナビゲーションバーの明るさに合わせることを示します。

さらに、 [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/)メソッドは[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)に適用された現在の[ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)列挙型の値を取得するために使用できます。

その結果、[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)のステータスバーのテキスト色はナビゲーションバーの明るさに合わせて調整されます。この例では、ステータスバーのテキスト色は [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) の [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)と[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)をユーザが切り替えたときに変化します。

![](ios-images/status-bar-text-color-mode.png "Status Bar Text Color Mode Platform-Specific")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Entry の Font Size の調整

この platform-specific は、[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) のフォントサイズを入力されたテキストがコントロール内に収まるように拡大縮小するために使用されます。これは XAML で [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) 添付プロパティに `boolean` 値を指定して使用します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 名前空間に存在し、入力テキストのフォントサイズを [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) に収まるようにに拡大縮小するために使用します。さらに、 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間の [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) クラスには、この platform-specific を無効にする [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) メソッドや、[ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) メソッドを呼ぶことでフォントサイズの拡大縮小を切り替えることに使える [ `SetAdjustsFontSizeToFitWidth`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) メソッドもあります。

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

その結果、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) のフォントサイズは入力テキストがコントロール内に収まるように拡大縮小されます。

![](ios-images/entry-font-size.png "Entry の Font Size の調整 Platform-Specific")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Picker の Item Selection の制御

この platform-specific は、[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) での item selection の発生するタイミングを制御し、ユーザーが Picker 内でアイテムを動かしている時に item selection を発生させるか、 **完了** ボタンを押した時に1度だけ発生させるかを指定できます。これは、 XAML で `Picker.UpdateMode` 添付プロパティに `UpdateMode` 列挙型の値を設定して使用します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 `Picker.SetUpdateMode` メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 名前空間に存在し、 `UpdateMode` 列挙型が提供する2つの値を使って、 item selection が発生するタイミングを制御するために使用されます。

- `Immediately` – [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) でユーザーがアイテムを動かしているときにitem selectionが発生します。これは Xamarin.Forms の既定の動作です。
- `WhenFinished` – [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) でユーザーが**完了**ボタンを押した時に1回だけ item seletion が発生します。

さらに、`SetUpdateMode` メソッドは、現在の `UpdateMode` を返す `UpdateMode` メソッドを呼んで列挙型の値を切り替えることに使用できます。

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

その結果、 [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) に指定された `UpdateMode` が適用され、item selection が発生するタイミングを制御します。

[![](ios-images/picker-updatemode.png "Picker UpdateMode Platform-Specific")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>ページのステータスバーの可視性の設定

この platform-specific は、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) のステータスバーの可視性を設定するために使用されます。これにはステータスバーがどのように `Page` に出入りするのかを制御する機能を含みます。これは、 XAML で `Page.PrefersStatusBarHidden` 添付プロパティに `StatusBarHiddenMode` 列挙型の値を設定して使用します。そして任意で `Page.PreferredStatusBarUpdateAnimation` 添付プロパティに `UIStatusBarAnimation` 列挙型の値を設定します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 `Page.SetPrefersStatusBarHidden` メソッドは、`Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間に存在し、`StatusBarHiddenMode` 列挙型の値（ `Default` / `True` / `False` ）の1つを指定することで [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 上のステータスバーの可視性を設定するために使用されます。 `StatusBarHiddenMode.True` と `StatusBarHiddenMode.False` は、デバイスの方向に関係なくステータスバーの可視性を設定し、`StatusBarHiddenMode.Default` は縦向きの小さな環境でステータス バーを非表示にします。

その結果、[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) のステータスバーの可視性を設定することができます。

![](ios-images/hide-status-bar.png "Status Bar Visibility Platform-Specific")

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) では、`StatusBarHiddenMode` 列挙型の値が設定されると、全ての子ページのステータスバーも更新します。 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) から派生するすべての型では、 `StatusBarHiddenMode` 列挙型の値が設定されると、現在のページのステータスバーだけを更新します。

`Page.SetPreferredStatusBarUpdateAnimation` メソッドは、ステータスバーがどのようにして [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)に出入りするのかを `UIStatusBarAnimation` 列挙型の値（ `None` / `Fade` / `Slide` ）の1つを指定して使用します。 `Fade` か `Slide` を指定すると、ステータスバーが `Page` を出入りする際に0.25秒のアニメーションが実行されます。

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>ScrollView 内のタッチ処理の遅延

iOSでは、[ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 内でタッチジェスチャーが開始される時、暗黙のタイマーが呼ばれます。そして `ScrollView` は、そのタイマーの間のユーザーのアクションに基づいて、そのジェスチャーを処理するか、その中のコンテンツに伝えるかを決めます。既定では iOS の `ScrollView` はコンテンツのタッチを遅らせますが、これは `ScrollView` のコンテンツでジェスチャーが発生すべきときに発生しない、というようないくつかの状況での問題の原因となりえます。したがって、この platform-specific は `ScrollView` がタッチジェスチャーを処理するか、その中のコンテンツに伝えるのかどうかを制御します。これは、 XAML で `ScrollView.ShouldDelayContentTouches` 添付プロパティに `boolean` 値を設定して使用します。

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` メソッドは、この platform-specific が iOS上 でのみ動作することを指定します。 `ScrollView.SetShouldDelayContentTouches` メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 名前空間に存在し、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) がタッチジェスチャーを処理するか、その中のコンテンツに伝えるのかどうかを制御するために使用されます。さらに、 `SetShouldDelayContentTouches` メソッドは、コンテンツのタッチを遅らせるかどうかを返す `ShouldDelayContentTouches` メソッドを呼ぶことで、コンテンツのタッチの遅延を切り替えることにも使用できます。

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

その結果は、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) はコンテンツのタッチを受けるときの遅延は無効にできます。そのためこのシナリオでは、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) の [`Detail`](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) ではなく[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) がジェスチャーを受け取ります。

[![](ios-images/scrollview-delay-content-touches.png "ScrollView Delay Content Touches Plaform-Specific")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>まとめ

この記事は、Xamarin.Forms に組み込みの iOS の platform-specifics の使用方法を説明しました。 platform-specifics は、effects や custom renderers を実装することなく、特定のプラットフォームでだけ利用できる機能を使用できます。


## <a name="related-links"></a>関連リンク

- [PlatformSpecifics の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
