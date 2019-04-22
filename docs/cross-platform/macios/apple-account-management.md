---
title: Apple のアカウント管理
description: このドキュメントでは、Visual studio for Mac と Visual Studio 2019 Apple アカウントの管理機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 8617d6e0c0930f581c45dbb461dfcb5d85a2becc
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855056"
---
# <a name="apple-account-management"></a>Apple のアカウント管理

Apple アカウントの管理インターフェイスは、Apple ID に関連付けられているすべての開発チームを表示する方法を提供します。 一覧が表示される各チームの詳細を表示することもできます_署名 Id_と_Provisioning Profiles_コンピューターにインストールされています。

Apple ID の認証は使用してコマンドラインで実行[fastlane](https://fastlane.tools/)します。 fastlane は、正常に認証するためのコンピューターにインストールする必要があります。 Fastlane とそのインストール方法の詳細についての詳細については、 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md)ガイド。

Apple アカウント ダイアログでは、次の操作を行うことができます。

* **作成し、証明書の管理**
* **作成し、プロビジョニング プロファイルの管理**

これを行う方法については、このガイドで説明します。

> [!NOTE]
> Apple アカウント管理用の Xamarin のツールでは、有料の Apple 開発者アカウントに関する情報のみ表示されます。 有料の Apple developer アカウントを使用せず、デバイス上のアプリをテストする方法については、次を参照してください、 [Xamarin.iOS アプリの無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)ガイド。

自動的に作成し、署名 Id、アプリの Id とプロビジョニング プロファイルを管理する iOS 自動プロビジョニング ツールを使用することもできます。 これらの機能の使用に関する詳細についてを参照してください、 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)ガイド。

## <a name="requirements"></a>必要条件

Apple アカウントの管理は、Visual Studio for Mac、Visual Studio 2019、および Visual Studio 2017 (バージョン 15.7 以降) でご確認いただけます。

この機能を使用する Apple Developer アカウントが必要です。 Apple 開発者アカウントの詳細についてで使用できる、 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)ガイド。

