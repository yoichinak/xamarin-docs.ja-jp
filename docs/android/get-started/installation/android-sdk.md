---
title: Xamarin.Android 向け Android SDK を設定する
description: Visual Studio には、Xamarin.Android アプリの開発に必要な Android SDK ツール、プラットフォーム、およびその他のコンポーネントをダウンロードするために使用する SDK Manager が含まれています。
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/09/2018
ms.openlocfilehash: 66b9e89080d88fa6f8192b42bc8ab0a97c92c20c
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58071009"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Xamarin.Android 向け Android SDK を設定する

_Visual Studio には、Xamarin.Android アプリの開発に必要な Android SDK ツール、プラットフォーム、およびその他のコンポーネントをダウンロードするために使用する SDK Manager が含まれています。_


## <a name="overview"></a>概要

このガイドでは、Visual Studio および Visual Studio for Mac で Xamarin Android SDK Manager ツールを使用する方法を説明しました。

> [!NOTE]
> このガイドが適用されるのは、Visual Studio 2017 と Visual Studio for Mac のみです。  

(**.NET によるモバイル開発**の一部分としてインストールされる) Xamarin Android SDK Manager を使用すると、Xamarin.Android アプリを開発するために必要な最新の Android コンポーネントをダウンロードできます。 これは非推奨とされた Google のスタンドアロン SDK Manager の代わりとなります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="requirements"></a>要件

Xamarin Android SDK Manager を使用するには、次が必要です。

- Visual Studio 2017 (Community、Professional、または Enterprise Edition)。 Visual Studio 2017 バージョン 15.7 以降。

- Visual Studio Tools for Xamarin バージョン 4.10.0 以降 (**.NET によるモバイル開発**ワークロードの一部としてインストールされる)。 

Xamarin Android SDK Manager には、Java Development Kit (Xamarin.Android と同時に自動的にインストールされます) も必要です。 JDK の代わりとして、次のような選択肢があります。

-   既定では、Xamarin.Android は [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) を使用します。これは、API レベル 24 以上で開発する場合に必須となります (JDK 8 は 23 以下の API レベルもサポートしています)。

-   API レベル 23 以下のみで開発している場合は、[JDK 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) を引き続き使用することができます。

-   Visual Studio 15.8 Preview 5 以降を使用している場合は、JDK 8 ではなく [Microsoft が配布する OpenJDK](openjdk.md) (現在プレビュー段階です) を使用することができます。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

 
## <a name="sdk-manager"></a>SDK Manager 

Visual Studio で SDK Manager を起動するには、**[ツール]、[Android]、[Android SDK Manager]** の順にクリックします。

