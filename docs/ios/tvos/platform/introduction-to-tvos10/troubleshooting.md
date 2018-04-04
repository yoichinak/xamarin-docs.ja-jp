---
title: トラブルシューティング
description: この記事は、tvOS 10 Xamarin.tvOS アプリで使用するためのいくつかのトラブルシューティングのヒントを提供します。
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8875e658ead17820655a2401079627875c14958b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>トラブルシューティング

_この記事は、tvOS 10 Xamarin.tvOS アプリで使用するためのいくつかのトラブルシューティングのヒントを提供します。_

次のセクションでは、tvOS 10 Xamarin.tvOS とそれらの問題の解決策を使用する場合に発生する可能性がある既知の問題を一覧表示します。

- [App Store](#App-Store)
- [バイナリの互換性](#Binary-Compatibility)
- [CFNetwork HTTP プロトコル](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>アプリ ストア

既知の問題:

 - サンド ボックス環境でアプリ内購入をテストするときに、認証ダイアログが 2 回表示さ可能性があります。
 - サンド ボックス環境でホストされているコンテンツをアプリ内購入をテストするときに、コンテンツのダウンロードが完了するまで、アプリが前面に表示になるたびにパスワード ダイアログ ボックスが表示されます。

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

 - 呼び出す`NSObject.ValueForKey`は、`null`キー、例外が発生します。
 - 呼び出すときに、フォントを名前で参照する`UIFont.WithName`クラッシュが発生します。
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

## <a name="coreimage"></a>CoreImage

`CIImageProcessor` API が、任意の入力 image の数になりました。 `CIImageProcessor` TvOS 10 ベータ 1 に含まれている API は削除されます。

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作後に、`UserInfo`のプロパティ、`NSUserActivity`オブジェクトを空にすることがあります。 明示的に呼び出す`BecomeCurrent`NSUserActivity' 問題を回避する現在のオブジェクト。

<a name="UIKit" />

## <a name="uikit"></a>UIKit

既知の問題:

 - 背景の外観に、変更`UINavigationBar`、`UITabBar`または`UIToolBar`新しい外観を解決するのにはレイアウト パスの場合があります。 内のこれらの外観を変更しようとする`LayoutSubviews`、 `UpdateConstraints`、`WillLayoutSubviews`または`DidUpdateSubviews`イベントは、レイアウトが無限ループに陥ることができます。
 - TvOS 10 では、呼び出すことで、`RemoveGestureRecognizer`のメソッド、`UIView`オブジェクトが、実行中のジェスチャ レコグナイザーを明示的にキャンセルします。
 - 提示されたコント ローラーの表示、ステータス バーの外観を変更できます。
 - tvOS 10 を呼び出す開発者が要求`base.AwakeFromNib`をサブクラス化する`UIViewController`をオーバーライドして、`AwakeFromNib`メソッドです。
 - カスタム アプリ`UIView`オーバーライド サブクラス`LayoutSubviews`呼び出す前に、レイアウトをダーティと`base.LayoutSubviews`tvOS 10 内のレイアウトが無限ループをトリガーする可能性があります。
 - 割り当てられたときの回転方向固有または flippable イメージ資産を入れません`UIButton`オブジェクト。





## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
