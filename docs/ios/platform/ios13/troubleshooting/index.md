---
title: Xamarin と iOS 13 のトラブルシューティング
description: このセクションには、iOS 13 に関連する Xamarin 機能のトラブルシューティングのヒントが含まれています。
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: 99fd7e41e1521c6a9254d286bf989281658ccf24
ms.sourcegitcommit: 85c45dc28ab3625321c271804768d8e4fce62faf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "67039789"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>IOS 13 と Xamarin.iOS のトラブルシューティングのヒント

![プレビュー機能](~/media/shared/preview.png)

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>起動からシミュレーターを停止する Xcode 11 への更新

更新した後**Xcode 11 beta 1**シミュレーターが起動されるたび、次の例外がスローされ、シミュレーターが起動しません。 これは、すべてのシミュレーターで発生します。

### <a name="exception"></a>例外

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>回避策

行われるまで、[修正](https://github.com/xamarin/xamarin-macios/issues/6216)、次の手順は、開発者が作業を続行できるようにするために古いシミュレーター フレームワークを再インストール後に指定できます。

> [!NOTE]
> 次の手順では、Xcode の 2 つのアプリケーションがあると仮定します。
> - **Xcode11 beta1.app** – ベータ版シミュレーターと Visual Studio for mac は動作しません
> - **Xcode102.app** – 安定したバージョンの Xcode 10。 あなたを呼び出すことがありますはも**Xcode.app**します。
>
> 構成に応じて、以下のコマンド ライン例を変更します。

1. Xcode の選択で選択されている Xcode 11 があることを確認します。

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. 必要な場合は、実行、セットアップは、初めてのツールです。

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. 次のフレームワークを削除します。

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. 古い Xcode バージョンに戻す

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. 最初の起動ツールを再実行、_古い_選択した Xcode バージョン

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

次の手順を実行した後は、iOS シミュレーターの使用をもう一度できる必要があります。