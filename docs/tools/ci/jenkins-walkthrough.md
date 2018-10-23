---
title: Xamarin における Jenkins の使用
description: このドキュメントでは、Xamarin アプリケーションで継続的インテグレーションに Jenkins を使用する方法について説明します。 その中で、Jenkins のインストール、構成、および使用方法について説明します。
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: 8094f1ab78252e6d6bd8f5991bcb567b36ed1e9b
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935346"
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin における Jenkins の使用

_このガイドでは、継続的インテグレーション サーバーとして Jenkins を設定し、Xamarin で作成したモバイル アプリケーションのコンパイルを自動化する方法を示します。その中で、OS X での Jenkins のインストール、構成、およびソースコード管理システムへ変更をコミットした時に、Xamarin.iOS と Xamarin.Android をコンパイルするためのジョブの設定方法について説明します。_

[Xamarin を使用した継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md) では、早期にコードの破損や不整合の警告を提供する便利なソフトウェア開発の手法として継続的インテグレーションを説明しています。 CI は、開発者が発生した課題や問題を処理し、ソフトウェアを開発のために安定した状態に保持し続けることを可能にします。 このチュートリアルでは、両方のドキュメントの内容を併用する方法について説明します。

このガイドでは、OS X を実行する専用のコンピューターに Jenkins をインストールし、コンピューターの起動時に自動的に実行するように構成する方法を示します。 Jenkins をインストールしてすぐに、Msbuild をサポートする追加のプラグインをインストールします。 Jenkins は標準で Git をサポートしています。 TFS をソース コード管理に使っている場合は、追加のプラグインとコマンドラインユーティリティもインストールする必要があります。

Jenkins を構成し、任意の必要なプラグインをインストールしたら、Xamarin.iOS と Xamarin.Android プロジェクトをコンパイルする 1 つ以上のジョブを作成します。 ジョブは、いくつかの作業の実行に必要なメタデータと手順の集まりです。 通常、ジョブは次のような構成になります。

-  **ソースコード管理 (SCM)** – これは、ソースコード管理に接続する方法と、どのファイルを取得するかの情報を含む Jenkins 構成ファイル内のメタデータエントリです。
-  **トリガー** – トリガーは、開発者がソースコードリポジトリに変更をコミットする場合などの特定のアクションに基づいてジョブを開始するために使用します。
-  **ビルド手順** – これは、ソースコードをコンパイルし、モバイルデバイスにインストールできるバイナリを作成するプラグインまたはスクリプトです。
-  **省略可能なビルド アクション** – これには、単体テストの実行、静的コード解析の実行、コードの署名、またはその他の関連する作業をビルドするための別のジョブの開始を含めることができます。
-  **通知** – ジョブは、ビルドの状態に関するいくつかの種類の通知を送信することができます。
-  **セキュリティ** – 省略可能ですが、Jenkins セキュリティ機能を有効にすることも強くお勧めします。


このガイドは、Jenkins サーバーをこれらの各要点をカバーしてセットアップする手順について説明します。 このガイドの終わりまでに、Xamarin のモバイルプロジェクトの IPA および APK を作成するための Jenkins のインストールと構成方法に関する十分な理解が得られるでしょう。

## <a name="requirements"></a>必要条件

理想的なビルドサーバーは、アプリケーションのビルドあるいはテストが唯一の目的である専用のスタンドアロンコンピューターです。 専用のコンピューターは、他の役割 (web サーバーなど) のために要求される成果物が、ビルドを汚染しないようにします。 たとえば、ビルドサーバーが web サーバーとしても機能している場合、web サーバーは、いくつかの共通ライブラリの競合バージョンを要求する可能性があります。 この競合のため、web サーバーが正しく機能しない、または Jenkins が、ユーザーに展開するときに、動作しないビルドを作成する可能性があります。

Xamarin のモバイル アプリ向けのビルド サーバーは、開発者のワークステーションに非常によく似た設定をします。 それには、Jenkins、Visual Studio for Mac、Xamarin.iOS および Xamarin.Android をインストールする ユーザーアカウントがあります。 またすべてのコード署名証明書、プロビジョニング プロファイル、およびキーストアもインストールする必要があります。 *通常、ビルドサーバーのユーザーアカウントは、開発者アカウントから分離します。 必ずビルドサーバーのユーザーアカウントでログインして、すべてのソフトウェア、キー、証明書のインストールと構成を行ってください。*

