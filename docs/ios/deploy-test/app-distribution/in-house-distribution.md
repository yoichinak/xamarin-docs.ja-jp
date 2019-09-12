---
title: Xamarin.iOS アプリ用の社内配布
description: このドキュメントでは、Apple Enterprise Developer Program のメンバーとして、アプリケーションの社内配布の概要を提供します。
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: a27536585cbd320a5595d71b156459e25a1fa7a9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763063"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Xamarin.iOS アプリ用の社内配布

_このドキュメントでは、Apple Enterprise Developer Program のメンバーとして、アプリケーションの社内配布の概要を提供します。_

Xamarin.iOS アプリの開発が完了したら、ソフトウェア開発ライフサイクルの次の手順は、アプリをユーザーに配布することです。 独自のアプリは、**Apple Developer Enterprise Program** を介して*社内* (旧称 Enterprise) 配布できます。これには次の利点があります。

- アプリケーションをレビューのために Apple に提出する必要がありません。
- アプリケーションを展開先となるデバイスの数に制限がありません。
  - Apple では、社内アプリケーションは社内でのみ使用することと非常に明確にしていることに留意してください。

また、Enterprise Program での次の点にも留意してください。

- 配布またはテスト (TestFlight を含む) のために iTunes Connect へのアクセスは提供されません。
- メンバーシップのコストは、1 年あたり 299 ドルです。

すべてのアプリは、Apple によって署名されている必要があります。

<a name="testing" />

## <a name="testing-your-application"></a>アプリケーションのテスト

アプリケーションのテストは、アドホック配布を使用して行われます。 テストの詳細については、[アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)ガイドの手順に従います。 最大 100 台のデバイスまでしかテストできないことに注意してください。

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>配布の設定

他の Apple Developer Program と同じく、Apple Developer Enterprise Program では、チーム管理者とエージェントのみが配布証明書とプロビジョニング プロファイルを作成できます。

Apple Developer Enterprise Program 証明書は、3 年間有効で、プロビジョニング プロファイルは 1 年後に期限切れになります。

