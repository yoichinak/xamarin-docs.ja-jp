---
title: Xamarin iOS アプリのデータと Cloud Services
description: このドキュメントでは、Xamarin iOS アプリでローカルデータ、iCloud、および CloudKit を操作する方法について説明しているガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: 4b3bf1fcc6be7713c87b16ef9ec083189793b324
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008053"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Xamarin iOS アプリのデータと Cloud Services

## <a name="data-accessiosdata-clouddataindexmd"></a>[データ アクセス](~/ios/data-cloud/data/index.md)

ほとんどのアプリケーションでは、デバイスにデータをローカルに保存する必要があります。 データ量が非常に小さい場合を除き、通常、データベースアクセスを管理するには、アプリケーションにデータベースとデータ層が必要です。 iOS には、Sqlite データベースエンジンが組み込まれています。また、データへのアクセスは、SQLite Data Provider に付属している Xamarin のプラットフォームによって簡素化されています。

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

IOS 5 の iCloud ストレージ API を使用すると、アプリケーションはユーザードキュメントとアプリケーション固有のデータを中央の場所に保存し、すべてのユーザーのデバイスからそれらの項目にアクセスできます。

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit フレームワークを使用すると、iCloud にアクセスするアプリケーションの開発が効率化されます。 これには、アプリケーションのデータと資産の権限の取得だけでなく、アプリケーションの情報を安全に格納できることも含まれます。 このキットは、個人情報を共有せずに iCloud Id を使用してアプリケーションにアクセスできるようにすることで、ユーザーに対して匿名のレイヤーを提供します。
