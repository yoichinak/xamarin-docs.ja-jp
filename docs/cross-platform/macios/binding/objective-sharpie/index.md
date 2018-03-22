---
title: "目標ペンを使わず"
description: "このセクションで説明目的ペンを使わず、Objective C ライブラリへのバインドを作成するプロセスを自動化するため、Xamarin のコマンド ライン ツールの概要"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 08796d38301ba9c6ae15c3ddfe700b1cd2166ace
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="objective-sharpie"></a>目標ペンを使わず

_このセクションで説明目的ペンを使わず、Objective C ライブラリへのバインドを作成するプロセスを自動化するため、Xamarin のコマンド ライン ツールの概要_

- [概要](#overview) & [履歴](#history)
- [はじめに](get-started.md)
- [ツールとコマンド](tools.md)
- [機能](platform/index.md)
- [例](examples/index.md)
- [チュートリアルを完了します。](~/ios/platform/binding-objective-c/walkthrough.md)
- [リリース履歴](releases.md)

## <a name="overview"></a>概要

目標ペンを使わずは、ブートス トラップ バインディングの最初のパスを支援するコマンド ライン ツールです。
パブリック API をマップするネイティブ ライブラリのヘッダー ファイルを解析して、それと、[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file)(以前は手動で実行する処理)。

目標ペンを使わず Clang 解析ヘッダー、ファイルを使用するバインディングは、完全一致と完全可能で、します。 これは、時間と品質のバインドを生成するためにかかる労力に大幅に減らすことができます。

> [!IMPORTANT]
> 目標ペンを使わず、Objective C の (および C の拡張機能によって) 高度な知識と経験を積んだ開発者向けツールです。 Objective C ライブラリをバインドする前に、コマンドライン (およびのネイティブ ライブラリのしくみをよく理解) ネイティブ ライブラリを構築する方法を十分に理解が必要です。

## <a name="history"></a>履歴

おがされて進化ありを使用目的ペンを使わず内部的に Xamarin で最後の 3 年間です。 目標ペンを使わずのべき乗に運営、Api は、iOS、Mac OS X 10.10 8 以降、Xamarin.iOS および Xamarin.Mac で導入されたおよび watchOS 2.0 が完全目標ペンを使わずにブートス トラップします。 Xamarin に大きく依存して目標ペンを使わず内部的には、独自の製品をビルドするためです。

ただし、目的のペンを使わず、Objective C、C、clang コンパイラ、コマンドラインを使用する方法および一般にネイティブ ライブラリをまとめるの高度な知識を必要とする非常に高度なツールです。 この高いバーが原因とフル インストールを所有するウィザードで、間違った期待値を設定、そのため、目標ペンを使わず現在のみコマンド ライン ツールとして使用できます。

## <a name="related-links"></a>関連リンク

- [目標のペンを使わずダウンロード](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [チュートリアル: バインド Objective C ライブラリ](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインディングの種類のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
