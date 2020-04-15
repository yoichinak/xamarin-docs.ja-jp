---
title: Remoted iOS Simulator for Windows
description: Remoted iOS Simulator for Windows を使用すると、Visual Studio 2019 とともに、Windows に表示される iOS シミュレーター上でアプリをテストできます。
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: davidortinau
ms.author: daortin
ms.date: 04/26/2019
ms.openlocfilehash: d5898f9c6ee30eb1f12bf6480b93a609e762e6ea
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "75886594"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator for Windows

Remoted iOS Simulator for Windows を使用すると、Visual Studio 2019 および Visual Studio 2017 とともに、Windows に表示される iOS シミュレーター上でアプリをテストできます。

[![Windows で実行されている iOS シミュレーター](images/hero-sml.png "Windows で実行されている iOS シミュレーター")](images/hero.png#lightbox)

## <a name="getting-started"></a>作業の開始

Remoted iOS Simulator for Windows は、 Visual Studio 2019 および Visual Studio 2017 に Xamarin の一部として自動的にインストールされます。 これを使用するには、次の手順に従います。

1. [Visual 2019 を Mac ビルド ホストとペアリングします](~/ios/get-started/installation/windows/connecting-to-mac/index.md)。
2. Visual Studio で、iOS または tvOS プロジェクトのデバッグを開始します。 Remoted iOS Simulator for Windows が Windows マシンに表示されます。

ステップ バイ ステップのガイドについては、[こちらの動画](deploy.md)をご覧ください。

## <a name="simulator-window"></a>シミュレーター ウィンドウ

シミュレーターのウィンドウの上部にあるツールバーには、次のような便利なボタンがいくつかあります。

- **ホーム** – iOS デバイスのホーム ボタンをシミュレートします。
- **ロック** – シミュレーターをロックします (スワイプしてロックを解除します)。
- **スクリーンショット** – シミュレーターのスクリーンショットを保存します (**Pictures\Xamarin\iOS Simulator\\** に格納されます)。
- [**設定**](#settings) – キーボード、場所、およびその他の設定が表示されます。
- [**その他のオプション**](#other-options) – 回転、シェイク ジェスチャ、Touch ID などのさまざまなシミュレーター オプションが表示されます。

    [![iOS シミュレーター マップの例](images/maps-app-sml.png "iOS シミュレーター マップの例")](images/maps-app.png#lightbox)

## <a name="settings"></a>設定

ツールバーの歯車アイコンをクリックすると、 **[設定]** ウィンドウが開きます。

[![iOS シミュレーター設定](images/settings-sml.png "iOS シミュレーター設定")](images/settings.png#lightbox)

これらの設定を使用すると、ハードウェア キーボードを有効にしたり、デバイスが報告する場所を選択したり (静的な場所と動的な場所の両方がサポートされています)、Touch ID を有効にしたり、シミュレーターのコンテンツと設定をリセットしたりすることができます。

## <a name="other-options"></a>その他のオプション

ツールバーの省略記号ボタンでは、回転、シェイク ジェスチャ、再起動などの他のオプションが表示されます。 シミュレーターのウィンドウで任意の場所を右クリックすると、これらと同じオプションを一覧として表示できます。

[![iOS シミュレーターの追加設定](images/more-sml.png "iOS シミュレーターの追加設定")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>タッチスクリーンのサポート

最近のほとんどの Windows コンピューターにはタッチスクリーンが搭載されています。 Remoted iOS Simulator for Windows ではタッチ操作がサポートされているため、物理 iOS デバイスで使用するのと同じピンチ、スワイプ、および複数の指によるタッチ ジェスチャを使用してアプリをテストすることができます。

同様に、Remoted iOS Simulator for Windows は、Windows のスタイラス入力を Apple Pencil 入力として扱います。

## <a name="sound-handling"></a>サウンド処理

シミュレーターで再生されるサウンドは、ホストの Mac のスピーカーから出力されます。
iOS サウンドは、Windows コンピューターでは聞こえません。

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator for Windows の無効化

Remoted iOS Simulator for Windows を無効にするには、 **[ツール] > [オプション] > [Xamarin] > [iOS の設定]** に移動し、 **[Remote Simulator to Windows]** をオフにします。

[![シミュレーターを使用するためのチェックボックス](images/options-sml.png "シミュレーターを使用するためのチェックボックス")](images/options.png#lightbox)

このオプションを無効にすると、デバッグ時に、接続されている Mac ビルド ホストで iOS シミュレーターが開きます。

## <a name="troubleshooting"></a>トラブルシューティング

Remoted iOS Simulator で問題が発生した場合は、次の場所でログを確認できます。

- **Mac** – `~/Library/Logs/Xamarin/Simulator.Server`
- **Windows** – `%LOCALAPPDATA%\Xamarin\Logs\Xamarin.Simulator`

[Visual Studio で問題を報告する](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio)場合は、これらのログを添付すると役立つ場合があります (アップロードを非公開にするオプションがあります)。
