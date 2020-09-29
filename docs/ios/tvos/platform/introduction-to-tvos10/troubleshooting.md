---
title: Xamarin でビルドされた tvOS 10 アプリのトラブルシューティング
description: この記事では、Xamarin アプリで tvOS 10 を操作するためのトラブルシューティングのヒントをいくつか紹介します。 これは、App Store、バイナリの互換性、CFNetwork HttpProtocol、CloudKit、Core Image、NSUserActivity、および UIKit に関連する問題について説明しています。
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 65a76c0196b79a17f935f59902c8e6d2f9f25933
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434920"
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

<a name="App-Store"></a>

## <a name="app-store"></a>App Store

既知の問題:

- サンドボックス環境でアプリ内購入をテストする場合、[認証] ダイアログボックスが2回表示されることがあります。
- サンドボックス環境でホストされたコンテンツを使用してアプリ内購入をテストする場合、コンテンツのダウンロードが完了するまで、アプリがフォアグラウンドに移動するたびにパスワードダイアログが表示されます。

<a name="Binary-Compatibility"></a>

## <a name="binary-compatibility"></a>バイナリの互換性

既知の問題:

- を呼び出すと、キーによって例外が発生 `NSObject.ValueForKey` `null` します。
- を呼び出すときに名前でフォントを参照すると `UIFont.WithName` 、クラッシュが発生します。
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

API では、 `CIImageProcessor` 任意の入力イメージの数がサポートされるようになりました。 `CIImageProcessor` TvOS 10 beta 1 に含まれていた API は削除されます。

<a name="NSUserActivity"></a>

## <a name="nsuseractivity"></a>NSUserActivity

ハンドオフ操作の後、 `UserInfo` オブジェクトのプロパティが空になる `NSUserActivity` 場合があります。 現在の `BecomeCurrent` 回避策として nsuseractivity ' オブジェクトを明示的に呼び出します。

<a name="UIKit"></a>

## <a name="uikit"></a>UIKit

既知の問題:

- の背景の外観が変更 `UINavigationBar` さ `UITabBar` れたか、または `UIToolBar` レイアウトパスによって新しい外観が解決される可能性があります。 、、またはイベントの内部でこれらの外観を変更しようとすると、 `LayoutSubviews` `UpdateConstraints` `WillLayoutSubviews` `DidUpdateSubviews` レイアウトループが無限になる可能性があります。
- TvOS 10 では、オブジェクトのメソッドを呼び出すと、進行中の `RemoveGestureRecognizer` `UIView` ジェスチャ認識エンジンが明示的に取り消されます。
- 表示されたビューコントローラーは、ステータスバーの外観に影響を与えるようになりました。
- tvOS 10 では、メソッドをサブ `base.AwakeFromNib` クラス化およびオーバーライドするときに、を呼び出す必要があり `UIViewController` `AwakeFromNib` ます。
- を `UIView` 呼び出す前にレイアウトをオーバーライドしてダーティにするカスタムサブクラスを持つアプリ `LayoutSubviews` は、 `base.LayoutSubviews` tvOS 10 で無限のレイアウトループをトリガーすることがあります。
- 方向固有または flippable の画像アセットは、オブジェクトに割り当てられたときには反転されません `UIButton` 。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)