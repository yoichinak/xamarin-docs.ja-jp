---
title: Android での WebView のズーム
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、WebView のズームを有効にする Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e41efd4bf100205f20b8f4e02a74fad193134a4
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556426"
---
# <a name="webview-zoom-on-android"></a>Android での WebView のズーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有の機能により、のピンチとズームのコントロールが有効になり [`WebView`](xref:Xamarin.Forms.WebView) ます。 次のように、バインド可能なプロパティを値に設定することにより、XAML で使用され `WebView.EnableZoomControls` `WebView.DisplayZoomControls` `boolean` ます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

`WebView.EnableZoomControls`バインド可能なプロパティは、でピンチからズームを有効にするかどうかを制御し、バインド可能な [`WebView`](xref:Xamarin.Forms.WebView) `WebView.DisplayZoomControls` プロパティは、ズームコントロールをに重ねて表示するかどうかを制御し `WebView` ます。

また、fluent API を使用して C# からプラットフォーム固有のものを使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

メソッドは、 `WebView.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 `WebView.EnableZoomControls`名前空間のメソッドを [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 使用して、でピンチ操作によるズームを有効にするかどうかを制御し [`WebView`](xref:Xamarin.Forms.WebView) ます。 `WebView.DisplayZoomControls`同じ名前空間のメソッドを使用して、ズームコントロールをに重ねて表示するかどうかを制御し `WebView` ます。 また、メソッドとメソッドを使用して、ピンチとズームの両方の `WebView.ZoomControlsEnabled` `WebView.ZoomControlsDisplayed` コントロールが有効になっているかどうかを返すことができます。

結果として、ではピンチからズームを有効にすることができ、 [`WebView`](xref:Xamarin.Forms.WebView) ズームコントロールはにオーバーレイできます `WebView` 。

[![Android でのズームされる WebView のスクリーンショット](webview-zoom-controls-images/webview-zoom.png "拡大した WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "拡大した WebView")

> [!IMPORTANT]
> ズームコントロールは、それぞれのバインド可能なプロパティまたはメソッドを使用して、に重ねられるように有効にし、表示する必要があり [`WebView`](xref:Xamarin.Forms.WebView) ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)