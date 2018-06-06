---
title: watchOS 3 トラブルシューティング
description: このドキュメントは、Xamarin で watchOS 3 を使用する場合に役立ちますいくつかのトラブルシューティングのヒントを示します。 ヒントは、アクティビティ、Apple Pay、バック グラウンド更新、NSURLConnection、プライバシー、および詳細に関連します。
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0aca2c96533e17e4aeb2f57d38a87d39f700fb45
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791029"
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 トラブルシューティング

_この記事は、watchOS 3 Xamarin Apple Watch アプリで使用するためのいくつかのトラブルシューティングのヒントを提供します。_

このページには、Xamarin とそれらの問題の解決策で watchOS 3 を使用する場合に発生する可能性がある既知の問題が一覧表示されます。

## <a name="activities"></a>アクティビティ

アクティビティが正常に動作する共有ですべてのペア Apple ウォッチを watchOS 3 実行しなければなりません。

既知の問題:

- アクティビティの共有の通知に応答することがありますは失敗します。
- アクティビティの共有を含む、通知メッセージに返信があります。
- アクティビティの共有の通知メッセージ上のコンテキストのテキストは不正確になります。

## <a name="apple-pay"></a>Apple Pay

既知の問題:

- 場合を押すと Apple Pay で新しい支払注意の入力は、不適切な有効期限の日付または CW コード**次**実行中のプロセスがクラッシュします。
- 暗証番号 (PIN) を必要とするアプリで Apple Pay 購入がクラッシュする可能性があります。

## <a name="auto-mac-unlock"></a>Mac の自動ロック解除します。

自動的に、Apple Watch を使用する watchOS 3 ベータ 2 (以降) と macOS Sierra beta 2 (またはそれ以上) を使用しているユーザーの iCloud アカウントに 2 要素認証が有効になっている場合は、使用して、そのファルダのロックを解除

## <a name="background-refresh"></a>バック グラウンド更新

システム リソースに違反すると、次の例外コードを持つ watchOS 3 アプリのクラッシュが発生します。

- **0xc51bad01** -アプリが多すぎる CPU 時間を使用します。
- **0xc51bad02** -アプリが多すぎる壁時間を使用します。
- **0xc51bad03** -アプリは、現在のタスクを完了するための十分なランタイムがありません。

## <a name="clock"></a>Clock

新しくインストールした Apple Watch アプリからの複雑さは、空白として表示可能性があります。 この問題を解決する Apple Watch を再起動します。

## <a name="connectivity"></a>接続

既知の問題:

- watchOS では、Apple Watch 上で保護されたユーザー データのアクセス許可をユーザーには確認できません。 Watch アプリでデータを使用する前に、iPhone アプリでのアクセスを許可します。
- Apple Watch WatchConnectivity の送信が失敗した状態に入ることができますを解決する Apple Watch を再起動します。

## <a name="notifications"></a>通知

メディア添付ファイルが大きすぎる場合は、ユーザーの iPhone、Apple Watch ではないで表示されます。

## <a name="nsurlconnection"></a>NSURLConnection

どの`NSURLConnection`古い TLS プロトコルを使用して接続は失敗します。 すべての SSL/TLS 接続の RC4 対称暗号は既定では無効になります。 さらをできるだけ早く SHA 1 および 3 des 暗号化を使用して、アプリを停止することをお勧め、セキュリティで保護されたトランスポート API が不要になった SSLv3 をサポートします。

WatchOS 3、時点で SSL/TLS 接続のセキュリティが設定されている厳密に Apple です。 影響を受けたサービスやアプリケーションは、最新の TLS プロトコルのバージョンを使用する web サーバーを更新する必要があります。

## <a name="nsurlsession"></a>NSURLSession

WatchOS 第 3 の時点で、`HTTPBodyStream`のプロパティ、`NSMutableURLRequest`クラスは、以降、開かれていないストリームに設定する必要があります`NSURLConnection`と`NSURLSession`今すぐこの要件を厳密に適用します。

## <a name="privacy"></a>プライバシー

既知の問題:

使用する場合`https://`Url が両方`NSURLSession`と`NSURLConnection`TLS ハンドシェイク中に、RC4 暗号スイートをサポートしないようにします。 次のエラー コードのいずれかを生成することがあります。

- **-1200 または-98** -`NSURLErrorSecurityConnectionFailed`と SecureTransport エラーです。
- **[3:-9824]-1200** -http の読み込みに失敗しました。
- **-1200**  -  `NSURLConnection`エラーで終了しました。

WatchOS 3、時点で SSL/TLS 接続のセキュリティが設定されている厳密に Apple です。 影響を受けたサービスやアプリケーションは、最新の TLS プロトコルのバージョンを使用する web サーバーを更新する必要があります。 参照してください[NSURLConnection](#NSURLConnection)上の詳細についてはします。

## <a name="snapshots"></a>スナップショット

WatchKit いない採用しているアプリ、新しい`HandelBackgroundTask`watchOS 3 で定期的な更新が API では送信されなくなります。 

## <a name="watchkit"></a>WatchKit

アプリ watchOS ドッキング ステーションにバック グラウンドに入ると、SpriteKit と SceneKit のシーンは一時停止されます。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
