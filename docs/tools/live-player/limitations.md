---
title: "制限事項"
description: "Xamarin Live プレーヤーのいくつかの制限"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>制限事項

![プレビュー機能](~/media/shared/preview.png)

## <a name="device-requirements"></a>デバイスの要件
Xamarin Player のライブ アプリには、次のデバイスがサポートされています。

### <a name="ios"></a>iOS
- iOS 9.0 以降。
- ARM64 プロセッサ。
- チェック、 [App Store](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8)サポートされているデバイスの一覧についてはします。

### <a name="android"></a>Android
- Android 4.2 以降。
- ARM-v7a、v8a ARM、ARM64 v8a、x86、または x86_64 プロセッサ。


## <a name="limitations"></a>制限事項

Xamarin Player のライブ実行できますが、次の項目を含むものにいくつかの制限があります。

### <a name="android"></a>Android
- Android ユーザー インターフェイスの設計 AXML ファイルは現在サポートされていません。

### <a name="ios"></a>iOS
- IOS ストーリー ボードの一部の機能はサポートされていません。
- iOS XIB ファイルはサポートされていません。

### <a name="xamarinforms"></a>Xamarin.Forms
- カスタム レンダラーがサポートされていません。
- 効果はサポートされていません。
- カスタム バインド可能なプロパティを持つカスタム コントロールはサポートされていません。
- 埋め込みリソースはサポートされていません (ie。、PCL に画像やその他のリソースを埋め込む)。

### <a name="misc"></a>[その他]
- リフレクションの制限付きサポート (現在、SQLite、Json.NET のように、いくつかの一般的な NuGets に影響します)。 その他の NuGets は引き続きサポートされます。
- 一部のシステム クラスをオーバーライドすることはできません (たとえば、サブクラスを実装することはできません)。
- (ただしに構成されているフォト ギャラリーのアクセスなどの一般的な操作の) Live プレーヤーの Xamarin アプリで動作する準備が必要なプラットフォーム機能ことはできません。
- カスタムのターゲットとビルド手順は無視されます。 たとえば、Fody などのツールを組み込むことはできません。

> [!WARNING]
> **注:** Xamarin Studio で Xamarin Player の Live は機能しません

その他の問題を報告してください[bugzilla](https://aka.ms/live-player-report-issue)です。


## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [セットアップ](~/tools/live-player/install.md)
