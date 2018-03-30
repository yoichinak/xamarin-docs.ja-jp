---
title: 接続のトラブルシューティング
description: このガイドでは、新しい接続マネージャーの使用中に発生する可能性がある問題を解決する方法について説明します。接続できない問題や SSH の問題などを解決します。
ms.topic: article
ms.prod: xamarin
ms.assetid: A1508A15-1997-4562-B537-E4A9F3DD1F06
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 505bd755140e3d4cdd162caf694de386fc72a68a
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="connection-troubleshooting"></a>接続のトラブルシューティング

_このガイドでは、新しい接続マネージャーの使用中に発生する可能性がある問題を解決する方法について説明します。接続できない問題や SSH の問題などを解決します。_

## <a name="log-file-location"></a>ログ ファイルの場所

- **Mac** – ~/Library/Logs/Xamarin-[MAJOR.MINOR]
- **Windows** – %LOCALAPPDATA%\Xamarin\Logs

ログ ファイルは、Visual Studio で **[ヘルプ]、[Xamarin]、[ログの Zip]** の順に参照すると見つけることができます。


## <a name="wheres-the-xamarin-build-host-app"></a>Xamarin Build Host アプリはどこにありますか?

古いバージョンの Xamarin.iOS の Xamarin Build Host は必要なくなりました。 Visual Studio はリモート ログイン経由でエージェントを自動的に展開し、バックグラウンドで実行するようになりました。 Mac と Windows マシンのいずれかで実行される追加アプリはありません。


## <a name="troubleshooting-remote-login"></a>リモート ログインのトラブルシューティング

