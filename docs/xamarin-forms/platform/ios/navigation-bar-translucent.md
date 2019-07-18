---
title: IOS で NavigationPage バーの透明度
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、NavigationPage でナビゲーション バーの透明度を変更するプラットフォームに固有の iOS を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 1150941B-56DB-4781-BE36-A4C4F9F2C500
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d6a3538b4083112a2bf1a343a9db438e4894815b
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925649"
---
# <a name="navigationpage-bar-translucency-on-ios"></a>IOS で NavigationPage バーの透明度

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この iOS プラットフォームに固有の使用上のナビゲーション バーの透明度を変更する、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)、設定によって、XAML で使用されると、 [ `NavigationPage.IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty)添付プロパティを`boolean`値。

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [`NavigationPage.EnableTranslucentNavigationBar`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドは、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間に存在し、ナビゲーションバーを透過させるために使用されます。 さらに、 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間の [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage)クラスには、ナビゲーションバーをデフォルトの状態に戻す[`DisableTranslucentNavigationBar`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドや[`IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}))メソッドを呼ぶことで、ナビゲーションバーの透明度を切り替えることに使用できる[`SetIsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) メソッドもあります。

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

その結果、ナビゲーション バーの透明度を変更することができます。

![](navigation-bar-translucent-images/translucent-navigation-bar.png "透明ナビゲーション バーのプラットフォーム仕様")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
