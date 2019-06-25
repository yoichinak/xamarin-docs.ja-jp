---
title: Web サービスの使用
description: このガイドが提供する別の web サービスと通信する方法を示します作成、読み取り、更新、および Xamarin.Forms アプリケーションに (CRUD) の機能を削除します。 ASMX サービス、WCF サービス、REST サービス、および Azure Mobile Apps との通信について書かれています。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 163b82590e95a1587836502883cd5f9f2afbe19c
ms.sourcegitcommit: 6e04246207aa743820029e8c217a43cfdd24f991
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2019
ms.locfileid: "67352110"
---
# <a name="consuming-web-services"></a>Web サービスの使用

_このガイドが提供する別の web サービスと通信する方法を示します作成、読み取り、更新、および Xamarin.Forms アプリケーションに (CRUD) の機能を削除します。ASMX サービス、WCF サービス、REST サービス、および Azure Mobile Apps との通信について書かれています。_

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[ASP.NET Web サービス (ASMX) の使用](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET Web サービス (ASMX) は、簡易オブジェクト アクセス プロトコル (SOAP) を使用して HTTP 経由でメッセージを送信する web サービスをビルドする機能を提供します。 SOAP は、構築、および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。 のみの SOAP メッセージを送受信する方法を理解する必要があります。 この記事では、Xamarin.Forms アプリケーションから ASMX web サービスを使用する方法を示します。

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Windows Communication Foundation (WCF) Web サービスを使用](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統一されたフレームワークです。 セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。 ASP.NET Web サービス (ASMX) と、WCF との間の違いがありますが、WCF が ASMX が用意されているのと同じ機能をサポートしていることを理解することが重要、HTTP 経由の SOAP メッセージ。 この記事では、Xamarin.Forms アプリケーションから WCF SOAP サービスを使用する方法を示します。

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャ スタイルです。 REST 要求は、web ページを取得し、サーバーにデータを送信する web ブラウザーを使用して、同じ HTTP 動詞を使用して HTTP 経由で行われます。 この記事では、Xamarin.Forms アプリケーションから rest ベースの web サービスを使用する方法を示します。

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Azure Mobile Apps の使](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure のモバイル アプリでは、モバイルの認証、オフライン sync、およびプッシュ通知のサポートにより、Azure App Service でホストされているスケーラブルなバックエンドでアプリを開発できます。 これは Node.js バックエンドを使用する Azure のモバイル アプリに適用できるのみ、この記事では、クエリ、挿入、更新、および Azure Mobile Apps インスタンス内のテーブルに格納されたデータを削除する方法について説明します。

## <a name="related-links"></a>関連リンク

- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
- [非同期サポートの概要](~/cross-platform/platform/async.md)
