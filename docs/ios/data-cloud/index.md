---
title: データとクラウド サービス
description: 安定化と配置に関するガイド
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 81aebe5fd7431e578b75c5b61e1d2c92ce546909
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="data-and-cloud-services"></a>データとクラウド サービス


##  <a name="data-accessiosdata-clouddataindexmd"></a>[データ アクセス](~/ios/data-cloud/data/index.md)

ほとんどのアプリケーションでは、デバイスをローカルでのデータを保存するには、いくつか必要があります。 データの量が小さい普通でない限り通常が必要に、データベースとデータベースへのアクセスを管理するアプリケーションでデータ層です。 iOS が「組み込み」Sqlite データベース エンジンと、SQLite データ プロバイダーが付属する Xamarin のプラットフォームでデータへのアクセスが簡素化されます。

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

5、iOS で iCloud ストレージ API には、中央の場所にユーザーのドキュメントおよびアプリケーションに固有のデータを保存し、すべてのユーザーのデバイスからそれらの項目にアクセスするアプリケーションができます。

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit フレームワークは、そのアクセス iCloud アプリケーションの開発を簡略化します。 これには、アプリケーション データと資産の権限だけでなくアプリケーションの情報を安全に保管することの取得が含まれます。 このキットは、個人情報を共有せず、iCloud Id を持つアプリケーションへのアクセスを許可することで、匿名のレイヤーにユーザーを表示します。