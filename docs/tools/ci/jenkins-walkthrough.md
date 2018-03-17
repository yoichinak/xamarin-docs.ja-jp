---
title: "Xamarin で Jenkins の使用"
description: "このガイドでは、Jenkins 継続的インテグレーション サーバーとして設定し、Xamarin で作成したモバイル アプリケーションのコンパイルを自動化する方法を示します。 これには、OS X 上の Jenkins のインストール、構成、および変更は、ソース コード管理システムにコミットするときに、Xamarin.iOS および Xamarin.Android アプリケーションをコンパイルするジョブを設定する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: ff754a690627e7e2f0a5cd39dd669a4c9ddd47fb
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="using-jenkins-with-xamarin"></a>Xamarin で Jenkins の使用

_このガイドでは、Jenkins 継続的インテグレーション サーバーとして設定し、Xamarin で作成したモバイル アプリケーションのコンパイルを自動化する方法を示します。これには、OS X 上の Jenkins のインストール、構成、および変更は、ソース コード管理システムにコミットするときに、Xamarin.iOS および Xamarin.Android アプリケーションをコンパイルするジョブを設定する方法について説明します。_

[Xamarin を使用した継続的な統合の概要](~/tools/ci/intro-to-ci.md)破損または互換性のないコードを早期に警告を提供する便利なソフトウェア開発の手法として継続的な統合を紹介します。 CI できるように問題を解決し、問題を問題が発生し、展開に適切な状態で、ソフトウェアを保持します。 このチュートリアルでは、両方のドキュメントの内容を併用する方法について説明します。

このガイドでは、OS X を実行する専用のコンピューターに Jenkins をインストールし、コンピューターの起動時に自動的に実行するように構成する方法を示します。 Jenkins がインストールされると、Msbuild をサポートする追加プラグインをインストールします。 Jenkins では、すぐ Git がサポートしています。 TFS ソース コード管理されている、その他のプラグインとコマンド ライン ユーティリティもインストールする必要があります。

Jenkins が構成されているし、任意の必要なプラグインがインストールされている、し Xamarin.iOS、Xamarin.Android プロジェクトをコンパイルする 1 つまたは複数のジョブを作成します。 ジョブは、手順と作業の実行に必要なメタデータのコレクションです。 通常、ジョブは、次ので構成されます。

-  **ソース コード管理 (SCM)** – これは、ソース コード管理に接続する方法および取得するファイルに情報を含む Jenkins 構成ファイル内のメタ データ エントリです。
-  **トリガー** – トリガーは、開発者が、ソース コード リポジトリに変更をコミットする場合など、特定のアクションに基づいてジョブを開始するために使用します。
-  **ビルド手順**– これは、プラグインは、ソース コードをコンパイルし、モバイル デバイスにインストールできるバイナリを作成するスクリプト。
-  **省略可能なビルド アクション**-単体テスト、コード、コードの署名を静的な分析を実行することを実行しているなどがありますまたは関連する作業をビルドする他を実行する別のジョブを開始します。
-  **通知**– ジョブは、いくつかの種類のビルドの状態に関する通知を送信可能性があります。
-  **セキュリティ**– は省略可能な Jenkins セキュリティ機能も有効にすることを強くお勧めします。


このガイドは、これらのポイントの各をカバーする Jenkins サーバーをセットアップする手順について説明します。 末尾をセットアップして IPA および APK の Xamarin モバイル プロジェクトの作成に Jenkins を構成する方法をよく理解お割り当てる必要があります。

## <a name="requirements"></a>要件

最適なビルド サーバーは、可能性のあるアプリケーションのテストのビルドとの唯一の目的に専用のスタンドアロン コンピューターです。 専用のコンピューターにより、他の役割 (web サーバーなど) に必要な可能性のある成果物には、ビルドはソケットピンしないようにします。 たとえば、ビルド サーバーが web サーバーとしても機能している場合、web サーバーは、競合しているバージョンのいくつか共通ライブラリを必要があります。 この競合があるため、web サーバーが正しく機能しませんまたは Jenkins がユーザーに展開するときに動作しないビルドを作成可能性があります。

Xamarin のモバイル アプリのビルド サーバーは、非常に非常によく似た開発者のワークステーションを設定します。 どの Jenkins では、Visual Studio for Mac のユーザー アカウントがあるし、Xamarin.iOS および Xamarin.Android がインストールされます。 すべてのコード署名証明書、プロビジョニング プロファイル、およびキー ストアもインストールする必要があります。 *通常、ビルド サーバーのユーザー アカウントは、開発者アカウントから別 - インストールし、すべてのソフトウェア、キー、およびビルド サーバーのユーザー アカウントでログインしたときに証明書を構成してください。*