> [!IMPORTANT]
> 以下のトラブルシューティング手順は、新しいシステムの初回セットアップ中に発生する問題を主に対象としています。  以前に特定の環境で接続を正常に使用しており、接続が突然または断続的に動作を停止するようになった場合は、(ほとんどの場合に) 次のいずれかが役に立つかどうかの確認に直接進むことができます。 
>   * 下の[既存のビルド ホスト プロセスに起因するエラー](#errors)の説明に従って、残っているプロセスを中止します。 
>   * 下の[ブローカー、IDB、ビルド、デザイナー エージェントの消去](#clearing)の説明に従ってエージェントをクリアしてから、有線インターネット接続を使用して、「[MacBuildHost.local に接続できませんでした。もう一度お試しください。](#tryagain)」の説明に従って IP アドレスを使用して直接接続します。  
> 以上のどの方法でも問題が解決されない場合は、[手順 9](#stepnine) の指示に従い、新しいバグ報告を提出してください。

1. 互換性のあるバージョンの Xamarin.iOS が Mac にインストールされていることを確認します。 Visual Studio 2017 の場合は、Visual Studio for Mac の**安定**配布チャネルであることを確認します。 Visual Studio 2015 以前の場合、両方の IDE で配布チャネルが同じであることを確認します。
    * Visual Studio for Mac で、**[Visual Studio for Mac] > [更新の確認...]** の順に進み、**[更新チャネル]** を表示、または変更します。
    * Visual Studio 2015 以前の場合、**[ツール]、[オプション]、[Xamarin]、[その他]** の順に選択し、配布チャネルを確認します。

2. Mac で **[リモート ログイン]** が有効になっていることを確認します。 **[対象ユーザーのみ]** にアクセスを設定し、Mac ユーザーがリストまたはグループに含まれていることを確認します。

    [![](troubleshooting-images/troubleshooting-image1.png "[対象ユーザーのみ] にアクセスを設定します")](troubleshooting-images/troubleshooting-image1.png#lightbox)

3. ファイアウォールが SSH の既定値であるポート 22 で着信接続を許可するように設定されていることを確認します。

    [![](troubleshooting-images/troubleshooting-image2.png "ファイアウォールがポート 22 で着信接続を許可するように設定されていることを確認します")](troubleshooting-images/troubleshooting-image2.png#lightbox)

    **[Automatically allow signed software to receive incoming connections]\(署名済みソフトウェアには着信接続の受信を自動的に許可する\)** を無効にしている場合、OS X はペアリング プロセス中にダイアログを表示し、着信接続の受信を `mono-sgen` または `mono-sgen32` に許可するか尋ねます。 このダイアログでは必ず **[許可]** をクリックしてください。

    [![](troubleshooting-images/troubleshooting-image4a.png "このダイアログでは [許可] をクリックします")](troubleshooting-images/troubleshooting-image4a.png#lightbox)

4. その Mac でユーザー アカウントにログインしており、GUI セッションが有効になっていることを確認します。

5. _氏名_ではなく_ユーザー名_で Mac に接続していることを確認します。 ユーザー名を利用することで、アクセント記号付きの文字など、氏名に関する既知の制限を回避できます。

    **Terminal.app** で `whoami` コマンドを実行すると_ユーザー名_を確認できます。

    たとえば、下のスクリーンショットでは、アカウント名が **Amy Burns** ではなく **amyb** になります。

    [![](troubleshooting-images/troubleshooting-image5a.png "ターミナル アプリからのアカウント名の取得")](troubleshooting-images/troubleshooting-image5a.png#lightbox)


6. Mac に使用している IP アドレスが正しいことを確認します。 Mac で IP アドレスを見つけるには、**[システム環境設定]、[共有]、[リモート ログイン]** の順に選択します。

    [![](troubleshooting-images/troubleshooting-image17.png "[システム環境設定] アプリの IP アドレス")](troubleshooting-images/troubleshooting-image17.png#lightbox)

7. Mac の IP アドレスを確認したら、Windows の `cmd.exe` でそのアドレスに `ping` を試します。

    ```
    ping 10.1.8.95
    ```
    
    ping に失敗した場合、Windows コンピューターから Mac に_到達できません_。 この問題は、2 台のコンピューター間のローカル エリア ネットワーク構成レベルで解決する必要があります。 両方のマシンが同じローカル ネットワークにあることを確認してください。

8. 次に、OpenSSH の `ssh` クライアントが Windows から Mac に接続できるかテストします。 このプログラムをインストールする方法の 1 つは、[Git for Windows](https://git-for-windows.github.io/) をインストールすることです。 インストール後、**Git Bash** コマンド プロンプトを起動し、自分のユーザー名と IP アドレスで Mac に `ssh` を試します。

    ```bash
    ssh amyb@10.1.8.95
    ```
    <a name="stepnine" />

9. **手順 8 で成功した**場合、接続状態で `ls` のような単純なコマンドを試します。

    ```bash
    ssh amyb@10.1.8.95 'ls'
    ```
    
    Mac のホーム ディレクトリのコンテンツが一覧表示されるはずです。 `ls` コマンドは正しく機能するが、Visual Studio 接続に失敗する場合は、「[既知の問題と制限事項](#knownissues)」セクションで Xamarin に固有の問題を確認してください。 該当する問題が見当たらない場合は、[新しいバグ レポートを提出](https://bugzilla.xamarin.com/newbug)し、ログを添付してください。詳細は、[詳細ログ ファイルの確認](#verboselogs)に関するセクションにあります。

10. **手順 8 で失敗した**場合、Mac のターミナルで次のコマンドを実行し、SSH サーバーが_何らか_の接続を受信しているか確認します。

    ```bash
    ssh localhost
    ```
    
11. 手順 8 で失敗したが、**手順 10 で成功した**場合、問題はおそらく、ネットワーク構成に起因し、Mac ビルド ホストのポート 22 に Windows から到達できないことにあります。 次のような構成問題が考えられます。

    - OS X ファイアウォール設定で接続を禁止しています。 手順 3 をもう一度確認してください。

        OS X ファイアウォールをアプリ別に構成した結果、無効な状態になることがあります。[システム環境設定] に表示される設定が実際の動作を反映していません。 構成ファイル (**/Library/Preferences/com.apple.alf.plist**) を削除し、コンピューターを再起動すると、既定の動作が復元されることがあります。 このファイルを削除する 1 つの方法は、Finder で **[移動]&gt;[フォルダーへ移動]** の順に選択して **/Library/Preferences** に入り、**com.apple.alf.plist** ファイルをごみ箱に移動することです。

    - Mac と Windows コンピューターの間にあるルーターの 1 つのファイアウォール設定が接続をブロックしています。

    - Windows 自体がリモート ポート 22 への発信接続を禁止しています。 これは普通ではありません。 発信接続を禁止するように Windows ファイアウォールを設定することはできますが、既定の設定ではすべての発信接続が許可されます。

    - Mac ビルド ホストが `pfctl` ルールを介してすべての外部ホストからポート 22 へのアクセスを禁止しています。 過去に `pfctl` を構成していることがわかっていない限り、これはありえません。

12. 手順 8 に失敗し、**手順 10 に失敗した**場合、問題はおそらく、Mac の SSH サーバー プロセスが実行されていないか、現在のユーザーにログインを許可するように構成されていないことにあります。 この場合、もっと複雑な問題を調査する前に、手順 2 のリモート ログイン設定をもう一度確認してください。

<a name="knownissues" />

## <a name="known-issues-and-limitations"></a>既知の問題と制限事項

> [!NOTE]
> このセクションは、上の手順 8 と 9 にあるように、OpenSSH SSH クライアントを利用し、Mac のユーザー名とパスワードで Mac ビルド ホストに接続している場合にのみ当てはまります。

### <a name="invalid-credentials-please-try-again"></a>"資格情報が無効です。 もう一度お試しください。"

既知の原因:

- **制限** – このエラーは、名前にアクセント記号付きの文字が含まれるとき、アカウントの_氏名_でビルド ホストにログインしようとすると表示されることがあります。 これは、Xamarin が SSH 接続に利用する [SSH.NET ライブラリ](https://sshnet.codeplex.com/)の制限です。 **回避策**: 上の手順 5 をご覧ください。

### <a name="unable-to-authenticate-with-ssh-keys-please-try-to-log-in-with-credentials-first"></a>"SSH キーで認証できませんでした。 最初に資格情報でログインしてください。"

既知の原因:

- **SSH セキュリティの制限** – このメッセージは多くの場合、Mac の完全修飾パス **$HOME/.ssh/authorized\_keys** にあるファイルまたはディレクトリの 1 つで _[その他]_ メンバーまたは _[グループ]_ メンバーに対して書き込みアクセス許可が有効になっていることを意味します。 **一般的な修正方法**: Mac のターミナル コマンド プロンプトで `chmod og-w "$HOME"` を実行します。 特定のファイルまたはディレクトリが問題を引き起こしている場合の詳細については、ターミナルで `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` を実行し、デスクトップから **sshd.log** ファイルを開き、"Authentication refused: bad ownership or modes" を探します。

### <a name="trying-to-connect-never-completes"></a>"接続しようとしています..." が完了しない

- **バグ [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)** – この問題は、**[システム環境設定] の [ユーザーとグループ]** で Mac ユーザーの **[詳細オプション]** コンテキスト メニューの **[ログイン シェル]** が **/bin/bash** 以外の値に設定されている場合に Xamarin 4.1 で発生することがあります。 (Xamarin 4.2 以降では、このシナリオは、このバグではなく "接続できませんでした" というエラー メッセージにつながります。)**回避策**: **ログイン シェル**を既定の **/bin/bash** に戻します。

<a name="tryagain" />

### <a name="couldnt-connect-to-macbuildhostlocal-please-try-again"></a>"MacBuildHost.local に接続できませんでした。 もう一度お試しください。"

報告されている原因:

- **バグ** – "An unexpected error occurred while configuring SSH for the user ...Session operation has timed out" (ユーザー ... の SSH を構成中に予期しないエラーが発生しました。セッションはタイムアウトになりました。) にさらに詳しいエラーが表示されたとの報告を受けています。 **回避策:** ローカル ユーザー アカウントでビルド ホストにログインします。

- **バグ** – 接続ダイアログで Mac の名前をダブルクリックしてビルド ホストに接続しようとしたとき、このエラーが発生したとの報告を受けています。 **考えられる回避策**: IP アドレスを利用し、[Mac を手動で追加](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add)します。

- **バグ [#35971](https://bugzilla.xamarin.com/show_bug.cgi?id=35971)** – Mac ビルド ホストと Windows の間で無線ネットワーク接続を利用したとき、このエラーが発生したとの報告を受けています。 **考えられる回避策**: 両方のコンピューターを有線ネットワーク接続に移します。

- **バグ [#36642](https://bugzilla.xamarin.com/show_bug.cgi?id=36642)** – Xamarin 4.0 で、Mac の **$HOME/.bashrc** ファイルにエラーがあるとき、このメッセージが必ず表示されます。 (Xamarin 4.1 以降では、**.bashrc** ファイルのエラーは接続プロセスに影響を与えません。)**回避策**: **.bashrc** ファイルをバックアップの場所に移します (あるいは、必要ないことがわかっている場合、削除します)。

- **バグ [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)** – このエラーは、**[システム環境設定] の [ユーザーとグループ]** で Mac ユーザーの **[詳細オプション]** コンテキスト メニューの **[ログイン シェル]** が **/bin/bash** 以外の値に設定されている場合に発生することがあります。 **回避策**: **ログイン シェル**を既定の **/bin/bash** に戻します。

- **制限** – このエラーは、インターネットにアクセスできないルーターに Mac ビルド ホストが接続されている場合に (あるいは、Windows コンピューターの DNS 逆引き参照を要求されるとタイムアウトする DNS サーバーを Mac が使用している場合に) 発生することがあります。 Visual Studio は、SSH フィンガープリントを取得し、結局は接続に失敗するのに約 30 秒かかります。

    **考えられる回避策**: "UseDNS no" を **sshd\_config** ファイルに追加します。 変更前にこの SSH 設定についてお読みください。 たとえば、[unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option](http://unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option) をご覧ください。

    次の手順は、設定を変更する方法の 1 つです。 手順を完了するには、Mac で管理者アカウントにログインする必要があります。

    1. ターミナル コマンド プロンプトで `ls /etc/ssh/sshd_config` と `ls /etc/sshd_config` を実行し、**sshd\_config** ファイルの場所を確認します。 残りのすべての手順では、"該当するファイルまたはディレクトリがありません" を_返さない_この場所を必ず使用します。

        [![](troubleshooting-images/troubleshooting-image18.png "ターミナルでの "ls /etc/ssh/sshd_config" と "ls /etc/sshd_config" の実行")](troubleshooting-images/troubleshooting-image18.png#lightbox)

    3. ターミナルで `cp /etc/ssh/sshd_config "$HOME/Desktop/"` を実行し、デスクトップにファイルをコピーします。

    4. デスクトップのファイルをテキスト エディターで開きます。 たとえば、ターミナルで `open -a TextEdit "$HOME/Desktop/sshd_config"` を実行できます。

    5. ファイルの一番下に次の行を追加します。

        ```
        UseDNS no
        ```
        
    6. `UseDNS yes` という行があればそれを削除し、新しい設定が適用されるようにします。

    7. ファイルを保存します。

    8. ターミナルで `sudo cp "$HOME/Desktop/sshd_config" /etc/ssh/sshd_config` を実行し、編集したファイルを元の場所にコピーします。 入力が求められた場合、パスワードを入力します。

    9. **[システム環境設定]&gt;[共有]&gt;[リモート ログイン]** の順にアクセスして**リモート ログイン**を無効にし、再度有効にして、SSH サーバーを再起動します。

<a name="clearing" />

### <a name="clearing-the-broker-idb-build-and-designer-agents-on-the-mac"></a>Mac でブローカー、IDB、ビルド、デザイナー エージェントを消去する

Mac エージェントの "インストール"、"アップロード"、"起動" 中に発生した問題がログ ファイルで確認された場合、**XMA** キャッシュ フォルダーを削除し、Visual Studio にエージェントの再アップロードを強制させるという方法があります。

1. Mac のターミナルで次のコマンドを実行します。

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```
    
2. **XMA** フォルダーをコントロール クリックし、**[ごみ箱に入れる]** を選択します。

    [![](troubleshooting-images/troubleshooting-image8.png "XMA フォルダーをごみ箱に移動します")](troubleshooting-images/troubleshooting-image8.png#lightbox)

3. Windows のキャッシュもクリアしておくといいでしょう。 Windows でコマンド プロンプトを管理者として開きます。

    ```
    del %localappdata%\Temp\Xamarin\XMA
    ```
    
## <a name="warning-messages"></a>警告メッセージ

このセクションでは、出力ウィンドウやログに表示され、通常は無視してもかまわないメッセージについて説明します。

### <a name="there-is-a-mismatch-between-the-installed-xamarinios--and-the-local-xamarinios"></a>"There is a mismatch between the installed Xamarin.iOS ... and the local Xamarin.iOS" (インストールされている Xamarin.iOS ... とローカル Xamarin.iOS との間に不一致があります)

Mac と Windows の両方が同じ Xamarin 配布チャネルに更新されることを確定している限り、この警告は無視できます。

### <a name="failed-to-execute-ls-usrbinmono-exitstatus1"></a>"'ls /usr/bin/mono': ExitStatus=1 の実行に失敗しました"

Mac で OS X 10.11 (El Capitan) 以降を実行している限り、このメッセージは無視できます。 このメッセージは OS X 10.11 では問題ではありません。Xamarin は **/usr/local/bin/mono** も確認するためです。これは OS X 10.11 で `mono` に求められる正しい場所です。

### <a name="bonjour-service-macbuildhost-did-not-respond-with-its-ip-address"></a>"Bonjour Service 'MacBuildHost' が IP アドレスに応答しませんでした"

接続ダイアログに Mac ビルド ホストの IP アドレスが表示されないのであれば、このメッセージは無視できます。 そのダイアログに IP アドレスが_ない_場合でも、[Mac を手動で追加](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add)できます。

### <a name="invalid-user-a-from-101895-and-inputuserauthrequest-invalid-user-a-preauth"></a>"Invalid user a from 10.1.8.95" (10.1.8.95 のユーザー a が無効です) と "input\_userauth\_request: 無効なユーザー a [preauth]"

**sshd.log** を見ているとき、このメッセージに気付くことがあります。 このメッセージは通常の接続プロセスの一部です。 _SSH フィンガープリント_の取得時、Xamarin でユーザー名 **a** が一時的に利用されることで表示されます。

## <a name="output-window-and-log-files"></a>出力ウィンドウとログ ファイル

ビルド ホストに接続しているとき、Visual Studio でエラーが表示された場合、2 か所で追加メッセージを確認します。出力ウィンドウとログ ファイルです。

### <a name="output-window"></a>[出力] ウィンドウ

出力ウィンドウから始めることが推奨されます。 主要な接続手順とエラーに関するメッセージが表示されます。 出力ウィンドウで Xamarin メッセージを表示するには:

1. メニューで **[表示]、[出力]** の順に選択するか、**[出力]** タブをクリックします。
2. **[出力元の表示]** ドロップダウン メニューをクリックします。
3. **[Xamarin]** を選択します。

[![](troubleshooting-images/troubleshooting-image11.png "[出力] タブで [Xamarin] を選択します")](troubleshooting-images/troubleshooting-image11.png#lightbox)

### <a name="log-files"></a>ログ ファイル

問題を診断するために必要な情報が出力ウィンドウで十分に得られない場合、次にログ ファイルで探します。 ログ ファイルには出力ウィンドウには表示されない追加の診断メッセージが含まれます。 ログ ファイルを表示するには:

1. Visual Studio を起動します。

    > [!IMPORTANT]
    > **.svclogs** は既定では有効になっていません。 アクセスするには、詳細ログありで Visual Studio を起動する必要があります。[詳細ログ](~/cross-platform/troubleshooting/questions/version-logs.md#visual-studio-startup-verbose-logs) ガイドに説明があります。 詳細については、ブログ投稿の「[Troubleshooting Extensions with the Activity Log](https://blogs.msdn.microsoft.com/visualstudio/2010/02/24/troubleshooting-extensions-with-the-activity-log/)」 (アクティビティ ログで拡張機能の問題を解決する) を参照してください。

2. ビルド ホストに接続してみます。

3. Visual Studio で接続エラーが表示されたら、**[ヘルプ]、[Xamarin]、[ログの Zip]** の順に選択し、ログを収集します。

    [![](troubleshooting-images/troubleshooting-image12.png "[ヘルプ]、[Xamarin]、[ログの Zip] の順に選択し、ログを収集します")](troubleshooting-images/troubleshooting-image12.png#lightbox)

4. .zip ファイルを開くと、下のサンプルのようなファイルの一覧が表示されます。 接続エラーの場合、最も重要なファイルは **\*Ide.log** ファイルと **\*Ide.svclog** ファイルです。 このファイルには、同じメッセージが微妙に異なる 2 つの形式で入っています。 **.svclog** は XML であり、メッセージを拾い読みする場合に便利です。 **.log** はプレーン テキストであり、コマンド ライン ツールでメッセージをフィルター処理する場合に便利です。

    すべてのメッセージに目を通すには、**.svclog** ファイルを選択し、開きます。

    [![](troubleshooting-images/troubleshooting-image13.png "svclog ファイルを選択します")](troubleshooting-images/troubleshooting-image13.png#lightbox)

5. **.svclog** ファイルを **Microsoft Service Trace Viewer** で開きます。 メッセージの関連グループを見るには、スレッド別にメッセージを参照します。 スレッド別に閲覧するには、最初に **[グラフ]** タブを選択し、**[レイアウト モード]** ドロップダウン メニューをクリックし、**[スレッド]** を選択します。

    [![](troubleshooting-images/troubleshooting-image14.png "[レイアウト モード] ドロップダウン メニューをクリックし、[スレッド] を選択します")](troubleshooting-images/troubleshooting-image14.png#lightbox)

<a name="verboselogs" />

### <a name="verbose-log-files"></a>詳細ログ ファイル

通常のログ ファイルでは問題を診断するために必要な情報が十分に得られない場合、最後の手段として詳細ログを有効にしてみます。 詳細ログはバグ レポートでも好まれます。

1. Visual Studio を終了します。

2. [**開発者コマンド プロンプト**](https://msdn.microsoft.com/en-us/library/ms229859(v=vs.110).aspx)を起動します。

3. コマンド プロンプトで次のコマンドを実行し、詳細ログありで Visual Studio を起動します。

    ```bash
    devenv /log
    ```

4. Visual Studio からビルド ホストに接続してみます。

5. Visual Studio で接続エラーが表示されたら、**[ヘルプ]、[Xamarin]、[ログの Zip]** の順に選択し、ログを収集します。

6. Mac のターミナルで次のコマンドを実行し、最近のログ メッセージがあれば、それを SSH サーバーからデスクトップ上のファイルにコピーします。

    ```bash
    grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"
   ```

詳細ログでも問題を直接解決するために必要な情報が十分に得られない場合、[新しいバグ レポートを提出](https://bugzilla.xamarin.com/newbug)し、手順 5 の .zip ファイルと手順 6 の .log ファイルを両方添付してください。

## <a name="troubleshooting-build-and-deployment-errors"></a>ビルドと配置エラーの解決

このセクションでは、Visual Studio でビルド ホストに接続できた後に発生する問題についていくつか取り上げます。

### <a name="unable-to-connect-to-address1921681222-with-usermacuser"></a>"Unable to connect to Address='192.168.1.2:22' with User='macuser'" (User='macuser' で Address='192.168.1.2:22' に接続できません)

既知の原因:

- **Xamarin 4.1 セキュリティ機能** – このエラーは、Xamarin 4.1 以降の使用後に Xamarin 4.0 にダウングレードした場合に_発生します_。 この場合、このエラーと共に "Private key is encrypted but passphrase is empty" (秘密キーが暗号化されていますが、パスフレーズが空です) という警告も表示されます。 これは Xamarin 4.1 の新しいセキュリティ機能に起因する_意図的な_変更です。 **推奨される解決策**: **%LOCALAPPDATA%\Xamarin\MonoTouch** から **id\_rsa** と **id\_rsa.pub** を削除し、Mac ビルド ホストに再接続します。

- **SSH セキュリティの制限** – このメッセージと共に "Could not authenticate the user using the existing ssh keys" (既存の ssh キーでユーザーを認証できませんでした) という警告も表示されるとき、それは多くの場合、Mac の完全修飾パス **$HOME/.ssh/authorized\_keys** にあるファイルまたはディレクトリの 1 つで _[その他]_ メンバーまたは _[グループ]_ メンバーに対して書き込みアクセス許可が有効になっていることを意味します。 **一般的な修正方法**: Mac のターミナル コマンド プロンプトで `chmod og-w "$HOME"` を実行します。 特定のファイルまたはディレクトリが問題を引き起こしている場合の詳細については、ターミナルで `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` を実行し、デスクトップから **sshd.log** ファイルを開き、"Authentication refused: bad ownership or modes" を探します。

### <a name="solutions-cannot-be-loaded-from-a-network-share"></a>ネットワーク共有からソリューションを読み込めない

ソリューションは、ローカル Windows ファイル システムかマッピングされているドライブにある場合にのみコンパイルされます。

ネットワーク共有に保存されているソリューションはエラーをスローするか、コンパイルを完全に拒否することがあります。 Visual Studio で使用されている **.sln** ファイルはローカル Windows ファイル システムに保存してください。

この問題に起因して次のエラーがスローされます。

```bash
error : Building from a network share path is not supported at the moment. Please map a network drive to '\\SharedSources\HelloWorld\HelloWorld' or copy the source to a local directory.
```

関連バグ: [#36195](https://bugzilla.xamarin.com/show_bug.cgi?id=36195)

### <a name="missing-provisioning-profiles-or-failed-to-create-the-a-fat-library-error"></a>プロビジョニング プロファイルの不足または "Failed to create the a fat library" (fat ライブラリを作成できませんでした) エラー

Mac で Xcode を起動し、Apple 開発者アカウントでログインしており、iOS 開発プロファイルがダウンロードされていることを確認します。

[![](troubleshooting-images/troubleshooting-image7.png "Apple 開発者アカウントでログインしており、iOS 開発プロファイルがダウンロードされていることを確認します")](troubleshooting-images/troubleshooting-image7.png#lightbox)

### <a name="a-socket-operation-was-attempted-to-an-unreachable-network"></a>"到達できないネットワークでソケット操作を実行しようとしました"

報告されている原因:

- **拡張 [#36118](https://bugzilla.xamarin.com/show_bug.cgi?id=36118)** – Visual Studio でビルド ホストへの接続に IPv6 アドレスを使用しているとき、このエラーでビルドに失敗します。 (ビルド ホスト接続はまだ IPv6 アドレスに対応していません。)

### <a name="xamarinios-visual-studio-plugin-fails-to-load-after-reinstallation-of-betaalpha-channel"></a>ベータ/アルファ チャネルの再インストール後、Xamarin.iOS Visual Studio プラグインが読み込みに失敗する

関連バグ [#40781](https://bugzilla.xamarin.com/show_bug.cgi?id=40781)

この問題は、Visual Studio で MEF コンポーネント キャッシュを更新できなかったときに発生することがあります。 その場合、この Visual Studio 拡張機能 ([https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)) をインストールすると解決することがあります。

Visual Studio MEF コンポーネント キャッシュが消去され、キャッシュ破損の問題が解消されます。

<a name="errors" />

### <a name="errors-due-to-existing-build-host-processes-on-the-mac"></a>Mac 上の既存のビルド ホスト プロセスに起因するエラー

以前のビルド ホスト接続のプロセスが現在アクティブな接続の動作を邪魔することがあります。 プロセスが残っていないか確認するには、Visual Studio を閉じ、Mac のターミナルで次のコマンドを実行します。

```bash
ps -A | grep mono
```

[![](troubleshooting-images/troubleshooting-image10.png "Mac のターミナルでのコマンドの実行")](troubleshooting-images/troubleshooting-image10.png#lightbox)

既存のプロセスを終了するには、次のコマンドを使用します。

```bash
killall mono
```

### <a name="clearing-the-mac-build-cache"></a>Mac ビルド キャッシュを消去する

ビルドの問題を解決しているとき、Mac に保存されている一時的なビルド ファイルに動作が関連していないことを確認するには、ビルド キャッシュ フォルダーを削除します。

1. Mac のターミナルで次のコマンドを実行します。

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```

2. **mtbs** フォルダーをコントロール クリックし、**[ごみ箱に入れる]** を選択します。

    [![](troubleshooting-images/troubleshooting-image9.png "mtbs フォルダーをごみ箱に移動します")](troubleshooting-images/troubleshooting-image9.png#lightbox)


## <a name="related-links"></a>関連リンク

- [Mac への接続](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [XMA を使用する Visual Studio 環境への Mac の接続 (ビデオ)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
