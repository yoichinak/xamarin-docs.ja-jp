---
title: "Android SDK セットアップ"
description: "Visual Studio には、Google のスタンドアロン SDK Manager の代わりとなる Android SDK Manager が含まれています。 このガイドでは、Xamarin.Android アプリの開発に必要な Android SDK ツール、プラットフォーム、およびその他のコンポーネントを SDK Manager を使用してダウンロードする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 585bcac193d6824bc7c96092c14e40fd7971b0e2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="android-sdk-setup"></a>Android SDK セットアップ

_Visual Studio には、Google のスタンドアロン SDK Manager の代わりとなる Android SDK Manager が含まれています。このガイドでは、Xamarin.Android アプリの開発に必要な Android SDK ツール、プラットフォーム、およびその他のコンポーネントを SDK Manager を使用してダウンロードする方法について説明します。_


## <a name="overview"></a>概要

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

このガイドでは、Windows (または[Mac](?tabs=vsmac)) 上の Visual Studio に Xamarin Android SDK Manager をインストールして使用する方法を説明します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

このガイドでは、Mac 用 (または [Windows](?tabs=vswin) 用) の Visual Studio に Xamarin Android SDK Manager をインストールして使用する方法を説明します。

> [!NOTE]
> このガイドが適用されるのは、Visual Studio 2017 と Visual Studio for Mac のみです。  

-----

Xamarin Android SDK Manager を使用すると、Xamarin.Android アプリを開発するために必要な最新の Android コンポーネントをダウンロードできます。
これは使用されていない Google のスタンドアロン SDK Manager の代わりとなります。

Android SDK に含まれている SDK Manager の代わりに Xamarin Android SDK Manager を使用するのはなぜなのでしょうか。 Android SDK Tools パッケージのバージョン 25.2.3 では、Google によって Android SDK を維持する新しいツールが導入されています。 この新しい **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)** というツールは、Android SDK のスタンドアロンの UI マネージャーの代わりとなるコマンド ライン ユーティリティです。 つまり、SDK Tools バージョン 26.0.1 (Android 8.0 に必須) 以降に更新し、UI インターフェイスを使用して Android SDK の管理を継続したい場合、Xamarin Android SDK Manager を使用する必要があります。

## <a name="requirements"></a>必要条件

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android SDK Manager を使用するには、次が必要です。

- Visual Studio 2017 Community Edition 以降。 Visual Studio 2017 バージョン 15.5 以降。

- Xamarin for Visual Studio バージョン 4.5.0 以降。 

Xamarin Android SDK Manager は、Visual Studio と対応していません。
2015. Visual Studio 2015 のユーザーは、Google が Android SDK で提供している SDK Manager ツールを使用する必要があります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Visual Studio for Mac 7.0.0.3146 (またはそれ以上)。

-----

Xamarin Android SDK Manager には、Java Development Kit (Xamarin.Android と同時に自動的にインストールされます) も必要です。
Xamarin.Android は [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) を使用します。これは、API レベル 24 以上で開発する場合に必要となります (JDK 8 は 23 以下の API レベルもサポートしています)。 API レベル 23 以下のみで開発している場合は、[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) を引き続き使用することができます。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>インストール

Xamarin SDK Manager は、Visual Studio 2017 にインストール時に追加できます。 Visual Studio をインストールする場合、**[個別のコンポーネント]** をクリックして、**[開発作業]** セクションまでスクロール ダウンします。
まだオンにしていない場合、**[Xamarin SDK Manager]** をオンにします。

![[個別のコンポーネント] からの Xamarin SDK Manager の有効化](android-sdk-images/win/01-sdk-manager-install.png)

Visual Studio 2017 を既にインストールしている場合は、「[Modify Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio)」 (Visual Studio 2017 を変更する) で Visual Studio を変更する方法について確認し、上記の手順に従って Xamarin SDK Manager を有効にします。 SDK Manager を更新するプロンプトが表示される場合は、Xamarin SDK Manager をインストールする同じ手順を使用します。

**[ツール]、[Android]、[Android SDK Manager]** (この後で説明します) の順にクリックすると、Google Android SDK Manager の代わりに Xamarin Android SDK Manager が起動されます。 Google のスタンドアロン Android SDK Manager をサポートしている Android SDK の以前のバージョンを使用している場合、Xamarin Android SDK Manager をインストールしても競合は発生せず、Visual Studio 外からスタンドアロンの Google SDK Manager を起動して Android SDK を管理できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
-----

 
## <a name="sdk-manager"></a>SDK Manager 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio で SDK Manager を起動するには、**[ツール]、[Android]、[Android SDK Manager]** の順にクリックします。

