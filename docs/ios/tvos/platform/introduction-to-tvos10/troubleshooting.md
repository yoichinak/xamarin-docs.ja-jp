---
title: Xamarin でビルドされた tvOS 10 アプリのトラブルシューティング
description: この記事では、Xamarin アプリで tvOS 10 を操作するためのトラブルシューティングのヒントをいくつか紹介します。 これは、App Store、バイナリの互換性、CFNetwork HttpProtocol、CloudKit、Core Image、NSUserActivity、および UIKit に関連する問題について説明しています。
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: c734bfdd1baaf89cf25d687657e5f14bdbc7834c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283450"
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

## <a name="app-store"></a>アプリ ストア

既知の問題:

- サンドボックス環境でアプリ内購入をテストする場合、[認証] ダイアログボックスが2回表示されることがあります。
- サンドボックス環境でホストされたコンテンツを使用してアプリ内購入をテストする場合、コンテンツのダウンロードが完了するまで、アプリがフォアグラウンドに移動するたびにパスワードダイアログが表示されます。

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

- を`NSObject.ValueForKey`呼び出す`null`と、キーによって例外が発生します。
- を呼び出す`UIFont.WithName`ときに名前でフォントを参照すると、クラッシュが発生します。
- と`NSURLSession`は`NSURLConnection`どちらも、url の TLS ハンドシェイク中に RC4 `http://`暗号スイートを使用しなくなりました。
- `ViewWillLayoutSubviews`またはのいずれかのメソッドでスーパービューのジオメトリを変更する`LayoutSubviews`と、アプリがハングすることがあります。
- すべての SSL/TLS 接続では、RC4 対称暗号が既定で無効になっています。 さらに、セキュリティで保護されたトランスポート API は SSLv3 をサポートしなくなりました。アプリは、できるだけ早く SHA-1 と3DES 暗号化の使用を停止することをお勧めします。

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP プロトコル

クラスのプロパティは、から`NSURLConnection`開かれていないストリームに設定`NSURLSession`する必要があります。これにより、この要件が厳密に適用されるようになります。 `HTTPBodyStream` `NSMutableURLRequest`

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

実行時間の長い操作では、 _"ファイルを保存するためのアクセス許可がありません"_ が返されます。 エラー.

<a name="CoreImage" />

## <a name="core-image"></a>コアイメージ

API `CIImageProcessor`では、任意の入力イメージの数がサポートされるようになりました。 `CIImageProcessor`TvOS 10 beta 1 に含まれていた API は削除されます。

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作の後、 `UserInfo` `NSUserActivity`オブジェクトのプロパティが空になる場合があります。 現在の`BecomeCurrent`回避策として nsuseractivity ' オブジェクトを明示的に呼び出します。

<a name="UIKit" />

## <a name="uikit"></a>UIKit

既知の問題:

- の背景の`UINavigationBar` `UITabBar`外観が変更され`UIToolBar`たか、またはレイアウトパスによって新しい外観が解決される可能性があります。 `LayoutSubviews`、 、`UpdateConstraints`または`DidUpdateSubviews`イベントの内部でこれらの外観を変更しようとすると、レイアウトループが無限になる可能性があります。 `WillLayoutSubviews`
- TvOS 10 では、 `RemoveGestureRecognizer` `UIView`オブジェクトのメソッドを呼び出すと、進行中のジェスチャ認識エンジンが明示的に取り消されます。
- 表示されたビューコントローラーは、ステータスバーの外観に影響を与えるようになりました。
- tvOS 10 では`base.AwakeFromNib` `AwakeFromNib` 、メソッドをサブクラス`UIViewController`化およびオーバーライドするときに、を呼び出す必要があります。
- を呼び出す`UIView` `LayoutSubviews` 前`base.LayoutSubviews`にレイアウトをオーバーライドしてダーティにするカスタムサブクラスを持つアプリは、tvOS 10 で無限のレイアウトループをトリガーすることがあります。
- 方向固有または flippable の画像アセットは、オブジェクトに`UIButton`割り当てられたときには反転されません。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
