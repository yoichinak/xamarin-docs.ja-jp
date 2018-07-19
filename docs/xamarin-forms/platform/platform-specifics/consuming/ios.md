---
title: iOS プラットフォーム仕様
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では Xamarin.Forms に組み込まれている iOS のプラットフォーム仕様の使い方を説明します。
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: 68a38fc43cd744e0382f35baa83643a9f0f7e53d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998987"
---
# <a name="ios-platform-specifics"></a>iOS プラットフォーム仕様

_プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。この記事では Xamarin.Forms に組み込まれている iOS のプラットフォーム仕様の使い方を説明します。_

iOS では、Xamarin.Forms に次のプラットフォーム仕様が含まれています。

- [ `VisualElement`](xref:Xamarin.Forms.VisualElement)へに対するぼかしのサポート。 詳細については、「[ぼかしの適用](#blur)」を参照してください。
- ナビゲーションバーでページタイトルを大タイトルとして表示するかどうかを制御します。 詳細については、「[大タイトルの表示](#large_title)」を参照してください。
- すべての iOS デバイスの安全である画面の領域には、そのページの内容の確認が配置されています。 詳細については、「[セーフ エリア レイアウト ガイドの有効化](#safe_area_layout)」を参照してください。
- 透明のナビゲーション バー。 詳細については「[ナビゲーション バーの透明化](#translucent_navigation_bar)」を参照してください。
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)上のステータスバーのテキストの色をナビゲーションバーの明るさに合わせて調整するかどうかを制御します。 詳細については、「[ステータス バーのテキストの色モードの調整](#status_bar_color_mode)」を参照してください。
- フォントサイズを調整することで入力した文字が [`Entry`](xref:Xamarin.Forms.Entry)内に収まるようにします。 詳細については、次を参照してください。[エントリのフォント サイズを調整する](#adjust_font_size)します。
- [`Picker`](xref:Xamarin.Forms.Picker)でアイテムの選択が発生するタイミングを制御します。 詳細については、「[Picker のアイテム選択の制御](#picker_update_mode)」を参照してください。
- [`Page`](xref:Xamarin.Forms.Page) のステータス バーの可視性を設定します。 詳細については、「[Page でのステータスバーの可視性の設定](#set_status_bar_visibility)」を参照してください。
- [`ScrollView`](xref:Xamarin.Forms.ScrollView)がタッチ ジェスチャを処理するか、そのコンテンツに渡すかを制御します。 詳細については、次を参照してください。 [、ScrollView でのコンテンツの仕上げを遅らせる](#delay_content_touches)します。
- 区切り記号のスタイルを設定、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 詳細については、次を参照してください。 [、ListView の区切り記号のスタイルを設定](#listview-separatorstyle)します。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。[レガシ カラー モードを無効にすると](#legacy-color-mode)します。
- ドロップ シャドウを有効にすると、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。[ドロップ シャドウを有効にする](#drop-shadow)します。
- 有効にすると、 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)でスクロールして表示をキャプチャし、スクロール ビューとパン ジェスチャを共有します。 詳細については、次を参照してください。[同時パン ジェスチャ認識を有効にする](#simultaneous-pan-gesture)します。

<a name="blur" />

## <a name="applying-blur"></a>ぼかしの適用

このプラットフォーム仕様は下に重なったコンテンツをぼかすために使われ、Xamlで、 [ `VisualElement.BlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty)添付プロパティを[ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle)の列挙型の値に設定して使用します。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS でのみ動作することを指定します。 [ `VisualElement.UseBlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle))メソッドは [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間に存在し、 [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle)列挙型が適用する4つの値（[ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None)、 [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight)、 [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light)、[ `Dark`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark)）を使ってぼかし効果を適用するために使用されます。


結果として、指定された[ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle)が [ `BoxView` ](xref:Xamarin.Forms.BoxView)のインスタンスに適用され、下に重なっている [ `Image` ](xref:Xamarin.Forms.Image)をぼかします。

![](ios-images/blur-effect.png " \"Blur 効果 Platform-Specific\"")

> [!NOTE]
> ぼかし効果を追加するときに、 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)、タッチ イベントは引き続き受信できますが、`VisualElement`します。

<a name="large_title" />

## <a name="displaying-large-titles"></a>大タイトルの表示

このプラットフォーム仕様は、iOS11 以上を使うデバイスで、ナビゲーションバー上で大タイトルとしてページタイトルを表示するために使われます。 大タイトルは左寄せで大きいフォントが使われます。そして画面の領域を効率的に使うために、ユーザーがコンテンツをスクロールした時に、標準のタイトルに移り変わります。 しかし横向きの画面では、コンテンツのレイアウトを最適化するためにタイトルはナビゲーションバーの中央に戻ります。 この機能は XAML で `NavigationPage.PrefersLargeTitles` 添付プロパティを `boolean` 値に設定して使用します。

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

または、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `NavigationPage.SetPrefersLargeTitle`メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間に存在し、大タイトルを有効にするかどうかを制御します。

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)で大タイトルが有効になると、ナビゲーションスタックにあるすべてのページは大タイトルで表示されるようになります。 この動作は個々のページ上で `Page.LargeTitleDisplay` 添付プロパティを `LargeTitleDisplayMode` 列挙型の値に設定することで上書きできます。


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

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  `Page.SetLargeTitleDisplay` メソッドは、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、[ `Page`](xref:Xamarin.Forms.Page) 上の大タイトルの動作を `LargeTitleDisplayMode`列挙型が提供する次の 3 つ値を使って制御します。

- `Always` – 強制的にナビゲーションバーとフォントサイズに大きいフォーマットを使う。
- `Automatic` – ナビゲーションスタックの前のアイテムと同じスタイル（大または小）を使う
- `Never` – 強制的に標準の小さいフォーマットのナビゲーションバーを使う。

さらに、`SetLargeTitleDisplay` メソッドは、現在の `LargeTitleDisplay` を返す `LargeTitleDisplayMode` メソッドを呼び出すことによって列挙値を切り替えるのに使用することができます。

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

その結果、指定した `LargeTitleDisplayMode` が[`Page`](xref:Xamarin.Forms.Page) に適用され、大タイトルの動作を制御します。

![](ios-images/large-title.png "ぼかし効果のプラットフォーム仕様")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>セーフ エリア レイアウト ガイドの有効化

このプラットフォーム仕様は iOS11 以上を使う全てのデバイスでページコンテンツを安全な画面の領域に配置させるために使います。することを保証します。 特に、iPhone X でデバイスの角の丸まり、ホームインジケータ、またはセンサーハウジングによってコンテンツが切り抜かれないように。これは XAML で `Page.UseSafeArea` 添付プロパティを `boolean` 値に設定して使用します。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  `Page.SetUseSafeArea` メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、セーフ エリア レイアウト ガイドを有効にするかどうかを制御します。

その結果、ページコンテンツをすべての iPhone において、画面の安全な領域に配置することができます。

[![](ios-images/safe-area-layout.png "セーフ エリア レイアウト ガイド")](ios-images/safe-area-layout-large.png#lightbox "セーフ エリア レイアウト ガイド")

> [!NOTE]
> Apple によって定義されたセーフ エリアは、Xamarin.Forms では[`Page.Padding`](xref:Xamarin.Forms.Page.Padding)プロパティを設定するために使用され、このプロパティの設定済みのすべての値は上書きされます。

セーフ エリアは[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `Page.SafeAreaInsets` メソッドを使って、その [`Thickness`](xref:Xamarin.Forms.Thickness)の値を変更することでカスタマイズが可能です。 取得した値を必要に応じて変更し、ページのコンストラクタまたは[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)のオーバーライドで、 `Padding` プロパティに再割り当てすることができます。

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

## <a name="making-the-navigation-bar-translucent"></a>ナビゲーション バーの透明化

このプラットフォーム仕様はナビゲーションバーの透明度を変更するために使われます。これは XAML で[`NavigationPage.IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty)添付プロパティを `boolean` 値に設定して使用します。

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 [`NavigationPage.EnableTranslucentNavigationBar`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドは、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間に存在し、ナビゲーションバーを透過させるために使用されます。 さらに、 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間の [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage)クラスには、ナビゲーションバーをデフォルトの状態に戻す[`DisableTranslucentNavigationBar`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドや[`IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドを呼ぶことで、ナビゲーションバーの透明度を切り替えることに使用できる[`SetIsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) メソッドもあります。

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

その結果、ナビゲーション バーの透明度を変更することができます。

![](ios-images/translucent-navigation-bar.png "透明ナビゲーション バーのプラットフォーム仕様")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>ステータス バーのテキストの色モードの調整

このプラットフォーム仕様は、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のステータスバーのテキストの色をナビゲーションバーの明るさに合うように調整するかどうかを制御します。 これは、 XAML で[`NavigationPage.StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty)添付プロパティを[`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)列挙型の値に設定して使用します。

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

代わりに、fluent API を使用して c# から使用できます。

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

`NavigationPage.On<iOS>` メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 [`NavigationPage.SetStatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)) メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) のステータスバーのテキスト色をナビゲーションバーの明るさに合うように調整するかどうかを、[`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)列挙型が提供する2つの値を使って制御します。

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) – ステータスバーのテキストの色を調整しないことを示します。
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) – ステータスバーのテキスト色をナビゲーションバーの明るさに合わせることを示します。

さらに、 [`GetStatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドは[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)に適用された現在の[`StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)列挙型の値を取得するために使用できます。

その結果、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)のステータスバーのテキストの色はナビゲーションバーの明るさに合わせて調整されます。 この例では、ステータスバーのテキストの色は [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) の [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) ページと[`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) ページをユーザが切り替えたときに変化します。

![](ios-images/status-bar-text-color-mode.png "ステータス バーのテキストの色モードのプラットフォーム仕様")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>エントリのフォント サイズの調整

このプラットフォーム仕様は、入力されたテキストがコントロール内に収まるように¨[`Entry`](xref:Xamarin.Forms.Entry) のフォントサイズを拡大縮小するために使用されます。 これは XAML で [`Entry.AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) 添付プロパティを `boolean` 値を設定して使用します。

```xaml
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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 [ `Entry.EnableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}))メソッドは、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間に存在し、入力されたテキストのフォントサイズを [`Entry`](xref:Xamarin.Forms.Entry)に収まるように拡大縮小するために使用します。 さらに、 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間の [`Entry`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) クラスには、このプラットフォーム仕様を無効にする [`DisableAdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) メソッドや、[`AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) メソッドを呼ぶことでフォントサイズの拡大縮小を切り替えることに使える[`SetAdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},System.Boolean)) メソッドもあります。

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

その結果、 [`Entry`](xref:Xamarin.Forms.Entry)のフォントサイズは入力テキストがコントロール内に収まるように拡大縮小されます。

![](ios-images/entry-font-size.png "エントリ フォント サイズの調整のプラットフォーム仕様")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Picker のアイテム選択の制御

このプラットフォーム仕様は、[`Picker`](xref:Xamarin.Forms.Picker)でのアイテム選択の発生するタイミングを制御し、ユーザーがコントロール内でアイテムを動かしている時にアイテム選択を発生させるか、**完了** ボタンを押した時に 1 度だけ発生させるかを指定できます。 これは、 XAML で `Picker.UpdateMode` 添付プロパティを `UpdateMode` 列挙型の値に設定して使用します。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Picker.SetUpdateMode`メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、 `UpdateMode` 列挙型が提供する次の 2 つの値を使って、アイテム選択が発生するタイミングを制御するために使用されます。

- `Immediately` – [`Picker`](xref:Xamarin.Forms.Picker)でユーザーがアイテムを動かしているときにアイテム選択が発生します。 これは Xamarin.Forms の既定の動作です。
- `WhenFinished` – [`Picker`](xref:Xamarin.Forms.Picker) でユーザーが**完了**ボタンを押した時に 1 回だけアイテムの選択が発生します。

さらに、`SetUpdateMode` メソッドは、現在の`UpdateMode`を返す`UpdateMode`メソッドを呼んで列挙型の値を切り替えることに使用できます。

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

その結果、[`Picker`](xref:Xamarin.Forms.Picker)に指定された`UpdateMode`が適用され、アイテム選択が発生するタイミングを制御します。

[![](ios-images/picker-updatemode.png "Picker UpdateMode のプラットフォーム仕様")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Page でのステータスバーの可視性の設定


このプラットフォーム仕様は、 [`Page`](xref:Xamarin.Forms.Page) のステータスバーの可視性を設定するために使用されます。これにはステータスバーがどのように `Page` に出入りするのかを制御する機能を含みます。 これは、 XAML で `Page.PrefersStatusBarHidden` 添付プロパティを `StatusBarHiddenMode` 列挙型の値に設定して使用します。そして任意で `Page.PreferredStatusBarUpdateAnimation` 添付プロパティを `UIStatusBarAnimation` 列挙型の値に設定します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  `Page.SetPrefersStatusBarHidden` メソッドは、`Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間に存在し、`StatusBarHiddenMode` 列挙型の値（`Default`、`True`、`False`）の 1 つを指定することで [`Page`](xref:Xamarin.Forms.Page) 上のステータスバーの可視性を設定するために使用されます。 `StatusBarHiddenMode.True` と `StatusBarHiddenMode.False` のそれぞれの値は、デバイスの方向に関係なくステータスバーの可視性を設定し、`StatusBarHiddenMode.Default` の値は縦向きの小さな環境でステータス バーを非表示にします。

その結果、[`Page`](xref:Xamarin.Forms.Page)のステータスバーの可視性を設定することができます。

![](ios-images/hide-status-bar.png "ステータス バーの可視性のプラットフォーム仕様")

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)では、`StatusBarHiddenMode` 列挙型の値にが設定されると、すべての子ページのステータスバーも更新されます。 [`Page`](xref:Xamarin.Forms.Page)から派生する他のすべての型では、`StatusBarHiddenMode` 列挙型の値が設定されると、現在のページのステータスバーだけが更新されます。

`Page.SetPreferredStatusBarUpdateAnimation` メソッドは、`UIStatusBarAnimation` 列挙型の値（`None`、`Fade`、`Slide`）の1つを指定することによって、ステータス バーがどのようにして [`Page`](xref:Xamarin.Forms.Page)に出入りするかを設定するのに使用されます。 `Fade` か `Slide` を指定すると、ステータスバーが `Page` を出入りする際に0.25秒のアニメーションが実行されます。

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>ScrollView でのコンテンツのタッチの遅延

iOSでは、[`ScrollView`](xref:Xamarin.Forms.ScrollView) 内でタッチジェスチャーが開始される時、暗黙のタイマーが呼ばれます。そして `ScrollView` は、そのタイマーの間のユーザーのアクションに基づいて、そのジェスチャーを処理するか、その中のコンテンツに渡すかを決めます。 既定では iOS の `ScrollView` はコンテンツのタッチを遅らせますが、これは `ScrollView` のコンテンツでジェスチャーが発生すべきときに発生しない、というようないくつかの状況での問題の原因となりえます。 したがって、このプラットフォーム仕様は `ScrollView` がタッチジェスチャーを処理するか、その中のコンテンツに渡すかどうかを制御します。 これは、 XAML で `ScrollView.ShouldDelayContentTouches` 添付プロパティを `boolean` 値に設定して使用します。

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

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `ScrollView.SetShouldDelayContentTouches` メソッドは、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、 [`ScrollView`](xref:Xamarin.Forms.ScrollView)がタッチジェスチャーを処理するか、その中のコンテンツに渡すかどうかを制御するために使用されます。 さらに、 `SetShouldDelayContentTouches` メソッドは、コンテンツのタッチを遅らせるかどうかを返す `ShouldDelayContentTouches` メソッドを呼び出すことで、コンテンツのタッチの遅延を切り替えることにも使用できます。

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

その結果は、 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)はコンテンツのタッチを受けるときの遅延は無効にできます。そのためこのシナリオでは、 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) の [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) ではなく[`Slider`](xref:Xamarin.Forms.Slider) がジェスチャーを受け取ります。

[![](ios-images/scrollview-delay-content-touches.png "ScrollView でのコンテンツ タッチの遅延のプラットフォーム仕様")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>区切り記号のスタイルを ListView の設定

このプラットフォームに固有のセルの間の区切り記号かどうかを制御する、 [ `ListView` ](xref:Xamarin.Forms.ListView)の幅全体を使用して、`ListView`します。 これは、 XAML で[`ListView.SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty)添付プロパティを[`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)セル間の区切り記号かどうか、名前空間を使用して、 [ `ListView` ](xref:Xamarin.Forms.ListView)完全な使用幅、`ListView`で、 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)列挙型の 2 つの値を提供します。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – 既定の iOS の区切り記号の動作を示します。 これは Xamarin.Forms の既定の動作です。
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – の 1 つの端から区切り記号が描画されることを示します、`ListView`にします。

結果はするが、指定された[ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)値に適用されます、 [ `ListView`](xref:Xamarin.Forms.ListView)セル間の区切り記号の幅を制御します。

![](ios-images/listview-separatorstyle.png "後のプラットフォームに固有の ListView")

> [!NOTE]
> 区切り記号のスタイルに設定されていると`FullWidth`に変更することはできません`Default`実行時にします。

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>レガシ カラー モードを無効にします。

Xamarin.Forms のビューのいくつかの機能の色をレガシ モード。 このモードでは、ときに、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)ビューのプロパティに設定されて`false`ビューが既定のネイティブ色無効の状態を使用してユーザー設定の色をオーバーライドします。 旧バージョンと互換性のため、この色のレガシ モードでは、サポートされているビューの既定の動作は残ります。

このプラットフォームに固有では、ビューが無効になっている場合でも、ユーザーがビューの設定の色が維持されるようにこの色のレガシ モードが無効にします。 XAML で設定して使用される、 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty)添付プロパティを`false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間を使用してレガシのカラー モードが無効になっているかどうか。 さらに、 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))を従来のカラー モードが無効になっているかどうかを返すメソッドを使用できます。

結果は、ことは、従来のカラー モードを無効にできます、ように、ユーザーがビューの設定の色もビューが無効になっています。

![](ios-images/legacy-color-mode-disabled.png "従来のカラー モードを無効になっています")

> [!NOTE]
> 設定するときに、 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup)レガシ カラー モードが完全に無視されます、ビューにします。 表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

<a name="drop-shadow" />

## <a name="enabling-a-drop-shadow"></a>ドロップ シャドウを有効にします。

このプラットフォームに固有の使用をドロップ シャドウを有効にする、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 設定して XAML で使用される、 [ `VisualElement.IsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty)添付プロパティを`true`、省略可能なその他の番号と共に、ドロップ シャドウを制御するプロパティをアタッチします。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

`VisualElement.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `VisualElement.SetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)にドロップ シャドウが有効になっているかどうか、名前空間を使用して、`VisualElement`します。 さらに、ドロップ シャドウを制御する、次のメソッドを呼び出すことができます。

- [`SetShadowColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Color)) – ドロップ シャドウの色を設定します。 既定の色は[ `Color.Default`](xref:Xamarin.Forms.Color.Default*)します。
- [`SetShadowOffset`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Size)) –、影のオフセットを設定します。 影がキャストされますとしてを指定して方向を変更して、オフセット、 [ `Size` ](xref:Xamarin.Forms.Size)値。 `Size`までの距離 (負の値) の左または右 (正の値)、最初の値と上記の距離をされている 2 番目の値 (負の値) または (正の値) の下、構造体の値は、デバイスに依存しない単位で表されます. このプロパティの既定値は (0.0, 0.0) のすべての面にキャスト、影がその結果、`VisualElement`します。
- [`SetShadowOpacity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) – 値の範囲にある、0.0 (透明) 1.0 (不透明) にドロップ シャドウの不透明度を設定します。 不透明度の既定値は、0.5 です。
- [`SetShadowRadius`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) – ドロップ シャドウを表示するために使用するぼかしの半径を設定します。 既定の半径の値には 10.0 です。

> [!NOTE]
> ドロップ シャドウの状態は呼び出すことによってクエリを実行できる、 [ `GetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))、 [ `GetShadowColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))、 [ `GetShadowOffset` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))、 [ `GetShadowOpacity` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))、および[ `GetShadowRadius` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))メソッド。

