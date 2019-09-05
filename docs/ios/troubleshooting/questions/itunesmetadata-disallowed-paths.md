---
title: アプリの送信が 許可されていないパス (iTunesMetadata. plist) が...?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: ad663c47520826cc549011c9e5fc7cbb078d6214
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291029"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>アプリの送信が 許可されていないパス (iTunesMetadata. plist) が...?

> エラー:エラー 90047:"許可されていないパス (" iTunesMetadata. plist ") は次の場所で見つかりました:Payload/iPhoneApp1 "

このエラーは、ユーザーが[バグ 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180)などの問題に直面するのを防ぐために、Apple の App Store 検証プロセスが変更された結果です。 この特定のエラーは、インストールした Xamarin の特定のバージョンには関連して_いない_ため、ダウングレードは役に立ち_ません_。

この問題の回避策と最新の更新プログラムについては、[こちら](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)のフォーラムの説明を参照してください。
