---
title: データと Xamarin.iOS アプリにクラウド サービス
description: このドキュメントは、Xamarin.iOS アプリでローカル データ、iCloud、および CloudKit を使用する方法を説明するガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: 21a6c1c0c0ceb5596a056f0818dec39041808504
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117151"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>データと Xamarin.iOS アプリにクラウド サービス

##  <a name="data-accessiosdata-clouddataindexmd"></a>[データ アクセス](~/ios/data-cloud/data/index.md)

ほとんどのアプリケーションでは、デバイスにローカルにデータを保存するには、いくつか要件があります。 データの量が普通に小規模でない限り、通常が必要です、データベースとデータベースへのアクセスを管理するアプリケーションでのデータ層。 iOS は、Sqlite データベース エンジンの「組み込み」を備え、SQLite のデータ プロバイダーに付属する Xamarin のプラットフォームによって、データへのアクセスが簡略化します。

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

IOS 5 で iCloud ストレージ API には、中央の場所にユーザーのドキュメントおよびアプリケーションに固有のデータを保存し、すべてのユーザーのデバイスからそれらの項目にアクセスするアプリケーションができます。

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit フレームワークでは、アプリケーションの開発に、そのアクセス iCloud が効率化します。 これには、アプリケーション データと資産の権利とアプリケーションの情報を安全に保管することの取得が含まれます。 このキットでは、個人情報を共有することがなく、iCloud Id とアプリケーションへのアクセスを許可することでユーザーの匿名性レイヤーを提供します。