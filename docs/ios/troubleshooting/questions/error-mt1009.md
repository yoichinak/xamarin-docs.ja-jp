---
title: 'エラー MT1009: アセンブリをコピーできませんでした。'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776080"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>エラー MT1009: アセンブリをコピーできませんでした。

> [!IMPORTANT]
> Xamarin.iOS の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。

」の説明に従って、[リリース ノート](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/)、Xamarin.iOS 7.2.6 の既知の問題です。 この問題は Xamarin.iOS を別のユーザー アカウントを使用してインストールするより高い特権を必要とするファイルのアクセス許可により、開発者の主なアカウントです。

問題を回避してください Mac ワークステーション Terminal.app 開き、次のコマンドを実行します。

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

