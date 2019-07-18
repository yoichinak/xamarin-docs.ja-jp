---
title: Xamarin.iOS で非推奨の通知テクノロジ
description: このドキュメントでは、iOS 10 で導入された、ユーザー通知フレームワーク置き換わって iOS 通知のテクノロジについて説明します。
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/07/2016
ms.openlocfilehash: 63134298e437e7ac9b99ac4d716f6265752651c3
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865685"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Xamarin.iOS で非推奨の通知テクノロジ

このセクションでは、ローカルの実装し、Xamarin.iOS でのプッシュ通知する方法を示します。 IOS 通知のさまざまな UI 要素について説明し、API の説明を作成して、通知を表示するのに関係するのです。

> [!IMPORTANT]
> このセクションの情報は、iOS 9 に関連し、前に、そのままにしたここで以前の iOS バージョンをサポートするためにします。 IOS 10 以降を参照してください、[ユーザー通知フレームワーク ガイド](~/ios/platform/user-notifications/index.md)iOS デバイスでローカルとリモート通知の両方をサポートするためです。

## <a name="sections"></a>セクション

<a name="Local Notifications In iOS" />

## <a name="local-notifications-in-ioslocal-notifications-in-iosmd"></a>[IOS でのローカル通知](local-notifications-in-ios.md)

このセクションでは、Xamarin.iOS でローカル通知を実装する方法について説明します。 IOS 通知のさまざまな UI 要素について説明し、API の説明を作成して、通知を表示するのに関係するのです。

<a name="Local Notifications Walkthrough" />

## <a name="walkthrough---using-local-notifications-in-xamarinioslocal-notifications-in-ios-walkthroughmd"></a>[チュートリアル - Xamarin.iOS でのローカル通知の使用](local-notifications-in-ios-walkthrough.md)

このセクションでは、Xamarin.iOS アプリケーションでローカル通知を使用する方法について説明します。 これを作成して、アプリで受信された場合にアラートがポップアップ表示される通知の発行の基礎を紹介します。

<a name="Remote Notifications In iOS" />

## <a name="remote-notifications-in-iosremote-notifications-in-iosmd"></a>[IOS でのリモート通知](remote-notifications-in-ios.md)

このセクションでは、iOS でプッシュ通知を説明します。 これは、Apple プッシュ通知ゲートウェイ サービス (APNS)、iOS アプリケーションに公開通知が果たす役割について説明します。 プッシュ通知を有効にして、説明に必要なセキュリティ証明書を作成する方法を説明します。 最後にこのセクションでは、クライアント モバイル デバイスを追跡するアプリケーション サーバーを実行する必要がありますハウスキーピング タスクの一部についてはします。

## <a name="related-links"></a>関連リンク

- [通知 (サンプル)](https://developer.xamarin.com/samples/monotouch/Notifications/)
