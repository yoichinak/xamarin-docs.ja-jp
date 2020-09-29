---
title: Xamarin の非推奨の通知テクノロジ
description: このドキュメントでは、iOS 10 で導入されたユーザー通知フレームワークを優先するために非推奨とされた iOS 通知テクノロジについて説明します。
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2016
ms.openlocfilehash: 0822f630d74563f5e6e0dcefa456cf5f5c07a8f5
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436946"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Xamarin の非推奨の通知テクノロジ

このセクションでは、Xamarin. iOS でローカルおよびプッシュ通知を実装する方法について説明します。 ここでは、iOS の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。

> [!IMPORTANT]
> このセクションの情報は、iOS 9 とそれ以前のバージョンをサポートするために残されています。 IOS 10 以降については、iOS デバイスでのローカル通知とリモート通知の両方をサポートするための [ユーザー通知フレームワークガイド](~/ios/platform/user-notifications/index.md) を参照してください。

## <a name="sections"></a>セクション

<a name="Local Notifications In iOS"></a>

## <a name="local-notifications-in-ios"></a>[IOS でのローカル通知](local-notifications-in-ios.md)

このセクションでは、Xamarin. iOS でローカル通知を実装する方法について説明します。 ここでは、iOS の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。

<a name="Local Notifications Walkthrough"></a>

## <a name="walkthrough---using-local-notifications-in-xamarinios"></a>[チュートリアル-Xamarin でのローカル通知の使用](local-notifications-in-ios-walkthrough.md)

このセクションでは、Xamarin iOS アプリケーションでローカル通知を使用する方法について説明します。 ここでは、アプリで受信したときにアラートをポップアップ表示する通知を作成して公開する方法の基本について説明します。

<a name="Remote Notifications In iOS"></a>

## <a name="remote-notifications-in-ios"></a>[IOS でのリモート通知](remote-notifications-in-ios.md)

このセクションでは、iOS でのプッシュ通知について説明します。 IOS アプリケーションに通知を発行するときに、Apple Push notification Gateway サービス (APNS) とそれが果たす役割について説明します。 ここでは、プッシュ通知を有効にするために必要なセキュリティ証明書の作成方法と、について説明します。 最後に、このセクションでは、クライアントモバイルデバイスを追跡するためにアプリケーションサーバーで実行する必要があるいくつかのハウスキーピングタスクについて説明します。

## <a name="related-links"></a>関連リンク

- [通知 (サンプル)](/samples/xamarin/ios-samples/notifications)