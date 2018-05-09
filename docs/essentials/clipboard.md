---
title: Xamarin.Essentials クリップボード
description: クリップボード クラスを使用するテキストをアプリケーション間でシステム クリップボードにコピーして貼り付けることができます。
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 218f90746f130f02c47040a9191b1001fa80c203
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
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
