---
title: Xamarin – 内部で tvOS
description: Xamarin.iOS に基づく、Xamarin で tvOS の内部動作を説明するドキュメント。 コンテンツのリンクは、アセンブリ、ターゲットのフレームワークについて説明し、iOS の概念に関連します。
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: 3eca425e38a01053f084ddbc5ad2edb93f6f6427
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122960"
---
# <a name="tvos-in-xamarin-internals"></a>Xamarin – 内部で tvOS 

##  <a name="assembliesiostvosinternalsassembliesmd"></a>[アセンブリ](~/ios/tvos/internals/assemblies.md)

Xamarin.tvOS アプリケーション for Xamarin でサポートされるアセンブリのリスト。

##  <a name="target-frameworksiostvosinternalsframeworksmd"></a>[ターゲット フレームワーク](~/ios/tvos/internals/frameworks.md)

この記事では、Xamarin.tvOS と Xamarin.tvOS アプリケーションの特定のターゲットを選択した場合の影響で使用可能なターゲット フレームワーク (基本クラス ライブラリ) の種類について説明します。

## <a name="related-ios-articles"></a>IOS の関連記事

次の記事は iOS に固有で tvOS に関連する (tvOS 9 が iOS 9 のサブセットであるため) です。

###  <a name="unified-apicross-platformmaciosunifiedindexmd"></a>[Unified API](~/cross-platform/macios/unified/index.md)

Apple TV、iOS の間で共有する簡単なコードが Api の 64 ビットと 64 ビット コンパイルのサポートの概要とコードベースを許可する新しい Unified Api が導入されています。  

###  <a name="api-designiosinternalsapi-designindexmd"></a>[API の設計](~/ios/internals/api-design/index.md)

API のバインドの背後にあるデザインの原則について説明します。

###  <a name="limitationsiosinternalslimitationsmd"></a>[制限事項](~/ios/internals/limitations.md)

このセクションでは、落とし穴とに関して、Xamarin.iOS Xamarin.tvOS に適用されていますで注意すべき制限事項を説明します。

###  <a name="linkeriosdeploy-testlinkermd"></a>[リンカー](~/ios/deploy-test/linker.md)

設定と使用方法を変更する方法についても、最小限のアプリケーション パッケージを確実に、リンカーの動作方法について説明します。

###  <a name="localization-and-internationalizationiosapp-fundamentalslocalizationindexmd"></a>[ローカリゼーションと国際化](~/ios/app-fundamentals/localization/index.md)

このガイドでは、エンコーディングを国際化をサポートするために Xamarin.iOS アプリケーションの追加について説明します。

###  <a name="mtouchiosdeploy-testmtouchmd"></a>[mtouch](~/ios/deploy-test/mtouch.md)

iOS で使用できるアプリケーションにプロジェクトをビルドするコマンド ライン ツール mtouch.exe のノートと情報。

###  <a name="linking-native-librariesiosplatformnative-interopmd"></a>[ネイティブ ライブラリのリンク](~/ios/platform/native-interop.md)

Xamarin.iOS では、ネイティブの C ライブラリと OBJECTIVE-C ライブラリの両方とのリンクをサポートしています。 このドキュメントでは、Xamarin.iOS プロジェクト、ネイティブの C ライブラリにリンクする方法について説明します。 OBJECTIVE-C ライブラリの作業を行うことについては、次を参照してください。、&nbsp; [Objective-c のバインド型](~/ios/platform/binding-objective-c/index.md)&nbsp;ドキュメント。

##  <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)

メモと OBJECTIVE-C セレクター (メソッド) を直接呼び出すために使用します。

###  <a name="systemdataiosdata-cloudsystemdatamd"></a>[System.Data](~/ios/data-cloud/system.data.md)

情報と System.Data を使用して、組み込みの SQLite データベース システムにアクセスする手順。

###  <a name="threadingiosapp-fundamentalsthreadingmd"></a>[スレッド化](~/ios/app-fundamentals/threading.md)

Xamarin.iOS アプリケーション内でスレッドを使用してメモします。

###  <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[XIB コードの生成](~/ios/internals/xib-code-generation.md)

どの Visual Studio for Mac は、Interface Builder を UI の設計に使用できるように、Xcode の Interface Builder と統合します。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
