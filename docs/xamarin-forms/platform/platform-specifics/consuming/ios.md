---
title: "iOS プラットフォーム仕様"
description: "プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。 この記事では、iOS プラットフォームの具体的な Xamarin.Forms に組み込まれているを使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 798bb2b15534a620acbe76080e171af1a548ac25
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="ios-platform-specifics"></a>iOS プラットフォーム仕様

_プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。この記事では、iOS プラットフォームの具体的な Xamarin.Forms に組み込まれているを使用する方法を示します。_

Ios の場合は、Xamarin.Forms には、次のプラットフォームの設定が含まれています。

- いずれかのサポートをぼかす[ `VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)です。 詳細については、次を参照してください。[ぼかし適用](#blur)です。
- ページのナビゲーション バーで大規模なタイトルとしてページ タイトルが表示されるかどうかを制御します。 詳細については、次を参照してください。[大きいタイトルを表示する](#large_title)です。
- すべての iOS デバイスに対して安全では、画面の領域には、そのページのコンテンツを確保するが配置されています。 詳細については、次を参照してください。 [、安全な領域のレイアウト ガイドを有効にする](#safe_area_layout)です。
- 半透明のナビゲーション バー。 詳細については、次を参照してください。[ナビゲーション バー半透明行う](#translucent_navigation_bar)です。
- 制御する、ステータス バーのテキスト色に対するかどうか、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)ナビゲーション バーの明るさと一致するように調整します。 詳細については、次を参照してください。[ステータス バーのテキスト色のモードを調整する](#status_bar_color_mode)です。
- テキストを入力することを確認に組み込まれて、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)フォント サイズを調整することによってです。 詳細については、次を参照してください。[エントリのフォント サイズを調整する](#adjust_font_size)です。
- 内の項目の選択のタイミングを制御する、 [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)です。 詳細については、次を参照してください。[の選択項目の選択を制御する](#picker_update_mode)です。
- ステータス バーの可視性の設定、 [ `Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)です。 詳細については、次を参照してください。 [、ページにステータス バーの可視性を設定する](#set_status_bar_visibility)です。
- 制御するかどうか、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)タッチ ジェスチャを処理するか、そのコンテンツに渡します。 詳細については、次を参照してください。 [、ScrollView でのコンテンツの調整を遅らせること](#delay_content_touches)です。

<a name="blur" />

## <a name="applying-blur"></a>ぼかし適用します。

このプラットフォームに固有は、その下位にある階層型の内容をぼかす使用し、設定によって、XAML で使用される、 [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/)添付プロパティの値を[ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)列挙します。

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

`BoxView.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)のぼかし効果を適用する名前空間が使用される、 [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)を提供する 4 つの列挙値: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/)、 [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/)、 [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/)、および[ `Dark`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/)です。

結果は、指定した[ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/)に適用される、 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)インスタンス、どのぼかし、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)下にある層します。

![](ios-images/blur-effect.png "ぼかし効果プラットフォーム固有")

<a name="large_title" />

## <a name="displaying-large-titles"></a>大規模なタイトルを表示します。

このプラットフォームに固有の仕様を使用して、ナビゲーション バーで、iOS 11 以上を使用しているデバイスの大規模なタイトルとしてページ タイトルを表示します。 大タイトルは左揃え、大きいフォントを使用し、実際の画面が効率的に使用されるよう、ユーザーが開始、コンテンツがスクロールされる標準のタイトルに遷移します。 ただし、横方向にタイトルはコンテンツのレイアウトを最適化するために、ナビゲーション バーの中央に戻ります。 使用される XAML で設定して、`NavigationPage.PrefersLargeTitles`添付プロパティを`boolean`値。

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

`NavigationPage.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 `NavigationPage.SetPrefersLargeTitle`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間は、大きいタイトルが有効になっているかどうかを制御します。

大きいタイトルは有効になっていれば、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)、大きいタイトルを移動スタック内のすべてのページが表示されます。 設定ページでこの動作をオーバーライドできます、`Page.LargeTitleDisplay`添付プロパティの値を`LargeTitleDisplayMode`列挙します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

または、fluent API を使用して、c# からページの動作をオーバーライドできます。

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

`Page.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 `Page.SetLargeTitleDisplay`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間に大きなタイトル動作を制御する、 [ `Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)で、`LargeTitleDisplayMode`提供する 3 つの可能な列挙型値:

- `Always` – のナビゲーション バーとフォントを強制的に大規模な形式を使用するサイズ。
- `Automatic` – 移動スタック内の前の項目として同じスタイル (大きさにかかわらず) を使用します。
- `Never` -通常、小規模の形式のナビゲーション バーの使用を強制します。

さらに、`SetLargeTitleDisplay`メソッドを呼び出すことで列挙値を切り替えるを使用することができます、`LargeTitleDisplay`現在を返すメソッド`LargeTitleDisplayMode`:

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

