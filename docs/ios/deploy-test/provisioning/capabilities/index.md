---
title: Xamarin.iOS の機能の使用
description: アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。 このガイドでは、すべての機能に必要な設定について説明します。
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 7bb4e142a8b7bd0cf0691da381729dc226028193
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121368"
---
# <a name="working-with-capabilities-in-xamarinios"></a>Xamarin.iOS の機能の使用

_アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。このガイドでは、すべての機能に必要な設定について説明します。_

Apple は、機能を拡張し、iOS アプリで実行可能な操作の範囲を広げる手段として、_アプリ サービス_としてよく知られる_機能_を開発者に提供しています。 この機能を使用して、開発者はアプリケーションにより緊密に統合されたプラットフォーム機能を追加することができます。これにより、Siri などのアプリの追加デバイス サービスからの金融取引の開始が可能になります。
これらの機能は、Xamarin.iOS プロジェクトで使用できます。 サービスの完全なリストを以下に示します。

- アプリ グループ
- 関連付けられているドメイン
- データの保護
- Game Center
- HealthKit
- HomeKit
- Wireless Accessory Configuration
- iCloud
- アプリ内購入
- Inter-App オーディオ
- Apple Pay
- ウォレット
- プッシュ通知
- 個人の VPN
- Siri
- マップ
- バックグラウンド モード
- キーチェーンの共有
- ネットワークの拡張機能
- ホット スポットの構成
- マルチパス
- NFC タグの読み取り

これらの機能は、Visual Studio for Mac か Visual Studio 2019 を介して、または Apple Developer ポータルで手動で有効にすることができます。 ウォレット、Apple Pay、および iCloud などの特定の機能には、アプリ ID の追加構成が必要になります。

このガイドでは、アプリケーションでこれらの各 App Services を、Visual Studio で自動的に有効にする方法と、Developer Center を介して手動で有効にする方法について説明します。また、場合によっては必要になる追加の設定についても説明します。 

## <a name="adding-app-services"></a>App Services の追加

機能を使用するには、アプリに、正しいサービスが有効になっている App ID を含む有効なプロビジョニング プロファイルが必要です。 このプロビジョニング プロファイルは、Visual Studio for Mac や Visual Studio 2019 で自動的に作成するか、Apple Developer Center で手動で作成できます。

このセクションでは、Visual Studio の自動プロビジョニングか Developer Center を使用して、ほとんどの機能を有効にする方法について説明します。 iCloud、Apple Pay、およびアプリ グループなど、追加の設定が必要な機能がいくつかあります。 これらについては隣接するガイドで詳しく説明します。

