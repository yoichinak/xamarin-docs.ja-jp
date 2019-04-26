---
title: Web サービスへのアクセスの認証
description: このガイドでは、独自のデータにアクセスすることのみをバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: d598a9b3de31ea6823530f911c3544bf3cebb37f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331183"
---
# <a name="authenticating-access-to-web-services"></a>Web サービスへのアクセスの認証

_このガイドでは、独自のデータにアクセスすることのみをバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。トピックを取り上げていますが、Xamarin.Auth コンポーネントを使用して、OAuth id プロバイダーに対する認証に REST サービスで基本認証を使用し、別のプロバイダーによって提供される組み込みの認証メカニズムを使用しています。_

## <a name="authenticating-a-restful-web-servicerestmd"></a>[RESTful Web サービスの認証](rest.md)

HTTP では、リソースへのアクセスを制御するいくつかの認証メカニズムの使用をサポートします。 基本認証では、適切な資格情報を持つクライアントのみにリソースへのアクセスを提供します。 この記事では、基本認証を使用して RESTful web サービスのリソースへのアクセスを保護する方法を示します。

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Id プロバイダーでユーザーを認証します。](oauth.md)

Xamarin.Auth は、ユーザーを認証し、自分のアカウントを格納するをクロス プラットフォーム SDK です。 これには、Google、Microsoft、Facebook、Twitter などの id プロバイダーを使用するためのサポートを提供する OAuth 認証子が含まれます。 この記事では、Xamarin.Auth を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Azure Mobile Apps を使ったユーザー認証](azure.md)

Azure Mobile Apps の認証とアプリケーションのユーザーの承認をサポートするためにさまざまな外部 id プロバイダーを使用します。 アクセス許可は、認証されたユーザーのみにアクセスを制限するテーブルで設定できます。 この記事では、Azure Mobile Apps を使用して、Xamarin.Forms アプリケーションの認証プロセスを管理する方法について説明します。

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Azure Active Directory B2C のユーザー認証](azure-ad-b2c.md)

Azure Active Directory B2C は、コンシューマー向けの web アプリケーションとモバイル アプリケーションのクラウド id 管理ソリューションです。 この記事では、Microsoft Authentication Library (MSAL) と Azure Active Directory B2C を使用して、Xamarin.Forms アプリケーションにコンシューマーの id 管理を統合する方法を示します。

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Azure Active Directory B2C とAzure Mobile Apps との統合](azure-ad-b2c-mobile-app.md)

Azure Mobile Apps の認証ワークフローを管理する azure Active Directory B2C を使用できます。 この方法を使えば、id 管理エクスペリエンスを完全にクラウドで定義でき、モバイル アプリケーションのコードを変更することなく修正できます。 この記事では、Azure Active Directory B2C を使用して、Xamarin.Forms の Azure Mobile Apps インスタンスに対する認証および承認を提供する方法を示します。

## <a name="related-links"></a>関連リンク

- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
- [非同期サポートの概要](~/cross-platform/platform/async.md)
