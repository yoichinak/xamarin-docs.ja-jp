---
title: コマンド ライン エミュレーター
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 11/05/2018
ms.openlocfilehash: 995b0783604f752915daaa77a8362899ac61e174
ms.sourcegitcommit: f541a92b4f896474f6a5467ccff2028dafa6fee7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "50983589"
---
# <a name="command-line-emulator"></a>コマンド ライン エミュレーター

## <a name="running-the-android-emulator-from-the-command-line"></a>コマンド ラインからの Android Emulator の実行

コマンド ラインからの Android Emulator の実行を有効にする場合、Android SDK で提供される "エミュレーター" ツールを使用することができます。 このツールを使用して、OS X の場合はターミナルから、Windows コンピューターの場合はコマンド プロンプトからエミュレーターを実行することができます。

特定の Android Emulator を起動するには、Android SDK の場所のツール ディレクトリ (C:\android-sdk-windows\tools など) から次のコマンドを実行します。

Windows の場合

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

macOS の場合

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

パーティション サイズを必要とする理由は、既定ではエミュレーターのサイズが小さいため、エミュレーターに Xamarin.Android プラットフォームをインストールするのに十分なスペースをエミュレーターで確保できるようにするためです。

その他のパラメーターの詳細については、こちらの Android のサイトで確認することができます - [https://developer.android.com/studio/run/emulator-commandline](https://developer.android.com/studio/run/emulator-commandline)
