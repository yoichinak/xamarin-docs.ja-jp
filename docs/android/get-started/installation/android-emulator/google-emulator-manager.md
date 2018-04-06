---
title: Google エミュレーター マネージャー
description: Google エミュレーター マネージャーを使用して Android 仮想デバイスを作成および管理する方法
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a399aa1c314f1e93377a7831b430e563d9fd1b13
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="google-emulator-manager"></a>Google エミュレーター マネージャー

ハードウェアの高速化が有効になっていることを確認した後は (「[Android Emulator ハードウェアの高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)」を参照)、アプリのテストとデバッグに使う仮想デバイスを作成します。 従来の Google エミュレーター マネージャー (*Android 仮想デバイス (AVD) マネージャー* とも呼ばれます) を使用して、Android SDK エミュレーターで使用するための仮想デバイスを作成することができます。

> [!NOTE]
> Android 8.0 Oreo を対象とする場合は、[Xamarin Android Device Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) を使用して仮想デバイスを作成および構成する必要があります。


## <a name="installing-system-images"></a>システム イメージのインストール

ターゲットとなる Android API レベルに応じて、Android SDK エミュレーターで使用される API レベルに固有のシステム イメージをダウンロードしてインストールする必要があります。 Android API レベルごとに、一連の **x86** システム イメージがあります。仮想デバイスを作成するには、これらのイメージをダウンロードしてインストールする必要があります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

必要なシステム イメージをインストールするには、Android SDK マネージャーを起動して (**[ツール]、[Android]、[Android SDK マネージャー]** の順に移動)、サポートする API レベルまでスクロールします。 API レベルごとに、次のシステム イメージの横にあるチェック ボックスをオンにします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

必要なシステム イメージをインストールするには、Android SDK マネージャーを起動して (**[ツール]、[SDK マネージャー]** の順に移動)、サポートする API レベルまでスクロールします。 API レベルごとに、次のシステム イメージの横にあるチェック ボックスをオンにします。

-----

-   **Intel x86 Atom System Image**
-   **Google APIs Intel x86 Atom System Image**

後者のシステム イメージは、Google API (Google マップ API など) を仮想デバイスに追加します。 

