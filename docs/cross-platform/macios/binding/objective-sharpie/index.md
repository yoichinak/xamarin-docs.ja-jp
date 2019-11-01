---
title: 目標マジックペンを使用したバインディングの作成
description: このセクションでは、目的の C ライブラリへのバインドを作成するプロセスを自動化するために使用される Xamarin のコマンドラインツールである、目標マジックペンの概要について説明します。
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: 4d6ab6cf48c5c365a4d8d05ef108a4d3a5d16134
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016192"
---
# <a name="creating-bindings-with-objective-sharpie"></a>目標マジックペンを使用したバインディングの作成

_このセクションでは、目的の C ライブラリへのバインドを作成するプロセスを自動化するために使用される Xamarin のコマンドラインツールである、目標マジックペンの概要について説明します。_

- [概要](#overview) & [履歴](#history)
- [はじめに](get-started.md)
- [ツールとコマンド](tools.md)
- [機能](platform/index.md)
- [例](examples/index.md)
- [チュートリアルを完了する](~/ios/platform/binding-objective-c/walkthrough.md)
- [リリース履歴](releases.md)

## <a name="overview"></a>概要

目標マジックペンは、バインドの最初のパスをブートストラップするためのコマンドラインツールです。
これは、ネイティブライブラリのヘッダーファイルを解析して、パブリック API を[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file)(以前に手動で実行したプロセス) にマップすることによって機能します。

目標マジックペンは Clang parse ヘッダーファイルを使用するため、バインディングはできるだけ正確で完全なものになります。 これにより、品質のバインドを生成するためにかかる時間と労力を大幅に削減できます。

> [!IMPORTANT]
> 目標マジックペンは、経験豊富な Xamarin 開発者向けのツールです (拡張機能、C)。 目的の C ライブラリをバインドする前に、コマンドラインでネイティブライブラリを構築する方法についての十分な知識を持っている必要があります (また、ネイティブライブラリのしくみについてよく理解している必要があります)。

## <a name="history"></a>歴史

過去3年間、Xamarin で内部的に目標のマジックペンを発展させ、使用しています。 目標マジックペンの力を証として、iOS 8、Mac OS X 10.10、および watchOS 2.0 が目標マジックペンと完全にブートストラップされていたため、Xamarin. iOS と Xamarin. Mac で導入された Api について説明します。 Xamarin は、独自の製品を構築するために、目標のマジックペンに大きく依存しています。

ただし、目標マジックペンは非常に高度なツールであり、目的は C と C の高度な知識、コマンドラインでの clang コンパイラの使用方法、および一般的にはネイティブライブラリの配置方法を必要とします。 この大まかな点から、GUI ウィザードを使用しても間違った期待を設定しています。そのため、目標マジックペンは現在、コマンドラインツールとしてのみ使用できます。

## <a name="related-links"></a>関連リンク

- [目標マジックペンのダウンロード](https://aka.ms/objective-sharpie)
- [チュートリアル: 目的 C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインディングの種類のリファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
