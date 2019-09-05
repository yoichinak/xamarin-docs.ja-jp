---
title: Xamarin.iOS アプリの無料プロビジョニング
description: このドキュメントでは、Xamarin.iOS の開発者が Apple の有料開発者プログラムに新規登録せずに物理デバイス上でアプリをテストする方法について説明しています。
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/16/2018
ms.openlocfilehash: 8279487fc5effd5c2c019bffa5ceb820d2240400
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291433"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Xamarin.iOS アプリの無料プロビジョニング

無料プロビジョニングを使用すると、Xamarin.iOS の開発者は **Apple Developer Program** に参加**することなく**、iOS デバイス上でアプリの展開およびテストができます。
シミュレーター テストは便利で使いやすいですが、アプリが実際のメモリ、ストレージ、ネットワーク接続の制約下で正しく機能することを確認するため、実際の iOS デバイスでテストすることが重要です。

無料プロビジョニングを使用してアプリをデバイスに展開するには:

- Xcode を使用して、必要な*署名 ID* (開発者の証明書と秘密キー) と*プロビジョニング プロファイル* (明示的なアプリ ID と接続されている iOS デバイスの UDID を含む) を作成します。
- Xamarin.iOS アプリケーションを展開するには、Visual Studio for Mac または Visual Studio 2019 の Xcode で作成された署名 ID とプロビジョニング プロファイルを使用します。

> [!IMPORTANT]
> [自動プロビジョニング](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)により、Visual Studio for Mac または Visual Studio 2019 で開発者テスト用のデバイスを自動的にセットアップできるようになります。 ただし、自動プロビジョニングは無料プロビジョニングとの互換性がありません。 自動プロビジョニングを使用するには、有料の Apple Developer Program アカウントが必要です。

## <a name="requirements"></a>必要条件

無料プロビジョニングを使用して Xamarin.iOS アプリケーションをデバイスに展開するには:

- 使用する Apple ID が Apple Developer Program に接続されている必要はありません。
- Xamarin.iOS アプリが、ワイルドカードのアプリ ID ではなく明示的なアプリ ID を使用する必要があります。
- Xamarin.iOS アプリで使用するバンドル ID は一意である必要があり、別のアプリで過去に使用したものは使用できません。 無料プロビジョニングで使用されたバンドル ID は再利用**できません**。
- 既にアプリを配布している場合、そのアプリは無料プロビジョニングで展開できません。
- アプリで App Services を使用している場合は、[デバイス プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services)のガイドを参照してプロビジョニング プロファイルを作成する必要があります。 

