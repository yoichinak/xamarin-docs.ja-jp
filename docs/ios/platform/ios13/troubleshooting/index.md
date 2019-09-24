---
title: Xamarin と iOS 13 のトラブルシューティング
description: このセクションでは、iOS 13 に関連する Xamarin 機能のトラブルシューティングのヒントについて説明します。
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: e03990a74ac8477b56b109e26a97b05ad13dba53
ms.sourcegitcommit: 76f930ce63b193ca3f7f85f768b031e59cb342ec
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198449"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>IOS 13 および Xamarin のトラブルシューティングのヒント。 iOS

![プレビュー機能](~/media/shared/preview.png)

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>Xcode 11 に更新すると、シミュレーターが起動しなくなります。

シミュレーターを起動するたびに**Xcode 11 beta 1**に更新すると、次の例外がスローされ、シミュレーターは起動しません。 これは、すべてのシミュレーターで発生します。

### <a name="exception"></a>例外

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>回避策

[修正](https://github.com/xamarin/xamarin-macios/issues/6216)が行われるまで、次の手順に従って古いシミュレーターフレームワークを再インストールし、開発者が作業を続行できるようにすることができます。

> [!NOTE]
> これらの手順は、2つの Xcode アプリケーションがあることを前提としています。
>
> - **Xcode11-beta1** –シミュレーターと Visual Studio for Mac で動作しないベータ版です。
> - **Xcode102** – Xcode 10 の安定したバージョンです。 **Xcode**と呼ばれることもあります。
>
> 構成に応じて、以下のコマンドラインの例を変更します。

1. Xcode で Xcode 11 が選択されていることを確認します。

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. 必要に応じて、最初にセットアップツールを実行します。

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. 次のフレームワークを削除します。

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. 古い Xcode バージョンに切り替える

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. 先ほど選択した_古い_Xcode バージョンの最初の起動ツールを再実行します

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

次の手順を実行すると、iOS シミュレーターを再び使用できるようになります。