結果は、指定した`LargeTitleDisplayMode`に適用される、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)、大規模なタイトルの動作を制御します。

![](ios-images/large-title.png "ぼかし効果プラットフォーム固有")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>安全な領域のレイアウト ガイドを有効にします。

このプラットフォームに固有のページ コンテンツが iOS 11 以降を使用しているすべてのデバイスに対して安全では、画面の領域に配置されていることを確認してください。 を使用します。 具体的には、確認に役立つ角の丸みのあるデバイスやホームのインジケーターでは、iphone X センサー ハウジングでそのコンテンツがクリップはありません。使用される XAML で設定して、`Page.UseSafeArea`添付プロパティを`boolean`値。

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

`Page.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 `Page.SetUseSafeArea`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間が、安全な領域のレイアウト ガイドが有効になっているかどうかを制御します。

結果はすべての Iphone 安全では、画面の領域をページのコンテンツを配置することができます。

[![](ios-images/safe-area-layout.png "安全な領域のレイアウト ガイド")](ios-images/safe-area-layout-large.png#lightbox "セーフ エリア レイアウト ガイド")

> [!NOTE]
> Apple で定義された安全な領域は、Xamarin.Forms で設定に使用される、 [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/)プロパティ、およびそれまでの値のこのプロパティが設定されているよりも優先されます。

取得することによって、安全な領域をカスタマイズすることができます、 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)値と、`Page.SafeAreaInsets`メソッドから、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間。 として変更することができますし、必須であり、再割り当て、`Padding`ページのコンス トラクター内のプロパティまたは[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/)オーバーライドします。

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

## <a name="making-the-navigation-bar-translucent"></a>ナビゲーション バーを半透明にします。

このプラットフォームに固有のナビゲーション バーの透明度を変更するために使用し、設定によって、XAML で使用される、 [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/)添付プロパティを`boolean`値。

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

`NavigationPage.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間を使用して、ナビゲーション バーを半透明です。 さらに、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/)クラス内で、`Xamarin.Forms.PlatformConfiguration.iOSSpecific`名前空間にはまた、 [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/)ナビゲーション バーを既定の状態に復元する方法、および[ `SetIsNavigationBarTranslucent`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/)メソッドを呼び出すことで、ナビゲーション バーの透過性を切り替えるために使用する、 [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/)メソッド。

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

結果は、ナビゲーション バーの透明度を変更することができます。

![](ios-images/translucent-navigation-bar.png "プラットフォームに固有の半透明のナビゲーション バー")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>ステータス バーのテキストの色のモードを調整します。

このプラットフォームに固有のコントロールで、ステータス バーのテキストがカラーかどうか、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)ナビゲーション バーの明るさと一致するように調整します。 使用される XAML で設定して、 [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/)添付プロパティの値を[ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)列挙します。

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

`NavigationPage.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)コントロールの名前空間は、ステータス バーのテキスト色に対するかどうか、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)に合わせて調整されます、ナビゲーション バーの明るさと、 [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)列挙型の 2 つの値を提供します。

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) – ステータス バーのテキストの色を調整いないことを示します。
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) – ステータス バーのテキストの色がナビゲーション バーの明るさを一致することを示します。

さらに、 [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/)の現在の値を取得するメソッドを使用することができます、 [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/)列挙型に適用される、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

結果は、ステータス バーの上のテキストの色、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)ナビゲーション バーの明るさを一致するように調整されることができます。 この例ではユーザーとステータス バーのテキストの色の変更のスイッチ間、 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)と[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)のページ、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "ステータス バーのテキストの色モード プラットフォーム固有")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>エントリのフォント サイズを調整します。

このプラットフォームに固有の使用のフォント サイズを調整する、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)を入力テキストがコントロール内に収まることを確認します。 使用される XAML で設定して、 [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/)添付プロパティを`boolean`値。

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

`Entry.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)に収まることを確認する入力テキストのフォント サイズを調整する、名前空間が使用される、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). さらに、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/)クラス内で、`Xamarin.Forms.PlatformConfiguration.iOSSpecific`名前空間にはまた、 [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/)このプラットフォームに固有で無効にするメソッドと[ `SetAdjustsFontSizeToFitWidth`](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/)メソッドを呼び出すことによってスケーリングのフォント サイズを切り替えるために使用する、 [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/)メソッド。

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

結果は、のフォント サイズを提供する、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)入力テキストがコントロール内に収まることを確認してくださいに合わせてスケーリングされます。

![](ios-images/entry-font-size.png "エントリのフォント サイズのプラットフォームに固有を調整します。")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>選択項目の選択を制御します。

このプラットフォームに固有のコントロールで項目の選択が発生する、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)、ユーザーが項目の選択が、コントロール内の項目を参照するときに、または 1 回だけ発生することを指定できるように、**完了**ボタンが押されました。 使用される XAML で設定して、`Picker.UpdateMode`添付プロパティの値を`UpdateMode`列挙します。

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

