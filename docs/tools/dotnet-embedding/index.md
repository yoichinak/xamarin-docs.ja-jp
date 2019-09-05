---
title: .NET Embedding
description: .Net 埋め込みによって、既存C#のF#.net コード (、など) を他のプログラミング言語で記述されたコードで使用できるようになります。
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: af068e5a09cc11eec33508a4f2eb33186168aae6
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290224"
---
# <a name="net-embedding"></a>.NET Embedding

![[プレビュー]](~/media/shared/preview.png)

.NET の埋め込みにより、既存の .NET コード (C#、F#、および他のユーザー) を他のプログラミング言語から、さまざまな異なる環境で使用します。

つまり、既存の iOS アプリから使用する .NET ライブラリがある場合は、それを行うことができます。   また、ネイティブC++ライブラリとリンクする場合は、これを行うこともできます。   または Java から .NET コードを使用します。

.NET 埋め込みは、 [Embeddinator-4000](https://github.com/mono/Embeddinator-4000)オープンソースプロジェクトに基づいています。

## <a name="environments-and-languages"></a>環境と言語

ツールは、使用する環境と、それを使用する言語を認識しています。   たとえば、iOS プラットフォームではジャストインタイム (JIT) コンパイルが許可されていないため、.NET 埋め込みは、iOS で使用できるネイティブコードに .NET コードを静的にコンパイルします。  その他の環境では JIT コンパイルを許可し、そのような環境では JIT コンパイルを選択します。

さまざまな言語コンシューマーがサポートされるため、.NET コードは慣用的なコードとしてターゲット言語で公開されます。   現在サポートされている言語の一覧を次に示します。

- [**Objective C** ](objective-c/index.md) – 慣用 Objective-C API への .NET のマッピング
- [**Java** ](android/index.md) – 慣用 Java API への .NET のマッピング
- [**C** ](get-started/c.md) – C API と同様のオブジェクト指向への .NET のマッピング

その他の言語については後で説明します。

## <a name="getting-started"></a>作業の開始

使用を開始するには、現在サポートされている言語ごとに、次のいずれかのガイドをご確認ください。

- [**目的-C**](get-started/objective-c/index.md) -MacOS と iOS について説明します。
- [**Java**](get-started/java/index.md) – MacOS と Android について
- [**C**](get-started/c.md) –デスクトッププラットフォームの c 言語について説明します。

## <a name="related-links"></a>関連リンク

- [Embeddinator-4000 (GitHub)](https://github.com/mono/Embeddinator-4000)
