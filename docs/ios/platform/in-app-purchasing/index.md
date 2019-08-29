---
title: Xamarin. iOS でのアプリ内購入
description: このドキュメントでは、StoreKit Api を使用してデジタル製品とサービスを販売する方法について説明します。 構成、利用できる製品、非消費製品、トランザクション、サブスクリプションなどについて説明するガイドにリンクしています。
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 2e429002923d4bfdd2cf5ded4ef1508f8ebf20b8
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121510"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Xamarin. iOS でのアプリ内購入

iOS アプリケーションでは、StoreKit を使用してデジタル製品またはサービスを販売できます。 iOS が提供する一連の Api は、Apple のサーバーと通信して、ユーザーが Apple ID を介して財務トランザクションを実行します。 StoreKit Api は、主に製品情報の取得とトランザクションの実行に関係しています。ユーザーインターフェイスコンポーネントはありません。 アプリ内購入を実装するアプリケーションは、独自のユーザーインターフェイスを構築し、ユーザーに必要な製品またはサービスを提供するカスタムコードを使用して購入した項目を追跡する必要があります。

アプリ内購入機能を提供するには、次のいくつかの手順を実行する必要があります。

- **アプリを構成**する–アプリケーションのプロビジョニングプロファイルが正しくセットアップされている必要があります。
- **製品の作成**– iTunes Connect ポータルで製品の説明と価格を作成する必要があります。
- **StoreKit の実装**– STOREKIT API は、販売されている製品の種類に応じて実装する必要があります。
- **ユーザーインターフェイスと製品自体を構築**する–各購入を追跡し、必要に応じてバックアップ/復元するメカニズムを含む、製品を実装する必要があります。
- **売上および入荷資金の監視**– iTunes Connect によって提供される情報を使用して、売上傾向を監視し、収入を追跡します。

このドキュメントでは、これらのすべての手順を完了して、Xamarin. iOS を使用してアプリ内購入を提供する方法について説明します。

## <a name="requirements"></a>必要条件

アプリ内購入をサポートするには、Xcode 7 以降で Xamarin. iOS 5.0 以降を使用する必要があります。

## <a name="contents"></a>目次

- [アプリ内購入の基本と構成](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

- [StoreKit の概要と製品情報の取得](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

- [コンシューマブル製品の購入](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

- [非コンシューマブル製品の購入](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

- [トランザクションと検証](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

- [サブスクリプションとレポート](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Summary

この記事では、アプリ内購入の概念を紹介しました。この記事では、Xamarin を使用してアプリケーションを構成し、それを利用し、例を紹介する方法を説明しました。 ここで説明しました。

- **IOS プロビジョニングポータル**-アプリ内購入機能を有効にするためのガイドライン。
- **ITunes Connect** –アプリで販売する製品を構成します。
- **ストアキット**–アプリ内購入機能の構築に使用されるクラスについて説明します。
- **アプリを購入用にコーディング**する-アプリ内購入を Xamarin iOS アプリに組み込む方法の例。
- **レポート**– iTunes Connect で使用できる統計の概要。


## <a name="related-links"></a>関連リンク

- [InAppPurchaseSample](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit/)
- [アプリ購入のプログラミングガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect 開発者ガイド](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [ストアキットフレームワークリファレンス](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [アプリ内購入製品識別子 Q & A](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [アプリ内購入に関するテクニカルノート](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [最初のアプリストアの送信](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store リソースセンター](https://developer.apple.com/appstore/index.html)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [アプリの管理](https://developer.apple.com/appstore/resources/managing/index.html)
