---
title: Xamarin.iOS の自動プロビジョニング
description: Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。 このガイドでは、自動署名を使用して、開発証明書とプロファイルを要求する方法について説明します。
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 03/05/2020
ms.openlocfilehash: 069c40b74876bea1d3a0c8fca23b3d90c4b91635
ms.sourcegitcommit: 997f7b6a1a1bc50b98c3ca5bbc75d6875ba2ae9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510680"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Xamarin.iOS の自動プロビジョニング

_Xamarin.iOS が正常にインストールされたら、iOS 開発の次の手順は、iOS デバイスをプロビジョニングすることです。このガイドでは、自動プロビジョニングを使用して、開発証明書とプロファイルを要求する方法について調べます。_

## <a name="requirements"></a>必要条件

自動プロビジョニングは、Visual Studio for Mac、Visual Studio 2019、Visual Studio 2017 (バージョン 15.7 以降) で利用できます。 

> [!NOTE]
> また、この機能を使用するには、Apple Developer 有料アカウントも必要です。 Apple Developer アカウントの詳細については、[デバイス プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)のガイドを参照してください。
> 有料の Apple 開発者アカウントをお持ちでない場合は、[Xamarin.iOS の無料プロビジョニング] (~/ios/get-started/installation/device-provisioning/free-provisioning.md) ガイドを参照してください。

> [!NOTE]
> 開始する前に、[Apple Developer ポータル](https://developer.apple.com/account/)または [App Store Connect](https://appstoreconnect.apple.com/) のいずれかで、まずライセンス契約に同意してください。


## <a name="enable-automatic-provisioning"></a>自動プロビジョニングを有効にする

自動署名プロセスを開始する前に、「[Apple のアカウント管理](~/cross-platform/macios/apple-account-management.md)」ガイドに記載されているとおりに、Visual Studio で Apple ID を確実に追加しておく必要があります。 

Apple ID を追加すると、関連する_チーム_を使用できます。 こうすることで、チームに対して証明書、プロファイル、およびその他の ID を作成できます。 プロビジョニング プロファイルに含まれるアプリ ID のプレフィックスを作成するためにもチーム ID が使用されます。 チーム ID を使用することで、Apple は開発者の身元を検証できます。

iOS デバイスで開発のためにアプリに自動的に署名するには、次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で iOS プロジェクトを開きます。

2. **Info.plist** ファイルを開きます。

3. **[署名]** セクションで、 **[自動プロビジョニング]** を選択します。

    ![チーム セレクターのドロップダウン](automatic-provisioning-images/image2.png)

4. **[チーム]** ドロップダウンからチームを選択します。

5. 数秒後に、署名証明書とプロビジョニング プロファイルが作成されます。

    ![証明書とプロファイルが正常に作成されました](automatic-provisioning-images/image5.png)

    自動署名に失敗すると、**自動署名パッド**にエラーの理由が表示されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Visual Studio 2017 または Visual Studio 2019 (バージョン 16.4 以前) を使用している場合は、続行する前に、[Mac ビルド ホストとペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)必要があります。

1. **ソリューション エクスプローラー**で、iOS プロジェクト名を右クリックして **[プロパティ]** を選択します。 次に、 **[iOS バンドル署名]** タブに移動します。

    ![iOS のプロパティの [バンドル署名] ページのスクリーンショット。](automatic-provisioning-images/bundle-signing-win.png)

2. **[自動プロビジョニング]** スキームを選択します。

3. **[チーム]** ドロップダウン メニューから自分のチームを選択し、自動署名プロセスを開始します。 Visual Studio によって、処理が正常に完了したかどうかが示されます。

    !["自動プロビジョニングが正常に完了しました" というメッセージが強調表示されている [バンドル署名] ページのスクリーンショット。](automatic-provisioning-images/signing-success-win.png)

-----

## <a name="run-automatic-provisioning"></a>自動プロビジョニングの実行

自動プロビジョニングが有効になっている場合、次のいずれかが発生すると、必要に応じて Visual Studio でプロセスが再実行されます。

- iOS デバイスを Mac に接続した
  - これで、デバイスが Apple Developer Portal に登録されているかどうかが自動的に確認されます。 これが行われていない場合は追加され、それを含む新しいプロビジョニング プロファイルが生成されます。
- アプリのバンドル ID が変更された
  - これでアプリ ID が更新されます。 このアプリ ID が含まれる新しいプロビジョニング プロファイルが作成されます。
- Entitlements.plist ファイルで、サポートされる機能が有効になります。
  - この機能がアプリ ID に追加され、アプリ ID が更新された新しいプロビジョニング プロファイルが生成されます。
  - 現在はサポートされていない機能もあります。 サポートされる機能の詳細については、「[Working with Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md)」(機能の使用) ガイドを参照してください。

## <a name="wildcard-app-ids"></a>ワイルドカード アプリ ID

Visual Studio for Mac および Visual Studio 2019 (バージョン 16.5 以降) では、自動プロビジョニングでは、**Info.plist** に指定されている**バンドル識別子**に基づいた明示的なアプリ ID の代わりに、既定で、ワイルドカード アプリ ID とプロビジョニング プロファイルが作成され、使用されるようになります。 ワイルドカード アプリ ID により、Apple Developer Portal で維持するプロファイルと ID の数が減ります。

場合によっては、アプリ権利で明示的なアプリ ID が必要になることがあります。 次の権利では、ワイルドカード アプリ ID がサポートされていません。

- アプリ グループ
- 関連付けられているドメイン
- Apple Pay
- Game Center
- HealthKit
- HomeKit
- Hotspot
- アプリ内購入
- マルチパス
- NFC
- 個人の VPN
- プッシュ通知
- Wireless Accessory Configuration

アプリでこれらのいずれかの権利を使用している場合、Visual Studio によって、(ワイルドカードではなく) 明示的なアプリ ID の作成が試みられます。

## <a name="troubleshoot"></a>トラブルシューティング 

- 新しい Apple Developer アカウントが承認されるまでに数時間かかることがあります。 アカウントが承認されるまで、自動プロビジョニングを有効にすることはできません。
- 自動プロビジョニング プロセスがエラー メッセージ `Authentication Service Is Unavailable` で失敗した場合は、[App Store Connect](https://appstoreconnect.apple.com/) または [appleid.apple.com](https://appleid.apple.com) のいずれかにサインインして、最新のサービス契約に同意していることを確認します。
- エラー メッセージ `Authentication Error: Xcode 7.3 or later is required to continue developing with your Apple ID.` が表示された場合は、選択したチームが Apple Developer Program に対してアクティブな有料メンバーシップを持っていることを確認してください。 Apple Developer 有料アカウントを使用するには、「[Xamarin.iOS アプリの無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)」ガイドを参照してください。

## <a name="related-links"></a>関連リンク

- [無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [アプリの配布](~/ios/deploy-test/app-distribution/index.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
- [Apple - アプリ配布ガイド](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
