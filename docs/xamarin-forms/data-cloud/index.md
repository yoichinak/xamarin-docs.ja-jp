---
title: データとクラウド サービス
description: Xamarin.Forms アプリケーションは、さまざまなテクノロジを使用して実装されている web サービスを利用できるし、このガイドはこれを行う方法を確認します。
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
---
# <a name="data--cloud-services"></a>データとクラウド サービス

_Xamarin.Forms アプリケーションは、さまざまなテクノロジを使用して実装されている web サービスを利用できるし、このガイドはこれを行う方法を確認します。_

Xamarin プラットフォームのクロスプラット フォームの web サービスの消費量の概要については、次を参照してください。[を Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)です。

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[サンプルについて理解する](~/xamarin-forms/data-cloud/walkthrough.md)

この記事では、別の web サービスと通信する方法については、Xamarin.Forms サンプル アプリケーションのチュートリアルを提供します。 トピックは、アプリケーションでは、ページ、データ モデルの構造は、web サービスの操作を呼び出すとします。

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Web サービスの使用](~/xamarin-forms/data-cloud/consuming/index.md)

このガイドを提供する別の web サービスと通信する方法を示しています作成、読み取り、更新、および削除 (CRUD) の機能を Xamarin.Forms アプリケーションです。 説明のトピックにはとの通信が含まれて[ASMX サービス](consuming/asmx.md)、 [WCF サービス](consuming/wcf.md)、 [REST サービス](consuming/rest.md)、および[Azure Mobile Apps](consuming/azure.md)です。

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Web サービスへのアクセスの認証](~/xamarin-forms/data-cloud/authentication/index.md)

このガイドでは、自分のデータにアクセスすることのみバックエンドを共有するユーザーを有効にする Xamarin.Forms アプリケーションに認証サービスを統合する方法について説明します。 トピックを取り上げています[REST サービスと基本認証を使用して](authentication/rest.md)、 [Xamarin.Auth コンポーネントを使用して OAuth id プロバイダーに対して認証する](authentication/oauth.md)、組み込みの認証を使用してによって提供されるメカニズム[Azure Mobile Apps](authentication/azure.md)です。

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Web サービスとデータを同期する](sync/index.md)

この記事では、オフラインの同期機能を Xamarin.Forms アプリケーションに追加する方法について説明します。 オフラインの同期により、ユーザーは、データの変更や、表示、追加すると、モバイル アプリケーションを操作したり、ネットワーク接続がない場合でもです。 変更は、ローカル データベースに格納し、デバイスがオンラインで、変更を同期 web サービスとします。

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[プッシュ通知の送信](push-notifications/index.md)

この記事では、Xamarin.Forms アプリケーションにプッシュ通知を追加する方法を示します。 Azure Notification Hubs は、モバイル プッシュ通知を送信する任意のバックエンドから任意のモバイル プラットフォームでは、異なるプラットフォーム通知システムと通信すること、バックエンドの複雑さを排除しながら、スケーラブルなプッシュ インフラストラクチャを提供します。

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[クラウドにファイルを格納する](storage/index.md)

この記事では、Xamarin.Forms を使用して、Azure ストレージ内のテキストおよびバイナリ データを格納する方法と、データにアクセスする方法を示します。 Azure ストレージは、非構造化、および構造化データの格納に使用できるスケーラブルなクラウド記憶域ソリューションです。

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[クラウドでデータを検索する](search/index.md)

この記事では、Microsoft Azure Search のライブラリを使用して、Azure Search を Xamarin.Forms アプリケーションに統合する方法を示します。 Azure Search は、インデックスの作成とクエリ機能によりアップロードされたデータを提供するクラウド サービスです。 これは、インフラストラクチャの要件と従来アプリケーションでの検索機能の実装に関連付けられている検索アルゴリズムの複雑さを削除します。

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[ドキュメント データベースにデータを格納する](cosmosdb/index.md)

このガイドでは、Azure Cosmos DB 標準 .NET クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法を示します。 Azure Cosmos DB ドキュメント データベースは、JSON ドキュメントでは、シームレスなスケールとグローバルのレプリケーションを必要とするアプリケーションのデータベースの高速、高可用性、スケーラブルなサービスを提供する低待機時間のアクセスを提供する NoSQL データベースです。

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Cognitive Services とインテリジェンスの追加](cognitive-services/index.md)

このガイドでは、Xamarin.Forms アプリケーションで Microsoft 認知サービス Api の一部を使用する方法について説明します。 認識サービスとは、一連の Api、Sdk、および開発者は、顔認識、音声認識、および言語理解などの機能を追加することで、アプリケーションをより高度な利用できるサービスです。
