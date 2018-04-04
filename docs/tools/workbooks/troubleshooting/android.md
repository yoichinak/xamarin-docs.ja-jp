---
title: Android での Xamarin のブックのトラブルシューティング
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 03c775f6f14263878f9f4a4633ba72931b233b17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android での Xamarin のブックのトラブルシューティング

## <a name="emulator-support"></a>エミュレーターのサポート

Android のブックを実行するには、Android エミュレーターを使用可能にする必要があります。 物理的な Android デバイスがサポートされていません。

HAXM と共に Google のエミュレーターは、コンピューターでサポートする場合はお勧めします。
HYPER-V では、システムで有効にする必要があります、代わりに Visual Studio の Android エミュレーターに移動します。

5.0 以降、Android を実行するエミュレーターが必要です。 ARM エミュレーターはサポートされていません。 使用して`x86`または`x86_64`デバイスのみです。

お読みください[Android エミュレーターを設定する方法、ドキュメント][ android-emu]プロセスに慣れていない場合。

> [!NOTE]
> 1.1 およびそれ以前のブックを再試行してください (され失敗!) を使用して ARM エミュレーター可能な場合にします。 開く、または Android のブックを作成する前に、選択した場合は、この、起動、x86 エミュレーターを回避します。 ブックは、互換性がある限り、実行中のエミュレーターへの接続に常に優先します。

## <a name="workbooks-wont-load"></a>ブックは読み込まれません

### <a name="workbook-window-spins-forever-never-loads-windows"></a>ブック ウィンドウの回転、forever しない読み込み (Windows)

最初に、エミュレーターがエミュレーターの web ブラウザーで任意の web サイトをテストすることによって完全作業用のネットワーク アクセスを持つことを確認してください。

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio の Android エミュレーターは、インターネットに接続できません。

エミュレーターがネットワーク アクセスを持たない場合は、HYPER-V ネットワーク スイッチの修正手順に従う必要があります。 Wi-fi ネットワークに頻繁に切り替える場合は、これを定期的に繰り返す必要があります。

0. **インターネットから Windows を一時的に切断することがありますこれは、すべての重要なネットワーク操作が完了したら、確認してください。**
1. エミュレーターを終了します。
2. `Hyper-V Manager` を開きます。
3. `Actions`、開かれている`Virtual Switch Manager...`です。
4. すべての仮想スイッチを削除します。
5. [`OK`] をクリックします。
6. VS Android エミュレーターを起動します。 仮想ネットワーク スイッチを再作成を求められます可能性があります。
7. VS の Android エミュレーターのブラウザーがインターネットにアクセスできることをテストします。

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>関連リンク

- [バグの報告](~/tools/workbooks/install.md#reporting-bugs)
