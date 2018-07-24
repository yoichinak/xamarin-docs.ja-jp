---
title: Remoted iOS Simulator for Windows
description: リモート iOS Simulator の Windows では、Visual Studio 2017 の横のウィンドウに表示される iOS シミュレーターでアプリをテストすることができます。
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149808"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator for Windows

リモート iOS Simulator の Windows では、Visual Studio 2017 の横のウィンドウに表示される iOS シミュレーターでアプリをテストすることができます。

[![](ios-simulator-images/hero-sml.png "iOS シミュレーター Windows で実行されています。")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>作業の開始

Windows 用のリモート iOS シミュレーターは、Visual Studio 2017 で使う Xamarin の一部として自動的にインストールされます。 これを使用するには、次の手順に従います。

1. [Mac をビルド ホストに Visual 2017 をペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)です。
2. Visual Studio 2017、iOS または tvOS プロジェクトのデバッグを開始します。 Windows 用のリモート iOS シミュレーターは、Windows コンピューターに表示されます。

## <a name="simulator-window"></a>シミュレーター ウィンドウ

シミュレーター ウィンドウの上部のツールバーには、便利なボタンの数が含まれています。

- **ホーム**– iOS デバイスで、[ホーム] ボタンのシミュレート
- **ロック**– シミュレーター (ロックを解除する方向にスワイプ) のロック
- **スクリーン ショット**–、シミュレーターのスクリーン ショットを保存
- [**設定**](#settings) : キーボード、場所、およびその他の設定が表示されます
- [**その他のオプション**](#other-options) – 回転やシェイク ジェスチャなどのさまざまなシミュレーター オプションを表示

    [![](ios-simulator-images/maps-app-sml.png "iOS シミュレーターの使用例をマップします。")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>設定

ツールバーの歯車アイコンをクリックすると開きます、**設定**ウィンドウ。

[![](ios-simulator-images/settings-sml.png "iOS シミュレーターの設定")](ios-simulator-images/settings.png#lightbox)

これらの設定では、デバイスが必要がある場所を選択する、ハードウェア キーボードを有効にすることができます (静的および移動の場所の両方がサポート)、レポートが Touch ID を有効にして、コンテンツと、シミュレーターの場合は設定をリセットします。

## <a name="other-options"></a>その他のオプション

ツールバーの省略記号ボタンでは、回転、シェイク ジェスチャ、および再起動などの他のオプションが表示されます。 これらと同じオプションは、シミュレーターのウィンドウで任意の場所を右クリックして、リストとして表示できます。

[![](ios-simulator-images/more-sml.png "iOS シミュレーターの追加の設定")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>タッチ スクリーンのサポート

最近のほとんどの Windows コンピューターでは、タッチ スクリーンがあります。 リモート iOS Simulator の Windows では、タッチの相互作用をサポートするため、同じピンチ、スワイプして、物理的な iOS デバイスで使用するマルチ本指タッチ ジェスチャとアプリをテストすることができます。

同様に、リモート iOS シミュレーター for Windows は、Apple の鉛筆の入力として Windows スタイラス入力を処理します。

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>リモート iOS シミュレーター for Windows を無効にします。

リモート iOS シミュレーター for Windows を無効にするに移動**ツール > オプション > Xamarin > iOS 設定**をオフにし、**リモート Windows Simulator**です。

[![](ios-simulator-images/options-sml.png "シミュレーターを使用する チェック ボックス")](ios-simulator-images/options.png#lightbox)

このオプションを無効にすると、デバッグが開きますは、接続された Mac 上の iOS シミュレーターは、ホストを作成します。