無料プロビジョニングに関連する制限事項についてはこのドキュメントの「[制限事項](#limitations)」のセクションを、iOS アプリケーションの配布については、[アプリ配布のガイド](~/ios/deploy-test/app-distribution/index.md)を参照してください。

## <a name="testing-on-device-with-free-provisioning"></a>無料プロビジョニングを使用したデバイスのテスト

無料プロビジョニングで Xamarin.iOS アプリをテストする場合は、次の手順に従います。

### <a name="use-xcode-to-create-a-signing-identity-and-provisioning-profile"></a>Xcode を使用して署名 ID とプロビジョニング プロファイルを作成する

1. Apple ID を持っていない場合は、[作成します](https://appleid.apple.com)。
2. Xcode を開き、 **[Xcode]、[Preferences]\(設定\)** の順に移動します。
3. **[Accounts]** \(アカウント\) で **+** ボタンを使用して既存の Apple ID を追加します。 次のスクリーンショットのようになります。

    ![Xcode の設定 – アカウント](free-provisioning-images/launchapp1.png "Xcode の設定 – アカウント")

4. Xcode の設定を閉じます。
5. アプリの展開先に iOS デバイスを接続します。
6. Xcode で新しいプロジェクトを作成します。 **[File]\(ファイル\)、[New]\(新規\)、[Project]\(プロジェクト\)** の順に移動して、 **[Single View App]\(単一ビュー アプリ\)** を選択します。
7. 新しいプロジェクト ダイアログで、 **[Team]\(チーム\)** に今追加した Apple ID を設定します。 ドロップダウン リストでは、 **[Your Name (Personal Team)]\(ユーザーの名前 (個人チーム)\)** のように表示されます。

    ![新しいアプリの作成](free-provisioning-images/launchapp2.png "新しいアプリの作成")

8. 新しいプロジェクトが作成されたら、(シミュレーターではなく) iOS デバイスを対象とする Xcode ビルド スキームを選択します。

    ![Xcode のビルド スキームの選択](free-provisioning-images/xcodescheme.png "Xcode のビルド スキームの選択")

9. Xcode の**プロジェクト ナビゲーター**で最上位のノードを選択して、アプリのプロジェクトの設定を開きます。
10. **[General]\(一般\)、[Identity]\(ID\)** の下で、**バンドル ID** が Xamarin.iOS アプリのバンドル ID と_完全に一致している_ことを確認します。

    ![バンドル ID の設定](free-provisioning-images/launchapp5.png "バンドル ID の設定")

    > [!IMPORTANT]
    > Xcode は明示的なアプリ ID のプロビジョニング プロファイルのみを作成し、この ID は Xamarin.iOS アプリのアプリ ID と同一である必要があります。
    > 異なる場合、無料プロビジョニングを使用して Xamarin.iOS アプリを展開できなくなります。

11. **[Deployment Info]\(展開情報\)** の下で、展開対象の iOS のバージョンが、接続されている iOS デバイスにインストールされているバージョン以下であることを確認します。
12. **[Signing]\(署名\)** の下で、 **[Automatically manage signing]\(署名の自動管理\)** を選択して、ドロップダウン リストから自分のチームを選びます。

    ![署名の自動管理](free-provisioning-images/launchapp6.png "署名の自動管理")

    Xcode で、プロビジョニング プロファイルと署名 ID が自動的に生成されます。 この情報を表示するには、プロビジョニング プロファイルの横の情報アイコンをクリックします。

    ![プロビジョニング プロファイルの表示](free-provisioning-images/launchapp7.png "プロビジョニング プロファイルの表示")

    > [!TIP]
    > Xcode でのプロビジョニング プロファイルの生成時にエラーが発生した場合は、Xcode の現在選択されているビルド スキームが、シミュレーターではなく接続されている iOS デバイスを対象としていることを確認してください。

13. Xcode でテストするには、[Run]\(実行\) ボタンをクリックして空のアプリケーションをデバイスに展開します。

### <a name="deploy-your-xamarinios-app"></a>Xamarin.iOS アプリを展開する

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. iOS デバイスを USB 経由または[ワイヤレス](~/ios/deploy-test/wireless-deployment.md)で Mac ビルド ホストに接続します。
2. Visual Studio for Mac の **Solution Pad** で **Info.plist** をダブルクリックします。
3. **[署名]** で **[手動プロビジョニング]** を選択します。
4. **[iOS バンドル署名]** ボタンをクリックします。 を追加します。
5. **[構成]** に **[デバッグ]** を選択します。
6. **[プラットフォーム]** に **[iPhone]** を選択します。
7. Xcode で作成された**署名 ID** を選択します。
8. Xcode で作成された**プロビジョニング プロファイル**を選択します。

    ![署名 ID とプロビジョニング プロファイルの設定](free-provisioning-images/launchapp8.png "署名 ID とプロビジョニング プロファイルの設定")

    > [!TIP]
    > 署名 ID または正しいプロビジョニング プロファイルが表示されない場合は、必要に応じて Visual Studio for Mac を再起動します。

9. **[OK]** をクリックし、 **[プロジェクト オプション]** を保存して閉じます。
10. iOS デバイスを選択し、アプリを実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2019 または Visual Studio 2017 が [Mac ビルド ホストとペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)されていることを確認します。
2. iOS デバイスを USB 経由または[ワイヤレス](~/ios/deploy-test/wireless-deployment.md)で Mac ビルド ホストに接続します。
3. Visual Studio 2019 または Visual Studio 2017 の**ソリューション エクスプローラー**で、Xamarin.iOS プロジェクトを右クリックし、 **[プロパティ]** を選択します。
4. **[iOS バンドル署名]** に移動します。
5. **[構成]** に **[デバッグ]** を選択します。
6. **[プラットフォーム]** に **[iPhone]** を選択します。
7. **[手動プロビジョニング]** を選択します。
8. Xcode で作成された**署名 ID** を選択します。
9. Xcode で作成された**プロビジョニング プロファイル**を選択します。
    
    ![署名 ID とプロビジョニング プロファイルの設定](free-provisioning-images/setprofile-w157.png "署名 ID とプロビジョニング プロファイルの設定")

    > [!TIP]
    > この署名 ID とプロビジョニング プロファイルは Xcode により作成され、Mac ビルド ホストに格納されました。 Mac ビルド ホストとは[ペアリングされている](~/ios/get-started/installation/windows/connecting-to-mac/index.md)ため、Visual Studio 2019 または Visual Studio 2017 からアクセスすることができます。 一覧に表示されない場合は、場合によって Visual Studio 2019 または Visual Studio 2017 を再起動する必要があります。

10. プロジェクトのプロパティを保存して閉じます。
11. iOS デバイスを選択し、アプリを実行します。

-----

## <a name="limitations"></a>制限事項

Apple は、無料プロビジョニングを使用して iOS デバイスでアプリケーションを実行できるタイミングと方法について複数の制限を課し、*開発者自身*のデバイスにのみ展開できるように確保しています。

- iTunes Connect へのアクセスが制限されるので、アプリケーションを無料プロビジョニングする開発者は、App Store と TestFlight への発行などのサービスを使用できません。 アドホック配布または社内配布を利用するには、Apple Developer アカウント (法人または個人) が必要です。
- 無料プロビジョニングで作成したプロビジョニング プロファイルは 1 週間後、署名 ID は 1 年後に期限切れになります。 
- Xcode は明示的なアプリ ID のプロビジョニング プロファイルのみを作成するため、インストールするすべてのアプリについて[前述](#testing-on-device-with-free-provisioning)の手順を実行する必要があります。
- 無料プロビジョニングでは、ほとんどのアプリケーション サービスのプロビジョニングを使用できません。 これには、Apple Pay、Game Center、iCloud、アプリ内購入、プッシュ通知、および Wallet が含まれます。 Apple の「[Supported capabilities (iOS)](https://help.apple.com/developer-account/#/dev21218dfd6)」(サポートされている機能 (iOS)) ガイドに、すべての機能のリストを提供しています。 アプリケーション サービスで使用するアプリをプロビジョニングする方法については、[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)のガイドを参照してください。

## <a name="summary"></a>まとめ

このガイドでは、無料プロビジョニングを使用して iOS デバイスにアプリケーションをインストールする場合の利点と制限について説明しました。 無料プロビジョニングを使用して Xamarin.iOS アプリをインストールする方法を示した、ステップ バイ ステップのチュートリアルを提供しました。

## <a name="related-links"></a>関連リンク

- [デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)
- [アプリケーション サービスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services)
