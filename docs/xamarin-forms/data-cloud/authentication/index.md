---
title: Xamarin.フォーム Web サービス認証
description: このガイドでは、認証サービスを Xamarin.Forms アプリケーションに統合して、ユーザーが自分のデータにしかアクセスできないままバックエンドを共有できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: c3ad061953b08ef12bea7b6a63b7c39bbd89a5f6
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388578"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.フォーム Web サービス認証

## <a name="authenticate-a-restful-web-service"></a>[RESTful Web サービスの認証](rest.md)

HTTP では、リソースへのアクセスを制御するための認証メカニズムの使用がサポートされています。 基本認証では、正しい資格情報を持つクライアントにのみリソースへのアクセスが提供されます。 この記事では、基本認証を使用して RESTful Web サービス リソースへのアクセスを保護する方法について説明します。

## <a name="authenticate-users-with-azure-active-directory-b2c"></a>[Azure Active Directory B2C を使用してユーザーを認証する](azure-ad-b2c.md)

Azure Active Directory B2C は、コンシューマー向け Web アプリケーションおよびモバイル アプリケーション用のクラウド ID 管理ソリューションです。 この記事では、マイクロソフト認証ライブラリ (MSAL) と Azure Active Directory B2C を使用して、コンシューマー ID 管理を Xamarin.Forms アプリケーションに統合する方法について説明します。

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>[Azure Cosmos DB ドキュメント データベースと Xamarin.Forms を使用してユーザーを認証します。](azure-cosmosdb-auth.md)

Azure Cosmos DB ドキュメント データベースは、パーティション分割されたコレクションをサポートし、複数のサーバーとパーティションにまたがって、ストレージとスループットを無制限にサポートできます。 この資料では、アクセス制御をパーティション分割されたコレクションと組み合わせる方法について説明し、ユーザーが Xamarin.Forms アプリケーションで自分のドキュメントにのみアクセスできるようにします。
