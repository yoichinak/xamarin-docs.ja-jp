---
title: Microsoft Azure Mobile Apps
description: "サンプルとコードは、Azure ポータルのドキュメントをダウンロードします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fb3c26b7d090ca42328c61192c794dec1544d1d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_サンプルとコードは、Azure ポータルのドキュメントをダウンロードします。_

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


これらのリンクは、Xamarin のドキュメントで使用できるは、 [Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) web サイトです。
Xamarin を Azure の機能を追加するアプリはダウンロードなどの単純な[モバイル クライアントから Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)です。

## <a name="working-with-the-xamarin-azure-component"></a>Xamarin の Azure コンポーネントの操作

一般的なドキュメント[Xamarin クライアント ライブラリ (コンポーネント) の操作](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/)Azure Mobile Apps でのさまざまなタスクを実行します。 このページには、多数詳細な説明と例を以下に示すチュートリアルのアーティクルの各なしのサンプル コード スニペットにはが含まれています。

## <a name="getting-started"></a>作業の開始

この記事では、Xamarin Azure アプリの最初に起動して実行手順を説明します。
ポータルで新しい Azure のモバイル アプリを作成してし、ダウンロードして構成済みのアプリの実行方法を説明します。

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## <a name="validate-modify-and-augment-data-in-scripts"></a>検証、修正、およびスクリプト内のデータの強化

サーバー側の検証およびその他の機能を実装するデータ テーブルを Azure のモバイル サービスにサーバー側のスクリプトを追加する方法を示します。

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## <a name="add-paging-to-data"></a>データへのページングを追加します。

大規模な Skip() および Take() を使用してデータ セットのページングが簡単な例です。

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## <a name="get-started-with-users"></a>ユーザーを概要します。

構成すると、Azure Mobile Services を使用してログイン画面のコーディングには、完全な手順を説明します。 サポートされている認証プロバイダーには、Microsoft、Google、Facebook、Twitter およびが含まれます。

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>スクリプト内のユーザーを承認します。

Javascript バックエンドのいくつかのサンプル コード

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>プッシュを概要します。

Apple および Google の web サイトにプッシュ通知を構成して、Azure モバイル サービスからデバイスにプッシュ通知を送信する手順を完了します。

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## <a name="get-started-with-notification-hubs"></a>Notification Hubs を概要します。

Apple および Google web サイトでプッシュ通知を構成、Azure 通知ハブの構成、デバイスにプッシュ通知を生成するための手順を完了します。

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## <a name="related-links"></a>関連リンク

- [GettingStarted (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure の PCL 例 (によって@paulbatum) (サンプル)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure のモバイル クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure のモバイル アプリのラーニング パス](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)