[![Android SDK Manager のメニュー アイテムの場所](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Xamarin Android SDK Manager** が **[Android SDK とツール]** 画面で開きます。 この画面には、**[プラットフォーム]** と **[ツール]** の 2 つのタブがあります。

[![[プラットフォーム] タブが開かれた Android SDK Manager のスクリーン ショット](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**[Android SDK とツール]** 画面の詳細については、以降のセクションで詳しく説明します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac で SDK Manager を起動するには、**[ツール]、[SDK Manager]** の順にクリックします。
 
![Android SDK Manager のメニュー アイテムの場所](android-sdk-images/mac/sdkmanager-01.png )

**Android SDK Manager** は、**[プラットフォーム]**、**[ツール]**、および **[場所]** の 3 つのタブがある **[Preferences window]**\(環境設定ウィンドウ\) に開きます。

![[プラットフォーム] タブが開かれた Android SDK Manager のスクリーン ショット](android-sdk-images/mac/sdkmanager-02.png)

Xamarin Android SDK Manager のタブは以降のセクションで説明します。

-----



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Android SDK の場所

上の図のとおり、Android SDK の場所は **[Android SDK とツール]** 画面の上部に構成されています。 この場所は、**[プラットフォーム]** および **[ツール]** タブが正しく動作するために正しく構成される必要があります。 次の 1 つ以上の理由により、Android SDK の場所を設定する必要があります。

1. Xamarin SDK Manager で Android SDK を見つけることができない。 

2. Android SDK を別の (既定でない) 場所にインストールした。 

Android SDK の場所を設定するには、**[Android SDK の場所]** の右端の &hellip; をクリックします。 これにより Android SDK の場所へのナビゲーションに使用できる、**[フォルダーの参照]** ダイアログが開きます。 次のスクリーンショットでは、**Program Files (x86)\\Android** の下の Android SDK が選択されています。

![Android SDK の場所を指定している Windows [フォルダーの参照] ダイアログのスクリーンショット](android-sdk-images/win/05-browse-for-folder.png)

**[OK]** をクリックすると、選択された場所にインストールされている Android SDK が Xamarin Android SDK Manager によって管理されます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="locations-tab"></a>[場所] タブ

**[場所]** タブには、Android SDK、Android NDK および Java SDK (JDK) の場所を構成する 3 つの設定があります。 これらの場所は、**[プラットフォーム]** および **[ツール]** タブが正しく動作するために正しく構成される必要があります。

SDK Manager が起動されると、インストールされている各パッケージのパスが自動的に判別され、パスの横に緑のチェックマークが表示され、それが**検出**済みであることが示されます。

![[場所] タブのスクリーンショット](android-sdk-images/mac/sdkmanager-03.png)

**[既定値にリセット]** ボタンをクリックして、SDK Manager に既定の場所から SDK、NDK、および JDK を検索させます。 

Android SDK および Java JDK の場所を変更する場合、通常は **[場所]** タブを使用します。 Xamarin.Android アプリを開発するために NDK をインストールする必要はありません。NDK は、アプリの一部を C や C++ などのネイティブ コードの言語を使用して開発する必要がある場合にのみ使用します。

-----


### <a name="tools-tab"></a>[ツール] タブ

**[ツール]** タブには_ツール_と_その他_の一覧が表示されます。 Android SDK のツール、プラットフォーム ツールおよびビルド ツールをインストールするには、このタブを使用します。
また、Android Emulator、低レベル デバッガー (LLDB)、NDK HAXM アクセラレータ、および Google Play ライブラリをインストールすることもできます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

たとえば、Google Android Emulator パッケージをダウンロードするには、**[Android Emulator]** の横のチェック マークをクリックし、**[変更の適用]** ボタンをクリックします。

[![[ツール] タブからの Android Emulator のインストール](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

たとえば、Google Android Emulator パッケージをダウンロードするには、**[Android Emulator]** の横のチェック マークをクリックし、**[更新プログラムのインストール]** ボタンをクリックします。

![[ツール] タブからの Android Emulator のインストール](android-sdk-images/mac/sdkmanager-08.png)

-----


[_Some components can be updated.Do you want to update them now?_]\(一部のコンポーネントを更新できます。今すぐ更新しますか?\) というメッセージが含まれるダイアログが表示される場合があります。 **[はい]**をクリックします。 次に、[ライセンスの同意] ダイアログが表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![[ライセンスの同意] 画面](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![[ライセンスの同意] 画面](android-sdk-images/mac/sdkmanager-09.png)

-----

使用条件に同意する場合、**[同意]** をクリックします。 ウィンドウの下部に、ダウンロードとインストールの進行状況を示す進行状況バーが表示されます。 インストールが完了すると、**[ツール]** タブに選択したツールとその他の機能がインストールされたことが表示されます。



### <a name="platforms-tab"></a>[プラットフォーム] タブ

**[プラットフォーム]** タブに、各プラットフォームのプラットフォーム SDK バージョンとその他のリソース (システム イメージなど) 一覧が表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![[プラットフォーム] ウィンドウのスクリーン ショット](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![[プラットフォーム] ウィンドウのスクリーン ショット](android-sdk-images/mac/sdkmanager-11.png)

-----

この画面には、(**Android 7.0** などの) Android のバージョン、コード名 (**Nougat**)、(**24** などの) API レベル、および (プラットフォームがインストール済みの場合は**インストール済みなどの**) 状態が表示されます。 **[プラットフォーム]** タブは対象の Android API レベルのコンポーネントをインストールするために使用します (Android のバージョンと API レベルの詳細については、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください)。

プラットフォームのすべてのコンポーネントがインストールされている場合、プラットフォーム名の横にチェック マークが表示されます。 プラットフォームのすべてのコンポーネントがインストールされてない場合は、そのプラットフォームのボックスは塗りつぶされています。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

プラットフォームの左側の **+** ボックスをクリックすると、そのプラットフォームのコンポーネント (とインストール済みのコンポーネント) を確認することができます。
プラットフォームのコンポーネント一覧を折りたたむには、**-** をクリックします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

プラットフォームのコンポーネントを表示 (し、インストール済みのコンポーネントを確認) するには、プラットフォームの左の**矢印**をクリックして、それを展開します。
**下向きの矢印**をクリックして、プラットフォームのコンポーネント一覧を折りたたみます。

-----

SDK に別のプラットフォームを追加する場合、そのプラットフォームのすべてのコンポーネントをインストールするチェック マークが表示されるまで、プラットフォームの横にあるボックスをクリックし、**[変更の適用]** をクリックします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android SDK に Android 7.1 Nougat コンポーネントを追加する例](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android SDK に Android 4.4 コンポーネントを追加する例](android-sdk-images/mac/sdkmanager-12.png)

-----

SDK のみをインストールするには、プラットフォームの横にあるボックスを 1 回クリックします。 その後、必要な個々のコンポーネントを選択します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![いくつかの Android 7.1 コンポーネントを追加する例](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![いくつかの Android 4.4 コンポーネントを追加する例](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

インストールするコンポーネントの数が **[変更の適用]** ボタンの横に表示されます。 上の例では、6 つのコンポーネントをインストールする準備が整っています。 **[変更の適用]** ボタンをクリックすると、**[ライセンスの同意]** 画面が表示されます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

インストールするコンポーネントの数が **[変更の適用]** ボタンの横に表示されます。 **[変更の適用]** ボタンをクリックすると、**[ライセンスの同意]** 画面が表示されます。

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![[プラットフォーム] タブの [ライセンスの同意] ダイアログ](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![[プラットフォーム] タブの [ライセンスの同意] ダイアログ](android-sdk-images/mac/sdkmanager-14.png)

-----

使用条件に同意する場合、**[同意]** をクリックします。 このダイアログ ボックスは、コンポーネントを複数インストールする必要がある場合、2 回以上表示される場合があります。 ウィンドウの下部に、ダウンロードとインストールの進行状況を示す進行状況バーが表示されます。 ダウンロードおよびインストール手順が完了すると (ダウンロードするコンポーネント数に応じてこれには何分もかかる場合があります)、追加されたコンポーネントはチェック マーク付きで**インストール済み**と表示されます。

これで最新かつ最高の Android API レベルでアプリを開発する準備ができました。


 
## <a name="summary"></a>まとめ

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

このガイドでは、Visual Studio に Xamarin Android SDK Manager ツールをインストールして使用する方法を説明しました。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

このガイドでは、Visual Studio for Mac で Xamarin Android SDK Manager ツールを使用する方法を説明しました。

-----


## <a name="related-links"></a>関連リンク

- [Android SDK ツールの変更](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
