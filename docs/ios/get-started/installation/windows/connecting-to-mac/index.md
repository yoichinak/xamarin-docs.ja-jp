---
title: "Mac への接続"
description: "Xamarin.iOS for Visual Studio を使用すると、開発者は Visual Studio IDE を使って、Windows コンピューター上で iOS アプリケーションの作成、ビルド、デバッグを行うことができます。 このガイドでは、Xamarin.iOS for Visual Studio で用意されている機能と、Mac ビルド ホストへの接続を確立する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: c60927593f062c8ac9694d889ffbf581c09bab82
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-to-the-mac"></a>Mac への接続

_Xamarin.iOS for Visual Studio を使用すると、開発者は Visual Studio IDE を使って、Windows コンピューター上で iOS アプリケーションの作成、ビルド、デバッグを行うことができます。このガイドでは、Xamarin.iOS for Visual Studio で用意されている機能と、Mac ビルド ホストへの接続を確立する方法について説明します。_

Visual Studio は、SSH 経由で Mac に接続します。これにより、次のような利点が提供されます。

- Visual Studio は、ビルド エージェントを直接、起動および制御できます。 手動で開始および停止する必要がある、ユーザーに表示されるアプリケーションはなくなりました。

- Visual Studio の新しい接続マネージャーは、Mac ビルド ホストを検出、認証、および記憶します。

- すべての通信は SSH 経由で安全にトンネルされるため、必要なポート接続はポート 22 への 1 つだけです。

- 変更が行われるとすぐに Visual Studio は通知を受け取ります。 たとえば、iOS デバイスがプラグインされると、ツールバーがすぐに更新されます。

- Visual Studio の複数のインスタンスを同時に接続できます。

- 開発時に割り込んで接続されることはありません。 iOS Designer のデバッグまたは使用など、Mac が必要な操作を実行する場合、Mac への接続を求めるメッセージのみが表示されます。

Mac への接続は、ブローカーによって制御される機能の異なる部分への複数のプロセスで構成されています (例: iOS Designer エージェントおよびビルド エージェント)。 このブローカーは、Visual Studio によって制御および更新され、クラッシュする場合は、独立したプロセスのいずれかを自動的に再起動します。

次の図は、Xamarin.iOS 開発ワークフローの簡単な概要です。

