---
title: iOS の高度な概念と内部構造
description: このトピックでは、Monotouch.dialog API の設計、.NET 基底クラスライブラリ (BCL) のアセンブリとクラス、およびを Xcode の Interface Builder と Apple のツールチェーンと統合する方法について Visual Studio for Mac 説明します。
ms.prod: xamarin
ms.assetid: 951713CD-D6AD-981C-A09E-4F2C98588D8B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 4157e73379be2adc7c92b8cdbc05c4cc4489daec
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022319"
---
# <a name="ios-advanced-concepts-and-internals"></a>iOS の高度な概念と内部構造

_このトピックでは、Monotouch.dialog API の設計、.NET 基底クラスライブラリ (BCL) のアセンブリとクラス、およびを Xcode の Interface Builder と Apple のツールチェーンと統合する方法について Visual Studio for Mac 説明します。_

## <a name="api-designiosinternalsapi-designindexmd"></a>[API の設計](~/ios/internals/api-design/index.md)

API バインディングの背後にある設計原則について説明します。

## <a name="available-assembliescross-platforminternalsavailable-assembliesmd"></a>[使用できるアセンブリ](~/cross-platform/internals/available-assemblies.md)

.NET 基底クラスライブラリ (BCL) から使用できるアセンブリとクラスを一覧表示します。

## <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[XIB コードの生成](~/ios/internals/xib-code-generation.md)

また、Visual Studio for Mac と Xcode の Interface Builder で Interface Builder を使用して UI をデザインする方法についても説明します。

> [!IMPORTANT]
> このドキュメントでは、Xcode の Interface Builder のみとの統合 Visual Studio for Mac について説明します。 iOS Designer の詳細については、 [iOS designer](~/ios/user-interface/designer/index.md)のドキュメントを参照してください。

## <a name="ios-architectureiosinternalsarchitecturemd"></a>[iOS のアーキテクチャ](~/ios/internals/architecture.md)

Xamarin iOS アプリケーションは Mono 実行環境内で実行され、完全な事前 (AOT) コンパイルを使用しC#てコードを ARM アセンブリ言語にコンパイルします。 このガイドでは、低レベルでの Xamarin の詳細について説明します。

## <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[Objective-C セレクター](~/ios/internals/objective-c-selectors.md)

Objective-C セレクター (メソッド) を直接呼び出す場合の注意と使用方法。

## <a name="limitationslimitationsmd"></a>[制限事項](limitations.md)

Xamarin.iOS で認識できる落とし穴と制限。
