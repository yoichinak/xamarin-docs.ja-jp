---
title: Xamarin Live Player のセットアップ
description: このドキュメントでは、Xamarin Live Player を設定し、実行中のアプリケーションにライブ編集を加えるを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: f9cfc69c2cd711460233e609d63bcbb8eb172ccf
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854757"
---
# <a name="xamarin-live-player-setup"></a>Xamarin Live Player のセットアップ

Xamarin Live Player では、アプリへのライブ編集を行うことができ、それらの変更反映ライブ デバイス。 エミュレーターを設定する、またはケーブルを使用してデプロイする必要はありません – Xamarin Live Player アプリ内で、コードが実行されます。 この記事では、Xamarin Live Player を設定する方法について説明します。

![プレビュー機能](~/media/shared/preview.png)

> [!NOTE]
> Live Player のプレビューには、Visual Studio 2017 ではできるだけです。

## <a name="1-get-the-android-app"></a>1.Android アプリを入手します。

Xamarin Live Player は for Android によって直接 intalling [HockeyApp](https://aka.ms/xlp-hockeyapp)します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2.Visual Studio 2017 を取得します。

Xamarin Live Player が必要です。

- Visual Studio 2017 15.4年以降。
- Visual Studio コンピューターとが同じ WiFi ネットワーク上のデバイス。

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3.Xamarin Live Player を使用する最初の時間

1. 開いている**Visual Studio 2017**します。
2. 移動して**ツール > オプション.** を選択し、 **Xamarin > その他の**タブ。
3. ティック**Xamarin Live Player を有効にする**:

    ![チェック オプション ウィンドウで、Xamarin Live Player を有効にするボックス](install-images/vs2017-options.png)

4. 作成するか、Xamarin プロジェクトを開きます (または[サンプル](~/tools/live-player/samples.md))。
5. 選択**Live Player**デバイスの一覧で。

    ![デバイスの一覧には、Xamarin Live Player のオプションが含まれています。](install-images/devices-empty-windows.png)

    - 既にデバイスをペアリングがある場合は、オプションとして使用ができます。
    - それ以外の場合求められますを必要な場合にデバイスをペアリングします。

6. キーを押して、**実行**ボタン、または、次のいずれかのオプションから選択、**実行**または右クリック メニュー。

    - **デバッグなしで開始**– アプリとデバイスの変更が発生する参照を編集することができます (アプリが、変更を加えるし、ファイルの保存と再起動される)。
    - **デバッグの開始**– ブレークポイントを設定し、変数を検査することができますが、コードを編集することはできません。

    または、選択**ツール > Xamarin Live Player > 現在のビューの実行を Live**アプリとデバイスの変更が発生する参照を編集することができます。 (アプリケーションのメイン画面) ではなく、現在のビューが表示されます。

7. デバイスは既にペアリングされています、Xamarin Live Player アプリがデバイスで実行されている場合は、コードはすぐ実行されます。

    デバイスをペアリングしていない場合、デバイスをペアリングする手順について QR コードに表示されます。

    ![ペアのデバイス ウィンドウ](install-images/manage-empty-windows.png)

    ペアリングには、デバイスに接続できない場合は、エラーが表示される可能性があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2.Visual Studio for Mac を入手します。

Xamarin Live Player が必要です。

- OS X 10.11、macOS 10.12、またはそれ以上
- Visual Studio for Mac
- Mac とが同じ WiFi ネットワーク上のデバイス

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3.Xamarin Live Player を使用する最初の時間

1. 開いている**Visual Studio for Mac**します。
2. 移動して**Visual Studio > の基本設定.** を選択し、**プロジェクト > Xamarin Live Player (プレビュー)** タブ。
3. ティック**Xamarin Live Player を有効にする**:

    [![Xamarin Live Player を有効にするボックス](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. 作成するか、Xamarin プロジェクトを開きます (または[サンプル](~/tools/live-player/samples.md))。
5. 選択**Live Player**デバイスの一覧にします。

    ![デバイスの一覧には、Xamarin Live Player のオプションが含まれています。](install-images/devices.png)

    - 既にデバイスをペアリングがある場合は、オプションとして使用ができます。
    - それ以外の場合求められますを必要な場合にデバイスをペアリングします。
    - 選択**Xamarin Live Player デバイスしています.** の Xamarin Live Player を使用するデバイスを管理します。

6. キーを押して、**実行**ボタン、または、次のいずれかのオプションから選択、**実行**または右クリック メニュー。

    ![メニュー オプションを実行します。](install-images/run-menu.png)

    - **デバッグなしで開始**– アプリとデバイスの変更が発生する参照を編集することができます (アプリが、変更を加えるし、ファイルの保存と再起動される)。
    - **デバッグの開始**– ブレークポイントを設定し、変数を検査することができますが、コードを編集することはできません。
    - **ライブ実行の現在のビュー** – アプリとデバイスの変更が発生する参照を編集することができます。 (アプリケーションのメイン画面) ではなく、現在のビューが表示されます。

7. デバイスは既にペアリングされています、Xamarin Live Player アプリがデバイスで実行されている場合は、コードはすぐ実行されます。

    デバイスをペアリングしていない場合、デバイスをペアリングする手順について QR コードに表示されます。

    ![ペアのデバイス ウィンドウ](install-images/manage-empty.png)

    ペアリングには、デバイスに接続できない場合は、エラーが表示されます。

    ![エラー メッセージをデバイスに接続できません。](install-images/error-cannot-connect.png)

-----

問題が発生するか、または接続できない場合は、[制限事項とトラブルシューティング](~/tools/live-player/troubleshooting.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [Live Player を使用するサンプル](https://developer.xamarin.com/samples/xamarin-live-player/all/)
- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
