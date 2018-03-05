---
title: "コマンド ライン エミュレーター"
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: b8e8ec3de3afccab15d54f666974af678c1b8c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
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

他のパラメーターの詳細については、Android サイト ([http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)) を参照してください。
