---
title: 'エラー MT1009: Could not copy the assembly (アセンブリをコピーできませんでした)'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8973093d851cf2c38fe57bada0e9d01e9664b15b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293009"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>エラー MT1009: Could not copy the assembly (アセンブリをコピーできませんでした)

> [!IMPORTANT]
> この問題は、最新バージョンの Xamarin. iOS で解決されました。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

[リリースノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_7/xamarin.ios_7.2/index.md)に記載されているように、これは7.2.6 の既知の問題です。 この問題が発生するのは、Xamarin が、別のユーザーアカウントを使用してインストールされている場合に、より高い特権を必要とするファイルのアクセス許可があるためです。

この問題を回避するには、Mac ワークステーションでターミナルアプリを開き、次のコマンドを実行します。

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`
