---
title: "手動プロビジョニング"
description: "Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。 このガイドでは、開発証明書とプロファイルの要求、アプリケーション サービスの使用、デバイスへのアプリの展開について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/15/2017
ms.openlocfilehash: 2ad3bd55ae0abc44b0c9757bd79c2711eddf171d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="manual-provisioning"></a>手動プロビジョニング

_Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。このガイドでは、開発証明書とプロファイルの要求、アプリケーション サービスの使用、デバイスへのアプリの展開について説明します。_

<a name="signingidentity" />

## <a name="creating-a-signing-identity"></a>署名 ID を作成する

開発用デバイスの設定の最初の手順は、署名 ID の作成です。 署名 ID は、次の 2 つで構成されます。

- 開発証明書
- 秘密キー

開発証明書と関連付けられている[キー](#keypairs)は iOS 開発者にとって重要です。つまり、それらは、Apple との間に ID を確立し、開発のために開発者を特定のデバイスおよびプロファイルに関連付け、開発者のアプリケーションに開発者のデジタル署名を付けます。 Apple は、展開を許可されているデバイスへのアクセスをコントロールするために証明書をチェックします。

開発チーム、証明書、およびプロファイルを管理するには、Apple の Members Center の [[Certificates, Identifiers & Profiles]](https://developer.apple.com/account/overview.action) セクションにアクセスします。 Apple は、デバイスまたはシミュレーター用のコードを構築するために、署名 ID を要求します。  

> [!IMPORTANT]
> 一度に使用できるのは 2 つの iOS 開発証明書だけであることに注意することが重要です。 これ以上作成する必要がある場合は、既存の証明書を取り消す必要があります。 失効した証明書を使用しているコンピューターは、アプリに署名できません。

署名 ID を生成するには、次の手順を実行します。

1. [Developer Portal の [Certificates, Identifiers, and Profiles]\(証明書、ID、プロファイル\) セクション](https://developer.apple.com/account/overview.action)にログインし、**[iOS Apps]**\(iOS アプリ\) 列から **[Certificates]**\(証明書\) を選択します。 **+** を選択して、新しい証明書を作成します。

    [![](manual-provisioning-images/cert-plus.png "[+] をクリックして新しい証明書を作成します")](manual-provisioning-images/cert-plus.png#lightbox)

2. 証明書の種類として **[iOS App Development]**\(iOS アプリの開発\) オプションを選択し、**[Continue]**\(続行\) をクリックします。 アカウントの時計によっては、この画面の表示が異なる場合があります。

    [ ![](manual-provisioning-images/cert-first.png "証明書の種類として [iOS App Development] オプションを選択します")](manual-provisioning-images/cert-first.png#lightbox)

3. 証明書署名要求を要求します。これは証明書を生成するために手動でアップロードします。 これを行うには、Mac で**キーチェーン アクセス**を起動します。 メイン メニューに移動し、下の図のように **[Certificate Assistant]**\(証明書アシスタント\) と **[Request a Certificate from a Certificate Authority...]**\(証明機関から証明書を要求\) を選択します。

      [![](manual-provisioning-images/key-first.png "証明書の署名要求を要求します")](manual-provisioning-images/key-first.png#lightbox)

4. 情報を入力し、オプションを選択して **ディスクに保存します**。

    [![](manual-provisioning-images/key-second.png "情報を入力します")](manual-provisioning-images/key-second.png#lightbox)

5. ここで簡単に見つけることができる場所に CSR を保存します。

    [![](manual-provisioning-images/cert-third.png "CSR を保存します")](manual-provisioning-images/cert-third.png#lightbox)

6. プロビジョニング ポータルに戻り、証明書をポータルにアップロードして、送信します。

    [![](manual-provisioning-images/cert-second.png "証明書をポータルにアップロードします")](manual-provisioning-images/cert-second.png#lightbox)

    管理者特権がない場合は、管理者またはチーム エージェントによって証明書が承認される必要があります。

7. 証明書が承認されたら、プロビジョニング ポータルからダウンロードします。

    [![](manual-provisioning-images/status-dev.png "プロビジョニング ポータルから証明書をダウンロードします")](manual-provisioning-images/status-dev.png#lightbox)

8. ダウンロードした証明書をダブルクリックして、キーチェーン アクセスを起動し、**[My Certificates]**\(自分の証明書\) パネルを開いて、新しい証明書と関連付けられている秘密キーを表示します。

    [![](manual-provisioning-images/keychain.png "キーチェーン アクセスの証明書")](manual-provisioning-images/keychain.png#lightbox)

<a name="keypairs" />

### <a name="understanding-certificate-key-pairs"></a>証明書キー ペアについて

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

開発者プロファイルには、証明書、その関連付けられたキー、およびアカウントに関連付けられているプロビジョニング プロファイルが含まれています。 実際には、開発者のプロファイルの 2 つのバージョンがあり、1 つは Developer Portal にあり、もう 1 つはローカルの Mac 上にあります。 2 つの違いは、含まれているキーの種類です。_ポータル上のプロファイルには、証明書に関連付けられているすべての公開キーが含まれますが、ローカルの Mac 上のコピーには、すべての秘密キーが含まれています_。 この証明書を有効にするには、キーのペアが一致しなければなりません。 秘密キーが失われた場合、すべての証明書とプロビジョニング プロファイルを再生成する必要があるので、ローカルの Mac に開発者プロファイルのバックアップを保管します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

開発者プロファイルには、証明書、その関連付けられたキー、およびアカウントに関連付けられているプロビジョニング プロファイルが含まれています。 実際には、開発者のプロファイルの 2 つのバージョンがあり、1 つは Developer Portal にあり、もう 1 つは Mac 上にあります。 2 つの違いは、含まれているキーの種類です。_ポータル上のプロファイルには、証明書に関連付けられているすべての公開キーが含まれますが、Mac 上のコピーには、すべての秘密キーが含まれています_。 この証明書を有効にするには、キーのペアが一致しなければなりません。 秘密キーが失われた場合、すべての証明書とプロビジョニング プロファイルを再生成する必要があるので、Xamarin Build Host の Mac に開発者プロファイルのバックアップを保管します。

-----

> [!WARNING]
> **注:** 証明書および関連付けられたキーが失われると、既存の証明書を失効させる必要があり、さらに一時的な展開のために登録されたものを含む、すべての関連するデバイスを再プロビジョニングする必要があるので、大きな問題になる可能性があります。 開発証明書を正常に設定したら、バックアップ コピーをエクスポートし、安全な場所に保管します。 これを行う方法の詳細については、Apple のドキュメントの「[Maintaining Certificates](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html)」(証明書の保守) の「Exporting and Importing Certificates and Profiles」(証明書とプロファイルのエクスポートとインポート) セクションを参照してください。

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>開発用の iOS デバイスのプロビジョニング

Apple と ID を確立し、開発証明書を持っているので、プロビジョニング プロファイルと必要なエンティティをセットアップし、Apple デバイスにアプリを配置できるようにする必要があります。 デバイスでは Xcode でサポートされている iOS のバージョンが実行されている必要があります。場合によっては、デバイス、Xcode、またはその両方を更新する必要があります。

<a name="adddevice" />

## <a name="add-a-device"></a>デバイスの追加

開発用のプロビジョニング プロファイルを作成するときには、アプリケーションを実行できるデバイスを指定する必要があります。 これを可能にするために、カレンダー年度ごとに最大 100 台のデバイスを Developer Portal に追加することができ、ここから、特定のプロビジョニング プロファイルに追加するデバイスを選択できます。 Developer Portal にデバイスを追加するのには、Mac 上で以下の手順に従います

1. Xcode を起動します。
2. 指定された USB ケーブルで Mac にプロビジョニングするデバイスを接続します。
2. **Windows** メニューから **[デバイス]** を選択します。

  [![](manual-provisioning-images/add01.png "Windows メニューから [デバイス] を選択します")](manual-provisioning-images/add01.png#lightbox)

3. デバイス ウィザードの左側にある **デバイス** の一覧から必要な iOS デバイスを選択します。
4. **[識別子]** 文字列を強調表示し、クリップボードにコピーします。

  [![](manual-provisioning-images/add02.png "識別子文字列を強調表示します")](manual-provisioning-images/add02.png#lightbox)

5. Safari で、[Apple Developer Center](https://developer.apple.com/membercenter/index.action) に移動してログインします。
6. **[Certificates, Identifiers & Profiles]** のリンクをクリックします。

  [![](manual-provisioning-images/add03.png "[Certificates, Identifiers & Profiles] のリンクをクリックします")](manual-provisioning-images/add03.png#lightbox)

7. **[Devices]** リンクをクリックします。

  [![](manual-provisioning-images/add04.png "[Devices] リンクをクリックします")](manual-provisioning-images/add04.png#lightbox)

8. **+** ボタンをクリックします。

  [![](manual-provisioning-images/add05.png "[+] ボタンをクリックします")](manual-provisioning-images/add05.png#lightbox)

9. 新しいデバイスの名前を指定し、上でコピーしたデバイスの **ID** を **[UUID]** フィールドに貼り付けます。

  [![](manual-provisioning-images/add06.png "新しいデバイスの名前とデバイス ID を指定します")](manual-provisioning-images/add06.png#lightbox)

10. **[Continue]** をクリックします。
11. 情報を確認して、**[Register]** ボタンをクリックします。

  [![](manual-provisioning-images/add07.png "情報を確認します")](manual-provisioning-images/add07.png#lightbox)

Xamarin.iOS アプリケーションのテストまたはデバッグに使用されるすべての iOS デバイスについて上記の手順を繰り返します。

Developer ポータルにデバイスを追加した後に、プロビジョニング プロファイルを作成し、デバイスを追加する必要があります。


<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>開発プロビジョニング プロファイルの作成

開発証明書と同様に、プロビジョニング プロファイルは Apple の Members Center の [[Certificates, Identifiers & Profiles]](https://developer.apple.com/account/overview.action) セクションで手動で作成できます。

プロビジョニング プロファイルを作成する前に、*アプリ ID* を作成する必要があります。 アプリ ID は、アプリケーションを一意に識別する逆引き DNS スタイル文字列です。 次の手順は、**ワイルドカード アプリ ID** を作成する方法を示しています。この ID は、ほとんどのアプリケーションのビルドとインストールに使用できます。 **明示的なアプリ ID** は、1 つのアプリケーション (一致するバンドル ID を持つアプリケーション) のインストールのみを許可し、一般的に Apple Pay や HealthKit などの特定の iOS の機能に使用されます 明示的なアプリ ID を作成する方法については、「[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)」ガイドを参照してください。

### <a name="app-id"></a>アプリ ID

1. [Developer Portal](https://developer.apple.com/account/overview.action) で、Apple Developer Center の [*Certificate, Identifiers and Profiles*]\(証明書、ID、プロファイル\) セクションを参照します。 **[Identifiers]**\(ID\) の下で **[App IDs]**\(アプリ ID\) を選択します。
2. **+** ボタンをクリックし、**名前**を指定します。

    [![](manual-provisioning-images/appid05a.png "名前を指定します")](manual-provisioning-images/appid05a.png#lightbox)
3. アプリのプレフィックスが事前設定されている必要があります。 アプリのサフィックスとして **[Wildcard App ID]** を選択します。 次の形式でバンドル ID を入力します`com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "バンドル ID を入力します")](manual-provisioning-images/appid05b.png#lightbox)

3. **[Continue]\(続行\)** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

### <a name="provisioning-profile"></a>プロファイルのプロビジョニング

アプリ ID が作成されたら、プロビジョニング プロファイルを生成することができます。 このプロビジョニング プロファイルには、このプロファイルが*どの*アプリ (ワイルドカード アプリ ID である場合は複数のアプリ) に関連付けられているか、*誰*がプロファイルを使用できるか (どのような開発者証明書が追加されるかによって決まります)、および*どの*デバイスがアプリをインストールできるかに関する情報が含まれています。

開発のプロビジョニング プロファイルを手動で作成するには、次の操作を行います。

1. Safari を使用して、[Apple Developers Member Center](https://developer.apple.com/membercenter/index.action) を参照し、[*Certificates, Identifiers & Profiles*]\(証明書、ID、プロファイル\) セクションで、[プロビジョニング プロファイル] を選択します。
2. 右上隅にある **+** ボタンをクリックし、新しいプロファイルを作成します。
3. **[Development]**\(開発\) セクションで、**[iOS App Development]**\(iOS アプリの開発\) の横にあるラジオ ボタンを選択し、**[Continue]**\(続行\) をクリックします。

    [![](manual-provisioning-images/provisioning-profile01.png "作成するプロファイルの種類を選択します")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. ドロップダウン メニューから、使用するアプリ ID を選択します。

    [![](manual-provisioning-images/provisioning-profile02.png "使用するアプリ ID を選択します")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. プロビジョニング プロファイルに含める証明書を選択し、**[Continue]** をクリックします。

    [![](manual-provisioning-images/provisioning-profile03.png "プロビジョニング プロファイルに含める証明書を選択します")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. アプリがインストールされるすべてのデバイスを選択します。

    [![](manual-provisioning-images/provisioning-profile04.png "アプリがインストールされるすべてのデバイスを選択します")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. プロビジョニング プロファイルのわかりやすい名前を指定し、**[Continue]** をクリックしてプロファイルを作成します。

    [![](manual-provisioning-images/provisioning-profile05.png "プロビジョニング プロファイルのわかりやすい名前を指定します")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. **[Download]** をクリックして、プロビジョニングプロファイルを Mac にダウンロードします。

    [![](manual-provisioning-images/provisioning-profile06.png "プロビジョニング プロファイルをダウンロードします")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Xcode で、プロビジョニング プロファイルをインストールするファイルをダブルクリックします。 Xcode では、開いたときを除いて、プロファイルをインストールしたことを示す視覚的な手がかりが表示されないことがあります。 **[Xcode]、[Preferences]、[Accounts]** の順に参照することでこれを確認できます。 Apple ID を選択し、**[View Details...]** をクリックします。下の図に示すように、新しいプロビジョニング プロファイルが表示されます。

      [![](manual-provisioning-images/provisioning-profile07.png "Xcode でのプロファイルの表示")](manual-provisioning-images/provisioning-profile07.png#lightbox)

プロビジョニング プロファイルが正常に作成された後に、すべての開発証明書を Visual Studio for Mac および Visual Studio で使用できるようにするために、Xcode の更新が必要な場合があります。

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Xcode でプロファイルと証明書をダウンロードする

Apple Developer Portal で作成された証明書とプロビジョニング プロファイルが Xcode に自動的に表示されない場合があります。 そのため、Visual Studio for Mac や Visual Studio でアクセスできるようにするために、それらのダウンロード必要な場合があります。 Apple Developer Portal で作成された証明書を更新およびダウンロードするには、次の手順を実行します。

1.   Visual Studio for Mac または Visual Studio を終了します。
2.   Xcode を起動します。
3.   **[Xcode メニュー] > [設定]** を選択します。
4.   **[アカウント]** タブをクリックします。
5.   チームを選択し、**[Download Manual Profiles]\(手動プロファイルのダウンロード\)** ボタンをクリックします。[![](manual-provisioning-images/selectteam1.png "手動プロファイルのダウンロード")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Xcode を終了します。
7.  Visual Studio for Mac または Visual Studio を起動します。

新しい証明書またはプロビジョニング プロファイルが、Visual Studio for Mac または Visual Studio で使用可能になります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> **注:** 新しい証明書や変更された証明書、または Xcode によって更新されたプロファイルが表示される前に、Visual Studio for Mac の停止と再起動が必要になる場合があります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> **注:** 新しい証明書や変更された証明書、または Xcode によって更新されたプロファイルが表示される前に、Visual Studio の停止と再起動が必要になる場合があります。

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、Xamarin.iOS アプリケーション用にアクティブ化できる機能とも呼ばれる特別なアプリケーション サービスの選択肢を提供します。 **アプリ ID** が作成されるときに、両方の iOS プロビジョニング ポータル、および Xamarin.iOS のプロジェクトの一部である **Entitlements.plist** ファイルでこれらのアプリケーション サービスを構成する必要があります。 アプリにアプリケーション サービスを追加する方法については、「[Introduction to Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md)」(機能の概要) ガイドおよび「[Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md)」 (権利に関する作業) ガイドを参照してください。

* 必要なアプリ サービスを使用してアプリ ID を作成します。
* このアプリ ID が含まれる新しい[プロビジョニング プロファイル](#provisioningprofile)を作成します。
* Xamarin.iOS プロジェクトでの権利を設定します。

<a name="deploy" />

## <a name="deploying-to-a-device"></a>デバイスの展開

この時点で、プロビジョニングが完了し、アプリをデバイスに展開する準備ができている必要があります。 この操作を行うには、次の手順に従います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[プロジェクトのオプション] > [iOS バンドル署名] で、チームのセレクターが [なし] に設定されていることを確認します。

1. デバイスを Mac に接続します。
2. プロジェクトの **Info.plist** で、バンドル ID がアプリ ID と一致していることを確認します (アプリ ID がワイルドカードではない場合)。

  ![](manual-provisioning-images/deploydevice01xs.png "識別子の入力")

3. プロジェクトを右クリックして、[プロジェクトのオプション] ダイアログを表示し、**[ビルド] > [iOS バンドル署名]** を参照します。 **[署名 ID]** と **[プロビジョニング プロファイル]** の両方の横にあるドロップダウン リストで、Visual Studio for Mac が正しいプロファイルを認識できることを確認し、特定の ID とプロファイルを選択します。

  ![](manual-provisioning-images/deploydevice02xs.png "特定の ID とプロファイルを選択します")

これが **[自動]** に設定されている場合、Visual Studio for Mac は、手順 #2 で設定されたバンドル ID を基にして ID とプロファイルを選択します。

4. ビルド構成がシミュレーターではなく **[iPhone]** / **[iPad]** に設定されていることを確認します。
5. Visual Studio for Mac で **[実行]** をクリックして、デバイスで実行されているアプリを表示します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. デバイスを Mac に接続します。
2. プロジェクトの **Info.plist** で、バンドル ID がアプリ ID と一致していることを確認します。

  ![](manual-provisioning-images/servicevs01.png "識別子の入力")

3. プロジェクトを右クリックして、[プロジェクトのオプション] ダイアログを表示し、**[ビルド] > [iOS バンドル署名]** を参照します。 **[署名 ID]** と **[プロビジョニング プロファイル]** の両方の横にあるドロップダウン リストで、Visual Studio が新しいプロファイルを認識できることを確認し、特定の ID とプロファイルを選択します。

    これが **[自動]** に設定されている場合、Visual Studio は、手順 #2 で設定されたバンドル ID を基にして ID とプロファイルを選択します。

4. ビルド構成がシミュレーターではなく **[iPhone]** または **[iPad]** に設定されていることを確認します。
5. Visual Studio で **[実行]** をクリックして、デバイスで実行されているアプリを表示します。


-----

## <a name="summary"></a>まとめ

このガイドでは、Xamarin.iOS 用の開発環境のセットアップに必要な手順について説明しました。 開発者、チーム、アプリを実行できるデバイス、個々のアプリ ID に関する情報を使用してアプリケーションのコードに署名する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [アプリの配布](~/ios/deploy-test/app-distribution/index.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
- [Apple - アプリ配布ガイド](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
