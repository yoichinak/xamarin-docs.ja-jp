---
title: Xamarin.Essentials:クリップボードのトピック
description: このドキュメントでは、Xamarin.Essentials の Clipboard クラスについて説明します。これを使用すると、複数のアプリケーション間で、システム クリップボードにテキストをコピーして貼り付けることができます。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d0a984f0f3bf27447e250c12e38fd9adcfb0029f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802449"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials:クリップボードのトピック

**Clipboard** クラスを使用すると、テキストをシステム クリップボードにコピーして別のアプリケーションに貼り付けることができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-clipboard"></a>Clipboard の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

現在**クリップボード**に貼り付けることができるテキストがあるかどうかを確認するには:

```csharp
var hasText = Clipboard.HasText;
```

テキストを**クリップボード**に設定するには:

```csharp
await Clipboard.SetTextAsync("Hello World");
```

テキストを**クリップボード**から読み取るには:

```csharp
var text = await Clipboard.GetTextAsync();
```

クリップボードのコンテンツのいずれかが変更されるたびに、イベントがトリガーされます。

```csharp
public class ClipboardTest
{
    public ClipboardTest()
    {
        // Register for clipboard changes, be sure to unsubscribe when needed
        Clipboard.ClipboardContentChanged += OnClipboardContentChanged;
    }

    void OnClipboardContentChanged(object sender, EventArgs    e)
    {
        Console.WriteLine($"Last clipboard change at {DateTime.UtcNow:T}";);
    }
}
```

> [!TIP]
> クリップボードへのアクセスは、メイン ユーザー インターフェイス スレッドで行う必要があります。 メイン ユーザー インターフェイス スレッド上でメソッドを呼び出す方法については、[MainThread](~/essentials/main-thread.md) の API に関する記事をご覧ください。

## <a name="api"></a>API

- [Clipboard のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Clipboard)
- [Clipboard API のドキュメント](xref:Xamarin.Essentials.Clipboard)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
