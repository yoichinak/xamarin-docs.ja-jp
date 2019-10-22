---
title: Android での WebView 混合コンテンツ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、API 21 以上を対象とするアプリケーションで WebView に混合コンテンツを表示する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: f9b4a12d3049cea37235cc04805a7411a9bdb236
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696488"
---
# <a name="webview-mixed-content-on-android"></a>Android での WebView 混合コンテンツ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有の設定は、 [`WebView`](xref:Xamarin.Forms.WebView)が API 21 以上を対象とするアプリケーションに混合コンテンツを表示できるかどうかを制御します。 混合コンテンツは、HTTPS 接続を介して最初に読み込まれるコンテンツですが、HTTP 接続を介してリソース (画像、オーディオ、ビデオ、スタイルシート、スクリプトなど) を読み込みます。 XAML では、 [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty)添付プロパティを[`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)列挙値に設定することによって使用されます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

または、fluent API C#を使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

@No__t_0 メソッドは、このプラットフォーム固有のが Android でのみ実行されることを指定します。 [@No__t_3](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間の[`WebView.SetMixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling))メソッドは、混合コンテンツを表示できるかどうかを制御するために使用されます。 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)列挙体では、次の3つの値を指定できます。

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – [`WebView`](xref:Xamarin.Forms.WebView)が HTTPS オリジンによる HTTP オリジンからのコンテンツの読み込みを許可することを示します。
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – [`WebView`](xref:Xamarin.Forms.WebView)が HTTPS オリジンによる HTTP オリジンからのコンテンツの読み込みを許可しないことを示します。
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – [`WebView`](xref:Xamarin.Forms.WebView)が最新のデバイス web ブラウザーのアプローチとの互換性を試行することを示します。 一部の HTTP コンテンツは、HTTPS オリジンによって読み込まれる可能性があり、その他の種類のコンテンツはブロックされます。 ブロックまたは許可されているコンテンツの種類は、各オペレーティングシステムのリリースで変更される可能性があります。

結果として、指定された[`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)値が[`WebView`](xref:Xamarin.Forms.WebView)に適用され、混合コンテンツを表示できるかどうかを制御します。

[![WebView 混合コンテンツ処理プラットフォーム固有](webview-mixed-content-images/webview-mixedcontent.png "WebView 混合コンテンツ処理プラットフォーム固有")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView 混合コンテンツ処理プラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
