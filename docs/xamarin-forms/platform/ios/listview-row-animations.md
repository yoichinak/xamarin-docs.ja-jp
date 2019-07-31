---
title: IOS での ListView 行アニメーション
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ListView 項目のコレクションを更新するときに行アニメーションを無効にするかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 9e44c22e670847102cf0bc0f79a860abcac30760
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652832"
---
# <a name="listview-row-animations-on-ios"></a>IOS での ListView 行アニメーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の設定では、 [`ListView`](xref:Xamarin.Forms.ListView)項目コレクションを更新するときに行アニメーションを無効にするかどうかを制御します。 XAML で設定して使用される、`ListView.RowAnimationsEnabled`バインド可能なプロパティを`false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 名前空間のメソッド`ListView.SetRowAnimationsEnabled` [`ListView`](xref:Xamarin.Forms.ListView)は、項目のコレクションの更新時に行アニメーションを無効にするかどうかを制御するために使用されます。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) また`ListView.GetRowAnimationsEnabled` 、メソッドを使用して、 `ListView`で行アニメーションが無効になっているかどうかを返すこともできます。

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView)既定では、行アニメーションが有効になっています。 したがって、に`ListView`新しい行が挿入されると、アニメーションが発生します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
