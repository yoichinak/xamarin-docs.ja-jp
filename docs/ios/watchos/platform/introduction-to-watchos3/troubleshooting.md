---
title: watchOS 3 トラブルシューティング
description: このドキュメントは、Xamarin で watchOS 3 を使用する場合に便利ないくつかのトラブルシューティングのヒントを提供します。 ヒントは、アクティビティ、Apple Pay、バック グラウンド更新、NSURLConnection、プライバシー、および詳細に関連します。
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6d2aaf12bd6c45f6268cf87a77d2ee03a9d7a888
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224033"
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 トラブルシューティング

_この記事では、Apple Watch の Xamarin アプリでは、watchOS 3 の操作のトラブルシューティングのヒントをいくつかを示します。_

このページには、Xamarin とそれらの問題の解決策で watchOS 3 を使用するときに発生する既知の問題が一覧表示されます。

## <a name="activities"></a>アクティビティ

アクティビティが正常に機能する共有ですべてのペアになっている Apple Watch を watchOS 3 実行する必要があります。

既知の問題:

- アクティビティの共有の通知に返信することがありますは失敗します。
- アクティビティの共有通知メッセージに返信があります。
- アクティビティの共有の通知メッセージを上のコンテキストのテキストが正しくないがあります。

## <a name="apple-pay"></a>Apple Pay

既知の問題:

- 場合に達したときに Apple Pay で新しい支払ケアの入力は、不適切な有効期限の日付または CW コード**次**実行中のプロセスがクラッシュします。
- 暗証番号 (PIN) を必要とするアプリで Apple Pay の購入がクラッシュする可能性があります。

## <a name="auto-mac-unlock"></a>Mac の自動ロック解除します。

自動、Apple Watch を使用するユーザーの iCloud アカウントに 2 要素認証が有効になっている場合に 2 つ (またはそれ以上) の watchOS 3 のベータ版と macOS Sierra beta 2 (またはそれ以上) を使用すると、そのファルダのロックを解除

## <a name="background-refresh"></a>バック グラウンド更新

システム リソースに違反すると、次の例外コードを持つ 3 watchOS アプリのクラッシュが発生します。

- **0xc51bad01** -アプリが多すぎる CPU 時間を使用します。
- **0xc51bad02** -アプリが壁の時間がかかりすぎるを使用します。
- **0xc51bad03** -アプリは、現在のタスクを完了するための十分なランタイムがありませんでした。

## <a name="clock"></a>Clock

新しくインストールした Apple Watch アプリから複雑な問題は、空白として表示可能性があります。 この問題を解決する Apple Watch を再起動します。

## <a name="connectivity"></a>接続

既知の問題:

- watchOS で Apple Watch 上の保護されたユーザー データのアクセス許可をユーザーに求めるメッセージされません。 Watch アプリでデータを使用する前に、iPhone アプリでのアクセスを許可します。
- Apple Watch WatchConnectivity のすべての転送が失敗する状態に入ることができますを修正する Apple Watch を再起動します。

## <a name="notifications"></a>通知

メディア添付ファイルが大きすぎる場合は、ユーザーの iPhone、Apple Watch ではないで表示されます。

## <a name="nsurlconnection"></a>NSURLConnection

すべて`NSURLConnection`古い TLS プロトコルを使用して接続は失敗します。 すべての SSL/TLS 接続の RC4 対称暗号は既定で無効になりました。 さらに、トランスポートのセキュリティで保護された API が SSLv3 がサポートされなくされ、アプリでは、SHA 1 および 3 des 暗号化を使用して、できるだけ早く停止するをお勧めします。

WatchOS 3 で時点で SSL/TLS 接続のセキュリティを厳密に Apple によって強制されるされます。 影響を受けるサービスとアプリは、最新の TLS プロトコルのバージョンを使用する web サーバーを更新する必要があります。

## <a name="nsurlsession"></a>NSURLSession

WatchOS 3 で時点で、`HTTPBodyStream`のプロパティ、`NSMutableURLRequest`クラスは、以降、開かれていないストリームに設定する必要があります`NSURLConnection`と`NSURLSession`今すぐこの要件を厳密に適用します。

## <a name="privacy"></a>プライバシー

既知の問題:

使用する場合`https://`両方の Url`NSURLSession`と`NSURLConnection`不要になった TLS ハンドシェイク中に RC4 暗号スイートをサポートします。 次のエラー コードのいずれかを生成可能性があります。

- **-1200 または-98** -`NSURLErrorSecurityConnectionFailed`と SecureTransport エラー。
- **[3:-9824]-1200** -http の読み込みに失敗しました。
- **-1200**  -  `NSURLConnection`エラーで終了しました。

WatchOS 3 で時点で SSL/TLS 接続のセキュリティを厳密に Apple によって強制されるされます。 影響を受けるサービスとアプリは、最新の TLS プロトコルのバージョンを使用する web サーバーを更新する必要があります。 参照してください[NSURLConnection](#nsurlconnection)上の詳細についてはします。

## <a name="snapshots"></a>スナップショット

新しいを採用していない WatchKit アプリ`HandelBackgroundTask`API は、watchOS 3 で定期的な更新プログラムを受けられなくなります。 

## <a name="watchkit"></a>WatchKit

WatchOS ドックのアプリがバック グラウンドに入ったとき、SpriteKit と SceneKit シーンを一時停止されます。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
