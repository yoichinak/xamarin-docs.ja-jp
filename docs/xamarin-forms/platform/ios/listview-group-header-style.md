---
title: IOS で ListView グループ ヘッダーのスタイル
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ListView ヘッダー セルをスクロール中にフローティングするかどうかを制御、iOS プラットフォームに固有の使用方法について説明します。
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 58b3787b71cff9b78f1c6b577be6c320367f1cee
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557454"
---
# <a name="listview-group-header-style-on-ios"></a>IOS で ListView グループ ヘッダーのスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

この iOS プラットフォームに固有のコントロールかどうか[ `ListView` ](xref:Xamarin.Forms.ListView)ヘッダー セルをスクロール中には float です。 XAML で設定して使用される、`ListView.GroupHeaderStyle`バインド可能なプロパティの値を`GroupHeaderStyle`列挙体。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  `ListView.SetGroupHeaderStyle`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間を使用しているかどうか[ `ListView` ](xref:Xamarin.Forms.ListView)ヘッダー セルをスクロール中には float です。 `GroupHeaderStyle`列挙体が 2 つの値を提供します。

- `Plain` – ヘッダー セルが float タイミングを示す、 [ `ListView` ](xref:Xamarin.Forms.ListView)は (既定値) をスクロールします。
- `Grouped` – あるヘッダー セル浮動しないときを示します、 [ `ListView` ](xref:Xamarin.Forms.ListView)がスクロールします。

さらに、`ListView.GetGroupHeaderStyle`を返すメソッドを使用することができます、`GroupHeaderStyle`に適用される、 [ `ListView`](xref:Xamarin.Forms.ListView)します。

結果はするが、指定された`GroupHeaderStyle`値に適用されます、 [ `ListView` ](xref:Xamarin.Forms.ListView)、ヘッダー セルをスクロール中にフローティングするかどうかを制御します。

[![浮動小数点と非フローティング ListView ヘッダー セルの iOS でスクリーン ショット](listview-group-header-style-images/group-header-styles.png "の浮動小数点と非フローティングのヘッダー セルで、ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "の浮動小数点と非フローティングのヘッダー セルで、ListView")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