[![iOS 開発ワークフロー](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  Visual Studio は、実際には、別の MSBuild プロセスを開始してプロジェクトをビルドします。 このプロセスは、Mac への新しい接続を作成します。つまり、Visual Studio がビルドを行うとき、実際には Windows から Mac に 2 つの SSH 接続が存在します。 [コマンド ライン](#commandline)からのビルドでは、作成される MSBuild プロセスは 1 つだけです。 図をわかりやすくするため、すべての接続は単に 1 つの矢印で表されています。

## <a name="requirements"></a>必要条件

Xamarin.iOS for Visual Studio は驚きの機能を実行します。これにより、開発者は、Visual Studio IDE を使って Windows コンピューター上で iOS アプリケーションの作成、ビルド、デバッグを行うことができます。 Xamarin.iOS for Visual Studio だけでこれを行うことはできません。iOS アプリケーションを作成するには、Apple のコンパイラが必要であり、配置するには、Apple の証明書とコード署名ツールが必要です。 つまり、Xamarin.iOS for Visual Studio のインストールで、これらのタスクを実行するには、Mac OS X コンピューター (*ホスト*または*ビルド ホスト*として参照されます) へのネットワーク接続が必要です。 いったん構成されると、Xamarin のツールはプロセスを可能な限りシームレスにします。

### <a name="system-requirements"></a>システム要件

システム要件は「[Windows に Xamarin.iOS をインストールする](~/ios/get-started/installation/windows/index.md#system-requirements)」ガイドで確認できます


#### <a name="compatibility"></a>互換性

> [!IMPORTANT]
>  Windows マシンで使用する Xamarin.iOS のバージョンは、接続している Mac と同じバージョンである必要があります。 これは、次の方法で確認できます。                                                    
>                                                                                                                 
> - **Visual Studio 2015 以前の場合**: Visual Studio for Mac と同じ[更新チャネル](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)を使用していることを確認します。
>                                                                                                                 
> - **Visual Studio 2017、リリース バージョンの場合**: Visual Studio for Mac の**安定**チャネルを使用していることを確認します。
>                                                                                                                 
> - **Visual Studio 2017、プレビュー バージョンの場合**: Visual Studio for Mac の**アルファ** チャネルを使用していることを確認します。 Visual Studio では、Xamarin.iOS SDK と Xcode の存在、および互換性のあるバージョンがあるかを確認することはありません。
>   これは、ビルド エラーを発生させるビルド エージェント、およびデザイナー エラーを発生させる iOS Designer エージェントによって確認されます。

### <a name="connecting-to-the-mac"></a>Mac への接続

#### <a name="mac-setup"></a>Mac のセットアップ

Mac ホストを設定するには、Visual Studio の Xamarin 拡張機能と Mac 間の通信を有効にする必要があります。 この操作を行うには、次の手順に従って、Mac で **[リモートログイン]** を許可します。

1. *[Spotlight]* (**⌘ - スペース キー**) を開き、*[リモートログイン]* を検索し、結果で *[共有]* を選びます。 これにより、*[共有]* パネルで *[システム環境設定]* が開きます。

   [![リモート ログインのスポットライト検索](images/spotlight.png)](images/spotlight.png#lightbox)

2. Xamarin for Visual Studio が Mac に接続することを許可するには、左にある *[サービス]* リストの *[リモートログイン]* オプションをオンにします。

   [![[サービス] リストの [リモート ログイン] オプションをオンにします](images/sharing.png)](images/sharing.png#lightbox)

3. *[リモートログイン]* のアクセス許可が *[すべてのユーザー]* に設定されているか、使用している Mac のユーザー名またはグループが右側のリストの許可されたユーザーのリストに含まれていることを確認します。

さらに、署名されたアプリケーションを既定でブロックするように OS X ファイアウォールを設定している場合は、`mono-sgen` による着信接続の受信を許可することが必要な場合があります。 その場合、警告ダイアログが表示されます。

Mac 上で現在開いているセッションがあれば、同じネットワーク上の場合、Visual Studio によって検出されます。

Visual Studio によって Mac 上のエージェントが起動および停止されるため、ユーザーとして他に実行する必要がある操作はありません。

### <a name="windows-setup"></a>Windows のセットアップ

必ず Windows マシンに Xamarin ツールを[インストール](~/ios/get-started/installation/windows/index.md)します。

### <a name="connecting-to-the-mac-build-host"></a>Mac Build Host への接続

Mac ビルド ホストへの接続には、2 つの方法があります。

iOS ツール バー:

[![iOS ツール バー](images/image1.png)](images/image1.png#lightbox)

または、Visual Studio で **[ツール] > [オプション]** を参照し、**[Xamarin] > [iOS の設定]** を選択して、**[Xamarin Mac Agent の検索]** ボタンをクリックします。

[![Xamarin Mac Agent の検索](images/image2.png)](images/image2.png#lightbox)

どちらかの方法で移動すると、次に示すように、**[Mac Agent]** ダイアログが表示されます。

[![[Mac Agent] ダイアログ](images/image3.png)](images/image3.png#lightbox)

これにより、以前に接続されて既知のマシンとして保存されているマシン、または *[リモートログイン]* を利用できるマシンのすべてのリストが表示されます。

接続するマシンをダブルクリックして、Mac を選択します。 最初に Mac に接続すると、リモート接続を許可するために、Mac ユーザーの資格情報を求めるメッセージが表示されます。

[![Mac ユーザーの資格情報を入力します](images/image4.png)](images/image4.png#lightbox)

エージェントはこれらの資格情報を使用して、Mac への新しい SSH 接続を作成します。 成功すると、SSH キーが作成され、Mac の `authorized_keys` ファイルに[登録](#commandline)されます。 後続の接続で、エージェントは、最後に接続された既知のビルド ホストに接続するために、そのユーザー名とキー ファイルを使用します。

> [!NOTE]
>  **注:**資格情報を入力するときは、_フル ネーム_ではなく、_ユーザー名_を使用する必要があります。  ターミナルで `whoami` コマンドを使用して、ユーザー名を見つけることができます。  たとえば、下のスクリーンショットでは、アカウント名が **Amy Burns** ではなく **amyb** になります。
>
> ![ターミナル アプリでのユーザー名の検索](images/image5.png)

接続が正常に確立されると、次に示すように、[ホストの選択] ダイアログで横に**接続済み**アイコンを示して表示されます。

[![接続済みアイコンが横に表示されている [ホストの選択] ダイアログ](images/image6.png)](images/image6.png#lightbox)

どの時点でも 1 台の Mac のみを接続することができます。

リストの各マシンは、接続されているかに関係なく、右クリックでコンテキスト メニューを表示し、必要に応じて、**[接続]**、**[切断]**、または **[この Mac を記憶しない]** を選択できます。

[![[接続]、[切断]、または [この Mac を記憶しない] コンテキスト メニュー](images/image7.png)](images/image7.png#lightbox)

**[この Mac を記憶しない]** を選択した場合、もう一度接続するために資格情報を再入力する必要があります。

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>手動で Mac を追加

特定の状況では、[ホストの選択] ダイアログでその mDNS の名前を確認できない場合、手動で Mac を追加する必要がある可能性があります。 この操作を行うには、次の手順に従います。

1. 次のいずれかの方法で、Mac の IP アドレスを検索します。Mac 上で **[システム環境設定] > [共有] > [リモートログイン]** を参照します。

   [![[システム環境設定] の Mac の IP アドレス](images/image8.png)](images/image8.png#lightbox)

   または、コマンド ラインを使用する場合は、ターミナルに「`ipconfig getifaddr en0`」と入力して、自分の IP アドレスを確認することができます (接続の種類によって、変数が `en1`、`en2` などになる場合があります)。

   [![ターミナル アプリの IP アドレス](images/image9.png)](images/image9.png#lightbox)

2. Visual Studio に戻って、[ホストの選択] ダイアログで **[Mac の追加...]** を選択します。

   [![[ホストの選択] ダイアログ](images/image10.png)](images/image10.png#lightbox)

3. [Mac の追加] ダイアログに Mac の IP アドレスを入力し、**[追加]** をクリックします。

   [![[Mac の追加] ダイアログに Mac の IP アドレスを入力します](images/image11.png)](images/image11.png#lightbox)

4. 最後に、Mac の管理者アカウントの (フル ネームではなく) ユーザー名と対応するパスワードを入力します。

   [![ユーザー名とパスワードを入力します](images/image12.png)](images/image12.png#lightbox)

**[ログイン]** をクリックすると、Visual Studio は SSH を使用して Mac マシンにログインし、既知のマシンとしてこの Mac を追加します。

<a name="commandline"/>

### <a name="command-line-support"></a>コマンド ラインのサポート

また、エージェントでは、コマンド ラインからの Xamarin.iOS 構成のビルドもサポートします。  これを使用するには、次の MSBuild への必須パラメーターを渡す必要があります。

- `ServerAddress` – Mac サーバーの IP アドレス。

- `ServerUser` – Mac サーバーへのログインに使用するユーザー名 (フル ネームではない)。

- `ServerPassword` – Mac ホストへのログインに使用するパスワード (省略可能)。

`ServerPassword` パラメーターは必須ではありません。

代わりに、特定の Windows、Mac、およびユーザー構成のために、Visual Studio またはコマンド ラインのいずれかを使用して、初めてパスワードが渡されると、その特定のキー ペアが生成され、今後の使用のために Windows マシンに保存されます。 これは、**%localappdata%\Xamarin\MonoTouch\id_rsa** に配置されます。  `ServerPassword` パラメーターを渡さない場合は、`id_rsa` キーファイルが認証に使用されます。

**xamUser** アカウントとパスワード **mypassword** を使用して、Mac 10.211.55.2 に接続するコマンドの例を次に示します。

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>まとめ

この記事では、Visual Studio を使用して Xamarin.iOS アプリのビルドを許可し、Visual Studio と Mac 上の iOS ビルドやデザイナー ツール間を接続する方法について示しました。

### <a name="related-links"></a>関連リンク

- [インストール](~/ios/get-started/installation/windows/index.md)
- [接続のトラブルシューティング](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [XMA を使用する Visual Studio 環境への Mac の接続 (ビデオ)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
