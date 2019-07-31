---
title: IOS の安全な領域レイアウトガイド
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ios プラットフォーム固有のを使用する方法について説明します。これにより、iOS 11 以上を使用するすべてのデバイスで安全なページコンテンツが画面の領域に配置されるようになります。
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c6a2ec5a4d1466b7118e6cc7b03cc5518b27e2fb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644540"
---
# <a name="safe-area-layout-guide-on-ios"></a>IOS の安全な領域レイアウトガイド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、iOS 11 以上を使用するすべてのデバイスで安全なページコンテンツが画面の領域に配置されるようにするために使用されます。 特に、iPhone X でデバイスの角の丸まり、ホームインジケータ、またはセンサーハウジングによってコンテンツが切り抜かれないように。これは XAML で `Page.UseSafeArea` 添付プロパティを `boolean` 値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Page.SetUseSafeArea` メソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間に存在し、セーフ エリア レイアウト ガイドを有効にするかどうかを制御します。

その結果、ページコンテンツをすべての iPhone において、画面の安全な領域に配置することができます。

[![](page-safe-area-images/safe-area-layout.png "セーフ エリア レイアウト ガイド")](page-safe-area-images/safe-area-layout-large.png#lightbox "セーフ エリア レイアウト ガイド")

> [!NOTE]
> Apple によって定義されたセーフ エリアは、Xamarin.Forms では[`Page.Padding`](xref:Xamarin.Forms.Page.Padding)プロパティを設定するために使用され、このプロパティの設定済みのすべての値は上書きされます。

セーフ エリアは[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間の `Page.SafeAreaInsets` メソッドを使って、その [`Thickness`](xref:Xamarin.Forms.Thickness)の値を変更することでカスタマイズが可能です。 取得した値を必要に応じて変更し、ページのコンストラクタまたは[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)のオーバーライドで、 `Padding` プロパティに再割り当てすることができます。

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
