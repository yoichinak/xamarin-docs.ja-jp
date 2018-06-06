---
title: Xamarin ライブ Player の制限事項
description: このドキュメントでは、Xamarin Live Player の制限事項について説明します。 デバイスの要件について説明、プロジェクトの種類、およびその他の他のトピックで動作している機能です。
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793676"
---
# <a name="limitations-of-xamarin-live-player"></a>Xamarin ライブ Player の制限事項

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
- カスタムのターゲットとビルド手順は無視されます。 たとえば、Fody、不適切、AutoFac、および AutoMapper などのツールを組み込むことはできません。
- F# プロジェクトが Android でサポートされていませんし、iOS でサポートが制限されます。
- カスタムのジェネリック クラスとインターフェイスの高度なシナリオがサポートされていない可能性があります。

その他の問題を報告してください[bugzilla](https://aka.ms/live-player-report-issue)です。

## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [セットアップ](~/tools/live-player/install.md)
