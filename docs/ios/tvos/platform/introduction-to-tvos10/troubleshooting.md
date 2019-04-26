---
title: TvOS 10 のトラブルシューティング、Xamarin でビルドされたアプリ
description: この記事では、Xamarin アプリでの tvOS 10 の操作のトラブルシューティングのヒントをいくつかを示します。 これには、App Store、および関連するバイナリの互換性、CFNetwork HttpProtocol、CloudKit、Core のイメージ、NSUserActivity、UIKit 問題について説明します。
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3815790cfb73f93f399c14d3da44aa3210725388
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60932439"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>TvOS 10 のトラブルシューティング、Xamarin でビルドされたアプリ

次のセクションでは、Xamarin とそれらの問題の解決策で tvOS 10 を使用するときに発生する既知の問題を一覧表示します。

- [App Store](#App-Store)
- [バイナリの互換性](#Binary-Compatibility)
- [CFNetwork HTTP プロトコル](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core イメージ](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>App Store

既知の問題:

 - サンド ボックス環境でアプリ内購入をテストするときに認証ダイアログ ボックスが 2 回表示されます。
 - サンド ボックス環境でホストされているコンテンツをアプリ内購入をテストするときに、コンテンツのダウンロードが完了するまで、アプリがフォア グラウンドに移動するたびにパスワード ダイアログ ボックスが表示されます。

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

 - 呼び出す`NSObject.ValueForKey`は、`null`キー、例外が発生します。
 - 呼び出すときに、フォントを名前で参照する`UIFont.WithName`クラッシュが発生します。
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

`CIImageProcessor` API で、任意の入力イメージ数をサポートします。 `CIImageProcessor` TvOS 10 beta 1 に含まれている API は削除されます。

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作後に、`UserInfo`のプロパティを`NSUserActivity`オブジェクトを空にすることがあります。 明示的に呼び出す`BecomeCurrent`NSUserActivity' 問題を回避する現在のオブジェクト。

<a name="UIKit" />

## <a name="uikit"></a>UIKit

既知の問題:

 - 背景の外観を変更`UINavigationBar`、`UITabBar`または`UIToolBar`新しい外観を解決するのには、レイアウト パスがあります。 内のこれらの外観を変更しようとして、 `LayoutSubviews`、 `UpdateConstraints`、`WillLayoutSubviews`または`DidUpdateSubviews`イベントがループに無限のレイアウトになることができます。
 - TvOS 10、呼び出すことで、`RemoveGestureRecognizer`のメソッドを`UIView`オブジェクトが明示的に任意の実行中のジェスチャ レコグナイザーを取り消します。
 - ビュー コント ローラーが表示されるステータス バーの外観を変更できます。
 - tvOS 10 を呼び出す開発者を要求する`base.AwakeFromNib`をサブクラス化する`UIViewController`をオーバーライドして、`AwakeFromNib`メソッド。
 - ユーザー設定を使用したアプリ`UIView`オーバーライド サブクラス`LayoutSubviews`呼び出す前にレイアウトをダーティと`base.LayoutSubviews`tvOS 10 で、レイアウトの無限ループをトリガーする可能性があります。
 - 割り当てられたときの回転を要求している特定の方向または flippable イメージ資産はありません`UIButton`オブジェクト。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
