---
title: リモート iOS シミュレーター (Windows 用)
description: Windows 上の Visual Studio 内に完全にテストおよびデバッグの iOS アプリ
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: bcf0aa2b1677af0b980c6ca48bb29c1cad32e52d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>リモート iOS シミュレーター (Windows 用)

_Windows 上の Visual Studio 内に完全にテストおよびデバッグの iOS アプリ_

[![](ios-simulator-images/hero-sml.png "iOS シミュレーター Windows で実行されています。")](ios-simulator-images/hero.png#lightbox)

## <a name="download-and-install"></a>ダウンロードとインストール

ダウンロード、[インストーラー](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi)と、Windows コンピューターにインストールします。 Xamarin 用の visual Studio ツールは、既にインストールしてください。

> [!NOTE]
> Visual Studio でのリモート iOS シミュレーターを使用するには、xamarin がインストールされているネットワークに接続された Mac が必要です。

## <a name="getting-started"></a>作業の開始

リモート iOS シミュレーターの使用。

1. Visual Studio がリモート iOS シミュレーターを開始する前に少なくとも 1 回、Mac に接続されていることを確認してください。
2. IOS または tvOS アプリことを確認、**スタートアップ プロジェクト**しデバッグを開始します。

リモート iOS シミュレーターを無効にすることができます**ツール > オプション > Xamarin > iOS 設定**のボックスをオフにして**リモート Windows Simulator**ここで示すようにします。

[![](ios-simulator-images/options-sml.png "シミュレーターを使用する チェック ボックス")](ios-simulator-images/options.png#lightbox)

接続されている Mac コンピューターでは、iOS シミュレーターを開きです。 リモート iOS シミュレーターをオンにするには、このオプションを確認します。

## <a name="features"></a>フィーチャー

リモート iOS シミュレーターは、テストし、Windows 上の Visual Studio から完全シミュレーターでの iOS アプリをデバッグする方法を提供します。

### <a name="simulator-window"></a>シミュレーター ウィンドウ

ウィンドウのツールバーには、シミュレーターと対話するボタンの数が含まれています。

- **ホーム**– デバイスの [ホーム] ボタンをシミュレートします。
- **ロック**– (ロックを解除するスワイプできます)、シミュレーターをロックします。
- **スクリーン ショット**– をディスクに、シミュレーターのスクリーン ショットを保存します。
- [**設定**](#settings) – キーボードと場所を構成します。
- その他の[**オプション**](#options) – のさまざまなシミュレーターのオプションが使用できる、回転のシェイクなど他の状態は、シミュレーターを起動します。 いくつかのオプションは、隠されている場合は、ツールバーで、またはウィンドウを右クリックして表示される省略記号アイコンからアクセスできます。

    [![](ios-simulator-images/maps-app-sml.png "iOS シミュレーターの使用例をマップします。")](ios-simulator-images/maps-app.png#lightbox)


### <a name="settings"></a>設定

「歯車」アイコンを開く、**設定**ウィンドウ。

[![](ios-simulator-images/settings-sml.png "iOS シミュレーターの設定")](ios-simulator-images/settings.png#lightbox)

これにより、シミュレーターでのハードウェア キーボードを有効化し、のどの場所が、デバイス (静的な場所、または他の移動の場所のオプションを含む) に報告を選択することができます。



### <a name="other-options"></a>その他のオプション

シェイク ジェスチャをトリガーして、シミュレーターの再起動の回転など、シミュレーター内で使用可能なすべてのオプションを表示するシミュレーター ウィンドウ内を右クリックします。

[![](ios-simulator-images/more-sml.png "iOS シミュレーターの追加の設定")](ios-simulator-images/more.png#lightbox)

### <a name="touchscreen-support"></a>タッチ スクリーンのサポート

最近のほとんどの Windows コンピューターはタッチ スクリーンを持ち、リモート iOS シミュレーターでは、対話をテストするユーザー、iOS アプリのシミュレーター ウィンドウのタッチすることができます。

これには、はさむ、スワイプ、および複数本指タッチ ジェスチャ - 以前でしただけ簡単にテストする物理デバイスである点が含まれます。

Windows でのスタイラスのサポートは、シミュレーターで Apple 鉛筆入力にも変換されます。

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
