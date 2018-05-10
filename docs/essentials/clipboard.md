---
title: Xamarin.Essentials クリップボード
description: クリップボード クラスを使用するテキストをアプリケーション間でシステム クリップボードにコピーして貼り付けることができます。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 44ba8090851b65327682b3d290d58fb23bb8aae4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials クリップボード

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
ClipBoard.SetText("Hello World");
```

テキストを読み取る、**クリップボード**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [クリップボードのソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [クリップボード API ドキュメント](xref:Xamarin.Essentials.Clipboard)
