---
title: Xamarin Live Player の制限事項
description: このドキュメントでは、Xamarin Live Player の制限事項について説明します。 デバイスの要件について説明します、機能、プロジェクトの種類、およびその他の他のトピックで動作します。
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: aff6990df1b710190f11c2d7fa09c8399e94f8af
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "40251244"
---
# <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player の制限事項

![プレビュー機能](~/media/shared/preview.png)

## <a name="device-requirements"></a>デバイスの要件

Xamarin Live Player アプリには、次の Android デバイスがサポートされています。

- Android 4.2 以降。
- ARM v7a、ARM v8a、ARM64 v8a、x86、または x86_64 プロセッサ。

## <a name="limitations"></a>制限事項

これには次の項目を含むを Xamarin Live Player を実行することで、いくつかの制限があります。

### <a name="ios"></a>iOS

Live Player は、iOS のご利用いただけません。

### <a name="xamarinforms"></a>Xamarin.Forms

- カスタム レンダラーがサポートされていません。
- 効果がサポートされていません。
- カスタム バインド可能なプロパティを持つカスタム コントロールがサポートされていません。
- 埋め込みリソースがサポートされていません (つまり。 PCL の画像やその他のリソースの埋め込み)。
- サード パーティ製の MVVM フレームワークが (サポートされていませんPrism、Mvvm の間、Mvvm Light など)。

### <a name="other-project-types"></a>その他のプロジェクトの種類

- Live Player は、(Android XML を使用して、ユーザー インターフェイス) をネイティブの Android プロジェクト用のものではありません。

### <a name="misc"></a>[その他]

- リフレクションのサポートの制限 (現在、SQLite と Json.NET のように、いくつかの人気のある Nuget に影響します)。 その他の Nuget をサポートも可能性があります。
- 一部のシステム クラスをオーバーライドすることはできません (たとえば、サブクラスを実装できません)。
- (ただしに構成されているフォト ギャラリーへのアクセスといった一般的な操作) Xamarin Live Player アプリで動作するプロビジョニングが必要なプラットフォーム機能のことはできません。
- カスタムのターゲットとビルド手順は無視されます。 たとえば、Fody、改修、AutoFac、および AutoMapper などのツールが組み込まれることはできません。
- F# プロジェクトはサポートされていません
- カスタムのジェネリック クラスとインターフェイスを使用して、高度なシナリオは、サポートされていません。

使用**問題を報告する**で[Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)または[Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem) Xamarin Live Player の問題を報告します。

## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [セットアップ](~/tools/live-player/install.md)
