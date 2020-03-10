---
title: Apple のアカウント管理
description: このドキュメントでは、Visual Studio for Mac と Visual Studio 2019 で Apple アカウント管理機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: davidortinau
ms.author: daortin
ms.date: 03/05/2020
ms.openlocfilehash: 17607e09a141fd29cd81cde93d812b20e62a9af8
ms.sourcegitcommit: 60d2243809d8e980fca90b9f771e72f8c0e64d71
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "78946251"
---
# <a name="apple-account-management"></a>Apple のアカウント管理

Visual Studio の Apple アカウント管理インターフェイスには、Apple ID に関連付けられている開発チームの情報を表示する方法が用意されています。 これにより、次の操作を行うことができます。

- **Apple 開発者アカウントを追加する**
- **署名証明書とプロビジョニングプロファイルの表示**
- **新しい署名証明書を作成する**
- **既存のプロビジョニングプロファイルをダウンロードする**

> [!IMPORTANT]
> Apple アカウント管理用の Xamarin のツールには、有料の Apple 開発者アカウントに関する情報のみが表示されます。 有料の Apple 開発者アカウントを使用せずに、デバイスでアプリをテストする方法については、「 [Xamarin の無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)」を参照してください。

## <a name="requirements"></a>必要条件

Apple アカウントの管理は、Visual Studio for Mac、Visual Studio 2019、Visual Studio 2017 (バージョン15.7 以降) で利用できます。 また、この機能を使用するには、Apple 開発者向けの有料アカウントが必要です。 Apple 開発者アカウントの詳細については、「[デバイスプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド」を参照してください。

> [!NOTE]
> 開始する前に、まず[Apple Developer portal](https://developer.apple.com/account/)のユーザーライセンス契約に同意してください。

## <a name="add-an-apple-developer-account"></a>Apple developer アカウントを追加する

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **Visual Studio > の 設定 > Apple Developer Account**  の順に選択し、 **+**  ボタンをクリックして サインイン ダイアログを開きます。

    ![Visual Studio for Mac の設定で、Apple 開発者アカウントページの AScreenshot。](apple-account-management-images/add-account-vsm.png)

2. Apple ID とパスワードを入力し、 **[サインイン]** をクリックします。 これにより、このコンピューターのセキュリティで保護されたキーチェーンに資格情報が保存されます。

3. アラートダイアログで **[常に許可]** を選択して、Visual Studio で資格情報を使用できるようにします。

    ![アラートダイアログを常に許可する](apple-account-management-images/image4.png)

4. アカウントが正常に追加されると、Apple ID と、Apple ID が含まれているすべてのチームが表示されます。

    ![アカウントが追加された Apple 開発者アカウントダイアログ](apple-account-management-images/image5.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Visual Studio 2017 または Visual Studio 2019 (バージョン16.4 以前) を使用している場合は、続行する前に、 [Mac ビルドホストにペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)する必要があります。

1. **Xamarin > Apple アカウント > [ツール] > [オプション**] にアクセスし、 **[追加]** をクリックします。

    ![Visual Studio の [オプション] の [Apple アカウント] ページのスクリーンショット。](apple-account-management-images/add-account-vsw.png)

2. Apple ID とパスワードを入力し、 **[ログイン]** をクリックします。

3. アカウントが正常に追加されると、Apple ID と、Apple ID が含まれているすべてのチームが表示されます。

    ![アカウントが追加された開発者アカウントページのスクリーンショット。](apple-account-management-images/accounts-vsw.png)

-----

## <a name="view-signing-certificates-and-provisioning-profiles"></a>署名証明書とプロビジョニングプロファイルの表示

チームを選択し、 **[詳細の表示...]** をクリックします。 コンピューターにインストールされている署名 id とプロビジョニングプロファイルの一覧を表示するダイアログを開くには

[チームの詳細] ダイアログには、署名 Id の一覧が種類別に表示されます。 証明書が次のような場合は、**状態**列に表示されます。 

- **有効**–署名 id (証明書と秘密キーの両方) がコンピューターにインストールされ、期限切れになっていません。

- **キーチェーン内にありません**-Apple のサーバーに有効な署名 id があります。 コンピューターにこれをインストールするには、別のコンピューターからエクスポートする必要があります。 秘密キーが含まれていないため、Apple Developer ポータルから署名 id をダウンロードすることはできません。

- **秘密キーがありませ**ん–キーチェーンに秘密キーのない証明書がインストールされています。

- **[期限切れ]** –証明書の有効期限が切れています。 これは、キーチェーンから削除する必要があります。

  ![チームの詳細のダイアログ情報](apple-account-management-images/image7.png)

## <a name="create-a-signing-certificate"></a>署名証明書を作成する

新しい署名 id を作成するには、 **[証明書の作成]** をクリックしてドロップダウンメニューを開き、作成する[証明書の種類](https://help.apple.com/xcode/mac/current/#/dev80c6204ec)を選択します。 正しいアクセス許可を持っている場合は、数秒後に新しい署名 id が表示されます。

ドロップダウンのオプションがグレー表示され、選択されていない場合は、この種類の証明書を作成するための適切なチームアクセス許可がないことを意味します。

## <a name="download-provisioning-profiles"></a>プロビジョニングプロファイルのダウンロード

[チームの詳細] ダイアログには、開発者アカウントに接続されているすべてのプロビジョニングプロファイルの一覧も表示されます。 すべてのプロビジョニングプロファイルをダウンロードするには、 **[すべてのプロファイルをダウンロード]** をクリックします。


## <a name="troubleshoot"></a>[トラブルシューティング]

- 新しい Apple developer アカウントが承認されるまでに数時間かかることがあります。 アカウントが承認されるまで、自動プロビジョニングを有効にすることはできません。

- Apple 開発者アカウントを追加すると、メッセージ `Authentication Error: Xcode 7.3 or later is required to continue developing with your Apple ID.`が表示されない場合は、使用している Apple ID に Apple Developer Program へのアクティブな有料メンバーシップがあることを確認してください。 有料の Apple 開発者アカウントを使用するには、「 [Xamarin の無料プロビジョニング](~/ios/get-started/installation/device-provisioning/free-provisioning.md)」ガイドを参照してください。

- 新しい署名証明書を作成しようとすると、エラー `You have reached the limit for certificates of this type`で失敗した場合は、許可されている証明書の最大数が生成されています。 この問題を解決するには、 [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution)にアクセスして、いずれかの運用証明書を失効させます。

- Visual Studio for Mac でアカウントのログ記録に問題が発生している場合は、キーチェーンアプリケーションを開き、 **[カテゴリ]** で **[パスワード]** を選択することができます。 `deliver.` を検索し、見つかったすべてのエントリを削除します。

## <a name="known-issues"></a>既知の問題

- 既定では、プロビジョニング プロファイルの配布は、App Store を対象とします。 社内またはアドホックのプロファイルは、手動で作成する必要があります。
