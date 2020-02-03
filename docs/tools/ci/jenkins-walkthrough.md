---
title: Xamarin での Jenkins の使用
description: このドキュメントでは、Xamarin アプリケーションで継続的インテグレーションに Jenkins を使用する方法について説明します。 その中で、Jenkins のインストール、構成、および使用方法について説明します。
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 4f91e683b826657a9740de7e0b98137858130042
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725378"
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin での Jenkins の使用

_このガイドでは、Jenkins を継続的インテグレーションサーバーとしてセットアップし、Xamarin で作成されたモバイルアプリケーションのコンパイルを自動化する方法について説明します。ここでは、ソースコード管理システムに変更がコミットされた場合に、Jenkins アプリケーションを OS X にインストールし、構成し、ジョブを設定して Xamarin アプリケーションをコンパイルする方法について説明します。_

[Xamarin との継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)では、壊れたコードや互換性のないコードを早期に警告する、便利なソフトウェア開発手法として継続的インテグレーションが導入されています。 CI は、開発者が発生した課題や問題を処理し、ソフトウェアを開発のために安定した状態に保持し続けることを可能にします。 このチュートリアルでは、両方のドキュメントのコンテンツを一緒に使用する方法について説明します。

このガイドでは、OS X を実行する専用コンピューターに Jenkins をインストールし、コンピューターの起動時に自動的に実行されるように構成する方法について説明します。 Jenkins をインストールしてすぐに、Msbuild をサポートする追加のプラグインをインストールします。 Jenkins は標準で Git をサポートしています。 TFS をソース コード管理に使っている場合は、追加のプラグインとコマンドラインユーティリティもインストールする必要があります。

Jenkins を構成し、任意の必要なプラグインをインストールしたら、Xamarin.iOS と Xamarin.Android プロジェクトをコンパイルする 1 つ以上のジョブを作成します。 ジョブは、いくつかの作業の実行に必要なメタデータと手順の集まりです。 通常、ジョブは次のような構成になります。

- **ソースコード管理 (SCM)** –これは Jenkins 構成ファイル内のメタデータエントリであり、ソースコード管理への接続方法と取得するファイルに関する情報が含まれています。
- **トリガー** –トリガーは、開発者がソースコードリポジトリに変更をコミットしたときなど、特定のアクションに基づいてジョブを開始するために使用されます。
- **ビルド命令**–これは、ソースコードをコンパイルし、モバイルデバイスにインストールできるバイナリを生成するプラグインまたはスクリプトです。
- **オプションのビルドアクション**–これには、単体テストの実行、コードに対する静的分析の実行、コードの署名、他のビルド関連作業を実行するための別のジョブの開始などが含まれます。
- **通知**–ジョブは、ビルドの状態に関する何らかの通知を送信する場合があります。
- **セキュリティ**–省略可能ですが、Jenkins のセキュリティ機能も有効にすることを強くお勧めします。

このガイドは、Jenkins サーバーをこれらの各要点をカバーしてセットアップする手順について説明します。 このガイドの終わりまでに、Xamarin のモバイルプロジェクトの IPA および APK を作成するための Jenkins のインストールと構成方法に関する十分な理解が得られるでしょう。

## <a name="requirements"></a>必要条件

理想的なビルドサーバーは、アプリケーションのビルドあるいはテストが唯一の目的である専用のスタンドアロンコンピューターです。 専用のコンピューターは、他の役割 (web サーバーなど) のために要求される成果物が、ビルドを汚染しないようにします。 たとえば、ビルドサーバーが web サーバーとしても機能している場合、web サーバーは、いくつかの共通ライブラリの競合バージョンを要求する可能性があります。 この競合のため、web サーバーが正しく機能しない、または Jenkins が、ユーザーに展開するときに、動作しないビルドを作成する可能性があります。

Xamarin のモバイル アプリ向けのビルド サーバーは、開発者のワークステーションに非常によく似た設定をします。 それには、Jenkins、Visual Studio for Mac、Xamarin.iOS および Xamarin.Android をインストールする ユーザーアカウントがあります。 またすべてのコード署名証明書、プロビジョニング プロファイル、およびキーストアもインストールする必要があります。 *通常、ビルドサーバーのユーザーアカウントは、開発者アカウントとは別のものです。ビルドサーバーのユーザーアカウントを使用してログインした状態で、すべてのソフトウェア、キー、および証明書をインストールして構成する必要があります。*