次の図は、一般的な Jenkins ビルドサーバーの上記のすべての要素を示しています。

 [![](jenkins-walkthrough-images/image1.png "一般的な Jenkins ビルドサーバーの上記のすべての要素を示しています. ")](jenkins-walkthrough-images/image1.png#lightbox)

iOS アプリケーションは、Mac OS X を実行しているコンピューター上でのみ、署名とビルドが可能です。Mac Mini は、合理的な低コストの選択ですが、OS X 10.10 (Yosemite) 以降を実行できるコンピュータであればどれでも十分です。

TFS をソース コード管理に使用している場合は、Microsoftから提供されている [Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785) をインストールすることをお勧めします。 Team Explorer Everywhere は、OS X のターミナルで TFS へのクロスプラットフォーム アクセスを提供します。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins のインストール

Jenkins を使用するための最初のタスクは、インストールです。 OS X で Jenkins を動かす方法は 3 つあります。

-  デーモンとして、バックグラウンドで実行する。
-  Tomcat、Jetty、JBoss などのサーブレットコンテナ内で。
-  ユーザーアカウントで実行される通常のプロセスとして。


ほとんどの従来の継続的インテグレーション アプリケーションは、デーモン (OS X または \*nix) として、または、サービス (Windows) としてバックグラウンドで動作します。 これは、機能は、ビルド環境のセットアップが簡単に実行される、必要に応じて、GUI の相互作用が存在しないシナリオに適しています。 これは、GUIによる対話が要求されないシナリオに適しています。そしてその場合は、ビルド環境のセットアップを簡単に実行できます。 モバイルアプリは、キーストアと署名証明書も必要としますが、Jenkins がデーモンとして実行している場合、アクセスに問題がある可能性があります。

Jenkins.App は、Jenkins をインストールする便利な方法です。 これは、Jenkins サーバーの開始と停止を簡略化する AppleScript ラッパーです。 Bash シェルで実行する代わりに、Jenkins が次のスクリーンショットで示すように Dock 内のアイコンのアプリとして動作します。

 [![](jenkins-walkthrough-images/image2.png "Bash シェルで実行する代わりに、Jenkins が次のスクリーンショットで示すように Dock 内のアイコンのアプリとして動作します. ")](jenkins-walkthrough-images/image2.png#lightbox)

Jenkins の開始または停止は、Jenkins.App の開始または停止と同じくらい簡単です。

Jenkins.App をインストールするには、以下のスクリーンショットで示されているプロジェクトのダウンロードページから、最新バージョンをダウンロードします。

 [![](jenkins-walkthrough-images/image3.png "アプリのプロジェクトからの最新バージョンのダウンロード ページで、このスクリーン ショットで示されているダウンロード")](jenkins-walkthrough-images/image3.png#lightbox)

Zip ファイルを、ビルドサーバーの `/Applications` フォルダに展開し、他の OS X アプリケーションと同様に開始します。
初めて Jenkins.App を起動する場合は、Jenkins をダウンロードすることを通知するダイアログが表示されます。

 [![](jenkins-walkthrough-images/image4.png "アプリには、Jenkins をダウンロードすることを通知するダイアログが表示されます。")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins.App が、ダウンロードを終えると、次のスクリーンショットに示すように Jenkins スタートアップをカスタマイズするかどうかを確認する別のダイアログボックスが表示されます。

 [![](jenkins-walkthrough-images/image5.png "アプリがダウンロードを終えると、このスクリーンショットが示すように Jenkins スタートアップをカスタマイズするかどうかを求める別のダイアログボックスが表示されます。")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins のカスタマイズは省略可能で、アプリの開始時に毎回実行する必要はありません。Jenkins の規定の設定は、ほとんどの状況で動作します。

Jenkins をカスタマイズする必要がある場合は、**Change defaults** ボタンをクリックします。 すると 2 つの連続するダイアログボックスが表示されます。 ひとつは、Java コマンドラインパラメーターを求めるもので、もうひとつは Jenkins コマンドラインパラメーターを求めるものです。 次の 2 つのスクリーンショットは、これら 2 つのダイアログボックスを表示しています。

 [![](jenkins-walkthrough-images/image6.png "このスクリーン ショットは、これらのダイアログを示しています。")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "このスクリーン ショットは、これらのダイアログを示しています。")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins を実行したら、ユーザーがコンピュータへログインするたびに起動できるように、ログイン項目として設定した方が良いでしょう。 これは、次のスクリーンショットに示すように、Dock の Jenkins アイコンを右クリックし、 **オプション…ログイン時に開く** を選択することで可能です。

 [![](jenkins-walkthrough-images/image8.png "これは、このスクリーンショットに示すように、Dock の Jenkins アイコンを右クリックし、 オプション ログイン時に開く を選択することで可能です。")](jenkins-walkthrough-images/image8.png#lightbox)

これにより、Jenkins.App は、コンピュータの起動時ではなく、ユーザーがログインするたびに自動的に起動することが可能になります。 OS X が起動時に自動ログインを使用しているユーザーアカウントを指定することが可能です。 **システム環境設定** を開き、このスクリーンショットに示すように **ユーザーとグループ** アイコンを選択します。

 [![](jenkins-walkthrough-images/image9.png "システム環境設定を開き、このスクリーンショットに示すように、ユーザーとグループのアイコンを選択")](jenkins-walkthrough-images/image9.png#lightbox)

**ログインオプション** ボタンをクリックし、OS X が起動時にログインに使用するアカウントを選択します。

この時点で、Jenkins のインストールは完了です。 ただし、Xamarin のモバイル アプリケーションをビルドしたい場合は、一部のプラグインをインストールする必要があります。


### <a name="installing-plugins"></a>プラグインのインストール

Jenkins.App インストーラーが完了したら、Jenkins を開始して、次のスクリーンショットに示すように web ブラウザーを起動して URL http://localhost:8080 を開きます。

 [![](jenkins-walkthrough-images/image10.png "このスクリーン ショットに示すように、8080")](jenkins-walkthrough-images/image10.png#lightbox)

このページから、次のスクリーンショットに示すように、左上の角にあるメニューより **Jenkins > Jenkinsの管理 > プラグインの管理** を選択します。

 [![](jenkins-walkthrough-images/image11.png "このページの、左上隅にあるメニューから Jenkins Jenkinsの管理 プラグインの管理を選択します")](jenkins-walkthrough-images/image11.png#lightbox)

すると、**Jenkins プラグインマネージャー** ページが表示されます。 利用可能 タブをクリックすると、ダウンロードしてインストールできる 600 を超えるプラグインの一覧が表示されます。 これを以下のスクリーン ショットに示します。

 [![](jenkins-walkthrough-images/image12.png "利用可能 タブをクリックすると、ダウンロードしてインストールできる 600 を超えるプラグインの一覧が表示されます。")](jenkins-walkthrough-images/image12.png#lightbox)

600 のすべてのプラグインの中からその一部を探すためにスクロールすることは、とても面倒でエラーが発生しやすくなる可能性があります。 Jenkins は、画面の右上にフィルター検索フィールドを提供しています。 このフィルターフィールドを使用して検索すると、次のプラグインの 1 つまたは全てを見つけてインストールすることが簡単にできます。

-  **Jenkins MSBuild プラグイン** – このプラグインは、Visual Studio および Visual Studio for Mac のソリューション (.sln) とプロジェクト (.csproj) のビルドを可能にします。
-  **Environment Injector プラグイン** – これはオプションですが、ジョブとビルドレベルでの環境変数を設定できる便利なプラグインです。 このプラグインは、アプリケーションのコード署名に使用されるパスワードなどの変数の追加の保護も提供します。 しばしば  *EnvInject プラグイン* と省略されます。
-  **Team Foundation Server プラグイン** – これは、Team Foundation Server またはソースコード管理に Team Foundation Service を使っている場合のみ必要なオプションのプラグインです。

Jenkins では、追加のプラグインなしで Git がサポートされます。

すべてのプラグインをインストールした後は、Jenkins を再起動し、各プラグインのグローバル設定を構成します。 プラグインのグローバル設定は、次のスクリーン ショットに示すように、左上の **Jenkins > Jenkinsの管理 > システムの設定** にあります。

 [![](jenkins-walkthrough-images/image13.png "プラグインのグローバル設定は、左上の Jenkins / Jenkinsの管理 / システムの設定 にあります。")](jenkins-walkthrough-images/image13.png#lightbox)

このメニュー オプションを選択するときに実行をする、**システムの構成 [Jenkins]** ページ。 このページには、Jenkins 自体を構成して、プラグインのグローバル値の一部を設定セクションが含まれています。  次のスクリーン ショットは、このページの例を示しています。

 [![](jenkins-walkthrough-images/image14.png "このスクリーン ショットは、このページの例を示しています。")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>MSBuild プラグインの構成

MSBuild プラグインは、使用するように構成する必要があります **/Library/Frameworks/Mono.framework/Commands/xbuild** Mac ソリューションとプロジェクト ファイルを Visual Studio をコンパイルします。 下にスクロールして、**システムの構成 [Jenkins]** までページ、**追加の MSBuild**ボタンが表示されたら、次のスクリーン ショットに示すように。

 [![](jenkins-walkthrough-images/image15.png "MSBuild の追加 ボタンが表示されるまでシステム Jenkins の構成ページを下へスクロールします。")](jenkins-walkthrough-images/image15.png#lightbox)

このボタンをクリックし、記入、**名前**と**パス**に**MSBuild**に表示されるフォームのフィールドです。 名前、 **MSBuild**インストールには、わかりやすい、中にする必要があります、 **MSBuild へのパス**へのパスにする必要があります`xbuild`、これは通常 **/Library フレームワーク/Mono.framework/Commands/xbuild**です。 Jenkins が使用できる、保存またはページの下部にある [適用] ボタンをクリックして変更を保存して後`xbuild`ソリューションをコンパイルします。

#### <a name="configuring-the-tfs-plugin"></a>TFS プラグインの構成

このセクションは、ソースコード管理に TFS を使用する場合は必須です。

OS X ワークステーションが TFS サーバーと対話するためには、Team Explorer Everywhere をワークステーションにインストールする必要があります。 Team Explorer Everywhere は、TFS にアクセスするためのクロスプラットフォーム コマンドライン クライアントを含む Microsoft のツールのセットです。 Team Explorer Everywhere は、Microsoft からダウンロードでき、 3 つの手順でインストールすることができます。

1. ユーザーアカウントにアクセスできるディレクトリにアーカイブファイルを展開します。 例えば、**~/tee** にファイルを展開します。
2. 上記の手順で展開されたファイルを保持するフォルダーを含めるシェルまたはシステムへのパスを構成します。 例えば以下のようにします。

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Team Explorer Everywhere がインストールされていることを確認するには、ターミナル セッションを開き、実行、`tf`コマンド。 Tf が正しく構成されている場合、ターミナル セッションで、次の出力が表示されます。

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

Jenkins を完全なパスを構成する必要がある TFS 用のコマンド ライン クライアントがインストールされると、`tf`コマンド ライン クライアント。 下にスクロールして、**システムの構成 [Jenkins]** 次のスクリーン ショットに示すように、Team Foundation Server のセクションが表示されるまでのページします。

 [![](jenkins-walkthrough-images/image17.png "Team Foundation Server のセクションが見つかるまで、システム Jenkins の構成ページを下へスクロールします。")](jenkins-walkthrough-images/image17.png#lightbox)

`tf` コマンドにフルパスを入力して、**Save** ボタンをクリックします。

### <a name="configure-jenkins-security"></a>Jenkins セキュリティの構成

最初にインストールされたため任意のユーザーを設定し、任意の種類のジョブを匿名で実行することは、Jenkins はセキュリティを無効にするがします。 このセクションでは、Jenkins ユーザー データベースを使用して認証と承認を構成するセキュリティを構成する方法について説明します。

選択するとセキュリティ設定が見つかりません**Jenkins > 管理 Jenkins > グローバル セキュリティの構成**このスクリーン ショットに示すように、します。

 [![](jenkins-walkthrough-images/image18.png "Jenkins を選択してセキュリティの設定が見つかりません/管理 Jenkins/グローバル セキュリティの構成")](jenkins-walkthrough-images/image18.png#lightbox)

**グローバル セキュリティの構成** ページで、チェック、**を有効にするセキュリティ** チェック ボックスおよび**アクセス制御**フォームが表示されます、次のスクリーン ショットのような。

 [![](jenkins-walkthrough-images/image19.png "グローバル セキュリティの構成 ページで、チェックを有効にするセキュリティ チェックとアクセス制御フォームが表示されるこのスクリーン ショットのような")](jenkins-walkthrough-images/image19.png#lightbox)

次のスクリーンショットに示すように、**Security Realm Section** の **Jenkinsのユーザーデータベース** ラジオボタンを押して、**ユーザーにサインアップを許可** にもチェックを入れたことを確認します。

 [![](jenkins-walkthrough-images/image20.png "セキュリティの領域で、Jenkins 独自のユーザー データベースのオプション ボタンを切り替えるし、サインアップするユーザーを許可する をチェックすることを確認してください。")](jenkins-walkthrough-images/image20.png#lightbox)

最後に、Jenkins を再起動し、新しいアカウントを作成します。 作成した最初のアカウントはルートアカウントで、このアカウントは自動的に管理者に昇格されます。 **グローバルセキュリティの設定** ページに戻り、**Matrix-based security** ラジオボタンにチェックを入れます。 次のスクリーンショットで示すように、ルートアカウントにはフルアクセスを付与し、匿名アカウントには読み取り専用アクセスを付与した方が良いでしょう。

 [![](jenkins-walkthrough-images/image21.png "ルート アカウントのフル アクセスを許可し、匿名アカウントに読み取り専用のアクセスを付与する必要があります。")](jenkins-walkthrough-images/image21.png#lightbox)

これらの設定を保存して Jenkins を再起動すると、セキュリティが有効になります。


#### <a name="disabling-security"></a>セキュリティを無効にする

パスワードを忘れたまたは Jenkins wide ロックアウトが発生した場合は、次の手順に従ってセキュリティを無効にすることができます。

1. Jenkins を停止します。 Jenkins.app を使用している場合は、Dock の Jenkins.App アイコンを右クリックし、ポップアップメニューから終了を選択して停止できます。

    ![](jenkins-walkthrough-images/image19.png "ドック、および終了をポップアップ表示されるメニューから選択することでアプリのアイコン")
2. ファイルを開く **~/.jenkins/config.xml**をテキスト エディターでします。
3. 値を変更、`<usesecurity></usesecurity>`要素から`true`に`false`です。
4. 削除、`<authorizationstrategy></authorizationstrategy>`と`<securityrealm></securityrealm>`ファイルからの要素。
5. Jenkins を再起動します。

## <a name="setting-up-a-job"></a>ジョブの設定

Jenkins では、最上位レベルに編成すべてにソフトウェアの構築に必要なさまざまなタスクの*ジョブ*です。 ジョブでは、ビルド、ソース コード、ビルドの実行頻度、ビルド、ために必要な任意の特殊な変数を取得する方法や、ビルドが失敗した場合を開発者に通知する方法などに関する情報を提供する、関連付けられているメタデータもあります。

ジョブが作成を選択して**Jenkins > 新しいジョブ**右上隅にある次のスクリーン ショットに示すように、メニューから。


![](jenkins-walkthrough-images/image22.png "ジョブは、メニューの右上 Jenkins 新規ジョブ作成 を選択して作成します。")

これが表示されます、**新しいジョブ [Jenkins]** ページ。 ジョブの名前を入力し、選択、**空きスタイル ソフトウェア プロジェクトのビルド**ラジオ ボタンをクリックします。 次のスクリーン ショットは、この例を示しています。

![](jenkins-walkthrough-images/image23.png "ジョブの名前を入力し、フリースタイル・プロジェクトのビルド ラジオボタンを選択します。")

**OK** ボタンクリックすると、ジョブの構成ページが表示されます。 これは、次のスクリーンショットのようになります。

![](jenkins-walkthrough-images/image24.png "このスクリーンショットのようになります。")

Jenkins は、次のパスにあるハード ディスク上のディレクトリ内のジョブ、編成: **~/.jenkins/jobs/[JOB 名]**

このフォルダーには、ログ、構成ファイル、コンパイルに必要なソースコードなどのすべてのファイルと成果物が含まれています。

最初のジョブを作成したら、次の 1 つ以上構成する必要があります。

 - ソースコード管理システムを指定する必要があります。
 - 1 つ以上の *ビルド アクション* をプロジェクトに追加する必要があります。 これらは、アプリケーションをビルドするために必要な手順やタスクです。
 - ジョブには 1 つの*ビルド トリガー*を割り当てる必要があります – Jenkins がどのくらいの頻度でコードを取得し最終的なプロジェクトをビルドするのかを知らせる命令のセットです。

### <a name="configuring-source-code-control"></a>ソース コード管理の構成

Jenkins が行う最初のタスクは、ソース コード管理システムからソース コードを取得することです。 Jenkins は、現在利用可能な一般的なソース コード管理システムの多くをサポートしています。 このセクションでは、2 つの一般的なシステムである Git と Team Foundation Server について説明します。 これらそれぞれのソース コード管理システムについては、以下のセクションで詳しく説明します。

#### <a name="using-git-for-source-code-control"></a>ソース コード管理に Git を使用する

ソース コード管理用に TFS を使用している場合は、このセクションは[スキップ](#Using_TFS_for_Source_Code_Management)して、TFS を使用する次のセクションに進んでください。

Jenkins は、追加プラグインは必要ありません out of box: Git をサポートします。 Git を使用するのには、をクリックして、 **Git**ラジオ ボタンと、次のスクリーン ショットに示すように、Git リポジトリの URL を入力します。

![](jenkins-walkthrough-images/image25.png "Git を使用するには、Git ラジオボタンをクリックして、Git リポジトリの URL を入力します。")

変更を保存すると、Git の設定は完了です。

#### <a name="using-tfs-for-source-code-management"></a>ソース コード管理に TFS を使用する

このセクションは、TFS ユーザーにのみ該当します。

をクリックして、 **Team Foundation Server**ラジオ ボタンをクリックし、TFS の構成 セクションが表示されます、次のスクリーン ショットでは新機能に似ています。


![](jenkins-walkthrough-images/image26.png "Team Foundation Server ラジオボタンをクリックすると、TFS 構成セクションが表示されます。")

TFS に必要な情報を指定します。 次のスクリーンショットは、完成したフォームの例を示しています。

![](jenkins-walkthrough-images/image27.png "このスクリーンショットは、完成したフォームの例を示しています。")

#### <a name="testing-the-source-code-control-configuration"></a>ソース コード管理構成のテスト

適切にソース コード管理を構成したら、**保存** をクリックして変更を保存します。 すると、次のスクリーンショットのようにジョブのホームページに戻ります。

![](jenkins-walkthrough-images/image28.png "このスクリーンショットのようにジョブのホームページに戻ります")

ソースコード管理が適切に構成されていることを検証する最も簡単な方法は、ビルドアクションが 1 つも指定されていなくても、手動でビルドをトリガーすることです。 ビルドを手動で開始するために、ジョブのホームページには、次のスクリーンショットに示すように、左側のメニュー内に **ビルド実行** リンクがあります。

![](jenkins-walkthrough-images/image29.png "ビルドを手動で開始するために、ジョブのホームページの左側のメニューに ビルド実行 リンクがあります")

ビルドが開始されると、履歴の作成 ダイアログには、点滅青い円形、進行状況バー、ビルド番号およびビルドが開始された、次のスクリーン ショットのような時間が表示されます。

![](jenkins-walkthrough-images/image30.png "青い点滅円、進行状況バー、ビルド番号およびビルドの開始時刻にビルドが開始されると、履歴の作成 ダイアログが表示されます。")

ジョブが成功すると、青の円形が表示されます。 ジョブが失敗した場合は、赤い円形が表示されます。

ビルド部分に発生した可能性がある問題のトラブルシューティングを助けるために、Jenkins は、すべてのジョブのコンソール出力をキャプチャします。 コンソール出力を見るには、**ビルド履歴** のジョブをクリックして、左側のメニューの **コンソール出力** リンクをクリックします。 次のスクリーンショットでは、**コンソール出力** リンクと、成功したジョブの出力の一部を示しています。

![](jenkins-walkthrough-images/image31.png "このスクリーンショットは、コンソール出力リンクと成功したジョブの出力の一部を示しています。")

#### <a name="location-of-build-artifacts"></a>ビルド成果物の場所

Jenkins は、ソースコート全体を *ワークスペース* と呼ばれる特別なフォルダーに保存します。 このディレクトリは、以下の場所のフォルダー内にあります。

    ~/.jenkins/jobs/[JOB NAME]/workspace

ワークスペースへのパスは、`$WORKSPACE` という名前の環境変数に格納されています。

ジョブのランディング ページに移動して、をクリックして Jenkins でワークスペースのフォルダーを参照することは、**ワークスペース**左側のメニューにリンクします。 次のスクリーン ショットは、という名前のジョブ ワークスペースの例を示しています**HelloWorld**:。

![](jenkins-walkthrough-images/image32.png "このスクリーン ショットは、HelloWorld という名前のジョブ ワークスペースの例を示しています。")

### <a name="build-triggers"></a>トリガーの作成

Jenkins ビルドを開始するためのいくつかの異なる方法がある – これらと呼ばれます*ビルド トリガー*です。 ビルド トリガーでは、Jenkins ジョブを開始し、プロジェクトをビルドするかを決めることができます。 2 つの一般的なビルド トリガーは。

- **定期的にビルド** – このトリガーは、Jenkins が、2 時間ごとや平日の午前 0 時ごとなどの指定された間隔でジョブを開始するようにします。 ビルドは、ソース コード リポジトリに変更があったかどうかに関係なく開始されます。
- **ポーリング SCM** – このトリガーは、定期的にソース コード管理をポーリングします。 ソース コード リポジトリに変更がコミットされた場合、Jenkins は、新しいビルドを開始します。

ポーリング SCM は、開発者がビルドを破壊するような変更をコミットした時に、すばやくフィードバックできるため、人気のあるトリガーです。 これは、最近コミットされたコードが問題の原因となったということをチームに通知でき、開発者は問題にすばやく取り組むことができます。

定期的なビルドは、テスターに配布可能なアプリケーションのバージョンの作成によく使用されます。 たとえば、QA チームのメンバーが前の週の作業をテストできるように、金曜日の夜に定期的なビルドをスケジュールする、というようなことがあります。


### <a name="compiling-a-xamarinios-applications"></a>Xamarin.iOS アプリケーションをコンパイルします。
使用して、コマンドラインでコンパイルする Xamarin.iOS プロジェクト`xbuild`または`msbuild`です。 シェル コマンドは、Jenkins を実行しているユーザー アカウントのコンテキストで実行されます。 ユーザー アカウントがアプリケーションを正しく配布のパッケージ化するように、プロビジョニング プロファイルへのアクセスを持っている必要があります。 ジョブの構成 ページにこのシェル コマンドを追加することができます。

**ビルド**セクションまで下にスクロールします。 **ビルド手順の追加** ボタンをクリックして、次のスクリーンショットに示すように **シェルの実行** を選択します。

![](jenkins-walkthrough-images/image33.png "ビルド ステップの追加 ボタンをクリックし、シェルの実行を選択")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトのビルド

Xamarin.Android プロジェクトのビルドは、Xamarin.iOS プロジェクトをビルドする概念によく似ています。 Xamarin.Android プロジェクトから APK を作成するには、次の 2 つの手順を実行して Jenkins を構成する必要があります。

 - MSBuild プラグインを使用して、プロジェクトをコンパイルします。
 - 有効なリリース用のキーストアを使って APK を署名し zipalign します。

これら 2 つの手順は、次の 2 つのセクションで詳しく取り上げます。

### <a name="creating-the-apk"></a>APK の作成

をクリックして、**ビルド ステップの追加**ボタンをクリックし、選択 **、Visual Studio プロジェクトや MSBuild を使用してソリューションをビルド**次のスクリーン ショットに示すように。

![](jenkins-walkthrough-images/image36.png "作成する APK クリックして追加ビルド ステップ ボタンとビルドの選択、Visual Studio プロジェクトやソリューションの MSBuild の使用")

ビルド ステップがプロジェクトに追加されたら、出現したフォーム フィールドに入力します。 次のスクリーンショットは、完成したフォームの 1 つの例を示します。

![](jenkins-walkthrough-images/image37.png "ビルド ステップがプロジェクトに追加されたら、出現したフォーム フィールドに入力します。")


このビルド ステップは、**$WORKSPACE** フォルダーで `xbuild` を実行します。 MSBuild のビルド ファイルは、**Xamarin.Android.csproj** ファイルに設定されています。 **コマンドライン引数** には、ターゲットの **PackageForAndroid** のリリース ビルドを指定します。 この手順の成果物は、次の場所にある APK になります。

    $WORKSPACE/[PROJECT NAME]/bin/Release

次のスクリーンショットは、この APK の例を示しています。

![](jenkins-walkthrough-images/image38.png "このスクリーンショットは、この APK の例を示しています。")

この APK は、秘密キーストアで署名されておらず、zipalign する必要があるので、まだデプロイできません。

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>リリース用の APK に署名して Zipalign する

APK の署名と zipalign は、技術的に独立した 2 つのタスクで、Android SDK の 2 つの個別のコマンド ライン ツールによって実行されます。 ただし、1 つのビルド アクションで実行すると便利です。 APK の署名と zipalign の詳細については、リリース用の Android アプリケーションの準備に関する Xamarin のドキュメントを参照してください。

これら 2 つのコマンドは、プロジェクト間で異なる コマンド ライン パラメーターが必要です。 さらに、これらのコマンド ライン パラメーターの一部は、ビルド実行時にはコンソール出力に表示されないパスワードです。 環境変数にこれらのコマンド ライン パラメーターの一部を保存します。 署名または zipalign に必要な環境変数は、以下の表で説明します。

|環境変数|説明|
|--- |--- |
|KEYSTORE_FILE|これは、APK の署名にキー ストアへのパス|
|KEYSTORE_ALIAS|キーストアを APK の署名に使用されるキーです。|
|INPUT_APK|によって作成される APK`xbuild`です。|
|SIGNED_APK|によって生成された署名付き APK`jarsigner`です。|
|FINAL_APK|これは、zip 配置によって生成される APK`zipalign`です。|
|STORE_PASS|これは、ファイルを singing のキー ストアの内容にアクセスに使用されるパスワードです。|

必要条件のセクションで述べたように、これらの環境変数は、EnvInject プラグインを使用して、ビルド時に展開することができます。 ジョブは、次のスクリーンショットに示すように、環境変数のインジェクトに基づいて新しいビルド ステップを追加する必要があります。

![](jenkins-walkthrough-images/image39.png "ジョブは、環境変数のインジェクトに基づいて新しいビルド ステップを追加する必要があります。")

出現したフォーム フィールドの **プロパティ** に、次の形式で 1 行につき 1 つずつ、環境変数を追加します。

    ENVIRONMENT_VARIABLE_NAME = value

次のスクリーンショットは、APK の署名に必要な環境変数を示しています。

![](jenkins-walkthrough-images/image40.png "このスクリーンショットは、APK の署名に必要な環境変数を示しています。")

APK ファイルの環境変数の一部は、`WORKSPACE` 環境変数上でビルドされることに注意してください。

最後の環境変数は、キーストアの内容にアクセスするパスワード `STORE_PASS` です。 パスワードは、機密性の高い値であり、ログファイルでは隠したり省略したりする必要があります。 EnvInject プラグインは、これらをログに表示しないように、これらの値を保護するように構成できます。

直前に、**ビルド**ジョブの構成のセクションは、**ビルド環境**セクションです。 ときに、**パスワードを挿入** チェック ボックスが切り替えられると、何らかの形式のフィールドを表示します。 これらのフォーム フィールドは、環境変数の値と名前のキャプチャに使用されます。 次のスクリーン ショットは、追加の例、`STORE_PASS`環境変数。

![](jenkins-walkthrough-images/image41.png "このスクリーンショットは STOREPASS 環境変数を追加する例です。")

環境変数が初期化されると、次の手順は、署名のビルド ステップを追加し、APK の整列を zip はします。 別の環境変数を挿入するビルド ステップになる後すぐに**シェルの実行**を実行するコマンド ビルド`jarsigner`と`zipalign`です。 次のスニペットに示すように、1 行の各コマンドを実行します。


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

次のスクリーンショットでは、手順に `jarsigner` と `zipalign` コマンドを入力する方法の例を示しています。

![](jenkins-walkthrough-images/image42.png "このスクリーンショットは、この手順に jarsigner と zipalign コマンドを入力する方法の例を示しています。")

すべてのビルド アクションが準備できたら、すべての動作を確認するために手動でビルドをトリガーすることをお勧めします。 ビルドに失敗した場合、**コンソール出力** でビルドが失敗した原因についての情報を確認する必要があります。

### <a name="submitting-tests-to-test-cloud"></a>Test Cloud へテストを送信する

自動テストは、シェル コマンドを使用してテスト クラウドに送信することができます。 Xamarin Test Cloud でテストの実行の設定に関する詳細についてを使用するためこのガイドを参照してください。 [Xamarin.UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)です。


## <a name="summary"></a>まとめ

このガイドでは、Mac OS X 上のビルド サーバーとして Jenkins を導入し、リリースに向けて Xamarin モバイル アプリケーションをコンパイルして準備するための構成を行いました。 Jenkins は、Mac OS X コンピューターに、ビルド処理をサポートするための複数のプラグインと一緒にインストールしました。 TFS または Git のいずれかからコードを取得し、そのコードをリリースの準備ができているアプリケーションへコンパイルするというジョブを作成し構成しました。 また、ジョブを実行する時間をスケジュールする 2 つの異なる方法について調査しました。

## <a name="related-links"></a>関連リンク

- [継続的インテグレーション](https://developer.xamarin.com~/testcloud/ci/)
- [Xamarin Test Cloud へのテストの送信](https://developer.xamarin.com~/testcloud/submitting/)
- [Xamarin に Jenkins がサポートされていないのはなぜですか](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
