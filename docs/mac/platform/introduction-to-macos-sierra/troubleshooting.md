---
title: Xamarin.Mac - macOS Sierra のトラブルシューティング
description: このドキュメントは、macOS Sierra Xamarin.Mac アプリで使用するためのいくつかのトラブルシューティングのヒントを提供します。 ヒントは、Mac App Store、Apple Pay、バイナリの互換性、CFNetwork、CloudKit、および詳細に関連します。
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 5b2571d9562fd137257e2dd0ea2ada8f071bab92
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792328"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - macOS Sierra のトラブルシューティング

_この記事は、macOS Sierra Xamarin.Mac アプリで使用するためのいくつかのトラブルシューティングのヒントを提供します。_

次のセクションでは、macOS Sierra Xamarin.mac とそれらの問題の解決策を使用する場合に発生する可能性がある既知の問題を一覧表示します。

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [バイナリの互換性](#Binary-Compatibility)
- [CFNetwork HTTP プロトコル](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core イメージ](#CoreImage)
- [通知](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>アプリ ストア

既知の問題:

- サンド ボックス環境でアプリ内購入をテストするときに、認証ダイアログが 2 回表示さ可能性があります。
- サンド ボックス環境でホストされているコンテンツをアプリ内購入をテストするときに、コンテンツのダウンロードが完了するまで、アプリが前面に表示になるたびにパスワード ダイアログ ボックスが表示されます。

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Apple Pay に新しい支払いカードを追加するときに、不適切な有効期限の日付またはセキュリティ コード (CW) が入力された場合、カードのプロビジョニング プロセスは終了されます。

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

- 呼び出す`NSObject.ValueForKey`は、`null`キー、例外が発生します。
- 両方`NSURLSession`と NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' Url。
- いずれかで、スーパー ビューのジオメトリを変更する場合、アプリはハングする可能性が、`ViewWillLayoutSubviews`または`LayoutSubviews`メソッドです。
- すべての SSL/TLS 接続の RC4 対称暗号は既定では無効になります。 さらをできるだけ早く SHA 1 および 3 des 暗号化を使用して、アプリを停止することをお勧め、セキュリティで保護されたトランスポート API が不要になった SSLv3 をサポートします。

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP プロトコル

`HTTPBodyStream`のプロパティ、`NSMutableURLRequest`クラスは、以降、開かれていないストリームに設定する必要があります`NSURLConnection`と`NSURLSession`今すぐこの要件を厳密に適用します。

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

長時間実行される処理が返されます、 _「ファイルを保存するアクセス許可がありません」。_ エラーがあります。

<a name="CoreImage" />

## <a name="core-image"></a>Core イメージ

`CIImageProcessor` API が、任意の入力 image の数になりました。 `CIImageProcessor` MacOS Sierra ベータ 1 に含まれている API は削除されます。

<a name="Notifications" />

## <a name="notifications"></a>通知

通知コンテンツの拡張機能を使用するときにコント ローラーの表示が正しく解放されないと、拡張機能のメモリの上限に達すると、クラッシュ可能性があります。

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作後に、`UserInfo`のプロパティ、`NSUserActivity`オブジェクトを空にすることがあります。 明示的に呼び出す`BecomeCurrent``NSUserActivity`問題を回避する現在のオブジェクト。

<a name="Safari" />

## <a name="safari"></a>Safari

セキュリティで保護された WebGeolocation が必要です (`https://`) iOS 10 や macOS Sierra 場所データの悪意のある使用を防ぐための両方で動作する URL。







## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
