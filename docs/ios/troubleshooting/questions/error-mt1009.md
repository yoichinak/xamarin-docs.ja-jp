---
title: "エラー MT1009: アセンブリをコピーできませんでした。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 030c54275ef384f4a020abaf79456705e8fac0d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>エラー MT1009: アセンブリをコピーできませんでした。

> [!IMPORTANT]
> Xamarin.iOS の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。

」の説明に従って、[リリース ノート](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/)、Xamarin.iOS 7.2.6 の既知の問題です。 この問題は Xamarin.iOS を別のユーザー アカウントを使用してインストールするより高い特権を必要とするファイルのアクセス許可により、開発者の主なアカウントです。

問題を回避してください Mac ワークステーション Terminal.app 開き、次のコマンドを実行します。

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

