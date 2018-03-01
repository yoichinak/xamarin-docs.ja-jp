---
title: "Apple アカウント管理"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/05/2017
ms.openlocfilehash: 0cf7456cec2e934516e15ac6cbc57109e6b57a79
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="apple-account-management"></a>Apple アカウント管理

Apple アカウント管理のインターフェイスは、Apple ID を持つ関連するすべての開発チームを表示する方法を提供します。 一覧を表示することによって各チームの詳細を表示することもできます_署名 Id_と_プロビジョニング プロファイル_コンピューターにインストールされています。

Apple ID の認証は使用してコマンドラインで実行[がありません](https://fastlane.tools/)です。 ありませんは、正常に認証するためのコンピューターにインストールする必要があります。 詳細についてがなく、そのインストール方法の詳細について、[がありません](~/ios/deploy-test/provisioning/fastlane/index.md)ガイドです。

Visual Studio for Mac の Apple アカウント ダイアログでは、次の操作を行うことができます。

* **作成し、証明書の管理** 
* **作成し、プロビジョニング プロファイルの管理** 

これを行う方法の詳細については、このガイドで説明します。

次の iOS バンドル署名 * ツールを使用することもできます。

* **既存のプロファイルに新しい署名 id を追加します。** 
* **新しいデバイスをプロビジョニングします。** 

これらの機能を使用する方法についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。
️
## <a name="requirements"></a>必要条件

Apple アカウント管理を Visual Studio for mac の利用 Visual Studio for Windows では現在使用できません。

この機能を使用する Apple 開発者アカウントが必要です。 Apple 開発者アカウントの詳細についてで使用できる、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。

- インターネットに接続していることを確認します。 ありませんが、Apple 開発者ポータルに直接通信するためです。
- いることを確認[がありませんツールがインストールされて](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)です。
- 最新のありませんツールがあることを確認[https://download.fastlane.tools](https://download.fastlane.tools)です。
- 開始する前に確認で、ユーザーのライセンス契約を受け入れるように、[開発者ポータル](https://developer.apple.com/account/)です。

# <a name="adding-an-apple-developer-account"></a>Apple 開発者アカウントを追加します。

1. 移動するアカウントの管理 ダイアログを開くには**Visual Studio > 設定 > Apple の開発者アカウント**:

    ![Apple の開発者アカウント オプション](apple-account-management-images/image1.png)

2. キーを押して、  **+** 次のように、ダイアログ ボックスで、記号を表示するボタンをクリックします。 

    ![ダイアログがありません。](apple-account-management-images/image2.png)

4. Apple ID とパスワードを入力し、クリックして、**サイン イン**ボタンをクリックします。 このコンピューターで、資格情報をセキュリティで保護されたキーチェーンにこれは、保存されます。 [ありません](~/ios/deploy-test/provisioning/fastlane/index.md)資格情報を安全に処理し、Apple の開発者ポータルに渡すために使用します。
 
5. 選択**常に許可する**[Visual Studio での資格情報を使用するようにする警告] ダイアログ ボックス。

    ![](apple-account-management-images/image4.png)

6. 自分のアカウントが正常に追加したら、Apple ID が含まれるチームおよび Apple ID が表示されます。

    ![](apple-account-management-images/image5.png)

7. 任意のチームとキーを押して選択、**の詳細を表示しています.** ボタンをクリックします。 これにより、すべての署名 Id と、コンピューターにインストールされているプロビジョニング プロファイルの一覧が表示されます。

    ![](apple-account-management-images/image6.png)

<a name="managing">
    
## <a name="managing-signing-identities-and-provisioning-profiles"></a>署名 Id の管理とプロビジョニング プロファイル

チームの詳細 ダイアログには、型ごとに構成された署名 Id の一覧が表示されます。 **ステータス**列するように勧めるかどうか、証明書は。 

* **有効な**– 署名 id (証明書と秘密キーの両方) がコンピューターにインストールされているしが切れていません。

* **キーチェーン内にない**– Apple のサーバーに有効な署名 id があります。 これをコンピューターにインストールするに別のコンピューターからエクスポートする必要があります。 ように、秘密キーは含まれません、Apple 開発者ポータルから署名 id をダウンロードすることはできません。

* **秘密キーが不足している**– キーチェーンに秘密キーがないと、証明書がインストールされています。

* **有効期限が切れて**– 証明書の有効期限が切れています。 これは、キーチェーンから削除する必要があります。

  ![](apple-account-management-images/image7.png)

### <a name="create-a-signing-identities"></a>署名 Id を作成します。

新しい署名 id を作成するには、選択、**新しい証明書を作成する**ドロップダウン ボタンをクリックし、必要な型を選択します。 新しい署名適切なアクセス許可がある場合は、identity が数秒後に表示されます。

ドロップダウン リストのオプションが淡色表示にして、オフの場合、次のように場合、されませんが、正しい team アクセス許可をこの種類の証明書を作成することを意味します。

![](apple-account-management-images/image8.png)

### <a name="download-provisioning-profiles"></a>プロビジョニング プロファイルをダウンロードします。

チームの詳細 ダイアログには、開発者アカウントに接続されているすべてのプロビジョニング プロファイルの一覧も表示されます。 すべてのプロビジョニング プロファイルをローカル コンピューターにダウンロードするにはキーを押して、**すべてのプロファイルをダウンロード**ボタン

![](apple-account-management-images/image9.png)

## <a name="ios-bundle-signing"></a>iOS バンドル署名*

デバイスにアプリを配置する方法の詳細についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。


## <a name="troubleshooting"></a>トラブルシューティング

#### <a name="view-details-dialog-is-empty"></a>ビューの詳細 ダイアログ ボックスが空

これは、バグに関連する既知の問題では現在[#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)です。 Mac の最新の安定したバージョンの Visual Studio を使用していることを確認してください。

#### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>自分のアカウントでのログ記録の問題が発生する場合は、次の点をやり直してください。

* キーチェーン アプリケーションを開き、カテゴリ で *パスワード*です。 検索`deliver.`、およびすべてのエントリを削除します。

#### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"アカウントの追加のエラーです。 特定のアプリ パスワードでサインインしてください。"

これは、アカウントの 2 要素認証が有効になっているためです。 Mac の最新の安定したバージョンの Visual Studio を使用していることを確認してください。

#### <a name="failed-to-create-new-certificate"></a>新しい証明書を作成できませんでした。
「この種類の証明書の上限に達しました」

![](apple-account-management-images/image10.png)

許可される証明書の最大数が生成されました。 この問題を解決する」を参照、 [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution)運用証明書のいずれかを取り消すとします。

#### <a name="known-issues"></a>既知の問題:

* 場合がありますの詳細の表示 ダイアログには、膨大な署名 id とプロファイルをフェッチする時間がかかります。
* 多くの場合、フォーカスがある可能性がありますを返しません Visual Studio for Mac アカウントを追加できませんが、詳細情報を入力した後。 大文字と小文字の場合は、プロセスをもう一度やり直してください。
* Visual Studio for Mac で作成されたプロビジョニング プロファイルでは、プロジェクトで選択された権利 (Entitlements.plist) を考慮することはありません。 この機能は、今後の IDE のバージョンで追加されます。
* 既定では、プロビジョニング プロファイルの配布は、App Store を対象とします。 社内またはアドホックのプロファイルは、手動で作成する必要があります。