---
title: Windows に Xamarin.iOS をインストールする
description: この記事では、Xamarin iOS for Visual Studio を設定する方法について説明します。 Visual Studio 用の Xamarin 拡張機能のインストール プロセス、および Mac にインストールされている Apple SDK への接続について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: 08bf8b2b7c56983c43cf1ae080ab112e81851fbb
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="installing-xamarinios-on-windows"></a>Windows に Xamarin.iOS をインストールする

_この記事では、Xamarin iOS for Visual Studio を設定する方法について説明します。Visual Studio 用の Xamarin 拡張機能のインストール プロセス、および Mac にインストールされている Apple SDK への接続について説明します。_

## <a name="overview"></a>概要

Xamarin.iOS for Visual Studio を使うと、iOS アプリケーションを Windows コンピューターで作成してテストすることができます。そのとき、ネットワークで接続された Mac がビルドと配置サービスを提供します。

Visual Studio 内での iOS 向けの開発では、次のことができます。

- iOS、Android、および Windows アプリケーション用のクロスプラットフォーム ソリューションを作成する。
- iOS ソース コードなど、すべてのクロスプラットフォーム プロジェクトに Visual Studio ツール (*Resharper*、*Team Foundation Server* など) を使う。
- 慣れた IDE を使いながら、Apple のすべての API の Xamarin.iOS バインディングを利用する。

Xamarin.iOS for Visual Studio は、Visual Studio が Mac 上の Windows 仮想マシンで実行している構成 (Parallels または VMWare を使用)、または Mac と同じネットワーク上で認識される別のコンピューター上にある構成をサポートします。 どちらの構成でも、Visual Studio は SSH を使って迅速かつ安全に Mac に接続します。

この記事では、Mac と Windows コンピューターの両方に Xamarin.iOS ツールをインストールして構成する手順、および開発者が Visual Studio を使って Xamarin.iOS アプリケーションをビルド、デバッグ、展開できるように Mac ホストに接続する手順について説明します。

次の図は、Xamarin.iOS 開発ワークフローの簡単な概要です。

