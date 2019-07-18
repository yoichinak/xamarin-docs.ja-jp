---
title: Xamarin.Forms の Web サービス認証
description: このガイドでは、独自のデータにアクセスすることのみをバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: a36dfba7aa07de1633ca9620674ddb4887902ba9
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650422"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.Forms の Web サービス認証

## <a name="authenticate-a-restful-web-servicerestmd"></a>[RESTful Web サービスを認証します。](rest.md)

HTTP では、リソースへのアクセスを制御するいくつかの認証メカニズムの使用をサポートします。 基本認証では、適切な資格情報を持つクライアントのみにリソースへのアクセスを提供します。 この記事では、基本認証を使用して RESTful web サービスのリソースへのアクセスを保護する方法について説明します。

## <a name="authenticate-users-with-an-identity-provideroauthmd"></a>[Id プロバイダーでユーザーを認証します。](oauth.md)

Xamarin.Auth は、ユーザーを認証し、自分のアカウントを格納するをクロス プラットフォーム SDK です。 これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するためのサポートを提供する OAuth 認証子が含まれます。 この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。

## <a name="authenticate-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Azure Active Directory B2C でユーザーを認証します。](azure-ad-b2c.md)

Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Microsoft Authentication Library (MSAL) と Azure Active Directory B2C を使用して、Xamarin.Forms アプリケーションにコンシューマーの id 管理を統合する方法について説明します。

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[Azure Cosmos DB ドキュメント データベースと Xamarin.Forms でユーザーを認証します。](azure-cosmosdb-auth.md)

Azure Cosmos DB ドキュメント データベースでは、パーティション分割されたコレクションは、無制限のストレージとスループットをサポートしながら複数のサーバーと、パーティションにまたがることができますをサポートします。 この記事では、各自のドキュメントで、Xamarin.Forms アプリケーションでのみアクセスできるように、パーティションのコレクションとアクセス制御を結合する方法について説明します。
