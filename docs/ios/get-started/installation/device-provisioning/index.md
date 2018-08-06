---
title: Xamarin.iOS のデバイス プロビジョニング
description: このドキュメントでは、アプリケーションのテストに利用できるようにデバイスをプロビジョニングする方法について説明します。 プッシュ通知などの機能を使用できるようにアプリを構成する方法についても説明します。
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: f0d6d2343350455a101033aced7cec0c31695503
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353231"
---
# <a name="device-provisioning-for-xamarinios"></a>Xamarin.iOS のデバイス プロビジョニング

Xamarin.iOS アプリケーションを開発する場合、シミュレーターだけでなく、物理デバイスにもアプリを展開してテストすることが重要です。 デバイスで実行すると、メモリやネットワーク接続などのハードウェアの制限があるため、デバイス独自のバグやパフォーマンスの問題が発生する可能性があります。 物理デバイスでテストするには、デバイスを*プロビジョニング*し、Apple にテスト用にデバイスが使用されることを通知する必要があります。

下図で強調表示されているセクションは、iOS のプロビジョニングの設定に必要な手順を示しています。

[![](images/provisioningdiagram.png "この図で強調表示されているセクションは、iOS のプロビジョニングの設定に必要な手順を示しています")](images/provisioningdiagram.png#lightbox)

次の手順は、アプリケーションの配布です。 展開の詳細については、「[App Distribution](~/ios/deploy-test/app-distribution/index.md)」(アプリの配布) ガイドを参照してください。

デバイスにアプリケーションを展開する前に、Apple の Developer Program の有効なサブスクリプションがあるか *、*[無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)を使用する必要があります。 Apple は 2 つのプログラム オプションを用意しています。

- **Apple Developer Program**: 個人か組織の代表かにかかわらず、Apple Developer Program では、アプリの開発、テスト、および配布を行うことができます。
- **Apple Developer Enterprise Program**: Enterprise プログラムは、アプリを開発し、社内でのみ配布する組織に最適です。 Enterprise プログラムのメンバーは、iTunes Connect にはアクセスできません。また、作成したアプリは App Store に公開できません。

これらのいずれかのプログラムに登録するには、[Apple Developer Portal](https://developer.apple.com/programs/enroll/) にアクセスします。 Apple 開発者として登録するには、[Apple ID](https://appleid.apple.com/) を持っている必要があります。 このガイドは、読者が Apple Developer Program の**メンバーである**と仮定して作成しています。

また、Apple は Xcode 7 で[無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)を導入しました。Apple の Developer Program の*メンバーにならなくても*、単一のデバイス上で単一のアプリケーションを実行できます。 この方法でプロビジョニングする場合は、複数の制限があります。詳細については、[こちら](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations)を参照してください。

デバイス上で実行するアプリケーションには、一連のメタデータ (または*サムプリント*) を含める必要があります。メタデータには、アプリケーションと開発者に関する情報が含まれています。 Apple はこのサムプリントを使用して、ユーザーのデバイスに展開するとき、または実行されるときにアプリケーションが改ざんされていないことを確認します。 メタデータを含めるには、アプリ開発者が開発者として Apple ID を登録し、アプリ ID を設定し、証明書を要求し、アプリケーションを展開するデバイスを登録する必要があります。

アプリケーションをデバイスに展開すると、プロビジョニング プロファイルも iOS デバイスにインストールされます。 プロビジョニング プロファイルは、アプリがビルド時に署名され、Apple によって暗号化された方法で署名されていることを検証するために存在します。 プロビジョニング プロファイルと "サムプリント" の両方を使い、以下の情報を確認してデバイスにアプリケーションを展開できるかどうかが決定されます。

- **発行元** (証明書: プロビジョニング プロファイルに対応する公開キーがある秘密キーでアプリが署名されているかどうか。 証明書は、開発者と開発チームを関連付けるためにも使用されます)
- **内容** (個人のアプリ ID: Info.plist のバンドル識別子がプロビジョニング プロファイルのアプリ ID と一致するかどうか)
- **場所** (デバイス: デバイスはプロビジョニング プロファイルに含まれているかどうか)

これらの手順で、開発プロセス中に作成または使用されるすべて (アプリケーションとデバイスを含む) を Apple Developer アカウントまでトレースできます。

## <a name="provisioning-your-device"></a>デバイスのプロビジョニング

iOS デバイスをプロビジョニングする方法は 2 つあります。

* **自動 (推奨)** - プロジェクトで **[自動プロビジョニング]** スキームを選択すると、Visual Studio で署名 ID、アプリ ID、プロビジョニング プロファイルが自動的に作成され、管理されます。 プロビジョニングを自動的に管理する方法については、「[自動プロビジョニング](automatic-provisioning.md)」ガイドを参照してください。 これは、iOS デバイスをプロビジョニングする場合にお勧めの方法です。

* 
  **手動** - ID、アプリケーション ID、およびプロビジョニング プロファイルの署名は、「[手動プロビジョニング](manual-provisioning.md)」ガイドに記載されているように、Apple Developer Portal を使用して作成および管理できます。 これらのアーティファクトは、「[Apple Account Management](~/cross-platform/macios/apple-account-management.md)」(Apple アカウント管理) ガイドの説明に従って管理できます。

## <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、Xamarin.iOS アプリケーション用にアクティブ化できる機能とも呼ばれる特別なアプリケーション サービスの選択肢を提供します。 **アプリ ID** が作成されるときに、両方の iOS プロビジョニング ポータル、および Xamarin.iOS のプロジェクトの一部である **Entitlements.plist** ファイルでこれらのアプリケーション サービスを構成する必要があります。 アプリにアプリケーション サービスを追加する方法については、「[Introduction to Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md)」(機能の概要) ガイドおよび「[Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md)」 (権利に関する作業) ガイドを参照してください。

* 必要なアプリ サービスを使用してアプリ ID を作成します。
* このアプリ ID が含まれる新しい[プロビジョニング プロファイル](#provisioning-your-device)を作成します。
* Xamarin.iOS プロジェクトでの権利を設定します。

## <a name="related-links"></a>関連リンク

- [無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [アプリの配布](~/ios/deploy-test/app-distribution/index.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
- [Apple - アプリ配布ガイド](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
