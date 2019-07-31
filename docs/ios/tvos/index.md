---
title: Xamarin での tvOS の概要
description: このドキュメントでは、Xamarin を使用して tvOS アプリをビルドする方法を示すさまざまなガイドとサンプルにリンクしています。 このガイドでは、ユーザーインターフェイスの開発、データストレージ、アイコンなど、さまざまな機能について説明します。
ms.prod: xamarin
ms.assetid: 14345503-1742-41F5-B2EF-EE31AB7C3516
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 02/02/2018
ms.openlocfilehash: b6bb036962522e83144843e38b1aa320e6145fda
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648054"
---
# <a name="introduction-to-tvos-in-xamarin"></a>Xamarin での tvOS の概要

## <a name="introducing-tvos"></a>TvOS の概要

Apple は、iOS 11 に基づいて最新バージョンの tvOS オペレーティングシステムを実行している apple tv ハードウェア (Apple TV 4K) の5世代をリリースしました。

Apple TV platform は開発者にオープンであり、充実したイマーシブアプリを作成し、Apple TV の組み込みの App Store でリリースできます。

TvOS の詳細については、[はじめに](~/ios/tvos/get-started/index.md)のドキュメントを参照してください。

> [!VIDEO https://youtube.com/embed/Q04oIYymfGM]

**tvOS と Xamarin ビデオ**

## <a name="documentation"></a>ドキュメント

次のドキュメントは、Xamarin を使用して tvOS アプリのビルドを開始する際に役立ちます。

- [TvOS 11 の概要](~/ios/tvos/platform/introduction-to-tvos11.md)-この記事では、tvOS 11 で使用できる tvOS 開発者向けの新機能について説明します。
- [TvOS 10 の概要](~/ios/tvos/platform/introduction-to-tvos10/index.md)-この記事では、tvOS 10 で使用できるすべての新しい api と変更された api と機能について、tvOS 開発者向けに紹介します。
- [TvOS 9 の概要](~/ios/tvos/platform/tvos9.md)–この記事では、tvOS 9 で使用できるすべての新しい api と変更された api と機能について、tvOS 開発者向けに紹介します。 
- [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)–このガイドでは、最初の tvOS アプリを作成する手順について説明します。このプロセスでは、Visual Studio for Mac、Xcode、Interface Builder などの開発ツールチェーンが導入されています。 また、UI コントロールをコードに公開するアウトレットとアクションについても説明します。最後に、tvOS アプリケーションをビルド、実行、テストする方法についても説明します。
- [アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)-この記事では、tvOS アプリ内のアイコンとイメージのデザインと操作について説明します。
- [ナビゲーションとフォーカスの操作](~/ios/tvos/app-fundamentals/navigation-focus.md)-この記事では、フォーカスの概念と、tvOS アプリ内でのナビゲーションの表示と処理に使用する方法について説明します。
- [リソースとデータストレージ](~/ios/tvos/app-fundamentals/resources-data-storage.md)-この記事では、tvOS アプリでのリソースと永続データストレージの使用について説明します。
- [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md) -この記事では、tvOS アプリでの新しい Siri リモートおよび bluetooth ゲームコントローラーのサポートについて説明します。
- [ユーザーインターフェイス](~/ios/tvos/user-interface/index.md)–ユーザーインターフェイス (UI) コントロールを含む一般的なユーザーエクスペリエンス (ux) には、tvOS を操作するときに Xcode の INTERFACE BUILDER と UX の設計原則を使用します。
- [デプロイ、テスト、およびメトリック](~/ios/tvos/deploy-test/index.md): このセクションでは、アプリのテストと配布方法に関するトピックについて説明します。 このトピックには、デバッグに使用するツール、テスト担当者へのデプロイ、Apple TV App Store にアプリケーションを発行する方法などが含まれます。
- [サポートされているアセンブリ](~/ios/tvos/internals/assemblies.md)– Xamarin. tvOS アプリの xamarin でサポートされているアセンブリの一覧です。
- [サポートされているフレームワークとサポートされていないフレームワーク](~/ios/tvos/internals/frameworks.md): Xamarin. tvOS アプリ用に xamarin でサポートされているフレームワークの一覧です。

## <a name="sample-projects"></a>サンプル プロジェクト

Xamarin でビルドされた tvOS アプリのサンプル:

- [Hello, tvOS](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-hello-tvos) –このサンプルでは、tvOS の単純な "Hello World" アプリを実装し、tvOS の使用方法の基本を説明します。
- [Tvalerts](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvalerts) –このサンプルでは、tvOS アプリでアラートを操作する方法を示します。
- [tvButtons](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvbuttons) –このサンプルでは、ボタンの操作方法を示しています。 tvOS アプリです。
- [tvRemote](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvremote) –このサンプルでは、tvOS アプリが Siri リモートと対話してユーザーインターフェイスを移動できるいくつかの方法を示します。
- [tvCollection](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvcollection) –このサンプルでは、tvOS アプリでコレクションビューコントローラーを操作する方法を示します。
- [tvNavBars](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvnavbars) –このサンプルでは、tvOS アプリのナビゲーションバーを操作する方法を示します。
- [tvPages](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvpages) –このサンプルでは、tvOS アプリでページコントロールを操作する方法を示します。
- [tvProgress](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvprogress) –このサンプルでは、tvOS アプリで進行状況インジケーターを操作する方法を示します。
- [tvSplit](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvsplit) –このサンプルでは、tvOS アプリで分割ビューコントローラーを操作する方法を示します。
- [tvStackView](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvstackview) -このサンプルでは、tvOS アプリでスタックビューを操作する方法を示します。
- [UICatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-uicatalog) – tvOS で、uikit フレームワークの多くのビューとコントロールを使用する方法を示します。 システムによって提供される特定のコントロールまたはビューを検索する場合は、このサンプルを参照してください。

さらに、Apple は、tvOS アプリの Xamarin のサポートをC#使用するために、にトランスコードできる次のサンプルアプリを提供しています。

- [DemoBots:SpriteKit とゲームプレイキットを使用したクロスプラットフォームゲームの構築](https://developer.apple.com/library/prerelease/tvos/samplecode/DemoBots/)

## <a name="known-issues-and-troubleshooting"></a>既知の問題とトラブルシューティング

Xamarin で tvOS を構築するときに問題が発生した場合は、[リリースノート](https://docs.microsoft.com/xamarin/ios/release-notes/)、 [Xamarin、iOS フォーラム](https://forums.xamarin.com/categories/ios)、 [xamarin Bugzilla Tracker](https://bugzilla.xamarin.com/query.cgi?product=iOS)、 [GitHub](https://github.com/xamarin/xamarin-macios/issues)で既存の問題を確認してください。

[GitHub で](https://github.com/xamarin/xamarin-macios/issues)新しい問題と提案を報告します。


## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin を使用した tvOS 用アプリの構築 (ビデオ)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
