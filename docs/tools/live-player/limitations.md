---
title: Xamarin Live Player の制限事項
description: このドキュメントでは、Xamarin Live Player の制限事項について説明します。 デバイスの要件について説明します、機能、プロジェクトの種類、およびその他の他のトピックで動作します。
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815283"
---
# <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player の制限事項

![プレビュー機能](~/media/shared/preview.png)

## <a name="device-requirements"></a>デバイスの要件
Xamarin Live Player アプリには、次のデバイスがサポートされています。

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 以降。
- ARM v7a、ARM v8a、ARM64 v8a、x86、または x86_64 プロセッサ。

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 以降。
- ARM64 プロセッサ。

-----

## <a name="limitations"></a>制限事項

これには次の項目を含むを Xamarin Live Player を実行することで、いくつかの制限があります。

### <a name="xamarinforms"></a>Xamarin.Forms

- カスタム レンダラーがサポートされていません。
- 効果がサポートされていません。
- カスタム バインド可能なプロパティを持つカスタム コントロールがサポートされていません。
- 埋め込みリソースがサポートされていません (つまり。 PCL の画像やその他のリソースの埋め込み)。
- サード パーティ製の MVVM フレームワークが (サポートされていませんPrism、Mvvm の間、Mvvm Light など)。
- IOS での資産カタログがサポートされていません。

### <a name="other-project-types"></a>その他のプロジェクトの種類

- Live Player は、ネイティブの Android または iOS プロジェクト (Android XML やストーリー ボードをユーザー インターフェイスの使用) するためのものではありません。

### <a name="misc"></a>[その他]

- リフレクションのサポートの制限 (現在、SQLite と Json.NET のように、いくつかの人気のある Nuget に影響します)。 その他の Nuget をサポートも可能性があります。
- 一部のシステム クラスをオーバーライドすることはできません (たとえば、サブクラスを実装できません)。
- (ただしに構成されているフォト ギャラリーへのアクセスといった一般的な操作) Xamarin Live Player アプリで動作するプロビジョニングが必要なプラットフォーム機能のことはできません。
- カスタムのターゲットとビルド手順は無視されます。 たとえば、Fody、改修、AutoFac、および AutoMapper などのツールが組み込まれることはできません。
- F# プロジェクトが Android でサポートされていないと、ios サポートの制限
- カスタムのジェネリック クラスとインターフェイスを使用して、高度なシナリオは、サポートされていません。

その他の問題を報告してください[bugzilla](https://aka.ms/live-player-report-issue)します。

## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [セットアップ](~/tools/live-player/install.md)
