---
title: 'エラー MT1009: アセンブリをコピーできませんでした。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4a67537cc53aeecf1b86d11dbf041cea79587dd2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105405"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>エラー MT1009: アセンブリをコピーできませんでした。

> [!IMPORTANT]
> Xamarin.iOS の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新バージョンで問題が発生した場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。

」の説明に従って、[リリース ノート](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/)、7.2.6 Xamarin.iOS の既知の問題になります。 この問題は、開発者の主なアカウントの別のユーザー アカウントを使用して Xamarin.iOS がインストールされている場合は、高い特権を必要とするファイルのアクセス許可のため、します。

問題の回避、Mac のワークステーションで Terminal.app を開くし、次のコマンドを実行してください。

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