- インターネットに接続していることを確認します。 Fastlane が Apple Developer ポータルに直接通信するためです。
- 確保[fastlane ツールがインストールされている](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)します。
- 最新の fastlane ツールがあることを確認[ https://download.fastlane.tools](https://download.fastlane.tools)します。
- 開始する前にことで、ユーザーのライセンス契約に同意することを確認して、[開発者ポータル](https://developer.apple.com/account/)します。

## <a name="adding-an-apple-developer-account"></a>Apple 開発者アカウントを追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 移動するアカウント管理ダイアログ ボックスを開くには**Visual Studio > 設定 > Apple 開発者アカウント**:

    ![Apple 開発者アカウントのオプション](apple-account-management-images/image1.png)

2. キーを押して、 **+** 次のように、ダイアログ ボックスで、記号を表示するボタンをクリックします。 

    ![fastlane ダイアログ。](apple-account-management-images/image2.png)

4. Apple ID とパスワードを入力し、クリックして、**サインイン**ボタンをクリックします。 このコンピューターで、資格情報をセキュリティで保護されたキーチェーンにこれは、保存されます。 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md)資格情報を安全に処理して、Apple の開発者ポータルに渡したりするために使用します。
 
5. 選択**常に許可する**アラート ダイアログに Visual Studio、資格情報を使用することを許可します。

    ![アラート ダイアログを常に許可します。](apple-account-management-images/image4.png)

6. 自分のアカウントを正常に追加された後は、Apple ID と、Apple ID の一部である任意のチームを確認します。

    ![Apple 開発者アカウントのダイアログ ボックスがアカウントの追加](apple-account-management-images/image5.png)

7. チームおよびキーを押して選択、**の詳細を表示しています.** を追加します。 これにより、すべての署名 Id と、コンピューターにインストールされているプロビジョニング プロファイルの一覧が表示されます。

    ![署名 id とプロビジョニング プロファイルがコンピューターにビューの詳細画面が表示されました。](apple-account-management-images/image6.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2019 への Apple ID の追加を開始する前に、開発環境が確認[Mac ビルド ホストにペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)します。

1. アカウントの管理 ウィンドウを開くには、するには**ツール > オプション > Xamarin > Apple アカウント**:

    ![Apple アカウントのオプション 画面](apple-account-management-images/prov1.png)

1. 選択、**追加**ボタンをクリックし、Apple ID とパスワードを入力します。

    ![ユーザー名とパスワード ダイアログ ボックス](apple-account-management-images/prov1a.png)

1. 自分のアカウントを正常に追加された後は、Apple ID と、Apple ID の一部である任意のチームを確認します。
 
1. チームおよびキーを押して選択、**の詳細を表示しています.** を追加します。 これにより、すべての署名 Id と、コンピューターにインストールされているプロビジョニング プロファイルの一覧が表示されます。

    ![ユーザー名とパスワード ダイアログ ボックス](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>署名 Id の管理とプロビジョニング プロファイル

チームの詳細 ダイアログでは、種類別に整理、署名 Id の一覧が表示されます。 **状態**列ではお勧めするかどうか、証明書は。 

* **有効な**– 署名 id (証明書と秘密キーの両方) がコンピューターにインストールされているし、期限が切れていません。

* **キーチェーン内にありません**– Apple のサーバーで有効な署名 id があります。 これは、コンピューターにインストールするに別のコンピューターからエクスポートする必要があります。 秘密キーは含まれません、Apple Developer Portal から署名 id をダウンロードできません。

* **秘密キーが見つかりません**– 証明書と秘密キーがありませんが、キーチェーンにインストールされています。

* **有効期限が切れて**– 証明書の期限が切れています。 これは、キーチェーンから削除する必要があります。

  ![チームの詳細 ダイアログの情報](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>署名 Id を作成します。

新しい署名 id を作成するには、選択、**証明書の作成**ドロップダウン ボタンをクリックして、必要な種類を選びます。 新しい署名の正しいアクセス許可がある場合は、identity が数秒後に表示されます。

ドロップダウン リストのオプションがグレーで表示、選択解除されている場合は、この種類の証明書を作成する適切なチームのアクセス許可がないことを意味します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![証明書オプションを作成します。](apple-account-management-images/image8.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![証明書オプションを作成します。](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>プロビジョニング プロファイルをダウンロードします。

チームの詳細 ダイアログには、開発者アカウントに接続されているすべてのプロビジョニング プロファイルの一覧も表示されます。 すべてのプロビジョニング プロファイルをローカル コンピューターにダウンロードするにはキーを押して、**すべてのプロファイルをダウンロード**ボタン

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![プロビジョニング プロファイル セクションをダウンロードします。](apple-account-management-images/image9.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![プロビジョニング プロファイル セクションをダウンロードします。](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS バンドル署名

デバイスにアプリを展開する方法の詳細についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="view-details-dialog-is-empty"></a>ビューの詳細 ダイアログが空です。

これは現在、既知の問題に関連するバグ[#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)します。 Mac 用の Visual Studio の最新の安定バージョンを使用しているかどうかを確認します。

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>自分のアカウントでのログ記録の問題が発生する場合は、次をやり直してください。

* キーチェーンのアプリケーションを開き、カテゴリを選択します*パスワード*します。 検索`deliver.`、すべてのエントリを削除します。

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"エラー アカウントを追加します。 アプリ固有のパスワードでサインインしてください"

これは、アカウントで 2 要素認証が有効になっているためにです。 Mac 用の Visual Studio の最新の安定バージョンを使用しているかどうかを確認します。

### <a name="failed-to-create-new-certificate"></a>新しい証明書を作成できませんでした。
「この種類の証明書の上限に達しました」

![証明書の制限 ダイアログ](apple-account-management-images/image10.png)

許可されている証明書の最大数が生成されました。 この問題を解決する」を参照、 [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution)運用の証明書のいずれかを取り消すとします。

## <a name="known-issues"></a>既知の問題

* 既定では、プロビジョニング プロファイルの配布は、App Store を対象とします。 社内またはアドホックのプロファイルは、手動で作成する必要があります。
