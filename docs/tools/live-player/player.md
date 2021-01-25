---
title: Xamarin Live Player アプリ
description: このドキュメントでは、デバイスでライブでコード変更をプレビューするために使用できる Xamarin Live Player アプリについて説明します。 セットアップ、サンプル、ログ、設定、デバイスの管理などについて説明します。
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: davidortinau
ms.author: daortin
ms.date: 06/13/2019
ms.openlocfilehash: d79eb9ad1ca57361e063f6ce73910d6bb149cf72
ms.sourcegitcommit: 424eaef56fd2933c98e72f1d3e7ac71730fe4835
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98758072"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player アプリ

![プレビュー機能](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player プレビューが終了しました。 アプリは使用できなくなりました。 以下の手順は、Visual Studio 2017 でプレビューの使用を継続しているお客様向けに提供されています。

> [!TIP]
> Visual Studio 2019 または Visual Studio for Mac の [XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md) を使用して、編集時に画面のデザインを表示できます。

起動時に、Xamarin Live Player アプリは次のようになります。

![Live Player Android アプリのスクリーンショット](player-images/app-android-sml.png)

[ペアリング] **を Visual Studio に** 押すと、カメラを使用して、コンピューターに表示されているバーコードをスキャンします。

![Android バーコードスキャナーのスクリーンショット](player-images/scan-android-sml.png)

接続に成功した場合、コードはデバイスでほぼ即座に実行されます ( [Calculator サンプル](https://github.com/xamarin/mobile-samples/tree/master/LivePlayer/BasicCalculator)など)。

![デバイスで実行されているサンプル電卓アプリ](player-images/basic-calculator-sml.png)

## <a name="options"></a>Options

アプリの下部にある情報ボタン **(i)** を押すと、[ **オプション** ] メニューが表示されます。

[![[オプション] メニューのスクリーンショット](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>ログ

問題を診断するためのログを表示します。

### <a name="settings"></a>設定

- コンパイルエラーと実行時エラーの表示を切り替えます。
- バージョン情報。
- フィードバックを送信します。

[![設定のスクリーンショット](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>デバイスの管理

デバイスを初めて接続する場合は、「 [必要条件 & セットアップ](~/tools/live-player/install.md)」に記載されている手順に従ってください。 IDE を使用して複数のデバイスをペアリングし、それらを管理することができます。

# <a name="visual-studio-2017"></a>[Visual Studio 2017](#tab/windows)

Visual Studio で、[ツール] を選択し、[**デバイスの管理] > Xamarin Live Player >** ます。

![デバイスの管理ウィンドウ](player-images/manage-tools-menu-vs.png)

このウィンドウでは、次の操作を実行できます。

- コードをスキャンして新しいデバイスをペアリングする
- または、画面に表示されているコードを入力してデバイスをペアリングします。
- 一覧から既存のデバイスを削除する

このウィンドウには、デバイスの一覧からアクセスすることもできます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で、[**ツール] > (Xamarin Live Player) [デバイスの管理**] を選択します。

![[ツール] ウィンドウで選択したデバイスの管理画面が表示されます。](player-images/manage-tools-menu.png)

このウィンドウでは、次の操作を実行できます。

- コードをスキャンして新しいデバイスをペアリングする
- または、画面に表示されているコードを入力してデバイスをペアリングします。
- 一覧から既存のデバイスを削除する

![スクリーンショットは、アプリをプレビューするオプションが表示された [Xamarin Live Player] ウィンドウを示しています。](player-images/manage.png)

このウィンドウには、デバイスの一覧からアクセスすることもできます。

![デバイス一覧から Xamarin Live Player デバイスを選択する](player-images/manage-device-menu.png)

-----

問題が発生した場合は、「 [制限事項とトラブルシューティング」を](~/tools/live-player/troubleshooting.md)参照してください。

## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
