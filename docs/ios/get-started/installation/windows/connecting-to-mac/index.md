---
title: Mac とペアリング
description: このガイドでは、[Mac とペアリング] を使って Visual Studio 2017 を Mac ビルド ホストに接続する方法を説明します。
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: adaa74e206b1e756398f1ef1a38f387082c1e8f5
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2018
---
# <a name="pair-to-mac"></a>Mac とペアリング

_このガイドでは、[Mac とペアリング] を使って Visual Studio 2017 を Mac ビルド ホストに接続する方法を説明します。_

## <a name="overview"></a>概要

ネイティブ iOS アプリケーションのビルドには、Mac 上でのみ動作する Apple のビルド ツールにアクセスする必要があります。 このため、Xamarin.iOS アプリケーションをビルドするには、Visual Studio 2017 はネットワークからアクセス可能な Mac に接続する必要があります。

Visual Studio 2017 の [Mac とペアリング] 機能は、Windows ベースの iOS 開発者の生産性が上がるように、Mac ビルド ホストの検出、接続、認証、記憶を行います。 

[Mac とペアリング] を使うと、次のような開発ワークフローが可能になります。

- 開発者は、Visual Studio 2017 で Xamarin.iOS のコードを記述できます。

- Visual Studio 2017 は、Mac ビルド ホストへのネットワーク接続を開き、そのマシン上のビルド ツールを使って iOS アプリのコンパイルと署名を行います。

- Mac 上で別のアプリケーションを実行する必要はありません。Visual Studio 2017 は SSH 経由で Mac ビルドを安全に呼び出します。

- 変更が行われるとすぐに Visual Studio 2017 は通知を受け取ります。 たとえば、iOS デバイスが Mac に接続されたり、ネットワーク上で使用できるようになったりすると、iOS ツール バーが即座に更新されます。

- Visual Studio 2017 の複数のインスタンスを Mac に同時に接続できます。

- Windows のコマンド ラインを使って、iOS アプリケーションをビルドできます。

