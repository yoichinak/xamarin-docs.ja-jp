---
title: Microsoft Azure と Xamarin
description: このドキュメントでは、Visual Studio for Mac、Azure Mobile Apps、Active Directory 認証、WebAPI の接続済みサービスに関するドキュメントへのリンクを示します。
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: conceptdev
ms.author: crdun
ms.date: 10/09/2017
ms.openlocfilehash: 0979a0b65cc3d5b4944dadaf67aaa14cf1b3cf73
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287599"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure と Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure アプリ Services の機能は、クラウドデータストレージやクロスプラットフォームのプッシュ通知など、Xamarin アプリに簡単に追加することができる")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Evolve 2016:Azure と Xamarin を使用した接続型アプリの開発](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Visual Studio for Mac の接続済みサービス

Visual Studio for Mac の新しい[接続済みサービス](connected-services.md)機能を使用すると、開発者は IDE 内からモバイルアプリケーションに Azure の機能をすばやく簡単に追加できます。 現在、アルファチャネルでのテストに使用できます。

## <a name="azure-app-services"></a>Azure アプリサービス

Azure[モバイルクライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)を実装するプロセスについて[説明する azure Mobile Apps ドキュメント](~/cross-platform/data-cloud/mobile-apps.md)のコレクションがあります。
Xamarin は、 [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/)および[Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/)用の Azure メッセージング NuGet パッケージも提供しており、プラットフォーム間でプッシュ通知を実装するのに役立ちます。

Mobile Apps、Web Api、ストレージなどにアクセスするように、 [Azure アプリ Services ポータル](https://portal.azure.com/)でアプリを構成します。 [App services の違い](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/)について説明し、 [Microsoft のビデオ](https://azure.microsoft.com/campaigns/azure-march-announcement/)をご覧ください。

## <a name="active-directory-authentication"></a>Active Directory 認証

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md)を使用すると、Xamarin [. Auth コンポーネント](https://www.nuget.org/packages/Xamarin.Auth/)を使用して xamarin アプリのユーザーをログインできます。
その後、アプリは Office 365 などの追加サービスにアクセスできます。

## <a name="webapi"></a>WebAPI

Microsoft の Web API は、Xamarin アプリケーションで簡単に使用できる REST のようなインターフェイスを公開しています。
[Azure の Web サイト](https://trywebsites.azurewebsites.net/)を簡単に作成し、WebAPI ベースのアプリを構築して Xamarin アプリに接続することができます。


### <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)

このチュートリアルでは、REST、WCF、SOAP web サービステクノロジを Xamarin モバイルアプリケーションに統合する方法について説明します。 さまざまなサービス実装を調べ、使用可能なツールとライブラリを評価して、サービスデータを使用するためのサンプルパターンを提供します。 最後に、Xamarin モバイルアプリケーションで使用するための RESTful web サービスを作成する基本的な概要を説明します。

## <a name="samples"></a>サンプル

[ドキュメントサンプル](https://github.com/xamarin/mobile-samples/tree/master/Azure)に加えて、次の完全なアプリケーションでは、Xamarin アプリに組み込まれているさまざまな Azure 機能について説明しています。

- [スポーツ](https://github.com/xamarin/Sport)–データストレージ & プッシュ通知を使用するフレンドリなスポーツリーグ追跡アプリです。
- [モーメント](https://github.com/pierceboggan/Moments)–画像に Azure Storage を使用するインスタント写真共有です。
- [XAMARIN CRM](https://github.com/xamarin/app-crm) –バックエンドに Web API を使用します。
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -Azure Mobile Apps。

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) : Ebooks の[アーキテクチャシリーズ](https://www.microsoft.com/net/learn/architecture)のサンプルです。
- ビルド2016の[Mydriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IoT サンプル。


## <a name="related-links"></a>関連リンク

- [Azure PCL の例 ( @paulbatumby) (サンプル)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portal](https://azure.microsoft.com/)
- [Xamarin 用モバイルクライアント (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
