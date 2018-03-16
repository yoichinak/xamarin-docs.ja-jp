---
title: Xamarin Android Device Manager
description: "現在プレビュー中の Xamarin Android Device Manager は、Google の従来のデバイス マネージャーに代わるものです。 このガイドでは、Xamarin Android Device Manager を使って、Android デバイスをエミュレートする Android 仮想デバイス (AVD) を作成および構成する方法を説明します。 仮想デバイスを使うと、物理デバイスがなくてもアプリを実行してテストすることができます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: c38a0a7f6897cd90f81c92348280539b33524b9c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="xamarin-android-device-manager"></a>Xamarin Android Device Manager

_現在プレビュー中の Xamarin Android Device Manager は、Google の従来のデバイス マネージャーに代わるものです。このガイドでは、Xamarin Android Device Manager を使って、Android デバイスをエミュレートする Android 仮想デバイス (AVD) を作成および構成する方法を説明します。仮想デバイスを使うと、物理デバイスがなくてもアプリを実行してテストすることができます。_

![現在プレビュー中](~/media/shared/preview.png)

 
## <a name="overview"></a>概要

ハードウェアの高速化が有効になっていることを確認した後は (「[ハードウェアの高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)」を参照)、アプリのテストとデバッグに使う仮想デバイスを作成します。 Xamarin Android Device Manager を使って、Android SDK エミュレーターで使うための仮想デバイスを作成できます。

[Google デバイス マネージャー](~/android/get-started/installation/android-emulator/google-emulator-manager.md)の代わりに Xamarin Android Device Manager を使うのはなぜでしょうか。
Android SDK Tools バージョン 26.0.1 の時点で、Google は、新しい CLI (コマンド ライン インターフェイス) ツールを優先し、UI ベースの AVD および SDK マネージャーをサポートしなくなりました。 この変更により、Android SDK Tools 26.0.1 以降 (Android 8.0 Oreo の開発に必要) に更新するときは、[Xamarin SDK Manager](~/android/get-started/installation/android-sdk.md) と Xamarin Android Device Manager を使う必要があります。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

このガイドでは、Windows (または [Mac](?tabs=vsmac)) 上の Visual Studio に Xamarin Android Device Manager をインストールして使用する方法を説明します。

