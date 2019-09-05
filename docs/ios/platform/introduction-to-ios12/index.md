---
title: iOS 12 の概要
description: このドキュメントでは、Xamarin のプレビューリリースでバインドが提供さC#れる IOS 12 api の概要について説明します。
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/08/2018
ms.openlocfilehash: b3f3db32e87d83ea4e076d439df3342e5ca2ed50
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284630"
---
# <a name="introduction-to-ios-12"></a>iOS 12 の概要

このドキュメントでは、Xamarin のプレビューリリースでバインドが提供さC#れる IOS 12 api の概要について説明します。

Xamarin を使用して iOS 12 アプリのビルドを開始するには、[ファーストステップガイド](get-started.md)を参照してください。

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit は、iOS に含まれる拡張された現実のフレームワークです。 ARKit 2 を使用すると、複数のユーザーが拡張された現実のシーンで相互に対話できるようになり、オブジェクトをスペース内に保持し、後でそれらに戻ることができるようになり、2D イメージの認識と追跡と3D オブジェクト認識が可能になります。 iOS 12 では、アプリで usdz モデルをレンダリングする方法として、AR の概要も提供しています。

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri ショートカット](siri-shortcuts.md)

Siri のショートカットを使用すると、開発者はアプリケーションを Siri とより深く統合できます。 Siri ショートカットでは、ユーザーは音声コマンドを使用してコンテンツを開いたり、バックグラウンドタスクを開始したりできます。また、これらのタスクは、Siri がロック画面に提示するショートカットを使用して開始することもできます。

## <a name="core-ml-2coremlmd"></a>[Core ML 2](coreml.md)

コア ML 2 では、モデル量子化と柔軟なモデルによってアプリケーションのサイズが削減され、新しいバッチ予測 API によってアプリケーションのパフォーマンスが向上します。また、machine learning での進歩をサポートするカスタムモデルを使用します。

## <a name="notification-improvementsnotificationsindexmd"></a>[通知の機能強化](notifications/index.md)

IOS 12 では、グループ化された通知を使用して、アプリまたはスレッド関連のグループでユーザー通知を提示することができます。 概要テキストは、通知グループに関する詳細情報を提供します。

IOS 12 の通知コンテンツ拡張機能では、カスタムユーザーインターフェイスと動的アクションボタンを使用できます。

## <a name="natural-language-frameworknatural-languagemd"></a>[自然言語フレームワーク](natural-language.md)

自然言語フレームワークを使用すると、アプリケーションはさまざまな種類の言語分析を実行できます。 たとえば、音声の一部を識別し、テキストブロックで表される言語を特定することができます。

## <a name="vision-frameworkiosplatformintroduction-to-ios11visionmd"></a>[ビジョンフレームワーク](~/ios/platform/introduction-to-ios11/vision.md)

ビジョンフレームワークには、さまざまな向きで顔を検出できる改善された顔検出機能が含まれています。 また、要求のリビジョンでは、特定のビジョンフレームワークアルゴリズムリビジョンを選択できます。

## <a name="photo-and-video-apis"></a>Photo Api と video Api

IOS 12 では、縦のセグメンテーション API は縦効果のマットを返します。これは、縦方向の画像の背景から前景を記述する線形マスクで、さまざまなイメージ効果を作成するのに役立ちます。 また、iOS 12 では、TrueDepth カメラの深度データをリアルタイムのビデオ効果に使用することもできます。

## <a name="passwords"></a>パスワード

iOS 12 を使用すると、ユーザーや開発者がパスワードを簡単に操作できるようになります。

- パスワードのオートフィルと自動強力なパスワードを使用すると、アプリケーションにサインアップしてログインするときに、iOS アプリケーションで強力なパスワードを自動的に生成、保存、および使用することができます。
- セキュリティコードのオートコンプリートを使用すると、手動による切り取りと貼り付けや覚えやすいようを行わなくても、SMS ベースの認証コードを使用できます。
- クラス`ASWebAuthenticationSession`は、フェデレーション認証サービスを使用するプロセスを効率化します。
- 資格情報プロバイダー拡張機能を使用すると、サードパーティのパスワードアプリケーションがログインフィールドにユーザー名とパスワードを入力できるようになります。

## <a name="healthkit-updates"></a>HealthKit の更新

iOS 11.3 では、ユーザーがさまざまな医療機関から正常性レコード情報をダウンロードして iOS デバイスで表示できるようにする[正常性レコード](https://www.apple.com/healthcare/health-records/)が導入されました。 iOS 12 は、サードパーティ製のアプリケーションがこのデータに安全にアクセスできるようにする Api を追加します。

## <a name="imessage-app-presentation-contexts"></a>iMessage アプリのプレゼンテーションコンテキスト

IOS 12 では、iMessage アプリでプレゼンテーションコンテキストがサポートされるため、アプリは通常の iMessage アプリとして、または写真やビデオ効果のコンテキストで実行できます。

## <a name="network-framework"></a>ネットワークフレームワーク

IOS アプリケーションで一般的に使用され`URLSession`ている api の基礎となるネットワークスタックが、スタンドアロンフレームワークとして使用できるようになりました。これにより、TCP、UDP、TLS、IPv4/IPv6 などを簡単に操作できます。

## <a name="carplay"></a>CarPlay

IOS 12 では、サードパーティ製のアプリは、新しい CarPlay フレームワークを使用して、マップと、CarPlay のターンバイターンの手順を提供できます。

## <a name="deprecations"></a>廃止

IOS 12 では、Apple は非推奨となりました。

- OpenGL ES では、[開発者](https://developer.apple.com/ios/whats-new/)が金属を採用するようにしています。
- [`UIWebView`](xref:UIKit.UIWebView)。を[優先`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview?language=objc)します。
