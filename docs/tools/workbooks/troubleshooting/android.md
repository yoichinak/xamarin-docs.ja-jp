---
title: Android での Xamarin Workbooks のトラブルシューティング
description: このドキュメントでは、Android での Xamarin Workbooks の使用に関するトラブルシューティングのヒントを提供します。 エミュレーターのサポート、読み込まれないブック、およびその他のトピックについて説明します。
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 0d04b42a8d9f230c48bb09059296eb3740336dc6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511842"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android での Xamarin Workbooks のトラブルシューティング

## <a name="emulator-support"></a>エミュレーターのサポート

Android ブックを実行するには、Android エミュレーターを使用できるようにする必要があります。 Android の物理デバイスはサポートされていません。

コンピューターでサポートされている場合は、Google のエミュレーターと HAXM を使用することをお勧めします。
システムで Hyper-v が有効になっている必要がある場合は、代わりに Visual Studio Android Emulator にアクセスしてください。

Android 5.0 以降を実行するエミュレーターが必要です。 ARM エミュレーターはサポートされていません。 また`x86` は`x86_64`デバイスのみを使用します。

このプロセスに慣れていない場合は、 [Android エミュレーターの設定に関するドキュメントを][android-emu]参照してください。

> [!NOTE]
> ブック1.1 以前のバージョンでは、使用可能な場合は ARM エミュレーターを使用します。 この問題を回避するには、Android ブックを開いたり作成したりする前に、任意の x86 エミュレーターを起動します。 ブックは、互換性がある限り、実行中のエミュレーターに接続することが常に優先されます。

## <a name="workbooks-wont-load"></a>ブックが読み込まれない

### <a name="workbook-window-spins-forever-never-loads-windows"></a>ブックウィンドウは無限にスピンし、読み込みは行われません (Windows)

最初に、エミュレーターの web ブラウザーで web サイトをテストして、エミュレーターのネットワークアクセスが完全に機能していることを確認します。

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android Emulator がインターネットに接続できません

エミュレーターがネットワークにアクセスできない場合は、次の手順に従って Hyper-v ネットワークスイッチを修正する必要があります。 Wi-fi ネットワークを頻繁に切り替える場合は、これを定期的に繰り返す必要があります。

1. **重要なネットワーク操作が完了していることを確認します。これにより、Windows がインターネットから一時的に切断される可能性があります。**
1. エミュレーターを閉じます。
1. `Hyper-V Manager`を開きます。
1. で、を`Virtual Switch Manager...`開きます。 `Actions`
1. すべての仮想スイッチを削除します。
1. [`OK`] をクリックします。
1. VS Android Emulator を起動します。 仮想ネットワークスイッチを再作成するよう求めるメッセージが表示される場合があります。
1. VS Android Emulator のブラウザーがインターネットにアクセスできることをテストします。

[android-emu]: ~/android/deploy-test/debugging/debug-on-emulator.md

## <a name="related-links"></a>関連リンク

- [バグの報告](~/tools/workbooks/install.md#reporting-bugs)
