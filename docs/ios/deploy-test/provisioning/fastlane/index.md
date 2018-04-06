---
title: iOS 向け fastlane の概要
description: このガイドでは、iOS アプリケーションのコード署名で使用できるさまざまな fastlane ツールを紹介します。
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 68c252edecc4ebffb764c0de328ab605975471c4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-fastlane-for-ios"></a>iOS 向け fastlane の概要

_このガイドでは、iOS アプリケーションのコード署名で使用できるさまざまな fastlane ツールを紹介します_

fastlane はオープン ソース プロジェクトであり、iOS および Android アプリのリリースの、わかりにくく煩雑に感じられることが多いプロセスを簡略化するために作成されています。 以下のようなアプリ リリースの特定の側面をそれぞれ処理するいくつかのユーティリティで構成されています。

- [deliver](https://github.com/fastlane/fastlane/tree/master/deliver#readme) – スクリーン ショット、メタデータ、およびアプリケーション バンドルを管理し、iTunes Connect にアップロードします。
- [produce](https://github.com/fastlane/fastlane/tree/master/produce#readme) – iTunes Connect および開発者ポータル (AppID としてよく知られる) でアプリを作成します。 これには、アプリ グループとアプリケーション サービスのサポートも含まれます。
- [pem](https://github.com/fastlane/fastlane/tree/master/pem#readme) – プッシュ通知とプロビジョニング プロファイルを作成および管理します。
- [gym](https://github.com/fastlane/fastlane/tree/master/gym#readme) – iOS アプリケーションのビルドと署名に使用できます  (Xamarin アプリでは既に MSBuild を使用して、アプリのビルド、署名、およびアーカイブが行われています)。
- [cert](https://github.com/fastlane/fastlane/tree/master/cert#readme) – コード署名証明書を作成および管理します。 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme) – プロビジョニング プロファイルを作成および管理します。
- [match](https://github.com/fastlane/fastlane/tree/master/match#readme) – 証明書とプロファイルを作成および維持し、git リポジトリに格納して、開発チーム間で同期できるようにします。

fastlane はさまざまな方法で使用できます。たとえば、ターミナル コマンドを使用したり、ファイル ベースの方法を使用したり、継続的インテグレーション ビルドの環境変数を使用します。 

このガイドでは特に iOS アプリでの開発用のデバイスの設定について説明し、**cert**、**sigh** および **match** ユーティリティに焦点を当てています。 

提供されるコンテンツは、アプリの配布を支援する出発点として使用できます。これには、継続的インテグレーション サーバーでのプロセスの完全な自動化が含まれます。 ただし、fastlane は Xcode プロジェクトをサポートするためのツールを作成するサードパーティのツールであるため、一部のツールや、`fastlane init` などのコマンドは csproj ファイルでは予期したとおり動作しない可能性があります。 fastlane の使用、その他のツール、あるいは fastlane を使用する Android のリリースについて詳しくは、[https://fastlane.tools/](https://fastlane.tools/) をご覧ください。

<a name="Installation" />

## <a name="installation"></a>インストール

1. Xcode コマンド ライン ツールが macOS コンピューターにインストールされていることを確認します。 ツールをインストールするには、ターミナルで `xcode-select --install` コマンドを使用します。 既にインストールされている場合は、次のエラーが表示されます。

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. fastlane ツールを [https://download.fastlane.tools](https://download.fastlane.tools) からダウンロードします。 

    > [!NOTE]
    > fastlane ツールは、`brew cask install fastlane` を使用して Homebrew からインストールすることも、`sudo gem install fastlane –NV` を使用して Rubygems (2.0 以上) を介してインストールすることもできます。 ただし、インストーラーを使用すれば、確実に正しい依存関係が得られます。 

3. ファイルを解凍して fastlane をインストールし、`install` 実行可能ファイルをダブルクリックします。 "ファイルは不明な開発者からのものであるため、開けない" ことを知らせるエラーが表示された場合は、[OK] を押して次の操作を行ってください。
    - Control キーを押したまま、`install` 実行可能ファイルをクリックします。 これにより、次のダイアログが表示されます。

      ![](images/fastlane-image12.png "[インストール] ダイアログ")
    
    - [OK] を押して、fastlane ツールのインストールを開始します。

4. ターミナルに次のようなダイアログでメッセージが表示されます。 `y` を押します。

  ![](images/fastlane-image13.png "ターミナルのプロンプト")
 
4. 初めて fastlane を使用する場合は `which fastlane` を実行します。 パスは次のようになります。 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

5. パスが上記と一致する場合は、作業を開始する準備ができています。

     一致しない場合は、macOS で以下のコマンドを使用して、`.bash_profile` (ホーム ディレクトリ内の非表示のプレーンテキスト ファイル) を開きます。

    ```bash
    open ~/.bash_profile
    ```

6. 次の PATH 環境変数を追加し、それを保存します。 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

7.  もう一度 `which fastlane` を実行し、パスが `/Users/[user]/.fastlane/bin` のようになっていることを確認します。


## <a name="updating-fastlane"></a>fastlane の更新

fastlane は非常にアクティブなオープン ソース プロジェクトであり、定期的に新しいリリースがプッシュされます。 新しいバージョンの fastlane が使用可能な場合は、fastlane コマンドを実行すると次のように通知されます。

[![](images/fastlane-image0.png "fastlane の更新プロンプト")](images/fastlane-image0.png#lightbox)


fastlane を新しいバージョンに更新するには、[ここ](https://download.fastlane.tools)から最新のパッケージをダウンロードし、インストール パッケージをダブルクリックして実行します。

[![](images/fastlane-image0a.png "インストール パッケージの実行")](images/fastlane-image0a.png#lightbox)


## <a name="contents"></a>目次

この一連のガイドでは、開発または配布準備の際に iOS アプリをコード署名するために fastlane で使用する一部のツールを示します。 ここでは次のツールについて説明します。

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

cert と sigh では、ローカル コンピューター上での署名証明書とプロビジョニング プロファイルの作成および管理を行います。 match では、このプロセスのさらに一歩先に進みます。 証明書とプロビジョニング プロファイルを作成および管理し、git リポジトリに格納して、開発チームのすべてのメンバーがアクセスできるようにします。 そのしくみと使用方法については、各セクションを参照してください。

## <a name="using-fastlane-tools-with-xamarin"></a>Xamarin での fastlane ツールの使用

fastlane で署名 ID とプロビジョニング プロファイルを作成したら、Visual Studio for Mac でバンドル署名オプションを簡単に設定できます。ただし、証明書と秘密キーが macOS キーチェーン内にあり、プロビジョニング プロファイルがフォルダー `~/Library/MobileDevice/Provisioning Profiles` 内にあることが前提となります。

Xamarin.iOS アプリケーションのコード署名オプションを設定するには、次のように、プロジェクト名を右クリックし、**[プロジェクト オプション]、[ビルド]、[iOS バンドル署名]** の順に選択して、署名 ID とプロビジョニング プロファイルを明示的に設定します。

[![](images/fastlane-image11.png "署名 ID とプロビジョニング プロファイルを明示的に設定します")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>関連リンク

- [fastlane ドキュメント](https://fastlane.tools/)
- [fastlane コード署名ドキュメント](https://docs.fastlane.tools/codesigning/getting-started/)
- [コード署名ガイド](https://codesigning.guide/)
