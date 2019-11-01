---
title: .NET Embedding
description: .Net 埋め込みによって、既存C#のF#.net コード (、など) を他のプログラミング言語で記述されたコードで使用できるようになります。
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 5a0e7eeaee9b3189de63d0b82a3822cc68023505
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006943"
---
# <a name="net-embedding"></a>.NET Embedding

![[プレビュー]](~/media/shared/preview.png)

.Net 埋め込みでは、既存のC#.net F#コード (、など) を他のプログラミング言語やさまざまな環境で使用できます。

つまり、既存の iOS アプリから使用する .NET ライブラリがある場合は、それを行うことができます。   また、ネイティブC++ライブラリとリンクする場合は、これを行うこともできます。   または Java から .NET コードを使用します。

.NET 埋め込みは、 [Embeddinator-4000](https://github.com/mono/Embeddinator-4000)オープンソースプロジェクトに基づいています。

## <a name="environments-and-languages"></a>環境と言語

ツールは、使用する環境と、それを使用する言語を認識しています。   たとえば、iOS プラットフォームではジャストインタイム (JIT) コンパイルが許可されていないため、.NET 埋め込みは、iOS で使用できるネイティブコードに .NET コードを静的にコンパイルします。  その他の環境では JIT コンパイルを許可し、そのような環境では JIT コンパイルを選択します。

さまざまな言語コンシューマーがサポートされるため、.NET コードは慣用的なコードとしてターゲット言語で公開されます。   現在サポートされている言語の一覧を次に示します。

- [**目的-c**](objective-c/index.md) – .net から慣用的なへのマッピング-c api
- [**Java**](android/index.md) – .net から慣用的な Java api へのマッピング
- [**C**](get-started/c.md) – .Net を c api のようなオブジェクト指向にマッピングする

その他の言語については後で説明します。

## <a name="getting-started"></a>作業の開始

使用を開始するには、現在サポートされている言語ごとに、次のいずれかのガイドをご確認ください。

- [**目的-C**](get-started/objective-c/index.md) -MacOS と iOS について説明します。
- [**Java**](get-started/java/index.md) – MacOS と Android について
- [**C**](get-started/c.md) –デスクトッププラットフォームの c 言語について説明します。

## <a name="related-links"></a>関連リンク

- [Embeddinator-4000 (GitHub)](https://github.com/mono/Embeddinator-4000)
