---
title: Xamarin Live Player アプリ
description: このドキュメントでは、コードの変更をプレビューするために使用するには、アプリがデバイスで live Xamarin Live Player について説明します。 これは、セットアップ、サンプル、ログ、デバイスの管理の設定について説明します。
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: fce0eeae4ef5776842ea1b45c36163118042dc49
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157728"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player アプリ

![プレビュー機能](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player のプレビューが終了しました。 アプリが使用できなくします。 以下の手順は、Visual Studio 2017 のプレビューを使用して引き続きお客様に提供されます。

> [!TIP]
> 使用することができます、 [XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)でそれらを編集すると、画面のデザインを表示するには、Visual Studio 2019 または Visual Studio for Mac。

スタートアップ時に、Xamarin Live Player アプリは、ようになります。

![Live Player の Android アプリのスクリーン ショット](player-images/app-android-sml.png)

キーを押すと**Visual Studio とペアリング**カメラを使用して、コンピューターに表示されているバーコードをスキャンします。

![Android のバーコード スキャナーのスクリーン ショット](player-images/scan-android-sml.png)

接続が成功した場合と、コードがほぼ瞬時にデバイスで実行する必要があります (など、[電卓のサンプル](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator))。

![デバイスで実行されているサンプルの電卓アプリケーション](player-images/basic-calculator-sml.png)

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

最初にデバイスを接続する手順については、[要件およびセットアップ](~/tools/live-player/install.md)します。 複数のデバイスをペアリングし、IDE を使用して管理できます。

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

Visual Studio で、次のように選択します**ツール > Xamarin Live Player > デバイスを管理しています...**

![[デバイス] ウィンドウを管理します。](player-images/manage-tools-menu-vs.png)

このウィンドウでは、以下を実行できます。

- コードをスキャンして、新しいデバイスをペアリングします。
- また、画面に表示されるコードを入力してデバイスをペアリングします。
- 一覧から既存のデバイスを削除します。

このウィンドウは、デバイスの一覧からアクセスすることもできます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac では、次のように選択します**ツール > (Xamarin Live Player) デバイスを管理しています...**

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

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)

