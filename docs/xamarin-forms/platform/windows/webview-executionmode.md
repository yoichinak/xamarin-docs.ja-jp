---
title: Windows での WebView 実行モード
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、WebView がコンテンツをホストするスレッドを設定する Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4D3C4112-0FF6-47F8-BC22-579D92032807
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 314851c4697e95ef8662710c986d0b175249fad6
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940842"
---
# <a name="webview-execution-mode-on-windows"></a>Windows での WebView 実行モード

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

このプラットフォーム固有のは、がそのコンテンツをホストするスレッドを設定し [`WebView`](xref:Xamarin.Forms.WebView) ます。 これは、 `WebView.ExecutionMode` バインド可能なプロパティを列挙値に設定することによって XAML で使用され `WebViewExecutionMode` ます。

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.ExecutionMode="SeparateThread" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

WebView webView = new Xamarin.Forms.WebView();
webView.On<Windows>().SetExecutionMode(WebViewExecutionMode.SeparateThread);
```

`WebView.On<Windows>`メソッドは、このプラットフォーム固有のがユニバーサル Windows プラットフォームでのみ実行されることを指定します。 名前空間のメソッドを使用して、が `WebView.SetExecutionMode` [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) そのコンテンツをホストするスレッドを設定し `WebView` `WebViewExecutionMode` ます。列挙体には、次の3つの値を指定できます。

- `SameThread` コンテンツが UI スレッドでホストされていることを示します。 これは、Windows 上のの既定値です `WebView` 。
- `SeparateThread` コンテンツがバックグラウンドスレッドでホストされていることを示します。
- `SeparateProcess` コンテンツがアプリプロセスとは別のプロセスでホストされていることを示します。 WebView インスタンスごとに個別のプロセスは存在しないため、すべてのアプリの WebView インスタンスが同じプロセスを共有します。

また、メソッドを使用して、の現在のを `GetExecutionMode` 返すこともでき `WebViewExecutionMode` `WebView` ます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