次のスクリーンショットでは、Android 6.0 を実行する仮想マシンを作成できるように、**Intel x86 Atom** イメージがインストールされます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android エミュレーターの Android 6.0 x86 システム イメージの選択](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android エミュレーターの Android 6.0 x86 システム イメージの選択](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png#lightbox)

-----

64 ビットのアプリを開発している場合は、代わりに次のシステム イメージをインストールします。

-   **Intel x86 Atom_64 System Image**
-   **Google APIs Intel x86 Atom_64 System Image**

これらの 64 ビットのシステム イメージを使用して 32 ビットのアプリを実行することができます。ただし、Android SDK エミュレーターでは 32 ビットの **Intel x86 Atom System Image** のほうが若干速く実行されます。

Android Wear 用アプリを開発している場合は、次のシステム イメージをインストールします。

-   **Android Wear Intel x86 Atom System Image**
-   **Google APIs Intel x86 Atom System Image**

これらのシステム イメージがインストールされたら、仮想デバイスの構成 (これについては次のセクションで説明します) 時に適切な API レベルと CPU/ABI を選択して、**x86** ベースの Android 仮想デバイスを作成できます。


## <a name="configuring-virtual-devices"></a>仮想デバイスの構成

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

仮想デバイスは、**Android エミュレーター マネージャー** (_Android 仮想デバイス マネージャー_または _AVD マネージャー_ともいう) を使用して構成されます。 Visual Studio から Android エミュレーター マネージャーを起動するには、次のように、ツール バーの **Android エミュレーター マネージャー**のアイコンをクリックします。

[![AVD アイコンの場所](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png#lightbox)

メニュー バーから Android エミュレーター マネージャーを起動することもできます。その場合は、次のように、**[ツール]、[Android]、[Android エミュレーター マネージャー]** の順に選択します。

[![Android エミュレーター マネージャーのメニュー項目の場所](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png#lightbox)

**[Android 仮想デバイス (AVD) マネージャー]** ダイアログには、既存の Android 仮想デバイスのリストが表示されます。

[![Android 仮想デバイス マネージャー](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

仮想デバイスは、**Android エミュレーター マネージャー** (_Android 仮想デバイス マネージャー_または _AVD マネージャー_ともいう) を使用して構成されます。 

メニュー バーから Android エミュレーター マネージャーを起動できます。その場合は、**[ツール]、[Google エミュレーター マネージャー]** の順に選択します。

[![Android エミュレーター マネージャーのメニュー項目の場所](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png#lightbox)

**[Android 仮想デバイス (AVD) マネージャー]** ダイアログには、既存の Android 仮想デバイスのリストが表示されます。

[![Android 仮想デバイス マネージャー](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png#lightbox)

-----

さまざまなデバイスの特性と API レベルで新しい仮想デバイス イメージを作成することができます。カスタム デバイス定義と仮想デバイスの作成方法については、次のセクションで説明します。


### <a name="creating-a-custom-device-definition"></a>カスタム デバイス定義の作成

カスタム デバイス定義を作成するには、**[Android 仮想デバイス (AVD) マネージャー]** で **[作成...]** をクリックします。 **[Create new Android Virtual Device (AVD)]\(新規 Android 仮想デバイスの作成 (AVD)\)** ダイアログが開きます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 6 に基づくカスタム デバイス定義](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nexus 6 に基づくカスタム デバイス定義](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png#lightbox)

-----

このダイアログで、次のオプションを構成します。

-   **AVD 名** &ndash; デバイス定義の一意の名前。 上の例のスクリーンショットでは、名前は **MyNexus** に設定されています。 AVD 名にスペースを含めることはできないことに注意してください。AVD 名にスペースを使用しようとすると、**[OK]** ボタンが無効になります。

-   **装置** &ndash; エミュレートするハードウェア プロファイル (**Nexus 5** や **Nexus 6** など) を選択します。

-   **ターゲット** &ndash; 仮想デバイスの Android API レベルを選択します。 この設定は、アプリの最小 Android バージョン以上である必要があります。

-   **CPU/ABI** &ndash; **[Google APIs Intel Atom (x86)]** を選択して、デバイス定義で Google API が使用できるようにします。

-   **スキン** &ndash; 仮想デバイスの外観を選択します。 上の例のスクリーンショットでは、**[HVGA]** スキンが選択されています (この記事の最後のエミュレーターのスクリーンショットは **HVGA** スキンの例です)。

-   **メモリ オプション** &ndash; 通常、既定の RAM 設定では高すぎるため、Windows の場合、"**768M を超える RAM のエミュレートは失敗する可能性があります**" という内容の警告が表示されます。 ほとんどのユーザーに対して、RAM を 768 MB に設定することをお勧めします (上のスクリーンショットを参照)。 RAM 値が大きいとエミュレーターの速度が低下する可能性があります。

-   **ホスト GPU を使用する** &ndash; このオプションを選択すると、エミュレーターはホスト コンピューターの GPU (Graphical Processing Unit) を使用してグラフィック操作を実行します。 エミュレーターのパフォーマンスをさらに向上させるために、このオプションを有効にすることをお勧めします。 **エミュレーション オプション** セクションの詳細については、「[What are Snapshot and Use Host GPU emulation options used for?](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for)」 (エミュレーション オプションの [スナップショット] と [ホスト GPU を使用する] の用途) を参照してください。


残りのオプションについては、既定の設定のままでかまいません。 準備ができたら、**[OK]** をクリックして新しい仮想デバイスを作成します。 新しい仮想デバイスの構成結果については、次のダイアログに詳しく示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![新しい AVD の作成後の結果を示すダイアログ](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![新しい AVD の作成後の結果を示すダイアログ](google-emulator-manager-images/mac/07-create-results.png)

-----

このダイアログにリストされる構成プロパティの詳しい説明については、「[ハードウェア プロファイルのプロパティ](https://developer.android.com/studio/run/managing-avds.html#hpproperties)」を参照してください。
**[OK]** をクリックすると、既存の Android 仮想デバイスのリストに新しいデバイスの構成が表示されます。 次のスクリーンショットでは、**MyNexus** がリストに追加されています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイス リストに追加された MyNexus](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デバイス リストに追加された MyNexus](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png#lightbox)

-----

新しいカスタム仮想デバイスは、デバイスのプルダウン メニューにも追加されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイスのプルダウン メニューに追加された MyNexus](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デバイスのプルダウン メニューに追加された MyNexus](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png#lightbox)

-----



### <a name="cloning-a-device-definition"></a>デバイス定義の複製

既存のデバイス定義を選択し、それを*複製*して、新しいカスタム デバイス定義を作成することができます。 これは、ニーズに合わせてごく一部を微調整する必要がある既存のデバイス定義の場合に便利です。 **[Android 仮想デバイス (AVD) マネージャー]** の **[Device Definitions]\(デバイス定義\)** タブには、使用可能なすべてのデバイス定義がリストされます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![使用可能なデバイス定義のリスト](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![使用可能なデバイス定義のリスト](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png#lightbox)

-----

このリストの構成済みデバイスを変更することはできません &ndash; 編集できるのは、ユーザーが作成した仮想デバイスのみです。 デバイス定義を選択し、**[Clone]\(複製\)** をクリックして、構成済みデバイス定義から新しいデバイス定義を派生させることができます。 たとえば、**Nexus 5** 定義を選択して、**[Clone]\(複製\)** をクリックすると、次のダイアログが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイスの複製ダイアログ](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デバイスの複製ダイアログ](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png#lightbox)

-----

次のスクリーンショットでは、名前が **Nexus 5 Custom** に変更され、新しいカスタム デバイス定義を作成するためにデバイスのパラメーターが変更されています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Custom Nexus 5 AVD](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Custom Nexus 5 AVD](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png#lightbox)

-----

**[Clone Device]\(デバイスの複製\)** をクリックすると、新しいデバイス定義が作成され、次のように **[Device Definitions]\(デバイス定義\)** リストに表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![新しいユーザーのデバイス定義として表示されている Nexus 5 Custom](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![新しいユーザーのデバイス定義として表示されている Nexus 5 Custom](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png#lightbox)

-----

上記のように、ユーザーが作成したデバイス定義にはそれぞれ緑色のアイコンが表示されていることに注意してください。 この新しいデバイス定義を使用して、新しい AVD を作成することができます。その場合、定義を選択して、**[AVD の作成]** をクリックします。 **[Create new Android Virtual Device (AVD)]\(新規 Android 仮想デバイスの作成 (AVD)\)** ダイアログが表示されます。 次の例では、新しい仮想デバイスに対して、**AVD\_for\_Nexus\_5\_Custom** という名前が自動的に生成されています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5 Custom ユーザー デバイス定義から AVD を作成する](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nexus 5 Custom ユーザー デバイス定義から AVD を作成する](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png#lightbox)

-----

**[OK]** をクリックすると、既存の Android 仮想デバイスのリストにカスタム デバイス構成が表示されます。 さらに、デバイスのプルダウン メニューに追加されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![デバイスのドロップダウン メニューに追加された新しいカスタム AVD](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![デバイスのドロップダウン メニューに追加された新しいカスタム AVD](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png#lightbox)

-----

