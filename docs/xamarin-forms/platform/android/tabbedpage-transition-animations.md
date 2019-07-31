---
title: Android での TabbedPage ページ切り替えアニメーション
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、TabbedPage 内のページ間を移動するときに、切り替え効果のアニメーションを無効にする Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 4f8d6ec2b06855364970bc9b672c3d3f7b9bfdfc
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649837"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Android での TabbedPage ページ切り替えアニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、プログラムによって、またはタブバーを使用しているときに、ページ間を[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)移動するときに、遷移アニメーションを無効にするために使用されます。 XAML で設定して使用される、`TabbedPage.IsSmoothScrollEnabled`バインド可能なプロパティを`false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

代わりに、fluent API を使用して C# から使用できます。

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

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
