---
title: Android Device Manager による仮想デバイスの管理
description: この記事では、Android Device Manager を使って、Android の物理デバイスをエミュレートする Android 仮想デバイス (AVD) を作成し、構成する方法を説明します。 仮想デバイスを使うと、物理デバイスがなくてもアプリを実行してテストすることができます。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: fd1361f00bf10089f7a9dead5a5adaa1e7c29727
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571585"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Android Device Manager による仮想デバイスの管理

_この記事では、Android Device Manager を使って、Android の物理デバイスをエミュレートする Android 仮想デバイス (AVD) を作成し、構成する方法を説明します。仮想デバイスを使うと、物理デバイスがなくてもアプリを実行してテストすることができます。_

ハードウェアの高速化が有効になっていることを確認した後は (「[エミュレーター パフォーマンスのためのハードウェア高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)」を参照)、_Android Device Manager_ (_Xamarin Android Device Manager_ ともいう) を使って、アプリのテストとデバッグで使用できる仮想デバイスを作成します。

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Windows 上の Android Device Manager

この記事では、Android Device Manager を使って、Android 仮想デバイスを作成、複製、カスタマイズ、起動する方法について説明します。

[![Android Device Manager の [Devices]\(デバイス\) タブのスクリーンショット](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

[Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) で実行する _Android 仮想デバイス_ (AVD) を作成および構成するには、Android Device Manager を使います。
各 AVD は、物理的な Android デバイスをシミュレートするエミュレーター構成です。 これにより、異なる物理 Android デバイスをシミュレートするさまざまな構成でアプリを実行してテストすることができます。

## <a name="requirements"></a>必要条件

Android Device Manager を使用するには、次の項目が必要です。

- Visual Studio 2019 Community、Professional、または Enterprise。

- または、Visual Studio 2017 バージョン 15.8 以降が必要です。 Visual Studio Community、Professional、Enterprise Edition がサポートされています。

- Visual Studio Tools for Xamarin バージョン 4.9 以降。

- Android SDK をインストールする必要があります (「[Xamarin.Android 向け Android SDK を設定する](~/android/get-started/installation/android-sdk.md)」を参照)。
  Android SDK がまだインストールされていない場合は、その既定の場所にインストールします。**C:\\Program Files (x86)\\Android\\android-sdk**。

- ([Android SDK Manager](~/android/get-started/installation/android-sdk.md) を使用して) 次のパッケージをインストールする必要があります。 
  - **Android SDK Tools バージョン 26.1.1** 以降
  - **Android SDK Platform-Tools 27.0.1** 以降
  - **Android SDK Build Tools 27.0.3** 以降 
  - **Android Emulator 27.2.7** 以降。 

  これらのパッケージの状態は、次のスクリーンショットのように **[インストール済み]** と表示される必要があります。

  [![Android SDK Tools のインストール](device-manager-images/win/02-sdk-tools-sml.png)](device-manager-images/win/02-sdk-tools.png#lightbox)

## <a name="launching-the-device-manager"></a>Device Manager の起動

**[ツール]、[Android]、[Android Device Manager]** の順にクリックし、 **[ツール]** メニューから Android Device Manager を起動します。

[![[ツール] メニューから Device Manager を起動する](device-manager-images/win/03-tools-menu-sml.png)](device-manager-images/win/03-tools-menu.png#lightbox)

起動時に次のエラー ダイアログが表示される場合は、「[トラブルシューティング](#troubleshooting)」セクションの回避策の説明をご覧ください。

![Android SDK インスタンスのエラー ダイアログ](device-manager-images/win/04-sdk-error.png)

## <a name="main-screen"></a>メイン画面

Android Device Manager を初めて起動すると、現在構成されているすべての仮想デバイスが画面に表示されます。 仮想デバイスごとに**名前**、**OS** (Android バージョン)、**プロセッサ**、**メモリ** サイズ、および画面の**解像度**が表示されます。

[![インストールされているデバイスの一覧とそれらのパラメーター](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

一覧のデバイスを選択すると、右側に **[Start]\(開始\)** ボタンが表示されます。 **[Start]\(開始\)** ボタンをクリックして、この仮想デバイスのエミュレーターを起動できます。

[![デバイス イメージの [Start]\(開始\) ボタン](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

選んだ仮想デバイスでエミュレーターが開始した後、 **[Start]\(開始\)** ボタンは **[Stop]\(停止\)** ボタンに変わり、このボタンを使ってエミュレーターを停止できます。

[![実行中のデバイスの [Stop]\(停止\) ボタン](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>新しいデバイス

新しいデバイスを作成するには、 **[New]\(新規\)** ボタン (画面の右上にあります) をクリックします。

[![新しいデバイスを作成するための [New]\(新規\) ボタン](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

**[New]\(新規\)** をクリックすると、 **[New Device]\(新しいデバイス\)** 画面が表示されます。

[![Device Manager の [New Device]\(新しいデバイス\) 画面](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

**[New Device]\(新しいデバイス)** 画面で新しいデバイスを構成するには、次の手順を実行します。

1. デバイスの新しい名前を指定します。 次の例では、新しいデバイスに「**Pixel_API_27**」という名前を指定しています。

   [![新しいデバイスの名前の指定](device-manager-images/win/10-device-name-sml.png)](device-manager-images/win/10-device-name.png#lightbox)

2. **[Base Device]\(ベース デバイス\)** プルダウン メニューをクリックして、エミュレートする物理デバイスを選びます。

   [![エミュレートする物理デバイスの選択](device-manager-images/win/11-device-menu-sml.png)](device-manager-images/win/11-device-menu.png#lightbox)

3. **[Processor]\(プロセッサ\)** プルダウン メニューをクリックして、この仮想デバイスのプロセッサの種類を選択します。 **[x86]** を選択すると、エミュレーターで[ハードウェア高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)を使用できるので最高のパフォーマンスを達成できます。
   **[x86_64]** オプションでもハードウェア高速化を利用しますが、 **[x86]** よりやや遅くなります ( **[x86_64]** は通常 64 ビット アプリのテストに使用されます)。

   [![プロセッサの種類の選択](device-manager-images/win/12-processor-type-menu-sml.png)](device-manager-images/win/12-processor-type-menu.png#lightbox)

4. **[OS]** プルダウン メニューをクリックして Android バージョン (API レベル) を選択します。 たとえば、API レベル 27 の仮想デバイスを作成するには、 **[Oreo 8.1 - API 27]** を選択します。

   [![Android バージョンの選択](device-manager-images/win/13-android-version-w158-sml.png)](device-manager-images/win/13-android-version-w158.png#lightbox)

   まだインストールされていない Android API レベルを選択すると、Device Manager の画面の下部に "**A new device will be downloaded**" (新しいデバイスがダウンロードされます) というメッセージが表示されます。必要なファイルをダウンロードしてインストールします。これで新しい仮想デバイスが作成されます。

   ![新しいデバイス イメージがダウンロードされます。](device-manager-images/win/14-automatic-download-w158.png)

5. 仮想デバイスに Google Play 開発者サービス API を含める場合は、 **[Google API]** オプションを有効にします。 Google Play ストア アプリを含めるには、 **[Google Play Store]\(Google Play ストア\)** オプションを有効にします。

   [![Google Play 開発者サービスと Google Play ストアの選択](device-manager-images/win/15-google-play-services-sml.png)](device-manager-images/win/15-google-play-services.png#lightbox)

   Google Play ストアのイメージは、Pixel、Pixel 2、Nexus 5、Nexus 5 X などの一部の基本デバイスの種類でのみ使用できる点に注意してください。

6. 変更する必要のあるプロパティを編集します。 プロパティを変更する場合は、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

7. 明示的に設定する必要がある他のプロパティを追加します。 **[New Device]\(新しいデバイス\)** 画面には最もよく変更されるプロパティのみが表示されていますが、 **[Add Property]\(プロパティの追加\)** プルダウン メニュー (下部) をクリックしてプロパティを追加できます。

   [![[Add Property]\(プロパティの追加\) プルダウン メニュー](device-manager-images/win/16-add-property-menu-sml.png)](device-manager-images/win/16-add-property-menu.png#lightbox)

    また、プロパティ一覧の上部にある **[Custom...]\(カスタム\)** を選択してカスタム プロパティを定義することもできます。

8. 新しいデバイスを作成するには、 **[Create]\(作成\)** ボタン (右下隅) をクリックします。

   [![[Create]\(作成\) ボタン](device-manager-images/win/17-create-button-sml.png)](device-manager-images/win/17-create-button.png#lightbox)

9. **[License Acceptance]** \(ライセンスの同意\) 画面が表示される場合があります。 ライセンス条項に同意する場合は、 **[Accept]\(同意する\)** をクリックします。

   [![[License Acceptance]\(ライセンスの同意\) 画面](device-manager-images/win/18-license-acceptance-sml.png)](device-manager-images/win/18-license-acceptance.png#lightbox)

10. Android Device Manager により、インストールされている仮想デバイスのリストに新しいデバイスが追加されます。デバイスが作成されている間は、 **[Creating]\(作成中\)** という進行状況のインジケーターが表示されます。

    [![作成進行状況インジケーター](device-manager-images/win/19-creating-the-device-sml.png)](device-manager-images/win/19-creating-the-device.png#lightbox)

11. 作成プロセスが完了すると、新しいデバイスがインストール済み仮想デバイスのリストに **[Start]\(開始\)** ボタンと共に表示され、起動できる状態になります。

    [![起動できる状態の新しく作成されたデバイス](device-manager-images/win/20-created-device-sml.png)](device-manager-images/win/20-created-device.png#lightbox)

### <a name="edit-device"></a>デバイスの編集

既存の仮想デバイスを編集するには、デバイスを選んで、 **[Edit]\(編集\)** ボタン (画面の右上隅にあります) をクリックします。

[![デバイスを変更するための [Edit]\(編集\) ボタン](device-manager-images/win/21-edit-button-sml.png)](device-manager-images/win/21-edit-button.png#lightbox)

**[Edit]\(編集\)** をクリックすると、選んだ仮想デバイスのデバイス エディターが起動します。

[![デバイス エディター画面](device-manager-images/win/22-device-editor-sml.png)](device-manager-images/win/22-device-editor.png#lightbox)

**[Device Editor]\(デバイス エディター\)** 画面では、 **[Property]\(プロパティ\)** 列に仮想デバイスのプロパティが一覧表示され、 **[Value]\(値\)** 列に各プロパティの対応する値が表示されます。 プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。

プロパティを変更するには、 **[Value]\(値\)** 列の値を編集します。
たとえば、次のスクリーンショットでは、`hw.lcd.density` プロパティを **480** から **240** に変更しています。

[![デバイスの編集の例](device-manager-images/win/23-device-editing-sml.png)](device-manager-images/win/23-device-editing.png#lightbox)

必要な構成の変更を行った後、 **[Save]\(保存\)** ボタンをクリックします。
仮想デバイス プロパティの変更の詳細については、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

### <a name="additional-options"></a>その他のオプション

右上隅の **[Additional Options]\(その他のオプション\)** (&hellip;) プルダウン メニューから、デバイスの操作に関するその他のオプションを指定できます。

[![その他のオプション メニューの場所](device-manager-images/win/24-overflow-menu-sml.png)](device-manager-images/win/24-overflow-menu.png#lightbox)

その他のオプションのメニューには、次の項目が含まれます。

- **[Duplicate and Edit]\(複製して編集\)** &ndash; 現在選択しているデバイスを複製し、異なる一意の名前を付けて **[New Device]\(新しいデバイス\)** 画面で開きます。 たとえば、 **[Pixel_API_27]** を選択し、 **[Duplicate and Edit]\(複製して編集\)** をクリックすると、名前にカウンターが追加されます。

  [![[Duplicate and Edit]\(複製して編集\) 画面](device-manager-images/win/25-dupe-and-edit-sml.png)](device-manager-images/win/25-dupe-and-edit.png#lightbox)

- **[Reveal in Explorer]\(エクスプローラーで表示\)** &ndash; 仮想デバイスのファイルが含まれるフォルダーを、Windows のエクスプローラー ウィンドウで開きます。 たとえば、 **[Pixel_API_27]** を選択し、 **[Reveal in Explorer]\(エクスプローラーで表示\)** をクリックすると、次の例のようなウィンドウが開きます。

  [![[Reveal in Explorer]\(エクスプローラーで表示\) をクリックした結果](device-manager-images/win/26-reveal-in-explorer-sml.png)](device-manager-images/win/26-reveal-in-explorer.png#lightbox)

- **[Factory Reset]\(出荷時の設定にリセット\)** &ndash; 選択したデバイスをその既定の設定にリセットし、デバイスの実行中にユーザーが行ったデバイスの内部状態に対する変更を消去します (現在の[クイック ブート](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)のスナップショットが存在する場合は、それも消去されます)。 仮想デバイスを作成または編集するときに行った変更は消去されません。 このリセットは元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Factory Reset]\(出荷時の設定にリセット\)** をクリックしてリセットを確定します。

  ![[Factory Reset]\(出荷時の設定にリセット\) ダイアログ](device-manager-images/win/27-factory-reset.png)

- **[Delete]\(削除\)** &ndash; 選んだ仮想デバイスを完全に削除します。 デバイスの削除は元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Delete]\(削除\)** をクリックして、デバイスを削除することを確認します。

  ![[Delete device]\(デバイスの削除\) ダイアログ](device-manager-images/win/28-delete-device-w158.png)

::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>macOS 上の Android Device Manager

この記事では、Android Device Manager を使って、Android 仮想デバイスを作成、複製、カスタマイズ、起動する方法について説明します。

[![Android Device Manager の [Devices]\(デバイス\) タブのスクリーンショット](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> このガイドは、Visual Studio for Mac のみに該当します。
Xamarin Studio は Android Device Manager と互換性がありません。

[Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) で実行する *Android 仮想デバイス* (AVD) を作成および構成するには、Android Device Manager を使います。
各 AVD は、物理的な Android デバイスをシミュレートするエミュレーター構成です。 これにより、異なる物理 Android デバイスをシミュレートするさまざまな構成でアプリを実行してテストすることができます。

## <a name="requirements"></a>必要条件

Android Device Manager を使用するには、次の項目が必要です。

- Visual Studio for Mac 7.6 以降。

- Android SDK をインストールする必要があります (「[Xamarin.Android 向け Android SDK を設定する](~/android/get-started/installation/android-sdk.md)」を参照)。

- ([Android SDK Manager](~/android/get-started/installation/android-sdk.md) を使用して) 次のパッケージをインストールする必要があります。 
  - **SDK Tools バージョン 26.1.1** 以降
  - **Android SDK Platform-Tools 28.0.1** 以降 
  - **Android SDK Build-Tools 26.0.3** 以降

  これらのパッケージの状態は、次のスクリーンショットのように **[インストール済み]** と表示される必要があります。

  [![Android SDK Tools のインストール](device-manager-images/mac/02-sdk-tools-sml.png)](device-manager-images/mac/02-sdk-tools.png#lightbox)

## <a name="launching-the-device-manager"></a>Device Manager の起動

**[ツール]、[デバイス マネージャー]** の順にクリックし、Android Device Manager を起動します。

[![[ツール] メニューから Device Manager を起動する](device-manager-images/mac/03-tools-menu-sml.png)](device-manager-images/mac/03-tools-menu.png#lightbox)

起動時に次のエラー ダイアログが表示される場合は、「[トラブルシューティング](#troubleshooting)」セクションの回避策の説明をご覧ください。

![Android SDK インスタンスのエラー ダイアログ](device-manager-images/mac/04-sdk-instance-error.png)

## <a name="main-screen"></a>メイン画面

Android Device Manager を初めて起動すると、現在構成されているすべての仮想デバイスが画面に表示されます。 仮想デバイスごとに**名前**、**OS** (Android バージョン)、**プロセッサ**、**メモリ** サイズ、および画面の**解像度**が表示されます。

[![インストールされているデバイスの一覧とそれらのパラメーター](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

一覧のデバイスを選択すると、右側に **[Play]\(再生\)** ボタンが表示されます。 **[Play]\(再生\)** ボタンをクリックして、この仮想デバイスのエミュレーターを起動できます。

[![デバイス イメージの [Play]\(再生\) ボタン](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

選んだ仮想デバイスでエミュレーターが開始した後、 **[Play]\(再生\)** ボタンは **[Stop]\(停止\)** ボタンに変わり、このボタンを使ってエミュレーターを停止できます。

[![実行中のデバイスの [Stop]\(停止\) ボタン](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

エミュレーターを停止するときに、次のクイック ブートのために現在の状態を保存するかどうかを確認するプロンプトが表示されることがあります。

![クイック ブートのために現在の状態を保存するダイアログ](device-manager-images/mac/08-save-for-quick-boot-m76.png)

現在の状態を保存すると、この仮想デバイスが再び起動されたときにエミュレーターの起動が速くなります。 クイック ブートの詳細については、「[クイック ブート](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)」を参照してください。

### <a name="new-device"></a>新しいデバイス

新しいデバイスを作成するには、 **[New Device]\(新しいデバイス\)** ボタン (画面の左上にあります) をクリックします。

[![新しいデバイスを作成するための [New]\(新規\) ボタン](device-manager-images/mac/09-new-button-sml.png)](device-manager-images/mac/09-new-button.png#lightbox)

**[New Device]\(新しいデバイス\)** をクリックすると、 **[New Device]\(新しいデバイス\)** 画面が表示されます。

[![Device Manager の [New Device]\(新しいデバイス\) 画面](device-manager-images/mac/10-new-device-editor-sml.png)](device-manager-images/mac/10-new-device-editor.png#lightbox)

**[New Device]\(新しいデバイス\)** 画面で新しいデバイスを構成するには、次の手順のようにします。

1. デバイスの新しい名前を指定します。 次の例では、新しいデバイスに「**Pixel_API_27**」という名前を指定しています。

   [![新しいデバイスの名前の指定](device-manager-images/mac/11-device-name-m76-sml.png)](device-manager-images/mac/11-device-name-m76.png#lightbox)

2. **[Base Device]\(ベース デバイス\)** プルダウン メニューをクリックして、エミュレートする物理デバイスを選びます。

   [![エミュレートする物理デバイスの選択](device-manager-images/mac/12-device-menu-m76-sml.png)](device-manager-images/mac/12-device-menu-m76.png#lightbox)

3. **[Processor]\(プロセッサ\)** プルダウン メニューをクリックして、この仮想デバイスのプロセッサの種類を選択します。 **[x86]** を選択すると、エミュレーターで[ハードウェア高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)を使用できるので最高のパフォーマンスを達成できます。
   **[x86_64]** オプションでもハードウェア高速化を利用しますが、 **[x86]** よりやや遅くなります ( **[x86_64]** は通常 64 ビット アプリのテストに使用されます)。

   [![プロセッサの種類の選択](device-manager-images/mac/13-processor-type-menu-m76-sml.png)](device-manager-images/mac/13-processor-type-menu-m76.png#lightbox)

4. **[OS]** プルダウン メニューをクリックして Android バージョン (API レベル) を選択します。 たとえば、API レベル 27 の仮想デバイスを作成するには、 **[Oreo 8.1 - API 27]** を選択します。

   [![Android バージョンの選択](device-manager-images/mac/14-android-screenshot-m76-sml.png)](device-manager-images/mac/14-android-screenshot-m76.png#lightbox)

   まだインストールされていない Android API レベルを選択すると、Device Manager の画面の下部に "**A new device will be downloaded**" (新しいデバイスがダウンロードされます) というメッセージが表示されます。必要なファイルをダウンロードしてインストールします。これで新しい仮想デバイスが作成されます。

   ![新しいデバイス イメージがダウンロードされます。](device-manager-images/mac/15-automatic-download-m76.png)

5. 仮想デバイスに Google Play 開発者サービス API を含める場合は、 **[Google API]** オプションを有効にします。 Google Play ストア アプリを含めるには、 **[Google Play Store]\(Google Play ストア\)** オプションを有効にします。

   [![Google Play 開発者サービスと Google Play ストアの選択](device-manager-images/mac/16-google-play-services-m76-sml.png)](device-manager-images/mac/16-google-play-services-m76.png#lightbox)

   Google Play ストアのイメージは、Pixel、Pixel 2、Nexus 5、Nexus 5 X などの一部の基本デバイスの種類でのみ使用できる点に注意してください。

6. 変更する必要のあるプロパティを編集します。 プロパティを変更する場合は、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

7. 明示的に設定する必要がある他のプロパティを追加します。 **[New Device]\(新しいデバイス\)** 画面には最もよく変更されるプロパティのみが表示されていますが、 **[Add Property]\(プロパティの追加\)** プルダウン メニュー (下部) をクリックしてプロパティを追加できます。

   [![[Add Property]\(プロパティの追加\) プルダウン メニュー](device-manager-images/mac/17-add-property-menu-m76-sml.png)](device-manager-images/mac/17-add-property-menu-m76.png#lightbox)

   また、このプロパティ一覧の上部にある **[Custom...]\(カスタム\)** をクリックしてカスタム プロパティを定義することもできます。

8. 新しいデバイスを作成するには、 **[Create]\(作成\)** ボタン (右下隅) をクリックします。

   ![[Create]\(作成\) ボタン](device-manager-images/mac/18-create-button-m76.png)

9. Android Device Manager により、インストールされている仮想デバイスのリストに新しいデバイスが追加されます。デバイスが作成されている間は、 **[Creating]\(作成中\)** という進行状況のインジケーターが表示されます。

   [![作成進行状況インジケーター](device-manager-images/mac/19-creating-the-device-m76-sml.png)](device-manager-images/mac/19-creating-the-device-m76.png#lightbox)

10. 作成プロセスが完了すると、新しいデバイスがインストール済み仮想デバイスのリストに **[Start]\(開始\)** ボタンと共に表示され、起動できる状態になります。

    [![起動できる状態の新しく作成されたデバイス](device-manager-images/mac/20-created-device-m76-sml.png)](device-manager-images/mac/20-created-device-m76.png#lightbox)

### <a name="edit-device"></a>デバイスの編集

既存の仮想デバイスを編集するには、 **[Additional Options]\(追加オプション\)** プルダウン メニュー (歯車アイコン) を選んで、 **[Edit]\(編集\)** を選びます。

[![新しいデバイスを変更するための [Edit]\(編集\) メニュー選択](device-manager-images/mac/21-edit-button-m76-sml.png)](device-manager-images/mac/21-edit-button-m76.png#lightbox)

**[Edit]\(編集\)** をクリックすると、選んだ仮想デバイスのデバイス エディターが起動します。

[![デバイス エディター画面](device-manager-images/mac/22-device-editor-sml.png)](device-manager-images/mac/22-device-editor.png#lightbox)

**[Device Editor]\(デバイス エディター\)** 画面では、 **[Property]\(プロパティ\)** 列に仮想デバイスのプロパティが一覧表示され、 **[Value]\(値\)** 列に各プロパティの対応する値が表示されます。 プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。

プロパティを変更するには、 **[Value]\(値\)** 列の値を編集します。
たとえば、次のスクリーンショットでは、`hw.lcd.density` プロパティを **480** から **240** に変更しています。

[![デバイスの編集の例](device-manager-images/mac/23-device-editing-sml.png)](device-manager-images/mac/23-device-editing.png#lightbox)

必要な構成の変更を行った後、 **[Save]\(保存\)** ボタンをクリックします。
仮想デバイス プロパティの変更の詳細については、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

### <a name="additional-options"></a>その他のオプション

デバイスの操作に関するその他のオプションを、 **[Play]\(再生\)** ボタンの左側にあるプルダウン メニューから指定できます。

[![その他のオプション メニューの場所](device-manager-images/mac/24-overflow-menu-sml.png)](device-manager-images/mac/24-overflow-menu.png#lightbox)

その他のオプションのメニューには、次の項目が含まれます。

- **[Edit]\(編集\)** &ndash; 前述のとおり、現在選択しているデバイスがデバイス エディターで開かれます。

- **[Duplicate and Edit]\(複製して編集\)** &ndash; 現在選択しているデバイスを複製し、異なる一意の名前を付けて **[New Device]\(新しいデバイス\)** 画面で開きます。 たとえば、 **[Pixel 2 API 28]** を選択し、 **[Duplicate and Edit]\(複製して編集\)** をクリックすると、名前にカウンターが追加されます。

  [![[Duplicate and Edit]\(複製して編集\) 画面](device-manager-images/mac/25-dupe-and-edit-sml.png)](device-manager-images/mac/25-dupe-and-edit.png#lightbox)

- **[Reveal in Finder]\(ファインダーで表示\)** &ndash; 仮想デバイスのファイルが含まれるフォルダーを、macOS のファインダー ウィンドウで開きます。 たとえば、 **[Pixel 2 API 28]** を選択し、 **[Reveal in Explorer]\(エクスプローラーで表示\)** をクリックすると、次の例のようなウィンドウが開きます。

  [![[Reveal in Finder]\(Finder で表示\) をクリックした結果](device-manager-images/mac/26-reveal-in-finder-sml.png)](device-manager-images/mac/26-reveal-in-finder.png#lightbox)

- **[Factory Reset]\(出荷時の設定にリセット\)** &ndash; 選択したデバイスをその既定の設定にリセットし、デバイスの実行中にユーザーが行ったデバイスの内部状態に対する変更を消去します (現在の[クイック ブート](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)のスナップショットが存在する場合は、それも消去されます)。 仮想デバイスを作成または編集するときに行った変更は消去されません。 このリセットは元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Factory Reset]\(出荷時の設定にリセット\)** をクリックしてリセットを確定します。

  ![[Factory Reset]\(出荷時の設定にリセット\) ダイアログ](device-manager-images/mac/27-factory-reset-m76.png)

- **[Delete]\(削除\)** &ndash; 選んだ仮想デバイスを完全に削除します。 デバイスの削除は元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Delete]\(削除\)** をクリックして、デバイスを削除することを確認します。

  ![[Delete device]\(デバイスの削除\) ダイアログ](device-manager-images/mac/28-delete-device-m76.png)

-----

<a name="troubleshooting"></a>

## <a name="troubleshooting"></a>トラブルシューティング

次のセクションでは、Android Device Manager を使用して仮想デバイスを構成するときに発生する可能性のある問題を診断して回避する方法について説明します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="android-sdk-in-non-standard-location"></a>標準以外の場所にある Android SDK

通常、Android SDK は次の場所にインストールされます。

**C:\\Program Files (x86)\\Android\\android-sdk**

SDK がこの場所にインストールされていない場合、Android Device Manager の起動時に次のエラーが発生する可能性があります。

![Android SDK インスタンスのエラー](device-manager-images/win/29-sdk-error.png)

この問題を回避するには、次の手順を実行します。

1. Windows デスクトップから、**C:\\Users\\<*ユーザー名*>\\AppData\\Roaming\\XamarinDeviceManager** に移動します。

   ![Android Device Manager のログ ファイルの場所](device-manager-images/win/30-log-files.png)

2. いずれかのログ ファイルをダブルクリックして開き、**構成ファイルのパス**を調べます。 次に例を示します。

   [![ログ ファイルでの構成ファイルのパス](device-manager-images/win/31-config-file-path-sml.png)](device-manager-images/win/31-config-file-path.png#lightbox)

3. この場所に移動し、**user.config** をダブルクリックして開きます。

4. **user.config** で `<UserSettings>` 要素を探し、それに **AndroidSdkPath** 属性を追加します。 Android SDK がインストールされているコンピューター上のパスをこの属性に設定し、ファイルを保存します。 たとえば、Android SDK が **C:\\Programs\\Android\\SDK** にインストールされている場合、`<UserSettings>` は次のようになります。

   ```xml
   <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:ProgramsAndroidSDK" />
   ```

**user.config** をこのように変更すると、Android Device Manager を起動できるようになります。

### <a name="wrong-version-of-android-sdk-tools"></a>不適切なバージョンの Android SDK Tools

Android SDK Tools 26.1.1 以降がインストールされていない場合、起動時に次のエラー ダイアログが表示されることがあります。

![Android SDK インスタンスのエラー ダイアログ](device-manager-images/win/32-sdk-instance-error.png)

このエラー ダイアログが表示された場合は、 **[SDK マネージャーを開く]** をクリックして Android SDK Manager を開きます。 Android SDK Manager で、 **[ツール]** タブをクリックして、以下のパッケージをインストールします。

- **Android SDK Tools 26.1.1** 以降
- **Android SDK Platform-Tools 27.0.1** 以降
- **Android SDK Build Tools 27.0.3** 以降

### <a name="snapshot-disables-wifi-on-android-oreo"></a>スナップショットによって Android Oreo の WiFi が無効になる

Android Oreo 用に構成された AVD で Wi-Fi アクセスをシミュレートしている場合、スナップショットの後で AVD を再起動すると、Wi-Fi アクセスが無効になる場合があります。

この問題を回避するには、次のようにします。

1. Android Device Manager で AVD を選びます。

2. 追加のオプション メニューから、 **[エクスプローラーで表示します]** をクリックします。

3. **snapshots > default_boot** に移動します。

4. **snapshot.pb** ファイルを削除します。

   ![snapshot.pb ファイルの場所](device-manager-images/win/33-delete-snapshot.png)

5. AVD を再起動します。

これらの変更を行った後は、AVD は Wi-Fi が再び機能する状態で再起動します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

### <a name="wrong-version-of-android-sdk-tools"></a>不適切なバージョンの Android SDK Tools

Android SDK Tools 26.1.1 以降がインストールされていない場合、起動時に次のエラー ダイアログが表示されることがあります。

![Android SDK インスタンスのエラー ダイアログ](device-manager-images/mac/29-sdk-instance-error.png)

このエラー ダイアログが表示された場合は、 **[OK]** をクリックして Android SDK Manager を開きます。 Android SDK Manager で、 **[ツール]** タブをクリックして、以下のパッケージをインストールします。

- **Android SDK Tools 26.1.1** 以降
- **Android SDK Platform-Tools 28.0.1** 以降
- **Android SDK Build-Tools 26.0.3** 以降

### <a name="snapshot-disables-wifi-on-android-oreo"></a>スナップショットによって Android Oreo の WiFi が無効になる

Android Oreo 用に構成された AVD で Wi-Fi アクセスをシミュレートしている場合、スナップショットの後で AVD を再起動すると、Wi-Fi アクセスが無効になる場合があります。

この問題を回避するには、次のようにします。

1. Android Device Manager で AVD を選びます。

2. 追加のオプション メニューから、 **[Finder で表示します]** をクリックします。

3. **snapshots > default_boot** に移動します。

4. **snapshot.pb** ファイルを削除します。

   [![snapshot.pb ファイルの場所](device-manager-images/mac/30-delete-snapshot-sml.png)](device-manager-images/mac/30-delete-snapshot.png#lightbox)

5. AVD を再起動します。

これらの変更を行った後は、AVD は Wi-Fi が再び機能する状態で再起動します。

-----

### <a name="generating-a-bug-report"></a>バグ報告の生成

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

上記のトラブルシューティングのヒントで解決できない問題が Android Device Manager で見つかった場合は、タイトル バーを右クリックし、 **[Generate Bug Report]\(バグ報告の生成\)** を選択して、バグ報告を提出してください。

[![バグ報告の提出に関するメニュー項目の場所](device-manager-images/win/34-bug-report-sml.png)](device-manager-images/win/34-bug-report.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

上記のトラブルシューティングのヒントで解決できない問題が Android Device Manager で見つかった場合は、 **[Help]\(ヘルプ\)、[Report a Problem]\(問題の報告\)** の順にクリックして、バグ報告を提出してください。

[![バグ報告の提出に関するメニュー項目の場所](device-manager-images/mac/31-bug-report-sml.png)](device-manager-images/mac/31-bug-report.png#lightbox)

::: zone-end

## <a name="summary"></a>まとめ

このガイドでは、Visual Studio Tools for Xamarin and および Visual Studio for Mac で利用できる Android Device Manager について紹介しました。 Android エミュレーターの開始と停止、実行する Android 仮想デバイス (AVD) の選択、仮想デバイスの編集方法などの、重要な機能について説明しました。 さらにカスタマイズを行うためのハードウェア プロファイル プロパティの編集方法について説明し、一般的な問題のトラブルシューティングのヒントを提供しました。

## <a name="related-links"></a>関連リンク

- [Android SDK ツールの変更](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/How-to-Create-and-Manage-Your-Own-Android-Emulators/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
