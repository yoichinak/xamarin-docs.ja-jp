---
title: 手順 1. アプリを登録して Azure Active Directory を使用する
description: このドキュメントでは、モバイルクライアントが安全にアクセスできるように Azure Active Directory に Azure アプリケーションを登録する方法について説明します。
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 6b10b2fd489881292d95bbf79375a6488aa88a17
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932552"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>手順 1. アプリを登録して Azure Active Directory を使用する

1. [Windowsazure.com](https://manage.windowsazure.com)に移動し、Azure Portal で Microsoft アカウントまたは組織アカウントでログインします。 Azure サブスクリプションをお持ちでない場合は、 [azure.com](https://www.azure.com)から試用版を入手できます。

2. サインインした後、[ **Active Directory** (1)] セクションに移動し、アプリケーションを登録するディレクトリ (2) を選択します。

   [![セクションで、アプリケーションを登録するディレクトリを選択します。](register-images/01.-active-directory-in-azure-portal-sml.jpg)](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. [**追加**] をクリックして新しいアプリケーションを作成し、[**組織で開発中のアプリケーションを追加**する] を選択します。

   [![組織で開発中のアプリケーションを追加](register-images/02.-add-new-application-sml.jpg)](register-images/02.-add-new-application.jpg#lightbox)

4. 次の画面で、アプリに名前を付けます (例として、 XAM-デモ)。
   アプリケーションの種類として [**ネイティブクライアントアプリケーション**] を選択していることを確認します。

   ![アプリケーションの種類として [ネイティブクライアントアプリケーション] を選択していることを確認します。](register-images/03.-app-name.jpg)

5. 最後の画面で、認証が完了したときにこの URI に返されるため、アプリケーションに固有の **リダイレクト URI*を指定します。

   ![最後の画面で、アプリケーションに固有のリダイレクト URI を指定します。これは、認証の完了時にこの URI に戻ります。](register-images/04.-app-redirect.jpg)

6. アプリが作成されたら、[**構成**] タブに移動します。後でアプリケーションで使用する**クライアント ID**を書き留めておきます。 また、この画面では、モバイルアプリケーションに Active Directory へのアクセス権を付与したり、Web API や Office365 などの別のアプリケーションを追加したりできます。認証が完了したら、モバイルアプリケーションで使用できます。

   ![また、この画面では、モバイルアプリケーションに Active Directory へのアクセス権を付与したり、Web API や Office365 などの別のアプリケーションを追加したりすることもできます。](register-images/05.-configure.jpg)

## <a name="related-links"></a>関連リンク

- [Microsoft NativeClient のサンプル](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
