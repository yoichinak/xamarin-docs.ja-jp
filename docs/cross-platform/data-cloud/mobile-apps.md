---
title: Microsoft Azure のモバイル アプリ
description: このドキュメントは、Azure に接続されている Xamarin アプリを構築する方法を説明するガイドにリンクしています。 これには、Xamarin の Azure コンポーネント、ユーザー、およびプッシュ通知の使用について説明します。
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: a1a0b078659441f0f45af66728a5f37d578d6274
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675086"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure のモバイル アプリ

_サンプルとコードは、Azure portal のドキュメントをダウンロードします。_

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
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


これらのリンクは、Xamarin のドキュメントで使用できるは、 [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) web サイト。
Xamarin アプリをダウンロードすることによって Azure の機能の追加、 [Azure Mobile クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)します。

## <a name="working-with-the-xamarin-azure-component"></a>Azure の Xamarin コンポーネントの操作

全般的なドキュメント[Xamarin クライアント ライブラリ (コンポーネント) の操作](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library)Azure Mobile Apps でのさまざまなタスクを実行します。 このページには、多数詳細な説明と例を以下のチュートリアル記事のそれぞれで使用できることがなく、サンプル コード スニペットにはが含まれています。

## <a name="getting-started"></a>作業の開始

この記事で稼働している最初の Azure の Xamarin アプリを取得する手順について説明します。
これには、ポータルで新しい Azure モバイル アプリを作成してし、ダウンロードして、事前構成済みのアプリを実行について説明します。

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>ユーザーを認証します。

構成すると、Azure Mobile Services を使用してログイン画面のコーディングの完全な手順を提供します。 サポートされている認証プロバイダーには、Microsoft、Google、Facebook、Twitter が含まれます。

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>スクリプト内のユーザーを承認します。

Javascript バックエンドのいくつかのサンプル コード

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>プッシュの使用

Apple および Google の web サイトでプッシュ通知を構成し、Azure Mobile Services からデバイスにプッシュ通知を送信する手順を完了します。

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Notification Hubs を概要します。

Apple と Google web サイトでプッシュ通知を構成、Azure 通知ハブを構成およびデバイスへのプッシュ通知を生成する手順を完了します。

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>関連リンク

- [GettingStarted (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure のモバイル クライアント](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps のラーニング パス](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
