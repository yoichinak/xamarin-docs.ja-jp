---
title: watchOS 3 のトラブルシューティング
description: このドキュメントでは、Xamarin で watchOS 3 を使用する際に役立ついくつかのトラブルシューティングのヒントを提供します。 ヒントは、アクティビティ、Apple Pay、バックグラウンド更新、Nn 接続、プライバシーなどに関連しています。
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: f10fb237bca92f49ac77657778ada8a47ed69c49
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292171"
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 のトラブルシューティング

_この記事では、Xamarin Apple Watch アプリで watchOS 3 を操作するためのトラブルシューティングのヒントをいくつか紹介します。_

このページでは、watchOS 3 と Xamarin を使用する場合に発生する可能性がある既知の問題と、それらの問題の解決策を示します。

## <a name="activities"></a>アクティビティ

アクティビティの共有が正常に機能するには、すべてのペアリングされた Apple Watch で watchOS 3 が実行されている必要があります。

既知の問題:

- アクティビティ共有通知への応答が失敗することがあります。
- メッセージを使用したアクティビティ共有通知への返信が失敗する場合があります。
- アクティビティ共有通知メッセージの上位のコンテキストテキストは正しくありません。

## <a name="apple-pay"></a>Apple Pay

既知の問題:

- Apple Pay の新しい支払い処理に対して正しくない有効期限日または CW コードを入力した場合、**次**にヒットすると、実行中のプロセスがクラッシュします。
- 暗証番号 (PIN) を必要とするアプリ内 Apple Pay 購入がクラッシュする場合があります。

## <a name="auto-mac-unlock"></a>Mac の自動ロック解除

WatchOS 3 beta 2 (またはそれ以降) と macOS Sierra beta 2 (またはそれ以降) を使用することにより、ユーザーの iCloud アカウントで2要素認証が有効になっている場合、その Apple Watch を使用して Mac を自動的にロック解除できます。

## <a name="background-refresh"></a>バックグラウンド更新

システムリソースに違反すると、次の例外コードで watchOS 3 アプリがクラッシュします。

- **0xc51bad01** -アプリで消費された CPU 時間が多すぎます。
- **0xc51bad02** -アプリが使用しているウォール時間が多すぎます。
- **0xc51bad03** -アプリには、現在のタスクを完了するための十分なランタイムがありませんでした。

## <a name="clock"></a>時計

新しくインストールされた Apple Watch アプリの複雑さは、空白として表示されることがあります。 この問題を解決するには Apple Watch を再起動してください。

## <a name="connectivity"></a>接続

既知の問題:

- watchOS では Apple Watch、保護されたユーザーデータに対するアクセス許可をユーザーにに要求しません。 Watch アプリでデータを使用する前に、iPhone アプリにアクセス権を付与します。
- Apple Watch は、すべての WatchConnectivity 転送が失敗した状態に入り、Apple Watch を再起動して修正します。

## <a name="notifications"></a>通知

メディア添付ファイルが大きすぎる場合は、ユーザーの iPhone に表示されますが、Apple Watch は表示されません。

## <a name="nsurlconnection"></a>NSURLConnection

以前`NSURLConnection`の TLS プロトコルを使用している接続はすべて失敗します。 すべての SSL/TLS 接続では、RC4 対称暗号が既定で無効になっています。 さらに、セキュリティで保護されたトランスポート API は SSLv3 をサポートしなくなりました。アプリは、できるだけ早く SHA-1 と3DES 暗号化の使用を停止することをお勧めします。

WatchOS 3 では、SSL/TLS 接続のセキュリティは Apple によって厳密に適用されています。 影響を受けるサービスとアプリは、最新の TLS プロトコルバージョンを使用するように web サーバーを更新する必要があります。

## <a name="nsurlsession"></a>NSURLSession

WatchOS 3 の時点で、 `HTTPBodyStream`では、 `NSMutableURLRequest`クラスのプロパティを未開封のストリーム`NSURLConnection` `NSURLSession`に設定し、この要件を厳密に適用する必要があります。

## <a name="privacy"></a>プライバシー

既知の問題:

Url を使用`https://`する`NSURLConnection`場合、とでは、TLSハンドシェイク中にRC4暗号スイートがサポートされなく`NSURLSession`なりました。 次のエラーコードのいずれかが生成される可能性があります。

- **-1200 または-98** - `NSURLErrorSecurityConnectionFailed`および securetransport エラー。
- **-1200 [3:-9824]** -Http の読み込みに失敗しました。
- **-**  -  1200`NSURLConnection`はエラーで終了しました。

WatchOS 3 では、SSL/TLS 接続のセキュリティは Apple によって厳密に適用されています。 影響を受けるサービスとアプリは、最新の TLS プロトコルバージョンを使用するように web サーバーを更新する必要があります。 詳細については、上記の[Nn1 接続](#nsurlconnection)を参照してください。

## <a name="snapshots"></a>スナップショット

新しい`HandelBackgroundTask` API を採用していない WatchKit アプリは、watchOS 3 で定期的な更新プログラムを受信しなくなります。 

## <a name="watchkit"></a>WatchKit

SpriteKit と SceneKit のシーンは、アプリが watchOS Dock の背景に入ったときに一時停止します。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [WatchOS 3 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
