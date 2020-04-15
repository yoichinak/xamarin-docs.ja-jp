---
title: Xamarin.iOS の手動プロビジョニング
description: Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。 このガイドでは、手動プロビジョニングを利用し、開発の証明書とプロファイルを設定する方法について説明しています。
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2020
ms.openlocfilehash: 04cb1b9303e571b2a10cdfa621dcd312162e2893
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303751"
---
# <a name="manual-provisioning-for-xamarinios"></a>Xamarin.iOS の手動プロビジョニング

_Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。このガイドでは、手動プロビジョニングを利用し、開発の証明書とプロファイルを設定する方法について説明しています。_

> [!NOTE]
> このページの指示は、Apple Developer Program に有料でアクセスしている開発者に関連します。 無料アカウントがある場合は、[無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)のガイドでデバイス上でのテストの詳細を確認してください。

## <a name="create-a-development-certificate"></a>開発証明書を作成する

開発用デバイスの設定の最初の手順は、署名証明書の作成です。 署名証明書は、次の 2 つで構成されます。

- 開発証明書
- 秘密キー

開発証明書と関連付けられている[キー](#understanding-certificate-key-pairs)は iOS 開発者にとって重要です。つまり、それらは、Apple との間に ID を確立し、開発のために開発者を特定のデバイスおよびプロファイルに関連付け、開発者のアプリケーションに開発者のデジタル署名を付けます。 Apple は、展開を許可されているデバイスへのアクセスをコントロールするために証明書をチェックします。

開発チーム、証明書、およびプロファイルを管理するには、Apple の Member Center の [[Certificates, Identifiers &amp; Profiles]](https://developer.apple.com/account/resources/certificates/list) セクションにアクセスします (要ログイン)。 Apple は、デバイスまたはシミュレーター用のコードを構築するために、署名 ID を要求します。  

> [!IMPORTANT]
> 一度に使用できるのは 2 つの iOS 開発証明書だけであることに注意することが重要です。 これ以上作成する必要がある場合は、既存の証明書を取り消す必要があります。 失効した証明書を使用しているコンピューターは、アプリに署名できません。

手動プロビジョニング プロセスを開始する前に、「[Apple のアカウント管理](~/cross-platform/macios/apple-account-management.md)」ガイドに記載されているとおりに、Visual Studio で Apple 開発者アカウントを確実に追加しておく必要があります。 Apple 開発者アカウントを追加したら、次の手順を実行して署名証明書を生成します。

1. Visual Studio の [Apple 開発者アカウント] ウィンドウにアクセスします。
    1. MAC: **Visual Studio > 基本設定 > Apple 開発者アカウント**
    2. Windows の場合:**ツール > オプション > Xamarin > Apple アカウント**

2. チームを選択し、 **[詳細の表示]** をクリックします。
3. **[証明書の作成]** をクリックし、**Apple Development** または **iOS Development**を選択します。 適切なクセス許可がある場合は、数秒後に新しい署名 ID が表示されます。

### <a name="understanding-certificate-key-pairs"></a>証明書キー ペアについて

開発者プロファイルには、証明書、その関連付けられたキー、およびアカウントに関連付けられているプロビジョニング プロファイルが含まれています。 実際には、開発者のプロファイルの 2 つのバージョンがあり、1 つは Developer Portal にあり、もう 1 つはローカルの Mac 上にあります。 2 つの違いは、含まれているキーの種類です。_ポータル上のプロファイルには、証明書に関連付けられているすべての公開キーが含まれますが、ローカルの Mac 上のコピーには、すべての秘密キーが含まれています_。 この証明書を有効にするには、キーのペアが一致しなければなりません。

> [!WARNING]
> 証明書および関連付けられたキーが失われると、既存の証明書を失効させる必要があり、さらに一時的な展開のために登録されたものを含む、すべての関連するデバイスを再プロビジョニングする必要があるので、大きな問題になる可能性があります。 開発証明書を正常に設定したら、バックアップ コピーをエクスポートし、安全な場所に保管します。 これを行う方法の詳細については、Apple のドキュメントの「[Maintaining Certificates](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html)」(証明書の保守) の「Exporting and Importing Certificates and Profiles」(証明書とプロファイルのエクスポートとインポート) セクションを参照してください。

<a name="provisioning" />

## <a name="provision-an-ios-device-for-development"></a>開発用の iOS デバイスのプロビジョニング

Apple と ID を確立し、開発証明書を持っているので、プロビジョニング プロファイルと必要なエンティティをセットアップし、Apple デバイスにアプリを配置できるようにする必要があります。 デバイスでは Xcode でサポートされている iOS のバージョンが実行されている必要があります。場合によっては、デバイス、Xcode、またはその両方を更新する必要があります。

<a name="adddevice" />

## <a name="add-a-device"></a>デバイスの追加

開発用のプロビジョニング プロファイルを作成するときには、アプリケーションを実行できるデバイスを指定する必要があります。 これを可能にするために、カレンダー年度ごとに最大 100 台のデバイスを Developer Portal に追加することができ、ここから、特定のプロビジョニング プロファイルに追加するデバイスを選択できます。 Developer Portal にデバイスを追加するのには、Mac 上で以下の手順に従います

1. 指定された USB ケーブルで Mac にプロビジョニングするデバイスを接続します。
2. Xcode を開き、 **[Window] > [Devices and Simulators]** に移動します。
3. **[デバイス]** タブで、左側のメニューからデバイスを選択します。
4. **[識別子]** 文字列を強調表示し、クリップボードにコピーします。

   ![iOS 識別子文字列の場所が強調表示されている Xcode デバイスとシミュレーターウィンドウ。](manual-provisioning-images/xcode-devices.png)

5. Web ブラウザーで、[開発者ポータルの [デバイス] セクション](https://developer.apple.com/account/resources/devices/list) にアクセスし、[ **+** ] ボタンをクリックします。

   ![[追加] ボタンが強調表示されている Apple Developer サイトのデバイス ページのスクリーンショット。](manual-provisioning-images/developer-portal-devices.png)

6. 適切な **プラットフォーム** を設定し、新しいデバイスの名前を指定します。 前の手順でコピーした識別子を **[デバイス ID]** フィールドに貼り付けます。

    ![フィールドが適切に入力されている新しいデバイス登録ページのスクリーンショットです。](manual-provisioning-images/new-device-info.png)

7. **[続行]** をクリックします。
8. 情報を確認して、 **[登録]** をクリックします。

Xamarin.iOS アプリケーションのテストまたはデバッグに使用されるすべての iOS デバイスについて上記の手順を繰り返します。

<a name="provisioningprofile" />

## <a name="create-a-development-provisioning-profile"></a>開発プロビジョニング プロファイルの作成

Developer ポータルにデバイスを追加した後に、プロビジョニング プロファイルを作成し、デバイスを追加する必要があります。 

プロビジョニング プロファイルを作成する前に、*アプリ ID* を作成する必要があります。 アプリ ID は、アプリケーションを一意に識別する逆引き DNS スタイル文字列です。 次の手順は、**ワイルドカード アプリ ID** を作成する方法を示しています。この ID は、ほとんどのアプリケーションのビルドとインストールに使用できます。 **明示的なアプリ ID** は、1 つのアプリケーション (一致するバンドル ID を持つアプリケーション) のインストールのみを許可し、一般的に Apple Pay や HealthKit などの特定の iOS の機能に使用されます 明示的なアプリ ID を作成する方法については、「[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)」ガイドを参照してください。

### <a name="new-wildcard-app-id"></a>新しいワイルドカード アプリ ID

1. [開発者ポータルの識別子セクション](https://developer.apple.com/account/resources/identifiers/list) にアクセスし、[ **+** ] ボタンをクリックします。
2. **App ID** を選択し、[**続行]** をクリックします。
3. **[説明]** を入力します。 次に **バンドル ID** を **ワイルドカード** に設定し、`com.[DomainName].*` の形式で ID を入力します。

   ![必須フィールドが入力された新しいアプリ ID 登録ページのスクリーンショット。](manual-provisioning-images/new-app-id.png)

4. **[続行]** をクリックします。
5. 情報を確認して、 **[登録]** をクリックします。

### <a name="new-provisioning-profile"></a>新しいプロビジョニング ファイル

アプリ ID が作成されたら、プロビジョニング プロファイルを生成することができます。 このプロビジョニング プロファイルには、このプロファイルが*どの*アプリ (ワイルドカード アプリ ID である場合は複数のアプリ) に関連付けられているか、*誰*がプロファイルを使用できるか (どのような開発者証明書が追加されるかによって決まります)、および*どの*デバイスがアプリをインストールできるかに関する情報が含まれています。

開発のプロビジョニング プロファイルを手動で作成するには、次の操作を行います。

1. [開発者ポータルのプロファイル セクション](https://developer.apple.com/account/resources/profiles/list) にアクセスし、[ **+** ] ボタンをクリックします。

2. **[開発]** で **[iOS アプリの開発]** を選択し、 **[続行]** をクリックします。

3. 使用するアプリ ID をドロップダウンメニューから選択し、 **[続行]** をクリックします。

4. プロビジョニング プロファイルに含める証明書を選択し、 **[続行]** をクリックします。

5. アプリがインストールされるすべてのデバイスを選択し、 **[続行]** をクリックします。

6. **プロビジョニング プロファイル名** を入力し、 **[生成]** をクリックします。

7. 必要に応じて、次のページで **ダウンロード** をクリックして、プロビジョニング プロファイルを Mac にダウンロードできます。

<a name="download" />

## <a name="download-provisioning-profiles-in-visual-studio"></a>プロビジョニング プロファイルを Visual Studio にダウンロードする

Apple 開発者ポータルで新しいプロビジョニング プロファイルを作成したら、Visual Studio を使用してアプリのバンドル署名に使用できるようにダウンロードします。

1. Visual Studio の [Apple 開発者アカウント] ウィンドウにアクセスします。
    1. MAC: **Visual Studio > 基本設定 > Apple 開発者アカウント**
    2. Windows の場合:**ツール > オプション > Xamarin > Apple アカウント**

2. チームを選択し、 **[詳細の表示]** をクリックします
3. 新しいプロファイルが [**プロビジョニング プロファイル]** 一覧に表示されていることを確認します。 場合によって Visual Studio を再起動して、リストを最新の情報に更新する必要があります。 
4. **すべてのプロファイルのダウンロード** をクリックします。

これで、新しいプロビジョニング プロファイルが Visual Studio で使用できるようになり、準備が整いました。

## <a name="deploy-to-a-device"></a>デバイスに展開する

この時点で、プロビジョニングが完了し、アプリをデバイスに展開する準備ができている必要があります。 この操作を行うには、次の手順に従います。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. お使いのデバイスを Mac に接続します。
2. **Info.plist** を開き、**バンドル識別子** が前の手順で作成されたアプリ ID と一致していることを確認します (アプリ ID がワイルドカードではない場合)。
3. **[署名]** セクションで、**スキーム** として **[手動プロビジョニング]** を選択します。

    ![手動プロビジョニングが選択された Visual Studio for Mac の Info.plist のスクリーンショット](manual-provisioning-images/vsm-info-plist.png)

4. **[バンドル署名オプション]** をクリックします
5. ビルド構成が **Debug | iPhone** に設定されていることを確認します。 **署名 ID** と **プロビジョニング プロファイル** ドロップダウンメニューの両方を開いて、適切な証明書とプロビジョニング プロファイルが表示されていることを確認します。 

   ![アプリの利用可能なすべてのプロビジョニングプロファイルが一覧表示されている、[プロビジョニングプロファイル] ドロップダウンメニューが表示された iOS バンドル署名のプロパティページ。](manual-provisioning-images/vsm-bundle-signing.png)

6. 使用する特定の ID とプロファイルを選択するか、 **[自動]** のままにします。 **[自動]** に設定されている場合、Visual Studio for Macは、**Info.plist** の **バンドル識別子** を基にして ID とプロファイルを選択します。 
7. **[OK]** をクリックします。
8. **[実行]** をクリックして、アプリをお使いのデバイスにデプロイします。


# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Mac ビルド ホストにお使いのデバイスを接続します。
2. **Info.plist** を開き、**バンドル識別子** が前の手順で作成されたアプリ ID と一致していることを確認します (アプリ ID がワイルドカードではない場合)。
3. **ソリューション エクスプローラー** で [iOS プロジェクト名] を右クリックし、 **[プロパティ]** を選択して、 **[iOS ビルド]** タブに移動します。
4. ビルド構成が **Debug | iPhone** に設定されていることを確認します。 **[バンドル署名]** で、**スキーム** として **[手動プロビジョニング]** を選択します。

    ![手動プロビジョニングが選択された Visual Studio for Mac の Info.plist のスクリーンショット](manual-provisioning-images/vs-bundle-signing.png)

5. **署名 ID** と **プロビジョニング プロファイル** ドロップダウンメニューの両方を開いて、適切な証明書とプロビジョニング プロファイルが表示されていることを確認します。
6. 使用する特定の ID とプロファイルを選択するか、 **[自動]** のままにします。 **[自動]** に設定されている場合、Visual Studio は、**Info.plist** の **バンドル識別子** を基にして ID とプロファイルを選択します。 
7. **[実行]** をクリックして、アプリをお使いのデバイスにデプロイします。

-----

## <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、Xamarin.iOS アプリケーション用にアクティブ化できる機能とも呼ばれる特別なアプリケーション サービスの選択肢を提供します。 **アプリ ID** が作成されるときに、両方の iOS プロビジョニング ポータル、および Xamarin.iOS のプロジェクトの一部である **Entitlements.plist** ファイルでこれらのアプリケーション サービスを構成する必要があります。 アプリにアプリケーション サービスを追加する方法については、「[Introduction to Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md)」(機能の概要) ガイドおよび「[Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md)」 (権利に関する作業) ガイドを参照してください。

- 必要なアプリ サービスを使用してアプリ ID を作成します。
- このアプリ ID が含まれる新しい[プロビジョニング プロファイル](#provisioningprofile)を作成します。
- Xamarin.iOS プロジェクトでの権利を設定します。

## <a name="related-links"></a>関連リンク

- [無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [アプリの配布](~/ios/deploy-test/app-distribution/index.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
- [Apple - アプリ配布ガイド](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
