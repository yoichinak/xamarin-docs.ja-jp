---
title: Xamarin.Mac - macOS Sierra のトラブルシューティング
description: このドキュメントは、Xamarin.Mac アプリでの macOS Sierra を操作するためのいくつかのトラブルシューティングのヒントを提供します。 ヒントは、Mac App Store、Apple Pay、バイナリの互換性、CFNetwork、CloudKit、および詳細に関連します。
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 09/22/2016
ms.openlocfilehash: 1b379bef98e498df4c58ba7209aa46b0b2542fe1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61031422"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - macOS Sierra のトラブルシューティング

_この記事では、Xamarin.Mac アプリでの macOS Sierra を操作するためのいくつかのトラブルシューティングのヒントを提供します。_

次のセクションでは、Xamarin.mac とそれらの問題の解決策を macOS Sierra を使用するときに発生する可能性がある既知の問題を一覧表示します。

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

## <a name="app-store"></a>App Store

既知の問題:

- サンド ボックス環境でアプリ内購入をテストするときに認証ダイアログ ボックスが 2 回表示されます。
- サンド ボックス環境でホストされているコンテンツをアプリ内購入をテストするときに、コンテンツのダウンロードが完了するまで、アプリがフォア グラウンドに移動するたびにパスワード ダイアログ ボックスが表示されます。

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Apple Pay に新しいペイメント カードを追加するときに、不適切な有効期限の日付またはセキュリティ コード (CW) が入力された場合は、カードのプロビジョニング プロセスが終了します。

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

- 呼び出す`NSObject.ValueForKey`は、`null`キー、例外が発生します。
- 両方`NSURLSession`と NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' Url。
- いずれかでスーパー ビューのジオメトリを変更した場合、アプリがハング、`ViewWillLayoutSubviews`または`LayoutSubviews`メソッド。
- すべての SSL/TLS 接続の RC4 対称暗号は既定で無効になりました。 さらに、トランスポートのセキュリティで保護された API が SSLv3 がサポートされなくされ、アプリでは、SHA 1 および 3 des 暗号化を使用して、できるだけ早く停止するをお勧めします。

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP プロトコル

`HTTPBodyStream`のプロパティ、`NSMutableURLRequest`クラスは、以降、開かれていないストリームに設定する必要があります`NSURLConnection`と`NSURLSession`今すぐこの要件を厳密に適用します。

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

長時間実行される操作を返します、 _「ファイルを保存するためのアクセス許可がありません」。_ エラーがあります。

<a name="CoreImage" />

## <a name="core-image"></a>Core イメージ

`CIImageProcessor` API で、任意の入力イメージ数をサポートします。 `CIImageProcessor` MacOS Sierra beta 1 に含まれている API は削除されます。

<a name="Notifications" />

## <a name="notifications"></a>通知

Notification Content 拡張機能を使用する場合、ビュー コント ローラーが正しく解放されないと、拡張機能のメモリ制限に達したときにクラッシュ発生する可能性があります。

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作後に、`UserInfo`のプロパティを`NSUserActivity`オブジェクトを空にすることがあります。 明示的に呼び出す`BecomeCurrent``NSUserActivity`問題を回避する現在のオブジェクト。

<a name="Safari" />

## <a name="safari"></a>Safari

セキュリティで保護された WebGeolocation が必要です (`https://`) iOS 10 と macOS Sierra 場所データの悪用を防ぐには、両方で動作する URL。







## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [新機能については OS X 10.12 です。](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
