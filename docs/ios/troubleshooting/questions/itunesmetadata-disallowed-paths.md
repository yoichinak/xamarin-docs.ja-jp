---
title: 'アプリの送信が失敗する理由: 許可されていないパス (iTunesMetadata. plist) が...?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 0255c918f7d6e984c9af2b396d9a2a00286286e4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030997"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>アプリの送信が失敗する理由: 許可されていないパス (iTunesMetadata. plist) が...?

> エラー: エラー: "ペイロード/iPhoneApp1" で90047、許可されていないパス ("iTunesMetadata. plist") が見つかりました。

このエラーは、ユーザーが[バグ 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180)などの問題に直面するのを防ぐために、Apple の App Store 検証プロセスが変更された結果です。 この特定のエラーは、インストールした Xamarin の特定のバージョンには関連して_いない_ため、ダウングレードは役に立ち_ません_。

この問題の回避策と最新の更新プログラムについては、[こちら](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)のフォーラムの説明を参照してください。
