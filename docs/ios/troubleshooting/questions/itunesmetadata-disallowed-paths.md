---
title: 'アプリの送信が失敗する理由と: (iTunesMetadata.plist) で検出されたときに、パスを許可されていませんか?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: a4d2769bb0cb4e119f1c90353657471b44eb6c7c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104493"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>アプリの送信が失敗する理由と: (iTunesMetadata.plist) で検出されたときに、パスを許可されていませんか?

> エラー: エラー ITMS-90047:"("iTunesMetadata.plist") で検出されたときに、パスを許可されていません: Payload/iPhoneApp1.app"

このエラーは、結果をユーザーがのような問題が発生するを防ぐために Apple の App Store の検証プロセスが変更された[バグ 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180)します。 この特定のエラーは_いない_は Xamarin がインストールされている特定のバージョンに関連して、そのためにダウン グレード_いない_ヘルプします。

フォーラムのディスカッションを参照してください。[ここ](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)と回避策は、この問題の最新の更新プログラム。
