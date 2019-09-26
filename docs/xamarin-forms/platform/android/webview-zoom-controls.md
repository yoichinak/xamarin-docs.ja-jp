---
title: Android での WebView のズーム
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。 この記事では、WebView のズームを有効にする Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 2142882add91d613263d11fa4c1e6d7ad142c7c7
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "68656001"
---
# <a name="webview-zoom-on-android"></a>Android での WebView のズーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有の機能により、の[`WebView`](xref:Xamarin.Forms.WebView)ピンチとズームのコントロールが有効になります。 次のように、 `WebView.EnableZoomControls` `WebView.DisplayZoomControls`バインド可能なプロパティを値に`boolean`設定することにより、XAML で使用されます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

バインド`WebView.EnableZoomControls`可能なプロパティは、 [`WebView`](xref:Xamarin.Forms.WebView)でピンチからズームを有効にするかどうか`WebView.DisplayZoomControls`を制御し、バインド可能なプロパティは、 `WebView`ズームコントロールをに重ねて表示するかどうかを制御します。

または、fluent API C#を使用して、プラットフォーム固有のを使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>`メソッドは、このプラットフォーム仕様が Android 上でのみ動作することを指定します。 名前空間[`WebView`](xref:Xamarin.Forms.WebView)の`WebView.EnableZoomControls`メソッドを使用して、でピンチ操作によるズームを有効にするかどうかを制御[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)します。 同じ名前空間の`WebView`メソッドを使用して、ズームコントロールをに重ねて表示するかどうかを制御します。`WebView.DisplayZoomControls` また`WebView.ZoomControlsEnabled` 、メソッドと`WebView.ZoomControlsDisplayed`メソッドを使用して、ピンチとズームの両方のコントロールが有効になっているかどうかを返すことができます。

結果として、では[`WebView`](xref:Xamarin.Forms.WebView)ピンチからズームを有効にすることができ、ズームコントロールはにオーバーレイ`WebView`できます。

[![Android でのズームされる WebView のスクリーンショット](webview-zoom-controls-images/webview-zoom.png "拡大")した WebView](webview-zoom-controls-images/webview-zoom-large.png#lightbox "拡大した WebView")

> [!IMPORTANT]
> ズームコントロールは、 [`WebView`](xref:Xamarin.Forms.WebView)それぞれのバインド可能なプロパティまたはメソッドを使用して、に重ねられるように有効にし、表示する必要があります。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
