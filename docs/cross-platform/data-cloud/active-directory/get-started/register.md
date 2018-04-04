---
title: 手順 1. Azure Active Directory を使用するアプリを登録します。
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c9a44a35e91e6368522f8632e185bd8a554d8593
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>手順 1. Azure Active Directory を使用するアプリを登録します。

1. 移動[windowsazure.com](https://manage.windowsazure.com)と Microsoft アカウントまたは Azure ポータルで、組織アカウントでログインします。 Azure サブスクリプションを持っていない場合は、評価版を取得できます[azure.com](http://www.azure.com)

2. にサインインした後に移動、 **Active Directory** (1) セクション (2) アプリケーションを登録するディレクトリを選択し、

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "セクションし、アプリケーションを登録するディレクトリを選択")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. をクリックして**追加**を新しいアプリケーションを作成するを選択し、**私の組織で開発中のアプリケーションを追加**

  [ ![](register-images/02.-add-new-application-sml.jpg "自分の所属組織で開発中のアプリケーションを追加します。")](register-images/02.-add-new-application.jpg#lightbox)

4. 次の画面で、アプリに名前を付けます。 XAM-DEMO)。
  選択するかどうかを確認**ネイティブ クライアント アプリケーション**アプリケーションの種類として。

  ![](register-images/03.-app-name.jpg "アプリケーションの種類としてネイティブ クライアント アプリケーションを選択するかどうかを確認します。")

5. 最終画面で、提供、**リダイレクト URI*はアプリケーションに固有の認証が完了したらこの URI に戻ります。

  ![](register-images/04.-app-redirect.jpg "最終画面で、認証が完了したらこの uri を返すので、アプリケーションに一意のリダイレクト URI を提供します")

6. 移動し、アプリを作成した後、**構成**タブです。書き留めて、**クライアント ID**後で、アプリケーションの際に使用します。 また、この画面で Active Directory にモバイル アプリケーションのアクセス権を付与したり、Web API など、office 365 の認証が完了したら、モバイル アプリケーションが使用できるように別のアプリケーションを追加できます。

    ![](register-images/05.-configure.jpg "また、この画面で Active Directory にモバイル アプリケーションのアクセス権を付与したり、Web API や office 365 などの別のアプリケーションの追加")



## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient サンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
