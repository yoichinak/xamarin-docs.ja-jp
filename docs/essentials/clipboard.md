---
title: 'Xamarin.Essentials: クリップボード'
description: このドキュメントでは、テキストをアプリケーション間でシステム クリップボードにコピーして貼り付けることができる Xamarin.Essentials でクリップボード クラスについて説明します。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842616"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: クリップボード

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**クリップボード**クラスでは、テキストをアプリケーション間でシステム クリップボードにコピーして貼り付けてすることができます。

## <a name="using-clipboard"></a>クリップボードの使用方法

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

場合にチェックする、**クリップボード**がテキストの現在の貼り付けの準備完了。

```csharp
var hasText = Clipboard.HasText;
```

テキストを設定する、**クリップボード**:

```csharp
Clipboard.SetText("Hello World");
```

テキストを読み取る、**クリップボード**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [クリップボードのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [クリップボード API ドキュメント](xref:Xamarin.Essentials.Clipboard)
