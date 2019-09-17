---
title: Objective Sharpie とバインディングの作成
description: このセクションは、Objective Sharpie、Xamarin のコマンドライン ツールは、OBJECTIVE-C ライブラリへのバインドを作成するプロセスを自動化するために使用の概要を提供します。
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: conceptdev
ms.author: crdun
ms.date: 10/11/2017
ms.openlocfilehash: d5b9fa1edc09b831dbc69ab092dfb5270942e67a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765711"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Objective Sharpie とバインディングの作成

_このセクションは、Objective Sharpie、Xamarin のコマンドライン ツールは、OBJECTIVE-C ライブラリへのバインドを作成するプロセスを自動化するために使用の概要を提供します。_

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

バインディングは、完全一致と可能な限り徹底的なので、Objective Sharpie は Clang 解析のヘッダー ファイルを使用します。 これにより、品質のバインドを生成するためにかかる時間と労力を大幅に削減できます。

> [!IMPORTANT]
> 目標マジックペンは、経験豊富な Xamarin 開発者向けのツールです (拡張機能、C)。 目的の C ライブラリをバインドする前に、コマンドラインでネイティブライブラリを構築する方法についての十分な知識を持っている必要があります (また、ネイティブライブラリのしくみについてよく理解している必要があります)。

## <a name="history"></a>履歴

進化し、xamarin 過去 3 年間の Objective Sharpie を内部的に使用されているしました。 Objective Sharpie の運営、Api は、iOS、Mac OS X 10.10 を 8 以降、Xamarin.iOS および Xamarin.Mac で導入された、watchOS 2.0 された Objective Sharpie だけでブートス トラップします。 Xamarin に大きく依存 Objective Sharpie 内部的には、独自の製品を構築するためです。

ただし、目標マジックペンは非常に高度なツールであり、目的は C と C の高度な知識、コマンドラインでの clang コンパイラの使用方法、および一般的にはネイティブライブラリの配置方法を必要とします。 この高いバーが原因と、GUI を持つそのウィザードの設定、間違った予測を行い、あり、そのため、Objective Sharpie 現在のみコマンド ライン ツールとして使用できます。

## <a name="related-links"></a>関連リンク

- [Objective Sharpie ダウンロード](https://aka.ms/objective-sharpie)
- [チュートリアル: 目的 C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインディングの種類のリファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
