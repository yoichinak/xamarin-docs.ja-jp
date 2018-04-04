---
title: .NET Embedding
description: .NET の埋め込みを使用 (c#、f#、および他のユーザー) を他のプログラミング言語から使用する、既存の .NET コード
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f5e778ef9ba31c1a9e880b9fc66c2e48ddb2420c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="net-embedding"></a>.NET Embedding

![[プレビュー]](~/media/shared/preview.png)

.NET の埋め込み (c#、f#、および他のユーザー) を他のプログラミング言語から、さまざまな異なる環境で使用する既存の .NET コードを許可します。

つまり、ある場合、既存の iOS アプリから使用する .NET ライブラリがある場合は、することができます。   または、ネイティブ C++ ライブラリとリンクする場合は、行うことができますもです。   または、Java から .NET コードを使用します。

## <a name="environments-and-languages"></a>環境および言語

このツールは、どちらもこれを使用する言語と同様に、環境を使用するのに注意してください。   たとえば、embeddinator iOS で使用できるネイティブ コードに .NET コードのコンパイルに静的には、iOS プラットフォームはジャスト イン タイム (JIT) コンパイルを許可されません。  他の環境は JIT コンパイルを許可し JIT コンパイルすることを選択、これらの環境でします。

対象言語の慣用コードとして .NET コードを明らかにさまざまな言語は、コンシューマーがサポートされます。   これは、現時点ではサポートされている言語の一覧を示します。

- [**Objective C** ](objective-c/index.md) – .NET の慣用 Objective C Api にマッピングします。
- [**Java** ](android/index.md) – .NET の慣用 Java Api にマッピングします。
- **C**: .NET に、オブジェクト指向 C Api と同様にマップします。

その他の言語は、後述します。

## <a name="getting-started"></a>作業の開始

、作業を開始するには、現在サポートされている言語ごとに、これらのガイドのいずれかを確認します。

- [**Objective C** ](get-started/objective-c/index.md) – macOS と iOS について説明します。
- [**Java** ](get-started/java/index.md) – macOS と Android について説明します。
- [**C** ](get-started/c.md) – デスクトップ プラットフォームでの C 言語について説明します。


## <a name="related-links"></a>関連リンク

- [Github の Embeddinator 4000](https://github.com/mono/Embeddinator-4000)
