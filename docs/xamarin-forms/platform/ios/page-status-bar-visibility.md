---
title: IOS でのページステータスバーの表示
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ページのステータスバーの表示を設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 22d54b1726858b1f46cf312f4962091374385704
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936826"
---
# <a name="page-status-bar-visibility-on-ios"></a>IOS でのページステータスバーの表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のステータスバーの表示を設定するために使用され [`Page`](xref:Xamarin.Forms.Page) ます。また、ステータスバーがを入力または脱退する方法を制御することもでき `Page` ます。 XAML で使用されるのは、 `Page.PrefersStatusBarHidden` 添付プロパティを列挙値に設定 `StatusBarHiddenMode` し、必要に応じて、 `Page.PreferredStatusBarUpdateAnimation` 添付プロパティを列挙値に設定すること `UIStatusBarAnimation` です。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

メソッドは、 `Page.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Page.SetPrefersStatusBarHidden`名前空間のメソッドは、 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` [`Page`](xref:Xamarin.Forms.Page) 列挙値のいずれか `StatusBarHiddenMode` `Default` (、 `True` 、または) を指定することによって、のステータスバーの表示を設定するために使用されます `False` 。 との値は、 `StatusBarHiddenMode.True` `StatusBarHiddenMode.False` デバイスの向きに関係なくステータスバーの可視性を設定し、 `StatusBarHiddenMode.Default` 値は垂直方向のコンパクトな環境でステータスバーを非表示にします。

結果として、のステータスバーの可視性は次のように [`Page`](xref:Xamarin.Forms.Page) 設定できます。

![ステータスバーの可視性プラットフォーム固有](page-status-bar-visibility-images/hide-status-bar.png)

> [!NOTE]
> では [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 、指定された列挙値によって、 `StatusBarHiddenMode` すべての子ページのステータスバーも更新されます。 その他のすべての [`Page`](xref:Xamarin.Forms.Page) 派生型では、指定された `StatusBarHiddenMode` 列挙値は現在のページのステータスバーのみを更新します。

メソッドは、、、 `Page.SetPreferredStatusBarUpdateAnimation` [`Page`](xref:Xamarin.Forms.Page) またはのいずれかの列挙値を指定して、ステータスバーがをどのように入力または脱退するかを設定するために使用されます `UIStatusBarAnimation` `None` `Fade` `Slide` 。 `Fade`または `Slide` 列挙値が指定されている場合は、ステータスバーがを出入りするときに 0.25 2 番目のアニメーションが実行され `Page` ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
