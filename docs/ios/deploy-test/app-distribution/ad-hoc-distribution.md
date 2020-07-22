---
title: Xamarin.iOS アプリ用のアドホック配布
description: このドキュメントでは、幅広い層の人々に主に Xamarin.iOS アプリケーションのテストのために使用されるアドホック配布手法の概要を示します。
ms.prod: xamarin
ms.assetid: 3B621CAD-103C-478A-97C3-829015F48D1A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: ae3c60699f7e31c66893723ac73248ced9a2d72a
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567971"
---
# <a name="ad-hoc-distribution-for-xamarinios-apps"></a>Xamarin.iOS アプリ用のアドホック配布

_このドキュメントでは、幅広い層の人々に主に Xamarin.iOS アプリケーションのテストのために使用されるアドホック配布手法の概要を示します。_

Xamarin.iOS アプリの開発が完了したら、ソフトウェア開発ライフサイクルの次の手順は、テストのためにアプリをユーザーに配布することです。

iTunes Connect は、アプリのテストを管理するための 1 つのオプションです。[TestFlight](~/ios/deploy-test/testflight.md) ガイドに詳しく説明されています。 ただし、Apple Developer Enterprise Program のメンバーは iTunes Connect にアクセスできないため、*アドホック*配布がこれらのアプリをテストする最適な方法となります。

Xamarin.iOS アプリケーションは、*アドホック*配布を使用してユーザーテストを実行できます。アドホック配布は、Apple Developer Program と Apple Developer Enterprise Program の両方で使用できます。また、最大 100 台の iOS デバイスをテストできます。

アドホック配布には App Store の承認を必要としないという利点があり、iTunes を介して、または Web サーバーから無線でインストールできます。 ただし、開発と配布の両方について、1 年のメンバーシップで **100** 台のデバイスに制限されており、Member Center で UDID を使用して手動でデバイスを追加する必要があります。 デバイスの追加の詳細については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#adddevice)」 (デバイスのプロビジョニング) ガイドを参照してください。

アドホック配布では、コード署名情報、およびアプリケーションの ID とアプリケーションをインストールできるデバイスを含むアドホック *プロビジョニング プロファイル*を使用して、アプリケーションをプロビジョニングする必要があります。

このガイドでは、アドホック配布のためのプロビジョニングに関する情報と、Xamarin.iOS アプリを配布する方法に関する情報が提供されます。

<a name="setup"></a>

## <a name="setting-up-for-distribution"></a>配布の設定

テスト目的で、社内配布用の Xamarin.iOS アプリケーションをリリースする予定の場合でも、固有のアドホック配布プロビジョニング プロファイルをビルドする必要があります。 このプロファイルでは、iOS デバイスにインストールできるように、リリースするアプリケーションにデジタル署名することができます。

次のセクションでは、配布証明書とプロビジョニング プロファイルを使用して設定する方法について説明します。

> [!NOTE]
> 配布証明書とプロビジョニング プロファイルを作成できるのは、チーム エージェントと管理者のみです。

<a name="createcertificate"></a>

## <a name="create-a-distribution-certificate"></a>配布証明書の作成

