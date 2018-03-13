---
title: "証明書と ID"
description: "このガイドでは、Xamarin.Mac アプリを発行するのに必要な証明書と ID を作成する手順について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a1065fb91a23827c4876654470cda5022aa1d3b8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="certificates-and-identifiers"></a>証明書と ID

_このガイドでは、Xamarin.Mac アプリを発行するのに必要な証明書と ID を作成する手順について説明します。_

## <a name="certificates-and-identifiers"></a>証明書と ID

開発用に Mac を構成するために、「[Apple Developer Member Center](http://developer.apple.com)」 (Apple 開発者メンバー センター) に進みます。 メイン メニューは、次のとおりです。

[![Apple Developer Member Center](certificates-identifiers-images/devcenter01.png "Apple Developer Member Center")](certificates-identifiers-images/devcenter01-large.png#lightbox)

**[Certificates, Identifiers & Profiles]** (証明書、ID、プロファイル) のリンクをクリックします。

[![証明書、ID、プロファイルの選択](certificates-identifiers-images/devcenter02.png "証明書、ID、プロファイルの選択")](certificates-identifiers-images/devcenter02-large.png#lightbox)

次に、**[Mac Apps]\(Mac Apps\)** セクションの **[Certificates]\(証明書\)** リンクをクリックします。

[![証明書リンクの選択](certificates-identifiers-images/devcenter03.png "証明書リンクの選択")](certificates-identifiers-images/devcenter03-large.png#lightbox)

**[All]\(すべて\)** リンクをクリックして、**+** ボタンをクリックします。

[![すべてを選択して新しい項目を追加](certificates-identifiers-images/certif01.png "すべてを選択して新しい項目を追加")](certificates-identifiers-images/certif01-large.png#lightbox)

ここから、必要に応じて**中間証明書** (Worldwide Developer Relations Certificate Authority と Developer ID Certificate Authority) をダウンロードします。 ただし、これらは開発者用に Xcode によって自動設定されるはずです。

このセクションの残りでは、Mac Developer アカウントの設定を完了する、4 つのセクションについてそれぞれ説明します。

-   **Mac App ID を登録する**: アプリケーション開発者は、記述するすべてのアプリケーションで、これらの手順に従う必要があります。
-   **macOS システムを登録する**: これは、テストするコンピューターを追加するときのみ必要です。
-   **証明書を作成する**: 証明書の設定時に 1 回、そしてそれを更新するときのみ必要です。
-   **プロビジョニング プロファイルを作成する**: 開発者は新しいアプリケーションを記述するたびに、および新しいシステムを追加するたびに、これらの手順に従う必要があります。

このメニューに戻るには、ページの左上の **[Overview]\(概要\)** リンクをクリックします。

### <a name="register-mac-app-id"></a>Mac App ID を登録する

開発者は、作成したアプリケーションごとに App ID を登録する必要があります。 以下の手順を使用して、“MacWriter” と言う基本サンプル アプリのエントリを作成します。

1. **[App ID Description]\(App ID の説明\)** を入力し、アプリケーションで必要なすべての **[App Services]\(アプリ サービス\)** を選択します。 

    [![説明とアプリ サービスの入力](certificates-identifiers-images/devcenter04.png "説明とアプリ サービスの入力")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. アプリの **[Bundle ID]\(バンドル ID\)** を入力し、**[Continue]\(続行\)** ボタンをクリックします。 

    [![バンドル ID の入力](certificates-identifiers-images/devcenter05.png "バンドル ID の入力")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. 情報を確認し、**[Submit]\(送信\)** ボタンをクリックします。 

    [![情報の確認](certificates-identifiers-images/devcenter06.png "情報の確認")](certificates-identifiers-images/devcenter06-large.png#lightbox)

(iCloud など) 一部の **App Services** は、さらに構成する必要がある場合があります。 その場合は、作成したばかりの新しい App ID を選択し、**[Edit]\(編集\)** ボタンをクリックします。

[![新しいアプリ ID の編集](certificates-identifiers-images/devcenter07.png "新しいアプリ ID の編集")](certificates-identifiers-images/devcenter07-large.png#lightbox)

iCloud サービスを構成するには、**[Edit]\(編集\)** ボタンをクリックします。

[![iCloud サービスの構成](certificates-identifiers-images/devcenter08.png "iCloud サービスの構成")](certificates-identifiers-images/devcenter08-large.png#lightbox)

開発者は、ここから使用するデータベースを構成できます。

[![データベースの構成](certificates-identifiers-images/devcenter09.png "データベースの構成")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>macOS システムを登録する

テスト用にプロビジョニング プロファイルを作成するには、開発者はその Mac コンピューターを登録する必要があります。 Mac アプリのテストには、最大 100 台のコンピューターを登録することができます。

Mac Developer Center の **[Devices]\(デバイス\)** セクションで **[All]\(すべて\)** を選択し、**+** ボタンをクリックします。

[![新しいコンピューターの追加](certificates-identifiers-images/devcenter10.png "新しいコンピューターの追加")](certificates-identifiers-images/devcenter10-large.png#lightbox)

追加するコンピューターの **[Name]\(名前\)** と **[UUID]** を入力し、**[Continue]\(続行\)** ボタンをクリックします。 情報を確認して、**[Register]\(登録\)** ボタンをクリックします。

[![新しいコンピューター情報の入力](certificates-identifiers-images/devcenter11.png "新しいコンピューター情報の入力")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>証明書を作成する

Mac アプリケーションの署名に使用するさまざまな種類の証明書を作成するには、[Certificates]\(証明書\) セクションを使用します。

[![新しい証明書の作成](certificates-identifiers-images/certif01.png "新しい証明書の作成")](certificates-identifiers-images/certif01-large.png#lightbox)

証明書には、主に次の 3 種類があります。

-   **Mac Development の証明書**: 一般的なアプリ開発ではオプションですが、開発者が iCloud やプッシュ通知などの機能を使用する予定の場合は必須です。 これらの機能にアクセスするプロビジョニング プロファイルを作成する前に、開発者には Development 証明書が必要です。
-   **Mac App Store**: 開発者には、アプリ用の証明書と、インストーラー用の別の証明書が必要です。
-   **Developer ID**: Mac App Store 以外で配布する場合、アプリとインストーラー用の証明書が必要です。

次のセクションでは、上記のそれぞれの種類の証明書を作成する例を紹介します。

#### <a name="mac-development-certificate"></a>Mac Development の証明書

前述のとおり、Mac Development の証明書は、iCloud やプッシュ通知などの macOS の機能が必要でなければ不要です。

新しい Development の証明書を作成するには、次を実行します。

1. **[Mac Development]\(Mac Development\)** ラジオ ボタンを選択し、**[Continue]\(続行\)** をクリックします。 

     [![Development の証明書の追加](certificates-identifiers-images/certif02.png "Development の証明書の追加")](certificates-identifiers-images/certif02-large.png#lightbox)
2. 次の画面には、キーチェーン アクセスを使用して、アップロードする証明書の署名要求ファイルを作成する方法について説明します。 

    [![キーチェーン アクセスのアップロード画面](certificates-identifiers-images/certif03.png "キーチェーン アクセスのアップロード画面")](certificates-identifiers-images/certif03-large.png#lightbox)
3. 証明書が最終的に作成されたときに簡単に識別できるよう、証明書にはわかりやすい通称を付けます。 次の手順で検索できるよう、ファイルの保存場所を記憶しておきます。 

     ![証明書のエクスポート](certificates-identifiers-images/image12.png "証明書のエクスポート")
4. 証明書の要求ファイル (拡張子 `.certSigningRequest`) は、Mac のローカルに保存されます。 次の手順で選択できるように保存先を記憶しておきます (既定の場所はデスクトップです)。 

     [![証明書ファイルのアップロード](certificates-identifiers-images/image13.png "証明書ファイルのアップロード")](certificates-identifiers-images/image13-large.png#lightbox)
5. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。 

     [![Development の証明書のダウンロード](certificates-identifiers-images/image15.png "Development の証明書のダウンロード")](certificates-identifiers-images/image15-large.png#lightbox)
6. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。 証明書は、**[Developer Certificate Utility]\(開発者の証明書ユーティリティ\)** に次のように表示されます。 

     [![Developer Certificate Utility](certificates-identifiers-images/image16.png "Developer Certificate Utility")](certificates-identifiers-images/image16-large.png#lightbox)
7. **[Keychain]\(キーチェーン\)** にも次のように表示されます。 

     ![キーチェーン アクセスの証明書](certificates-identifiers-images/image17.png "キーチェーン アクセスの証明書")

前述のとおり、Developer 証明書は、開発者が iCloud やプッシュ通知などの macOS 機能を実装しない限り不要です。 Mac App Store アプリをテストするのに必要な**開発用プロビジョニングプロファイル**の作成にも必要になります。

#### <a name="mac-app-store-certificates"></a>Mac App Store の証明書

App Store にアプリをリリースするには、アプリに署名をするために必要な **Mac App Store** 証明書と Mac Installer Package を作成する必要があります。

1. 証明書の種類として **[Mac App Store]** を選択して、**[Continue]\(続行\)** ボタンをクリックします。 

    [![App Store 証明書の作成](certificates-identifiers-images/certif04.png "App Store 証明書の作成")](certificates-identifiers-images/certif04-large.png#lightbox)
2. 作成する証明書の種類を選択します (App Store にリリースするには、各種類 1 つずつ必要です)。 

    [![証明書の種類の選択](certificates-identifiers-images/certif05.png "証明書の種類の選択")](certificates-identifiers-images/certif05-large.png#lightbox)
3. 次のページでは、**キーチェーン アクセス**を使用して証明書の要求ファイルを生成する方法を説明します。 次の手順に従ってください。 

     [![キーチェーン要求の生成](certificates-identifiers-images/certif06.png "キーチェーン要求の生成")](certificates-identifiers-images/certif06-large.png#lightbox)
4. わかりやすい**通称**を選択します。たとえば、名前に “App Store Application” というテキストを使用します。 

     ![わかりやすい名前の入力](certificates-identifiers-images/image20.png "わかりやすい名前の入力")
5. 証明書の要求ファイル (拡張子 `.certSigningRequest`) は、Mac のローカルに保存されます。 保存先を記憶しておいてください (既定の場所はデスクトップです)。 

     [![証明書の保存](certificates-identifiers-images/image21.png "証明書の保存")](certificates-identifiers-images/image21-large.png#lightbox)
6. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。 

      [![App Store 証明書のダウンロード](certificates-identifiers-images/image23.png "App Store 証明書のダウンロード")](certificates-identifiers-images/image23-large.png#lightbox)
7. **[Continue]\(続行\)** をクリックして、同じ手順を正確に実行し、別の証明書を (今回は*インストーラー*用に) ダウンロードします。 

     [![インストーラーの選択](certificates-identifiers-images/image24.png "インストーラーの選択")](certificates-identifiers-images/image24-large.png#lightbox)
8. わかりやすい**通称**を選択します。たとえば、名前に “App Store Installer” というテキストを使用します。 

     ![証明書名の設定](certificates-identifiers-images/image25.png "証明書名の設定")
9. 証明書の要求ファイル (拡張子 `.certSigningRequest`) は、Mac のローカルに保存されます。 保存先を記憶しておいてください (既定の場所はデスクトップです)。 

     [![証明書のアップロード](certificates-identifiers-images/image26.png "証明書のアップロード")](certificates-identifiers-images/image26-large.png#lightbox) 

     [![配布証明書のダウンロード](certificates-identifiers-images/image28.png "配布証明書のダウンロード")](certificates-identifiers-images/image28-large.png#lightbox)
10. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。 証明書は、[Developer Certificate Utility]\(開発者の証明書ユーティリティ\)に次のように表示されます。 

     [![Developer Certificate Utility](certificates-identifiers-images/image29.png "Developer Certificate Utility")](certificates-identifiers-images/image29-large.png#lightbox)
11. 2 つの新しい証明書が、次のように**キーチェーン**で確認できるようになります。 

     [![キーチェーン アクセスの証明書](certificates-identifiers-images/image30.png "キーチェーン アクセスの証明書")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>Developer ID の証明書

(Apple App Store を介してリリースするのではなく) 自分で Xamarin.Mac アプリケーションをリリースする場合、アプリを署名してリリースするために開発者には Developer ID の証明書を必要になります。

次の手順で行います。

1. **[Certificates]\(証明書\)** セクションで **+** ボタンをクリックし、**[Developer ID]\(Developer ID\)** ラジオ ボタンを選択します。 

    [![Developer ID の追加](certificates-identifiers-images/certif07.png "Developer ID の追加")](certificates-identifiers-images/certif07-large.png#lightbox)
2. **[Continue]\(続行\)** ボタンをクリックして、作成する Developer ID の種類を選択します。 

    [![Developer ID の種類の選択](certificates-identifiers-images/certif08.png "Developer ID の種類の選択")](certificates-identifiers-images/certif08-large.png#lightbox)
3. アプリケーション自体の署名用とアプリケーションのインストーラーの署名用に 2 つが必要になります。 これらのキーの証明書の要求を命名する際、後で識別できるよう、`Application` と `Installer` のテキストを含むわかりやすい名前を使用します。
4. 次の画面には、証明書の作成方法の詳細な手順があります。**[Continue]\(続行\)** ボタンをクリックします。 

    [![証明書の作成方法](certificates-identifiers-images/certif09.png "証明書の作成方法")](certificates-identifiers-images/certif09-large.png#lightbox)
5. わかりやすい**通称**を選択します。たとえば、名前に “Developer ID Application” というテキストを使用します。 

     ![証明書名の入力](certificates-identifiers-images/image33.png "証明書名の入力")
6. 証明書の要求ファイル (拡張子 `.certSigningRequest`) は、Mac のローカルに保存されます。 保存先を記憶しておいてください (既定の場所はデスクトップです)。 

     [![証明書のアップロード](certificates-identifiers-images/certif10.png "証明書のアップロード")](certificates-identifiers-images/certif10-large.png#lightbox) 

     [![Developer ID のダウンロード](certificates-identifiers-images/certif11.png "Developer ID のダウンロード")](certificates-identifiers-images/certif11-large.png#lightbox)
7. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。
8. **[Continue]\(続行\)** をクリックして、同じ手順を正確に実行し、別の証明書を (今回は*インストーラー*用に) ダウンロードします。
9. わかりやすい**通称**を選択します。たとえば、名前に “Developer ID Installer” というテキストを使用します。 

     ![共通名の入力](certificates-identifiers-images/image38.png "共通名の入力")
10. 証明書の要求ファイル (拡張子 `.certSigningRequest`) は、Mac のローカルに保存されます。 保存先を記憶しておいてください (既定の場所はデスクトップです)。 

     [![証明書のアップロード](certificates-identifiers-images/certif10.png "証明書のアップロード")](certificates-identifiers-images/certif10-large.png#lightbox)
11. 証明書がダウンロードできるようになります。**[Done]\(完了\)** をクリックする前に、**[Download]\(ダウンロード\)** ボタンをクリックします。 

     [![証明書のダウンロード](certificates-identifiers-images/certif11.png "証明書のダウンロード")](certificates-identifiers-images/certif11-large.png#lightbox)
12. **[Download]\(ダウンロード\)** をクリックして証明書を取得し、ダブルクリックしてそれを **[Keychain]\(キーチェーン\)** にインストールします。 証明書は、**[Developer Certificate Utility]\(開発者の証明書ユーティリティ\)** に次のように表示されます。 

     [![Developer Certificate Utility](certificates-identifiers-images/certif12.png "Developer Certificate Utility")](certificates-identifiers-images/certif12-large.png#lightbox)
13. **キーチェーン**に次の項目が表示されます。 

     [![キーチェーン アクセスの証明書](certificates-identifiers-images/image43.png "キーチェーン アクセスの証明書")](certificates-identifiers-images/image43-large.png#lightbox)


## <a name="related-links"></a>関連リンク

- [インストール](/visualstudio/mac/installation/)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID と GateKeeper](https://developer.apple.com/resources/developer-id/)
