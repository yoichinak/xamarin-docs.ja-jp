---
title: Android での WebView のズーム
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、WebView のズームを有効にする Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 2142882add91d613263d11fa4c1e6d7ad142c7c7
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656001"
---
# <a name="webview-zoom-on-android"></a>Android での WebView のズーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有の機能により、 [`WebView`](xref:Xamarin.Forms.WebView)のピンチとズームのコントロールが有効になります。 XAML では、`WebView.EnableZoomControls` と `WebView.DisplayZoomControls` バインド可能なプロパティを `boolean` 値に設定することによって使用されます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

@No__t_0 のバインド可能なプロパティは、 [`WebView`](xref:Xamarin.Forms.WebView)でピンチ操作が有効になっているかどうかを制御し、`WebView.DisplayZoomControls` バインド可能プロパティは、ズームコントロールを `WebView` に重ねて表示するかどうかを制御します。

または、fluent API C#を使用して、プラットフォーム固有のを使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

@No__t_0 メソッドは、このプラットフォーム固有のが Android でのみ実行されることを指定します。 [@No__t_2](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間の `WebView.EnableZoomControls` メソッドを使用して、 [`WebView`](xref:Xamarin.Forms.WebView)でピンチによるズームを有効にするかどうかを制御します。 同じ名前空間内の `WebView.DisplayZoomControls` メソッドは、ズームコントロールを `WebView` に重ねて表示するかどうかを制御するために使用されます。 さらに、`WebView.ZoomControlsEnabled` および `WebView.ZoomControlsDisplayed` メソッドを使用して、[ズーム] コントロールと [ズーム] コントロールがそれぞれ有効になっているかどうかを返すことができます。

結果として、 [`WebView`](xref:Xamarin.Forms.WebView)に対してピンチ化を有効にすることができ、ズームコントロールを `WebView` に重ね合わせることができます。

[![Android でのズームされる WebView のスクリーンショット](webview-zoom-controls-images/webview-zoom.png "拡大した WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "拡大した WebView")

> [!IMPORTANT]
> ズームコントロールは、それぞれのバインド可能なプロパティまたはメソッドを使用して、 [`WebView`](xref:Xamarin.Forms.WebView)にオーバーレイするように有効にし、表示する必要があります。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
