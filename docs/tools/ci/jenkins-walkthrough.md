---
title: Xamarin での Jenkins の使用
description: このドキュメントでは、Xamarin アプリケーションとの継続的インテグレーションに Jenkins を使用する方法について説明します。 Jenkins をインストール、構成、および使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 6d420faf59d940bb111b5ecd326a29083cab012e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029915"
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin での Jenkins の使用

_このガイドでは、Jenkins を継続的インテグレーションサーバーとしてセットアップし、Xamarin で作成されたモバイルアプリケーションのコンパイルを自動化する方法について説明します。ここでは、ソースコード管理システムに変更がコミットされた場合に、Jenkins アプリケーションを OS X にインストールし、構成し、ジョブを設定して Xamarin アプリケーションをコンパイルする方法について説明します。_

[Xamarin との継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)では、壊れたコードや互換性のないコードを早期に警告する、便利なソフトウェア開発手法として継続的インテグレーションが導入されています。 CI を使用すると、開発者は問題や問題に対処し、ソフトウェアを展開に適した状態に保つことができます。 このチュートリアルでは、両方のドキュメントのコンテンツを一緒に使用する方法について説明します。

このガイドでは、OS X を実行する専用コンピューターに Jenkins をインストールし、コンピューターの起動時に自動的に実行されるように構成する方法について説明します。 Jenkins のインストールが完了したら、MS ビルドをサポートするために追加のプラグインをインストールします。 Jenkins は、すぐに使える Git をサポートしています。 TFS がソースコード管理に使用されている場合は、追加のプラグインとコマンドラインユーティリティもインストールする必要があります。

Jenkins を構成し、必要なプラグインがインストールされたら、1つまたは複数のジョブを作成して Xamarin. Android プロジェクトと Xamarin. iOS プロジェクトをコンパイルします。 ジョブとは、いくつかの作業を実行するために必要な手順とメタデータのコレクションです。 ジョブは、通常、次の要素で構成されます。

- **ソースコード管理 (SCM)** –これは Jenkins 構成ファイル内のメタデータエントリであり、ソースコード管理への接続方法と取得するファイルに関する情報が含まれています。
- **トリガー** –トリガーは、開発者がソースコードリポジトリに変更をコミットしたときなど、特定のアクションに基づいてジョブを開始するために使用されます。
- **ビルド命令**–これは、ソースコードをコンパイルし、モバイルデバイスにインストールできるバイナリを生成するプラグインまたはスクリプトです。
- **オプションのビルドアクション**–これには、単体テストの実行、コードに対する静的分析の実行、コードの署名、他のビルド関連作業を実行するための別のジョブの開始などが含まれます。
- **通知**–ジョブは、ビルドの状態に関する何らかの通知を送信する場合があります。
- **セキュリティ**–省略可能ですが、Jenkins のセキュリティ機能も有効にすることを強くお勧めします。

このガイドでは、これらの各ポイントをカバーする Jenkins サーバーをセットアップする方法について説明します。 最終的には、Jenkins を設定して構成し、Xamarin モバイルプロジェクト用に IPA と APK を作成する方法をよく理解しておく必要があります。

## <a name="requirements"></a>［要件］

理想的なビルドサーバーは、アプリケーションをビルドしてテストするための専用のスタンドアロンコンピューターです。 専用のコンピューターを使用すると、他の役割 (web サーバーなど) に必要な成果物がビルドを汚染しないようにすることができます。 たとえば、ビルドサーバーが web サーバーとしても機能している場合は、web サーバーで共通ライブラリの競合するバージョンが必要になることがあります。 この競合により、web サーバーが正常に機能しないか、Jenkins がユーザーに配置されたときに動作しないビルドを作成する可能性があります。

Xamarin mobile apps のビルドサーバーは、開発者のワークステーションとほぼ同じように設定されます。 これには、Jenkins、Visual Studio for Mac、および Xamarin のユーザーアカウントがあります。 iOS と Xamarin Android がインストールされます。 すべてのコード署名証明書、プロビジョニングプロファイル、およびキーストアもインストールする必要があります。 *通常、ビルドサーバーのユーザーアカウントは、開発者アカウントとは別のものです。ビルドサーバーのユーザーアカウントを使用してログインした状態で、すべてのソフトウェア、キー、および証明書をインストールして構成する必要があります。*

