---
title: Android での TabbedPage ページ切り替えアニメーション
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、TabbedPage 内のページ間を移動するときに、切り替え効果のアニメーションを無効にする Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d3ae03ec6cbc3469422e6a2d57f186254e87f40c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140019"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Android での TabbedPage ページ切り替えアニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、プログラムによって、またはタブバーを使用しているときに、ページ間を移動するときに、遷移アニメーションを無効にするために使用され [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `TabbedPage.IsSmoothScrollEnabled` `false` ます。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

メソッドは、 `TabbedPage.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 `TabbedPage.SetIsSmoothScrollEnabled`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 内のページ間を移動するときに切り替えアニメーションを表示するかどうかを制御するために使用され [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。 また、 `TabbedPage` 名前空間のクラスに `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` は、次のメソッドもあります。

- `IsSmoothScrollEnabled`。内のページ間を移動するときに、切り替え効果アニメーションを表示するかどうかを取得するために使用され `TabbedPage` ます。
- `EnableSmoothScroll`。内のページ間を移動するときに、遷移アニメーションを有効にするために使用され `TabbedPage` ます。
- `DisableSmoothScroll`。内のページ間を移動するときに、遷移アニメーションを無効にするために使用され `TabbedPage` ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
