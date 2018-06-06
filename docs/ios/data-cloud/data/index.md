---
title: Xamarin.iOS データ アクセス
description: このドキュメントには、Xamarin.iOS アプリケーションでローカルのデータベースを操作する方法を説明するガイドへのリンクがします。 リンクされたコンテンツは、SQLite.NET、ADO.NET、およびその他について説明します。
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: a986ea9931f62497e5a6863c84bd4041983d66d9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784577"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS データ アクセス

Xamarin.iOS は、次のようなデータベース アクセス Api をサポートします。

-  ADO.NET framework。
-  SQLite NET サードパーティのライブラリです。

このガイドは、Xamarin.iOS アプリケーションで SQLite データベースにアクセスする ADO.NET および SQLite.NET を設定する方法を説明する前に一般にデータベースの概要を提供します。 

このドキュメントでのコードの大半は、完全にプラットフォーム間で変更せずに iOS または Android で実行されます。 説明した 2 つのサンプル アプリは。

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – 簡単なデータ操作は、結果をテキスト コントロールの表示を書き込みます
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – データ操作を一覧表示し、単純なデータ構造を編集する小規模な作業アプリケーションに統合します。

両方のサンプル ソリューションには、iOS と Android のサンプル アプリケーションのプロジェクトが含まれます。

Xamarin.Forms アプリケーション読み取り[データベースの操作](~/xamarin-forms/app-fundamentals/databases.md)xamarin.forms PCL ライブラリ SQLite を操作する方法について説明しています。

## <a name="sections"></a>セクション

-  [はじめに](introduction.md)
-  [構成](configuration.md)
-  [SQLite.NET ORM の使用](using-sqlite-orm.md)
-  [ADO.NET の使用](using-adonet.md)
-  [アプリでのデータの使用](using-data-in-an-app.md)

## <a name="summary"></a>まとめ

この章では、SQLite を使用して、データベース エンジンとして Xamarin.iOS 内のデータ アクセスについて説明します。 「直接」ADO.NET の構文を使用して、データベースにアクセスできるまたは SQLite.NET ORM を含めるし、c# でのデータ操作を実行できます。

2 つのサンプルを確認しました。 更新と削除機能をテキスト フィールドには、単純なアプリケーションを含む出力の作成、読み取り、非常に単純なデータ アクセス コードを含む 1 つです。 スレッド処理、および事前設定済み SQLite データベースを使用してアプリケーションをシードする方法についても説明しました。

クロスプラット フォームのデータ アクセスの他の例を参照してください、 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケース スタディです。

## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
