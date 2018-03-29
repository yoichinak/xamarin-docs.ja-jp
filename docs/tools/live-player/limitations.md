---
title: 制限事項
description: Xamarin Live プレーヤーのいくつかの制限
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: 71d37b79c962123bb5adf6f999e713f68086166c
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2018
---
# <a name="limitations"></a>制限事項

![プレビュー機能](~/media/shared/preview.png)

## <a name="device-requirements"></a>デバイスの要件
Xamarin Player のライブ アプリには、次のデバイスがサポートされています。

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 以降。
- ARM-v7a、v8a ARM、ARM64 v8a、x86、または x86_64 プロセッサ。

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 以降。
- ARM64 プロセッサ。

-----

## <a name="limitations"></a>制限事項

Xamarin Player のライブ実行できますが、次の項目を含むものにいくつかの制限があります。

### <a name="xamarinforms"></a>Xamarin.Forms
- カスタム レンダラーがサポートされていません。
- 効果はサポートされていません。
- カスタム バインド可能なプロパティを持つカスタム コントロールはサポートされていません。
- 埋め込みリソースはサポートされていません (ie。、PCL に画像やその他のリソースを埋め込む)。
- サード パーティ製 MVVM フレームワークが (ie サポートされていません。プリズム Mvvm 間、Mvvm Light, など)。
- IOS での資産カタログを指定することはできません。

### <a name="other-project-types"></a>その他のプロジェクトの種類
- ライブ Player は、ネイティブの Android または iOS プロジェクトを使用する Android XML またはストーリー ボードのユーザー インターフェイス) のものではありません。

### <a name="misc"></a>[その他]
- リフレクションの制限付きサポート (現在、SQLite、Json.NET のように、いくつかの一般的な NuGets に影響します)。 その他の NuGets をサポートすることも可能性があります。
- 一部のシステム クラスをオーバーライドすることはできません (たとえば、サブクラスを実装することはできません)。
- (ただしに構成されているフォト ギャラリーのアクセスなどの一般的な操作の) Live プレーヤーの Xamarin アプリで動作する準備が必要なプラットフォーム機能ことはできません。
- カスタムのターゲットとビルド手順は無視されます。 たとえば、Fody、Retit、AutoFac などのツールと AutoMapper を組み込むことはできません。
- F# プロジェクトが Android でサポートされていませんし、iOS でサポートが制限されます。
- カスタムのジェネリック クラスとインターフェイスの高度なシナリオがサポートされていない可能性があります。

その他の問題を報告してください[bugzilla](https://aka.ms/live-player-report-issue)です。


## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [セットアップ](~/tools/live-player/install.md)
