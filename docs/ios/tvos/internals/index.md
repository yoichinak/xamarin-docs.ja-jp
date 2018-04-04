---
title: tvOS の内部構造
description: Xamarin.tvOS は、iOS 製品用に作成して、高度なドキュメント、tvOS 製品とほぼ同じように、Xamarin.iOS 製品として同じ DNA を共有します。
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 83b8d5b6dc4e73f05160960f0e2547284de57799
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="tvos-internals"></a>tvOS の内部構造

_Xamarin.tvOS は、iOS 製品用に作成して、高度なドキュメント、tvOS 製品とほぼ同じように、Xamarin.iOS 製品として同じ DNA を共有します。_


##  <a name="assembliesiostvosinternalsassembliesmd"></a>[アセンブリ](~/ios/tvos/internals/assemblies.md)

Xamarin.tvOS アプリケーション for Xamarin でサポートされるアセンブリのリスト。

##  <a name="target-frameworksiostvosinternalsframeworksmd"></a>[ターゲット フレームワーク](~/ios/tvos/internals/frameworks.md)

この記事では、Xamarin.tvOS と Xamarin.tvOS アプリケーションの特定のターゲットを選択した場合の影響で使用可能なターゲット フレームワーク (基本クラス ライブラリ) の種類について説明します。

## <a name="related-ios-articles"></a>IOS の関連記事

次の記事は、(tvOS 9 が iOS 9 のサブセットであるため) は iOS に固有では、tvOS に関連します。

###  <a name="unified-apicross-platformmaciosunifiedindexmd"></a>[Unified API](~/cross-platform/macios/unified/index.md)

Apple TV と iOS の間で共有単純なコードが 64 ビット Api と 64 ビット コンパイルのサポートを導入およびコードベースを許可する新しい Unified Api が導入されています。  

###  <a name="api-designiosinternalsapi-designindexmd"></a>[API の設計](~/ios/internals/api-design/index.md)

バインディングは、API の背後にあるデザインの原則について説明します。

###  <a name="limitationsiosinternalslimitationsmd"></a>[制限事項](~/ios/internals/limitations.md)

このセクションでは、落とし穴とにおいては、Xamarin.iOS 多くは Xamarin.tvOS に適用されると認識する制限事項を示しています。

###  <a name="linkeriosdeploy-testlinkermd"></a>[リンカー](~/ios/deploy-test/linker.md)

設定と使用方法を変更する方法として、最小限の可能性のあるアプリケーション パッケージを確認する、リンカーのしくみについて説明します。

###  <a name="localization-and-internationalizationiosapp-fundamentalslocalizationindexmd"></a>[ローカリゼーションと国際化](~/ios/app-fundamentals/localization/index.md)

ここでは、国際化をサポートするために、Xamarin.iOS アプリケーションへのエンコーディングを追加します。

###  <a name="mtouchiosdeploy-testmtouchmd"></a>[mtouch](~/ios/deploy-test/mtouch.md)

iOS で使用できるアプリケーションにプロジェクトをビルドするコマンド ライン ツール mtouch.exe のノートと情報。

###  <a name="linking-native-librariesiosplatformnative-interopmd"></a>[ネイティブ ライブラリとリンク](~/ios/platform/native-interop.md)

Xamarin.iOS では、ネイティブの C ライブラリと Objective C ライブラリの両方を持つリンクをサポートします。 このドキュメントでは、Xamarin.iOS プロジェクトを含む、ネイティブの C ライブラリをリンクする方法について説明します。 Objective C ライブラリの作業を行うことについては、次を参照してください。、&nbsp; [Objective C 型のバインド](~/ios/platform/binding-objective-c/index.md)&nbsp;ドキュメント。

##  <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[Objective C セレクター](~/ios/internals/objective-c-selectors.md)

ノートと OBJECTIVE-C セレクター (メソッド) を直接呼び出すために使用します。

###  <a name="systemdataiosdata-cloudsystemdatamd"></a>[System.Data](~/ios/data-cloud/system.data.md)

情報と System.Data を使用して、組み込みの SQLite データベース システムにアクセスする手順。

###  <a name="threadingiosapp-fundamentalsthreadingmd"></a>[スレッド化](~/ios/app-fundamentals/threading.md)

Xamarin.iOS アプリケーション内でスレッドを使用するに関する注意事項です。

###  <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[XIB コードの生成](~/ios/internals/xib-code-generation.md)

Visual Studio for Mac 統合使用できるようにするインターフェイスのビルダーを使用して、UI の設計を Xcode のインターフェイスのビルダーを使用する方法。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
