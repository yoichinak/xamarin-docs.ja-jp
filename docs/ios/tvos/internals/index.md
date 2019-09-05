---
title: Xamarin の tvOS –内部
description: Xamarin での tvOS の内部動作について説明したドキュメント。 xamarin. iOS に基づいています。 リンクコンテンツでは、アセンブリ、ターゲットフレームワーク、および関連する iOS の概念について説明します。
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/07/2016
ms.openlocfilehash: ffcf4d3a491cb6ad865da35d387782b7bd1fca01
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283573"
---
# <a name="tvos-in-xamarin-internals"></a>Xamarin の tvOS –内部 

## <a name="assembliesiostvosinternalsassembliesmd"></a>[アセンブリ](~/ios/tvos/internals/assemblies.md)

Xamarin. tvOS アプリケーションの Xamarin でサポートされているアセンブリの一覧。

## <a name="target-frameworksiostvosinternalsframeworksmd"></a>[ターゲット フレームワーク](~/ios/tvos/internals/frameworks.md)

この記事では、tvOS で使用できるターゲットフレームワーク (基本クラスライブラリ) の種類と、tvOS アプリケーションの特定のターゲットを選択した場合の影響について説明します。

## <a name="related-ios-articles"></a>関連する iOS の記事

次の記事は iOS に固有ですが、tvOS に関連しています (tvOS 9 は iOS 9 のサブセットであるため)。

### <a name="unified-apicross-platformmaciosunifiedindexmd"></a>[Unified API](~/cross-platform/macios/unified/index.md)

では、Apple TV と iOS のコードベースの間でコードを簡単に共有できる新しい統合 Api が導入されています。また、64ビットの Api と64ビットのコンパイルのサポートも導入されています。  

### <a name="api-designiosinternalsapi-designindexmd"></a>[API の設計](~/ios/internals/api-design/index.md)

API バインディングの背後にある設計原則について説明します。

### <a name="limitationsiosinternalslimitationsmd"></a>[制限事項](~/ios/internals/limitations.md)

このセクションでは、Xamarin. iOS に関して注意する必要がある落とし穴と制限事項について説明します。その多くは tvOS に適用されます。

### <a name="linkeriosdeploy-testlinkermd"></a>[リンカー](~/ios/deploy-test/linker.md)

リンカーを使用して、可能な限り最小のアプリケーションパッケージを確認する方法、およびその設定と使用方法を変更する方法について説明します。

### <a name="localization-and-internationalizationiosapp-fundamentalslocalizationindexmd"></a>[ローカリゼーションと国際化](~/ios/app-fundamentals/localization/index.md)

このガイドでは、国際化をサポートするための Xamarin. iOS アプリケーションへのエンコーディングの追加について説明します。

### <a name="mtouchiosdeploy-testmtouchmd"></a>[mtouch](~/ios/deploy-test/mtouch.md)

iOS で使用できるアプリケーションにプロジェクトをビルドするコマンド ライン ツール mtouch.exe のノートと情報。

### <a name="linking-native-librariesiosplatformnative-interopmd"></a>[ネイティブライブラリのリンク](~/ios/platform/native-interop.md)

Xamarin. iOS は、ネイティブ C ライブラリと目的 C ライブラリの両方を使用したリンクをサポートします。 このドキュメントでは、ネイティブ C ライブラリを Xamarin. iOS プロジェクトにリンクする方法について説明します。 目的 c ライブラリについても同様に説明します。詳細&nbsp;については、「[バインディングの目的-c の型](~/ios/platform/binding-objective-c/index.md)&nbsp;」を参照してください。

## <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[目標-C セレクター](~/ios/internals/objective-c-selectors.md)

目的 C セレクター (メソッド) を直接呼び出す場合の注意と使用方法。

### <a name="systemdataiosdata-cloudsystemdatamd"></a>[System.Data](~/ios/data-cloud/system.data.md)

システムデータを使用して組み込みの SQLite データベースシステムにアクセスする方法について説明します。

### <a name="threadingiosapp-fundamentalsthreadingmd"></a>[スレッド化](~/ios/app-fundamentals/threading.md)

Xamarin アプリケーション内でスレッド処理を使用する場合に注意してください。

### <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[XIB コードの生成](~/ios/internals/xib-code-generation.md)

Visual Studio for Mac を Xcode の Interface Builder と統合して、Interface Builder を使用して UI をデザインする方法について説明します。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
