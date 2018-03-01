---
title: "データとクラウド サービス"
description: "安定化と配置に関するガイド"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 5814936289164f14a07b33c219ad1fd01b00473b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="data-and-cloud-services"></a>データとクラウド サービス


##  <a name="data-accessiosdata-clouddataindexmd"></a>[データ アクセス](~/ios/data-cloud/data/index.md)

ほとんどのアプリケーションでは、デバイスをローカルでのデータを保存するには、いくつか必要があります。 データの量が小さい普通でない限り通常が必要に、データベースとデータベースへのアクセスを管理するアプリケーションでデータ層です。 iOS が「組み込み」Sqlite データベース エンジンと、SQLite データ プロバイダーが付属する Xamarin のプラットフォームでデータへのアクセスが簡素化されます。

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

5、iOS で iCloud ストレージ API には、中央の場所にユーザーのドキュメントおよびアプリケーションに固有のデータを保存し、すべてのユーザーのデバイスからそれらの項目にアクセスするアプリケーションができます。

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit フレームワークは、そのアクセス iCloud アプリケーションの開発を簡略化します。 これには、アプリケーション データと資産の権限だけでなくアプリケーションの情報を安全に保管することの取得が含まれます。 このキットは、個人情報を共有せず、iCloud Id を持つアプリケーションへのアクセスを許可することで、匿名のレイヤーにユーザーを表示します。