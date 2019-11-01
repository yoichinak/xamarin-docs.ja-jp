---
title: Xamarin でビルドされた tvOS 10 アプリのトラブルシューティング
description: この記事では、Xamarin アプリで tvOS 10 を操作するためのトラブルシューティングのヒントをいくつか紹介します。 これは、App Store、バイナリの互換性、CFNetwork HttpProtocol、CloudKit、Core Image、NSUserActivity、および UIKit に関連する問題について説明しています。
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: a6588dee675aee3e2580b70dfdea2920c6235775
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030611"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>Xamarin でビルドされた tvOS 10 アプリのトラブルシューティング

次のセクションでは、tvOS 10 と Xamarin を使用している場合に発生する可能性がある既知の問題と、それらの問題の解決策を示します。

- [App Store](#App-Store)
- [バイナリの互換性](#Binary-Compatibility)
- [CFNetwork HTTP プロトコル](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [コアイメージ](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>App Store

既知の問題:

- サンドボックス環境でアプリ内購入をテストする場合、[認証] ダイアログボックスが2回表示されることがあります。
- サンドボックス環境でホストされたコンテンツを使用してアプリ内購入をテストする場合、コンテンツのダウンロードが完了するまで、アプリがフォアグラウンドに移動するたびにパスワードダイアログが表示されます。

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

- `NSObject.ValueForKey` を呼び出すと、`null` キーによって例外が発生します。
- `UIFont.WithName` を呼び出すときに名前でフォントを参照すると、クラッシュが発生します。
- `NSURLSession` と `NSURLConnection` はどちらも `http://` Url の TLS ハンドシェイク中に RC4 暗号スイートを使用しなくなりました。
- `ViewWillLayoutSubviews` または `LayoutSubviews` のいずれかのメソッドでスーパービューのジオメトリを変更すると、アプリがハングすることがあります。
- すべての SSL/TLS 接続では、RC4 対称暗号が既定で無効になっています。 さらに、セキュリティで保護されたトランスポート API は SSLv3 をサポートしなくなりました。アプリは、できるだけ早く SHA-1 と3DES 暗号化の使用を停止することをお勧めします。

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP プロトコル

`NSMutableURLRequest` クラスの `HTTPBodyStream` プロパティは、`NSURLConnection` によって、また `NSURLSession` によってこの要件が厳密に適用されるようになったため、開かれていないストリームに設定する必要があります。

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

実行時間の長い操作では、 _"ファイルを保存するためのアクセス許可がありません"_ が返されます。 エラー.

<a name="CoreImage" />

## <a name="core-image"></a>コアイメージ

`CIImageProcessor` API では、任意の入力イメージ数がサポートされるようになりました。 tvOS 10 beta 1 に含まれていた `CIImageProcessor` API は削除されます。

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作後、`NSUserActivity` オブジェクトの `UserInfo` プロパティが空になる場合があります。 現在の回避策として `BecomeCurrent` NSUserActivity ' オブジェクトを明示的に呼び出します。

<a name="UIKit" />

## <a name="uikit"></a>UIKit

既知の問題:

- `UINavigationBar`、`UITabBar` または `UIToolBar` の背景の外観を変更すると、レイアウトパスによって新しい外観が解決される場合があります。 `LayoutSubviews`、`UpdateConstraints`、`WillLayoutSubviews` または `DidUpdateSubviews` イベント内でこれらの外観を変更しようとすると、レイアウトループが無限になる可能性があります。
- TvOS 10 では、`UIView` オブジェクトの `RemoveGestureRecognizer` メソッドを呼び出すと、実行中のジェスチャ認識エンジンが明示的に取り消されます。
- 表示されたビューコントローラーは、ステータスバーの外観に影響を与えるようになりました。
- tvOS 10 では、`UIViewController` をサブクラス化し、`AwakeFromNib` メソッドをオーバーライドするときに、開発者が `base.AwakeFromNib` を呼び出す必要があります。
- `base.LayoutSubviews` を呼び出す前に `LayoutSubviews` をオーバーライドしてレイアウトを変更するカスタム `UIView` サブクラスを持つアプリは、tvOS 10 で無限のレイアウトループをトリガーすることがあります。
- 方向固有または flippable の画像アセットは、`UIButton` オブジェクトに割り当てられたときには反転されません。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