[![Android SDK Manager のメニュー アイテムの場所](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Android SDK Manager が **[Android SDK とツール]** 画面で開きます。 この画面には、**[プラットフォーム]** と **[ツール]** の 2 つのタブがあります。

[![[プラットフォーム] タブが開かれた Android SDK Manager のスクリーン ショット](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**[Android SDK とツール]** 画面の詳細については、以降のセクションで詳しく説明します。


### <a name="android-sdk-location"></a>Android SDK の場所

上のスクリーンショットのとおり、Android SDK の場所は **[Android SDK とツール]** 画面の上部に構成されています。 この場所は、**[プラットフォーム]** および **[ツール]** タブが正しく動作するために正しく構成される必要があります。 次の 1 つ以上の理由により、Android SDK の場所を設定する必要があります。

1. Android SDK Manager で Android SDK が見つからなかった。 

2. Android SDK を別の (既定でない) 場所にインストールした。 

Android SDK の場所を設定するには、**[Android SDK の場所]** の右端の省略記号 (&hellip;) ボタンをクリックします。 これにより Android SDK の場所へのナビゲーションに使用できる、**[フォルダーの参照]** ダイアログが開きます。 次のスクリーンショットでは、**Program Files (x86)\\Android** の下の Android SDK が選択されています。

![Android SDK の場所を指定している Windows [フォルダーの参照] ダイアログのスクリーンショット](android-sdk-images/win/05-browse-for-folder.png)

**[OK]** をクリックすると、選択された場所にインストールされている Android SDK が SDK Manager によって管理されます。


### <a name="tools-tab"></a>[ツール] タブ

**[ツール]** タブには_ツール_と_その他_の一覧が表示されます。 Android SDK のツール、プラットフォーム ツールおよびビルド ツールをインストールするには、このタブを使用します。
また、Android Emulator、低レベル デバッガー (LLDB)、NDK HAXM アクセラレータ、および Google Play ライブラリをインストールすることもできます。


たとえば、Google Android Emulator パッケージをダウンロードするには、**[Android Emulator]** の横のチェック マークをクリックし、**[変更の適用]** ボタンをクリックします。

[![[ツール] タブからの Android Emulator のインストール](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

ダイアログに_次のパッケージでは、インストールする前にライセンス条項に同意することが必要です_というメッセージが表示される場合があります。

![[ライセンスの同意] 画面](android-sdk-images/win/07-license-acceptance.png)

使用条件に同意する場合、**[同意]** をクリックします。 ウィンドウの下部に、ダウンロードとインストールの進行状況を示す進行状況バーが表示されます。 インストールが完了すると、**[ツール]** タブに選択したツールとその他の機能がインストールされたことが表示されます。

### <a name="platforms-tab"></a>[プラットフォーム] タブ

**[プラットフォーム]** タブに、各プラットフォームのプラットフォーム SDK バージョンとその他のリソース (システム イメージなど) 一覧が表示されます。

[![[プラットフォーム] ウィンドウのスクリーン ショット](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

この画面には、(**Android 8.0** などの) Android のバージョン、コード名 (**Oreo**)、(**26** などの) API レベル、そのプラットフォームのコンポーネントの (**1 GB** などの) サイズが表示されます。 **[プラットフォーム]** タブを使用して、対象とする Android API レベルのコンポーネントを一覧表示します。 Android のバージョンと API レベルの詳細については、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」(Android API レベルについて) を参照してください。

プラットフォームのすべてのコンポーネントがインストールされていると、プラットフォーム名の横にチェック マークが表示されます。 プラットフォームのすべてのコンポーネントがインストールされてない場合は、そのプラットフォームのボックスは塗りつぶされています。 プラットフォームの左側の **+** ボックスをクリックすると、そのプラットフォームのコンポーネント (とインストール済みのコンポーネント) を確認することができます。
プラットフォームのコンポーネント一覧を折りたたむには、**-** をクリックします。

SDK に別のプラットフォームを追加する場合、そのプラットフォームのすべてのコンポーネントをインストールするチェック マークが表示されるまで、プラットフォームの横にあるボックスをクリックし、**[変更の適用]** をクリックします。

[![Android SDK に Android 7.1 Nougat コンポーネントを追加する例](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

特定のコンポーネントのみをインストールするには、プラットフォームの横にあるボックスを 1 回クリックします。 その後、必要な個々のコンポーネントを選択します。

[![いくつかの Android 7.1 コンポーネントを追加する例](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

インストールするコンポーネントの数が **[変更の適用]** ボタンの横に表示されます。 **[変更の適用]** ボタンをクリックすると、前に示したような **[ライセンスの同意]** 画面が表示されます。
使用条件に同意する場合、**[同意]** をクリックします。 このダイアログ ボックスは、コンポーネントを複数インストールする必要がある場合、2 回以上表示される場合があります。 ウィンドウの下部に、ダウンロードとインストールの進行状況を示す進行状況バーが表示されます。 ダウンロードおよびインストール手順が完了すると (ダウンロードするコンポーネント数に応じてこれには何分もかかる場合があります)、追加されたコンポーネントはチェック マーク付きで**インストール済み**と表示されます。

### <a name="respository-selection"></a>リポジトリの選択

既定では、Android SDK Manager は、Microsoft が管理するリポジトリからプラットフォームのコンポーネントとツールをダウンロードします。 Microsoft リポジトリでまだ使用できない試験段階のアルファ/ベータ版のプラットフォームとツールにアクセスする必要がある場合は、Google のリポジトリを使用するように SDK Manager を切り替えることができます。 この切り替えを行うには、右下隅の歯車アイコンをクリックして、**[リポジトリ]、[Google (サポート対象外)]** の順に選択します。

[![Google のリポジトリの選択](android-sdk-images/win/11-google-repo-w157-sml.png)](android-sdk-images/win/11-google-repo-w157.png#lightbox)

Google リポジトリを選択すると、以前は使用できなかった追加のパッケージが **[プラットフォーム]** タブに表示される場合があります。 (上のスクリーン ショットでは、Google リポジトリに切り替えたことで **[Android SDK Platform 28]** が追加されています。)Google リポジトリはサポートの対象外であるため、日常的な開発には推奨されないことに注意してください。

プラットフォームとツールのサポート対象のリポジトリに戻すには、**[Microsoft (推奨)]** をクリックします。 これにより、パッケージとツールの一覧が、既定の選択に戻ります。


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="requirements"></a>要件

Xamarin Android SDK Manager を使用するには、次が必要です。

-   Visual Studio for Mac 7.5 (またはそれ以降)。

Xamarin Android SDK Manager には、Java Development Kit (Xamarin.Android と同時に自動的にインストールされます) も必要です。 JDK の代わりとして、次のような選択肢があります。

-   既定では、Xamarin.Android は [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) を使用します。これは、API レベル 24 以上で開発する場合に必須となります (JDK 8 は 23 以下の API レベルもサポートしています)。

-   API レベル 23 以下のみで開発している場合は、[JDK 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) を引き続き使用することができます。

-   Visual Studio for Mac 7.7 以降を使用している場合は、JDK 8 ではなく [Microsoft が配布する OpenJDK](openjdk.md) (現在プレビュー段階です) を使用することができます。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。
 
## <a name="sdk-manager"></a>SDK Manager 

Visual Studio for Mac で SDK Manager を起動するには、**[ツール]、[SDK Manager]** の順にクリックします。
 
[![Android SDK Manager のメニュー アイテムの場所](android-sdk-images/mac/01-sdk-manager-menu-item-m75-sml.png)](android-sdk-images/mac/01-sdk-manager-menu-item-m75.png#lightbox)

**Android SDK Manager** は、**[プラットフォーム]**、**[ツール]**、および **[場所]** の 3 つのタブがある **[Preferences window]** \(環境設定ウィンドウ\) に開きます。

[![[プラットフォーム] タブが開かれた Android SDK Manager のスクリーン ショット](android-sdk-images/mac/02-sdk-manager-platforms-m75-sml.png)](android-sdk-images/mac/02-sdk-manager-platforms-m75.png#lightbox)

Android SDK Manager のタブは以降のセクションで説明します。


### <a name="locations-tab"></a>[場所] タブ

**[場所]** タブには、Android SDK、Android NDK および Java SDK (JDK) の場所を構成する 3 つの設定があります。 これらの場所は、**[プラットフォーム]** および **[ツール]** タブが正しく動作するために正しく構成される必要があります。

SDK Manager が起動されると、インストールされている各パッケージのパスが自動的に判別され、パスの横に緑のチェックマークが表示され、それが**検出**済みであることが示されます。

[![[場所] タブのスクリーンショット](android-sdk-images/mac/03-locations-tab-m75-sml.png)](android-sdk-images/mac/03-locations-tab-m75.png#lightbox)

**[既定値にリセット]** ボタンをクリックして、SDK Manager に既定の場所から SDK、NDK、および JDK を検索させます。 

Android SDK および Java JDK の場所を変更する場合、通常は **[場所]** タブを使用します。 Xamarin.Android アプリを開発するために NDK をインストールする必要はありません。NDK は、アプリの一部を C や C++ などのネイティブ コードの言語を使用して開発する必要がある場合にのみ使用します。

### <a name="tools-tab"></a>[ツール] タブ

**[ツール]** タブには_ツール_と_その他_の一覧が表示されます。 Android SDK のツール、プラットフォーム ツールおよびビルド ツールをインストールするには、このタブを使用します。
また、Android Emulator、低レベル デバッガー (LLDB)、NDK HAXM アクセラレータ、および Google Play ライブラリをインストールすることもできます。

たとえば、Google Android Emulator パッケージをダウンロードするには、**[Android Emulator]** の横のチェック マークをクリックし、**[変更の適用]** ボタンをクリックします。

[![[ツール] タブからの Android Emulator のインストール](android-sdk-images/mac/04-tools-tab-m75-sml.png)](android-sdk-images/mac/04-tools-tab-m75.png#lightbox)

ダイアログに_次のパッケージでは、インストールする前にライセンス条項に同意することが必要です_というメッセージが表示される場合があります。

[![[ライセンスの同意] 画面](android-sdk-images/mac/05-license-acceptance-m75-sml.png)](android-sdk-images/mac/05-license-acceptance-m75.png#lightbox)

使用条件に同意する場合、**[同意]** をクリックします。 ウィンドウの下部に、ダウンロードとインストールの進行状況を示す進行状況バーが表示されます。 インストールが完了すると、**[ツール]** タブに選択したツールとその他の機能がインストールされたことが表示されます。


### <a name="platforms-tab"></a>[プラットフォーム] タブ

**[プラットフォーム]** タブに、各プラットフォームのプラットフォーム SDK バージョンとその他のリソース (システム イメージなど) 一覧が表示されます。

[![[プラットフォーム] ウィンドウのスクリーン ショット](android-sdk-images/mac/06-platforms-tab-m75-sml.png)](android-sdk-images/mac/06-platforms-tab-m75.png#lightbox)

この画面には、(**Android 8.1** などの) Android のバージョン、コード名 (**Oreo**)、(**27** などの) API レベル、そのプラットフォームのコンポーネントの (**1 GB** などの) サイズが表示されます。 **[プラットフォーム]** タブを使用して、対象とする Android API レベルのコンポーネントを一覧表示します。 Android のバージョンと API レベルの詳細については、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」(Android API レベルについて) を参照してください。

プラットフォームのすべてのコンポーネントがインストールされていると、プラットフォーム名の横にチェック マークが表示されます。 プラットフォームのすべてのコンポーネントがインストールされてない場合は、そのプラットフォームのボックスは塗りつぶされています。 プラットフォームのコンポーネントを表示 (し、インストール済みのコンポーネントを確認) するには、プラットフォームの左の**矢印**をクリックして、それを展開します。
**下向きの矢印**をクリックして、プラットフォームのコンポーネント一覧を折りたたみます。

SDK に別のプラットフォームを追加する場合、そのプラットフォームのすべてのコンポーネントをインストールするチェック マークが表示されるまで、プラットフォームの横にあるボックスをクリックし、**[変更の適用]** をクリックします。

[![プラットフォームの全コンポーネントを追加する例](android-sdk-images/mac/07-install-all-m75-sml.png)](android-sdk-images/mac/07-install-all-m75.png#lightbox)

一部のコンポーネントのみをインストールするには、プラットフォームの横にあるボックスを 1 回クリックします。 その後、必要な個々のコンポーネントを選択します。

[![いくつかのコンポーネントを追加する例](android-sdk-images/mac/08-individual-components-m75-sml.png)](android-sdk-images/mac/08-individual-components-m75.png#lightbox)

インストールするコンポーネントの数が **[変更の適用]** ボタンの横に表示されます。 **[変更の適用]** ボタンをクリックすると、前に示したような **[ライセンスの同意]** 画面が表示されます。
使用条件に同意する場合、**[同意]** をクリックします。 このダイアログ ボックスは、コンポーネントを複数インストールする必要がある場合、2 回以上表示される場合があります。 ウィンドウの下部に、ダウンロードとインストールの進行状況を示す進行状況バーが表示されます。 ダウンロードおよびインストール手順が完了すると (ダウンロードするコンポーネント数に応じてこれには何分もかかる場合があります)、追加されたコンポーネントはチェック マーク付きで**インストール済み**と表示されます。

### <a name="respository-selection"></a>リポジトリの選択

既定では、Android SDK Manager は、Microsoft が管理するリポジトリからプラットフォームのコンポーネントとツールをダウンロードします。 Microsoft リポジトリでまだ使用できない試験段階のアルファ/ベータ版のプラットフォームとツールにアクセスする必要がある場合は、Google のリポジトリを使用するように SDK Manager を切り替えることができます。 この切り替えを行うには、右下隅の歯車アイコンをクリックして、**[リポジトリ]、[Google (サポート対象外)]** の順に選択します。

[![Google のリポジトリの選択](android-sdk-images/mac/09-google-repo-m75-sml.png)](android-sdk-images/mac/09-google-repo-m75.png#lightbox)

Google リポジトリを選択すると、以前は使用できなかった追加のパッケージが **[プラットフォーム]** タブに表示される場合があります。 (上のスクリーン ショットでは、Google リポジトリに切り替えたことで **[Android SDK Platform 28]** が追加されています。)Google リポジトリはサポートの対象外であるため、日常的な開発には推奨されないことに注意してください。

プラットフォームとツールのサポート対象のリポジトリに戻すには、**[Microsoft (推奨)]** をクリックします。 これにより、パッケージとツールの一覧が、既定の選択に戻ります。

-----

 
## <a name="summary"></a>まとめ

このガイドでは、Visual Studio および Visual Studio for Mac に Xamarin Android SDK Manager ツールをインストールして使用する方法を説明しました。


## <a name="related-links"></a>関連リンク

- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [Android SDK ツールの変更](~/android/troubleshooting/sdk-cli-tooling-changes.md)
