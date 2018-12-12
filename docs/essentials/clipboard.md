---
title: 'Xamarin.Essentials: クリップボード'
description: このドキュメントで説明する Xamarin.Essentials の Clipboard クラスを使用すると、テキストをシステム クリップボードにコピーして別のアプリケーションに貼り付けることができます。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 90ede9d0d0fbee9efabcce25c0ae7c3c439d9e69
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898704"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: クリップボード

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
await Clipboard.SetTextAsync("Hello World");
```

テキストを**クリップボード**から読み取るには:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Clipboard のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard API のドキュメント](xref:Xamarin.Essentials.Clipboard)
