---
title: Xamarin.iOS および tvOS アプリのワイヤレス展開
description: このドキュメントでは、Visual Studio for Mac や Visual Studio 2017 から iOS デバイスに Xamarin.iOS アプリをワイヤレス展開する方法について説明します。
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: 6d64acdcc84c16f33a1f543bf1c9506ae7c8e347
ms.sourcegitcommit: 2ee36611ef667affee7d417db947fbb614d75315
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479694"
---
# <a name="wireless-deployment-for-xamarinios-and-tvos-apps"></a>Xamarin.iOS および tvOS アプリのワイヤレス展開

開発者ワークフローの重要なフローの 1 つは、デバイスの展開です。 Xcode 9 では、ネットワークを介して iOS デバイスや Apple TV に展開するオプションが導入されました。アプリを展開およびデバッグするたびにデバイスを配線する必要はありません。 この機能は Visual Studio for Mac 7.4 と Visual Studio 15.6 リリースで導入されました。

このガイドでは、ネットワーク経由でデバイスとペアリングし、そのデバイスに展開する方法を詳しく説明します。

## <a name="requirements"></a>要件

ワイヤレス展開は、Visual Studio for Mac と Visual Studio の両方の機能として利用できます。

ワイヤレス展開を使用するには、以下が必要です。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

- macOS 10.12.4
- Visual Studio for Mac の最新バージョン
- Xcode 9.0 以降
- iOS 11.0 または tvOS 11.0 以降のデバイス

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Visual Studio の最新バージョン
- iOS 11.0 または tvOS 11.0 以降のデバイス

Mac ビルド ホストでは、次のコンポーネントをインストールする必要があります。

- macOS 10.12.4
- Visual Studio for Mac
- Xcode 9.0 以降

-----

## <a name="connecting-a-device"></a>デバイスを接続する

デバイスでワイヤレスに展開およびデバッグするには、Mac 上で iOS デバイスまたは Apple TV を Xcode とペアリングする必要があります。 ペアリングが済んだデバイスは、Visual Studio のデバイス ターゲットの一覧から選択できます。 

次のペアリング処理は、デバイスごとに 1 回だけ行う必要があります。 Xcode では接続設定が保持されます。

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>Xcode と iOS デバイスをペアリングする

1. Xcode を開き、**[Window] > [Devices and Simulators]** に移動します。
2. ライトニング ケーブルを使用して、iOS デバイスを Mac に接続します。 デバイスで **[このコンピューターを信頼する]** を選択する必要がある場合もあります。
3. ご使用のデバイスを選択し、**[Connect via network]** チェックボックスをオンにしてデバイスをペアリングします:![[Connect via network] オプションが表示されている [Device and Simulator] ウィンドウ](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Apple TV と Xcode をペアリングする

1. Mac と Apple TV が同じネットワークに接続していることを確認します。

2. Xcode を開き、**[Window] > [Devices and Simulators]** に移動します。

3. Apple TV で、**[設定] > [リモコンとデバイス] > [Remote App とデバイス]** に移動します。

4. Xcode の **[Discovered]** エリアで Apple TV を選択し、Apple TV に表示される確認コードを入力します。

5. **[Connect]** ボタンをクリックします。 正常にペアリングされると、Apple TV の横にネットワーク接続アイコンが表示されます。

## <a name="deploy-to-a-device"></a>デバイスを展開する

デバイスがワイヤレス接続し、展開に使用できるようになると、デバイスが USB 経由で接続されている場合と同じように、デバイス ターゲットの一覧に表示されます。

物理デバイスでテストするには、デバイスが[プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)されていなければなりません。 デバイスへの展開を試みる前に、その点を必ずご確認ください。 

iOS または tvOS デバイスに展開するには、次のステップを実行します。

1. 展開マシンとターゲット デバイスが同じワイヤレス ネットワーク上にあることを確認します。 

2. 使用するデバイスをターゲット デバイスの一覧から選択し、アプリケーションを実行します。

2. デバイスがロックされている場合、デバイスのロック解除を求めるメッセージが表示されます。 デバイスのロックを解除すると、アプリがデバイスに展開されます。

ワイヤレス展開後にワイヤレス デバッグが自動的に有効になるので、以前に設定したブレークポイントを使用し、これまでどおりにワークフローのデバッグを行えます。

## <a name="troubleshooting"></a>トラブルシューティング

1. iOS デバイスまたは Apple TV が Mac と同じネットワークに接続されていることを必ずご確認ください。

2. デバイスが Visual Studio に表示されない場合、Xcode の **[Devices and Simulators]** ウィンドウを確認します。 

    * Xcode 上でデバイスが接続済みとして表示**されない**場合には、デバイスの[ペアリング](#pair)をもう一度お試しください。

    * Xcode でデバイスが接続済みと表示される場合、Visual Studio とデバイスを再起動してみます。

3. デバイスの[プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)をまだ行っていない場合には、プロビジョニングする必要があります。

4. この機能の問題を上記のステップでは修正できない場合、[開発者コミュニティ](https://developercommunity.visualstudio.com/spaces/41/index.html)に問題を提出してください。

## <a name="related-links"></a>関連リンク

- [ワイヤレス デバイスと Xcode のペアリング](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Debug-to-iOS-Devices-Over-Wi-Fi/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
