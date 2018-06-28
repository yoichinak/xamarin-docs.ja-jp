---
title: Xamarin.iOS アプリの無料プロビジョニング
description: このドキュメントでは、Xamarin.iOS の開発者が Apple の有料開発者プログラムに新規登録せずに物理デバイス上でアプリをテストする方法について説明しています。
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 03/19/2017
ms.openlocfilehash: 623f79f482170c6b1d8ecdb642afb2fc7acf061d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786023"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Xamarin.iOS アプリの無料プロビジョニング

_Apple の Xcode 7 リリースでは、すべての iOS と Mac 開発者にとって重大な変更がありました。それが無料プロビジョニングです。_

開発者は無料プロビジョニングを使用して、作成した Xamarin.iOS アプリケーションを自分の iOS デバイスに展開できます。いずれかの **Apple Developer Program** に参加する必要は**ありません**。 デバイス上でのテストにはシミュレーター上のテストよりも多くの利点があるため、開発者にとってはとても便利です。たとえば、メモリ、ストレージ、ネットワーク接続などの利点があります。

Apple Developer アカウントを使用しないプロビジョニングは Xcode で実行する必要があります。これによって*署名 ID* (開発者証明書と秘密キーが含まれます) と*プロビジョニング プロファイル* (明示的なアプリ ID と、接続されている iOS デバイスの UDID が含まれます)。

## <a name="requirements"></a>必要条件

無料プロビジョニングで Xamarin.iOS アプリケーションをデバイスに展開する方法を利用するには、Xcode 7 以降を使用する必要があります。

**使用する Apple ID は、任意の Apple Developer Program に接続されている必要はありません。**

アプリで使用するバンドル ID は一意である必要があり、別のアプリで過去に使用したものは使用できません。 無料プロビジョニングで使用されたバンドル ID は再利用できません。 既にアプリを配布している場合、そのアプリは無料プロビジョニングでプロビジョニングできません。 

詳細については、[アプリの配布](~/ios/deploy-test/app-distribution/index.md)のガイドをご覧ください。

アプリで App Services を使用している場合は、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md#appservices)」(デバイス プロビジョニング) ガイドを参照してプロビジョニング プロファイルを作成する必要があります。 他の制限については、以下の[関連するセクション](#limitations)をご覧ください。


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> アプリを起動する

アプリケーションをデバイスに展開する際に無料プロビジョニングを使用するには、Xcode を使用して署名 ID とプロビジョニング プロファイルを作成し、Visual Studio for Mac または Visual Studio を使用して、アプリへの署名に使用する正しいプロファイルを選択します。 次の手順を実行します。

1. Apple ID を持っていない場合は、[appleid.apple.com](https://appleid.apple.com/account) で作成します。
2. Xcode を開き、**[Xcode]、[Preferences]\(設定\)** の順に参照します。
3. **[Accounts]** \(アカウント\) で **+** ボタンを使用して既存の Apple ID を追加します。 次のスクリーンショットのようになります。

  [![](free-provisioning-images/launchapp1.png "Xcode [Preferences]\(設定\) [Accounts]\(アカウント\)")](free-provisioning-images/launchapp1.png#lightbox)

4. 展開する iOS デバイスを接続し、Xcode で新しい空白で単一ビューの iOS プロジェクトを作成します。 **[Team]** \(チーム\) ドロップダウンを、追加した Apple ID に設定します。 Apple ID は `your name (Personal Team - your Apple ID)` のような形式です。

  [![](free-provisioning-images/launchapp2.png "署名 ID を作成する")](free-provisioning-images/launchapp2.png#lightbox)

5. **[General]\(全般\) の [Identity]\(ID\)** セクションで、バンドル ID が Xamarin.iOS のバンドル ID と_完全に_一致することを確認し、展開対象の数が接続されている iOS デバイス数以下であることを確認します。 Xcode でプロビジョニング プロファイルを作成するにはアプリ ID を指定する必要があるので、この手順はとても重要です。

  [![](free-provisioning-images/launchapp5.png "明示的なアプリ ID でプロビジョニング プロファイルを作成する")](free-provisioning-images/launchapp5.png#lightbox)

6. [Signing]\(署名\) セクションで **[Automatically Manage Signing]** \(署名の自動管理\) を選択し、ドロップダウン リストから自分のチームを選択します。

  [![](free-provisioning-images/launchapp6.png "[Automatically Manage Signing]\(署名の自動管理\) を選択して、ドロップ ダウン リストから自分のチームを選択する")](free-provisioning-images/launchapp6.png#lightbox)

7. 前の手順で、プロビジョニング プロファイルと署名 ID が自動的に生成されます。 この情報を表示するには、プロビジョニング プロファイルの横の情報アイコンをクリックします。

  [![](free-provisioning-images/launchapp7.png "プロビジョニング プロファイルを表示する")](free-provisioning-images/launchapp7.png#lightbox)

8. Xcode でテストするには、[Run]\(実行\) ボタンをクリックして空のアプリケーションをデバイスに展開します。

9. IDE に戻り、同じデバイスを接続し、Xamarin.iOS プロジェクト名を右クリックして、**[プロジェクト オプション]** ダイアログを開きます。 [iOS バンドル署名] セクションを参照し、署名 ID とプロビジョニング プロファイルを明示的に設定します。

  [![](free-provisioning-images/launchapp8.png "署名 ID とプロビジョニング プロファイルを設定する")](free-provisioning-images/launchapp8.png#lightbox)

IDE に署名 ID または正しいプロビジョニング プロファイルが表示されない場合は、必要に応じて IDE を再起動します。


## <a name="a-namelimitations-limitations"></a><a name="limitations" />制限事項

Apple は、無料プロビジョニングを使用して iOS デバイスでアプリケーションを実行できるタイミングと方法について複数の制限を課し、*開発者自身*のデバイスにのみ展開できるように確保しています。 ここでは、その制限を紹介します。

iTunes Connect へのアクセスも制限されるので、アプリケーションを無料プロビジョニングする開発者は、App Store と TestFlight への発行などのサービスを使用できません。 アドホック配布または社内配布を利用するには、Apple Developer アカウント (法人または個人) が必要です。

この方法で作成したプロビジョニング プロファイルは 1 週間後、署名 ID は 1 年後に期限切れになります。 さらに、プロビジョニング プロファイルを作成するには、アプリ ID を指定する必要があるため、インストールするすべてのアプリについて[前述](#launching)の手順を実行する必要があります。

また、無料プロビジョニングでは、ほとんどの App Services のプロビジョニングを使用できません。 バインディングには、以下の項目が含まれます。

- Apple Pay
- Game Center
- iCloud
- アプリ内購入
- プッシュ通知
- ウォレット (旧称 Passbook)

詳細な一覧については、Apple の「[Supported Capabilities](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1)」(サポートされる機能) ガイドを参照してください。 App Services で使用するアプリをプロビジョニングする方法については、「[Working with Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md)」(機能の使用) ガイドを参照してください。


## <a name="summary"></a>まとめ

このガイドでは、無料プロビジョニングを使用して iOS デバイスにアプリケーションをインストールする場合の利点と制限について説明しました。 また、無料プロビジョニングを使用して Xamarin.iOS アプリをインストールする手順についても説明しました。

## <a name="related-links"></a>関連リンク

- [デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)
- [アプリケーション サービスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md#appservices)
