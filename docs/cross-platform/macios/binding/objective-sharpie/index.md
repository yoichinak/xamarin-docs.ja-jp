---
title: Objective Sharpie とバインディングの作成
description: このセクションは、Objective Sharpie、Xamarin のコマンドライン ツールは、OBJECTIVE-C ライブラリへのバインドを作成するプロセスを自動化するために使用の概要を提供します。
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: c298d3dc772929a123b6b65b0efe929f023a6563
ms.sourcegitcommit: 7dbfac5f68c55f40dd726aa1b03cff62082fe711
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67866353"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Objective Sharpie とバインディングの作成

_このセクションは、Objective Sharpie、Xamarin のコマンドライン ツールは、OBJECTIVE-C ライブラリへのバインドを作成するプロセスを自動化するために使用の概要を提供します。_

- [概要](#overview) & [履歴](#history)
- [はじめに](get-started.md)
- [ツールとコマンド](tools.md)
- [機能](platform/index.md)
- [例](examples/index.md)
- [チュートリアルを完了します。](~/ios/platform/binding-objective-c/walkthrough.md)
- [リリース履歴](releases.md)

## <a name="overview"></a>概要

油性の目標は、バインディングの最初のパスをブートス トラップするコマンド ライン ツールです。
パブリック API をマップするネイティブ ライブラリのヘッダー ファイルを解析することによって動作、[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file)(以前実行された手動でプロセス)。

バインディングは、完全一致と可能な限り徹底的なので、Objective Sharpie は Clang 解析のヘッダー ファイルを使用します。 これは、時間と品質のバインドを生成するためにかかる労力に大幅に削減できます。

> [!IMPORTANT]
> 油性の目標は、OBJECTIVE-C の (および C の拡張機能によって) 高度な知識と経験豊富な Xamarin 開発者向けツールです。 Objective C ライブラリをバインドしようとする前に、コマンドライン (およびネイティブ ライブラリの動作をよく理解) でネイティブ ライブラリを構築する方法を十分に理解が必要です。

## <a name="history"></a>履歴

進化し、xamarin 過去 3 年間の Objective Sharpie を内部的に使用されているしました。 Objective Sharpie の運営、Api は、iOS、Mac OS X 10.10 を 8 以降、Xamarin.iOS および Xamarin.Mac で導入された、watchOS 2.0 された Objective Sharpie だけでブートス トラップします。 Xamarin に大きく依存 Objective Sharpie 内部的には、独自の製品を構築するためです。

ただし、目的の油性は Objective C、C、コマンドラインで、clang コンパイラを使用する方法および一般的にどのネイティブ ライブラリがまとめての高度な知識を必要とする非常に高度なツールです。 この高いバーが原因と、GUI を持つそのウィザードの設定、間違った予測を行い、あり、そのため、Objective Sharpie 現在のみコマンド ライン ツールとして使用できます。

## <a name="related-links"></a>関連リンク

- [Objective Sharpie ダウンロード](https://aka.ms/objective-sharpie)
- [チュートリアル: OBJECTIVE-C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインドの種類のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)