次の図は、一般的な Jenkins ビルドサーバー上のこれらのすべての要素を示しています。

[![](jenkins-walkthrough-images/image1.png "This diagram illustrates all of these elements on a typical Jenkins build server")](jenkins-walkthrough-images/image1.png#lightbox)

iOS アプリケーションは、macOS を実行しているコンピューターでのみビルドおよび署名できます。 Mac ミニは妥当な低コストのオプションですが、OS X 10.10 (ヨーク Semite) 以上を実行できるコンピューターであれば十分です。

TFS がソースコード管理に使用されている場合は、 [Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)をインストールする必要があります。 Team Explorer Everywhere は、macOS のターミナルでの TFS へのクロスプラットフォームアクセスを提供します。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins のインストール

Jenkins を使用する最初のタスクは、インストールです。 OS X で Jenkins を実行するには、次の3つの方法があります。

- デーモンとして、バックグラウンドで実行されます。
- Tomcat、Jetty、JBoss などのサーブレットコンテナー内。
- ユーザーアカウントで実行される通常のプロセスとして。

ほとんどの従来の継続的インテグレーションアプリケーションは、デーモン (OS X または \*nix の場合) またはサービス (Windows の場合) としてバックグラウンドで実行されます。 これは、GUI による操作が不要で、ビルド環境のセットアップを簡単に実行できる場合に適しています。 また、Jenkins がデーモンとして実行されている場合に、アクセスに問題がある可能性があるキーストアと署名証明書も必要です。 これらの問題のため、このドキュメントでは、ビルドサーバーのユーザーアカウントで Jenkins を実行する3番目のシナリオに焦点を当てています。

Jenkins は、Jenkins をインストールするための便利な方法です。 これは、Jenkins サーバーの開始と停止を簡略化する、AppleScript ラッパーです。 Jenkins は、次のスクリーンショットに示すように、bash シェルで実行するのではなく、Dock のアイコンを使用してアプリとして実行されます。

[![](jenkins-walkthrough-images/image2.png "Instead of running in a bash shell, Jenkins runs as an app with icon in the Dock, as shown in this screenshot")](jenkins-walkthrough-images/image2.png#lightbox)

Jenkins の開始または停止は、Jenkins を開始または停止するのと同じように簡単です。

Jenkins をインストールするには、次のスクリーンショットに示されているように、プロジェクトのダウンロードページから最新バージョンをダウンロードします。

[![](jenkins-walkthrough-images/image3.png "App, download the latest version from the projects download page, pictured in this screenshot")](jenkins-walkthrough-images/image3.png#lightbox)

ビルドサーバーの `/Applications` フォルダーに zip ファイルを抽出し、他の OS X アプリケーションと同じように起動します。
Jenkins を初めて起動するときに、Jenkins をダウンロードすることを通知するダイアログが表示されます。

[![](jenkins-walkthrough-images/image4.png "App, it will present a dialog informing you that it will download Jenkins")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins がダウンロードを完了すると、次のスクリーンショットに示すように、Jenkins スタートアップをカスタマイズするかどうかを確認する別のダイアログが表示されます。

[![](jenkins-walkthrough-images/image5.png "App has finished its download, it will display another dialog asking you if you would like to customize the Jenkins startup, as seen in this screenshot")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins のカスタマイズはオプションであり、アプリを起動するたびに実行する必要はありません。 Jenkins の既定の設定は、ほとんどの状況で機能します。

Jenkins のカスタマイズが必要な場合は、 **[既定値の変更]** ボタンをクリックします。 これにより、2つの連続したダイアログが表示されます。1つは Java コマンドラインパラメーターを要求し、もう1つは Jenkins コマンドラインパラメーターを要求します。 次の2つのスクリーンショットは、これらの2つのダイアログを示しています。

[![](jenkins-walkthrough-images/image6.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image6.png#lightbox)

[![](jenkins-walkthrough-images/image7.png "This screenshot shows the dialogs")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins が実行されたら、ログイン項目として設定して、ユーザーがコンピューターにログインするたびに起動されるようにすることができます。 これを行うには、Dock の Jenkins アイコンを右クリックし、[オプション] を選択します。次のスクリーンショットに示すように、ログイン時に > 開きます。

[![](jenkins-walkthrough-images/image8.png "You can do this by right-clicking on the Jenkins icon in the Dock and choosing OptionsOpen at Login, as shown in this screenshot")](jenkins-walkthrough-images/image8.png#lightbox)

これにより、ユーザーがログインするたびに Jenkins が自動的に起動しますが、コンピューターが起動するときには自動的に起動しません。 ブート時にで自動的にログインするために OS X で使用されるユーザーアカウントを指定することができます。 **[システム環境設定]** を開き、次のスクリーンショットに示すように、 **[Users & Groups]** アイコンを選択します。

[![](jenkins-walkthrough-images/image9.png "Open the System Preferences, and select the User  Groups icon as shown in this screenshot")](jenkins-walkthrough-images/image9.png#lightbox)

**[ログインオプション]** ボタンをクリックし、ブート時に OS X がログインに使用するアカウントを選択します。

この時点で、Jenkins がインストールされています。 ただし、Xamarin モバイルアプリケーションをビルドする場合は、プラグインをいくつかインストールする必要があります。

### <a name="installing-plugins"></a>プラグインのインストール

Jenkins インストーラーが完了すると、次のスクリーンショットに示すように、Jenkins が開始され、URL http://localhost:8080 で web ブラウザーが起動します。

[![](jenkins-walkthrough-images/image10.png "8080, as shown in this screenshot")](jenkins-walkthrough-images/image10.png#lightbox)

このページで、次のスクリーンショットに示すように、左上隅にあるメニューから  **Jenkins > Manage Jenkins > プラグイン**の管理 を選択します。

[![](jenkins-walkthrough-images/image11.png "From this page, select Jenkins  Manage Jenkins  Manage Plugins from the menu in the upper left hand corner")](jenkins-walkthrough-images/image11.png#lightbox)

**[Jenkins Plugin Manager]** ページが表示されます。 [使用可能] タブをクリックすると、ダウンロードしてインストールできる600以上のプラグインの一覧が表示されます。 これを次のスクリーンショットに示します。

[![](jenkins-walkthrough-images/image12.png "If you click on the Available tab, you will see a list of over 600 plugins that can be downloaded and installed")](jenkins-walkthrough-images/image12.png#lightbox)

すべての600プラグインをスクロールしていくつかを見つけると、面倒でエラーが発生しやすくなります。 Jenkins では、インターフェイスの右上隅にフィルター検索フィールドが用意されています。 検索にこのフィルターフィールドを使用すると、次のプラグインの1つまたはすべてを簡単に見つけてインストールできます。

- **Jenkins MSBuild プラグイン**–このプラグインを使用すると、Visual Studio と Visual Studio for Mac ソリューション (.sln) とプロジェクト (.csproj) をビルドできます。
- **環境インジェクタープラグイン**–これはオプションですが便利なプラグインです。このプラグインを使用すると、ジョブおよびビルドレベルで環境変数を設定できます。 また、アプリケーションのコード署名に使用されるパスワードなど、変数に対する追加の保護も提供します。 *EnvInject プラグイン*として省略されることもあります。
- **Team Foundation Server プラグイン**–これは、ソースコード管理で Team Foundation Server または Team Foundation Services を使用している場合にのみ必要となる省略可能なプラグインです。

Jenkins は、追加のプラグインを使用せずに Git をサポートしています。

すべてのプラグインをインストールしたら、Jenkins を再起動し、各プラグインのグローバル設定を構成します。 プラグインのグローバル設定を見つけるには、次のスクリーンショットに示すように、左上隅にある **[Jenkins > Manage Jenkins > 構成]** を選択します。

[![](jenkins-walkthrough-images/image13.png "The global settings for a plugin can be found by selecting Jenkins / Manage Jenkins / Configure System from the upper left hand corner")](jenkins-walkthrough-images/image13.png#lightbox)

このメニューオプションを選択すると、 **[システムの構成 (Jenkins)]** ページが表示されます。 このページには、Jenkins 自体を構成し、いくつかのグローバルプラグイン値を設定するためのセクションが含まれています。  次のスクリーンショットは、このページの例を示しています。

[![](jenkins-walkthrough-images/image14.png "This screenshot illustrates an example of this page")](jenkins-walkthrough-images/image14.png#lightbox)

#### <a name="configuring-the-msbuild-plugin"></a>MSBuild プラグインの構成

MSBuild プラグインは、 **/Library/Frameworks/Mono.framework/Commands/xbuild**を使用して Visual Studio for Mac ソリューションとプロジェクトファイルをコンパイルするように構成する必要があります。 下のスクリーンショットに示すように、 **[システムの構成 [Jenkins]** ] ページを下にスクロールして **[MSBuild の追加]** ボタンを表示します。

 [![](jenkins-walkthrough-images/image15.png "Scroll down the Configure System Jenkins page until the Add MSBuild button appears")](jenkins-walkthrough-images/image15.png#lightbox)

このボタンをクリックし、表示されるフォームの**MSBuild**フィールドの**名前**と**パス**を入力します。 **Msbuild のインストール名**は、意味のあるものにする必要があります。 **Msbuild へのパス**は `xbuild`のパスにする必要があります。これは通常、 **/Library/Frameworks/Mono.framework/Commands/xbuild**です。 ページの下部にある [保存] または [適用] ボタンをクリックして変更を保存すると、Jenkins を `xbuild` 使用してソリューションをコンパイルできるようになります。

#### <a name="configuring-the-tfs-plugin"></a>TFS プラグインの構成

ソースコード管理に TFS を使用する場合は、このセクションは必須です。

MacOS ワークステーションが TFS サーバーと対話できるようにするには、ワークステーションに[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)がインストールされている必要があります。 Team Explorer Everywhere は、TFS にアクセスするためのクロスプラットフォームコマンドラインクライアントを含む、Microsoft の一連のツールです。 Team Explorer Everywhere は、Microsoft からダウンロードし、次の3つの手順でインストールできます。

1. ユーザーアカウントがアクセスできるディレクトリにアーカイブファイルを解凍します。 たとえば、ファイルを **~/t**に解凍できます。
2. 上記の手順1で解凍したファイルを保持するフォルダーを含めるように、シェルまたはシステムパスを構成します。 たとえば、オブジェクトに適用された

    ```
    echo export PATH~/tee/:$PATH' >> ~/.bash_profile
    ```

3. Team Explorer Everywhere がインストールされていることを確認するには、ターミナルセッションを開き、`tf` コマンドを実行します。 Tf が適切に構成されている場合は、ターミナルセッションで次の出力が表示されます。

    ```
    $ tf
    Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

    Available commands and their options:
    ```

TFS 用のコマンドラインクライアントがインストールされたら、`tf` コマンドラインクライアントへの完全パスを使用して Jenkins を構成する必要があります。 次のスクリーンショットに示すように、 **[システムの構成 [Jenkins]** ] ページを下にスクロールして、Team Foundation Server セクションを見つけます。

[![](jenkins-walkthrough-images/image17.png "Scroll down the Configure System Jenkins page until you find the Team Foundation Server section")](jenkins-walkthrough-images/image17.png#lightbox)

`tf` コマンドの完全なパスを入力し、 **[保存]** ボタンをクリックします。

### <a name="configure-jenkins-security"></a>Jenkins のセキュリティを構成する

Jenkins では、最初のインストール時にセキュリティが無効になっているため、任意のユーザーが任意の種類のジョブを匿名で設定して実行できます。 このセクションでは、認証と承認を構成するために Jenkins user データベースを使用してセキュリティを構成する方法について説明します。

セキュリティ設定を見つけるには、次のスクリーンショットに示すように、 **[Jenkins > Manage > Jenkins]** \ (グローバルセキュリティの構成 \) を選択します。

[![](jenkins-walkthrough-images/image18.png "Security settings can be found by selecting Jenkins / Manage Jenkins / Configure Global Security")](jenkins-walkthrough-images/image18.png#lightbox)

**[グローバルセキュリティの構成]** ページで、 **[セキュリティを有効にする]** チェックボックスをオンにし、次のスクリーンショットのように**Access Control**フォームが表示されるようにします。

[![](jenkins-walkthrough-images/image19.png "On the Configure Global Security page, check the Enable Security checkbox and the Access Control form should appear, similar to this screenshot")](jenkins-walkthrough-images/image19.png#lightbox)

次のスクリーンショットに示すように、[**セキュリティ領域] セクション**で**Jenkins の [独自のユーザーデータベース**] のオプションボタンをオンにし、[**ユーザーにサインアップを許可**する] もオンになっていることを確認します。

[![](jenkins-walkthrough-images/image20.png "Toggle the radio button for Jenkins own user database in the Security Realm Section, and ensure that Allow users to sign up is also checked")](jenkins-walkthrough-images/image20.png#lightbox)

最後に、Jenkins を再起動して、新しいアカウントを作成します。 最初に作成されるアカウントはルートアカウントで、このアカウントは自動的に管理者に昇格されます。 **[グローバルセキュリティの構成]** ページに戻り、 **[マトリックスベースのセキュリティ]** ラジオボタンをオンにします。 次のスクリーンショットに示すように、ルートアカウントにはフルアクセスが許可され、匿名アカウントには読み取り専用アクセス権が付与されている必要があります。

[![](jenkins-walkthrough-images/image21.png "The root account should be granted full access, and the anonymous account should be given read-only access")](jenkins-walkthrough-images/image21.png#lightbox)

これらの設定が保存され、Jenkins が再起動されると、セキュリティが有効になります。

#### <a name="disabling-security"></a>セキュリティの無効化

忘れたパスワードや Jenkins のロックアウトが発生した場合は、次の手順に従ってセキュリティを無効にすることができます。

1. Jenkins を停止します。 Jenkins を使用している場合は、Dock の Jenkins アイコンを右クリックし、ポップアップ表示されたメニューから [終了] を選択すると、この操作を行うことができます。

    ![Dock のアプリアイコンをクリックし、ポップアップ表示されたメニューから [終了] を選択します。](jenkins-walkthrough-images/image19.png)

2. テキストエディターで **~/.jenkins/config.xml**ファイルを開きます。
3. `<usesecurity></usesecurity>` 要素の値を `true` から `false`に変更します。
4. ファイルから `<authorizationstrategy></authorizationstrategy>` と `<securityrealm></securityrealm>` の要素を削除します。
5. Jenkins を再起動します。

## <a name="setting-up-a-job"></a>ジョブの設定

最上位レベルでは、Jenkins はソフトウェアを*ジョブ*に組み込むために必要なすべてのタスクを整理します。 また、ジョブにはメタデータが関連付けられています。これにより、ソースコードの取得方法、ビルドの実行頻度、ビルドに必要な特別な変数、ビルドが失敗した場合に開発者に通知する方法など、ビルドに関する情報が提供されます。

ジョブを作成するには、次のスクリーンショットに示すように、右上隅のメニューで **[Jenkins > 新しいジョブ]** を選択します。

![](jenkins-walkthrough-images/image22.png "Jobs are created by selecting Jenkins  New Job from the menu in the upper right hand corner")

これにより、 **[新しいジョブ [Jenkins]]** ページが表示されます。 ジョブの名前を入力し、 **[自由形式のソフトウェアプロジェクトをビルドする]** オプションボタンを選択します。 次のスクリーンショットは、この例を示しています。

![](jenkins-walkthrough-images/image23.png "Enter a name for the job, and select the Build a free-style software project radio button")

**[OK** ] ボタンをクリックすると、そのジョブの構成ページが表示されます。 これは、次のスクリーンショットのようになります。

![](jenkins-walkthrough-images/image24.png "This should resemble this screenshot")

Jenkins は、次のパスにあるハードディスク上のディレクトリにジョブを整理します。 **~/.jenkins/jobs/[ジョブ名]**

このフォルダーには、ログ、構成ファイル、コンパイルが必要なソースコードなど、ジョブに固有のすべてのファイルとアイテムが含まれています。

最初のジョブが作成されたら、次の1つまたは複数を使用して構成する必要があります。

- ソースコード管理システムを指定する必要があります。
- 1つ以上の*ビルドアクション*をプロジェクトに追加する必要があります。 これらは、アプリケーションをビルドするために必要な手順またはタスクです。
- このジョブには、1つの*ビルドトリガー*が割り当てられている必要があります。 Jenkins は、コードを取得して最終的なプロジェクトをビルドする頻度を指定します。

### <a name="configuring-source-code-control"></a>ソースコード管理の構成

最初のタスク Jenkins は、ソースコード管理システムからソースコードを取得します。 Jenkins では、現在利用できる多くの一般的なソースコード管理システムがサポートされています。 このセクションでは、2つの一般的なシステムである Git と Team Foundation Server について説明します。 これらの各ソースコード管理システムについては、以下のセクションで詳しく説明します。

#### <a name="using-git-for-source-code-control"></a>ソースコード管理に Git を使用する

ソースコード管理に TFS を使用している場合は、このセクションを[スキップ](#using-tfs-for-source-code-management)し、tfs を使用して次のセクションに進みます。

Jenkins は、すぐに使える Git をサポートしています。余分なプラグインは必要ありません。 Git を使用するには、次のスクリーンショットに示すように、 **git オプションボタン**をクリックし、git リポジトリの URL を入力します。

![](jenkins-walkthrough-images/image25.png "To use Git, click on the Git radio button and enter the URL for the Git repository")

変更が保存されると、Git の構成が完了します。

#### <a name="using-tfs-for-source-code-management"></a>ソースコード管理における TFS の使用

このセクションは TFS ユーザーにのみ適用されます。

**[Team Foundation Server]** ラジオボタンをクリックすると、次のスクリーンショットのような TFS 構成セクションが表示されます。

![](jenkins-walkthrough-images/image26.png "Click on the Team Foundation Server radio button and the TFS configuration section should appear")

TFS に必要な情報を指定します。 次のスクリーンショットは、完成したフォームの例を示しています。

![](jenkins-walkthrough-images/image27.png "This screenshot shows an example of the completed form")

#### <a name="testing-the-source-code-control-configuration"></a>ソースコード管理の構成のテスト

適切なソースコード管理が構成されたら、 **[保存]** をクリックして変更を保存します。 これにより、ジョブのホームページに戻ります。次のスクリーンショットのようになります。

![](jenkins-walkthrough-images/image28.png "This will return you to the home page for the job, which will resemble this screenshot")

ソースコード管理が適切に構成されていることを検証する最も簡単な方法は、ビルドアクションが指定されていない場合でも、手動でビルドをトリガーすることです。 ビルドを手動で開始するには、次のスクリーンショットに示すように、ジョブのホームページの左側のメニューに **[今すぐビルド]** リンクがあります。

![](jenkins-walkthrough-images/image29.png "To start a build manually, the home page of the job has a Build Now link in the menu on the left hand side")

ビルドが開始されると、[ビルド履歴] ダイアログには、次のスクリーンショットのように、点滅した青い円、進行状況バー、ビルド番号、ビルドの開始時刻が表示されます。

![](jenkins-walkthrough-images/image30.png "When a build has been started, the Build History dialog displays a flashing blue circle, a progress bar, the build number and the time that the build started")

ジョブが成功すると、青い円が表示されます。 ジョブが失敗した場合は、赤い円が表示されます。

ビルドの一部として発生する可能性のある問題のトラブルシューティングに役立つように、Jenkins はジョブのすべてのコンソール出力をキャプチャします。 コンソールの出力を表示するには、**ビルド履歴**でジョブをクリックし、左側のメニューの **[コンソール出力]** リンクをクリックします。 次のスクリーンショットは、**コンソールの出力**リンクと、正常に実行されたジョブからの出力の一部を示しています。

![](jenkins-walkthrough-images/image31.png "This screenshot shows the Console Output link, as well as some of the output from a successful job")

#### <a name="location-of-build-artifacts"></a>ビルド成果物の場所

Jenkins は、*ワークスペース*と呼ばれる特殊なフォルダーにソースコード全体を取得します。 このディレクトリは、次の場所にあるフォルダー内にあります。

```
~/.jenkins/jobs/[JOB NAME]/workspace
```

ワークスペースへのパスは、`$WORKSPACE`という名前の環境変数に格納されます。

ジョブのランディングページに移動し、左側のメニューの **[ワークスペース]** リンクをクリックすると、Jenkins のワークスペースフォルダーを参照できます。 次のスクリーンショットは、 **HelloWorld**という名前のジョブのワークスペースの例を示しています。

![](jenkins-walkthrough-images/image32.png "This screenshot shows an example of the workspace for a job named HelloWorld")

### <a name="build-triggers"></a>トリガーのビルド

Jenkins でビルドを開始するには、いくつかの方法があります。これらは*ビルドトリガー*と呼ばれます。 ビルドトリガーを使用すると、いつジョブを開始し、プロジェクトをビルドするかを Jenkins が判断できます。 次の2つの一般的なビルドトリガーがあります。

- **定期的にビルド**する–このトリガーは、Jenkins が、2時間ごと、または平日の午前0時など、指定された間隔でジョブを開始します。 ソースコードリポジトリに変更があったかどうかに関係なく、ビルドが開始されます。
- **ポーリング SCM** –このトリガーは、ソースコード管理を定期的にポーリングします。 ソースコードリポジトリに変更がコミットされている場合は、Jenkins によって新しいビルドが開始されます。

SCM のポーリングは、開発者がビルドを中断する変更をコミットするときに迅速なフィードバックを提供するため、一般的なトリガーです。 これは、最近コミットされたコードによって問題が発生していることを警告するチームにとって役立ちます。また、変更が引き続き最新であるという問題に対処することができます。

定期的なビルドは、テスト担当者に配布できるアプリケーションのバージョンを作成するためによく使用されます。 たとえば、QA チームのメンバーが前の週の作業をテストできるように、定期的なビルドが金曜日の夜にスケジュールされている場合があります。

### <a name="compiling-a-xamarinios-applications"></a>Xamarin. iOS アプリケーションのコンパイル
Xamarin. iOS プロジェクトは、`xbuild` または `msbuild`を使用してコマンドラインでコンパイルできます。 Shell コマンドは、Jenkins を実行しているユーザーアカウントのコンテキストで実行されます。 アプリケーションを配布用に適切にパッケージ化できるように、ユーザーアカウントがプロビジョニングプロファイルにアクセスできることが重要です。 このシェルコマンドを [ジョブの構成] ページに追加することができます。

**[ビルド]** セクションまで下にスクロールします。 次のスクリーンショットに示すように、 **[ビルドステップの追加]** ボタンをクリックし、 **[シェルの実行]** を選択します。

![](jenkins-walkthrough-images/image33.png "Click the Add build step button and select Execute shell")

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin Android プロジェクトのビルド

Xamarin の Android プロジェクトのビルドは、Xamarin の iOS プロジェクトを構築する概念とよく似ています。 Xamarin Android プロジェクトから APK を作成するには、次の2つの手順を実行するように Jenkins を構成する必要があります。

- MSBuild プラグインを使用してプロジェクトをコンパイルする
- APK に署名して、有効なリリースキーストアに配置します。

これらの2つの手順については、次の2つのセクションで詳しく説明します。

### <a name="creating-the-apk"></a>APK の作成

次のスクリーンショットに示すように、 **[ビルドステップの追加]** ボタンをクリックし、 **[MSBuild を使用して Visual Studio プロジェクトまたはソリューションをビルドする]** を選択します。

![](jenkins-walkthrough-images/image36.png "Creating the APK  Click on the Add build step button, and select Build a Visual Studio project or solution using MSBuild")

ビルドステップがプロジェクトに追加されたら、表示されるフォームフィールドに入力します。 次のスクリーンショットは、完成したフォームの一例です。

![](jenkins-walkthrough-images/image37.png "Once the build step is added to the project, fill in the form fields that appear")

このビルドステップは、 **$WORKSPACE**フォルダーで `xbuild` を実行します。 MSBuild ビルドファイルは、 **Xamarin. .csproj**ファイルに設定されます。 **コマンドライン引数**は、ターゲット**packageforandroid**のリリースビルドを指定します。 この手順の成果物は、次の場所にある APK になります。

```
$WORKSPACE/[PROJECT NAME]/bin/Release
```

次のスクリーンショットは、この APK の例を示しています。

![](jenkins-walkthrough-images/image38.png "This screenshot shows an example of this APK")

この APK は、プライベートキーストアで署名されていないため、zip で配置する必要があるため、デプロイの準備ができていません。

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>リリース用に APK に署名して Zipaligning インする

Apk の署名と zipalign は、技術的には2つの異なるコマンドラインツールによって Android SDK から実行される、技術的には別のタスクです。 ただし、1つのビルドアクションで実行すると便利です。 Apk の署名と zipalign の詳細については、リリース用の Android アプリケーションの準備に関する Xamarin のドキュメントを参照してください。

これらのコマンドはどちらも、プロジェクトごとに異なる可能性のあるコマンドラインパラメーターを必要とします。 また、これらのコマンドラインパラメーターの一部は、ビルドの実行時にコンソール出力に表示されないパスワードです。 これらのコマンドラインパラメーターの一部を環境変数に格納します。 署名や郵便の調整に必要な環境変数については、次の表で説明します。

|環境変数|説明|
|--- |--- |
|KEYSTORE_FILE|これは、APK に署名するためのキーストアへのパスです。|
|KEYSTORE_ALIAS|APK に署名するために使用されるキーストア内のキー。|
|INPUT_APK|`xbuild`によって作成された APK。|
|SIGNED_APK|`jarsigner`によって生成された符号付き APK。|
|FINAL_APK|これは、`zipalign`によって生成される zip アラインメントされた APK です。|
|STORE_PASS|これは、ファイルを歌うするためにキーストアのコンテンツにアクセスするために使用されるパスワードです。|

「要件」セクションで説明したように、EnvInject プラグインを使用してビルド時にこれらの環境変数を設定できます。 このジョブには、次のスクリーンショットに示すように、挿入環境変数に基づいて新しいビルドステップが追加されている必要があります。

![](jenkins-walkthrough-images/image39.png "The job should have a new build step added based on the Inject environment variables")

表示される [**プロパティのコンテンツ**フォーム] フィールドでは、環境変数が1行に1つずつ、次の形式で追加されます。

```
ENVIRONMENT_VARIABLE_NAME = value
```

次のスクリーンショットは、APK に署名するために必要な環境変数を示しています。

![](jenkins-walkthrough-images/image40.png "This screenshot shows the environment variables that are required for signing the APK")

APK ファイルの環境変数の一部が `WORKSPACE` 環境変数に基づいて構築されていることに注意してください。

最終的な環境変数は、キーストアの内容にアクセスするためのパスワードです: `STORE_PASS`。 パスワードは、ログファイルで隠ぺいまたは省略する必要がある機微な値です。 EnvInject プラグインは、これらの値がログに表示されないように保護するように構成できます。

ジョブ構成の **[ビルド]** セクションの直前に、 **[ビルド環境]** セクションがあります。 [**パスワードを挿入**する] チェックボックスをオンにすると、一部のフォームフィールドが表示されます。 これらのフォームフィールドは、環境変数の名前と値をキャプチャするために使用されます。 次のスクリーンショットは、`STORE_PASS` 環境変数を追加する例を示しています。

![](jenkins-walkthrough-images/image41.png "This screenshot is an example of adding the STOREPASS environment variable")

環境変数が初期化されたら、次の手順として、APK の署名と zip の配置のためのビルドステップを追加します。 環境変数を挿入するビルド手順は、`jarsigner` と `zipalign`を実行する別の**実行シェル**コマンドビルドになります。 各コマンドには、次のスニペットに示すように、1行ずつ移動します。

```
jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
zipalign -f -v 4 $SIGNED_APK $FINAL_APK
```

次のスクリーンショットは、`jarsigner` と `zipalign` のコマンドをステップに入力する方法の例を示しています。

![](jenkins-walkthrough-images/image42.png "This screenshot shows an example of how to enter the jarsigner and zipalign commands into the step")

すべてのビルドアクションが実施されたら、手動ビルドをトリガーしてすべてが動作していることを確認することをお勧めします。 ビルドが失敗した場合、ビルドが失敗した原因に関する情報について**コンソールの出力**を確認する必要があります。

### <a name="submitting-tests-to-test-cloud"></a>テストを Test Cloud に送信しています

シェルコマンドを使用して、自動テストを Test Cloud に送信できます。 Xamarin Test Cloud でのテストの実行の設定の詳細については、 [UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)の使用に関するこのガイドを参照してください。

## <a name="summary"></a>まとめ

このガイドでは、macOS でビルドサーバーとして Jenkins を導入し、Xamarin モバイルアプリケーションをリリース用にコンパイルおよび準備するように構成しました。 ビルドプロセスをサポートするために、複数のプラグインと共に macOS コンピューターに Jenkins をインストールしました。 TFS または Git からコードをプルし、そのコードをリリース対応アプリケーションにコンパイルするジョブを作成し、構成しました。 また、ジョブを実行するタイミングをスケジュールする2つの方法についても説明します。

## <a name="related-links"></a>関連リンク

- [継続的インテグレーション](~/tools/ci/index.md)
- [App Center テスト](https://docs.microsoft.com/appcenter/test-cloud/)
