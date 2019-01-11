---
title: Android で TabbedPage ページ遷移アニメーション
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォームに固有で、TabbedPage ページ間を移動するときに遷移アニメーションを無効にするを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 1e9c46fe9535c313581d6d0053559a28aa327887
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209147"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Android で TabbedPage ページ遷移アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

この Android プラットフォームに固有の使用の移動時にページを使用するかプログラムで、または、タブ バーを使用する場合は、遷移アニメーションを無効にする、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 XAML で設定して使用される、`TabbedPage.IsSmoothScrollEnabled`バインド可能なプロパティを`false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 `TabbedPage.SetIsSmoothScrollEnabled`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)でページ間を移動するときに、遷移のアニメーションが表示されるかどうか、名前空間を使用して、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 さらに、`TabbedPage`クラス、`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`名前空間は、次の方法もあります。

- `IsSmoothScrollEnabled`、ページ間を移動するときに、遷移のアニメーションが表示されるかどうかを取得に使用される、`TabbedPage`します。
- `EnableSmoothScroll`、ページ間で移動すると、遷移アニメーションを有効にするに使用される、`TabbedPage`します。
- `DisableSmoothScroll`、でページ間を移動する場合は、遷移アニメーションを無効にするため、`TabbedPage`します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
