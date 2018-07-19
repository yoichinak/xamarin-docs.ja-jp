---
title: Web サービスの使用
description: このガイドを提供する別の web サービスと通信する方法を示しています作成、読み取り、更新、および削除 (CRUD) の機能を Xamarin.Forms アプリケーションです。 説明のトピックには、ASMX サービス、WCF services、REST サービスと Azure Mobile Apps との通信が含まれます。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a4c842ea7fd37ade9be0a9cb3e3ff7e50a6d1491
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044575"
---
# <a name="consuming-web-services"></a>Web サービスの使用

(_T) のガイドが提供する別の web サービスと通信する方法を示します作成、読み取り、更新、および削除 (CRUD) の機能を Xamarin.Forms アプリケーションです。 説明のトピックには、ASMX サービス、WCF services、REST サービスと Azure Mobile Apps との通信が含まれます。

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[ASP.NET Web サービス (ASMX) の使用](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET Web サービス (ASMX) は、簡易オブジェクト アクセス プロトコル (SOAP) を使用して HTTP 経由でメッセージを送信する web サービスをビルドする機能を提供します。 SOAP は、構築および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスの実装に使用するプログラミング言語に関する知識は必要はありません。 のみ、SOAP メッセージを送受信する方法を理解する必要があります。 この記事では、Xamarin.Forms アプリケーションからの ASMX web サービスを利用する方法を示します。

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Windows Communication Foundation (WCF) Web サービスを使用](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統一フレームワークです。 セキュリティで保護された、信頼性、トランザクション、および相互運用可能な分散アプリケーションを構築する開発者ができるようにします。 ASP.NET Web サービス (ASMX) と、WCF の違いがあるが、WCF が asmx サービスを提供するのと同じ機能をサポートしていることを理解することが重要、HTTP 経由の SOAP メッセージ。 この記事では、Xamarin.Forms アプリケーションから WCF SOAP サービスを使用する方法を示します。

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャのスタイルです。 REST 要求は web ページを取得して、サーバーにデータを送信する web ブラウザーを使用して同じ HTTP 動詞を使用して HTTP 経由で行われます。 この記事では、Xamarin.Forms アプリケーションから RESTful web サービスを使用する方法を示します。

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Azure のモバイル アプリの使用](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure のモバイル アプリでは、モバイルの認証、オフライン sync、およびプッシュ通知のサポートにより、Azure App Service でホストされているスケーラブルなバックエンドでアプリを開発できます。 これは Node.js バックエンドを使用する Azure のモバイル アプリに適用できるのみ、この記事では、クエリ、挿入、更新、および Azure Mobile Apps インスタンス内のテーブルに格納されたデータを削除する方法について説明します。

## <a name="related-links"></a>関連リンク

- [Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)
- [非同期サポートの概要](~/cross-platform/platform/async.md)
