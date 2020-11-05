---
title: IOS の NavigationPage バーテキストカラーモード
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、NavigationPage のステータスバーのテキストの色がナビゲーションバーの輝度と一致するかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 03698A44-39F1-4030-9AF5-F10A6713828A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9459ce5e8b8f167f94d1f88e79d9acb32e4788bf
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370612"
---
# <a name="navigationpage-bar-text-color-mode-on-ios"></a>IOS の NavigationPage バーテキストカラーモード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このプラットフォーム固有の設定は、のステータスバーのテキストの色を、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ナビゲーションバーの明るさに合わせて調整するかどうかを制御します。 これは、 [`NavigationPage.StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) 添付プロパティを列挙体の値に設定することによって XAML で使用され [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) ます。

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

または、fluent API を使用して C# から使用することもできます。

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

メソッドは、 `NavigationPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `NavigationPage.SetStatusBarTextColorMode` ] (Xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms SetStatusBarTextColorMode () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage}、 Xamarin.Forms 。StatusBarTextColorMode) メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間で、のステータスバーのテキストの色 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) がナビゲーションバーの輝度と一致するように調整されているかどうかを制御します。列挙体では、 [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 2 つの値を指定できます。

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) –ステータスバーのテキストの色を調整しないことを示します。
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) –ステータスバーのテキストの色がナビゲーションバーの輝度と一致する必要があることを示します。

また、[ `GetStatusBarTextColorMode` ] (xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms GetStatusBarTextColorMode () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage})) メソッドを使用して [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 、に適用されている列挙体の現在の値を取得でき [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ます。

結果として、ナビゲーションバーの明るさに合わせて、のステータスバーのテキストの色を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 調整できます。 この例では、ユーザーがのページとページを切り替えると、ステータスバーのテキストの色が変わり [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ます。

![ステータスバーのテキストの色モード Platform-Specific](status-bar-text-color-images/status-bar-text-color-mode.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)