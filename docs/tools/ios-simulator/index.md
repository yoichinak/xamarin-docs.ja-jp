---
title: Remoted iOS Simulator for Windows
description: Windows 用のリモート iOS シミュレーターを使用すると、Visual Studio 2019 と同時に Windows に表示される iOS シミュレーターでアプリをテストできます。
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: conceptdev
ms.author: crdun
ms.date: 04/26/2019
ms.openlocfilehash: 3067493315c62c46f44fb94aa61a919e1449080b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289708"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator for Windows

Windows 用のリモート iOS シミュレーターを使用すると、Visual Studio 2019 および Visual Studio 2017 と共に Windows に表示される iOS シミュレーターでアプリをテストできます。

[![Windows で実行されている iOS シミュレーター](images/hero-sml.png "Windows で実行されている iOS シミュレーター")](images/hero.png#lightbox)

## <a name="getting-started"></a>作業の開始

Windows 用のリモート iOS シミュレーターは、Xamarin の一部として Visual Studio 2019 および Visual Studio 2017 に自動的にインストールされます。 使用するには、次の手順を実行します。

1. [Visual 2019 を Mac ビルドホストにペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)します。
2. Visual Studio で、iOS または tvOS プロジェクトのデバッグを開始します。 Windows 用のリモート iOS シミュレーターが Windows コンピューターに表示されます。

ステップバイステップガイドについては、こちらの[ビデオ](deploy.md)をご覧ください。

## <a name="simulator-window"></a>シミュレーターウィンドウ

シミュレーターのウィンドウの上部にあるツールバーには、いくつかの便利なボタンがあります。

- **Home** – iOS デバイスの [ホーム] ボタンをシミュレートします。
- **Lock** –シミュレーターをロックします (ロック解除するにはスワイプします)。
- **スクリーンショット**–シミュレーター ( **Pictures\Xamarin\iOS シミュレーター\\** に格納されています) のスクリーンショットを保存します。
- [[**設定**](#settings)] –キーボード、場所、およびその他の設定が表示されます。
- [**その他のオプション**](#other-options)–回転、シェイクジェスチャ、タッチ ID などのさまざまなシミュレーターオプションが表示されます。

    [![iOS シミュレーターマップの例](images/maps-app-sml.png "iOS シミュレーターマップの例")](images/maps-app.png#lightbox)

## <a name="settings"></a>設定

ツールバーの歯車アイコンをクリックすると、 **[設定]** ウィンドウが開きます。

[![iOS シミュレーターの設定](images/settings-sml.png "iOS シミュレーターの設定")](images/settings.png#lightbox)

これらの設定を使用すると、ハードウェアキーボードを有効にしたり、デバイスが報告する場所 (静的な場所と移動先の両方がサポートされている場所) を選択したり、タッチ ID を有効にしたり、シミュレーターのコンテンツと設定をリセットしたりすることができます。

## <a name="other-options"></a>その他のオプション

ツールバーの省略記号ボタンをクリックすると、回転、シェイクジェスチャ、再起動などの他のオプションがわかります。 これらの同じオプションは、シミュレーターのウィンドウで任意の場所を右クリックして、一覧として表示できます。

[![iOS シミュレーターの追加設定](images/more-sml.png "iOS シミュレーターの追加設定")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>タッチスクリーンのサポート

最新の Windows コンピューターにはタッチスクリーンが搭載されています。 Windows 用のリモート iOS シミュレーターではタッチ操作がサポートされているため、物理 iOS デバイスで使用するのと同じピンチ、スワイプ、および複数の指によるタッチジェスチャを使用してアプリをテストすることができます。

同様に、Windows 用のリモート iOS シミュレーターは、Windows のスタイラス入力を Apple 鉛筆入力として扱います。

## <a name="sound-handling"></a>サウンド処理

シミュレーターによって再生されるサウンドは、ホストの Mac のスピーカーから取得されます。
Windows コンピューターでは、iOS サウンドが聞こえません。

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Windows 用のリモート iOS シミュレーターを無効にする

リモートの iOS シミュレーター for Windows を無効にするには、**ツール > オプション に移動し > Xamarin > iOS の設定**に移動し、**リモートシミュレーター を**オフにします。

[![シミュレーターを使用するためのチェックボックス](images/options-sml.png "シミュレーターを使用するためのチェックボックス")](images/options.png#lightbox)

このオプションを無効にすると、接続されている Mac ビルドホストで iOS シミュレーターが開きます。
