---
title: Windows 上の ListView の SelectionMode
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ListView の項目がタップジェスチャに応答できるかどうかを制御する、Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 57EF3A7F-1407-4B31-AE21-D149293D4228
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: de412e064fa84e516dcb8e9b604068c84a2689e6
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563615"
---
# <a name="listview-selectionmode-on-windows"></a>Windows 上の ListView の SelectionMode

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

ユニバーサル Windows プラットフォームでは、は既定でネイティブイベントを使用して、 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) `ItemClick` ネイティブイベントではなく、対話に応答し `Tapped` ます。 これにより、Windows ナレーターとキーボードがと対話できるように、ユーザー補助機能が提供され `ListView` ます。 ただし、このメソッドは、操作不可能な内の tap ジェスチャもレンダリングし `ListView` ます。

このユニバーサル Windows プラットフォーム、の項目 [`ListView`](xref:Xamarin.Forms.ListView) がタップジェスチャに応答できるかどうか、およびネイティブで `ListView` イベントまたはイベントを発生させるかどうかを制御し `ItemClick` `Tapped` ます。 これは、 [`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) 添付プロパティを列挙体の値に設定することによって XAML で使用され [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) ます。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 [ `ListView.SetSelectionMode` ] (Xref: Xamarin.FormsPlatformConfiguration. WindowsSpecific ListView. SetSelectionMode ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。ListView}、 Xamarin.Forms 。ListViewSelectionMode)) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 内の項目がタップジェスチャに応答できるかどうかを制御します [`ListView`](xref:Xamarin.Forms.ListView) [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 。列挙体では、2つの値を指定できます。

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) –は、 `ListView` `ItemClick` 相互作用を処理するためにネイティブイベントを起動し、ユーザー補助機能を提供することを示します。 そのため、Windows ナレーターとキーボードは、と対話でき `ListView` ます。 ただし、の項目は `ListView` tap ジェスチャに応答できません。 これは、ユニバーサル Windows プラットフォーム上のインスタンスの既定の動作です `ListView` 。
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) –は、が `ListView` ネイティブイベントを起動して操作を処理することを示し `Tapped` ます。 そのため、の項目は `ListView` tap ジェスチャに応答できます。 ただし、ユーザー補助機能がないため、Windows ナレーターとキーボードはと対話できません `ListView` 。

> [!NOTE]
> `Accessible`と `Inaccessible` 選択モードは同時に指定でき [`ListView`](xref:Xamarin.Forms.ListView) `ListView` ません。タップジェスチャに応答するアクセス可能なまたはから選択する必要があります。

また、[ `GetSelectionMode` ] (xref: Xamarin.FormsPlatformConfiguration. windows 固有の. ListView. GetSelectionMode ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Windows、 Xamarin.Forms 。ListView})) メソッドを使用して、現在のを返すことができ [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) ます。

結果として、指定されたがに適用されます [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) [`ListView`](xref:Xamarin.Forms.ListView) 。このは、内の項目が `ListView` タップジェスチャに応答できるかどうか、およびネイティブで `ListView` イベントまたはイベントが発生するかどうかを制御し `ItemClick` `Tapped` ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)