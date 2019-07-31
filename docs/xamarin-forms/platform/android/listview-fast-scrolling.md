---
title: Android での ListView の高速スクロール
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ListView のデータをすばやくスクロールできるようにする、Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: ce51483da9599cf049cf005ae18b35d110aa325b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649991"
---
# <a name="listview-fast-scrolling-on-android"></a>Android での ListView の高速スクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、 [`ListView`](xref:Xamarin.Forms.ListView)内のデータをすばやくスクロールできるようにするために使用されます。 この機能は XAML で `ListView.IsFastScrollEnabled` 添付プロパティを `boolean` 値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>`メソッドはこのプラットフォーム仕様が Android上でのみ動作することを指定します。 `ListView.SetIsFastScrollEnabled`メソッドは、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間に存在し、アプリケーションがバックグラウンドに切り替わった時に[`ListView`](xref:Xamarin.Forms.ListView)ぺージイベントの発生を有効化または無効化するために使用されます。 `SetIsFastScrollEnabled`メソッドは、アプリケーションがバックグラウンドから復帰した時に`IsFastScrollEnabled`ページイベントの発生を有効化または無効化するために使用されます。

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

メソッドは、一時停止時にソフトキーボードの操作方式に[`ListView`](xref:Xamarin.Forms.ListView)が設定されていて、それが表示されている場合に、再開時にそれを表示するかどうかを制御するために使用されます。

[![](listview-fast-scrolling-images/fastscroll.png "ListView FastScroll プラットフォーム固有")](listview-fast-scrolling-images/fastscroll-large.png#lightbox "ListView FastScroll プラットフォームに固有")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
