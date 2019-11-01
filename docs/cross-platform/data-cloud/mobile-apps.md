---
title: Microsoft Azure Mobile Apps
description: このドキュメントでは、Azure に接続されている Xamarin アプリを構築する方法について説明しているガイドへのリンクを示します。 このトピックでは、Xamarin Azure コンポーネント、ユーザー、およびプッシュ通知の使用方法について説明します。
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: davidortinau
ms.author: daortin
ms.date: 04/02/2017
ms.openlocfilehash: 84517e4961dc3ad728b6cc352e9fb992d9e8b5bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016592"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

> [!NOTE]
> Visual Studio App Center は、エンドツーエンドの統合サービスをモバイルアプリ開発の中心としてサポートします。 開発者は、**ビルド**、**テスト**、および**配布**のサービスを使用して、継続的インテグレーションと配信パイプラインを設定できます。 アプリがデプロイされると、開発者は、**分析**と**診断**サービスを使用してアプリの状態と使用状況を監視し、**プッシュ**サービスを使用しているユーザーと連携できます。 開発者は、**認証**を利用してユーザーを認証し、**データ**サービスを使用してクラウド内のアプリデータを永続化して同期することもできます。
> モバイルアプリケーションにクラウドサービスを統合する場合は、今すぐ[App Center](https://appcenter.ms/signup?utm_source=XamarinDocs&utm_medium=Azure&utm_campaign=docs)でサインアップしてください。

_Azure portal のドキュメントについては、サンプルとコードをダウンロードしてください。_

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
as https://developer xamarin com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started https://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data https://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push https://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication https://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs https://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data  https://go.microsoft.com/fwlink/p/?LinkId=331330
-->

これらのリンクは、 [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) web サイトで入手できる Xamarin ドキュメント用です。
[Azure モバイルクライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)をダウンロードして Xamarin アプリに azure の機能を追加する。

## <a name="working-with-the-xamarin-azure-component"></a>Xamarin Azure コンポーネントの操作

一般的なドキュメント[Xamarin クライアントライブラリ (コンポーネント) を操作](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library)して、Azure Mobile Apps でさまざまなタスクを実行します。 このページには多数のサンプルコードスニペットが含まれていますが、以下に示す各チュートリアルの記事では、詳細な説明と例を紹介していません。

## <a name="getting-started"></a>作業の開始

この記事では、初めての Xamarin Azure アプリを起動して実行する手順について説明します。
ポータルでの新しい Azure モバイルアプリの作成と、事前に構成されたアプリのダウンロードと実行について説明します。

- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
- [Outlook Web Access (OWA)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
- [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>ユーザーを使ってみる

Azure Mobile Services を使用してログイン画面を構成およびコーディングするための完全な手順について説明します。 サポートされている認証プロバイダーには、Microsoft、Google、Facebook、Twitter があります。

- [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
- [Outlook Web Access (OWA)](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)

## <a name="authorize-users-in-scripts"></a>スクリプトでユーザーを承認する

Javascript バックエンドのサンプルコード

- [Todo](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)

## <a name="get-started-with-push"></a>プッシュを使ってみる

Apple および Google websites でプッシュ通知を構成する手順を完了し、Azure Mobile Services からデバイスにプッシュ通知を送信します。

- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
- [Outlook Web Access (OWA)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)

## <a name="get-started-with-notification-hubs"></a>Notification Hubs を使ってみる

Apple および Google websites でプッシュ通知を構成する手順を完了し、Azure Notification Hub を構成して、デバイスへのプッシュ通知を生成します。

- [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
- [Outlook Web Access (OWA)](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)

## <a name="related-links"></a>関連リンク

- [GettingStarted た (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [Get/Withusers (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [Get押し出し Withpush (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure モバイルクライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps ラーニングパス](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
