---
title: マイクロソフト・アズールとサマリン
description: このドキュメントは、接続されたサービスに関するドキュメントへのリンクを使用して、Visual Studio での接続済みサービス、Azure モバイル アプリ、アクティブ ディレクトリ認証、および WebAPI です。
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 2b6dfeb0de0fac59556280609dbf870c23a9298b
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388526"
---
# <a name="microsoft-azure-and-xamarin"></a>マイクロソフト・アズールとサマリン

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[進化する 2016: Azure と Xamarin を使用した接続アプリケーションの開発](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Visual Studio for Mac の接続済みサービス

Visual Studio for Mac の新しい[接続サービス](connected-services.md)機能を使用すると、開発者は IDE 内からモバイル アプリケーションに Azure の機能をすばやく簡単に追加できます。 アルファチャンネルでのテストに現在利用可能です。

## <a name="azure-app-services"></a>Azure App Service

[Azure モバイル アプリケーションのドキュメント](~/cross-platform/data-cloud/mobile-apps.md)のコレクションでは[、Azure モバイル クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)の実装プロセスについて説明します。
Xamarin は、プラットフォーム間でプッシュ通知を実装するために[、iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/)と[Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/)用の Azure メッセージング NuGet パッケージも提供しています。

モバイル アプリ、Web API、ストレージなどの機能にアクセスするように[、Azure アプリ サービス ポータル](https://portal.azure.com/)でアプリを構成します。 アプリ[サービスの違い](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/)について説明し[、Microsoft のビデオ](https://azure.microsoft.com/campaigns/azure-march-announcement/)で視聴する方法を説明します。

## <a name="active-directory-authentication"></a>Active Directory 認証

[Azure のアクティブ ディレクトリ](~/cross-platform/data-cloud/active-directory/index.md)を使用して、ユーザーを Xamarin アプリにログインできます。 アプリは、Office 365 などの追加のサービスにアクセスできます。

## <a name="webapi"></a>ウェブAPI

マイクロソフトの Web API は、Xamarin アプリケーションで簡単に使用できる REST のようなインターフェイスを公開しています。
[Azure Web サイト](https://trywebsites.azurewebsites.net/)を簡単にスピンアップし、Xamarin アプリに接続するための WebAPI ベースのアプリを構築できます。

### <a name="introduction-to-web-services"></a>[Web サービスの概要](~/cross-platform/data-cloud/web-services/index.md)

このチュートリアルでは、REST、WCF、および SOAP Web サービス テクノロジを Xamarin モバイル アプリケーションと統合する方法について説明します。 さまざまなサービス実装を調査し、それらを統合するために利用可能なツールとライブラリを評価し、サービス データを使用するためのサンプル パターンを提供します。 最後に、Xamarin モバイル アプリケーションで使用する RESTful Web サービスの作成の基本的な概要を説明します。

## <a name="samples"></a>サンプル

[ドキュメント サンプル](https://github.com/xamarin/mobile-samples/tree/master/Azure)に加えて、次の完全なアプリケーションでは、Xamarin アプリに組み込まれたさまざまな Azure 機能を示します。

- [スポーツ](https://github.com/xamarin/Sport)– プッシュ通知のデータストレージ&を使用するフレンドリーなスポーツリーグ追跡アプリ。
- [モーメント](https://github.com/pierceboggan/Moments)– イメージに Azure ストレージを使用するインスタント写真共有。
- [Xamarin CRM](https://github.com/xamarin/app-crm) – バックエンドに Web API を使用します。
- [マイショップ -](https://github.com/jamesmontemagno/MyShoppe) Azure モバイル アプリ。

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – 電子ブックの[アーキテクチャシリーズ](https://www.microsoft.com/net/learn/architecture)のサンプル.
- [マイドライビング](https://azure.microsoft.com/campaigns/mydriving/)– ビルド 2016 の Azure + IoT サンプル。

## <a name="related-links"></a>関連リンク

- [Azure PCL の@paulbatum例 (、) (サンプル)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure Portal](https://azure.microsoft.com/)
- [Xamarin (NuGet) 用のモバイル クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
