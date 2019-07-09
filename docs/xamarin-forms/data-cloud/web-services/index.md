---
title: Xamarin.Forms と Web サービス
description: このガイドを提供する別の web サービスと通信する方法を説明します。 作成、読み取り、更新、および削除 (CRUD) の機能、Xamarin.Forms アプリケーションにします。 ASMX サービス、WCF サービス、REST サービスとの通信について書かれています。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 1cf2714191528c5619b4f877bcb43e80464c44d1
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659009"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Forms と Web サービス

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

この記事では、別の web サービスと通信する方法について説明する Xamarin.Forms のサンプル アプリケーションのチュートリアルを示します。 トピックを取り上げますが、アプリケーションでは、ページ、データ モデルの構造を含む web サービスの操作を呼び出すとします。

## <a name="consume-an-aspnet-web-service-asmxxamarin-formsdata-cloudweb-servicesasmxmd"></a>[ASP.NET Web サービス (ASMX) の使用](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web サービス (ASMX) は、簡易オブジェクト アクセス プロトコル (SOAP) を使用して HTTP 経由でメッセージを送信する web サービスをビルドする機能を提供します。 SOAP は、構築、および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。 のみの SOAP メッセージを送受信する方法を理解する必要があります。 この記事では、Xamarin.Forms アプリケーションから ASMX web サービスを使用する方法を示します。

## <a name="consume-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudweb-serviceswcfmd"></a>[Windows Communication Foundation (WCF) Web サービスを使用します。](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統一されたフレームワークです。 セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。 ASP.NET Web サービス (ASMX) と、WCF との間の違いがありますが、WCF が ASMX が用意されているのと同じ機能をサポートしていることを理解することが重要、HTTP 経由の SOAP メッセージ。 この記事では、Xamarin.Forms アプリケーションから WCF SOAP サービスを使用する方法を示します。

## <a name="consume-a-restful-web-servicexamarin-formsdata-cloudweb-servicesrestmd"></a>[RESTful Web サービスを使用します。](~/xamarin-forms/data-cloud/web-services/rest.md)

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャ スタイルです。 REST 要求は、web ページを取得し、サーバーにデータを送信する web ブラウザーを使用して、同じ HTTP 動詞を使用して HTTP 経由で行われます。 この記事では、Xamarin.Forms アプリケーションから rest ベースの web サービスを使用する方法を示します。
