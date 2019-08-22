---
title: fastlane for iOS - match
description: このドキュメントでは fastlane の match コマンドについて説明します。このコマンドは、iOS 開発用にコード署名証明書とプロビジョニング プロファイルを作成して保持するために使用します。
ms.prod: xamarin
ms.assetid: C4A2A67E-0643-4CED-B1A9-79D65054F3CA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 92631fa50dc4826e70df4333bb55f7f69937d053
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526547"
---
# <a name="fastlane-for-ios---match"></a>fastlane for iOS - match

従来から、デバイスのプロビジョニングは、Xcode または Apple Developer ポータルを通じて、開発チームの各メンバーによって実行されてきました。 これは次の複数の手順で構成されています。

- 開発証明書を要求する
- デバイスをポータルに追加する
- アプリ ID の作成
- プロビジョニング プロファイルを作成する
- プロファイルと証明書をダウンロードする

手動でまたは Xcode を介して開発用にデバイスをセットアップするために必要な手順の詳細については、「[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)」ガイドを参照してください。

このガイドでは、Xcode の代替としての fastlane ツールの使用法を紹介します。

## <a name="installation"></a>インストール

fastlane のインストールの詳細については、[fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) の概要に関するガイドを参照してください。

<a name="whatismatch" />

## <a name="what-is-match"></a>match とは

match はコード署名証明書およびプロプロビジョニング ファイルの作成と維持を行い、iOS 開発チームが 1 つのコード署名 ID をすべての開発者間で共有できるようにします。

アプリを App Store に展開する、ベータ テストを実施する、またはアプリをデバイスにインストールする際に、開発チームの各メンバーは、独自の署名 ID を持ちます。 これにより、ID とプロファイルの競合が発生する可能性があり、プロファイルと App ID を手動で作成、循環、管理する必要があります。

代わりに、match がすべての証明書とプロファイルを作成および維持して、プライベートの Git リポジトリに格納します。 これにより、チーム内のすべての開発者がこれらの資格情報にアクセスして使用することができます。 さらに、これにより証明書のセキュリティが強化されます。プライベートの Git リポジトリに格納されるだけでなく、[パスフレーズ](#passphrase)によって暗号化もされます。 コード署名アーティファクトをリポジトリに格納することで、チーム エージェントと管理者が必要に応じて、証明書を更新および循環させることができます。つまり、各開発者に新しい証明書を配布する時間が短縮されます。

> [!IMPORTANT]
> match では現在、In-House Enterprise プロファイルをサポートしていません。

<a name="initializing" />

## <a name="initializing-your-project-with-match"></a>match を使用したプロジェクトの初期化

チーム管理者の場合は、プライベートの Git リポジトリを作成し、github.com または bitbucket.com のいずれかを使用してすべてのチーム メンバーを共同作成者として確実にリポジトリに追加することができます。

自分の端末を使用して、ディレクトリをプロジェクト ディレクトリに変更し、次を実行します。

```
fastlane match init
```

プロンプトが表示されたら、Git リポジトリの URL を入力します。

 [![](match-images/fastlane-image7.png "Git リポジトリの URL を入力します")](match-images/fastlane-image7.png#lightbox)

この URL は、次の図のように、github.com の **[Clone or Download]\(複製またはダウンロード\)** ボタンをクリックすることで確認およびコピーできます。

[![](match-images/fastlane-image6.png "github.com の [Clone or Download]\(複製またはダウンロード\) ボタンの下の URL")](match-images/fastlane-image6.png#lightbox)

プロジェクトを初期化することで matchfile が作成されます。これは環境変数を match ツールに渡すために編集可能なテキスト ファイルです。 matchfile の例を次に示します。

[![](match-images/fastlane-image8.png "matchfile の例")](match-images/fastlane-image8.png#lightbox)

<a name="running" />

## <a name="running-match"></a>match の実行

> [!NOTE]
> fastlane では、初めて match を実行する前に、[match nuke コマンド](#using)を使用して既存のプロファイルと証明書の消去を考慮することを推奨しています。

必要な環境によっては、次のコマンドのいずれかを使用して、新しい証明書とプロビジョニング プロファイルを作成し、新しい Git リポジトリに格納することができます。

```
fastlane match appstore

fastlane match adhoc

fastlane match development
```

新しい証明書とプロファイルを作成することに加え、これらのコマンドのいずれかを使用して、次のアイテムを Git リポジトリに追加 (またはアイテムが既に存在している場合は更新) することができます。

- 証明書フォルダー
- プロファイル フォルダー
- 基本的な手順が記載された Readme
- match のバージョン

[![](match-images/fastlane-image9.png "Git リポジトリのプロジェクト構造")](match-images/fastlane-image9.png#lightbox)

プロビジョニング プロファイルは `~/Library/MobileDevice/Provisioning Profiles` にインストールされます。 証明書と秘密キーは、キーチェーンに直接インストールされます。

<a name="using" />

### <a name="using-the-nuke-command"></a>`nuke` コマンドの使用

管理がきちんとされていない証明書がある場合は、`nuke` を使用して証明書とプロファイルを取り消すことができます。各環境に対して次のコマンドを使用します。

```
fastlane match nuke
```

特定の環境のすべての証明書とプロビジョニング プロファイルを取り消す場合は、次のコマンドを使用します。

```
fastlane match nuke development
```

 or

```
fastlane match nuke distribution
```

fastlane は、削除する前に、削除されるファイルを確認します。

<a name="passphrase" />

### <a name="passphrase"></a>パスフレーズ

`match` を初めて実行する際に、Git リポジトリのパスフレーズの設定を求められます。 他のマシンで match を実行する際にパスワードが必要になるため、パスワードを忘れないようにしてください。 これは、追加のセキュリティ層です。各ファイルは OpenSSL を使用して暗号化されます。 最初にパスフレーズを入力し、ローカル キーチェーンに追加されると、その後、新しいマシンで `match` を実行するたびに、このパスフレーズの入力が求められます。

環境変数を使用してプロファイルを復号化するためのパスフレーズを設定するには、`MATCH_PASSWORD` を使用します。

<a name="options" />

## <a name="additional-options"></a>追加オプション

match を使用する際に、次のオプションを使用して追加のサポートを提供できます。

- 使用可能なすべてのコマンドのリストに `-–help` フラグを使用します。

    ```
    fastlane match cert --help
    ```

- 出力の詳細レベルを上げるには、`-–verbose` フラグを使用します。

    ```
    fastlane match --development --verbose
    ```

- Developer ポータルでのデバイス数が変更されている場合に、プロビジョニング プロファイルの更新を強制するには、`--force_for_new_devices` フラグを使用します。

    ```
    fastlane match development --force_for_new_devices
    ```

## <a name="related-links"></a>関連リンク

- [fastlane - match](https://github.com/fastlane/fastlane/blob/master/match/README.md)
