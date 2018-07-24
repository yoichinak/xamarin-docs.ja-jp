---
title: Xamarin Live Player アプリ
description: このドキュメントでは、コードの変更をプレビューするために使用するには、アプリがデバイスで live Xamarin Live Player について説明します。 これは、セットアップ、サンプル、ログ、デバイスの管理の設定について説明します。
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/14/2017
ms.openlocfilehash: 88f7f62650484007c221aa7baaa684f872e0a8e9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830414"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player アプリ

![プレビュー機能](~/media/shared/preview.png)

次のスマート フォンにアプリをインストールすると、[セットアップ手順](~/tools/live-player/install.md)コンピューターに接続します。 いずれかの操作を再試行してください、[サンプル アプリ](~/tools/live-player/samples.md)これを利用します。

スタートアップ時に、次のように次の Xamarin Live Player アプリ (iOS と Android でそれぞれ)。

![Live Player の iOS アプリのスクリーン ショット](player-images/app-iphone-sml.png) ![Live Player の Android アプリのスクリーン ショット](player-images/app-android-sml.png)

キーを押すと**Visual Studio とペアリング**カメラを使用して、コンピューターに表示されているバーコードをスキャンします。

![IOS のバーコード スキャナーのスクリーン ショット](player-images/scan-iphone-sml.png) ![Android のバーコード スキャナーのスクリーン ショット](player-images/scan-android-sml.png)

接続が成功した場合と、コードがほぼ瞬時にデバイスで実行する必要があります (など、[電卓のサンプル](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator))。

![デバイスで実行されているサンプルの電卓アプリケーション](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>オプション

情報ボタンを押して **(i)** を表示するアプリの下部にある、**オプション**メニュー。

[![[オプション] メニューのスクリーン ショット](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>ログ

問題の診断ログを表示します。

### <a name="settings"></a>設定

- コンパイルと実行時のエラーの表示を切り替えます。
- バージョン情報です。
- フィードバックを送信します。

[![設定のスクリーン ショット](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>デバイスを管理します。

最初にデバイスを接続する手順については、[要件およびセットアップ](~/tools/live-player/install.md)します。 (たとえば、iOS および Android) の複数のデバイスをペアリングし、IDE を使用して管理できます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で、次のように選択します**ツール > Xamarin Live Player > デバイスを管理しています.。**

![[デバイス] ウィンドウを管理します。](player-images/manage-tools-menu-vs.png)

このウィンドウでは、以下を実行できます。

- コードをスキャンして、新しいデバイスをペアリングします。
- また、画面に表示されるコードを入力してデバイスをペアリングします。
- 一覧から既存のデバイスを削除します。

このウィンドウは、デバイスの一覧からアクセスすることもできます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac では、次のように選択します**ツール > (Xamarin Live Player) デバイスを管理しています.。**

![[デバイス] ウィンドウを管理します。](player-images/manage-tools-menu.png)

このウィンドウでは、以下を実行できます。

- コードをスキャンして、新しいデバイスをペアリングします。
- また、画面に表示されるコードを入力してデバイスをペアリングします。
- 一覧から既存のデバイスを削除します。

![[デバイス] ウィンドウを管理します。](player-images/manage.png)

このウィンドウは、デバイスの一覧からアクセスすることもできます。

![Xamarin Live Player デバイスをデバイスの一覧から選択します。](player-images/manage-device-menu.png)

-----

すべての問題を参照が発生した場合[制限事項とトラブルシューティング](~/tools/live-player/troubleshooting.md)します。

## <a name="related-links"></a>関連リンク

- [制限事項](~/tools/live-player/limitations.md)
- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [Xamarin Live Player のサンプル](samples.md)