> [!IMPORTANT]
> 自動プロビジョニングでは、追加したり、管理したりできない機能もあります。 次の一覧には、サポートされている機能が含まれています。
>
>- HealthKit 
>- HomeKit 
>- 個人の VPN 
>- Wireless Accessory Configuration 
>- Inter-App オーディオ 
>- SiriKit 
>- Hotspot 
>- ネットワークの拡張機能 
>- NFC タグの読み取り
>- マルチパス 
>
>プッシュ通知、Game Center、アプリ内購入、マップ、キーチェーンの共有、関連付けられているドメイン、およびデータ保護機能は現在サポートされていません。 これらの機能を追加するには、手動のプロビジョニングを使用し、「[Developer Center](#devcenter)」セクションの手順に従ってください。

## <a name="using-the-ide"></a>IDE の使用

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

機能は、Visual Studio for Mac の **Entitlements.plist** に追加されます。 機能を追加するには次の手順に従います。

1. iOS アプリケーションの **Info.plist** ファイルを開き、コンボ ボックスから **[Automatically Provisioning]** スキームと自分の**チーム**を選択します。 不明な点がある場合は、「[Automatic Provisioning](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)」(自動プロビジョニング) ガイドの手順を参照してください。

    ![[Automatically manage signing] オプション](images/manage-signing.png)

2. **Entitlements.plist** ファイルを開き、追加する機能を選択します。

    ![entitlements.plist ファイルに機能を追加する](images/image17.png)

    機能を選択すると、次の 2 つが実行されます。
    - その機能をアプリ ID に追加する
    - 権利のキー/値のペアを Entitlements.plist ファイルに追加する

    Visual Studio for Mac では、これらのタスクが実行されたときに次の成功メッセージを表示して通知します。

    ![entitlements.plist ファイルに機能を追加する](images/image18.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

機能は **Entitlements.plist** に追加されます。 Visual Studio 2019 の場合、次の手順で機能を追加します。

1. 「[Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」ガイドに基づき、Visual Studio 2019 と Mac をペアリングします。

2. **[プロジェクト]、[Provisioning Properties…]\(プロパティのプロビジョニング...\)** の順に選択し、プロビジョニング オプションを開きます。

3. コンボ ボックスから **[Automatically Provisioning]\(自動プロビジョニング\)** と自分の**チーム**を選択します。 不明な点がある場合は、「[Automatic Provisioning](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)」(自動プロビジョニング) ガイドの手順を参照してください。

    ![[Automatically manage signing] オプション](images/manage-signing-vs.png)

4. **Entitlements.plist** ファイルを開き、追加する機能を選択します。 ファイルを保存します。

    **Entitlement.plist** を保存すると、2 つの動作が行われます。

    - その機能をアプリ ID に追加する
    - 権利のキー/値のペアを Entitlements.plist ファイルに追加する

-----


<a name="devcenter" />

## <a name="using-the-developer-center"></a>Developer Center の使用

Developer Center の使用には 2 ステップのプロセスがあります。アプリ ID を作成してから、そのアプリ ID を使用してプロビジョニング プロファイルを作成する必要があります。 これらの手順を以下で詳しく説明します。

### <a name="creating-an-app-id-with-an-app-service"></a>アプリ サービスでのアプリ ID の作成

1. Mac (Windows コンピューターを使用する場合は、ビルド ホスト mac) 上で [Apple Developer Center](https://developer.apple.com/account) を参照して、ログインします。
2. **[Certificates, Identifiers, and Profiles]\(証明書、ID、プロファイル\)** を選択します。

    ![Apple Developer Center](images/image5.png)

3. **[識別子]** の下の **[アプリ ID]** を選択します。

    ![Developer Center でのアプリ ID の選択](images/image6.png)

4. 右上隅にある **+** ボタンを押し、新しいアプリ ID を作成します。
5. アプリ ID の説明を入力し、[Explicit App ID]\(明示的なアプリ ID\) を選択し、`com.domain.appname` という形式でバンドル ID を入力します。 このバンドル ID は、プロジェクトのバンドル ID と一致する必要があります。

    ![アプリ ID の詳細の追加](images/image7.png)

6. **[App Services]** で、アプリで必要なサービス (複数可) を選択します。

    ![App Services の選択ページ](images/image8.png)

7. **[続行]** を押します。
8. アプリ ID を確認します。 各サービスは、次のいずれかの状態になります: **[有効]** 、 **[無効]** 、 **[構成可能]** (次の図を参照)。 **[有効]** の場合は、プロビジョニング プロファイルで使用する準備ができています。 **[構成可能]** の場合は、この機能に対して追加の設定が必要です。 これらの追加手順については、以降のセクションで詳しく説明します。

    ![アプリ ID の確認](images/image9.png)

9. **[登録]** をクリックしてから **[完了]** をクリックします。 新しく作成されたアプリ ID は、iOS のアプリ ID リストに表示されます。


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>プロビジョニング プロファイルの作成

ここでは、このアプリ ID が含まれるプロビジョニング プロファイルを作成します。 次の手順を実行します。

1. Apple Developer Center で、 **[プロビジョニング プロファイル]、[すべて]** の順に移動します。

    ![[プロビジョニング プロファイル] セクション](images/image10.png)

2. 右上隅にある **+** ボタンを押し、新しいプロビジョニング プロファイルを作成します。
3. 必要なプロビジョニング プロファイルの種類を選択し、 **[続行]** をクリックします。

    ![プロビジョニング プロファイルの選択](images/image11.png)

4. ドロップダウン リストから、上の手順で作成されたアプリ ID を選択し、 **[続行]** を押します。

    ![アプリ ID の選択](images/image12.png)

5. アプリの署名に使用する証明書を選択し、 **[続行]** を押します。

    ![証明書の選択](images/image13.png)

6. このプロファイルに含まれるデバイスを選択し、 **[続行]** を押します。

    ![プロビジョニング プロファイルのデバイスを選択する](images/image14.png)

7. プロファイルを識別できるように名前を付け、 **[続行]** を押してプロファイルを生成します。

    ![プロビジョニング プロファイルに名前を付ける](images/image15.png)

8. **[ダウンロード]** ボタンを押してダウンロードし、ファインダーのファイルをダブルクリックしてプロビジョニング プロファイルをインストールします。

9. Visual Studio を使用している場合、 **[手動プロビジョニング]** オプションが選択されていることを確認します。

10. Visual Studio for Mac と Visual Studio では、 **[プロジェクト オプション] > [バンドルの署名]** の順に参照し、プロビジョニング プロファイルを今作成したものに設定します。

    ![Visual Studio for Mac プロジェクト オプション](images/image16.png)

> [!IMPORTANT]
> Entitlement.plist ファイルの権利キーと、Info.plist ファイルの秘密キーの設定が必要な場合もあります。 これらの権利の詳細については、「[Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md)」 (権利の使用) を参照してください。

<a name="nextsteps" />

## <a name="next-steps"></a>次の手順

サーバー側で機能が有効になっている場合でも、アプリで機能を使用できるようにするために実行する必要がある操作があります。 以下のリストでは、実行する必要がある可能性のある追加の手順について説明します。

- アプリでフレームワークの名前空間を使用します。
- アプリに必要な権利を追加します。 必要な権利とその追加方法については、[権利の概要](~/ios/deploy-test/provisioning/entitlements.md)に関するガイドで詳しく説明しています。

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>トラブルシューティング機能

以下のリストでは、アプリ サービスが有効なアプリを開発する際に障害となる可能性のある最も一般的な問題についていくつか詳しく説明します。

- Apple のデベロッパー ポータルの **[Certificates, IDs & Profiles]\(証明書、ID、およびプロファイル\)** セクションで正しい ID が正しく作成され、登録されていることを確認します。
- サービスがアプリ (または拡張機能) の ID に追加されており、Apple のデベロッパー ポータルの **[Certificates, IDs & Profiles]\(証明書、ID、およびプロファイル\)** で作成されたアプリ グループ/マーチャント ID/コンテナーを使用するように構成されていることを確認します。
- プロビジョニング プロファイルとアプリ ID がインストールされており、アプリの **Info.plist** (Xamarin プロジェクト内にある) で構成済みのアプリ ID のいずれかが使用されていることを確認します。
- アプリの **Entitlements.plist** ファイル (Xamarin プロジェクト内にある) で正しいサービスが有効になっていることを確認します。
- 適切な秘密キーが info.plist に設定されていることを確認します。
- アプリの **[iOS バンドル署名]** で、 **[カスタムの権利]** が **Entitlements.plist** に設定されていることを確認します。 これは、デバッグと iOS シミュレーターのビルドに対する既定の設定では_ありません_。

<a name="summary" />

## <a name="summary"></a>まとめ

このガイドでは機能 (_アプリ サービス_) について説明し、Visual Studio と Apple Developer Center で有効にする方法を説明しました。 ウォレット、iCloud、Apple Pay、およびアプリ グループなど、より複雑なサービスを設定する方法も詳しく説明しました。 最後に、次の設定手順と、単純なトラブルシューティングのオプションについて説明します。
