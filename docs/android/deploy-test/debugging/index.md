---
title: デバイスとエミュレーターで Xamarin.Android をデバッグする
description: Xamarin.Android アプリをテストおよびデバッグする方法
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 0f873f69de7f85a77bdd0ca7aafa33bff1d9b961
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021522"
---
# <a name="debugging"></a>デバッグ

このセクションでは、デバイスまたはエミュレーターで Xamarin.Android アプリをデバッグする方法について説明します。

## <a name="debugging-overview"></a>デバッグの概要

Android アプリケーションを開発するには、物理ハードウェア上で、またはエミュレーターを使用して、アプリケーションを実行する必要があります。 ハードウェアを使用するのが最適な方法ですが、常に実用的というわけではありません。 多くの場合、以下に示されるエミュレーターのいずれかを使用して、Android ハードウェアを簡単かつコスト効率良く、シミュレートまたはエミュレートすることができます。

### <a name="debugging-on-the-android-emulatorandroiddeploy-testdebuggingdebug-on-emulatormd"></a>[Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)

この記事では、Visual Studio から Android Emulator を起動し、仮想デバイスでアプリを実行する方法について説明します。

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[デバイスでのデバッグ](~/android/deploy-test/debugging/debug-on-device.md)

この記事では、物理 Android デバイスを構成して、Xamarin.Android アプリケーションを Visual Studio for Mac または Visual Studio のいずれかに直接展開する方法を説明します。

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android デバッグ ログ](~/android/deploy-test/debugging/android-debug-log.md)

開発者がアプリケーションのデバッグに使用する、よく使われるトリックに `Console.WriteLine` があります。 ただし、Android などのモバイル プラットフォームにコンソールはありません。 Android デバイスでは、アプリの書き込み中に使用する必要があることが多いログを提供します。 これは、取得するために入力するコマンドから、**logcat** と呼ばれることがあります。 この記事では、**logcat** を使用する方法について説明します。

> [!WARNING]
> **Xamarin Android Player** は非推奨とされていることに注意してください。 詳細については、[このブログ投稿のお知らせ](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/)を参照してください。 さらに、**Visual Studio の Android Emulator** は Visual Studio 2017 より非推奨とされています。