次の図は、一般的な Jenkins ビルドサーバーの上記のすべての要素を示しています。

[![](jenkins-walkthrough-images/image1.png "This diagram illustrates all of these elements on a typical Jenkins build server")](jenkins-walkthrough-images/image1.png#lightbox)

iOS アプリケーションは、macOS を実行しているコンピューターでのみビルドおよび署名できます。 Mac ミニは妥当な低コストのオプションですが、OS X 10.10 (ヨーク Semite) 以上を実行できるコンピューターであれば十分です。

TFS がソースコード管理に使用されている場合は、 [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)をインストールする必要があります。 Team Explorer Everywhere は、macOS のターミナルでの TFS へのクロスプラットフォームアクセスを提供します。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins のインストール

Jenkins を使用するための最初のタスクは、インストールです。 OS X で Jenkins を動かす方法は 3 つあります。

- デーモンとして、バックグラウンドで実行する。
- Tomcat、Jetty、JBoss などのサーブレットコンテナ内で。
- ユーザーアカウントで実行される通常のプロセスとして。

ほとんどの従来の継続的インテグレーションアプリケーションは、デーモン (OS X または \*nix の場合) またはサービス (Windows の場合) としてバックグラウンドで実行されます。 これは、機能は、ビルド環境のセットアップが簡単に実行される、必要に応じて、GUI の相互作用が存在しないシナリオに適しています。 これは、GUIによる対話が要求されないシナリオに適しています。そしてその場合は、ビルド環境のセットアップを簡単に実行できます。 モバイルアプリは、キーストアと署名証明書も必要としますが、Jenkins がデーモンとして実行している場合、アクセスに問題がある可能性があります。

Jenkins は、Jenkins をインストールするための便利な方法です。 これは、Jenkins サーバーの開始と停止を簡略化する AppleScript ラッパーです。 Bash シェルで実行する代わりに、Jenkins が次のスクリーンショットで示すように Dock 内のアイコンのアプリとして動作します。

[![](jenkins-walkthrough-images/image2.png "Instead of running in a bash shell, Jenkins runs as an app with icon in the Dock, as shown in this screenshot")](jenkins-walkthrough-images/image2.png#lightbox)

Jenkins の開始または停止は、Jenkins.App の開始または停止と同じくらい簡単です。

Jenkins.App をインストールするには、以下のスクリーンショットで示されているプロジェクトのダウンロードページから、最新バージョンをダウンロードします。

[![](jenkins-walkthrough-images/image3.png "App, download the latest version from the projects download page, pictured in this screenshot")](jenkins-walkthrough-images/image3.png#lightbox)

ビルドサーバーの `/Applications` フォルダーに zip ファイルを抽出し、他の OS X アプリケーションと同じように起動します。
初めて Jenkins.App を起動する場合は、Jenkins をダウンロードすることを通知するダイアログが表示されます。

[![](jenkins-walkthrough-images/image4.png "App, it will present a dialog informing you that it will download Jenkins")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins.App が、ダウンロードを終えると、次のスクリーンショットに示すように Jenkins スタートアップをカスタマイズするかどうかを確認する別のダイアログボックスが表示されます。

[![](jenkins-walkthrough-images/image5.png "App has finished its download, it will display another dialog asking you if you would like to customize the Jenkins startup, as seen in this screenshot")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins のカスタマイズは省略可能で、アプリの開始時に毎回実行する必要はありません。Jenkins の規定の設定は、ほとんどの状況で動作します。

Jenkins のカスタマイズが必要な場合は、 **[既定値の変更]** ボタンをクリックします。 すると 2 つの連続するダイアログボックスが表示されます。 ひとつは、Java コマンドラインパラメーターを求めるもので、もうひとつは Jenkins コマンドラインパラメーターを求めるものです。 次の 2 つのスクリーンショットは、これら 2 つのダイアログボックスを表示しています。

[![](jenkins-walkthrough-images/image6.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins を実行したら、ユーザーがコンピュータへログインするたびに起動できるように、ログイン項目として設定した方が良いでしょう。 これを行うには、Dock の Jenkins アイコンを右クリックし、[オプション] を選択します。次のスクリーンショットに示すように、ログイン時に > 開きます。

[![](jenkins-walkthrough-images/image8.png "You can do this by right-clicking on the Jenkins icon in the Dock and choosing OptionsOpen at Login, as shown in this screenshot")](jenkins-walkthrough-images/image8.png#lightbox)

これにより、Jenkins.App は、コンピュータの起動時ではなく、ユーザーがログインするたびに自動的に起動することが可能になります。 OS X が起動時に自動ログインを使用しているユーザーアカウントを指定することが可能です。 **[システム環境設定]** を開き、次のスクリーンショットに示すように、 **[Users & Groups]** アイコンを選択します。

[![](jenkins-walkthrough-images/image9.png "Open the System Preferences, and select the User  Groups icon as shown in this screenshot")](jenkins-walkthrough-images/image9.png#lightbox)

**[ログインオプション]** ボタンをクリックし、ブート時に OS X がログインに使用するアカウントを選択します。

この時点で、Jenkins のインストールは完了です。 ただし、Xamarin のモバイル アプリケーションをビルドしたい場合は、一部のプラグインをインストールする必要があります。

### <a name="installing-plugins"></a>プラグインのインストール

Jenkins インストーラーが完了すると、次のスクリーンショットに示すように、Jenkins が開始され、URL http://localhost:8080で web ブラウザーが起動します。

[![](jenkins-walkthrough-images/image10.png "8080, as shown in this screenshot")](jenkins-walkthrough-images/image10.png#lightbox)

このページで、次のスクリーンショットに示すように、左上隅にあるメニューから  **Jenkins > Manage Jenkins > プラグイン**の管理 を選択します。

[![](jenkins-walkthrough-images/image11.png "From this page, select Jenkins  Manage Jenkins  Manage Plugins from the menu in the upper left hand corner")](jenkins-walkthrough-images/image11.png#lightbox)

**[Jenkins Plugin Manager]** ページが表示されます。 利用可能 タブをクリックすると、ダウンロードしてインストールできる 600 を超えるプラグインの一覧が表示されます。 これを次のスクリーンショットに示します。

[![](jenkins-walkthrough-images/image12.png "If you click on the Available tab, you will see a list of over 600 plugins that can be downloaded and installed")](jenkins-walkthrough-images/image12.png#lightbox)

600 のすべてのプラグインの中からその一部を探すためにスクロールすることは、とても面倒でエラーが発生しやすくなる可能性があります。 Jenkins は、画面の右上にフィルター検索フィールドを提供しています。 このフィルターフィールドを使用して検索すると、次のプラグインの 1 つまたは全てを見つけてインストールすることが簡単にできます。

- **Jenkins MSBuild プラグイン**–このプラグインを使用すると、Visual Studio と Visual Studio for Mac ソリューション (.sln) とプロジェクト (.csproj) をビルドできます。
- **環境インジェクタープラグイン**–これはオプションですが便利なプラグインです。このプラグインを使用すると、ジョブおよびビルドレベルで環境変数を設定できます。 このプラグインは、アプリケーションのコード署名に使用されるパスワードなどの変数の追加の保護も提供します。 *EnvInject プラグイン*として省略されることもあります。
- **Team Foundation Server プラグイン**–これは、ソースコード管理で Team Foundation Server または Team Foundation Services を使用している場合にのみ必要となる省略可能なプラグインです。

Jenkins では、追加のプラグインなしで Git がサポートされます。

すべてのプラグインをインストールしたら、Jenkins を再起動し、各プラグインのグローバル設定を構成します。 プラグインのグローバル設定を見つけるには、次のスクリーンショットに示すように、左上隅にある **[Jenkins > Manage Jenkins > 構成]** を選択します。

[![](jenkins-walkthrough-images/image13.png "The global settings for a plugin can be found by selecting Jenkins / Manage Jenkins / Configure System from the upper left hand corner")](jenkins-walkthrough-images/image13.png#lightbox)

このメニューオプションを選択すると、 **[システムの構成 (Jenkins)]** ページが表示されます。 このページには、Jenkins 自体と、グローバルプラグインの値の一部の設定を構成するセクションが含まれています。  次のスクリーン ショットは、このページの例を示しています。

[![](jenkins-walkthrough-images/image14.png "This screenshot illustrates an example of this page")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>MSBuild プラグインの構成

MSBuild プラグインは、 **/Library/Frameworks/Mono.framework/Commands/xbuild**を使用して Visual Studio for Mac ソリューションとプロジェクトファイルをコンパイルするように構成する必要があります。 下のスクリーンショットに示すように、 **[システムの構成 [Jenkins]** ] ページを下にスクロールして **[MSBuild の追加]** ボタンを表示します。

 [![](jenkins-walkthrough-images/image15.png "Scroll down the Configure System Jenkins page until the Add MSBuild button appears")](jenkins-walkthrough-images/image15.png#lightbox)

このボタンをクリックし、表示されるフォームの**MSBuild**フィールドの**名前**と**パス**を入力します。 **Msbuild のインストール名**は、意味のあるものにする必要があります。 **Msbuild へのパス**は `xbuild`のパスにする必要があります。これは通常、 **/Library/Frameworks/Mono.framework/Commands/xbuild**です。 ページの下部にある [保存] または [適用] ボタンをクリックして変更を保存すると、Jenkins を `xbuild` 使用してソリューションをコンパイルできるようになります。

#### <a name="configuring-the-tfs-plugin"></a>TFS プラグインの構成

このセクションは、ソースコード管理に TFS を使用する場合は必須です。

MacOS ワークステーションが TFS サーバーと対話できるようにするには、ワークステーションに[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)がインストールされている必要があります。 Team Explorer Everywhere は、TFS にアクセスするためのクロスプラットフォーム コマンドライン クライアントを含む Microsoft のツールのセットです。 Team Explorer Everywhere は、Microsoft からダウンロードでき、 3 つの手順でインストールすることができます。

1. ユーザーアカウントにアクセスできるディレクトリにアーカイブファイルを展開します。 たとえば、ファイルを **~/t**に解凍できます。
2. 上記の手順で展開されたファイルを保持するフォルダーを含めるシェルまたはシステムへのパスを構成します。 次に例を示します。

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Team Explorer Everywhere がインストールされていることを確認するには、ターミナルセッションを開き、`tf` コマンドを実行します。 Tf が正しく構成されている場合、ターミナル セッションで、次の出力が表示されます。

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

TFS 用のコマンドラインクライアントがインストールされたら、`tf` コマンドラインクライアントへの完全パスを使用して Jenkins を構成する必要があります。 次のスクリーンショットに示すように、 **[システムの構成 [Jenkins]** ] ページを下にスクロールして、Team Foundation Server セクションを見つけます。

[![](jenkins-walkthrough-images/image17.png "Scroll down the Configure System Jenkins page until you find the Team Foundation Server section")](jenkins-walkthrough-images/image17.png#lightbox)

`tf` コマンドの完全なパスを入力し、 **[保存]** ボタンをクリックします。

### <a name="configure-jenkins-security"></a>Jenkins セキュリティの構成

最初にインストールしたとき、Jenkins はセキュリティが無効になっています。そのため、全てのユーザーが匿名ですべての種類の job を設定して実行することができます。 このセクションでは、Jenkins ユーザー データベースを使用して認証と承認を設定するセキュリティの構成方法について説明します。

セキュリティ設定を見つけるには、次のスクリーンショットに示すように、 **[Jenkins > Manage > Jenkins]** \ (グローバルセキュリティの構成 \) を選択します。

[![](jenkins-walkthrough-images/image18.png "Security settings can be found by selecting Jenkins / Manage Jenkins / Configure Global Security")](jenkins-walkthrough-images/image18.png#lightbox)

**[グローバルセキュリティの構成]** ページで、 **[セキュリティを有効にする]** チェックボックスをオンにし、次のスクリーンショットのように**Access Control**フォームが表示されるようにします。

[![](jenkins-walkthrough-images/image19.png "On the Configure Global Security page, check the Enable Security checkbox and the Access Control form should appear, similar to this screenshot")](jenkins-walkthrough-images/image19.png#lightbox)

次のスクリーンショットに示すように、[**セキュリティ領域] セクション**で**Jenkins の [独自のユーザーデータベース**] のオプションボタンをオンにし、[**ユーザーにサインアップを許可**する] もオンになっていることを確認します。

[![](jenkins-walkthrough-images/image20.png "Toggle the radio button for Jenkins own user database in the Security Realm Section, and ensure that Allow users to sign up is also checked")](jenkins-walkthrough-images/image20.png#lightbox)

最後に、Jenkins を再起動して、新しいアカウントを作成します。 作成した最初のアカウントはルートアカウントで、このアカウントは自動的に管理者に昇格されます。 **[グローバルセキュリティの構成]** ページに戻り、 **[マトリックスベースのセキュリティ]** ラジオボタンをオンにします。 次のスクリーンショットで示すように、ルートアカウントにはフルアクセスを付与し、匿名アカウントには読み取り専用アクセスを付与した方が良いでしょう。

[![](jenkins-walkthrough-images/image21.png "The root account should be granted full access, and the anonymous account should be given read-only access")](jenkins-walkthrough-images/image21.png#lightbox)

これらの設定を保存して Jenkins を再起動すると、セキュリティが有効になります。

#### <a name="disabling-security"></a>セキュリティを無効にする

パスワードを忘れたまたは Jenkins wide ロックアウトが発生した場合は、次の手順に従ってセキュリティを無効にすることができます。

1. Jenkins を停止します。 Jenkins.app を使用している場合は、Dock の Jenkins.App アイコンを右クリックし、ポップアップメニューから終了を選択して停止できます。

    ![Dock のアプリアイコンをクリックし、ポップアップ表示されたメニューから [終了] を選択します。](jenkins-walkthrough-images/image19.png)

2. テキストエディターで **~/.jenkins/config.xml**ファイルを開きます。
3. `<usesecurity></usesecurity>` 要素の値を `true` から `false`に変更します。
4. ファイルから `<authorizationstrategy></authorizationstrategy>` と `<securityrealm></securityrealm>` の要素を削除します。
5. Jenkins を再起動します。

## <a name="setting-up-a-job"></a>ジョブの設定

最上位レベルでは、Jenkins はソフトウェアを*ジョブ*に組み込むために必要なすべてのタスクを整理します。 ジョブはそれに関連するメタデータも持っており、ソースコードの取得方法・ビルド実行頻度・ビルドに必要な任意の特別な変数・ビルド失敗時に開発者に通知する方法などのビルドに関する情報を提供します。

ジョブを作成するには、次のスクリーンショットに示すように、右上隅のメニューで **[Jenkins > 新しいジョブ]** を選択します。

![](jenkins-walkthrough-images/image22.png "Jobs are created by selecting Jenkins  New Job from the menu in the upper right hand corner")

これにより、 **[新しいジョブ [Jenkins]]** ページが表示されます。 ジョブの名前を入力し、 **[自由形式のソフトウェアプロジェクトをビルドする]** オプションボタンを選択します。 次のスクリーンショットは、この例を示しています。

![](jenkins-walkthrough-images/image23.png "Enter a name for the job, and select the Build a free-style software project radio button")

**[OK** ] ボタンをクリックすると、そのジョブの構成ページが表示されます。 これは、次のスクリーンショットのようになります。

![](jenkins-walkthrough-images/image24.png "This should resemble this screenshot")

Jenkins は、次のパスにあるハードディスク上のディレクトリにジョブを整理します。 **~/.jenkins/jobs/[ジョブ名]**

このフォルダーには、ログ、構成ファイル、コンパイルに必要なソースコードなどのすべてのファイルと成果物が含まれています。

最初のジョブを作成したら、次の 1 つ以上構成する必要があります。

- ソースコード管理システムを指定する必要があります。
- 1つ以上の*ビルドアクション*をプロジェクトに追加する必要があります。 これらは、アプリケーションをビルドするために必要な手順やタスクです。
- このジョブには、1つの*ビルドトリガー*が割り当てられている必要があります。 Jenkins は、コードを取得して最終的なプロジェクトをビルドする頻度を指定します。

### <a name="configuring-source-code-control"></a>ソースコード管理の構成

Jenkins が行う最初のタスクは、ソース コード管理システムからソース コードを取得することです。 Jenkins は、現在利用可能な一般的なソース コード管理システムの多くをサポートしています。 このセクションでは、2 つの一般的なシステムである Git と Team Foundation Server について説明します。 これらそれぞれのソース コード管理システムについては、以下のセクションで詳しく説明します。

#### <a name="using-git-for-source-code-control"></a>ソース コード管理に Git を使用する

ソースコード管理に TFS を使用している場合は、このセクションを[スキップ](#using-tfs-for-source-code-management)し、tfs を使用して次のセクションに進みます。

Jenkins は、標準で Git をサポートします。追加のプラグインは必要ありません。 Git を使用するには、次のスクリーンショットに示すように、 **git オプションボタン**をクリックし、git リポジトリの URL を入力します。

![](jenkins-walkthrough-images/image25.png "To use Git, click on the Git radio button and enter the URL for the Git repository")

変更を保存すると、Git の設定は完了です。

#### <a name="using-tfs-for-source-code-management"></a>ソース コード管理に TFS を使用する

このセクションは、TFS ユーザーにのみ該当します。

**[Team Foundation Server]** ラジオボタンをクリックすると、次のスクリーンショットのような TFS 構成セクションが表示されます。

![](jenkins-walkthrough-images/image26.png "Click on the Team Foundation Server radio button and the TFS configuration section should appear")

TFS に必要な情報を指定します。 次のスクリーンショットは、完成したフォームの例を示しています。

![](jenkins-walkthrough-images/image27.png "This screenshot shows an example of the completed form")

#### <a name="testing-the-source-code-control-configuration"></a>ソース コード管理構成のテスト

適切なソースコード管理が構成されたら、 **[保存]** をクリックして変更を保存します。 すると、次のスクリーンショットのようにジョブのホームページに戻ります。

![](jenkins-walkthrough-images/image28.png "This will return you to the home page for the job, which will resemble this screenshot")

ソースコード管理が適切に構成されていることを検証する最も簡単な方法は、ビルドアクションが 1 つも指定されていなくても、手動でビルドをトリガーすることです。 ビルドを手動で開始するには、次のスクリーンショットに示すように、ジョブのホームページの左側のメニューに **[今すぐビルド]** リンクがあります。

![](jenkins-walkthrough-images/image29.png "To start a build manually, the home page of the job has a Build Now link in the menu on the left hand side")

ビルドが開始されると、ビルド履歴ダイアログに、次のスクリーンショットのような青く点滅する円形と、プログレスバー、ビルド番号およびビルドが開始された時間が表示されます。

![](jenkins-walkthrough-images/image30.png "When a build has been started, the Build History dialog displays a flashing blue circle, a progress bar, the build number and the time that the build started")

ジョブが成功すると、青の円形が表示されます。 ジョブが失敗した場合は、赤い円形が表示されます。

ビルド部分に発生した可能性がある問題のトラブルシューティングを助けるために、Jenkins は、すべてのジョブのコンソール出力をキャプチャします。 コンソールの出力を表示するには、**ビルド履歴**でジョブをクリックし、左側のメニューの **[コンソール出力]** リンクをクリックします。 次のスクリーンショットは、**コンソールの出力**リンクと、正常に実行されたジョブからの出力の一部を示しています。

![](jenkins-walkthrough-images/image31.png "This screenshot shows the Console Output link, as well as some of the output from a successful job")

#### <a name="location-of-build-artifacts"></a>ビルド成果物の場所

Jenkins は、*ワークスペース*と呼ばれる特殊なフォルダーにソースコード全体を取得します。 このディレクトリは、以下の場所のフォルダー内にあります。

```
~/.jenkins/jobs/[JOB NAME]/workspace
```

ワークスペースへのパスは、`$WORKSPACE`という名前の環境変数に格納されます。

ジョブのランディングページに移動し、左側のメニューの **[ワークスペース]** リンクをクリックすると、Jenkins のワークスペースフォルダーを参照できます。 次のスクリーンショットは、 **HelloWorld**という名前のジョブのワークスペースの例を示しています。

![](jenkins-walkthrough-images/image32.png "This screenshot shows an example of the workspace for a job named HelloWorld")

### <a name="build-triggers"></a>トリガーの作成

Jenkins でビルドを開始するには、いくつかの方法があります。これらは*ビルドトリガー*と呼ばれます。 ビルド トリガーは、Jenkins がいつジョブを開始しプロジェクトをビルドするかを決めることができます。 一般的なビルド トリガーの 2 つは以下の通りです。

- **定期的にビルド**する–このトリガーは、Jenkins が、2時間ごと、または平日の午前0時など、指定された間隔でジョブを開始します。 ビルドは、ソース コード リポジトリに変更があったかどうかに関係なく開始されます。
- **ポーリング SCM** –このトリガーは、ソースコード管理を定期的にポーリングします。 ソース コード リポジトリに変更がコミットされた場合、Jenkins は、新しいビルドを開始します。

ポーリング SCM は、開発者がビルドを破壊するような変更をコミットした時に、すばやくフィードバックできるため、人気のあるトリガーです。 これは、最近コミットされたコードが問題の原因となったということをチームに通知でき、開発者は問題にすばやく取り組むことができます。

定期的なビルドは、テスターに配布可能なアプリケーションのバージョンの作成によく使用されます。 たとえば、QA チームのメンバーが前の週の作業をテストできるように、金曜日の夜に定期的なビルドをスケジュールする、というようなことがあります。

### <a name="compiling-a-xamarinios-applications"></a>Xamarin.iOS アプリケーションのコンパイル
Xamarin. iOS プロジェクトは、`xbuild` または `msbuild`を使用してコマンドラインでコンパイルできます。 シェル コマンドは、Jenkins を実行しているユーザー アカウントのコンテキストで実行されます。 配布用のアプリケーションを正しくパッケージ化できるように、ユーザー アカウントがプロビジョニング プロファイルへのアクセスを持っていることが重要です。 ジョブの構成ページにこのシェル コマンドを追加することができます。

**[ビルド]** セクションまで下にスクロールします。 次のスクリーンショットに示すように、 **[ビルドステップの追加]** ボタンをクリックし、 **[シェルの実行]** を選択します。

![](jenkins-walkthrough-images/image33.png "Click the Add build step button and select Execute shell")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin Android プロジェクトのビルド

Xamarin.Android プロジェクトのビルドは、Xamarin.iOS プロジェクトをビルドする概念によく似ています。 Xamarin.Android プロジェクトから APK を作成するには、次の 2 つの手順を実行して Jenkins を構成する必要があります。

- MSBuild プラグインを使用して、プロジェクトをコンパイルします。
- 有効なリリース用のキーストアを使って APK を署名し zipalign します。

これらの2つの手順については、次の2つのセクションで詳しく説明します。

### <a name="creating-the-apk"></a>APK の作成

次のスクリーンショットに示すように、 **[ビルドステップの追加]** ボタンをクリックし、 **[MSBuild を使用して Visual Studio プロジェクトまたはソリューションをビルドする]** を選択します。

![](jenkins-walkthrough-images/image36.png "Creating the APK  Click on the Add build step button, and select Build a Visual Studio project or solution using MSBuild")

ビルド ステップがプロジェクトに追加されたら、出現したフォーム フィールドに入力します。 次のスクリーンショットは、完成したフォームの 1 つの例を示します。

![](jenkins-walkthrough-images/image37.png "Once the build step is added to the project, fill in the form fields that appear")

このビルドステップは、 **$WORKSPACE**フォルダーで `xbuild` を実行します。 MSBuild ビルドファイルは、 **Xamarin. .csproj**ファイルに設定されます。 **コマンドライン引数**は、ターゲット**packageforandroid**のリリースビルドを指定します。 この手順の成果物は、次の場所にある APK になります。

```
$WORKSPACE/[PROJECT NAME]/bin/Release
```

次のスクリーンショットは、この APK の例を示しています。

![](jenkins-walkthrough-images/image38.png "This screenshot shows an example of this APK")

この APK は、秘密キーストアで署名されておらず、zipalign する必要があるので、まだデプロイできません。

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>リリース用の APK に署名して Zipalign する

APK の署名と zipalign は、技術的に独立した 2 つのタスクで、Android SDK の 2 つの個別のコマンド ライン ツールによって実行されます。 ただし、1 つのビルド アクションで実行すると便利です。 APK の署名と zipalign の詳細については、リリース用の Android アプリケーションの準備に関する Xamarin のドキュメントを参照してください。

これら 2 つのコマンドは、プロジェクト間で異なる コマンド ライン パラメーターが必要です。 さらに、これらのコマンド ライン パラメーターの一部は、ビルド実行時にはコンソール出力に表示されないパスワードです。 環境変数にこれらのコマンド ライン パラメーターの一部を保存します。 署名または zipalign に必要な環境変数は、以下の表で説明します。

|環境変数|[説明]|
|--- |--- |
|KEYSTORE_FILE|APK に署名するためのキーストアへのパスです。|
|KEYSTORE_ALIAS|APK の署名に使用されるキーストアのキーです。|
|INPUT_APK|`xbuild`によって作成された APK。|
|SIGNED_APK|`jarsigner`によって生成された符号付き APK。|
|FINAL_APK|これは、`zipalign`によって生成される zip アラインメントされた APK です。|
|STORE_PASS|ファイルを署名するために、キーストアの中へのアクセスに使用されるパスワードです。|

必要条件のセクションで述べたように、これらの環境変数は、EnvInject プラグインを使用して、ビルド時に展開することができます。 ジョブは、次のスクリーンショットに示すように、環境変数のインジェクトに基づいて新しいビルド ステップを追加する必要があります。

![](jenkins-walkthrough-images/image39.png "The job should have a new build step added based on the Inject environment variables")

表示される [**プロパティのコンテンツ**フォーム] フィールドでは、環境変数が1行に1つずつ、次の形式で追加されます。

```
ENVIRONMENT_VARIABLE_NAME = value
```

次のスクリーンショットは、APK の署名に必要な環境変数を示しています。

![](jenkins-walkthrough-images/image40.png "This screenshot shows the environment variables that are required for signing the APK")

APK ファイルの環境変数の一部が `WORKSPACE` 環境変数に基づいて構築されていることに注意してください。

最終的な環境変数は、キーストアの内容にアクセスするためのパスワードです: `STORE_PASS`。 パスワードは、機密性の高い値であり、ログファイルでは隠したり省略したりする必要があります。 EnvInject プラグインは、これらをログに表示しないように、これらの値を保護するように構成できます。

ジョブ構成の **[ビルド]** セクションの直前に、 **[ビルド環境]** セクションがあります。 [**パスワードを挿入**する] チェックボックスをオンにすると、一部のフォームフィールドが表示されます。 これらのフォーム フィールドは、環境変数の値と名前を保存するために使用されます。 次のスクリーンショットは、`STORE_PASS` 環境変数を追加する例を示しています。

![](jenkins-walkthrough-images/image41.png "This screenshot is an example of adding the STOREPASS environment variable")

環境変数の初期設定が終わったら、次は、APK を署名して zipalign するためのビルド ステップを追加する手順に移ります。 環境変数を挿入するビルド手順は、`jarsigner` と `zipalign`を実行する別の**実行シェル**コマンドビルドになります。 各コマンドには、次のスニペットに示すように、1行ずつ移動します。

```
jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
zipalign -f -v 4 $SIGNED_APK $FINAL_APK
```

次のスクリーンショットは、`jarsigner` と `zipalign` のコマンドをステップに入力する方法の例を示しています。

![](jenkins-walkthrough-images/image42.png "This screenshot shows an example of how to enter the jarsigner and zipalign commands into the step")

すべてのビルド アクションが準備できたら、すべての動作を確認するために手動でビルドをトリガーすることをお勧めします。 ビルドが失敗した場合、ビルドが失敗した原因に関する情報について**コンソールの出力**を確認する必要があります。

### <a name="submitting-tests-to-test-cloud"></a>Test Cloud へテストを送信する

自動テストは、シェル コマンドを使用して Test Cloud に送信することができます。 Xamarin Test Cloud でのテストの実行の設定の詳細については、「 [Xamarin Android アプリを準備](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)する」と「 [xamarin IOS アプリを準備](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)する」を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、macOS でビルドサーバーとして Jenkins を導入し、Xamarin モバイルアプリケーションをリリース用にコンパイルおよび準備するように構成しました。 ビルドプロセスをサポートするために、複数のプラグインと共に macOS コンピューターに Jenkins をインストールしました。 TFS または Git のいずれかからコードを取得し、そのコードをリリースの準備ができているアプリケーションへコンパイルするというジョブを作成し構成しました。 また、ジョブを実行する時間をスケジュールする 2 つの異なる方法について調査しました。

## <a name="related-links"></a>関連リンク

- [継続的インテグレーション](~/tools/ci/index.md)
- [App Center テスト](https://docs.microsoft.com/appcenter/test-cloud/)
