---
title: デバイスとエミュレーターで Xamarin.Android アプリをデバッグする
description: Xamarin.Android アプリをテストおよびデバッグする方法
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 3b3fa14ec81bd4f06322197b7140654f9086ce73
ms.sourcegitcommit: 5821c9709bf5e06e6126233932f94f9cf3524577
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75556484"
---
# <a name="debug-xamarinandroid-apps"></a>Xamarin.Android アプリのデバッグ

このセクションでは、デバイスまたはエミュレーターで Xamarin.Android アプリをデバッグする方法について説明します。

## <a name="debugging-overview"></a>デバッグの概要

Android アプリケーションを開発するには、物理ハードウェア上で、またはエミュレーターを使用して、アプリケーションを実行する必要があります。 ハードウェアを使用するのが最適な方法ですが、常に実用的というわけではありません。 多くの場合、以下に示されるエミュレーターのいずれかを使用して、Android ハードウェアを簡単かつコスト効率良く、シミュレートまたはエミュレートすることができます。

### <a name="debugging-on-the-android-emulatorandroiddeploy-testdebuggingdebug-on-emulatormd"></a>[Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)

この記事では、Visual Studio から Android Emulator を起動し、仮想デバイスでアプリを実行する方法について説明します。

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[デバイスでのデバッグ](~/android/deploy-test/debugging/debug-on-device.md)

この記事では、物理 Android デバイスを構成して、Xamarin.Android アプリケーションを Visual Studio または Visual Studio for Mac のいずれかに直接展開する方法を示します。

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)

開発者がアプリケーションのデバッグに使用する、よく使われるトリックに `Console.WriteLine` があります。 ただし、Android などのモバイル プラットフォームにコンソールはありません。 Android デバイスでは、アプリの書き込み中に使用する必要があることが多いログを提供します。 これは、取得するために入力するコマンドから、**logcat** と呼ばれることがあります。 この記事では、**logcat** を使用する方法について説明します。