1. Apple Developer Member Center の *[Certificates, Identifiers & Profiles]\(証明書、ID、およびプロファイル\)* セクションに移動します。
2. *[Certificates]\(証明書\)* の下で **[Production]\(運用\)** を選択します。
3. **+** ボタンをクリックして、新しい証明書を作成します。
4. *[Production]\(運用\)* 見出しの下で、プログラム メンバーシップに応じて、 **[In-House and Ad Hoc]\(社内およびアドホック\)** または **[App Store and Ad Hoc]\(App Store およびアドホック\)** を選択します。

   [![[In-House and Ad Hoc]\(社内およびアドホック\)、または [App Store and Ad Hoc]\(App Store およびアドホック\) を選択します](ad-hoc-distribution-images/cert-first-small.png)](ad-hoc-distribution-images/cert-first-large.png#lightbox)

5. [Continue]\(続行\) をクリックし、指示に従って Keychain Access を使用して証明書署名要求を作成します。

   [![キーチェーン アクセスを使用して証明書署名要求を作成します](ad-hoc-distribution-images/createcertmanually02.png)](ad-hoc-distribution-images/createcertmanually02.png#lightbox)

6. 指示どおりに CSR を作成したら、[Continue]\(続行\) をクリックし、CSR を Member Center にアップロードします。

   [![CSR を Member Center にアップロードします](ad-hoc-distribution-images/createcertmanually03.png)](ad-hoc-distribution-images/createcertmanually03.png#lightbox)

7. [Generate]\(生成\) をクリックして証明書を作成します。
8. 最後に、完成した証明書をダウンロードし、ファイルをダブルクリックしてインストールします。
9. この時点で、証明書はコンピューターにインストールされますが、場合によっては、[プロファイルを更新](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)して、Xcode で表示されるようにする必要があります。

または、Xcode の [Preferences]\(環境設定\) ダイアログを使用して証明書を要求することができます。 この操作を行うには、次の手順に従います。

1. チームを選択し、 **[証明書の管理...** ] をクリックします。[![チームの選択](ad-hoc-distribution-images/selectteam.png)](ad-hoc-distribution-images/selectteam.png#lightbox)

2. 次に、**プラス (+)** ボタンをクリックして **[iOS App Store]** を選択します。[![iOS App Store の選択](ad-hoc-distribution-images/selectcert.png)](ad-hoc-distribution-images/selectcert.png#lightbox)

<a name="createprofile"></a>

## <a name="create-a-distribution-provisioning-profile"></a>配布プロビジョニング プロファイルの作成

<a name="createappid"></a>

### <a name="create-an-app-id"></a>アプリ ID の作成
作成する他のプロビジョニング プロファイルと同じように、ユーザーのデバイスに配布されるアプリを識別するため、アプリ ID が必要になります。 ID をまだ作成していない場合は、次の手順に従って作成します。

1. [Apple Developer Center](https://developer.apple.com/account/overview.action) で *[Certificate, Identifiers and Profiles]\(証明書、ID、およびプロファイル\)* セクションを参照します。 **[Identifiers]** \(ID\) の下で **[App IDs]** \(App ID\) を選択します。
2. **+** ボタンをクリックして、ポータルで識別するための**名前**を指定します。
3. アプリのプレフィックスは、チーム ID として既に設定されており、変更できません。 明示的またはワイルドカード アプリ ID を選択し、次のように逆引き DNS 形式でバンドル ID を入力します。
    - **明示的**: `com.[DomainName].[AppName]`
    - **ワイルドカード**: `com.[DomainName].*`
4. アプリで必要な任意の [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning-for-application-services) を選択します。
5. **[Continue]\(続行\)** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

配布プロファイルを作成するのに必要なコンポーネントがそろったら、次の手順に従って配布プロファイルを作成します。

1. Apple Provisioning ポータルに戻り、 **[Provisioning]\(プロビジョニング\)、[Distribution]\(配布\)** の順に選択します。[![[Provisioning]、[Distribution] の順に選択します](ad-hoc-distribution-images/distribute01.png)](ad-hoc-distribution-images/distribute01.png#lightbox)

2. **+** ボタンをクリックし、**アドホック**として作成する配布プロファイルの種類を選択します。

    [![アドホック配布の種類を作成します](ad-hoc-distribution-images/distribute02.png)](ad-hoc-distribution-images/distribute02.png#lightbox)

3. **[Continue]\(続行\)** ボタンをクリックし、配布プロファイルを作成するアプリ ID をドロップダウン リストから選択します。

    [![ドロップダウン リストからアプリ ID を選択します](ad-hoc-distribution-images/distribute03.png)](ad-hoc-distribution-images/distribute03.png#lightbox)

4. **[Continue]\(続行\)** ボタンをクリックし、アプリケーションに署名するために必要な配布証明書を選択します。

    [![アプリケーションに署名するために必要な配布証明書を選択します](ad-hoc-distribution-images/distribute04.png)](ad-hoc-distribution-images/distribute04.png#lightbox)

5. **[Continue]\(続行\)** ボタンをクリックし、新しい配布プロファイルの**名前**を入力します。

    [![新しい配布プロファイルの名前を入力します](ad-hoc-distribution-images/distribute06.png)](ad-hoc-distribution-images/distribute06.png#lightbox)

6. **[Generate]\(生成\)** ボタンをクリックし、新しいプロファイルを作成してプロセスを終了します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で新しい配布プロファイルを使用可能にするには、Visual Studio for Mac を終了して、(「[Xcode でプロファイルと証明書をダウンロードする](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)」セクションの手順に従って) Xcode で使用可能な署名 ID とプロビジョニング プロファイルのリストを更新する必要がある場合があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio で新しい配布プロファイルを使用可能にするには、Visual Studio を終了して、(「[Xcode でプロファイルと証明書をダウンロードする](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)」セクションの手順に従って、ビルド ホストの Mac で) Xcode で使用可能な署名 ID とプロビジョニング プロファイルのリストを更新する必要がある場合があります。

-----

<a name="selectprofile"></a>

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Xamarin.iOS プロジェクトでの配布プロファイルの選択

Xamarin.iOS アプリケーションの最終ビルドを行う準備ができたら、作成済みの配布プロファイルを選択します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

 Visual Studio for Mac で、次の操作を行います。

1. **ソリューション エクスプローラー**でプロジェクト名をダブルクリックして、編集用に開きます。
2. **[iOS バンドル署名]** を選択し、 **[構成]** ドロップダウン リストからビルドの種類を選択します。

    ![[構成] ドロップダウン リストからビルドの種類を選択します](ad-hoc-distribution-images/releasexs01.png)
3. ほとんどの場合、 **[署名 ID]** と **[プロビジョニング プロファイル]** は既定値の **[自動]** のままにしておいてかまいません。Visual Studio for Mac は、Info.plist のバンドル識別子に従って適切なプロファイルを選択します。

    ![既定値の [自動] に設定された署名 ID とプロビジョニング プロファイル](ad-hoc-distribution-images/releasexs02.png)
4. 必要に応じて、ドロップダウン リストから署名 ID と配布プロファイル (前の手順で作成したもの) を選択します。

    ![署名 ID と配布プロファイルを選択します](ad-hoc-distribution-images/releasexs03.png)
5. **[OK]** ボタンをクリックして、変更を保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)
 Visual Studio で、次の操作を行います。

1. **ソリューション エクスプローラー**でプロジェクト名を右クリックし、 **[プロパティ]** を選択して編集用に開きます。
2. **[iOS バンドル署名]** を選択し、 **[構成]** ドロップダウン リストからビルドの種類を選択します。

    ![[構成] ドロップダウン リストからビルドの種類を選択します](ad-hoc-distribution-images/releasevs01.png)
3. ほとんどの場合、 **[署名 ID]** と **[プロビジョニング プロファイル]** は既定値の **[自動]** のままにしておいてかまいません。Visual Studio for Mac は、Info.plist のバンドル識別子に従って適切なプロファイルを選択します。

    ![既定値の [自動] に設定された署名 ID とプロビジョニング プロファイル](ad-hoc-distribution-images/releasevs02.png)
4. 必要に応じて、ドロップダウン リストから署名 ID と配布プロファイル (前の手順で作成したもの) を選択します。

    ![署名 ID と配布プロファイルを選択します](ad-hoc-distribution-images/releasevs03.png)
5. プロジェクトのプロパティへの変更を保存します。

-----

<a name="adhoc"></a>

## <a name="ad-hoc-distribution"></a>アドホック配布

[TestFlight](~/ios/deploy-test/testflight.md) はベータ テストと配布の一般的な方法ですが、これは iTunes Connect の一部であるため、**Apple Developer Enterprise Program** のメンバーは使用できません。

アドホック配布では、開発者は、iTunes Connect がオプションでない場合に、さまざまなデバイスでアプリのベータ テストを行うことができます。 アドホックは社内配布の場合と同じように機能し、IPA を作成する必要があります。無線で、または iTunes を介して手動で配布することができます。

<a name="IPA_Creation"></a>

### <a name="ipa-support-for-ad-hoc-deployment"></a>アドホック展開のための IPA のサポート

プロビジョニングが終ったら、アプリケーションを *IPA* と呼ばれるファイルにパッケージ化できます。 これは、アプリケーションとその他のメタデータとアイコンを含む zip ファイルです。 IPA は、アプリケーションをローカルに iTunes に追加して、プロビジョニング プロファイルに含まれているデバイスと直接同期できるようにするために使用されます。

IPA の作成の詳細については、「[IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリケーションをテストするために必要なアドホック配布メカニズムについて説明します。

## <a name="related-links"></a>関連リンク

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)
