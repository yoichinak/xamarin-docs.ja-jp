---
title: Microsoft Azure と Xamarin
description: このドキュメントは、Mac、Azure Mobile Apps、Active Directory 認証、および web Api 用の Visual studio 接続済みサービスに関するドキュメントをリンクしています。
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: dd211fecad0bff58cb9ff6c6a99ae6a15c60eb7b
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674982"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure と Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services の機能はクラウド データ ストレージおよびクロス プラットフォームのプッシュ通知を含む、Xamarin アプリに簡単に追加")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Evolve 2016:Azure と Xamarin を使用して接続されているアプリの開発](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Visual Studio for Mac の接続済みサービス

新しい[接続済みサービス](connected-services.md)Visual Studio for Mac の機能は、迅速かつ簡単には、Azure の機能を IDE 内からモバイル アプリケーションに追加する開発者を支援します。 アルファ チャネルでのテスト用に現在使用できます。

## <a name="azure-app-services"></a>Azure App Services

コレクションがある[Azure Mobile Apps ドキュメント](~/cross-platform/data-cloud/mobile-apps.md)を実装するプロセスをガイドする、 [Azure Mobile クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)します。
Xamarin 用の Azure メッセージングの NuGet パッケージにもは[iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/)と[Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/)プラットフォーム間でプッシュ通知を実装するのに役立つ。

アプリの構成、 [Azure App Service ポータル](https://portal.azure.com/)Mobile Apps、Web Api、ストレージ、およびその他にアクセスします。 について[アプリ サービスのさまざまな方法](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/)で見ると[Microsoft からこれらのビデオ](https://azure.microsoft.com/campaigns/azure-march-announcement/)します。

## <a name="active-directory-authentication"></a>Active Directory 認証

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md)経由での Xamarin アプリでユーザーをログインに使用できる、 [Xamarin.Auth コンポーネント](https://www.nuget.org/packages/Xamarin.Auth/)します。
アプリは、Office 365 などの他のサービスにアクセスできます。

## <a name="webapi"></a>WebAPI

Microsoft の Web API は、Xamarin アプリケーションで簡単に使用できる REST のようなインターフェイスを公開します。
簡単にスピン アップできる、 [Azure web サイト](https://trywebsites.azurewebsites.net/)Xamarin アプリに接続する web Api ベースのアプリケーションを作成します。


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)

このチュートリアルで、残りの部分を統合する方法は、WCF と SOAP web サービス テクノロジで Xamarin モバイル アプリケーション。 さまざまなサービスの実装を調べ、評価に使用できるツールと、それらを統合するライブラリおよびサービスのデータを使用するためのサンプルのパターンを提供します。 最後に、Xamarin モバイル アプリケーションで消費の RESTful web サービスを作成する基本的な概要を提供します。

## <a name="samples"></a>サンプル

加え、[ドキュメントのサンプル](https://github.com/xamarin/mobile-samples/tree/master/Azure)、次の完全なアプリケーションは、Xamarin アプリに組み込むさまざまな Azure の機能を示します。

- [スポーツ](https://github.com/xamarin/Sport)– データ ストレージとプッシュ通知を使用するフレンドリ スポーツ リーグ追跡アプリです。
- [モーメント](https://github.com/pierceboggan/Moments)– インスタントの写真を共有するイメージの Azure Storage を使用します。
- [Xamarin CRM](https://github.com/xamarin/app-crm) – バック エンドの Web API を使用します。
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -Azure Mobile Apps です。

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – サンプルを[Architecture series](https://www.microsoft.com/net/learn/architecture)の電子ブックです。
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Build 2016 から Azure と IoT をサンプリングします。


## <a name="related-links"></a>関連リンク

- [Azure の PCL の例 (によって@paulbatum) (サンプル)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portal](https://azure.microsoft.com/)
- [Xamarin (NuGet) のモバイル クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
