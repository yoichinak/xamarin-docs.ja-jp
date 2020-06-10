---
title: "NavigationPage Bar Separator on iOS" 説明: "プラットフォーム-詳細" を使用すると、特定のプラットフォームでのみ使用できる機能を使用して、カスタムレンダラーや特殊効果を実装することができます。 この記事では、NavigationPage のナビゲーションバーの下部にある区切り線と影を非表示にする iOS プラットフォーム固有のを使用する方法について説明します。
ms. 製品: xamarin ms. assetid: 5A45748A-6779-4441-82F2-415BD68473B9: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="navigationpage-bar-separator-on-ios"></a>IOS 上の NavigationPage バーの区切り記号

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のナビゲーションバーの下部にある区切り線と影を非表示にし [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ます。 これは、バインド可能なプロパティをに設定することによって XAML で使用され [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) `false` ます。

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

メソッドは、 `NavigationPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `NavigationPage.SetHideNavigationBarSeparator` ] (Xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms SetHideNavigationBarSeparator () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage}, Boolean) メソッドを [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間で使用して、ナビゲーションバーの区切り記号を非表示にするかどうかを制御します。 また、[ `NavigationPage.HideNavigationBarSeparator` ] (xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms HideNavigationBarSeparator () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage})) メソッドを使用して、ナビゲーションバーの区切り記号が非表示になっているかどうかを返すことができます。

結果として、のナビゲーションバーの区切り記号を [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 非表示にすることができます。

![](navigation-bar-separator-images/navigationpage-hideseparatorbar.png "NavigationPage navigation bar hidden")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
