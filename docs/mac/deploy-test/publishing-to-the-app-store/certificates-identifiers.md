---
title: Xamarin.Mac の証明書と ID
description: このガイドでは、Xamarin.Mac アプリを発行するために必要な証明書と ID を作成する手順について説明します。
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/17/2019
ms.openlocfilehash: ae62a695670f50c5385b9279c6d6de79b6d59fb7
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697567"
---
# <a name="certificates-and-identifiers-in-xamarinmac"></a>Xamarin.Mac の証明書と ID

_このガイドでは、Xamarin.Mac アプリを発行するのに必要な証明書と ID を作成する手順について説明します。_

## <a name="setup"></a>セットアップ

開発用に Mac を構成するために、「[Apple Developer Member Center](https://developer.apple.com)」 (Apple 開発者メンバー センター) に進みます。 **[Account]\(アカウント\)** リンクをクリックしてサインインします。 メイン メニューは、次のとおりです。

> [!div class="mx-imgBorder"]
> [![Apple Developer Member Center](certificates-identifiers-images/devcenter01.png)](certificates-identifiers-images/devcenter01-large.png#lightbox)

**[Certificates, Identifiers & Profiles]\(証明書、ID、プロファイル\)** ボタン (または見出し **[Certificates]\(証明書\)** の横にあるプラス ボタン) をクリックします。

> [!div class="mx-imgBorder"]
> [![[Certificates, Identifiers & Profiles]\(証明書、ID、プロファイル\) を選択する](certificates-identifiers-images/devcenter02.png)](certificates-identifiers-images/devcenter02-large.png#lightbox)

証明書タイプを選択して、 **[Continue]\(続ける\)** をクリックします。

> [!div class="mx-imgBorder"]
> [![[Certificates]\(証明書\) リンクを選択する](certificates-identifiers-images/devcenter03.png)](certificates-identifiers-images/devcenter03-large.png#lightbox)

ここから、必要に応じて **中間証明書** (Worldwide Developer Relations Certificate Authority と Developer ID Certificate Authority) をダウンロードします (ページの下部にある最後の項目)。 ただし、これらは開発者用に Xcode によって自動設定されるはずです。

このセクションの残りの部分では、Mac 開発者に関連する次のセクションについて順に説明します。

- **Mac App ID を登録する**: アプリケーション開発者は、記述するすべてのアプリケーションで、これらの手順に従う必要があります。
- **macOS システムを登録する**: これは、テストするコンピューターを追加するときのみ必要です。
- **証明書を作成する**: 証明書の設定時に 1 回、その後は証明書を更新するときのみ必要です。
- **プロビジョニング プロファイルを作成する**: 開発者は新しいアプリケーションを記述するたびに、および新しいシステムを追加するたびに、これらの手順に従う必要があります。

## <a name="register-mac-app-id"></a>Mac App ID を登録する

アプリケーションごとにアプリ ID を登録する必要があります。 エントリを作成するには、次の手順に従います。

1. "+" (プラス記号) または **[Register an App ID]\(App ID を登録する\)** を押します。

    > [!div class="mx-imgBorder"]
    > [![スクリーンショットには、[Certificates, Identifiers and Profiles]\(証明書、ID、プロファイル\) の [Getting started with App IDs]\(App ID の概要\) が示されています。](certificates-identifiers-images/appid01.png)](certificates-identifiers-images/appid01-large.png#lightbox)

1. **[App IDs]** を選択します。

    > [!div class="mx-imgBorder"]
    > [![スクリーンショットには、[Register a New Identifier]\(新しい識別子の登録\) オプションが示されています。](certificates-identifiers-images/appid02.png)](certificates-identifiers-images/appid02-large.png#lightbox)

1. **[Description]\(説明\)** を入力し、アプリケーションで必要なすべての **[App Services]\(App サービス\)** を選択します。 a. プラットフォームは **macOS** である必要があります。a. **[Description]\(説明\)** を選択します (このポータルでのみ使用)。a. **Info.plist** と一致する **[Bundle ID]\(バンドル ID\)** を入力します。a. アプリで必要な機能を選択します。

    > [!div class="mx-imgBorder"]
    > [![説明と App サービスを入力する](certificates-identifiers-images/appid03.png)](certificates-identifiers-images/appid03-large.png#lightbox)

    **[Continue]\(続ける\)** を押して、選択した内容を確認します。

1. 情報が正しい場合は、 **[Register]\(登録\)** をクリックしてセットアップを完了します。

    > [!div class="mx-imgBorder"]
    > [![入力したデータを確認する](certificates-identifiers-images/appid04.png)](certificates-identifiers-images/appid04-large.png#lightbox)

1. 情報を確認し、 **[Submit]\(送信\)** ボタンをクリックします。

    > [!div class="mx-imgBorder"]
    > ![情報を確認する](certificates-identifiers-images/appid05.png)

(iCloud など) 一部の **App Services** は、さらに構成する必要がある場合があります。 その場合は、作成したばかりの新しい App ID を選択し、 **[Edit]\(編集\)** ボタンをクリックします。

> [!div class="mx-imgBorder"]
> [![新しい App ID を編集する](certificates-identifiers-images/appid06.png)](certificates-identifiers-images/appid06-large.png#lightbox)

たとえば、iCloud サービスを構成するには、 **[Edit]\(編集\)** ボタンをクリックします。

> [!div class="mx-imgBorder"]
> [![iCloud サービスを構成する](certificates-identifiers-images/appid07.png)](certificates-identifiers-images/appid07-large.png#lightbox)

## <a name="register-macos-devices"></a>macOS デバイスを登録する

テスト用にプロビジョニング プロファイルを作成するには、開発者はその Mac コンピューターを登録する必要があります。 テスト用として最大 100 台のコンピューターを登録できます。

1. Mac Developer Center の **[Devices]\(デバイス\)** セクションで **[All]\(すべて\)** を選択し、 **+** ボタンをクリックします。

    > [!div class="mx-imgBorder"]
    > [![新しいコンピューターを追加する](certificates-identifiers-images/device01.png)](certificates-identifiers-images/device01-large.png#lightbox)

1. 追加するコンピューターの **[Name]\(名前\)** と **[UUID]** を入力し、 **[Continue]\(続行\)** ボタンをクリックします。 情報を確認して、 **[Register]\(登録\)** ボタンをクリックします。

    > [!div class="mx-imgBorder"]
    > [![スクリーンショットには、名前と UUID を入力できる [Register a New Device]\(新しいデバイスの登録\) ページが示されています。](certificates-identifiers-images/device02.png)](certificates-identifiers-images/device02-large.png#lightbox)

1. 入力したデータをレビューして確認します。

    > [!div class="mx-imgBorder"]
    > [![スクリーンショットには、名前と UUID を確認できる [Register a New Device]\(新しいデバイスの登録\) ページが示されています。](certificates-identifiers-images/device03.png)](certificates-identifiers-images/device03-large.png#lightbox)

## <a name="create-certificates"></a>証明書を作成する

Mac アプリケーションの署名に使用するさまざまな種類の証明書を作成するには、[Certificates]\(証明書\) セクションを使用します。

> [!div class="mx-imgBorder"]
> [![新しい証明書の作成](certificates-identifiers-images/devcenter04.png)](certificates-identifiers-images/devcenter04-large.png#lightbox)

macOS の開発に関連する証明書には、主として次の 5 つの種類があります。

- **Mac Development**: 一般的なアプリ開発ではオプションですが、開発者が iCloud やプッシュ通知などの機能を使用する予定がある場合は必須です。 これらの機能にアクセスするプロビジョニング プロファイルを作成する前に、開発者には Development 証明書が必要です。
- **Mac App Distribution**: 開発者には、アプリ用の証明書と、インストーラー用の別の証明書が必要です。
- **Mac Installer Distribution**: 開発者には、アプリ用の証明書と、インストーラー用の別の証明書が必要です。
- **Developer ID Installer**: Mac App Store 以外の場所で配信するためのインストーラー用の証明書。
- **Developer ID Application**: Mac App Store 以外の場所で配信するためのアプリ用の証明書。

次のセクションでは、上記の証明書の一部について、作成方法の例を紹介します。

### <a name="mac-development-certificate"></a>Mac Development の証明書

前述のとおり、Mac Development の証明書は、iCloud やプッシュ通知などの macOS の機能が必要でなければ不要です。

新しい Development の証明書を作成するには、次を実行します。

1. **[Mac Development]\(Mac Development\)** ラジオ ボタンを選択し、 **[Continue]\(続行\)** をクリックします。

    > [!div class="mx-imgBorder"]
    > [![開発証明書を追加する](certificates-identifiers-images/certif02.png)](certificates-identifiers-images/certif02-large.png#lightbox)

1. _証明書署名リクエスト_ をアップロードします。 証明書署名リクエスト ファイル (拡張子 `.certSigningRequest`) は、Mac にローカルに保存されます。 **[Choose file]\(ファイルを選択\)** をクリックして証明書リクエストを選択し、 **[Continue]\(続ける\)** を押します。

    > [!div class="mx-imgBorder"]
    > [![証明書リクエスト ファイルをアップロードする](certificates-identifiers-images/certif03.png)](certificates-identifiers-images/certif03-large.png#lightbox)

    **キーチェーン アクセス** を使用して証明書リクエスト ファイルを作成する方法については、「[証明書署名リクエストを作成する](https://help.apple.com/developer-account/#/devbfa00fef7)」を参照してください。

1. **[Download]\(ダウンロード\)** を押して証明書ファイルを取得し、それをダブルクリックしてインストールします。

    > [!div class="mx-imgBorder"]
    > [![証明書ファイルをダウンロードする](certificates-identifiers-images/certif04.png)](certificates-identifiers-images/certif04-large.png#lightbox)

前述のとおり、Developer 証明書は、開発者が iCloud やプッシュ通知などの macOS 機能を実装しない限り不要です。 Mac App Store アプリをテストするのに必要な **開発用プロビジョニングプロファイル** の作成にも必要になります。

### <a name="mac-app-store-certificates"></a>Mac App Store の証明書

App Store でアプリをリリースするには、次の 2 つの証明書が必要です。

- **Mac App Distribution** 証明書: アプリケーションに署名するために使用されます。
- **Mac Installer Distribution** 証明書: インストーラーに署名するために使用されます。

> [!TIP]
> これらのキーの証明書の要求を命名する際、後で識別できるよう、`Application` と `Installer` のテキストを含むわかりやすい名前を使用します。

最初に、インストーラー証明書を作成します。

1. 証明書タイプとして **[Mac Installer Distribution]** を選択して、 **[Continue]\(続ける\)** をクリックします。

    > [!div class="mx-imgBorder"]
    > [![App Store の証明書を作成する](certificates-identifiers-images/certif05.png)](certificates-identifiers-images/certif05-large.png#lightbox)

1. 次のページでは、**キーチェーン アクセス** を使用して証明書の要求ファイルを生成する方法を説明します。 次の手順に従ってください。

    > [!div class="mx-imgBorder"]
    > [![証明書リクエストをアップロードする](certificates-identifiers-images/certif06.png)](certificates-identifiers-images/certif06-large.png#lightbox)

    **キーチェーン アクセス** を使用して証明書リクエスト ファイルを作成する方法については、「[証明書署名リクエストを作成する](https://help.apple.com/developer-account/#/devbfa00fef7)」を参照してください。 証明書の _タイプ_ (アプリケーションまたはインストーラー) を反映する証明書名を選択してください。

1. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。

    > [!div class="mx-imgBorder"]
    > [![App Store の証明書をダウンロードする](certificates-identifiers-images/certif07.png)](certificates-identifiers-images/certif07-large.png#lightbox)

**Mac App Distribution 証明書の場合も同じ手順に従います。**

![Mac App Distribution 証明書](certificates-identifiers-images/certif08.png)

### <a name="developer-id-certificates"></a>Developer ID の証明書

Xamarin.Mac アプリケーションをセルフリリースするには (Apple App Store を介してリリースしない場合)、次の 2 つの証明書が必要です。

- **Developer ID Installer** 証明書: アプリケーションに署名するために使用されます。
- **Developer ID Application** 証明書: インストーラーに署名するために使用されます。

> [!TIP]
> これらのキーの証明書の要求を命名する際、後で識別できるよう、`Application` と `Installer` のテキストを含むわかりやすい名前を使用します。

証明書の作成、ダウンロード、インストールが完了したら、**キーチェーン アクセス** に表示されます。

[キーチェーン アクセスの証明書リスト](certificates-identifiers-images/certif09.png)

## <a name="related-links"></a>関連リンク

- [インストール](/visualstudio/mac/installation/)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID と GateKeeper](https://developer.apple.com/developer-id/)
