---
title: Xamarin.Formsと Web サービス
description: このガイドでは、さまざまな web サービスと通信して、アプリケーションに対して作成、読み取り、更新、および削除 (CRUD) 機能を提供する方法について説明し Xamarin.Forms ます。 ここでは、ASMX サービス、WCF サービス、REST サービスとの通信について説明します。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5b2613b94d2c347d9bc6a94086f869b07ab8a55b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131886"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Formsと Web サービス

## <a name="introduction"></a>[はじめに](introduction.md)

この記事では、 Xamarin.Forms さまざまな web サービスと通信する方法を示すサンプルアプリケーションのチュートリアルを提供します。 ここでは、アプリケーションの構造、ページ、データモデル、および web サービス操作の呼び出しについて説明します。

## <a name="consume-an-aspnet-web-service-asmx"></a>[ASP.NET Web サービス (ASMX) を使用する](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET ウェブサービス (ASMX) は、Simple Object Access Protocol (SOAP) を使用して HTTP 経由でメッセージを送信する web サービスを構築する機能を提供します。 SOAP は、web サービスを構築してアクセスするための、プラットフォームに依存しない、言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、サービスの実装に使用されるプラットフォーム、オブジェクトモデル、プログラミング言語について何も知る必要はありません。 SOAP メッセージを送受信する方法を理解する必要があるだけです。 この記事では、アプリケーションから ASMX web サービスを使用する方法について説明し Xamarin.Forms ます。

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[Windows Communication Foundation (WCF) Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統合フレームワークです。 これにより、開発者は、セキュリティで保護された、信頼性の高い、トランザクションで相互運用可能な分散アプリケーションを構築できます。 ASP.NET ウェブサービス (ASMX) と WCF には違いがありますが、WCF は、ASMX が提供するのと同じ機能をサポートしています。これは、HTTP 経由の SOAP メッセージです。 この記事では、アプリケーションから WCF SOAP サービスを使用する方法について説明 Xamarin.Forms します。

## <a name="consume-a-restful-web-service"></a>[RESTful Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)

表現は、web サービスを構築するためのアーキテクチャスタイルです。 REST 要求は、web ブラウザーが web ページの取得やサーバーへのデータの送信に使用するのと同じ HTTP 動詞を使用して HTTP 経由で実行されます。 この記事では、アプリケーションから RESTful web サービスを使用する方法について説明 Xamarin.Forms します。
