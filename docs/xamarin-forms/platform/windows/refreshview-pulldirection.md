---
title: Windows 上の RefreshView Pull 方向
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、RefreshView のプル方向を変更できるようにする、Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 407A862B-281E-4384-9696-C0655830B84D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: cf2ab38bed7b45a48fcf0b5f86add49c0d4cc21f
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697651"
---
# <a name="refreshview-pull-direction-on-windows"></a>Windows 上の RefreshView Pull 方向

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

これにより、プラットフォーム固有のユニバーサル Windows プラットフォーム、データを表示しているスクロール可能なコントロールの向きに合わせて、`RefreshView` のプル方向を変更できます。 XAML では、`RefreshView.RefreshPullDirection` バインド可能なプロパティを `RefreshPullDirection` 列挙値に設定することによって使用されます。

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

または、fluent API C#を使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

`RefreshView.On<Windows>` メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォーム上でのみ実行されることを指定します。 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間の `RefreshView.SetRefreshPullDirection` メソッドは、`RefreshView` のプル方向を設定するために使用されます。 `RefreshPullDirection` 列挙体には、次の4つの値を指定できます。

- `LeftToRight` は、左から右へのプルによって更新が開始されることを示します。
- `TopToBottom` は、上から下へのプルによって更新が開始され、`RefreshView` の既定のプル方向であることを示します。
- `RightToLeft` は、右から左へのプルによって更新が開始されることを示します。
- `BottomToTop` は、下から上へのプルによって更新が開始されることを示します。

さらに、`GetRefreshPullDirection` メソッドを使用して、`RefreshView` の現在の `RefreshPullDirection` を返すことができます。

結果として、指定した `RefreshPullDirection` が `RefreshView` に適用され、データを表示しているスクロール可能なコントロールの向きに合わせてプル方向が設定されます。 次のスクリーンショットは、`LeftToRight` のプル方向の `RefreshView` を示しています。

[![UWP での、左から右へのプル方向の RefreshView のスクリーンショット](refreshview-pulldirection-images/refreshview-pulldirection.png "左から右方向のプル方向の RefreshView")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "左から右方向のプル方向の RefreshView")

> [!NOTE]
> プル方向を変更すると、進行状況の円の開始位置が自動的に回転し、矢印がプル方向の適切な位置で開始されます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
