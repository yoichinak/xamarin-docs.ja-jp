---
title: iOS 12 の概要
description: このドキュメントを提供する Xamarin のプレビュー リリースで c# バインディングは、一部の iOS 12 Api の概要を説明します。
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/08/2018
ms.openlocfilehash: 5ac19571bc1f1163539a48ea2689c743445d8047
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268866"
---
# <a name="introduction-to-ios-12"></a>iOS 12 の概要

このドキュメントを提供する Xamarin のプレビュー リリースで c# バインディングは、一部の iOS 12 Api の概要を説明します。

Xamarin を使った iOS 12 のアプリの構築を開始を参照してください、[ファースト ステップ ガイド](get-started.md)

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit は、iOS に含まれている拡張現実のフレームワークです。 ARKit 2、拡張現実のシーンでの相互作用する複数のユーザーにより領域内のオブジェクトを永続化し、後でそれらを返すとは 2D 画像認識かつ追跡、および 3D オブジェクトを認識します。 iOS 12 には、AR クイック検索、usdz アプリでの AR モデルを表示する方法も用意されています。

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri のショートカット](siri-shortcuts.md)

Siri のショートカットには、Siri と、アプリケーションをより深く統合する開発者ができるようにします。 Siri のショートカット、ユーザーは、音声コマンドを使用して、コンテンツを開くか、バック グラウンド タスクを開始するまたは Siri をロック画面に示すショートカットを通じてこれらのタスクを開始することができます。

## <a name="core-ml-2coremlmd"></a>[Core ML 2](coreml.md)

Core ML 2 は、量子化のモデルと柔軟なモデルを通じてアプリケーションのサイズが小さくなり、により新しいバッチ予測 API、アプリケーションのパフォーマンスが向上し、machine learning での進歩をサポートするためにカスタム モデルを使用します。

## <a name="notification-improvementsnotificationsindexmd"></a>[通知の機能拡張](notifications/index.md)

Ios 12 でグループ化された通知できるようにアプリまたはスレッドに関連するグループに存在するユーザーへの通知。 概要のテキストはさらに、通知先グループに関する情報を提供します。

IOS 12 で通知コンテンツの拡張機能により、カスタム ユーザー インターフェイスと動的アクション ボタン。

## <a name="natural-language-frameworknatural-languagemd"></a>[自然言語フレームワーク](natural-language.md)

自然言語、フレームワークは、アプリケーションをさまざまな種類の言語分析を実行できます。 たとえば、品詞を識別およびテキストのブロックで表される言語を決定できます。

## <a name="vision-frameworkiosplatformintroduction-to-ios11visionmd"></a>[ビジョン フレームワーク](~/ios/platform/introduction-to-ios11/vision.md)

ビジョン フレームワークには、さまざまな方向に顔を検出できる強化された顔検出機能が含まれています。 また、要求のリビジョンはビジョン framework アルゴリズムの特定のリビジョンを選択できます。

## <a name="photo-and-video-apis"></a>写真とビデオ Api

12、iOS で縦のセグメント化 API は縦向きの画像の背景から前景色を示し、さまざまなイメージの効果を作成するのに便利ですが、線形マスク縦効果マット – を返します。 iOS 12 もにより、リアルタイム ビデオ特殊効果の TrueDepth カメラからデータを使用することができます。

## <a name="passwords"></a>パスワード

iOS 12 では、ユーザーとパスワードを使用する開発者にとって容易。

- パスワードのオートコンプリートと強力なパスワードの自動使用する自動的に生成、保存、および iOS アプリケーションへのサインアップとアプリケーションへのログインで強力なパスワードを使用すること。
- セキュリティ コードの自動入力できるようになりますや手動の切り取りおよび貼り付け記憶しなくても、SMS ベースの認証コードを使用します。
- `ASWebAuthenticationSession`クラスは、フェデレーション認証サービスの使用のプロセスを効率化されます。
- オートコンプリートの資格情報プロバイダーの拡張機能では、ユーザー名とログインのフィールドにパスワードを指定するパスワードをサード パーティ製アプリケーションできます。

## <a name="healthkit-updates"></a>HealthKit の更新プログラム

iOS 11.3 導入[カルテ](https://www.apple.com/healthcare/health-records/)、さまざまな医療機関からの情報を記録し、iOS デバイスに表示するユーザーは、正常性をダウンロードできます。 iOS 12 は、サード パーティ製アプリケーションは、このデータに安全にアクセスできるようにする Api を追加します。

## <a name="imessage-app-presentation-contexts"></a>iMessage アプリ プレゼンテーションのコンテキスト

12、iOS では、iMessage アプリは、通常 iMessage アプリとしてまたは写真またはビデオ特殊効果のコンテキストで実行するアプリを許可するプレゼンテーションのコンテキストをサポートします。

## <a name="network-framework"></a>ネットワーク フレームワーク

基になるネットワーク フレームワークの場合は、ネットワーク スタック、 `URLSession` iOS アプリケーションでよく使用される Api は TCP、UDP、TLS、IPv4 および IPv6 を操作しやすく、スタンドアロン フレームワークとして使用できるようになりました。

## <a name="carplay"></a>CarPlay

Ios 12 でサード パーティ製アプリで配信できますマップと有効にする-めくりによってナビゲーション手順 CarPlay 新しい CarPlay フレームワークを使用しています。

## <a name="deprecations"></a>廃止された機能

Ios 12、Apple が非推奨とされます。

- OpenGL ES[開発者](https://developer.apple.com/ios/whats-new/)メタルを採用します。
- [`UIWebView`](xref:UIKit.UIWebView)、[簡素`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview?language=objc)します。
