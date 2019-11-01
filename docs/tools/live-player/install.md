---
title: Xamarin Live Player Visual Studio の構成
description: このドキュメントでは、Xamarin Live Player を使用して、実行中のアプリケーションをライブ編集する方法について説明します。
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: davidortinau
ms.author: daortin
ms.date: 06/13/2019
ms.openlocfilehash: 3dfe63cf3cf87a99d15879a0d4791248fa06f195
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029687"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Xamarin Live Player Visual Studio の構成

![プレビュー機能](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player プレビューが終了しました。 アプリは使用できなくなりました。 以下の手順は、Visual Studio 2017 でプレビューの使用を継続しているお客様向けに提供されています。

> [!TIP]
> Visual Studio 2019 または Visual Studio for Mac の[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)を使用して、編集時に画面のデザインを表示できます。

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player の使用

デバイスに Xamarin Live Player アプリが既にある必要があります。 ダウンロードすることはできません。

1. **Visual Studio 2017**を開きます。
2. [**ツール] > [オプション**] にアクセスし、 **[Xamarin > その他]** タブを選択します。
3. ティックの**有効化 Xamarin Live Player**:

    ![[オプション] ウィンドウの [有効化 Xamarin Live Player] チェックボックスをオンにします。](install-images/vs2017-options.png)

4. Xamarin プロジェクト (または[サンプル](~/tools/live-player/samples.md)) を作成または開きます。
5. デバイスの一覧で **[Live Player]** を選択します。

    ![デバイス一覧に Xamarin Live Player オプションが含まれています](install-images/devices-empty-windows.png)

    - 既にデバイスをペアリングしている場合は、オプションとして使用できます。
    - それ以外の場合は、必要に応じてデバイスをペアリングするように求められます。

6. **[実行]** ボタンを押すか、または **[実行]** メニューまたは右クリックメニューから次のいずれかのオプションを選択します。

    - **[デバッグなしで開始]** –アプリを編集して、デバイスで変更が発生したことを確認できます (変更が加えられ、ファイルが保存されると、アプリが再起動されます)。
    - **デバッグを開始**する–ブレークポイントを設定し、変数を検査できますが、コードを編集することはできません。

    または、ツール Xamarin Live Player > を選択し、**ライブ実行の現在のビュー** を > します。これにより、アプリを編集して、デバイスで行われた変更を確認できます。 (アプリケーションのメイン画面ではなく) 現在のビューが表示されます。

7. デバイスが既にペアリングされており、Xamarin Live Player アプリがデバイスで実行されている場合、コードはすぐに実行されます。

    デバイスがペアリングされていない場合は、デバイスをペアリングする方法を説明する QR コードが表示されます。

    ![デバイスウィンドウをペアリングする](install-images/manage-empty-windows.png)

    ペアリングのためにデバイスに接続できない場合は、エラーが表示されることがあります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player の使用

デバイスに Xamarin Live Player アプリが既にある必要があります。 ダウンロードすることはできません。

1. **Visual Studio for Mac**を開きます。
2. **Visual Studio > の [基本設定...** ] にアクセスし、 **[プロジェクト > Xamarin Live Player (プレビュー)]** タブを選択します。
3. ティックの**有効化 Xamarin Live Player**:

    [![[オプション] ウィンドウの [Xamarin Live Player を有効にする] チェックボックスをオンにします。](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Xamarin プロジェクト (または[サンプル](~/tools/live-player/samples.md)) を作成または開きます。
5. デバイスの一覧で **[Live Player]** を選択します。

    ![デバイス一覧に Xamarin Live Player オプションが含まれています](install-images/devices.png)

    - 既にデバイスをペアリングしている場合は、オプションとして使用できます。
    - それ以外の場合は、必要に応じてデバイスをペアリングするように求められます。
    - Xamarin Live Player で使用するデバイスを管理するには、 **[Xamarin Live Player デバイス...]** を選択します。

6. **[実行]** ボタンを押すか、または **[実行]** メニューまたは右クリックメニューから次のいずれかのオプションを選択します。

    ![[実行] メニューオプション](install-images/run-menu.png)

    - **[デバッグなしで開始]** –アプリを編集して、デバイスで変更が発生したことを確認できます (変更が加えられ、ファイルが保存されると、アプリが再起動されます)。
    - **デバッグを開始**する–ブレークポイントを設定し、変数を検査できますが、コードを編集することはできません。
    - **ライブ実行の現在のビュー** –アプリを編集して、デバイスで行われた変更を確認できます。 (アプリケーションのメイン画面ではなく) 現在のビューが表示されます。

7. デバイスが既にペアリングされており、Xamarin Live Player アプリがデバイスで実行されている場合、コードはすぐに実行されます。

    デバイスがペアリングされていない場合は、デバイスをペアリングする方法を説明する QR コードが表示されます。

    ![デバイスウィンドウをペアリングする](install-images/manage-empty.png)

    ペアリングのためにデバイスに接続できない場合は、次のエラーが表示されます。

    ![デバイスのエラーメッセージに接続できません](install-images/error-cannot-connect.png)

-----

問題が発生した場合、または接続できない場合は、「[制限事項とトラブルシューティング](~/tools/live-player/troubleshooting.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
