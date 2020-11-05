---
title: Android での WebView 混合コンテンツ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、API 21 以上を対象とするアプリケーションで WebView に混合コンテンツを表示する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cfcdd52187fe8e3af258aedb25e8c0df85cbea4e
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375487"
---
# <a name="webview-mixed-content-on-android"></a>Android での WebView 混合コンテンツ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有の設定は、が [`WebView`](xref:Xamarin.Forms.WebView) API 21 以上を対象とするアプリケーションで混合コンテンツを表示できるかどうかを制御します。 混合コンテンツは、HTTPS 接続を介して最初に読み込まれるコンテンツですが、HTTP 接続を介してリソース (画像、オーディオ、ビデオ、スタイルシート、スクリプトなど) を読み込みます。 これは、 [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) 添付プロパティを列挙体の値に設定することによって XAML で使用され [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) ます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

メソッドは、 `WebView.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `WebView.SetMixedContentMode` ] (Xref: Xamarin.FormsPlatformConfiguration. SetMixedContentMode (を指定します。 Xamarin.FormsIPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。WebView}、 Xamarin.Forms 。PlatformConfiguration. AndroidSpecific) メソッド [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) は、混合コンテンツを表示できるかどうかを制御するために使用されます。列挙体には、 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 次の3つの値を指定できます。

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) –は、 [`WebView`](xref:Xamarin.Forms.WebView) HTTPS オリジンが HTTP オリジンからのコンテンツを読み込むことを許可することを示します。
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) –が [`WebView`](xref:Xamarin.Forms.WebView) HTTPS オリジンによる HTTP オリジンからのコンテンツの読み込みを許可しないことを示します。
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) –は、 [`WebView`](xref:Xamarin.Forms.WebView) 最新のデバイス web ブラウザーのアプローチとの互換性を試行することを示します。 一部の HTTP コンテンツは、HTTPS オリジンによって読み込まれる可能性があり、その他の種類のコンテンツはブロックされます。 ブロックまたは許可されているコンテンツの種類は、各オペレーティングシステムのリリースで変更される可能性があります。

結果として、指定された [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 値がに適用され [`WebView`](xref:Xamarin.Forms.WebView) 、混合コンテンツを表示できるかどうかを制御します。

[![WebView 混合コンテンツ処理プラットフォーム固有](webview-mixed-content-images/webview-mixedcontent.png "WebView 混合コンテンツ処理プラットフォーム固有")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView 混合コンテンツ処理プラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)