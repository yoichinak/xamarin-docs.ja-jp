---
title: Xamarin Android データアクセス
description: ほとんどのアプリケーションでは、デバイスにデータをローカルに保存する必要があります。 データ量が非常に小さい場合を除き、通常、データベースアクセスを管理するには、アプリケーションにデータベースとデータ層が必要です。  Android には、SQLite データベースエンジンが組み込まれています。また、データの格納と取得に対するアクセスは、Xamarin のプラットフォームによって簡略化されています。 このドキュメントでは、クロスプラットフォーム方式で SQLite データベースにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 9906e617b7072ebf7b1213a7278d117dc4f560ab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023856"
---
# <a name="xamarinandroid-data-access"></a>Xamarin Android データアクセス

_ほとんどのアプリケーションでは、デバイスにデータをローカルに保存する必要があります。データ量が非常に小さい場合を除き、通常、データベースアクセスを管理するには、アプリケーションにデータベースとデータ層が必要です。 Android には、SQLite データベースエンジンが組み込まれています。また、データの格納と取得に対するアクセスは、Xamarin のプラットフォームによって簡略化されています。このドキュメントでは、クロスプラットフォーム方式で SQLite データベースにアクセスする方法について説明します。_

## <a name="data-access-overview"></a>データアクセスの概要

ほとんどのアプリケーションでは、デバイスにデータをローカルに保存する必要があります。 データ量が非常に小さい場合を除き、通常、データベースアクセスを管理するには、アプリケーションにデータベースとデータ層が必要です。 Android には、SQLite データベースエンジンが組み込まれています。また、データへのアクセスは、SQLite Data Provider に付属している Xamarin のプラットフォームによって簡素化されています。

Xamarin Android は、次のようなデータベースアクセス Api をサポートします。

- ADO.NET フレームワーク。
- SQLite-NET サードパーティライブラリ。

このセクションのコードの大部分は完全にクロスプラットフォームなので、iOS または Android では変更せずに実行できます。 ここでは、次の2つのサンプルアプリについて説明します。

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; 単純なデータ操作では、結果がテキスト表示コントロールに書き込まれます。

- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; は、データ操作を、単純なデータ構造の一覧表示と編集を行う小さな動作アプリケーションに統合します。

どちらのサンプルソリューションにも、iOS と Android のサンプルアプリケーションプロジェクトが含まれています。

Xamarin のアプリケーションについては、「[データベースの操作](~/xamarin-forms/data-cloud/data/databases.md)」を参照してください。これは、xamarin を使用した PCL ライブラリで SQLite を操作する方法について説明しています。

このセクションのトピックでは、データベースエンジンとして SQLite を使用した Xamarin. Android のデータアクセスについて説明します。 ADO.NET 構文を使用してデータベースに直接アクセスできます。または、SQLite.NET ORM を含め、でC#データ操作を実行することもできます。

2つのサンプルが確認されます。1つは、テキストフィールドに出力する非常に単純なデータアクセスコードと、作成、読み取り、更新、削除の機能を含む単純なアプリケーションです。 スレッド処理と、事前に設定された SQLite データベースを使用してアプリケーションをシード処理する方法についても説明します。

クロスプラットフォームデータアクセスのその他の例については、 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)のケーススタディを参照してください。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