結果は、ドロップ シャドウの有効になっている、 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement):

![](ios-images/drop-shadow.png "ドロップ シャドウが有効になっています。")

<a name="simultaneous-pan-gesture" />

## <a name="enabling-simultaneous-pan-gesture-recognition"></a>同時パン ジェスチャの認識を有効にします。

ときに、 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)がすべてのジェスチャがによってキャプチャされた pan のスクロール ビュー内のビューにアタッチされている、`PanGestureRecognizer`は、スクロール ビューに渡されます。 そのため、スクロール ビュー不要になったスクロールします。

このプラットフォームに固有の有効、`PanGestureRecognizer`キャプチャしてビューをスクロール、パン ジェスチャを共有するスクロール ビュー。 XAML で設定して使用される、 [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.platformconfiguration.iosspecific.application.pangesturerecognizershouldrecognizesimultaneouslyproperty?view=xamarin-forms)添付プロパティを`true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.iosspecific.application.setpangesturerecognizershouldrecognizesimultaneously?view=xamarin-forms)メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間を使用して、スクロール ビューでのパン ジェスチャ認識エンジンは、パン ジェスチャのキャプチャまたはキャプチャおよびパンを共有するかどうかジェスチャのビューをスクロールします。 さらに、 [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.iosspecific.application.getpangesturerecognizershouldrecognizesimultaneously?view=xamarin-forms)メソッドを使用して、パン ジェスチャは、スクロール ビューを含むと共有されているかどうかを返す、 [ `PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)します。

そのため、有効にすると、このプラットフォーム固有で、 [ `ListView` ](xref:Xamarin.Forms.ListView)が含まれています、 [ `PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)の両方を`ListView`、`PanGestureRecognizer`パン ジェスチャが表示されますおよびそれを処理します。 ただし、無効にすると、このプラットフォーム固有で、`ListView`が含まれています、 `PanGestureRecognizer`、`PanGestureRecognizer`をパン ジェスチャをキャプチャしてそれを処理し、`ListView`パン ジェスチャを受信しません。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms に組み込まれている iOS のプラットフォーム仕様の使用方法について説明しました。 プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/creating.md)
- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
