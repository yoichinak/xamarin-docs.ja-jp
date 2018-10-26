---
title: Remoted iOS Simulator for Windows
description: リモートの iOS シミュレーターの Windows では、Visual Studio 2017 と共に Windows に表示される iOS シミュレーターでアプリをテストできます。
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: lobrien
ms.author: laobri
ms.date: 05/11/2018
ms.openlocfilehash: c923a62916959c74b8cd753d25361afee68e75fe
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131533"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator for Windows

リモートの iOS シミュレーターの Windows では、Visual Studio 2017 と共に Windows に表示される iOS シミュレーターでアプリをテストできます。

[![](images/hero-sml.png "iOS シミュレーター Windows で実行されています。")](images/hero.png#lightbox)

## <a name="getting-started"></a>作業の開始

Windows 用のリモートの iOS シミュレーターは、Visual Studio 2017 での Xamarin の一部として自動的にインストールされます。 これを使用するには、次の手順に従います。

1. [Visual 2017 を Mac ビルド ホストにペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)します。
2. Visual Studio 2017 で iOS または tvOS プロジェクトのデバッグを開始します。 Windows のリモートの iOS シミュレーターは、Windows コンピューターに表示されます。

ウォッチ[このビデオ](deploy.md)のステップ バイ ステップ ガイドです。

## <a name="simulator-window"></a>シミュレーター ウィンドウ

シミュレーターのウィンドウの上部にあるツールバーには、さまざまな便利なボタンが含まれています。

- **ホーム**– iOS デバイスで [ホーム] ボタンをシミュレートします。
- **ロック**– シミュレーター (ロックを解除するのにはスワイプ) をロック
- **スクリーン ショット**– シミュレーターのスクリーン ショットを保存します。
- [**設定**](#settings) – キーボード、場所、およびその他の設定を表示します。
- [**その他のオプション**](#other-options) – 回転とシェイク ジェスチャなどのさまざまなシミュレーター オプションが表示されます

    [![](images/maps-app-sml.png "iOS シミュレーターの使用例をマップします。")](images/maps-app.png#lightbox)

## <a name="settings"></a>設定

ツールバーの歯車アイコンをクリックすると、**設定**ウィンドウ。

[![](images/settings-sml.png "iOS シミュレーターの設定")](images/settings.png#lightbox)

これらの設定では、デバイスが場所を選択する、ハードウェア キーボードを有効にすることができるレポート (静的および移動の場所の両方がサポート)、Touch ID を有効にして、コンテンツとシミュレーターの設定をリセットします。

## <a name="other-options"></a>その他のオプション

ツールバーの省略記号ボタンには、回転、シェイク ジェスチャ、および再起動などの他のオプションが表示されます。 シミュレーターのウィンドウで任意の場所を右クリックして、これらと同じオプションをリストとして表示できます。

[![](images/more-sml.png "iOS シミュレーターの追加の設定")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>タッチ スクリーン サポート

ほとんどの最新の Windows コンピューターでは、タッチ画面が存在します。 リモートの iOS シミュレーターの Windows では、タッチ操作をサポートするためには、同じピンチやスワイプ、物理 iOS デバイスで使用するマルチ指によるタッチ ジェスチャを使用してアプリケーションをテストできます。

同様に、リモートの iOS シミュレーターの Windows では、Apple 鉛筆の入力として Windows スタイラス入力を処理します。

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>リモートの iOS シミュレーターの Windows を無効にします。

リモートの iOS シミュレーターの Windows を無効にするに移動します。**ツール > オプション > Xamarin > iOS 設定**をオフにし**リモート Windows Simulator**します。

[![](images/options-sml.png "シミュレーターを使用する チェック ボックス")](images/options.png#lightbox)

このオプションを無効にすると、デバッグが開きますは、接続されている Mac 上の iOS シミュレーターは、ホストを構築します。
