---
title: .NET Embedding
description: .NET の埋め込み (c#、f#、および他のユーザー) 他のプログラミング言語で記述されたコードが消費できるように、既存の .NET コードを許可します。
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 16f59498a49d10a43e04989136d8835bf78bd89d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793122"
---
# <a name="net-embedding"></a>.NET Embedding

![[プレビュー]](~/media/shared/preview.png)

.NET の埋め込み (c#、f#、および他のユーザー) を他のプログラミング言語から、さまざまな異なる環境で使用する既存の .NET コードを許可します。

つまり、ある場合、既存の iOS アプリから使用する .NET ライブラリがある場合は、することができます。   または、ネイティブ C++ ライブラリとリンクする場合は、行うことができますもです。   または、Java から .NET コードを使用します。

.NET の埋め込みがに基づいて、 [Embeddinator 4000](https://github.com/mono/Embeddinator-4000)オープン ソース プロジェクトです。

## <a name="environments-and-languages"></a>環境および言語

このツールは、どちらもこれを使用する言語と同様に、環境を使用するのに注意してください。   たとえば、ので、.NET の埋め込みは静的にコンパイルに .NET コードを iOS で使用できるネイティブ コードに、iOS プラットフォームはジャスト イン タイム (JIT) コンパイルを許可されません。  他の環境は JIT コンパイルを許可し JIT コンパイルすることを選択、これらの環境でします。

対象言語の慣用コードとして .NET コードを明らかにさまざまな言語は、コンシューマーがサポートされます。   これは、現時点ではサポートされている言語の一覧を示します。

- [**Objective C** ](objective-c/index.md) – 慣用 Objective C Api への .NET のマッピング
- [**Java** ](android/index.md) – 慣用 Java Api への .NET のマッピング
- [**C** ](get-started/c.md) – C Api などのオブジェクト指向に .NET のマッピング

その他の言語は、後述します。

## <a name="getting-started"></a>作業の開始

、作業を開始するには、現在サポートされている言語ごとに、これらのガイドのいずれかを確認します。

- [**Objective C** ](get-started/objective-c/index.md) – macOS と iOS について説明します
- [**Java** ](get-started/java/index.md) – macOS と Android について説明します
- [**C** ](get-started/c.md) – デスクトップ プラットフォームでの C 言語の説明

## <a name="related-links"></a>関連リンク

- [Github の Embeddinator 4000](https://github.com/mono/Embeddinator-4000)