[![Xamarin Android Device Manager の [Devices]\(デバイス\) タブのスクリーンショット](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

このガイドでは、Mac 用 (または [Windows](?tabs=vswin) 用) の Visual Studio に Xamarin Android Device Manager をインストールして使用する方法を説明します。

[![Xamarin Android Device Manager の [Devices]\(デバイス\) タブのスクリーンショット](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> このガイドは、Visual Studio for Mac のみに該当します。
Xamarin Studio は、Xamarin Android Device Manager と互換性があります。

-----

[Android SDK エミュレーター](~/android/deploy-test/debugging/android-sdk-emulator/index.md)で実行する *Android 仮想デバイス* (AVD) を作成および構成するには、Xamarin Android Device Manager を使います。
各 AVD は、物理的な Android デバイスをシミュレートするエミュレーター構成です。 これにより、異なる物理 Android デバイスをシミュレートするさまざまな構成でアプリを実行してテストすることができます。 Xamarin Android Device Manager は、Google の (推奨されなくなった) スタンドアロン AVD Manager に代わるものです。

このガイドでは、Android Device Manager をインストールして開始する方法を説明します。 仮想デバイスを作成、複製、カスタマイズ、起動する方法を学習します。 このガイドでは、各仮想デバイスのプロパティ (API レベル、CPU、メモリ、解像度など) を構成して、加速度計、GPS、向き、光センサーなどのシミュレートされたセンサーを有効/無効にする方法、およびその仮想デバイスで使われるハードウェア高速化の種類を構成する方法についても説明します。

## <a name="requirements"></a>必要条件

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android Device Manager を使うには、次のものが必要です。

-   Visual Studio 2017 バージョン 15.5 以降。 Visual Studio Community エディション以上がサポートされます。

-   Xamarin for Visual Studio バージョン 4.8 以降。 Xamarin の更新については、「[Change the Updates Channel](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)」(更新チャネルを変更する) をご覧ください。

-   Windows 用 [Xamarin Device Manager インストーラー](https://go.microsoft.com/fwlink/?linkid=865528)の最新バージョン。

-   **Android SDK** &ndash; Android SDK をインストールし (「[Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照)、次のセクションで説明するように SDK Tools バージョン 26.0 をインストールする必要があります。 次の場所に Android SDK をインストールします (まだインストールされていない場合): **C:\\Program Files (x86)\\Android\\android-sdk**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Visual Studio for Mac 7.4 以降。

-   macOS 用 [Xamarin Device Manager インストーラー](https://go.microsoft.com/fwlink/?linkid=865527)の最新バージョン。

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) 以降を SDK Manager でインストールする必要があります。

-----

 
## <a name="installing-the-device-manager"></a>Device Manager のインストール

Xamarin Android Device Manager をインストールするには次の手順のようにします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Windows 用 [Xamarin Device Manager インストーラー](https://go.microsoft.com/fwlink/?linkid=865528)をダウンロードします。

2. **Xamarin.DeviceManager.msi** をダブルクリックして、インストールの指示に従います。 

    ![Xamarin Android Device Manager セットアップ ウィザード](xamarin-device-manager-images/win/30-installer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. macOS 用 [Xamarin Device Manager インストーラー](https://go.microsoft.com/fwlink/?linkid=865527)をダウンロードします。

2. **AndroidDevices.pkg** をダブルクリックして、インストールの指示に従います。 

    [![Xamarin Android Device Manager セットアップ ウィザード](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png#lightbox)

-----

 
## <a name="launching-the-device-manager"></a>Device Manager の起動

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 15.6 Preview 3 以降では、**[ツール]** メニューから Xamarin Android Device Manager を起動することができます。 Visual Studio 15.6 Preview 3 以降を使用している場合は、**[ツール] > [Android エミュレーター マネージャー]** をクリックしてデバイス マネージャーを起動します。

[![[ツール] メニューから起動します](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

以前のバージョンの Visual Studio を使用している場合は、Xamarin Android Device Manager を Windows の**[スタート]** メニューから起動する必要があります。

![[スタート] メニューの Xamarin Android Device Manager](xamarin-device-manager-images/win/31-start-menu.png)

**Xamarin Android Device Manager** を右クリックして、**[その他] > [管理者として実行]** を選びます。 起動時に次のエラー ダイアログが表示される場合は、「[トラブルシューティング](#troubleshooting)」セクションの回避策の説明をご覧ください。

![Android SDK インスタンスのエラー](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac 7.6 Preview 3 (現在アルファ チャネル中) 以降では、**[ツール] > [Emulator Manager]\(エミュレーター マネージャー\)** を選択すると Xamarin Android Device Manager を起動することができます。

[![[ツール] メニューから起動します](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

以前のバージョンの Visual Studio for Mac を使用している場合は、Xamarin Android Device Manager を個別に起動する必要があります。 **[アプリケーション]** フォルダーで **[Android デバイス]** を探してダブルクリックすると起動します。

[![ファインダーでの Xamarin Android Device Manager の場所](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png#lightbox)


-----

Android Device Manager を使う前に、Android SDK Tools バージョン 26.0.0 以降をインストールする必要があります。 Android SDK Tools 26.0.0 以降がインストールされていない、起動時に次のエラー ダイアログが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android SDK インスタンスのエラー ダイアログ](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android SDK インスタンスのエラー ダイアログ](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

このエラー ダイアログが表示された場合は、**[OK]** をクリックして Android SDK Manager を開きます。 Android SDK Manager の **[ツール]** タブをクリックして、**Android SDK Tools 26.0.2** 以降、**Android SDK Platform-Tools 26.0.0** 以降、**Android SDK Build-Tools 26.0.0** 以降をインストールします。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android SDK Tools 26.0 のインストール](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

これらのパッケージをインストールした後は、SDK Manager を閉じて Android Device Manager を再起動できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android SDK Tools 26.0 のインストール](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

 
## <a name="main-screen"></a>メイン画面

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android Device Manager を初めて起動すると、現在構成されているすべての仮想デバイスが画面に表示されます。 デバイスごとに、**名前**、**オペレーティング システム** (Android API レベル)、**CPU**、**メモリ** サイズ、および画面解像度が表示されます。

[![インストールされているデバイスの一覧とそれらのパラメーター](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android Device Manager を初めて起動すると、現在構成されているすべての仮想デバイスが画面に表示されます。 デバイスごとに、**名前**、**システム イメージ** (Android API レベル)、**CPU**、**メモリ** サイズ、および画面解像度が表示されます。

[![インストールされているデバイスの一覧とそれらのパラメーター](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

一覧のデバイスをクリックすると、右側に **[Start]\(開始\)** ボタンが表示されます。 **[Start]\(開始\)** ボタンをクリックして、この仮想デバイスのエミュレーターを起動できます。

[![デバイス イメージの [Start]\(開始\) ボタン](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

選んだ仮想デバイスのエミュレーターを起動するには、**[Play]\(再生\)** ボタンをクリックします。
 
[![デバイス イメージの [Start]\(開始\) ボタン](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

選んだ仮想デバイスでエミュレーターが開始した後、**[Start]\(開始\)** ボタンは **[Stop]\(停止\)** ボタンに変わり、このボタンを使ってエミュレーターを停止できます。

[![実行中のデバイスの [Stop]\(停止\) ボタン](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

選んだ仮想デバイスでエミュレーターが開始した後、**[Play]\(再生\)** ボタンは **[Stop]\(停止\)** ボタンに変わり、このボタンを使ってエミュレーターを停止できます。
 
[![実行中のデバイスの [Stop]\(停止\) ボタン](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)
 
-----

 
### <a name="new-device"></a>新しいデバイス

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

新しいデバイスを作成するには、**[New]\(新規\)** ボタン (画面の右上にあります) をクリックします。

[![新しいデバイスを作成するための [New]\(新規\) ボタン](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

新しいデバイスを作成するには、**[New Device]\(新しいデバイス\)** ボタン (画面の右上にあります) をクリックします。
 
[![新しいデバイスを作成するための [New]\(新規\) ボタン](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**[New]\(新規\)** をクリックすると、**[New Device]\(新しいデバイス\)** 画面が表示されます。

[![Device Manager の [New Device]\(新しいデバイス\) 画面](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

**[New Device]\(新しいデバイス\)** 画面で新しいデバイスを構成するには、次の手順のようにします。

1. **[Device]\(デバイス\)** プルダウン メニューをクリックして、エミュレートする物理デバイスを選びます。

    [![[Device]\(デバイス\) プルダウン メニュー](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. **[System image]\(システム イメージ\)** プルダウン メニューをクリックして、この仮想デバイスで使うシステム イメージを選びます。 このメニューの **[Installed]\(インストール済み\)** には、インストールされているシステム イメージが一覧表示されます。 **[Download]\(ダウンロード\)** セクションには、お使いの開発用コンピューターでは現在使用できませんが、自動的にインストールできるシステム イメージが一覧表示されます。

    [![[System image]\(システム イメージ\) プルダウン メニュー](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. デバイスの新しい名前を指定します。 次の例では、新しいデバイスに「**Nexus 5 API 25**」という名前を指定しています。

    [![新しいデバイスの名前の指定](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. 変更する必要のあるプロパティを編集します。 プロパティの変更については、後の「[プロファイルのプロパティ](#properties)」をご覧ください。

5. 明示的に設定する必要がある他のプロパティを追加します。 **[New Device]\(新しいデバイス\)** 画面には最もよく変更されるプロパティのみが表示されていますが、**[Add Property]\(プロパティの追加\)** プルダウン メニュー (左下隅) をクリックしてプロパティを追加できます。 次の例では、`hw.lcd.backlight` プロパティを追加しています。

    [![[Add Property]\(プロパティの追加\) プルダウン メニュー](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. 新しいデバイスを作成するには、**[Create]\(作成\)** ボタン (右下隅) をクリックします。

    ![[Create]\(作成\) ボタン](xamarin-device-manager-images/win/14-create-button.png)

7. **[ライセンスの同意]** 画面が表示される場合があります。 ライセンス条項に同意する場合は、**[Accept]\(同意する\)** をクリックします。

    ![[ライセンスの同意] 画面](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Android Device Manager により、インストールされている仮想デバイスのリストに新しいデバイスが追加されます。デバイスが作成されている間、**[Creating]\(作成中\)** という進行状況のインジケーターが表示されます。

    [![作成進行状況インジケーター](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. 作成プロセスが完了すると、新しいデバイスがインストール済み仮想デバイスのリストに **[Start]\(開始\)** ボタンと共に表示され、起動できる状態になります。

   [![起動できる状態の新しく作成されたデバイス](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**[New Device]\(新しいデバイス\)** をクリックすると、**[New Device]\(新しいデバイス\)** 画面が表示されます。

[![Device Manager の [New Device]\(新しいデバイス\) 画面](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

**[New Device]\(新しいデバイス\)** 画面で新しいデバイスを構成するには、次の手順のようにします。

1. **[Device]\(デバイス\)** プルダウン メニューをクリックして、エミュレートする物理デバイスを選びます。

    [![[Device]\(デバイス\) プルダウン メニュー](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. **[System image]\(システム イメージ\)** プルダウン メニューをクリックして、この仮想デバイスで使うシステム イメージを選びます。 このメニューの **[Installed]\(インストール済み\)** には、インストールされているシステム イメージが一覧表示されます。 **[Download]\(ダウンロード\)** セクションには (表示される場合)、お使いの開発用コンピューターでは現在使用できませんが、自動的にインストールできるシステム イメージが一覧表示されます。

    [![[System image]\(システム イメージ\) プルダウン メニュー](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. デバイスの新しい名前を指定します。 次の例では、新しいデバイスに「**Nexus 5X API 25**」という名前を指定しています。

    [![新しいデバイスの名前の指定](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. 変更する必要のあるプロパティを編集します。 プロパティの変更については、後の「[プロファイルのプロパティ](#properties)」をご覧ください。

5. 明示的に設定する必要がある他のプロパティを追加します。 **[New Device]\(新しいデバイス\)** 画面には最もよく変更されるプロパティのみが表示されていますが、**[Add Property]\(プロパティの追加\)** プルダウン メニュー (左下隅) をクリックしてプロパティを追加できます。

    [![[Add Property]\(プロパティの追加\) プルダウン メニュー](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. **[Custom]\(カスタム\)** をクリックして、デバイスの新しいプロパティを定義することもできます。

    ![[Create]\(作成\) ボタン](xamarin-device-manager-images/mac/14-custom-button.png)

7. 新しいデバイスを作成するには、**[Create]\(作成\)** ボタン (右下隅) をクリックします。

    ![[Create]\(作成\) ボタン](xamarin-device-manager-images/mac/15-create-button.png)

8. **[ライセンスの同意]** 画面が表示される場合があります。 ライセンス条項に同意する場合は、**[Accept]\(同意する\)** をクリックします。

9. Android Device Manager により、インストールされている仮想デバイスのリストに新しいデバイスが追加されます。デバイスが作成されている間、**[Creating]\(作成中\)** という進行状況のインジケーターが表示されます。

    [![作成進行状況インジケーター](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. 作成プロセスが完了すると、新しいデバイスがデバイスのリストに **[Play]\(再生\)** ボタンと共に表示され、起動できる状態になります。

   [![起動できる状態の新しく作成されたデバイス](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />
 
### <a name="edit-device"></a>デバイスの編集

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

既存の仮想デバイスを編集するには、デバイスを選んで、**[Edit]\(編集\)** ボタン (画面の右上隅にあります) をクリックします。

[![新しいデバイスを変更するための [Edit]\(編集\) ボタン](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

既存の仮想デバイスを編集するには、**[Additional Options]\(追加オプション\)** プルダウン メニュー (歯車アイコン) を選んで、**[Edit]\(編集\)** を選びます。
 
[![新しいデバイスを変更するための [Edit]\(編集\) メニュー選択](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

**[Edit]\(編集\)** をクリックすると、選んだ仮想デバイスのデバイス エディターが起動します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイス エディター画面](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![デバイス エディター画面](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

**デバイス エディター**画面では、最初の列に仮想デバイスのプロパティが一覧表示され、2 番目の列に各プロパティの対応する値が表示されます。 プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

たとえば、次のスクリーンショットでは、`hw.lcd.density` プロパティを **420** から **240** に変更しています。

[![デバイスの編集の例](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

たとえば、次のスクリーンショットでは、`hw.lcd.density` プロパティを **320** から **240** に変更し、`hw.ramSize` プロパティを **768** に変更しています。
 
[![デバイスの編集の例](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

必要な構成の変更を行った後、**[Save]\(保存\)** ボタンをクリックします。
仮想デバイスのプロパティの変更について詳しくは、後の「[プロファイルのプロパティ](#properties)」をご覧ください。


 
### <a name="additional-options"></a>その他のオプション

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

右上隅の &hellip; メニューから、デバイスの操作に関するその他のオプションを指定できます。

[![その他のオプション メニューの場所](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

デバイスの操作に関するその他のオプションを、**[Play]\(再生\)** ボタンの左側にあるプルダウン メニューから指定できます。

[![その他のオプション メニューの場所](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

その他のオプションのメニューには、次の項目が含まれます。

-   **[Duplicate and Edit]\(複製して編集\)** &ndash; 現在選ばれているデバイスを複製し、異なる一意名を付けて **[New Device]\(新しいデバイス\)** 画面で開きます。 たとえば、**VisualStudio_android-23_x86_phone** を選んで **[Duplicate and Edit]\(複製して編集\)** をクリックすると、名前にカウンターが追加されます。

    [![[Duplicate and Edit]\(複製して編集\) 画面](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **[Reveal in Explorer]\(エクスプローラーで表示\)** &ndash; 仮想デバイスのファイルが含まれるフォルダーを、Windows のエクスプローラー ウィンドウで開きます。 たとえば、**Nexus 5X API 25** を選んで **[Reveal in Explorer]\(エクスプローラーで表示\)** をクリックすると、次のようなウィンドウが開きます。

    [![[Reveal in Explorer]\(エクスプローラーで表示\) をクリックした結果](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **[Factory Reset]\(出荷時の設定にリセット\)** &ndash; 選んだデバイスを既定の設定にリセットし、デバイスの実行中にユーザーが行ったデバイスの内部状態に対する変更を消去します。 仮想デバイスを作成または編集するときに行った変更は消去されません。 このリセットは元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Wipe user data]\(ユーザー データをワイプ\)** をクリックして、リセットを確定します。

-   **[Delete]\(削除\)** &ndash; 選んだ仮想デバイスを完全に削除します。
    デバイスの削除は元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Delete]\(削除\)** をクリックして、デバイスを削除することを確認します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

その他のオプションのメニューには、次の項目が含まれます。

-   **[Edit]\(編集\)** &ndash; 前の「[デバイスの編集](#device-edit)」で説明したように、現在選ばれているデバイスがデバイス エディターで開かれます。

-   **[Duplicate and Edit]\(複製して編集\)** &ndash; 現在選ばれているデバイスを複製し、異なる一意名を付けて **[New Device]\(新しいデバイス\)** 画面で開きます。
    たとえば、**Nexus 5X API 25** を選んで **[Duplicate and Edit]\(複製して編集\)** をクリックすると、名前にカウンターが追加されます。

    [![[Duplicate and Edit]\(複製して編集\) 画面](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **[Reveal in Finder]\(ファインダーで表示\)** &ndash; 仮想デバイスのファイルが含まれるフォルダーを、macOS のファインダー ウィンドウで開きます。 たとえば、**Nexus 5X API 25** を選んで **[Reveal in Finder]\(ファインダーで表示\)** をクリックすると、次のようなウィンドウが開きます。

    [![[Reveal in Explorer]\(エクスプローラーで表示\) をクリックした結果](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **[Factory Reset]\(出荷時の設定にリセット\)** &ndash; 選んだデバイスを既定の設定にリセットし、デバイスの実行中にユーザーが行ったデバイスの内部状態に対する変更を消去します。 仮想デバイスを作成または編集するときに行った変更は消去されません。 このリセットは元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Wipe user data]\(ユーザー データをワイプ\)** をクリックして、リセットを確定します。

-   **[Delete]\(削除\)** &ndash; 選んだ仮想デバイスを完全に削除します。
    デバイスの削除は元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Delete]\(削除\)** をクリックして、デバイスを削除することを確認します。

-----

<a name="properties" />
 
## <a name="profile-properties"></a>プロファイルのプロパティ

**[New Device]\(新しいデバイス\)** および **[Device Edit]\(デバイスの編集\)** 画面では、最初の列に仮想デバイスのプロパティが一覧表示され、2 番目の列に各プロパティの対応する値が表示されます。 プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。 "*ハードウェア プロファイルのプロパティ*" および "*AVD のプロパティ*" は変更できます。
ハードウェア プロファイルのプロパティ (`hw.ramSize` や `hw.accelerometer` など) では、エミュレートされたデバイスの物理的な特性が記述されています。 このような特性としては、画面のサイズ、使用可能な RAM の量、加速度計が存在するかどうか、などがあります。 AVD のプロパティでは、実行時の AVD の動作が指定されています。 たとえば、AVD のプロパティを構成して、AVD がレンダリングに開発用コンピューターのグラフィックス カードを使う方法を指定することができます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

次のようにしてプロパティを変更できます。

-   ブール型のプロパティを変更するには、プロパティの右側にあるチェック マークをクリックします。

    ![ブール型プロパティの変更](xamarin-device-manager-images/win/25-boolean-value.png)

-   *enum* (列挙) 型のプロパティを変更するには、プロパティの右側にある下向き矢印をクリックして、新しい値を選びます。

    ![列挙型プロパティの変更](xamarin-device-manager-images/win/27-enum-value.png)

-   文字列型または整数型のプロパティを変更するには、値の列で現在の文字列または整数の設定をダブルクリックし、新しい値を入力します。

    ![整数型プロパティの変更](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

次のようにしてプロパティを変更できます。

-   ブール型のプロパティを変更するには、プロパティの右側にあるチェック マークをクリックします。

    ![ブール型プロパティの変更](xamarin-device-manager-images/mac/25-boolean-value.png)

-   *enum* (列挙) 型のプロパティを変更するには、プロパティの右側にあるプルダウン メニューをクリックして、新しい値を選びます。

    ![列挙型プロパティの変更](xamarin-device-manager-images/mac/27-enum-value.png)

-   文字列型または整数型のプロパティを変更するには、値の列で現在の文字列または整数の設定をダブルクリックし、新しい値を入力します。

    ![整数型プロパティの変更](xamarin-device-manager-images/mac/26-integer-value.png)


-----

次の表では、**[New Device]\(新しいデバイス\)** 画面および**デバイス エディター**画面に一覧表示されるプロパティについて詳しく説明します。

[!include[](~/android/includes/emulator-properties.md)]

これらのプロパティについて詳しくは、「[Hardware Profile Properties](https://developer.android.com/studio/run/managing-avds.html#hpproperties)」(ハードウェア プロファイルのプロパティ) をご覧ください。


<a name="troubleshooting" />

## <a name="troubleshooting"></a>トラブルシューティング

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ここでは、Xamarin Android Device Manager でのよくある問題とその回避策を説明します。

### <a name="android-sdk-in-non-standard-location"></a>標準以外の場所にある Android SDK

通常、Android SDK は次の場所にインストールされます。

**C:\\Program Files (x86)\\Android\\android-sdk**

SDK がこの場所にインストールされていない場合、起動時に次のエラーが発生する可能性があります。

![Android SDK インスタンスのエラー](xamarin-device-manager-images/win/32-sdk-error.png)

この問題を回避するには、次のようにします。

1. Windows デスクトップから、**C:\\Users\\<*ユーザー名*>\\AppData\\Roaming\\XamarinDeviceManager** に移動します。

    ![Xamarin Device Manager のログ ファイルの場所](xamarin-device-manager-images/win/33-log-files.png)

2. いずれかのログ ファイルをダブルクリックして開き、**構成ファイルのパス**を調べます。 例:

    [![ログ ファイルでの構成ファイルのパス](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. この場所に移動し、**user.config** をダブルクリックして開きます。 

4. **user.config** で **&lt;UserSettings&gt;** 要素を探し、それに **AndroidSdkPath** 属性を追加します。 Android SDK がインストールされているコンピューター上のパスをこの属性に設定し、ファイルを保存します。 たとえば、Android SDK が **C:\\Programs\\Android\\SDK** にインストールされている場合、**&lt;UserSettings&gt;** は次のようになります。
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

**user.config** をこのように変更すると、Xamarin Android Device Manager を起動できるようになります。

### <a name="generating-a-bug-report"></a>バグ報告の生成

上記のトラブルシューティングのヒントで解決できない問題が Xamarin Android Device Manager で見つかった場合は、タイトル バーを右クリックし、**[Generate Bug Report]\(バグ報告の生成\)** を選択して、バグ報告を提出してください。

![バグ報告の提出に関するメニュー項目の場所](xamarin-device-manager-images/win/35-bug-report.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

現在、Visual Studio for Mac の Xamarin Android Device Manager について、既知の問題および回避策はありません。 

### <a name="generating-a-bug-report"></a>バグ報告の生成

問題が見つかった場合は、**[ヘルプ] > [Generate Bug Report]\(バグ レポートの生成\)** をクリックしてバグ報告を提出してください。

![バグ報告の提出に関するメニュー項目の場所](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>まとめ

このガイドでは、Visual Studio for Mac および Visual Studio 用 Xamarin で利用できる Xamarin Android Device Manager について紹介しました。 Android エミュレーターの開始と停止、実行する Android 仮想デバイス (AVD) の選択、仮想デバイスの編集方法などの、重要な機能について説明しました。 また、ハードウェア プロファイルのプロパティを編集してさらにカスタマイズする方法についても説明しました。


## <a name="related-links"></a>関連リンク

- [Android SDK ツールの変更](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android SDK エミュレーターでのデバッグ](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
