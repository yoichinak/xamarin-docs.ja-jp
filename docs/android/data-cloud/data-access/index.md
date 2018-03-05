---
title: Xamarin.Android Data Access
description: "ほとんどのアプリケーションでは、デバイスをローカルでのデータを保存するには、いくつか必要があります。 データの量が小さい普通でない限り通常が必要に、データベースとデータベースへのアクセスを管理するアプリケーションでデータ層です。  Android が 'ビルドされた' SQLite データベース エンジンと、Xamarin のプラットフォームでデータを格納および取得のアクセスが簡素化されます。 このドキュメントでは、クロスプラット フォームの方法で、SQLite データベースにアクセスする方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 301c4330afe6ff7808ca7b09f6cc5260517aae43
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_ほとんどのアプリケーションでは、デバイスをローカルでのデータを保存するには、いくつか必要があります。データの量が小さい普通でない限り通常が必要に、データベースとデータベースへのアクセスを管理するアプリケーションでデータ層です。Android が 'ビルドされた' SQLite データベース エンジンと、Xamarin のプラットフォームでデータを格納および取得のアクセスが簡素化されます。このドキュメントでは、クロスプラット フォームの方法で、SQLite データベースにアクセスする方法を示します。_

## <a name="data-access-overview"></a>データ アクセスの概要

ほとんどのアプリケーションでは、デバイスをローカルでのデータを保存するには、いくつか必要があります。 データの量が小さい普通でない限り通常が必要に、データベースとデータベースへのアクセスを管理するアプリケーションでデータ層です。 Android の両方が「組み込み」Sqlite データベース エンジンと、SQLite データ プロバイダーが付属する Xamarin のプラットフォームでデータへのアクセスが簡素化されます。

Xamarin.Android などデータベース アクセス Api をサポートします。

-  ADO.NET framework。
-  SQLite NET サードパーティのライブラリです。

大部分のこのセクションのコードは、完全にプラットフォーム間で変更せずに iOS または Android で実行されます。 説明した 2 つのサンプル アプリは。

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash;単純なデータ操作は、結果をテキスト コントロールの表示を書き込みます

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash;データ操作を一覧表示し、単純なデータ構造を編集する小規模な作業アプリケーションに統合します。

両方のサンプル ソリューションには、iOS と Android のサンプル アプリケーションのプロジェクトが含まれます。

Xamarin.Forms アプリケーション読み取り[データベースの操作](~/xamarin-forms/app-fundamentals/databases.md)xamarin.forms PCL ライブラリ SQLite を操作する方法について説明しています。

このセクションのトピックでは、SQLite を使用して、データベース エンジンと Xamarin.Android 内のデータ アクセスについて説明します。 ADO.NET の構文を使用して「直接」、データベースにアクセスできるまたは SQLite.NET ORM を含めるし、c# でのデータ操作を実行できます。

2 つのサンプルのレビューを行います: 更新し、削除機能をテキスト フィールドには、単純なアプリケーションを含む出力の作成、読み取り、非常に単純なデータ アクセス コードを含む 1 つです。 スレッド処理、および事前設定済み SQLite データベースを使用してアプリケーションをシードする方法についても説明します。

クロスプラット フォームのデータ アクセスの他の例を参照してください、 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケース スタディです。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データ レシピ](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
