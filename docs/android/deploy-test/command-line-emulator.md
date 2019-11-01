---
title: コマンド ライン エミュレーター
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/05/2018
ms.openlocfilehash: 35e10ffc20e075e0245c7e42f7fd0aff24de4abb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021578"
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
