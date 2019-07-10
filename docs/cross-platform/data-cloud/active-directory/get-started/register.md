---
title: 手順 1. Azure Active Directory を使用するアプリを登録します。
description: このドキュメントでは、モバイル クライアントによって安全にアクセスできるように、Azure Active Directory を Azure アプリケーションを登録する方法について説明します。
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fd3cb0e8201a6e84c726b211852013bd5f35fba1
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675118"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>手順 1. Azure Active Directory を使用するアプリを登録します。

1. 移動します[windowsazure.com](https://manage.windowsazure.com) Microsoft アカウントまたは Azure ポータルで組織アカウントでログインします。 Azure サブスクリプションを持っていない場合から試用版を入手できます[azure.com](http://www.azure.com)

2. サインインした後に移動、 **Active Directory** (1) セクション (2) アプリケーションを登録するディレクトリを選択

  [![](register-images/01.-active-directory-in-azure-portal-sml.jpg "セクションし、アプリケーションを登録するディレクトリの選択")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. クリックして**追加**新しいアプリケーションを作成するを選択し、**組織で開発中のアプリケーションを追加**

  [![](register-images/02.-add-new-application-sml.jpg "組織で開発中のアプリケーションを追加します。")](register-images/02.-add-new-application.jpg#lightbox)

4. 次の画面で、アプリに名前を付けます (例。 XAM-DEMO)。
  選択するかどうかを確認**ネイティブ クライアント アプリケーション**としてアプリケーションの種類。

  ![](register-images/03.-app-name.jpg "アプリケーションの種類としてネイティブ クライアント アプリケーションを選択するかどうかを確認します。")

5. 最後の画面では、提供、**リダイレクト URI*はアプリケーションに固有の認証が完了したらこの URI に戻ります。

  ![](register-images/04.-app-redirect.jpg "最後の画面での認証の完了時にこの URI を返すので、アプリケーションに一意のリダイレクト URI を指定します。")

6. アプリを作成したらに移動して、**構成**タブ。メモ、**クライアント ID**後で、アプリケーションで使用します。 また、この画面で Active Directory へのモバイル アプリケーション アクセス権を付与したり、Web API または office 365 は、認証が完了すると、モバイル アプリケーションで使用できるように別のアプリケーションを追加できます。

    ![](register-images/05.-configure.jpg "また、この画面で Active Directory へのモバイル アプリケーション アクセス権を付与したり、Web API や office 365 などの別のアプリケーションを追加")



## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient サンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
