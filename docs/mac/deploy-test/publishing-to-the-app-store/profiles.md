---
title: Xamarin.Mac アプリのプロビジョニング プロファイル
description: このガイドでは、Xamarin.Mac アプリを発行するのに必要なプロビジョニング プロファイルを作成する手順について説明します。
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 04/12/2017
ms.openlocfilehash: de87e650f68e5a327564c0a4216bee2b3e04fe2f
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697606"
---
# <a name="provisioning-profiles-for-xamarinmac-apps"></a>Xamarin.Mac アプリのプロビジョニング プロファイル

プロビジョニング プロファイルを使用すると、開発者が複数の macOS (旧称 Mac OS X) 固有の機能 (iCloud やプッシュ通知など) を Xamarin.Mac アプリに組み込むことができます。 これらの機能を使用する開発中の各アプリケーション用の Mac プロビジョニング プロファイルを作成、ダウンロード、およびインストールする必要があります。

[![Apple Provisioning Portal](profiles-images/certif13.png)](profiles-images/certif13.png#lightbox)

## <a name="development-provisioning-profile"></a>開発プロビジョニング プロファイル

開発プロビジョニング プロファイルを使用すると、そのプロファイルで設定されている特定のコンピューター上で、Mac App Store を対象としたアプリをテストすることができます。 これは、iCloud やプッシュ通知などの macOS の機能を使用する場合に特に関係します。

> [!NOTE]
> 開発者は、開発プロビジョニング プロファイルを作成する前に、Mac 開発証明書が既に作成されている必要があります。 このスクリーンショットに示すように詳細を完了し、ビルドを作成するために使用できる **開発プロビジョニング プロファイル** を生成します。 有効な Mac 開発証明書を **[証明書]** ボックスで選択できる必要があり、さらに少なくとも 1 つのシステムがテスト用に登録されている必要があります。

次の手順で行います。

1. 作成するプロビジョニング プロファイルの種類を選択し、 **[続行]** ボタンをクリックします。

    [![プロファイルの種類の選択](profiles-images/certif14.png)](profiles-images/certif14.png#lightbox)
2. プロファイルを作成する対象のアプリケーションの ID を選択し、 **[続行]** ボタンをクリックします。

    [![アプリ ID の選択](profiles-images/certif15.png)](profiles-images/certif15.png#lightbox)
3. プロファイルに署名するために使用する開発者 ID を選択し、 **[続行]** をクリックします。

    [![開発者 ID の選択](profiles-images/certif16.png)](profiles-images/certif16.png#lightbox)
4. このプロファイルを使用できるコンピューターを選択し、 **[続行]** をクリックします。

    [![許可するコンピューターの選択](profiles-images/certif17.png)](profiles-images/certif17.png#lightbox)
5. 次に、**プロファイル名** を入力し、 **[生成]** ボタンをクリックします。

    [![スクリーンショットには、プロビジョニング ウィンドウへのプロファイル名の入力が示されています。](profiles-images/certif18.png)](profiles-images/certif18.png#lightbox)
6. **[ダウンロード]** ボタンをクリックし、新しいプロファイルをダウンロードします。

    [![スクリーンショットには、プロファイルの [ダウンロード] ボタンが示されています。](profiles-images/certif19.png)](profiles-images/certif19.png#lightbox)
7. Mac の **システム環境設定** アプリケーションの [Profiles Preferences]\(プロファイルの設定\) ウィンドウに開発プロビジョニング プロファイルがインストールされます。

    [![スクリーンショットには、インストールの確認ダイアログ ボックスが示されています。](profiles-images/certif20.png)](profiles-images/certif20.png#lightbox)
8. [プロファイルの設定] ウィンドウにインストールされているすべてのプロファイルが表示されます。

    [![すべてのインストールされているプロファイルの表示](profiles-images/image47.png)](profiles-images/image47.png#lightbox)
9. もう一度ダウンロードする必要がある場合は、プロファイルは、 **[Developer Certificate Utility]** \(開発者の証明書ユーティリティ\) にも表示されます。

    [![Developer Certificate Utility](profiles-images/image48.png)](profiles-images/image48.png#lightbox)

新しいアプリごとにまたは新しいコンピューターをテストに追加するたびに新しい開発プロビジョニング プロファイルを作成する必要があります。

## <a name="production-provisioning-profile"></a>実稼働プロビジョニング プロファイル

Mac App Store に送信するパッケージをビルドするには、実稼働プロビジョニング プロファイルが必要です。

次の手順で行います。

1. 作成するプロファイルの種類を選択し、 **[続行]** ボタンをクリックします。

    [![プロファイルの種類の選択](profiles-images/certif21.png)](profiles-images/certif21.png#lightbox)
2. プロファイルを作成する対象のアプリの ID を選択し、 **[続行]** ボタンをクリックします。

    [![アプリ ID の選択](profiles-images/certif15.png)](profiles-images/certif15.png#lightbox)
3. プロファイルに署名するための会社 ID を選択し、 **[続行]** ボタンをクリックします。

    [![会社 ID の選択](profiles-images/certif23.png)](profiles-images/certif23.png#lightbox)
4. **プロファイル名** を入力し、 **[生成]** ボタンをクリックします。

    [![プロファイルの生成](profiles-images/certif24.png)](profiles-images/certif24.png#lightbox)
5. **[ダウンロード]** をクリックして、プロビジョニング プロファイル ファイル (拡張子 `.provisionprofile`) を取得します。

    [![プロファイルのダウンロード](profiles-images/certif25.png)](profiles-images/certif25.png#lightbox)
6. **Xcode オーガナイザー** にドラッグするか、ダブルクリックしてインストールします。 プロファイルは、Xcode オーガナイザーに表示されます。

    [![プロファイルのインストール](profiles-images/image51.png)](profiles-images/image51.png#lightbox)
7. プロビジョニング プロファイルも一覧に表示されます。

    [![インストールされているプロファイルの表示](profiles-images/certif26.png)](profiles-images/certif26.png#lightbox)

開発者が、アプリ ID で使用されている機能を変更する場合 ( iCloud またはプッシュ通知の有効化など)、そのアプリ ID のプロビジョニング プロファイルを再作成する必要があります。

## <a name="related-links"></a>関連リンク

- [インストール](~//mac/get-started/installation.md)
- [Hello Mac のサンプル](~//mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [ツール ガイド: アプリのコード署名](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID と GateKeeper](https://developer.apple.com/developer-id/)
