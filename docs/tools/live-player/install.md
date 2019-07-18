---
title: Xamarin Live Player Visual Studio の構成
description: このドキュメントでは、Xamarin Live Player を使用して、ライブ編集を実行中のアプリケーションを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: a29a637526c2829b44ae89d505dac37a648dee77
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157744"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Xamarin Live Player Visual Studio の構成

![プレビュー機能](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player のプレビューが終了しました。 アプリが使用できなくします。 以下の手順は、Visual Studio 2017 のプレビューを使用して引き続きお客様に提供されます。

> [!TIP]
> 使用することができます、 [XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)でそれらを編集すると、画面のデザインを表示するには、Visual Studio 2019 または Visual Studio for Mac。

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player を使用します。

Xamarin Live Player アプリは、デバイスで既にが必要です。 ダウンロード可能なが不要になったです。

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

## <a name="using-xamarin-live-player"></a>Xamarin Live Player を使用します。

Xamarin Live Player アプリは、デバイスで既にが必要です。 ダウンロード可能なが不要になったです。

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

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
