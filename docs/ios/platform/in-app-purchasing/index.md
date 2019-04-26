---
title: アプリで Xamarin.iOS での購入
description: このドキュメントでは、デジタル製品および storekit や Api を使用してサービスを販売する方法について説明します。 構成、消耗品、非コンシューマブル製品、トランザクション、サブスクリプション、および詳細を説明するガイドにリンクします。
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4b301c18ea0e69c818cf65b3b7df1cc8351350f5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61402456"
---
# <a name="in-app-purchasing-in-xamarinios"></a>アプリで Xamarin.iOS での購入

デジタル製品やサービス StoreKit – 自分の Apple id。 を使用するユーザーと財務の取り引きを Apple のサーバーと通信する iOS によって提供される Api のセットを使用して iOS アプリケーションを販売できます。 Storekit や Api は製品情報の取得、トランザクションを実行すると、主な検討事項-ユーザー インターフェイス コンポーネントではありません。 アプリ内購入を実装するアプリケーションは、独自のユーザー インターフェイスを構築し、ユーザーに必要な製品やサービスを提供するカスタム コードでの購入済みの項目を追跡する必要があります。

アプリ内購入機能を提供するには、いくつかの手順が必要です。

-  **アプリの構成**– アプリケーションのプロビジョニング プロファイルでは、セットアップを正しくする必要があります。
-  **製品を作成する**– 製品の説明と価格は、iTunes Connect ポータルで作成する必要があります。
-  **StoreKit を実装する**– 販売されている製品の種類に応じて storekit や API を実装する必要があります。
-  **ユーザー インターフェイスと、製品自体の構築**– メカニズムをそれぞれの購入を追跡し、バックアップ/復元に該当する場合など、製品を実装する必要があります。
-  **Sales を監視および資金を受け取る**– iTunes Connect によって提供される情報を使用して売上傾向を監視し、収益を追跡します。

このドキュメントでは、アプリ内購入の Xamarin.iOS を使用して提供するこれらすべての手順を完了する方法について説明します。

## <a name="requirements"></a>必要条件

アプリ内購入をサポートするために、Xcode 7 以降、Xamarin.iOS 5.0 以降を使用する必要があります。

## <a name="contents"></a>目次

 * [アプリ内購入の基本と構成](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [StoreKit の概要と製品情報の取得](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [コンシューマブル製品の購入](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [非コンシューマブル製品の購入](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [トランザクションと検証](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [サブスクリプションとレポート](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>まとめ

この記事がアプリ内購入の概念が導入されましたに活用するためにアプリケーションを構成する方法を説明および Xamarin.iOS を使用した例を表示します。 これがについて説明します。

-  **iOS プロビジョニング ポータル**– アプリで有効にするためのガイドラインは、機能を購入します。
-  **iTunes Connect** – アプリを販売する製品を構成します。
-  **ストア キット**– アプリ内購入機能を構築するために使用するクラスの説明。
-  **購入する際のアプリをコーディング**– Xamarin.iOS アプリにアプリ内購入を構築する方法の例を示します。
-  **Reporting** – iTunes Connect 経由で入手できる統計の概要について説明します。


## <a name="related-links"></a>関連リンク

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [アプリの購入のプログラミング ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect 開発者ガイド](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [ストア キットのフレームワーク参照](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Q & A のアプリ内購入商品識別子](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [アプリ内購入のテクニカル ノート](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [最初のアプリ ストアに提出](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [アプリ ストアのリソース センター](https://developer.apple.com/appstore/index.html)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [アプリの管理](https://developer.apple.com/appstore/resources/managing/index.html)
