---
title: "Web サービスへのアクセスの認証"
description: "このガイドでは、自分のデータにアクセスすることのみバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。 別のプロバイダーによって提供される組み込みの認証メカニズムを使用しておよび基本認証を使用して、OAuth id プロバイダーに対して認証する Xamarin.Auth コンポーネントを使用して、REST サービスにトピックが含まれます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 0139a7a921861b5d1c9a3639ee2c7e25ee6cf5fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-access-to-web-services"></a>Web サービスへのアクセスの認証

_このガイドでは、自分のデータにアクセスすることのみバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。別のプロバイダーによって提供される組み込みの認証メカニズムを使用しておよび基本認証を使用して、OAuth id プロバイダーに対して認証する Xamarin.Auth コンポーネントを使用して、REST サービスにトピックが含まれます。_

## <a name="authenticating-a-restful-web-servicerestmd"></a>[RESTful Web サービスを認証](rest.md)

HTTP では、リソースへのアクセスを制御するいくつかの認証メカニズムの使用をサポートします。 基本認証では、正しい資格情報を持つクライアントのみにリソースへのアクセスを提供します。 この記事では、基本認証を使用して、RESTful web サービスのリソースへのアクセスを保護する方法を示します。

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Id プロバイダーとユーザーの認証](oauth.md)

Xamarin.Auth では、クロス プラットフォーム SDK はユーザーを認証して、自分のアカウントを格納するためです。 これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するためのサポートを提供する OAuth 認証子が含まれます。 この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Azure のモバイル アプリを使用してユーザーの認証](azure.md)

Azure のモバイル アプリでは、認証とアプリケーションのユーザーの承認をサポートするために、さまざまな外部の id プロバイダーを使用します。 アクセス許可は、認証されたユーザーのみにアクセスを制限するテーブルで設定できます。 この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションでの認証プロセスを管理する方法について説明します。

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Azure Active Directory B2C のユーザーの認証](azure-ad-b2c.md)

Azure Active Directory B2C は、消費者向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Microsoft の認証ライブラリ (MSAL) と Azure Active Directory B2C を使用して、Xamarin.Forms アプリケーションにコンシューマーの id 管理を統合する方法を示します。

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Azure Active Directory B2C とAzure Mobile Apps との統合](azure-ad-b2c-mobile-app.md)

Azure Active Directory B2C は、Azure Mobile Apps の認証ワークフローの管理に使用できます。 この方法は、id 管理エクスペリエンスを完全にクラウドでは、定義し、モバイル アプリケーションのコードを変更することがなく変更できます。 この記事では、Xamarin.Forms を使用した Azure Mobile Apps インスタンスに対する認証および承認を提供する Azure Active Directory B2C を使用する方法を示します。

## <a name="authenticating-users-with-an-amazon-simpledb-serviceawsmd"></a>[Amazon SimpleDB サービスとユーザーの認証](aws.md)

Amazon SimpleDB では、独自のリソース ベースのアクセス許可システムを提供していません。 代わりに、ことを確認ユーザーのみが独自のデータへのアクセス、SimpleDB ドメイン内の id プロバイダーに対する認証を使用できます。 この記事では、独自の SimpleDB データへのユーザーのアクセスを制限する方法について説明します。


## <a name="related-links"></a>関連リンク

- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
- [非同期サポートの概要](~/cross-platform/platform/async.md)
