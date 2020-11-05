---
title: IOS での ListView 区切り記号のスタイル
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ListView のセル間の区切り記号が ListView の完全な幅を使用するかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: A4CB45CE-9FB7-47ED-8C3D-93E39BF282E4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 527ae4eee614e1b3c6225c6cd2b220030faa7f3e
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372484"
---
# <a name="listview-separator-style-on-ios"></a>IOS での ListView 区切り記号のスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の制御は、のセル間の区切り記号がの [`ListView`](xref:Xamarin.Forms.ListView) 完全な幅を使用するかどうかを制御し `ListView` ます。 これは、 [`ListView.SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) 添付プロパティを列挙体の値に設定することによって XAML で使用され [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

メソッドは、 `ListView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `ListView.SetSeparatorStyle` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific Xamarin.Forms ()。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。ListView}、 Xamarin.Forms 。SeparatorStyle)) メソッドを名前空間で使用して、の [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) セル間の区切り記号がの完全な幅を使用するかどうかを制御します。列挙体では、 [`ListView`](xref:Xamarin.Forms.ListView) `ListView` [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 2 つの値を指定できます。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) –既定の iOS の区切り記号の動作を示します。 これは、の既定の動作です Xamarin.Forms 。
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) –のある端からもう一方の端まで、区切り記号が描画されることを示し `ListView` ます。

結果として、指定された [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 値がに適用され [`ListView`](xref:Xamarin.Forms.ListView) 、セル間の区切り記号の幅が制御されます。

![ListView SeparatorStyle Platform-Specific](listview-separator-style-images/listview-separatorstyle.png)

> [!NOTE]
> 区切り記号のスタイルをに設定すると `FullWidth` 、実行時にに戻すことはできません `Default` 。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)