---
title: Xamarin Player がライブ アプリ
description: このドキュメントでは、コードの変更をプレビューするために使用するアプリがデバイスでライブ Xamarin Live Player について説明します。 これは、セットアップ、サンプル、ログ、設定、デバイスの管理について説明します。
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/14/2017
ms.openlocfilehash: 88f7f62650484007c221aa7baaa684f872e0a8e9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794151"
---
# <a name="xamarin-live-player-app"></a>Xamarin Player がライブ アプリ

![プレビュー機能](~/media/shared/preview.png)

以下の電話で、アプリをインストールした後、[セットアップ手順](~/tools/live-player/install.md)お使いのコンピューターに接続します。 いずれかの操作を再試行してください、[アプリのサンプル](~/tools/live-player/samples.md)動作を取得します。

スタートアップ時に、Xamarin Player のライブ アプリが次のよう (iOS および Android でそれぞれ)。

![ライブ Player iOS アプリのスクリーン ショット](player-images/app-iphone-sml.png) ![ライブ Player Android アプリのスクリーン ショット](player-images/app-android-sml.png)

押すと**Visual Studio にペア**カメラを使用して、コンピューターに表示されているバーコードをスキャンします。

![IOS のバーコード スキャナのスクリーン ショット](player-images/scan-iphone-sml.png) ![Android のバーコード スキャナのスクリーン ショット](player-images/scan-android-sml.png)

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

## <a name="managing-devices"></a>デバイスの管理

最初にデバイスを接続する手順についてで[要件およびセットアップ](~/tools/live-player/install.md)です。 複数のデバイス (たとえば、iOS や Android など) をペアし、IDE を使用して管理することができます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で、次のように選択します**ツール > Xamarin Live Player > デバイスを管理しています.。**

![[デバイス] ウィンドウを管理します。](player-images/manage-tools-menu-vs.png)

このウィンドウでは、次の操作できます。

- コードをスキャンして、新しいデバイスのペア
- 代わりにその画面に表示されるコードを入力して、デバイスとペアします。
- 一覧から既存のデバイスを削除します。

このウィンドウは、デバイスの一覧からアクセスすることもできます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 用 Visual Studio で次のように選択します**ツール > (Xamarin Player Live) デバイスを管理しています.。**

![[デバイス] ウィンドウを管理します。](player-images/manage-tools-menu.png)

このウィンドウでは、次の操作できます。

- コードをスキャンして、新しいデバイスのペア
- 代わりにその画面に表示されるコードを入力して、デバイスとペアします。
- 一覧から既存のデバイスを削除します。

![[デバイス] ウィンドウを管理します。](player-images/manage.png)

このウィンドウは、デバイスの一覧からアクセスすることもできます。

![Xamarin Live プレーヤー デバイスをデバイスの一覧から選択します。](player-images/manage-device-menu.png)

-----

すべての問題を参照が発生した場合[制限事項とトラブルシューティング](~/tools/live-player/troubleshooting.md)です。

## <a name="related-links"></a>関連リンク

- [制限事項](~/tools/live-player/limitations.md)
- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [Xamarin Player のライブのサンプル](samples.md)