有効期限が切れた証明書は更新できないことに留意してください。代わりに、期限切れの証明書を新しい証明書に置き換える必要があります。[次で](#certificate)詳しく説明します。

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>配布証明書の作成

1. Apple Developer Member Center の *[Certificates, Identifiers & Profiles]\(証明書、ID、およびプロファイル\)* セクションに移動します。
2. *[Certificates]\(証明書\)* の下で **[Production]\(運用\)** を選択します。
3. **+** ボタンをクリックして、新しい証明書を作成します。
4. *[Production]\(運用\)* の見出しの下で **[In-House and Ad Hoc]\(社内およびアドホック\)** を選択します。

   [![](in-house-distribution-images/createcertmanually01.png "[In-House and Ad Hoc]\(社内およびアドホック\) を選択します")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. [Continue]\(続行\) をクリックし、指示に従って Keychain Access を使用して証明書署名要求を作成します。

   [![](in-house-distribution-images/createcertmanually02.png "キーチェーン アクセスを使用して証明書署名要求を作成します")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. 指示どおりに CSR を作成したら、[Continue]\(続行\) をクリックし、CSR を Member Center にアップロードします。

   [![](in-house-distribution-images/createcertmanually03.png "CSR を Member Center にアップロードします")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. [Generate]\(生成\) をクリックして証明書を作成します。
8. 完成した証明書をダウンロードし、ファイルをダブルクリックしてインストールします。
9. この時点で、証明書はコンピューターにインストールされますが、場合によっては、プロファイルを更新して、Xcode で表示されるようにする必要があります。

または、Xcode の [Preferences]\(環境設定\) ダイアログを使用して証明書を要求することができます。 この操作を行うには、次の手順に従います。

1. 自分のチームを選択し、 *[View Details]\(詳細の表示\)* をクリックします。

   [![](in-house-distribution-images/selectteam.png "チームを選択します")](in-house-distribution-images/selectteam.png#lightbox)

2. 次に、 **[iOS Distribution Certificate]\(iOS 配布証明書\)** の横の **[作成]** ボタンをクリックします。

   [![](in-house-distribution-images/selectcert.png "iOS 配布証明書を作成します")](in-house-distribution-images/selectcert.png#lightbox)

3. 次に、**プラス (+)** ボタンをクリックして **[iOS App Store]** を選択します。

   [![](in-house-distribution-images/selectcert.png "iOS App Store を選択します")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>配布プロビジョニング プロファイルの作成

<a name="appid" />

### <a name="creating-an-app-id"></a>アプリ ID の作成

作成する他のプロビジョニング プロファイルと同じく、ユーザーのデバイスに配布するアプリを識別するため、App ID が必要になります。 ID をまだ作成していない場合は、次の手順に従って作成します。

1. [Apple Developer Center](https://developer.apple.com/account/overview.action) で *[Certificate, Identifiers and Profiles]\(証明書、ID、およびプロファイル\)* セクションを参照します。 **[Identifiers]** \(ID\) の下で **[App IDs]** \(App ID\) を選択します。
2. **+** ボタンをクリックして、ポータルで識別するための**名前**を指定します。
3. アプリのプレフィックスは、チーム ID として既に設定されており、変更できません。 明示的またはワイルドカード アプリ ID を選択し、次のように逆引き DNS 形式でバンドル ID を入力します。**明示的**: com.[ドメイン名].[アプリ名] **ワイルドカード**: com.[ドメイン名].*
4. アプリで必要な任意の [App Services](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services) を選択します。
5. **[Continue]\(続行\)** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

配布プロファイルを作成するのに必要なコンポーネントがそろったら、次の手順に従って配布プロファイルを作成します。

1. Apple Provisioning ポータルに戻り、 **[Provisioning]\(プロビジョニング\)**  >  **[Distribution]\(配布\)** の順に選択します。

   [![](in-house-distribution-images/distribute01.png "[Provisioning]、[Distribution] の順に選択します")](in-house-distribution-images/distribute01.png#lightbox)

2. **+** ボタンをクリックし、**社内配布** として作成する配布プロファイルの種類を選択します。

   [![](in-house-distribution-images/distribute02.png "社内配布プロファイルを作成します")](in-house-distribution-images/distribute02.png#lightbox)

3. **[Continue]\(続行\)** ボタンをクリックし、配布プロファイルを作成するアプリ ID をドロップダウン リストから選択します。

   [![](in-house-distribution-images/distribute03.png "ドロップダウン リストからアプリ ID を選択します")](in-house-distribution-images/distribute03.png#lightbox)

4. **[Continue]\(続行\)** ボタンをクリックし、アプリケーションに署名するために必要な配布証明書を選択します。

   [ ![](in-house-distribution-images/distribute04.png "アプリケーションに署名するために必要な配布証明書を選択します")](in-house-distribution-images/distribute04.png#lightbox)

5. **[Continue]\(続行\)** ボタンをクリックし、新しい配布プロファイルの**名前**を入力します。

   [![](in-house-distribution-images/distribute06.png "新しい配布プロファイルの名前を入力します")](in-house-distribution-images/distribute06.png#lightbox)

6. **[Generate]\(生成\)** ボタンをクリックし、新しいプロファイルを作成してプロセスを終了します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 Visual Studio for Mac で新しい配布プロファイルを使用可能にするには、Visual Studio for Mac を終了し、(「[Requesting Signing Identities](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)」 (証明 ID の要求) セクションの手順に従って) Xcode で使用可能な署名 ID とプロビジョニング プロファイルのリストを更新する必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で新しい配布プロファイルを使用可能にするには、Visual Studio を終了して、(「[Requesting Signing Identities](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)」 (証明 ID の要求) セクションの手順に従って、ビルド ホストの Mac で) Xcode で使用可能な署名 ID とプロビジョニング プロファイルのリストを更新する必要があります。

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>アプリの社内配布

Apple Developer Enterprise Program では、ライセンス所有者がアプリケーションの配布と Apple が定めた[ガイドライン](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf)の順守に責任を持ちます。

アプリは、次のようなさまざまな方法で安全に配布できます。

- ITunes を介してローカルで
- MDM サーバー
- 内部のセキュリティで保護された Web サーバー
- 電子メール

これらのいずれかの方法でアプリを配布するには、次のセクションで説明するように、最初に IPA ファイルを作成する必要があります。

### <a name="creating-an-ipa-for-in-house-deployment"></a>社内展開用に IPA を作成する

プロビジョニングが終ったら、アプリケーションを *IPA* と呼ばれるファイルにパッケージ化できます。 これは、アプリケーションとその他のメタデータとアイコンを含む zip ファイルです。 IPA は、アプリケーションをローカルに iTunes に追加して、プロビジョニング プロファイルに含まれているデバイスと直接同期できるようにするために使用されます。

IPA の作成の詳細については、「[IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリケーションの社内配布の概要について説明しました。

## <a name="related-links"></a>関連リンク

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)
