---
title: Xamarin.Mac のパフォーマンス
description: このドキュメントでは、Xamarin.Mac アプリのパフォーマンスに関するいくつかの考慮事項について説明します。
ms.prod: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 273fb1980695a3dcd4a4fc2123b1ebafef1756a2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-performance"></a>Xamarin.Mac のパフォーマンス

## <a name="overview"></a>概要

Xamarin.Mac のアプリケーションは、Xamarin.iOS に似ており、次のような同じパフォーマンスに関する推奨事項の多くが適用されます。

- [Xamarin.iOS のパフォーマンス](~/ios/deploy-test/performance.md)
- [クロスプラットフォームのパフォーマンス](~/cross-platform/deploy-test/memory-perf-best-practices.md)

ただし、macOS 固有の推奨事項で役に立つものも多くあります。

## <a name="prefer-modern-target-framework"></a>最新のターゲット フレームワークが優先される

Xamarin.Mac アプリケーションには、パフォーマンスの特性と機能が異なる[ターゲット フレームワーク](~/mac/platform/target-framework.md)が複数あります。

可能な限り最新のものを優先し、依存ライブラリを使用してサポートを追加します。 最新のターゲット フレームワークのみが、アセンブリのサイズを大幅に減らすリンクの設定をすることができます。 これは AOT を有効にする場合に特に重要です。AOT の完全アセンブリのコンパイルが大規模な最終バンドルを生成できるからです。

## <a name="enable-the-linker"></a>リンカーを有効にする

読み込みでも "Just-In-Time" (JIT) でも、起動時間は最終的なバイナリのサイズに応じて線形的にある程度延長されます。 これを改善する最も簡単な方法が、[リンカー](~/mac/deploy-test/linker.md)で実行されないコードを削除することです。

この推奨事項は主に最新のターゲット フレームワークのユーザーに適用されますが、[プラットフォーム リンク](~/mac/deploy-test/linker.md)の使用により、制限付きでパフォーマンスを改善することもできます。

## <a name="enable-aot-when-appropriate"></a>必要に応じて AOT を有効にする

起動時のパフォーマンスの別の側面が、コンピューター語コードへのアセンブリの JIT コンパイルです。 Ahead Of Time (AOT) コンパイルでは起動時間を大幅に短縮できますが、多くのマイナス面もあります。これについては [AOT のドキュメント](~/mac/internals/aot.md)で説明しています。

## <a name="ensure-performant-delegates"></a>パフォーマンスの高いデリゲートを使用する

多くの Xamarin.Mac アプリケーションは、`NSCollectionView`、`NSOutlineView`、`NSTableView` などの Cocoa ビューを中心に展開されています。 多くの場合、これらのビューは Cocoa に提供する `Delegate` クラスと `DataSource` クラスを使用して、表示するものは何かという質問に答えます。

これらのエントリ ポイントの多くが頻繁に呼び出され、スクロール時には 1 秒間に複数回呼び出されることもあります。

ユーザー インターフェイスがブロックされないように、簡単に計算される値を関数が返すようにしたり、既にキャッシュされている情報が使用されたりするようにしてください。

## <a name="use-cocoa-provided-apis-for-reusing-views"></a>ビューの再利用に Cocoa が提供する API を使用する

多くの子ビューまたはセル (`NSCollectionView`、`NSOutlineView`、`NSTableView` など) が含まれる多くの Cocoa ビューは、ビューの作成および再利用のための API を提供します。 この API によって共有項目のプールが作成され、ビューを速くスクロールする場合のパフォーマンスの問題が回避されます。

## <a name="use-async-and-do-not-block-the-ui"></a>非同期を使用し、UI をブロックしない

デスクトップ アプリケーションは大量のデータを処理することが多いため、同期操作を待機している UI スレッドが簡単にブロックされてしまいます。

可能な限り、[非同期](~/cross-platform/platform/async.md)とスレッドを使用し、UI がブロックされないようにします。

長時間実行される操作では、Apple の [HIG](https://developer.apple.com/macos/human-interface-guidelines/indicators/progress-indicators/) に記載されている [NSProgressIndicator](https://developer.xamarin.com/samples/mac/ProgressBarExample/) やその他のオプションを使用してユーザーに通知することを検討してください。


## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのパフォーマンス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Xamarin.iOS のパフォーマンス](~/ios/deploy-test/performance.md)
