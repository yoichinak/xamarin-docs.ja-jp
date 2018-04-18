---
title: Xamarin Player のライブのセットアップ
description: 編集し、同時にリアルタイムで iOS または Android デバイスでアプリをテスト
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/22/2017
ms.openlocfilehash: 1c1d8e24ecea2e1606f7f134aaa5ecf619e155c6
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="xamarin-live-player-setup"></a>Xamarin Player のライブのセットアップ

Xamarin Live Player には、アプリにライブの編集を行うことができ、それらの変更反映ライブ デバイスにします。 Xamarin Player のライブ アプリ – エミュレーターを設定するか、ケーブルを使用して展開する必要はありません内、コードで実行されます。 この記事では、Xamarin Live Player を設定する方法について説明します。

![プレビュー機能](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1.アプリを入手します。

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Live Player は、Google Play から Android 用に使用できます。

[ ![Google Play に掲載](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

せず Google Play の Android デバイス用の Xamarin Live Player がを通じて使用可能な[HockeyApp](https://aka.ms/xlp-hockeyapp)配布します。 For Android は、の使用を選択して Google Play から直接インストールできますが初期プレビューでさらに、ビルド、[開くベータ プログラム](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

ユーザーの参加をお勧め、 [Xamarin Player のライブ アプリ_iOS プレビュー_ ](https://aka.ms/liveplayeralpha) TestFlight を介して最新機能へのクイック アクセスを利用します。

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2.Visual Studio 2017 を取得します。

Xamarin Live Player が必要です。

- Visual Studio 2017 15.4年またはそれ以降。
- Visual Studio コンピューターと同じ WiFi ネットワーク上のデバイス。

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3.初めて Xamarin Live Player の使用

1. 開いている**Visual Studio 2017**です。
2. 移動して**ツール > オプション.**を選択し、 **Xamarin > その他の**タブです。
3. ティック**Xamarin ライブ Player を有効にする**:

  ![Xamarin Live Player を有効にするボックスをオプションのウィンドウ](install-images/vs2017-options.png)

2. 作成するか、Xamarin のプロジェクトを開きます (または[サンプル](~/tools/live-player/samples.md))。
3. 選択**Live Player**デバイスの一覧で。

  ![デバイスの一覧には、Xamarin Live Player オプションが含まれています。](install-images/devices-empty-windows.png)

  * デバイスが既にペアリングされてする場合は、オプションとして使用ができます。
  * それ以外の場合に必要なときに、デバイスとペアに求められます。
4. キーを押して、**実行**ボタン、または選択してから、次のいずれかのオプション、**実行**または右クリック メニュー。

  - **デバッグなしで開始**– デバイスの変更が発生する、アプリを行うことができます (アプリが再起動変更を加えるし、ファイルを保存) します。
  - **デバッグを開始**– ブレークポイントを設定し、変数を調べることができますが、コードを編集することはできません。

  また、選択**ツール > Xamarin Live Player > 現在のビューの実行をライブ**アプリとデバイスの変更が発生するを参照してくださいを編集することができます。 (アプリケーションのメイン画面) ではなく、現在のビューが表示されます。

5. デバイスは既にペアリングされて、Xamarin Player のライブ アプリがデバイスで実行されている場合は、コードは直ちに実行されます。

  デバイスがない場合は、関連付ける、方法について記載された QR コードが表示されます、デバイスとペアに。

  ![[デバイス] ウィンドウのペア](install-images/manage-empty-windows.png)

  組み合わせについて、デバイスに接続できない場合、エラーが表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2.Visual Studio を Mac を取得します。

Xamarin Live Player が必要です。

- OS X 10.11、macOS 10.12、または大きい値
- Visual Studio for Mac
- Mac と同じ WiFi ネットワーク上のデバイス

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3.初めて Xamarin ライブ Player の使用

1. 開いている**Visual Studio for Mac**です。
2. 移動して**Visual Studio > 設定しています.**を選択し、**プロジェクト > Live Xamarin Player (プレビュー)**タブです。
3. ティック**Xamarin ライブ Player を有効にする**:

  [![Xamarin Live Player を有効にするボックスをオプションのウィンドウ](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. 作成するか、Xamarin のプロジェクトを開きます (または[サンプル](~/tools/live-player/samples.md))。
3. 選択**Live Player**デバイス一覧にします。

  ![デバイスの一覧には、Xamarin Live Player オプションが含まれています。](install-images/devices.png)

  * デバイスが既にペアリングされてする場合は、オプションとして使用ができます。
  * それ以外の場合に必要なときに、デバイスとペアに求められます。
  * 選択**Xamarin ライブ プレーヤー デバイスしています.** Xamarin Live Player を使用するデバイスを管理します。

4. キーを押して、**実行**ボタン、または選択してから、次のいずれかのオプション、**実行**または右クリック メニュー。

  ![メニュー オプションを実行します。](install-images/run-menu.png)

  - **デバッグなしで開始**– デバイスの変更が発生する、アプリを行うことができます (アプリが再起動変更を加えるし、ファイルを保存) します。
  - **デバッグを開始**– ブレークポイントを設定し、変数を調べることができますが、コードを編集することはできません。
  - **現在のビューの実行をライブ**– デバイスの変更が発生する、アプリを行うことができます。 (アプリケーションのメイン画面) ではなく、現在のビューが表示されます。

5. デバイスは既にペアリングされて、Xamarin Player のライブ アプリがデバイスで実行されている場合は、コードは直ちに実行されます。

  デバイスがペアになっていない場合、デバイスをペアリングする方法について QR コードに表示されます。

  ![[デバイス] ウィンドウのペア](install-images/manage-empty.png)

  組み合わせについて、デバイスに接続できない場合、エラーが表示されます。

  ![デバイスのエラー メッセージに接続できません。](install-images/error-cannot-connect.png)


-----

問題が発生して接続できません。 するかを参照してください。[制限事項とトラブルシューティング](~/tools/live-player/troubleshooting.md)です。


## <a name="related-links"></a>関連リンク

- [制限事項](~/tools/live-player/limitations.md)
- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
- [Xamarin Player のライブのサンプル](~/tools/live-player/samples.md)