> [!NOTE]
> このガイドの手順の前に、次の手順を実行します。 
> 
> - Windows コンピューターで、[Visual Studio 2017 をインストール](~/cross-platform/get-started/installation/windows.md)します
> - Mac で、[Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) と [Visual Studio for Mac をインストール](https://docs.microsoft.com/visualstudio/mac/installation)します
>
> Visual Studio for Mac をインストールしない方がよい場合は、Visual Studio 2017 は Xamarin.iOS と Mono で Mac ビルド ホストを自動的に構成できます。
> 詳しくは、「[Mac の自動プロビジョニング](#automatic-mac-provisioning)」をご覧ください。

## <a name="enable-remote-login-on-the-mac"></a>Mac でリモート ログインを有効にする

Mac ビルド ホストを設定するには、まずリモート ログインを有効にします。

1. Mac で、[システム環境設定] を開き、**[共有]** ウィンドウに移動します。

2. **[サービス]** リストの **[リモート ログイン]** オプションをオンにします

    ![リモート ログインを有効にする](images/sharing.png "リモート ログインを有効にする")

    **[すべてのユーザー]** にアクセスできるように構成されていること、またはお使いの Mac のユーザー名またはグループが許可されたユーザーのリストに含まれていることを確認します。

3. メッセージが表示されたら、macOS ファイアウォールを構成します。

    着信接続をブロックするように macOS ファイアウォールを設定した場合は、`mono-sgen` による着信接続の受信を許可することが必要な場合があります。 その場合は警告が表示されます。

4. Mac が Windows コンピューターと同じネットワーク上にある場合、Visual Studio 2017 で Mac を検出できるようになります。 Mac をまだ検出できない場合は、[Mac を手動で追加](#manually-add-a-mac)してみるか、または[トラブルシューティング ガイド](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)をご覧ください。

## <a name="connect-to-the-mac-from-visual-studio-2017"></a>Visual Studio 2017 から Mac に接続する

リモート ログインが有効になったので、Visual Studio 2017 を Mac に接続します。

1. Visual Studio 2017 で、既存の iOS プロジェクトを開くか、**[ファイル] > [新規] > [プロジェクト]** の順に選択し、iOS プロジェクト テンプレートを選択して、新しいプロジェクトを作成します。

2. **[Mac とペアリング]** ダイアログを開きます。 

    - [iOS] ツール バーの **[Mac とペアリング]** ボタンを使います。

        ![[Mac とペアリング] ボタンが強調表示されている [iOS] ツール バー](images/ios-toolbar.png "[Mac とペアリング] ボタンが強調表示されている [iOS] ツール バー")

    - または、**[ツール] > [iOS] > [Mac とペアリング]** の順に選択します。

    - **[Mac とペアリング]** ダイアログに、以前に接続したことがあって現在使用可能なすべての Mac ビルド ホストの一覧が表示されます。

        ![[Mac とペアリング] ダイアログ](images/pairtomac.png "[Mac とペアリング] ダイアログ")

3. 一覧で Mac を選択します。 **[接続]** をクリックします。 

4. ユーザー名とパスワードを入力します。
    
    - 特定の Mac に初めて接続するときは、そのコンピューターのユーザー名とパスワードの入力を求められます。

        ![Mac のユーザー名とパスワードを入力する](images/auth.png "Mac のユーザー名とパスワードを入力する")

        > [!TIP]
        > ログインするときは、フル ネームではなくシステム ユーザー名を使います。

    - [Mac とペアリング] はこれらの資格情報を使用して、Mac への新しい SSH 接続を作成します。 成功した場合、Mac 上の **authorized_keys** ファイルにキーが追加されます。 同じ Mac への以降の接続は自動的にログインします。

5. [Mac とペアリング] により自動的に Mac が構成されます。

    [Visual Studio 2017 バージョン 15.6 以降では](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning)、Visual Studio 2017 は接続された Mac ビルド ホスト上の Mono と Xamarin.iOS を、必要に応じてインストールまたは更新します (ただし、Xcode はやはり手動でインストールする必要があることに注意してください)。 詳しくは、「[Mac の自動プロビジョニング](#automatic-mac-provisioning)」をご覧ください。

6. 接続状態アイコンを確認します。
    
    - Visual Studio 2017 が Mac に接続されていると、**[Mac とペアリング]** ダイアログのその Mac の項目に、現在接続されていることを示すアイコンが表示されます。

        ![接続された Mac](images/connected.png "接続された Mac")

      接続できる Mac は一度に 1 台だけです。

      > [!TIP]
      > **[Mac とペアリング]** の一覧で Mac を右クリックすると表示されるコンテキスト メニューでは、**[接続...]**、**[この Mac を記憶しない]**、**[切断]** を選択できます。
      >
      > ![[Mac とペアリング] のコンテキスト メニュー](images/contextmenu.png "[Mac とペアリング] のコンテキスト メニュー") 
      >
      > **[この Mac を記憶しない]** を選んだ場合、選択した Mac に対する資格情報は保持されません。 その Mac に再接続するには、ユーザー名とパスワードを再入力する必要があります。

Mac ビルド ホストとのペアリングが成功すると、Visual Studio 2017 で Xamarin.iOS アプリをビルドできるようになります。 「[Xamarin.iOS for Visual Studio の概要](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)」ガイドをご覧ください。

Mac とペアリングできない場合は、[Mac を手動で追加](#manually-add-a-mac)してみるか、または[トラブルシューティング ガイド](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)をご覧ください。

## <a name="manually-add-a-mac"></a>Mac を手動で追加する

**[Mac とペアリング]** ダイアログの一覧に特定の Mac が表示されない場合は、手動で追加します。

1. Mac の IP アドレスを確認します。 

    - Mac で **[システム環境設定] > [共有] > [リモート ログイン]** を開きます。

        [![[システム環境設定] > [共有] で表示される Mac の IP アドレス](images/sharing-ipaddress.png "[システム環境設定] > [共有] で表示される Mac の IP アドレス")](images/sharing.png#lightbox)

    - または、コマンド ラインを使用します。 ターミナルで次のコマンドを実行します。 
   
        ```bash
        $ ipconfig getifaddr en0
        196.168.1.8
        ```

      ネットワークの構成によっては、`en0` ではなくインターフェイス名を使用する必要があります。 たとえば、`en1`、`en2` などです。

2. Visual Studio 2017 の **[Mac とペアリング]** ダイアログで、**[Mac の追加...]** を選びます。

    [![[Mac とペアリング] ダイアログの [Mac の追加] ボタン](images/addtomac.png "[Mac とペアリング] ダイアログの [Mac の追加] ボタン")](images/addtomac-large.png#lightbox)

3. Mac の IP アドレスを入力して、**[追加]** をクリックします。

    ![Mac の IP アドレスの入力](images/enteripaddress.png "Mac の IP アドレスの入力")

4. Mac のユーザー名とパスワードを入力します。

    ![ユーザー名とパスワードを入力する](images/auth.png "ユーザー名とパスワードを入力する")

   > [!TIP]
   > ログインするときは、フル ネームではなくシステム ユーザー名を使います。

5. **[ログイン]** をクリックして Visual Studio 2017 を SSH 経由で Mac に接続し、既知のマシンの一覧に追加します。

## <a name="automatic-mac-provisioning"></a>Mac の自動プロビジョニング

[Visual Studio 2017 バージョン 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) 以降では、[Mac とペアリング] により、Xamarin.iOS アプリケーションのビルドに必要なソフトウェア (Mono、Xamarin.iOS (Visual Studio for Mac IDE ではないソフトウェア フレームワーク)、およびさまざまな Xcode 関連ツール (ただし、Xcode 自体は含まれません)) で Mac が自動的にプロビジョニングされます。

> [!IMPORTANT]
> - [Mac とペアリング] は Xcode をインストールできません。手動で Mac ビルド ホストにインストールする必要があります。 これは Xamarin.iOS の開発に必要です。
> - Mac の自動プロビジョニングには、Mac でリモート ログインが有効になっている必要があり、Mac はネットワーク経由で Windows コンピューターにアクセスできる必要があります。 詳しくは、「[Mac でリモート ログインを有効にする](#enable-remote-login-on-the-mac)」をご覧ください。

Visual Studio 2017 が [Mac に接続している](#connect-to-the-mac-from-visual-studio-2017)場合、[Mac とペアリング] は必要なソフトウェアのインストール/更新を実行します。

### <a name="mono"></a>Mono

[Mac とペアリング] は、Mono がインストールされていることを確認します。 インストールされていない場合、[Mac とペアリング] は最新の安定バージョンの Mono をダウンロードして Mac にインストールします。 

次のスクリーンショットで示すように (クリックすると拡大します)、さまざまなプロンプトで進行状況が示されます。

||インストールのチェック|ダウンロード中|インストール
|---|---|---|---|
|Mono|[![Mono がインストールされていない](images/mono-missing.png "Mono がインストールされていない")](images/mono-missing-large.png#lightbox)|[![Mono のダウンロード中](images/mono-downloading.png "Mono のダウンロード中")](images/mono-downloading-large.png#lightbox)|[![Mono のインストール中](images/mono-installing.png "Mono のインストール中")](images/mono-installing-large.png#lightbox)|

### <a name="xamarinios"></a>Xamarin.iOS 

[Mac とペアリング] は、Windows コンピューターにインストールされているバージョンと一致するように、Mac 上の Xamarin.iOS をアップグレードします。

> [!IMPORTANT]
> [Mac とペアリング] は、Mac 上の Xamarin.iOS をアルファ/ベータから安定にダウングレードすることはありません。 Visual Studio for Mac をインストールした場合は、[リリース チャネル](https://docs.microsoft.com/visualstudio/mac/update)を次のように設定します。
> - Visual Studio 2017 を使用している場合は、Visual Studio for Mac で **[安定]** 更新チャネルを選択します。
> - Visual Studio 2017 Preview を使用している場合は、Visual Studio for Mac で **[アルファ]** 更新チャネルを選択します。

次のスクリーンショットで示すように (クリックすると拡大します)、さまざまなプロンプトで進行状況が示されます。

||インストールのチェック|ダウンロード中|インストール
|---|---|---|---|
|Xamarin.iOS|[![Xamarin.iOS がインストールされていない](images/xamios-missing.png "Xamarin.iOS がインストールされていない")](images/xamios-missing-large.png#lightbox)|[![Xamarin.iOS のダウンロード中](images/xamios-downloading.png "Xamarin.iOS のダウンロード中")](images/xamios-downloading-large.png#lightbox)|[![Xamarin.iOS のインストール中](images/xamios-installing.png "Xamarin.iOS のインストール中")](images/xamios-installing-large.png#lightbox)|

### <a name="xcode-tools-and-license"></a>Xcode ツールとライセンス

[Mac とペアリング] は、Xcode がインストールされ、そのライセンスが同意されているかどうかも調べて判断します。 [Mac とペアリング] では Xcode はインストールされませんが、次のスクリーンショットで示すように (クリックすると拡大します)、ライセンス同意のプロンプトは表示されます。

||インストールのチェック|ライセンスの同意|
|---|---|---|
|Xcode|[![Xcode がインストールされていない](images/xcode-missing.png "Xcode がインストールされていない")](images/xcode-missing-large.png#lightbox)|[![Xcode のライセンス](images/xcode-license.png "Xcode のライセンス")](images/xcode-license-large.png#lightbox)|

さらに、[Mac とペアリング] は Xcode と共に配布されるさまざまなパッケージをインストールまたは更新します。 例:

- **MobileDeviceDevelopment.pkg**
- **XcodeExtensionSupport.pkg**
- **MobileDevice.pkg**
- **XcodeSystemResources.pkg**

これらのパッケージのインストールは迅速に行われ、プロンプトは表示されません。

> [!NOTE]
> これらのツールは、Xcode コマンド ライン ツールとは異なります。macOS 10.9 の時点で、Xcode コマンド ライン ツールは [Xcode と共にインストール](https://developer.apple.com/library/content/technotes/tn2339/_index.html)されます。

### <a name="troubleshooting-automatic-mac-provisioning"></a>Mac の自動プロビジョニングのトラブルシューティング

Mac の自動プロビジョニングを使用していて問題が発生した場合は、Visual Studio 2017 IDE ログ (**%LOCALAPPDATA%\Xamarin\Logs\15.0**) を確認してください。 これらのログには、障害を診断したりサポートを受けたりするときに役立つエラー メッセージが含まれる可能性があります。

## <a name="build-ios-apps-from-the-windows-command-line"></a>Windows コマンド ラインから iOS アプリをビルドする 

[Mac とペアリング] は、コマンド ラインからの Xamarin.iOS アプリケーションのビルドをサポートしています。 例:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```
上記の例で `msbuild` に渡されるパラメーターは次のとおりです。

- `ServerAddress` – Mac ビルド ホストの IP アドレス。
- `ServerUser` – Mac ビルド ホストにログインするときに使用するユーザー名。
  フル ネームではなくシステム ユーザー名を使います。
- `ServerPassword` – Mac ビルド ホストにログインするときに使用するパスワード。
 
> [!NOTE]
> Visual Studio 2017 は、`msbuild` を **C:\Program Files (x86)\Microsoft Visual Studio\2017\<バージョン>\MSBuild\15.0\Bin** ディレクトリに格納します。

[Mac とペアリング] は、Visual Studio 2017 またはコマンド ラインから特定の Mac ビルド ホストに初めてログインするときに、SSH キーを設定します。 これらのキーがあると、将来のログインではユーザー名またはパスワードは必要ありません。 新しく作成されたキーは、**%LOCALAPPDATA%\Xamarin\MonoTouch** に格納されます。

コマンド ラインでのビルドの呼び出しで `ServerPassword` パラメーターを省略すると、[Mac とペアリング] は保存されている SSH キーを使って Mac ビルド ホストへのログインを試みます。

## <a name="summary"></a>まとめ

この記事では、[Mac とペアリング] を使用して Visual Studio 2017 を Mac ビルド ホストに接続し、Visual Studio 2017 の開発者が Xamarin.iOS でネイティブ iOS アプリケーションをビルドできるようにする方法を説明しました。

## <a name="next-steps"></a>次の手順

- [接続のトラブルシューティング](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin Mac ビルド エージェント - Xamarin University Lightning Lecture](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
- [Xamarin.iOS for Visual Studio の概要](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
