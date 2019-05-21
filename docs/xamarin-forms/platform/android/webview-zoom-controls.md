---
title: Android web ビューのズーム
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォームに固有の web ビューのズームをできるようにするを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 0e180ca5019bd4e5d1351cfb2f7a8c28ade2afe2
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971644"
---
# <a name="webview-zoom-on-android"></a>Android web ビューのズーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

この Android プラットフォームに固有でピンチ ズームとズーム コントロールを有効にする[ `WebView`](xref:Xamarin.Forms.WebView)します。 XAML で設定して使用される、`WebView.EnableZoomControls`と`WebView.DisplayZoomControls`バインド可能なプロパティを`boolean`値。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

`WebView.EnableZoomControls`バインド可能なプロパティで、ピンチ ズームを有効にするかどうかを制御する、 [ `WebView` ](xref:Xamarin.Forms.WebView)、および`WebView.DisplayZoomControls`バインド可能なプロパティは、ズーム コントロールでのオーバーレイ表示されるかどうかを制御、`WebView`します。

プラットフォームに固有の使用または、 C# fluent API を使用します。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 `WebView.EnableZoomControls`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)でピンチ ズームを有効にするかどうか、名前空間を使用して、 [ `WebView`](xref:Xamarin.Forms.WebView)します。 `WebView.DisplayZoomControls`メソッドを同じ名前空間を使用でズーム コントロールがオーバーレイ表示されるかどうかを制御する、`WebView`します。 さらに、`WebView.ZoomControlsEnabled`と`WebView.ZoomControlsDisplayed`有効かどうかピンチ ズームとズームのコントロールは、それぞれを返すメソッドを使用できます。

結果でそのピンチ、ズームを有効にすることができます、 [ `WebView` ](xref:Xamarin.Forms.WebView)、ズーム コントロールにオーバーレイでき、 `WebView`:

[![Android WebView を拡大のスクリーン ショット](webview-zoom-controls-images/webview-zoom.png "拡大 WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "WebView の拡大")

> [!IMPORTANT]
> ズーム コントロールする必要がありますが両方有効になっているし、それぞれのバインド可能なプロパティまたはでオーバーレイ対象のメソッドを使用して、表示、 [ `WebView`](xref:Xamarin.Forms.WebView)します。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
