---
title: 'Xamarin.Essentials: クリップボード'
description: このドキュメントでは、テキストをアプリケーション間でシステム クリップボードにコピーして貼り付けるできます Xamarin.Essentials でクリップボード クラスについて説明します。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782360"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: クリップボード

![プレリリース NuGet](~/media/shared/pre-release.png)

**クリップボード**クラスを使用してテキストをアプリケーション間でシステム クリップボードにコピーして貼り付けます。

## <a name="using-clipboard"></a>クリップボードの使用方法

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

かどうか確認する、**クリップボード**を貼り付ける準備が現在のテキストを含んでいます。

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
