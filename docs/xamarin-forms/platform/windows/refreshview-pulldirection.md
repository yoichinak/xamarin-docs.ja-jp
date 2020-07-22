---
title: Windows での RefreshView のプル方向
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、RefreshView のプル方向を変更できるようにする、Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 407A862B-281E-4384-9696-C0655830B84D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46a1b4d00b9eea276b9a3b3d5bffbdac3d31e0ef
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136579"
---
# <a name="refreshview-pull-direction-on-windows"></a>Windows での RefreshView のプル方向

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このユニバーサル Windows プラットフォームプラットフォーム固有であるため、 `RefreshView` データを表示しているスクロール可能なコントロールの向きに合わせてのプル方向を変更できます。 このメソッドは、バインド可能な `RefreshView.RefreshPullDirection` プロパティを列挙体の値に設定することにより、XAML で使用され `RefreshPullDirection` ます。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <RefreshView windows:RefreshView.RefreshPullDirection="LeftToRight"
                 IsRefreshing="{Binding IsRefreshing}"
                 Command="{Binding RefreshCommand}">
        <ScrollView>
            ...
        </ScrollView>
    </RefreshView>
 </ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

`RefreshView.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 `RefreshView.SetRefreshPullDirection`名前空間のメソッドは [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 、のプル方向を設定するために使用され `RefreshView` `RefreshPullDirection` ます。列挙体では、次の4つの値を指定できます。

- `LeftToRight`左から右へのプルによって更新が開始されることを示します。
- `TopToBottom`上から下へのプルが更新を開始し、の既定のプル方向であることを示し `RefreshView` ます。
- `RightToLeft`右から左へプルすると、更新が開始されることを示します。
- `BottomToTop`下から上へプルすると、更新が開始されることを示します。

また、メソッドを `GetRefreshPullDirection` 使用して、の現在のを返すこともでき `RefreshPullDirection` `RefreshView` ます。

結果として、指定したがに適用され、 `RefreshPullDirection` `RefreshView` データを表示しているスクロール可能なコントロールの向きに合わせてプル方向が設定されます。 次のスクリーンショットは、プル方向のを示してい `RefreshView` `LeftToRight` ます。

[![UWP での、左から右へのプル方向の RefreshView のスクリーンショット](refreshview-pulldirection-images/refreshview-pulldirection.png "左から右方向のプル方向の RefreshView")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "左から右方向のプル方向の RefreshView")

> [!NOTE]
> プル方向を変更すると、進行状況の円の開始位置が自動的に回転し、矢印がプル方向の適切な位置で開始されます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
