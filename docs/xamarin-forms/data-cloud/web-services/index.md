---
title: Xamarin.フォームと Web サービス
description: このガイドでは、さまざまな Web サービスと通信して、Xamarin.Forms アプリケーションに対して作成、読み取り、更新、および削除 (CRUD) 機能を提供する方法について説明します。 取り上げるトピックには、ASMX サービス、WCF サービス、REST サービスとの通信が含まれます。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 799a0a97f7c1a212b10dc62f8b3e7bd2cf2060c8
ms.sourcegitcommit: bf3b5925018e1bd6a00505e9f37500761b66809d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395457"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.フォームと Web サービス

## <a name="introduction"></a>[はじめに](introduction.md)

この記事では、さまざまな Web サービスと通信する方法を示す Xamarin.Forms サンプル アプリケーションのチュートリアルを提供します。 このトピックには、アプリケーションの解剖学、ページ、データ モデル、Web サービス操作の呼び出しなどがあります。

## <a name="consume-an-aspnet-web-service-asmx"></a>[ASP.NET Web サービス (ASMX) を使用する](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web サービス (ASMX) は、簡易オブジェクト アクセス プロトコル (SOAP) を使用して HTTP 経由でメッセージを送信する Web サービスを構築する機能を提供します。 SOAP は、Web サービスを構築およびアクセスするためのプラットフォームに依存しない言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、サービスの実装に使用されるプラットフォーム、オブジェクト モデル、またはプログラミング言語について何も知る必要はありません。 SOAP メッセージを送受信する方法を理解する必要があります。 この記事では、Xamarin.Forms アプリケーションから ASMX Web サービスを使用する方法を示します。

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[WCF (WCF) Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF は、サービス指向アプリケーションを構築するためのマイクロソフトの統一フレームワークです。 開発者は、セキュリティで保護された、信頼性の高い、トランザクション処理された、相互運用可能な分散アプリケーションを構築できます。 web サービス (ASMX) と WCF のASP.NET違いがありますが、WCF が ASMX が提供するのと同じ機能をサポートしていることを理解することが重要です: HTTP 経由の SOAP メッセージ。 この資料では、Xamarin.Forms アプリケーションから WCF SOAP サービスを使用する方法を示します。

## <a name="consume-a-restful-web-service"></a>[レストフル Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)

表現型状態転送 (REST) は、Web サービスを構築するためのアーキテクチャ スタイルです。 REST 要求は、Web ブラウザーが Web ページを取得したり、サーバーにデータを送信したりするために使用するのと同じ HTTP 動詞を使用して、HTTP を介して行われます。 この記事では、ResTful Web サービスを Xamarin.Forms アプリケーションから使用する方法を示します。