`Picker.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 `Picker.SetUpdateMode`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)項目の選択のタイミングを制御する、名前空間が使用されると、`UpdateMode`列挙型の 2 つの値を提供します。

- `Immediately` 項目の選択は、ユーザーが内の項目を参照としてが発生した、 [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)です。 これは、Xamarin.Forms で既定の動作です。
- `WhenFinished` 項目の選択は、ユーザーが押された後にのみ発生、**完了**ボタンをクリックして、 [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)です。

さらに、`SetUpdateMode`メソッドを呼び出すことで列挙値を切り替えるを使用することができます、`UpdateMode`現在を返すメソッド`UpdateMode`:

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

結果は、指定した`UpdateMode`に適用される、 [ `Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)項目の選択が発生したときを制御します。

[![](ios-images/picker-updatemode.png "ピッカー UpdateMode プラットフォーム固有")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>ステータス バーの設定ページの表示

このプラットフォームに固有の使用、ステータス バーの可視性をセットアップする、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)、ステータス バーの入力または出力を制御する機能が含まれていると、`Page`です。 使用される XAML で設定して、`Page.PrefersStatusBarHidden`添付プロパティの値を`StatusBarHiddenMode`列挙型、および必要に応じて、`Page.PreferredStatusBarUpdateAnimation`添付プロパティの値を`UIStatusBarAnimation`列挙。

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

`Page.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 `Page.SetPrefersStatusBarHidden`メソッドで、`Xamarin.Forms.PlatformConfiguration.iOSSpecific`名前空間を使用して、ステータス バーの可視性をセットアップする、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)のいずれかを指定することによって、`StatusBarHiddenMode`列挙値: `Default`、 `True`、または`False`です。 `StatusBarHiddenMode.True`と`StatusBarHiddenMode.False`値は、デバイスの方向に関係なくステータス バーの可視性を設定し、`StatusBarHiddenMode.Default`値が垂直方向にコンパクトな環境のステータス バーを非表示にします。

結果は、のステータス バーの可視性を提供する、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)設定することができます。

![](ios-images/hide-status-bar.png "ステータス バーの可視性プラットフォーム固有")

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)、指定した`StatusBarHiddenMode`列挙値のステータス バーにすべての子ページが更新されます。 その他のすべての[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-派生型、指定した`StatusBarHiddenMode`列挙値は、現在のページにステータス バーにのみ更新されます。

`Page.SetPreferredStatusBarUpdateAnimation`メソッドを使用して、ステータス バーの入力または出力を設定、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)のいずれかを指定することによって、`UIStatusBarAnimation`列挙値: `None`、 `Fade`、または`Slide`です。 場合、`Fade`または`Slide`ステータス バーの入力または出力のように、アニメーションが実行 0.25 の 1 秒間に、列挙値が指定されて、`Page`です。

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>ScrollView に遅延コンテンツ仕上げ

タッチ ジェスチャの開始時に、暗黙的なタイマーがトリガーされる、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) iOS でと`ScrollView`タイマー スパン内でユーザー操作に基づいて、ジェスチャを処理またはその内容に渡す必要があるかどうかが決定します。 します。 既定では、iOS`ScrollView`遅延コンテンツ趣きをもこの問題が発生すると一部の状況で、`ScrollView`コンテンツが必要なときに、ジェスチャを獲得できません。 このプラットフォームに応じたコントロールではそのため、かどうか、`ScrollView`タッチ ジェスチャを処理するか、そのコンテンツに渡します。 使用される XAML で設定して、`ScrollView.ShouldDelayContentTouches`添付プロパティを`boolean`値。

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

`ScrollView.On<iOS>`メソッドは、iOS でのこのプラットフォームに固有の実行はのみを指定します。 `ScrollView.SetShouldDelayContentTouches`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)名前空間を使用しているかどうか、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)タッチ ジェスチャを処理するか、そのコンテンツに渡します。 さらに、`SetShouldDelayContentTouches`メソッドを呼び出すことによって、コンテンツの調整を遅らせることを切り替えを使用することができます、`ShouldDelayContentTouches`コンテンツ仕上げを遅らせるかどうかを返します。

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

結果は、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)遅延コンテンツの調整のため受信を無効にすることができますをこのシナリオでは、 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)ジェスチャの受信ではなく、 [ `Detail`](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)のページ、 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView 遅延コンテンツ接してプラットフォーム固有の仕様")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>まとめ

この記事では、iOS プラットフォーム固有 Xamarin.Forms に組み込まれているを使用する方法が示されています。 プラットフォーム固有では、カスタム レンダラーや特殊効果を実装することがなく、特定のプラットフォームで利用可能なだけの機能を使用できます。


## <a name="related-links"></a>関連リンク

- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
