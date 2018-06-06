---
title: Apple アカウント管理
description: このドキュメントでは、Mac と Visual Studio 2017 の Visual Studio で、Apple のアカウントの管理機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: f77ab1c48e3200088d8c582634921df1ecf1001c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781508"
---
# <a name="apple-account-management"></a>Apple アカウント管理

Apple アカウント管理のインターフェイスは、Apple ID を持つ関連するすべての開発チームを表示する方法を提供します。 一覧を表示することによって各チームの詳細を表示することもできます_署名 Id_と_プロビジョニング プロファイル_コンピューターにインストールされています。

Apple ID の認証は使用してコマンドラインで実行[がありません](https://fastlane.tools/)です。 ありませんは、正常に認証するためのコンピューターにインストールする必要があります。 詳細についてがなく、そのインストール方法の詳細について、[がありません](~/ios/deploy-test/provisioning/fastlane/index.md)ガイドです。

Apple アカウント ダイアログでは、次の操作を行うことができます。

* **作成し、証明書の管理** 
* **作成し、プロビジョニング プロファイルの管理** 

これを行う方法の詳細については、このガイドで説明します。

また、自動的に作成し、署名 Id、アプリケーションの Id およびプロビジョニング プロファイルを管理する iOS のツールの自動プロビジョニングを使用することができます。

これらの機能を使用する方法についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。
️
## <a name="requirements"></a>必要条件

Apple アカウントの管理は Visual Studio for Mac と Visual Studio 2017 (15.7 以降のバージョン) で使用できます。

この機能を使用する Apple 開発者アカウントが必要です。 Apple 開発者アカウントの詳細についてで使用できる、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。

- インターネットに接続していることを確認します。 ありませんが、Apple 開発者ポータルに直接通信するためです。
- いることを確認[がありませんツールがインストールされて](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)です。
- 最新のありませんツールがあることを確認[ https://download.fastlane.tools](https://download.fastlane.tools)です。
- 開始する前に確認で、ユーザーのライセンス契約を受け入れるように、[開発者ポータル](https://developer.apple.com/account/)です。

## <a name="adding-an-apple-developer-account"></a>Apple 開発者アカウントを追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 移動するアカウントの管理 ダイアログを開くには**Visual Studio > 設定 > Apple の開発者アカウント**:

    ![Apple の開発者アカウント オプション](apple-account-management-images/image1.png)

2. キーを押して、 **+** 次のように、ダイアログ ボックスで、記号を表示するボタンをクリックします。 

    ![ダイアログがありません。](apple-account-management-images/image2.png)

4. Apple ID とパスワードを入力し、クリックして、**サイン イン**ボタンをクリックします。 このコンピューターで、資格情報をセキュリティで保護されたキーチェーンにこれは、保存されます。 [ありません](~/ios/deploy-test/provisioning/fastlane/index.md)資格情報を安全に処理し、Apple の開発者ポータルに渡すために使用します。
 
5. 選択**常に許可する**[Visual Studio での資格情報を使用するようにする警告] ダイアログ ボックス。

    ![常に警告ダイアログを許可します。](apple-account-management-images/image4.png)

6. 自分のアカウントが正常に追加したら、Apple ID が含まれるチームおよび Apple ID が表示されます。

    ![追加されたアカウントの Apple の開発者アカウント ダイアログ](apple-account-management-images/image5.png)

7. 任意のチームとキーを押して選択、**の詳細を表示しています.** を追加します。 これにより、すべての署名 Id と、コンピューターにインストールされているプロビジョニング プロファイルの一覧が表示されます。

    ![詳細画面を示すビュー署名 id と、コンピューター上のプロファイルのプロビジョニング](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 への Apple ID の追加を開始する前に必ず、開発環境が[Mac ビルド ホストにペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)です。

1. アカウントの管理 ウィンドウを開くには、するには**ツール > オプション > Xamarin > Apple アカウント**:

    ![アカウントの Apple のオプション 画面](apple-account-management-images/prov1.png)

1. 選択、**追加**ボタンをクリックし、Apple ID とパスワードを入力します。

    ![ユーザー名とパスワード ダイアログ ボックス](apple-account-management-images/prov1a.png)

1. 自分のアカウントが正常に追加したら、Apple ID が含まれるチームおよび Apple ID が表示されます。
 
1. 任意のチームとキーを押して選択、**の詳細を表示しています.** を追加します。 これにより、すべての署名 Id と、コンピューターにインストールされているプロビジョニング プロファイルの一覧が表示されます。

    ![ユーザー名とパスワード ダイアログ ボックス](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>署名 Id の管理とプロビジョニング プロファイル

チームの詳細 ダイアログには、型ごとに構成された署名 Id の一覧が表示されます。 **ステータス**列するように勧めるかどうか、証明書は。 

* **有効な**– 署名 id (証明書と秘密キーの両方) がコンピューターにインストールされているしが切れていません。

* **キーチェーン内にない**– Apple のサーバーに有効な署名 id があります。 これをコンピューターにインストールするに別のコンピューターからエクスポートする必要があります。 ように、秘密キーは含まれません、Apple 開発者ポータルから署名 id をダウンロードすることはできません。

* **秘密キーが不足している**– キーチェーンに秘密キーがないと、証明書がインストールされています。

* **有効期限が切れて**– 証明書の有効期限が切れています。 これは、キーチェーンから削除する必要があります。

  ![チームの詳細 ダイアログの情報](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>署名 Id を作成します。

新しい署名 id を作成するには、選択、**証明書の作成**ドロップダウン ボタンをクリックし、必要な型を選択します。 新しい署名適切なアクセス許可がある場合は、identity が数秒後に表示されます。

ドロップダウン リストのオプションがグレーで表示、選択されていない場合は、この種類の証明書を作成する適切なチームのアクセス許可があるいないことを意味します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![証明書のオプションを作成します。](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![証明書のオプションを作成します。](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>プロビジョニング プロファイルをダウンロードします。

チームの詳細 ダイアログには、開発者アカウントに接続されているすべてのプロビジョニング プロファイルの一覧も表示されます。 すべてのプロビジョニング プロファイルをローカル コンピューターにダウンロードするにはキーを押して、**すべてのプロファイルをダウンロード**ボタン

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![プロビジョニング プロファイル セクションをダウンロードします。](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![プロビジョニング プロファイル セクションをダウンロードします。](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS バンドル署名

デバイスにアプリを配置する方法の詳細についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="view-details-dialog-is-empty"></a>ビューの詳細 ダイアログ ボックスが空

これは、バグに関連する既知の問題では現在[#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)です。 Mac の最新の安定したバージョンの Visual Studio を使用していることを確認してください。

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>自分のアカウントでのログ記録の問題が発生する場合は、次の点をやり直してください。

* キーチェーン アプリケーションを開き、カテゴリ で *パスワード*です。 検索`deliver.`、およびすべてのエントリを削除します。

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"アカウントの追加のエラーです。 特定のアプリ パスワードでサインインしてください。"

これは、アカウントの 2 要素認証が有効になっているためです。 Mac の最新の安定したバージョンの Visual Studio を使用していることを確認してください。

### <a name="failed-to-create-new-certificate"></a>新しい証明書を作成できませんでした。
「この種類の証明書の上限に達しました」

![証明書の制限 ダイアログ](apple-account-management-images/image10.png)

許可される証明書の最大数が生成されました。 この問題を解決する」を参照、 [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution)運用証明書のいずれかを取り消すとします。

## <a name="known-issues"></a>既知の問題

* 既定では、プロビジョニング プロファイルの配布は、App Store を対象とします。 社内またはアドホックのプロファイルは、手動で作成する必要があります。
