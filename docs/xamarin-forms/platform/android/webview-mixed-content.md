---
title: Android での WebView 混合コンテンツ
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、API 21 以上を対象とするアプリケーションで WebView に混在したコンテンツを表示する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 286a7dceead327d727110d4ebbcecbc2341345b3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656074"
---
# <a name="webview-mixed-content-on-android"></a>Android での WebView 混合コンテンツ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有の設定は、 [`WebView`](xref:Xamarin.Forms.WebView)が API 21 以上を対象とするアプリケーションで混合コンテンツを表示できるかどうかを制御します。 混合コンテンツは、HTTP 接続経由で (イメージ、オーディオ、ビデオ、スタイル シート、スクリプト) などのリソースを読み込みます HTTPS 接続経由で最初に読み込まれるコンテンツです。 これは、 XAML で[`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty)添付プロパティを[`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間を使用して混合コンテンツを表示できるかどうかを[ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)3 つの値を提供する列挙体。

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTP の配信元からコンテンツの読み込みに HTTPS origin できるようになります。
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP の配信元からコンテンツを読み込むには許可されません。
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – ことを示します、 [ `WebView` ](xref:Xamarin.Forms.WebView)アプローチでは最新のデバイスの web ブラウザーの互換性があることを試みます。 一部の HTTP コンテンツが HTTPS の配信元で読み込むことができるし、他の種類のコンテンツはブロックされます。 ブロックまたは許可するコンテンツの種類は、各オペレーティング システムのリリースで変更できます。

結果はするが、指定された[ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)値に適用されます、 [ `WebView` ](xref:Xamarin.Forms.WebView)、混合コンテンツを表示できるかどうかを制御します。

[![WebView 混合コンテンツの処理プラットフォーム固有](webview-mixed-content-images/webview-mixedcontent.png "WebView 混合コンテンツの処理プラットフォーム固有")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView 混合プラットフォームに固有のコンテンツの処理")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
