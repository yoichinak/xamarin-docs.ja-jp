---
title: Apple のアカウント管理
description: このドキュメントでは、Visual Studio for Mac と Visual Studio 2019 で Apple アカウント管理機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: conceptdev
ms.author: crdun
ms.date: 05/06/2018
ms.openlocfilehash: 9629d775b45951279178dffa3600e7cd5073dd38
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290631"
---
# <a name="apple-account-management"></a>Apple のアカウント管理

Apple アカウント管理インターフェイスは、Apple ID に関連付けられているすべての開発チームを表示する方法を提供します。 また、コンピューターにインストールされている_署名 id_と_プロビジョニングプロファイル_の一覧を表示することで、各チームの詳細を表示することもできます。

Apple ID の認証は、 [fastlane](https://fastlane.tools/)を使用してコマンドラインで実行されます。 正常に認証されるには、お使いのコンピューターに fastlane がインストールされている必要があります。 Fastlane とそのインストール方法の詳細については、 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md)ガイドを参照してください。

[Apple アカウント] ダイアログでは、次の操作を行うことができます。

- **証明書の作成と管理**
- **プロビジョニングプロファイルの作成と管理**

この方法については、このガイドで説明しています。

> [!NOTE]
> Apple アカウント管理用の Xamarin のツールには、有料の Apple 開発者アカウントに関する情報のみが表示されます。 有料の Apple 開発者アカウントを使用せずに、デバイスでアプリをテストする方法については、「 [Xamarin の無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)」を参照してください。

IOS 自動プロビジョニングツールを使用して、署名 Id、アプリ Id、プロビジョニングプロファイルを自動的に作成および管理することもできます。 これらの機能の使用方法の詳細については、「 [Device Provisioning guide (デバイスプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド)」を参照してください。

## <a name="requirements"></a>必要条件

Apple アカウントの管理は、Visual Studio for Mac、Visual Studio 2019、Visual Studio 2017 (バージョン15.7 以降) で利用できます。

この機能を使用するには、Apple 開発者アカウントが必要です。 Apple 開発者アカウントの詳細については、「[デバイスプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド」を参照してください。

- インターネットに接続されていることを確認します。 これは、fastlane が Apple Developer ポータルと直接通信するためです。
- [Fastlane ツールがインストールされ](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)ていることを確認します。
- の[https://download.fastlane.tools](https://download.fastlane.tools)最新の fastlane ツールがあることを確認します。
- 開始する前に、[開発者ポータル](https://developer.apple.com/account/)でユーザーライセンス契約に同意してください。

## <a name="adding-an-apple-developer-account"></a>Apple developer アカウントの追加

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. アカウント管理ダイアログを開くには、 **Visual Studio の [> の基本設定] > [Apple 開発者アカウント**] を選択します。

    ![Apple 開発者アカウントのオプション](apple-account-management-images/image1.png)

2. 次に示すように、**ボタンを押して[サインイン]ダイアログを表示します。+** 

    ![fastlane ダイアログ。](apple-account-management-images/image2.png)

3. Apple ID とパスワードを入力し、 **[サインイン]** ボタンをクリックします。 これにより、このコンピューターのセキュリティで保護されたキーチェーンに資格情報が保存されます。 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md)は、資格情報を安全に処理し、Apple の開発者ポータルに渡すために使用されます。
 
4. アラートダイアログで **[常に許可]** を選択して、Visual Studio で資格情報を使用できるようにします。

    ![アラートダイアログを常に許可する](apple-account-management-images/image4.png)

5. アカウントが正常に追加されると、Apple ID と、Apple ID が含まれているすべてのチームが表示されます。

    ![アカウントが追加された Apple 開発者アカウントダイアログ](apple-account-management-images/image5.png)

6. 任意のチームを選択し、 **[詳細の表示...]** をクリックします。 を追加します。 これにより、コンピューターにインストールされているすべての署名 Id とプロビジョニングプロファイルの一覧が表示されます。

    ![コンピューターの署名 id とプロビジョニングプロファイルを示す詳細画面を表示する](apple-account-management-images/image6.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Apple ID を Visual Studio 2019 に追加する前に、開発環境が[Mac ビルドホストとペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)されていることを確認してください。

1. アカウントの管理 ウィンドウを開くには、**ツール > オプション > Xamarin > Apple アカウント** にアクセスします。

    ![Apple アカウントのオプション画面](apple-account-management-images/prov1.png)

1. **[追加]** ボタンを選択し、Apple ID とパスワードを入力します。

    ![[ユーザー名とパスワード] ダイアログ](apple-account-management-images/prov1a.png)

1. アカウントが正常に追加されると、Apple ID と、Apple ID が含まれているすべてのチームが表示されます。
 
1. 任意のチームを選択し、 **[詳細の表示...]** をクリックします。 を追加します。 これにより、コンピューターにインストールされているすべての署名 Id とプロビジョニングプロファイルの一覧が表示されます。

    ![[ユーザー名とパスワード] ダイアログ](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>署名 Id とプロビジョニングプロファイルの管理

[チームの詳細] ダイアログには、署名 Id の一覧が種類別に表示されます。 証明書が次のような場合は、**状態**列に表示されます。 

- **有効**–署名 id (証明書と秘密キーの両方) がコンピューターにインストールされ、期限切れになっていません。

- **キーチェーン内にありません**-Apple のサーバーに有効な署名 id があります。 コンピューターにこれをインストールするには、別のコンピューターからエクスポートする必要があります。 秘密キーが含まれていないため、Apple Developer ポータルから署名 id をダウンロードすることはできません。

- **秘密キーがありませ**ん–キーチェーンに秘密キーのない証明書がインストールされています。

- **[期限切れ]** –証明書の有効期限が切れています。 これは、キーチェーンから削除する必要があります。

  ![チームの詳細のダイアログ情報](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>署名 Id を作成する

新しい署名 id を作成するには、 **[証明書の作成]** ドロップダウンボタンを選択し、必要な種類を選択します。 正しいアクセス許可を持っている場合は、数秒後に新しい署名 id が表示されます。

ドロップダウンのオプションがグレー表示され、選択されていない場合は、この種類の証明書を作成するための適切なチームアクセス許可がないことを意味します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![証明書の作成オプション](apple-account-management-images/image8.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![証明書の作成オプション](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>プロビジョニングプロファイルのダウンロード

[チームの詳細] ダイアログには、開発者アカウントに接続されているすべてのプロビジョニングプロファイルの一覧も表示されます。 すべてのプロビジョニングプロファイルをダウンロードするには、[すべての**プロファイルをダウンロード**] ボタンを押します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![プロビジョニングプロファイルセクションのダウンロード](apple-account-management-images/image9.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![プロビジョニングプロファイルセクションのダウンロード](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS バンドル署名

デバイスへのアプリの展開の詳細については、「[デバイスプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="view-details-dialog-is-empty"></a>詳細表示ダイアログが空です

これは現在、バグ[#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)に関連する既知の問題です。 Visual Studio for Mac の最新の安定バージョンを使用していることを確認します。

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>アカウントのログ記録に問題が発生している場合は、次の操作を行ってください。

- キーチェーンアプリケーションを開き、[カテゴリ] で [*パスワード*] を選択します。 すべての`deliver.`エントリを検索し、削除します。

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"アカウントを追加中にエラーが発生しています。 アプリ固有のパスワードを使用してサインインしてください "

これは、アカウントで2要素認証が有効になっているためです。 Visual Studio for Mac の最新の安定バージョンを使用していることを確認します。

### <a name="failed-to-create-new-certificate"></a>新しい証明書を作成できませんでした
"この種類の証明書の上限に達しました"

![証明書制限ダイアログ](apple-account-management-images/image10.png)

許可される証明書の最大数が生成されました。 この問題を解決するには、 [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution)にアクセスして、いずれかの運用証明書を失効させます。

## <a name="known-issues"></a>既知の問題

- 既定では、プロビジョニング プロファイルの配布は、App Store を対象とします。 社内またはアドホックのプロファイルは、手動で作成する必要があります。