[![Xamarin.iOS 開発ワークフロー](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> Visual Studio は、実際には、別の MSBuild プロセスを開始してプロジェクトをビルドします。 このプロセスは、Mac への新しい接続を作成します。つまり、Visual Studio がビルドを行うとき、実際には Windows から Mac に 2 つの SSH 接続が存在します。 [コマンド ライン](~/ios/get-started/installation/windows/connecting-to-mac/index.md)からのビルドでは、作成される MSBuild プロセスは 1 つだけです。 図をわかりやすくするため、すべての接続は単に 1 つの矢印で表されています。

## <a name="requirements"></a>必要条件

Xamarin.iOS for Visual Studio は驚きの機能を実行します。これにより、開発者は、Visual Studio IDE を使って Windows コンピューター上で iOS アプリケーションの作成、ビルド、デバッグを行うことができます。 Xamarin.iOS for Visual Studio だけでこれを行うことはできません。iOS アプリケーションを作成するには、Apple のコンパイラが必要であり、配置するには、Apple の証明書とコード署名ツールが必要です。 つまり、Xamarin.iOS for Visual Studio のインストールがこれらのタスクを実行するには、Mac OS X コンピューターへのネットワーク接続が必要です。 いったん構成されると、Xamarin のツールはプロセスを可能な限りシームレスにします。


<a name="system-requirements"/>

### <a name="system-requirements"></a>システム要件

システム要件は次のとおりです。

### <a name="windows"></a>Windows

1. Windows 7 以上。

2. Visual Studio 2015 Professional 以降

    a.  Enterprise ライセンスがある場合は、Visual Studio Enterprise をインストールする必要があります。

3. Xamarin for Visual Studio。

プラグインのサポートがないため、Visual Studio の Express エディションでは Xamarin ツールは使用できません。 Xamarin は Visual Studio コミュニティでサポートされます。

### <a name="mac"></a>Mac

1. macOS Sierra (10.12) 以降を実行する Mac (最新の安定バージョンを推奨)。

2. Xamarin iOS SDK。 これは Visual Studio for Mac をダウンロードするときにインストールされます

3. Apple の Xcode IDE と iOS SDK (Mac App Store からの最新の安定バージョンを推奨)。

**Windows コンピューターは、ネットワーク経由で Mac にアクセスできる必要があります。**

### <a name="apple-developer-account"></a>Apple 開発者アカウント

アプリケーションをデバイスに展開したり、App Store に送信したりするには、Apple 開発者アカウントが必要です。 関連する開発者証明書とプロビジョニング プロファイルを作成し、ネットワークで接続された Mac にインストールしてからでないと、Xamarin.iOS for Visual Studio は動きません。 開発証明書を取得してデバイスをプロビジョニングする手順については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)」(デバイスのプロビジョニング) をご覧ください。

## <a name="features"></a>フィーチャー 

Xamarin.iOS for Visual Studio を使うと、Windows から Xamarin.iOS プロジェクトを作成、編集、ビルド、配置できます。 次の機能があります。

- 新しい iOS プロジェクトを作成する。

- iOS プロジェクトの編集、および Xamarin.Android プロジェクトと UWP プロジェクトも含むクロスプラットフォーム ソリューションの編集を行う。

- iOS プロジェクトのコンパイル、および Xamarin.Android プロジェクトと UWP プロジェクトも含むクロスプラットフォーム ソリューションのコンパイルを行う。

- iOS Designer を使ってストーリーボードと .xib をサポートする。

- iOS アプリケーションを展開してデバッグする。アプリ自体はシミュレーター、または Mac に接続されたデバイスで実行します。

- Windows 上の iOS シミュレーター – 使い方について詳しくは、[こちら](~/tools/ios-simulator.md)のガイドをご覧ください。

<a name="configuring" />

## <a name="configuring-your-mac"></a>Mac の構成

<a name="installation"/>

### <a name="installation"></a>インストール

Mac ホストに Xamarin.iOS ツールをインストールするには、[Visual Studio for Mac をインストールする](https://docs.microsoft.com/visualstudio/mac/installation)必要があります。

ソフトウェアをインストールした後は、次のセクションの手順に従い、macOS 上の Xamarin.iOS を構成して、Xamarin for Visual Studio がそれに接続できるようにします。

> [!IMPORTANT]
> Windows マシンで使用する Xamarin.iOS のバージョンは、接続している Mac と同じバージョンである必要があります。 これは、次の方法で確認できます。
>
> - **Visual Studio 2015 以前の場合**: Visual Studio for Mac と同じ[更新チャネル](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)を使用していることを確認します。
>
> - **Visual Studio 2017、リリース バージョンの場合**: Visual Studio for Mac の**安定**チャネルを使用していることを確認します。
>
> - **Visual Studio 2017、プレビュー バージョンの場合**: Visual Studio for Mac の**アルファ** チャネルを使用していることを確認します。

<a name="configuration" />


### <a name="configuration"></a>構成

Visual Studio 用 Xamarin 拡張機能と Mac の間の通信にアクセスするには、Mac で**リモート ログイン**を許可する必要があります。 以下の手順でセットアップします。

1. *Spotlight* (**Cmd-Space**) を開き、**リモート ログイン**を検索し、結果で **[Sharing]\(共有\)** を選びます。 これにより、**[Sharing]\(共有\)** パネルで **[System Preferences]\(システム環境設定\)** が開きます。

![リモート ログインのスポットライト検索](images/spotlight.png)

2. Xamarin for Visual Studio に Mac への接続を許可するには、左にある **[Service]\(サービス\)** リストの **[Remote Login]\(リモート ログイン\)** オプションをオンにします。

![[サービス] リストの [リモート ログイン] オプションをオンにします](images/sharing.png)

3. **[Remote Login]\(リモート ログイン\)** のアクセス許可が **[All users]\(すべてのユーザー\)** に設定されているか、使用している Mac のユーザー名またはグループが右側のリストの許可されたユーザーのリストに含まれていることを確認します。

Mac が Visual Studio で検出可能になります (同じネットワーク上にある場合)。

> [!NOTE]
> 署名されたアプリケーションを既定でブロックするように macOS ファイアウォールを設定している場合は、`mono-sgen` による着信接続の受信を許可することが必要な場合があります。 その場合、警告ダイアログが表示されます。

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>iOS 開発者のセットアップ

iOS の開発では、Mac コンピューターに関連する署名 ID を構成することが重要です。 これにより、アプリに正しく署名して、App Store またはアドホックを介してアプリを配布することができます。 Xamarin で iOS 開発用 Mac を設定する方法の手順については、次のリンクをご覧ください。

- [デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Mac を構成した後は、Windows コンピューターを設定します。

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Windows のインストール

Xamarin は、Visual Studio 2017 または 2015 の一部としてインストールできます。 Xamarin 用の Visual Studio ツールをインストールするには、[Windows のインストール](~/cross-platform/get-started/installation/windows.md)のガイドをご覧ください。

## <a name="installation-complete"></a>インストールの完了

インストール プロセスが完了した後、すべてを動作させるには、さらにいくつかの手順を実行する必要があります。

- [Visual Studio を Mac に接続する](#connectingtomac) – Xamarin.iOS プロジェクトをビルドするには、Visual Studio を Mac ビルド ホストに接続する必要があります。
- [Visual Studio のツールバーを構成する](#toolbar) – これにより、Visual Studio で Xamarin.iOS の機能に簡単にアクセスできるようになります。

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Mac への接続

Xamarin.iOS for Visual Studio から Mac ビルド ホストへの接続は、コンピューター間の SSH 接続を使って行われます。 接続について詳しくは、「[Connecting to Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」(Mac への接続) ガイドをご覧ください。

Mac に接続するには、次の手順のようにします。

- **[ツール] > [オプション]** を選び、**[Xamarin]** の **[iOS の設定]** を選びます。

  [![[iOS の設定] 画面](images/image2.png)](images/image2.png#lightbox)

- Mac が**リモート ログイン**を許可するように正しく[構成](#configuration)されている場合、一覧で Mac を選ぶことができます。

  [![[リモート ホスト] ダイアログ ボックス](images/xma3.png)](images/xma3.png#lightbox)

- Mac ホストの管理者資格情報の入力を求められます。

  [![[ログイン] ダイアログ ボックス](images/xma4.png)](images/xma4.png#lightbox)

- 接続すると、コンピューター名の横に "接続成功" アイコンが表示されます。

  [![コンピューター名の横に "接続成功" アイコンが表示された [Remote Has]\(リモート\) ダイアログ](images/image6.png)](images/image6.png#lightbox)

Visual Studio を起動するたびに再接続されます。

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Visual Studio ツールバーの構成

iOS プロジェクトを開くと iOS ツール バーが既定で表示されるので、構成する必要はありません。

iOS ツール バーが表示されない場合は、下記の手順を使用できます。

ツール バーを構成するには、**[表示] > [ツール バー]** メニューを開き、**[iOS]** エントリが選ばれていることを確認します。 次のスクリーンショットのように、メニュー項目を選びます。ツール バーを表示するには、オンにする必要があります。

[![[ツール バー] > [iOS] を選ぶ](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

Visual Studio 2017 より前のバージョンでは、**[ソリューション プラットフォーム]** ボタンを標準ツール バーに追加することが必要な場合があります。 これにより、iOS デバイスまたは iOS シミュレーターをデバッグ時に選ぶことができます。 以下の手順で設定します。

標準ツール バーの右側にあるメニュー ボタンをクリックします。

- **[ボタンの表示/非表示]** を選びます
- **[ソリューション プラットフォーム]** を選びます

[![[ソリューション プラットフォーム] を選びます](images/image35.png)](images/image35.png#lightbox)

**標準**ツール バーと **iOS** ツール バーが次のスクリーンショットのようになります。

[![標準ツール バーと iOS ツール バーがこのスクリーンショットのようになります](images/image36.png)](images/image36.png#lightbox)

ツール バーの構成が完了すると、Xamarin iOS for Visual Studio の使用を開始できる状態になります。

## <a name="summary"></a>まとめ

この記事では、Xamarin iOS for Visual Studio をインストール、構成、使用する手順を示しました。

Windows と Mac OS X に前もって必要なツールをインストールして構成する方法を説明しました。


## <a name="related-links"></a>関連リンク

- [インストール](~/cross-platform/get-started/installation/windows.md)
- [デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)
- [Xamarin.iOS for Visual Studio の概要](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [XMA を使用する Visual Studio 環境への Mac の接続 (ビデオ)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
