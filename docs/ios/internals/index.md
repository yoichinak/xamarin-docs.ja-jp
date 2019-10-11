---
title: iOS の高度な概念と内部構造
description: このトピックでは、Monotouch.dialog API の設計、.NET 基底クラスライブラリ (BCL) のアセンブリとクラス、およびを Xcode の Interface Builder と Apple のツールチェーンと統合する方法について Visual Studio for Mac 説明します。
ms.prod: xamarin
ms.assetid: 951713CD-D6AD-981C-A09E-4F2C98588D8B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: 4f6043190087d34ccaa4a63fcc801843194273ad
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291894"
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
> このドキュメントでは、Visual Studio for Mac と Xcode の Interface Builder との統合についてのみ説明します。 iOS Designer の詳細については、 [iOS Designer](~/ios/user-interface/designer/index.md)ドキュメントを参照してください。

## <a name="ios-architectureiosinternalsarchitecturemd"></a>[iOS のアーキテクチャ](~/ios/internals/architecture.md)

Xamarin.iOS アプリケーションは Mono 実行環境内で実行され、完全な事前 (AOT) コンパイルを使用して C# コードを ARM アセンブリ言語にコンパイルします。 このガイドでは、低レベルでの Xamarin.iOS の詳細について説明します。

## <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[Objective-C セレクター](~/ios/internals/objective-c-selectors.md)

Objective-C セレクター (メソッド) を直接呼び出す場合の注意と使用方法。

## <a name="limitationslimitationsmd"></a>[制限事項](limitations.md)

Xamarin. iOS で認識できる落とし穴と制限。
