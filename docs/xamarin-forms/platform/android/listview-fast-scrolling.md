---
title: ListView の高速 Android 上のスクロール
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォームに固有の ListView 内のデータを高速スクロールできるようにするを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 6ae266f9309e32c79ec6028d737cfdcaaa85b0d4
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926575"
---
# <a name="listview-fast-scrolling-on-android"></a>ListView の高速 Android 上のスクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

この Android プラットフォームに固有の使用のデータを高速スクロールを有効にする[ `ListView`](xref:Xamarin.Forms.ListView)します。 この機能は XAML で `ListView.IsFastScrollEnabled` 添付プロパティを `boolean` 値に設定して使用します。

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

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
