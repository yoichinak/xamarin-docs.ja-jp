---
title: 'Xamarin.Essentials: クリップボード'
description: このドキュメントで説明する Xamarin.Essentials の Clipboard クラスを使用すると、テキストをシステム クリップボードにコピーして別のアプリケーションに貼り付けることができます。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8dd238da678dfb5773801137d313b286590aa463
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675537"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: クリップボード

![プレリリースの NuGet](~/media/shared/pre-release.png)

**Clipboard** クラスを使用すると、テキストをシステム クリップボードにコピーして別のアプリケーションに貼り付けることができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-clipboard"></a>Clipboard の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

現在**クリップボード**に貼り付けることができるテキストがあるかどうかを確認するには:

```csharp
var hasText = Clipboard.HasText;
```

テキストを**クリップボード**に設定するには:

```csharp
Clipboard.SetText("Hello World");
```

テキストを**クリップボード**から読み取るには:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Clipboard のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard API のドキュメント](xref:Xamarin.Essentials.Clipboard)
