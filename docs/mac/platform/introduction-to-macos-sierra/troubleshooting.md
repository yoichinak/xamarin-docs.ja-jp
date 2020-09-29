---
title: Xamarin. Mac-macOS Sierra のトラブルシューティング
description: このドキュメントでは、Xamarin. Mac アプリで macOS Sierra を操作するためのトラブルシューティングのヒントをいくつか紹介します。 ヒントは、Mac App Store、Apple Pay、バイナリ互換性、CFNetwork、CloudKit などに関連しています。
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 09/22/2016
ms.openlocfilehash: d3ec300cbda5d22a285c3f63d3281bde29b94024
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430058"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin. Mac-macOS Sierra のトラブルシューティング

_この記事では、Xamarin. Mac アプリで macOS Sierra を操作するためのトラブルシューティングのヒントをいくつか紹介します。_

次のセクションでは、macOS Sierra を Xamarin. Mac と共に使用した場合に発生する可能性がある既知の問題とその解決策を示します。

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [バイナリの互換性](#Binary-Compatibility)
- [CFNetwork HTTP プロトコル](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [コアイメージ](#CoreImage)
- [通知](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store"></a>

## <a name="app-store"></a>App Store

既知の問題:

- サンドボックス環境でアプリ内購入をテストする場合、[認証] ダイアログボックスが2回表示されることがあります。
- サンドボックス環境でホストされたコンテンツを使用してアプリ内購入をテストする場合、コンテンツのダウンロードが完了するまで、アプリがフォアグラウンドに移動するたびにパスワードダイアログが表示されます。

<a name="Apple-Pay"></a>

## <a name="apple-pay"></a>Apple Pay

Apple Pay に新しい支払カードを追加するときに、無効な有効期限またはセキュリティコード (CW) を入力すると、カードのプロビジョニングプロセスが終了します。

<a name="Binary-Compatibility"></a>

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

- を呼び出すと、キーによって例外が発生 `NSObject.ValueForKey` `null` します。
- `NSURLSession`とはどちらも、 `NSURLConnection` URL の TLS ハンドシェイク中に RC4 暗号スイートを使用しなくなりました `http://` 。
- またはのいずれかのメソッドでスーパービューのジオメトリを変更すると、アプリがハングすることが `ViewWillLayoutSubviews` `LayoutSubviews` あります。
- すべての SSL/TLS 接続では、RC4 対称暗号が既定で無効になっています。 さらに、セキュリティで保護されたトランスポート API は SSLv3 をサポートしなくなりました。アプリは、できるだけ早く SHA-1 と3DES 暗号化の使用を停止することをお勧めします。

<a name="CFNetwork-HTTP-Protocol"></a>

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP プロトコル

`HTTPBodyStream`クラスのプロパティは、 `NSMutableURLRequest` から開かれていないストリームに設定する必要があり `NSURLConnection` ます。これにより、この要件が厳密に適用されるようになり `NSURLSession` ます。

<a name="CloudKit"></a>

## <a name="cloudkit"></a>CloudKit

実行時間の長い操作では、 _"ファイルを保存するためのアクセス許可がありません"_ が返されます。 " というエラーが発生する場合があります。

<a name="CoreImage"></a>

## <a name="core-image"></a>コアイメージ

API では、 `CIImageProcessor` 任意の入力イメージの数がサポートされるようになりました。 `CIImageProcessor` MacOS Sierra beta 1 に含まれていた API は削除されます。

<a name="Notifications"></a>

## <a name="notifications"></a>通知

通知コンテンツ拡張機能を使用する場合、ビューコントローラーが正しく解放されていないため、拡張メモリの制限に達したときにクラッシュする可能性があります。

<a name="NSUserActivity"></a>

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作の後、 `UserInfo` オブジェクトのプロパティが空になる `NSUserActivity` 場合があります。 現在の `BecomeCurrent` `NSUserActivity` 回避策としてオブジェクトを明示的に呼び出します。

<a name="Safari"></a>

## <a name="safari"></a>Safari

WebGeolocation は、 `https://` iOS 10 と macOS Sierra の両方で動作し、場所データの悪意のある使用を防ぐために、セキュリティで保護された () URL を必要とします。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [MacOS 10.12 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)