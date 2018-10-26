---
title: Android での Xamarin Workbooks のトラブルシューティング
description: このドキュメントでは、android、Xamarin Workbooks を操作するためのトラブルシューティングのヒントを提供します。 これは、エミュレーターのサポート、読み込まれないブックおよびその他のトピックについて説明します。
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: a93288829ff99027a4b33e7720a7f849df37e9b1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112612"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android での Xamarin Workbooks のトラブルシューティング

## <a name="emulator-support"></a>エミュレーターのサポート

Android のブックを実行するには、Android エミュレーターを使用できるようにする必要があります。 物理 Android デバイスがサポートされていません。

コンピューターがサポートされている場合は、HAXM と Google のエミュレーターを勧めします。
HYPER-V、システムで有効にする必要があります、代わりに Visual Studio Android Emulator を使用した移動します。

5.0 以降、Android を実行しているエミュレーターが必要です。 ARM のエミュレーターがサポートされていません。 使用`x86`または`x86_64`デバイスのみです。

お読みください[Android エミュレーターの設定に関するドキュメント][ android-emu]プロセスに慣れていない場合。

> [!NOTE]
> 1.1 またはそれ以前のブック (を失敗!) を使用している場合、ARM エミュレーターを使用します。 開く、または Android のブックを作成する前に、選択した場合は、この、起動、x86 エミュレーターを回避します。 ブックは互換性がある限り、実行中のエミュレーターへの接続に常に優先します。

## <a name="workbooks-wont-load"></a>ブックが読み込まれない

### <a name="workbook-window-spins-forever-never-loads-windows"></a>ブック ウィンドウには (Windows) の負荷を回転、永久にことはありません。

最初に、エミュレーターが、エミュレーターの web ブラウザーで任意の web サイトをテストして、ネットワーク アクセスを完全に作業が持つことを確認します。

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android Emulator は、インターネットに接続できません。

エミュレーターがネットワーク アクセスを持たない場合は、HYPER-V ネットワーク スイッチを解決する手順に従う必要があります。 Wi-fi ネットワークに頻繁に切り替えた場合は、これを定期的に繰り返す必要があります。

0. **すべての重要なネットワーク操作は完了するは、インターネットから Windows を一時的に切断することがありますこれを確認します。**
1. エミュレーターを閉じます。
2. `Hyper-V Manager`を開きます。
3. `Actions`オープン`Virtual Switch Manager...`します。
4. すべての仮想スイッチを削除します。
5. [`OK`] をクリックします。
6. VS Android エミュレーターを起動します。 おそらく、仮想ネットワーク スイッチを再作成するように求めされます。
7. VS の Android エミュレーターのブラウザーがインターネットにアクセスできることをテストします。

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>関連リンク

- [バグの報告](~/tools/workbooks/install.md#reporting-bugs)