次の図は、すべての一般的な Jenkins ビルド サーバーでこれらの要素を示しています。

 [![](jenkins-walkthrough-images/image1.png "この図のすべての一般的な Jenkins ビルド サーバーでこれらの要素")](jenkins-walkthrough-images/image1.png#lightbox)

iOS アプリケーションのビルドし、Mac OS X を実行しているコンピューター上の署名のみが可能です。Mac Mini は容易で低コストの適切な選択肢の場合、OS X 10.10 (Yosemite) を実行できる任意のコンピューターがまたは以降だけで十分です。

TFS は、ソース コード管理に使用されている場合はインストールする[Team Explorer Everywhere](http://www.microsoft.com/en-ca/download/details.aspx?id=40785)は、Microsoft から利用できます。 Team Explorer Everywhere は、OS X でターミナルで TFS をプラットフォーム間のアクセスを提供します。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins をインストールします。

Jenkins を使用して最初のタスクでは、それをインストールします。 3 つの方法で OS x: Jenkins を実行するには

-  として、デーモン、バック グラウンドで実行されています。
-  Tomcat、Jetty、JBoss などサーブレット コンテナーです。
-  ユーザー アカウントで実行されている通常のプロセスとして


バック グラウンドでデーモンとして継続的インテグレーションの従来のほとんどのアプリケーションを実行 (OS X 上または\*nix) または (Windows) でサービスとして。 これは、機能は、ビルド環境のセットアップが簡単に実行される、必要に応じて、GUI の相互作用が存在しないシナリオに適しています。 モバイル アプリは、キーストアと Jenkins がデーモンとして実行しているときに問題のあるアクセスになる可能性がある証明書の署名にも必要とします。 これらの問題のため、このドキュメントは – Jenkins をユーザー アカウントで、ビルド サーバーで実行されている 3 番目のシナリオについて説明します。

Jenkins.App は、Jenkins をインストールする便利な方法です。 これは、開始を簡略化する AppleScript ラッパーと Jenkins サーバーの停止です。 Bash シェルで実行されて、代わりに Jenkins として動作しアプリのアイコン、ドッキング ステーションに次のスクリーン ショットに示すように。

 [![](jenkins-walkthrough-images/image2.png "Bash シェルで実行されて、代わりに Jenkins として実行され、アプリ、ドッキング ステーションにアイコンこのスクリーン ショットに示すように")](jenkins-walkthrough-images/image2.png#lightbox)

開始または停止 Jenkins は、開始または停止 Jenkins.App と同じくらい簡単です。

Jenkins.App をインストールするには、プロジェクトのダウンロード ページから、次のスクリーン ショットに示されている最新バージョンをダウンロードします。

 [![](jenkins-walkthrough-images/image3.png "アプリのプロジェクトからの最新バージョンのダウンロード ページで、このスクリーン ショットで示されているダウンロード")](jenkins-walkthrough-images/image3.png#lightbox)

Zip ファイルを抽出、`/Applications`上のフォルダー、ビルド サーバー、および他のすべての OS X アプリケーションと同様に、開始します。
初めて Jenkins.App を起動する Jenkins をダウンロードするのでことを通知するダイアログが表示されます。

 [![](jenkins-walkthrough-images/image4.png "アプリケーションでは、Jenkins をダウンロードするのでことを通知するダイアログが表示されます。")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins.App が、ダウンロードを終了すると、希望どおり Jenkins スタートアップをカスタマイズする次のスクリーン ショットに示すようにするかを確認する別のダイアログ ボックスが表示されます。

 [![](jenkins-walkthrough-images/image5.png "アプリのダウンロードが終了した、希望どおり Jenkins スタートアップをカスタマイズするこのスクリーン ショットに示すよう求める別のダイアログ ボックスが表示されます。")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins をカスタマイズして、省略可能なは、Jenkins はほとんどの場合の動作を起動するたびに、アプリ – 既定の設定を実行する必要はありません。

Jenkins をカスタマイズする必要がある場合はクリックして、**既定値を変更**ボタンをクリックします。 これは、2 つの連続するダイアログ ボックスが表示されます。 Java コマンド ライン パラメーターの入力を求めるページと Jenkins コマンド ライン パラメーターを要求します。 次の 2 つのスクリーン ショットは、これら 2 つのダイアログ ボックスを表示します。

 [![](jenkins-walkthrough-images/image6.png "このスクリーン ショットは、これらのダイアログを示しています。")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "このスクリーン ショットは、これらのダイアログを示しています。")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins が実行されている、起動するたびにでコンピューターへのユーザー ログインできるように、ログインのアイテムとして設定することがあります。 ドッキング ステーションに Jenkins アイコンを右クリックし を選択してこれを行う**オプション.ログイン時に開いて**の次のスクリーン ショットに示すようにします。

 [![](jenkins-walkthrough-images/image8.png "これを行う、ドッキング ステーションに Jenkins アイコンを右クリックし、ログイン時 OptionsOpen を選択してこのスクリーン ショットに示すように")](jenkins-walkthrough-images/image8.png#lightbox)

これにより、Jenkins.App たびに自動的に起動する、ユーザーがログインがない場合、コンピューターを起動します。 ブート時に自動的にログインに使用する OS X を使用するユーザー アカウントを指定することができます。 開く、**システム環境設定**を選択し、**ユーザーとグループ**このスクリーン ショットに示すようにアイコン。

 [![](jenkins-walkthrough-images/image9.png "システム環境設定を開き、このスクリーン ショットに示すように、ユーザー グループのアイコンを選択")](jenkins-walkthrough-images/image9.png#lightbox)

をクリックして、**ログイン オプション**ボタンをクリックし、OS X がブート時にログインを使用するアカウントを選択します。

この時点で Jenkins がインストールされました。 ただし、Xamarin のモバイル アプリケーションを構築したい場合は一部のプラグインをインストールする必要があります。


### <a name="installing-plugins"></a>プラグインのインストール

Jenkins.App インストーラーが完了したら、Jenkins の開始し、 http://localhost:8080 、URL を使用して web ブラウザーを起動して、次のスクリーン ショットに示すようにします。

 [![](jenkins-walkthrough-images/image10.png "このスクリーン ショットに示すように、8080")](jenkins-walkthrough-images/image10.png#lightbox)

このページから、次のように選択します。 **Jenkins > 管理 Jenkins > プラグインの管理**で、左上隅にある次のスクリーン ショットに示すように、メニューから。

 [![](jenkins-walkthrough-images/image11.png "このページから、左上隅にあるメニューから Jenkins Jenkins 管理プラグインの管理を選択します")](jenkins-walkthrough-images/image11.png#lightbox)

これが表示されます、 **Jenkins プラグイン Manager**ページ。 利用可能なタブをクリックすると、ダウンロードしてインストールできますが、600 を超えるプラグインの一覧が表示されます。 これを以下のスクリーン ショットに示します。

 [![](jenkins-walkthrough-images/image12.png "利用可能なタブをクリックする場合は、ダウンロードしてインストールできますが、600 を超えるプラグインの一覧が表示されます。")](jenkins-walkthrough-images/image12.png#lightbox)

600 のすべてのプラグインをいくつかは面倒になることができますを検索してエラーが発生しやすいをスクロールします。 Jenkins は、インターフェイスの右上隅のフィルターの検索フィールドを提供します。 このフィルター フィールドを使用して検索すると、検索とインストールされている 1 つまたは複数の次のプラグインを単純化されます。

-  **Jenkins MSBuild プラグイン**– このプラグインでは、Visual Studio および Visual Studio を Mac ソリューション (.sln) とプロジェクト (.csproj) にビルドすることです。
-  **環境インジェクター プラグイン**– これは、ジョブで環境変数を設定し、レベルをビルドできるようにする省略可能であるものの、有用なプラグインします。 アプリケーションの署名コードに使用されるパスワードなどの変数の追加の保護を提供します。 としても省略されています、 *EnvInject プラグイン*です。
-  **Team Foundation Server プラグイン**– これが省略可能なプラグインでのみであるために必要なかどうかは、Team Foundation Server またはソース コード管理用の Team Foundation サービスを使用しています。

Jenkins では、任意の余分なプラグインなし Git がサポートされます。

すべてのプラグインをインストールした後は、Jenkins を再起動し、各プラグインのグローバル設定を構成します。 選択して、プラグインのグローバル設定が見つかりません**Jenkins > 管理 Jenkins > システムの構成**から、左上隅にある次のスクリーン ショットに示すようにします。

 [![](jenkins-walkthrough-images/image13.png "Jenkins を選択して、プラグインのグローバル設定が見つかりません/管理 Jenkins/左上からのシステムの構成は、コーナーを渡す")](jenkins-walkthrough-images/image13.png#lightbox)

このメニュー オプションを選択するときに実行をする、**システムの構成 [Jenkins]**ページ。 このページには、Jenkins 自体を構成して、プラグインのグローバル値の一部を設定セクションが含まれています。  次のスクリーン ショットは、このページの例を示しています。

 [![](jenkins-walkthrough-images/image14.png "このスクリーン ショットは、このページの例を示しています。")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>MSBuild のプラグインを構成します。

MSBuild プラグインは、使用するように構成する必要があります**/Library/Frameworks/Mono.framework/Commands/xbuild** Mac ソリューションとプロジェクト ファイルを Visual Studio をコンパイルします。 下にスクロールして、**システムの構成 [Jenkins]**までページ、**追加の MSBuild**ボタンが表示されたら、次のスクリーン ショットに示すように。

 [![](jenkins-walkthrough-images/image15.png "MSBuild の追加 ボタンが表示されるまでシステム Jenkins の構成ページを下へスクロールします。")](jenkins-walkthrough-images/image15.png#lightbox)

このボタンをクリックし、記入、**名前**と**パス**に**MSBuild**に表示されるフォームのフィールドです。 名前、 **MSBuild**インストールには、わかりやすい、中にする必要があります、 **MSBuild へのパス**へのパスにする必要があります`xbuild`、これは通常**/Library フレームワーク/Mono.framework/Commands/xbuild**です。 Jenkins が使用できる、保存またはページの下部にある [適用] ボタンをクリックして変更を保存して後`xbuild`ソリューションをコンパイルします。

#### <a name="configuring-the-tfs-plugin"></a>TFS プラグインを構成します。

このセクションでは、TFS を使用してソース コード管理する場合は必須です。

OS X ワークステーション TFS サーバーと対話するためには、Team Explorer Everywhere はワークステーションにインストールする必要があります。 Team Explorer Everywhere は、TFS にアクセスするため、クロス プラットフォーム コマンド ライン クライアントを含む Microsoft のツールのセットです。 Team Explorer Everywhere は、Microsoft からダウンロードして 3 つの手順でインストールすることができます。

1. ユーザー アカウントにアクセスできるディレクトリにアーカイブ ファイルを解凍します。 ファイルを解凍するなど、 **~/tee**です。
2. 上記の手順で圧縮されたファイルを保持するフォルダーを含めるシェルまたはシステムへのパスを構成します。 たとえば、オブジェクトに適用された

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Team Explorer Everywhere がインストールされていることを確認するには、ターミナル セッションを開き、実行、`tf`コマンド。 Tf が正しく構成されている場合、ターミナル セッションで、次の出力が表示されます。

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

Jenkins を完全なパスを構成する必要がある TFS 用のコマンド ライン クライアントがインストールされると、`tf`コマンド ライン クライアント。 下にスクロールして、**システムの構成 [Jenkins]**次のスクリーン ショットに示すように、Team Foundation Server のセクションが表示されるまでのページします。

 [![](jenkins-walkthrough-images/image17.png "Team Foundation Server のセクションが見つかるまで、システム Jenkins の構成ページを下へスクロールします。")](jenkins-walkthrough-images/image17.png#lightbox)

完全パスを入力、`tf`コマンドを使用し、**保存**ボタンをクリックします。

### <a name="configure-jenkins-security"></a>Jenkins セキュリティを構成します。

最初にインストールされたため任意のユーザーを設定し、任意の種類のジョブを匿名で実行することは、Jenkins はセキュリティを無効にするがします。 このセクションでは、Jenkins ユーザー データベースを使用して認証と承認を構成するセキュリティを構成する方法について説明します。

選択するとセキュリティ設定が見つかりません**Jenkins > 管理 Jenkins > グローバル セキュリティの構成**このスクリーン ショットに示すように、します。

 [![](jenkins-walkthrough-images/image18.png "Jenkins を選択してセキュリティの設定が見つかりません/管理 Jenkins/グローバル セキュリティの構成")](jenkins-walkthrough-images/image18.png#lightbox)

**グローバル セキュリティの構成** ページで、チェック、**を有効にするセキュリティ** チェック ボックスおよび**アクセス制御**フォームが表示されます、次のスクリーン ショットのような。

 [![](jenkins-walkthrough-images/image19.png "グローバル セキュリティの構成 ページで、チェックを有効にするセキュリティ チェックとアクセス制御フォームが表示されるこのスクリーン ショットのような")](jenkins-walkthrough-images/image19.png#lightbox)

ラジオ ボタンを切り替える**Jenkins のユーザー データベース**で、**セキュリティ レルム セクション**、ことを確認して**サインアップできるように**チェックするに示すように、次のスクリーン ショット:

 [![](jenkins-walkthrough-images/image20.png "セキュリティの領域で、Jenkins 独自のユーザー データベースのオプション ボタンを切り替えるし、サインアップするユーザーを許可する をチェックすることを確認してください。")](jenkins-walkthrough-images/image20.png#lightbox)

最後に、Jenkins を再起動し、新しいアカウントを作成します。 作成される最初のアカウントは、ルート アカウントで、このアカウントは、管理者に自動的に昇格されます。 戻る、**グローバル セキュリティの構成**ページ、およびのチェック、**マトリックスに基づいたセキュリティ**ラジオ ボタンをクリックします。 ルート アカウントのフル アクセスを付与する必要があり、匿名アカウントを負う読み取り専用アクセスでは、次のスクリーン ショットに示すように。

 [![](jenkins-walkthrough-images/image21.png "ルート アカウントのフル アクセスを許可し、匿名アカウントに読み取り専用のアクセスを付与する必要があります。")](jenkins-walkthrough-images/image21.png#lightbox)

これらの設定が保存され、Jenkins が再起動したら後のセキュリティ有効になります。


#### <a name="disabling-security"></a>セキュリティを無効にします。

パスワードを忘れたまたは Jenkins wide ロックアウトが発生した場合は、次の手順に従ってセキュリティを無効にすることは。

1. Jenkins を停止します。 Jenkins.app を使用している場合は、ドッキング ステーションに Jenkins.App アイコンを右クリックし、終了をポップアップ表示されるメニューから選択することでこれを実行できます。

    ![](jenkins-walkthrough-images/image19.png "ドック、および終了をポップアップ表示されるメニューから選択することでアプリのアイコン")
2. ファイルを開く**~/.jenkins/config.xml**をテキスト エディターでします。
3. 値を変更、`<usesecurity></usesecurity>`要素から`true`に`false`です。
4. 削除、`<authorizationstrategy></authorizationstrategy>`と`<securityrealm></securityrealm>`ファイルからの要素。
5. Jenkins を再起動します。

## <a name="setting-up-a-job"></a>ジョブを設定します。

Jenkins では、最上位レベルに編成すべてにソフトウェアの構築に必要なさまざまなタスクの*ジョブ*です。 ジョブでは、ビルド、ソース コード、ビルドの実行頻度、ビルド、ために必要な任意の特殊な変数を取得する方法や、ビルドが失敗した場合を開発者に通知する方法などに関する情報を提供する、関連付けられているメタデータもあります。

ジョブが作成を選択して**Jenkins > 新しいジョブ**右上隅にある次のスクリーン ショットに示すように、メニューから。


![](jenkins-walkthrough-images/image22.png "ジョブは、メニューの右上隅の Jenkins 新しいジョブを選択して作成されます。")

これが表示されます、**新しいジョブ [Jenkins]**ページ。 ジョブの名前を入力し、選択、**空きスタイル ソフトウェア プロジェクトのビルド**ラジオ ボタンをクリックします。 次のスクリーン ショットは、この例を示しています。

![](jenkins-walkthrough-images/image23.png "ジョブでは、名前を入力し、空きスタイル ソフトウェア プロジェクトのラジオ ボタンのビルドを選択します。")

クリックすると、 **OK**ボタンには、ジョブの構成ページが表示されます。 これは、次のスクリーン ショットをようになります。

![](jenkins-walkthrough-images/image24.png "このスクリーン ショットのようになりますこれは、")

Jenkins は、次のパスにあるハード ディスク上のディレクトリ内のジョブ、編成: **~/.jenkins/jobs/[JOB 名]**

このフォルダーには、すべてのファイルとログ、構成ファイル、およびソース コードをコンパイルする必要があるなど、ジョブに固有の成果物が含まれています。

最初のジョブが作成されてと、次の 1 つ以上構成する必要があります。

 - ソース コード管理システムを指定する必要があります。
 - 1 つまたは複数*ビルド アクション*プロジェクトに追加する必要があります。 これらは、手順や、アプリケーションをビルドするために必要なタスクです。
 - いずれかのジョブを割り当てる必要があります*ビルド トリガー* – 一連の命令の Jenkins どのくらいの頻度、コードを取得し、完成したプロジェクトをビルドすることを通知します。

### <a name="configuring-source-code-control"></a>ソース コード管理の構成

Jenkins は最初のタスクは、ソース コード管理システムからソース コードを取得します。 Jenkins では、多くの人気のあるソース コード管理システム現在利用可能なサポートされています。 このセクションでは、次の 2 つの一般的なシステムと Team Foundation Server の Git について説明します。 これらのソース コード管理システムは、以下のセクションで詳しく説明します。

#### <a name="using-git-for-source-code-control"></a>ソース コード管理に Git を使用します。

ソース コード管理用に TFS を使用している場合[スキップ](#Using_TFS_for_Source_Code_Management)このセクションで、TFS を使用して、次のセクションに進みます。

Jenkins は、追加プラグインは必要ありません out of box: Git をサポートします。 Git を使用するのには、をクリックして、 **Git**ラジオ ボタンと、次のスクリーン ショットに示すように、Git リポジトリの URL を入力します。

![](jenkins-walkthrough-images/image25.png "Git を使用して、Git のラジオ ボタンをクリックし、Git リポジトリの URL を入力")

変更が保存されると、Git の構成が完了しました。

#### <a name="using-tfs-for-source-code-management"></a>TFS ソース コード管理を使用

このセクションでは、TFS ユーザーにのみ適用されます。

をクリックして、 **Team Foundation Server**ラジオ ボタンをクリックし、TFS の構成 セクションが表示されます、次のスクリーン ショットでは新機能に似ています。


![](jenkins-walkthrough-images/image26.png "Team Foundation Server のラジオ ボタンをクリックして、TFS 構成セクションが表示されます。")

TFS の必要な情報を指定します。 次のスクリーン ショットは、完成したフォームの例を示します。

![](jenkins-walkthrough-images/image27.png "このスクリーン ショットは、完成したフォームの例を示しています。")

#### <a name="testing-the-source-code-control-configuration"></a>ソース コード コントロール構成のテスト

適切なソース コード管理を構成すると、クリックして**保存**変更を保存します。 ジョブでは、次のスクリーン ショットのようになりますホーム ページに戻りこれは。

![](jenkins-walkthrough-images/image28.png "これは、ジョブでは、このスクリーン ショットのようになりますホーム ページに戻ります")

ソース コード管理が正しく構成されていることを検証する最も簡単な方法は、指定したビルド操作がない場合でも、ビルドを手動でトリガーするです。 ジョブのホーム ページには、ビルドを手動で開始する、**今すぐ作成**次のスクリーン ショットに示すように、左側のメニュー内のリンクします。

![](jenkins-walkthrough-images/image29.png "ビルドを手動で開始するには、ジョブのホーム ページ リンクがあります、今すぐ作成左側のメニュー")

ビルドが開始されると、履歴の作成 ダイアログには、点滅青い円形、進行状況バー、ビルド番号およびビルドが開始された、次のスクリーン ショットのような時間が表示されます。

![](jenkins-walkthrough-images/image30.png "青い点滅円、進行状況バー、ビルド番号およびビルドの開始時刻にビルドが開始されると、履歴の作成 ダイアログが表示されます。")

ジョブが成功すると、青の円が表示されます。 ジョブが失敗した場合は、赤い円が表示されます。

ビルドの一部として発生する可能性がある問題のトラブルシューティングを支援するため Jenkins が、すべてのジョブのコンソール出力がキャプチャされます。 コンソールに出力を表示するには、ジョブは、をクリックして**ビルド履歴**、し、**コンソール出力**左側のメニューにリンクします。 次のスクリーン ショットでは、**コンソール出力**リンクと同様に成功したジョブから出力の一部。

![](jenkins-walkthrough-images/image31.png "このスクリーン ショットは、コンソール出力リンクと同様に成功したジョブから出力の一部を示しています。")

#### <a name="location-of-build-artifacts"></a>ビルド成果物の場所

Jenkins と呼ばれる特別なフォルダーに全体のソース コードの取得は、*ワークスペース*です。 このディレクトリは、次の場所にフォルダー内にあります。

    ~/.jenkins/jobs/[JOB NAME]/workspace

ワークスペースへのパスをという名前の環境変数に格納されます`$WORKSPACE`です。

ジョブのランディング ページに移動して、をクリックして Jenkins でワークスペースのフォルダーを参照することは、**ワークスペース**左側のメニューにリンクします。 次のスクリーン ショットは、という名前のジョブ ワークスペースの例を示しています**HelloWorld**:。

![](jenkins-walkthrough-images/image32.png "このスクリーン ショットは、HelloWorld という名前のジョブ ワークスペースの例を示しています。")

### <a name="build-triggers"></a>トリガーを作成します。

Jenkins ビルドを開始するためのいくつかの異なる方法がある – これらと呼ばれます*ビルド トリガー*です。 ビルド トリガーでは、Jenkins ジョブを開始し、プロジェクトをビルドするかを決めることができます。 2 つの一般的なビルド トリガーは。

- **定期的にビルド**– このトリガーは 2 時間ごとなどの指定された間隔または平日の午前 0 時にジョブを開始する Jenkins、させます。 かどうかに関係なく、ビルドは開始されているすべての変更、ソース コード リポジトリです。
- **ポーリング SCM** – このトリガーは、定期的にソース コード管理をポーリングします。 ソース コード リポジトリにコミットされたすべての変更、Jenkins は、新しいビルドを起動します。

SCM をポーリングという問題が、開発者が発生する中断するビルドの変更のコミット時に簡単にフィードバックを提供するための一般的なトリガーです。 これは、最近コミットされたコードは、問題の原因となってし、開発者は、変更は最新のものを念頭に、問題を解決できますチームの警告用に便利です。

定期的なビルドは、テスト担当者に配布可能なアプリケーションのバージョンの作成によく使用されます。 たとえば、QA チームのメンバーが前の週の作業をテストできるように、金曜日の夜の定期的なビルドをスケジュールする可能性があります。


### <a name="compiling-a-xamarinios-applications"></a>Xamarin.iOS アプリケーションをコンパイルします。
使用して、コマンドラインでコンパイルする Xamarin.iOS プロジェクト`xbuild`または`msbuild`です。 シェル コマンドは、Jenkins を実行しているユーザー アカウントのコンテキストで実行されます。 ユーザー アカウントがアプリケーションを正しく配布のパッケージ化するように、プロビジョニング プロファイルへのアクセスを持っている必要があります。 ジョブの構成 ページにこのシェル コマンドを追加することができます。

下方向にスクロール、**ビルド**セクションです。 をクリックして、**ビルド ステップの追加**ボタンをクリックして**シェルの実行**次のスクリーン ショットに示すように、します。

![](jenkins-walkthrough-images/image33.png "ビルド ステップの追加 ボタンをクリックし、シェルの実行を選択")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトのビルド

Xamarin.Android プロジェクトのビルドは、Xamarin.iOS プロジェクトをビルドする概念的によく似ています。 Xamarin.Android プロジェクトから、APK を作成するには、次の 2 つの手順を実行する Jenkins を構成する必要があります。

 - MSBuild のプラグインを使用して、プロジェクトをコンパイルします。
 - 記号と郵便番号は、有効なリリース キーストアを APK を揃えます。

これら 2 つの手順は、次の 2 つのセクションで詳しく取り上げます。

### <a name="creating-the-apk"></a>APK を作成します。

をクリックして、**ビルド ステップの追加**ボタンをクリックし、選択**、Visual Studio プロジェクトや MSBuild を使用してソリューションをビルド**次のスクリーン ショットに示すように。

![](jenkins-walkthrough-images/image36.png "作成する APK クリックして追加ビルド ステップ ボタンとビルドの選択、Visual Studio プロジェクトやソリューションの MSBuild の使用")

ビルド ステップがプロジェクトに追加されると、表示されるフォーム フィールドを入力します。 次のスクリーン ショットは、完成したフォームの 1 つの例を示します。

![](jenkins-walkthrough-images/image37.png "ビルド ステップがプロジェクトに追加されるが表示されるフォーム フィールドの入力します。")


このビルド ステップを実行する`xbuild`で、 **$WORKSPACE**フォルダーです。 MSBuild のビルド ファイルに設定されている、 **Xamarin.Android.csproj**ファイル。 **コマンドライン引数**ターゲットのリリース ビルドを指定**PackageForAndroid**です。 この手順の積になります、APK を次の場所に。

    $WORKSPACE/[PROJECT NAME]/bin/Release

次のスクリーン ショットは、この APK の例を示します。

![](jenkins-walkthrough-images/image38.png "このスクリーン ショットは、この APK の例を示しています。")

秘密キー ストアに署名されていないとする zip を配置する必要がありますが、この APK を展開については、できていません。

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>署名と Zipaligning リリース APK

Zipaligning、APK の署名と技術的には 2 つ独立したタスクは、Android SDK からの 2 つの個別のコマンドライン ツールによって実行されます。 ただし、1 つのビルド アクションとで実行すると便利です。 詳細については、署名と zipaligning、APK、リリース用の Android アプリケーションの準備の Xamarin のドキュメントを参照してください。

これらのコマンドの両方が、プロジェクトをプロジェクトに異なる場合がありますコマンド ライン パラメーターが必要です。 これらのコマンド ライン パラメーターにも、パスワードに、ビルドが実行されているときにコンソール出力には表示されません。 環境変数にこれらのコマンド ライン パラメーターを格納します。 署名および/または zip の配置に必要な環境変数は次の表で説明します。

|環境変数|説明|
|--- |--- |
|KEYSTORE_FILE|これは、APK の署名にキー ストアへのパス|
|KEYSTORE_ALIAS|キーストアを APK の署名に使用されるキーです。|
|INPUT_APK|によって作成される APK`xbuild`です。|
|SIGNED_APK|によって生成された署名付き APK`jarsigner`です。|
|FINAL_APK|これは、zip 配置によって生成される APK`zipalign`です。|
|STORE_PASS|これは、ファイルを singing のキー ストアの内容にアクセスに使用されるパスワードです。|

必要条件」のように、EnvInject プラグインを使用して、ビルド時にこれらの環境変数を設定できます。 ジョブは、新しいビルドが必要ステップが次のスクリーン ショットに示すように、挿入の環境変数に基づくを追加します。

![](jenkins-walkthrough-images/image39.png "ジョブは、新しいビルドが必要挿入環境変数に基づくステップを追加")

**ドキュメント プロパティを**次の形式で表示されるフィールド、環境変数を追加、行ごとに 1 つを形成します。

    ENVIRONMENT_VARIABLE_NAME = value

次のスクリーン ショットは、APK の署名に必要な環境変数を示しています。

![](jenkins-walkthrough-images/image40.png "このスクリーン ショットは、APK の署名に必要な環境変数を示しています。")

一部の APK ファイルの環境変数がビルドされていることを確認、`WORKSPACE`環境変数。

最終的な環境変数は、キー ストアの内容にアクセスするパスワード:`STORE_PASS`です。 パスワードは、機密性の高い値であり、隠されてまたはログ ファイルでは省略する必要があります。 EnvInject プラグインは、これらをログに表示しないように保護してこれらの値を構成できます。

直前に、**ビルド**ジョブの構成のセクションは、**ビルド環境**セクションです。 ときに、**パスワードを挿入** チェック ボックスが切り替えられると、何らかの形式のフィールドを表示します。 これらのフォーム フィールドは、環境変数の値と名前のキャプチャに使用されます。 次のスクリーン ショットは、追加の例、`STORE_PASS`環境変数。

![](jenkins-walkthrough-images/image41.png "このスクリーン ショットは STOREPASS 環境変数を追加する例です。")

環境変数が初期化されると、次の手順は、署名のビルド ステップを追加し、APK の整列を zip はします。 別の環境変数を挿入するビルド ステップになる後すぐに**シェルの実行**を実行するコマンド ビルド`jarsigner`と`zipalign`です。 次のスニペットに示すように、1 行の各コマンドを実行します。


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

次のスクリーン ショットを入力する方法の例を示しています、`jarsigner`と`zipalign`ステップにコマンド。

![](jenkins-walkthrough-images/image42.png "このスクリーン ショットは、ステップに、jarsigner zipalign コマンドを入力する方法の例を示しています。")

配置のすべてのビルド アクションが表示されたらは、すべてが動作を確認する手動のビルドをトリガーすることをお勧めします。 ビルドに失敗した場合、**コンソール出力**ビルドの失敗の原因について確認する必要があります。

### <a name="submitting-tests-to-test-cloud"></a>Test Cloud テストを送信します。

自動テストは、シェル コマンドを使用してテスト クラウドに送信することができます。 Xamarin Test Cloud でテストの実行の設定に関する詳細についてを使用するためのガイドがある[Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)または[Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)です。


## <a name="summary"></a>まとめ

このガイドでは Mac OS X 上のビルド サーバーとして Jenkins を導入され、コンパイルおよびリリース向けの Xamarin モバイル アプリケーションを準備するように構成します。 Jenkins は、ビルド処理をサポートするために複数のプラグインと Mac OS X コンピューターにインストールしました。 お作成され、TFS または Git のいずれかからコードをプルし、リリースの準備がアプリケーションにそのコードをコンパイルするジョブを構成します。 ジョブを実行するときのスケジュールを設定する 2 つの方法も調査します。

## <a name="related-links"></a>関連リンク

- [継続的インテグレーション](https://developer.xamarin.com~/testcloud/ci/)
- [Xamarin Test Cloud テストを送信します。](https://developer.xamarin.com~/testcloud/submitting/)
- [なぜ Jenkins でサポートされていない Xamarin しますか。](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
