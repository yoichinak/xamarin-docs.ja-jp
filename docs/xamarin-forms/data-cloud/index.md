---
title: データとクラウド サービス
description: Xamarin.Forms アプリケーションは、さまざまなテクノロジを使用して実装されている web サービスを利用でき、このガイドでは、これを行う方法を確認します。
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327507"
---
# <a name="data--cloud-services"></a>データとクラウド サービス

_Xamarin.Forms アプリケーションは、さまざまなテクノロジを使用して実装されている web サービスを利用でき、このガイドでは、これを行う方法を確認します。_

Xamarin プラットフォームでクロスプラット フォーム対応の web サービスの使用の概要については、次を参照してください。[を Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)します。

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[サンプルについて理解する](~/xamarin-forms/data-cloud/walkthrough.md)

この記事では、別の web サービスと通信する方法について説明する Xamarin.Forms のサンプル アプリケーションのチュートリアルを示します。 トピックを取り上げますが、アプリケーションでは、ページ、データ モデルの構造を含む web サービスの操作を呼び出すとします。

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Web サービスの使用](~/xamarin-forms/data-cloud/consuming/index.md)

このガイドが提供する別の web サービスと通信する方法を示します作成、読み取り、更新、および Xamarin.Forms アプリケーションに (CRUD) の機能を削除します。 通信について書かれています[ASMX サービス](consuming/asmx.md)、 [WCF services](consuming/wcf.md)、 [REST サービス](consuming/rest.md)、および[Azure Mobile Apps](consuming/azure.md)します。

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Web サービスへのアクセスの認証](~/xamarin-forms/data-cloud/authentication/index.md)

このガイドでは、独自のデータにアクセスすることのみをバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。 トピックを扱います[基本認証を使用して REST サービスと](authentication/rest.md)、 [、Xamarin.Auth コンポーネントを使用して OAuth id プロバイダーに対して認証する](authentication/oauth.md)、組み込みの認証を使用して、によって提供されるメカニズム[Azure Mobile Apps](authentication/azure.md)します。

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Web サービスとデータを同期する](sync/index.md)

この記事では、Xamarin.Forms アプリケーションにオフライン同期機能を追加する方法について説明します。 オフライン同期により、データの変更や、表示、追加、モバイル アプリケーションと対話するユーザーのネットワーク接続がないときでもです。 変更は、ローカルのデータベースに格納され、デバイスがオンラインになると変更は、web サービスと同期ことができます。

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[プッシュ通知の送信](push-notifications/index.md)

この記事では、Xamarin.Forms アプリケーションにプッシュ通知を追加する方法を示します。 Azure Notification Hubs は、バックエンドを異なるプラットフォーム通知システムと通信することの複雑さを排除しながら、任意のモバイル プラットフォームに任意のバックエンドからモバイル プッシュ通知を送信するため、スケーラブルなプッシュ インフラストラクチャを提供します。

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[クラウドにファイルを格納する](storage/index.md)

この記事では、Xamarin.Forms を使用して Azure Storage にテキストおよびバイナリ データを格納する方法と、データにアクセスする方法を示します。 Azure Storage とは、非構造化、および構造化データを格納するために使用できるスケーラブルなクラウド ストレージ ソリューションです。

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[クラウドでデータを検索する](search/index.md)

この記事では、Microsoft の Azure Search ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Search を統合する方法を示します。 Azure Search とは、インデックス作成とクエリのデータがアップロードするための機能を提供するクラウド サービスです。 これには、インフラストラクチャの要件と、アプリケーションに検索機能を実装するのに関連付けられた検索アルゴリズムの複雑さが削除されます。

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[ドキュメント データベースにデータを格納する](cosmosdb/index.md)

このガイドでは、Azure Cosmos DB .NET Standard クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法を示します。 Azure Cosmos DB ドキュメント データベースは、JSON ドキュメント、シームレスなスケーリングとグローバル レプリケーションを必要とするアプリケーションの高速、高可用性でスケーラブルなデータベース サービスを提供するための低待機時間のアクセスを提供する NoSQL データベースです。

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Cognitive Services とインテリジェンスの追加](cognitive-services/index.md)

このガイドでは、Xamarin.Forms アプリケーションで Microsoft Cognitive Services APIs の一部を使用する方法について説明します。 Cognitive Services では、Api、Sdk、および開発者は、顔認識、音声認識、および言語の理解などの機能を追加することで、アプリケーションをよりインテリジェントに利用可能なサービスのセットです。
