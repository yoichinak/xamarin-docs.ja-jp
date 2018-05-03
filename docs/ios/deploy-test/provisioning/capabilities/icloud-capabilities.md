---
title: iCloud 機能
description: アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。 このガイドでは、iCloud 機能に必要なセットアップについて説明します。
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: e426423854e7c569576c374ea1284c4de099a2d1
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
---
# <a name="icloud-capabilities"></a>iCloud 機能

_アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。このガイドでは、iCloud 機能に必要なセットアップについて説明します。_

iCloud は、iOS ユーザーに自分のコンテンツを格納し、デバイス間で共有するための便利で簡単な方法を提供します。 開発者は iCloud を使用して、ユーザーに次の 4 つのストレージの手段を提供できます。Key-Value ストレージ、UIDocument ストレージ、CoreData、および CloudKit を直接使用して個別のファイルとディレクトリにストレージを提供できます。 これらの詳細については、[iCloud の概要](~/ios/data-cloud/introduction-to-icloud.md)に関するガイドを参照してください。

_コンテナー_が原因で、iCloud 機能をアプリケーションに追加するのは、他の App Services よりも少し難しくなっています。 コンテナーは、iCloud でアプリに関する情報を格納するために使用され、1 つの iCloud アカウントに含まれているすべての情報を、ユーザーの iOS デバイス上でサンド ボックスのように分離することができます。 コンテナーの詳細については、[CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)に関するガイドを参照してください。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

<a name="icloud-developer-center" />

## <a name="developer-center"></a>Developer Center

Developer Center から新しいアプリをプロビジョニングするときに、実行する必要がある 2 つの手順があります。

1.  コンテナーを作成します。
2.  iCloud 機能を使用して App ID を作成し、それにコンテナーを追加します。
3. この App ID が含まれるプロビジョニング プロファイルを作成します。

これらの手順を以下に説明します。

1.  [Apple Developer Center](https://developer.apple.com/account/) の [Certificate, Identifiers and Profiles]\(証明書、ID、およびプロファイル\) セクションを参照します。 
    
     ![Apple Developer Center のメイン ページ](icloud-capabilities-images/image22.png)

2.  **[Identifiers]\(ID\)** の下で **[iCloud Containers]\(iCloud コンテナー\)** を選択し、**+** を選択して新しいコンテナーを作成します。  
    
    ![iCloud コンテナー画面](icloud-capabilities-images/image23.png)

3.  iCloud コンテナーの**説明**と一意の**識別子**を入力します。 
    
    ![iCloud コンテナーの登録画面](icloud-capabilities-images/image24.png)

4.  **[Continue]\(続行\)** を押して情報が正しいことを確認し、**[Register]\(登録\)** を押して iCloud コンテナーを作成します。  
    
    ![iCloud コンテナーの登録画面](icloud-capabilities-images/image25.png)

新しい App ID を作成し、それにコンテナーを追加するには、次の操作を行います。

1.  [Developer Center](https://developer.apple.com/account/) で、**[Identifiers]\(ID\)** の下の **[App IDs]\(App ID\)** をクリックします。 
    
    ![Developer Center の識別子セクション](icloud-capabilities-images/image26.png)

2.  **+** ボタンを選択し、新しい App ID を追加します。 
    
    ![新しい App ID の追加ボタン](icloud-capabilities-images/image27.png)

3.  App ID の **[Name]\(名前\)** を入力し、**[Explicit App ID]\(明示的な App ID\)** を指定します。
    
    ![新しい App ID の詳細の入力](icloud-capabilities-images/image28.png)

4.  **[App Services]** の下で、**[iCloud]** を選択し、**[Include CloudKit support]\(CloudKit のサポートを含める\)** を選択します。
    
    ![iCloud App Services の選択](icloud-capabilities-images/image29.png)

5.  **[Continue]\(続行\)**、**[Register]\(登録\)** の順に選択します。 確認画面で、iCloud が黄色のシンボルで [Configurable]\(構成可能\) が選択されていることを示していることに注目してください。   
    
    ![確認画面](icloud-capabilities-images/image30.png)

6.  App ID のリストに戻り、先ほど作成したものを選択します。 
    
    ![App ID の選択画面](icloud-capabilities-images/image31.png)

7.  この拡張されたセクションの下部までスクロールし、**[Edit]\(編集\)** をクリックします。
    
    ![App ID の編集](icloud-capabilities-images/image32.png)

8.  iCloud までリストを下にスクロールして、**[Edit]\(編集\)** ボタンをクリックします。  
    
    ![iCloud App ID の編集](icloud-capabilities-images/image33.png)

9.  この App ID で使用するコンテナーを選択します。  
    
    ![コンテナーの選択画面](icloud-capabilities-images/image34.png)

10. コンテナーの割り当てを確認し、**[Assign]\(割り当て\)** を押します。
 
これでこのアプリ ID を使用して、新しいプロビジョニング プロファイルの生成または再生成ができます。手順については、[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)に関するガイドを参照してください。 

iCloud の使用に関する詳細は、次のガイドを参照してください。

*   [iCloud の概要](~/ios/data-cloud/introduction-to-icloud.md)
*   [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)
*   [Document Picker の概要](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>次の手順
 
以下のリストでは、実行する必要がある可能性のある追加の手順について説明します。

* アプリでフレームワークの名前空間を使用します。
* アプリに必要な権利を追加します。 必要な権利とその追加方法については、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)に関するガイドを参照してください。
* アプリの **[iOS バンドル署名]** で、**[カスタムの権利]** が **Entitlements.plist** に設定されていることを確認します。 これは、デバッグと iOS シミュレーターのビルドに対する既定の設定では_ありません_。

App Services で問題が発生した場合は、メイン ガイドの[トラブルシューティング](~/ios/deploy-test/provisioning/capabilities/index.md)のセクションを参照してください。