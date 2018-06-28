---
title: Xamarin.iOS の自動プロビジョニング
description: Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。 このガイドでは、自動署名を使用して、開発証明書とプロファイルを要求する方法について説明します。
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: 323174b4a37a12828a32acb398fef63cd9b849e3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785818"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Xamarin.iOS の自動プロビジョニング

_Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。このガイドでは、自動署名を使用して、開発証明書とプロファイルを要求する方法について説明します。_

## <a name="requirements"></a>必要条件

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Visual Studio for Mac 7.3 以降
- Xcode 9 以降

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 バージョン 15.7 (以降)

また、次に該当する Mac ビルド ホストとペアリングする必要があります。

- Xcode 9 以降

-----

## <a name="enabling-automatic-signing"></a>自動署名を有効にする

自動署名プロセスを開始する前に、Visual Studio で Apple ID を追加しておく必要があります。手順については、「[Apple アカウント管理](~/cross-platform/macios/apple-account-management.md)」ガイドを参照してください。 Apple ID を追加すると、関連する_チーム_を使用できます。 こうすることで、チームに対して証明書、プロファイル、およびその他の ID を作成できます。 プロビジョニング プロファイルに含まれるアプリ ID のプレフィックスを作成するためにもチーム ID が使用されます。 チーム ID を使用することで、Apple は開発者の身元を検証できます。

> [!IMPORTANT]
> 開始する前に、[iTunes Connect](https://itunesconnect.apple.com/) または [appleid.apple.com](https://appleid.apple.com) にサインインして、最新の Apple アカウント ポリシーを承諾していることを確認します。 メッセージが表示された場合は、Apple からの新しいアカウント契約に同意する手順を完了します。 2018 年 5 月からのプライバシー契約を承諾しない場合、デバイスをプロビジョニングしようとする際に次の警告を受け取ります。
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

iOS デバイスで開発のためにアプリに自動的に署名するには、次の手順を実行します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac で iOS プロジェクトを開きます。

2. **Info.plist** ファイルを開きます。

3. **[署名]** セクションで、**[自動プロビジョニング]** を選択します。

    ![チーム セレクターのドロップダウン](automatic-provisioning-images/image2.png)

4. **[チーム]** ドロップダウンからチームを選択します。

6. 数秒後に、署名証明書とプロビジョニング プロファイルが作成されます。

    ![証明書とプロファイルが正常に作成されました](automatic-provisioning-images/image5.png)

    自動署名に失敗すると、**自動署名パッド**にエラーの理由が表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 「[Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」ガイドに基づき、Visual Studio 2017 と Mac をペアリングします。

2. **[プロジェクト]、[Provisioning Properties…]\(プロパティのプロビジョニング...\)** の順に選択し、プロビジョニング オプションを開きます。

3. **[Automatically Provisioning]\(自動プロビジョニング\)** スキームを選択します。

    ![自動スキームの選択](automatic-provisioning-images/prov4.png)

4. **[チーム]** コンボ ボックスからチームを選択し、自動署名プロセスを開始します。

    ![チームの選択](automatic-provisioning-images/prov3.png)

4. これで自動署名プロセスが始まります。 次に、Visual Studio によって App ID の生成が試行されます。プロファイルとこれらの成果物を署名に使用するための署名 ID がプロビジョニングされます。 ビルド出力で生成プロセスを確認できます。

    ![ビルド出力で確認できる成果物の生成](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>自動プロビジョニングのトリガー

自動署名を有効にすると、次のいずれかの状況になったときに、Visual Studio for Mac で必要に応じてこれらのアーティファクトが更新されます。

* iOS デバイスを Mac に接続した
    - これで、デバイスが Apple Developer Portal に登録されているかどうかが自動的に確認されます。 登録されていない場合は追加され、それを含む新しいプロビジョニング プロファイルが生成されます。
* アプリのバンドル ID が変更された
    - これでアプリ ID が更新されます。 このアプリ ID が含まれる新しいプロビジョニング プロファイルが作成されます。
* Entitlements.plist ファイルで、サポートされる機能が有効になります。
    - この機能がアプリ ID に追加され、アプリ ID が更新された新しいプロビジョニング プロファイルが生成されます。
    - 現在はサポートされていない機能もあります。 サポートされる機能の詳細については、「[Working with Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md)」(機能の使用) ガイドを参照してください。


## <a name="related-links"></a>関連リンク

- [無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [アプリの配布](~/ios/deploy-test/app-distribution/index.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
- [Apple - アプリ配布ガイド](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
