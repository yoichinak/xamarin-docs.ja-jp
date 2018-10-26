---
title: .NET Embedding
description: .NET の埋め込みにより、既存の .NET コード (c#、f#、および他のユーザー) 他のプログラミング言語で記述されたコードによって使用されます。
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 23233ea8b06e0db580ba99edf2705e3dae5b931f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107145"
---
# <a name="net-embedding"></a>.NET Embedding

![[プレビュー]](~/media/shared/preview.png)

.NET の埋め込みにより、既存の .NET コード (c#、f#、および他のユーザー) を他のプログラミング言語から、さまざまな異なる環境で使用します。

つまり、既存の iOS アプリから使用する .NET ライブラリがあれば、行うことができます。   または、ネイティブの C++ ライブラリとリンクする場合は、行うこともできますです。   または、Java から .NET コードを使用します。

.NET の埋め込みがに基づいて、 [Embeddinator 4000](https://github.com/mono/Embeddinator-4000)オープン ソース プロジェクトです。

## <a name="environments-and-languages"></a>環境および言語

このツールは、どちらもそれを使用する言語と同様に、環境で使用するのに注意してください。   たとえば、ので、.NET の埋め込みは静的にコンパイル、.NET コードを iOS で使用できるネイティブ コードに、iOS プラットフォームにはジャストイン タイム (JIT) コンパイルはできません。  その他の環境は、JIT コンパイルを許可し JIT コンパイルすることを選択して、環境でします。

ターゲット言語でコードの慣用として .NET コードが表示されるように、さまざまな言語のコンシューマーをサポートします。   これは、現時点ではサポートされている言語の一覧を示します。

- [**Objective C** ](objective-c/index.md) – 慣用 Objective C Api への .NET のマッピング
- [**Java** ](android/index.md) – 慣用 Java Api への .NET のマッピング
- [**C** ](get-started/c.md) – C Api と同様のオブジェクト指向への .NET のマッピング

その他の言語は、後述します。

## <a name="getting-started"></a>作業の開始

開始するのには、現在サポートされている言語ごとに、これらのガイドのいずれかを確認します。

- [**Objective C** ](get-started/objective-c/index.md) – macOS と iOS について説明します
- [**Java** ](get-started/java/index.md) – macOS と Android について説明します
- [**C** ](get-started/c.md) – デスクトップ プラットフォームでの C 言語について説明します

## <a name="related-links"></a>関連リンク

- [GitHub の Embeddinator-4000](https://github.com/mono/Embeddinator-4000)
