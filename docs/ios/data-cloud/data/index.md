---
title: Xamarin.iOS のデータ アクセス
description: このドキュメントは、Xamarin.iOS アプリケーションでローカルのデータベースを操作する方法を説明するガイドにリンクしています。 リンクされたコンテンツは、SQLite.NET、ADO.NET などについて説明します。
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: 420f52a055dc1c03a017723ab34c2fc3b5363656
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650225"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS のデータ アクセス

Xamarin.iOS は、次のようなデータベース アクセス Api をサポートします。

-  ADO.NET フレームワークです。
-  SQLite NET サード パーティのライブラリ。

このガイドは、Xamarin.iOS アプリケーションでの SQLite データベースにアクセスする ADO.NET と SQLite.NET を設定する方法を説明する前に、一般にデータベースの概要を提供します。 

このドキュメントのコードの大部分は、クロス プラットフォームの完全には、され、変更しなくても iOS または Android で実行されます。 説明した 2 つのサンプル アプリがあります。

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – 簡単なデータ操作が、結果をテキスト コントロールの表示を書き込みます
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – データ操作を一覧表示および単純なデータ構造を編集する小さな作業アプリケーションに統合します。

両方のサンプル ソリューションには、iOS と Android のサンプル アプリケーションのプロジェクトが含まれます。

Xamarin.Forms アプリケーションでは、読み取る[データベースでの作業](~/xamarin-forms/data-cloud/data/databases.md)PCL ライブラリを Xamarin.Forms での SQLite を使用する方法を説明しています。

## <a name="sections"></a>セクション

-  [はじめに](introduction.md)
-  [構成](configuration.md)
-  [SQLite.NET ORM の使用](using-sqlite-orm.md)
-  [ADO.NET の使用](using-adonet.md)
-  [アプリでのデータの使用](using-data-in-an-app.md)

## <a name="summary"></a>まとめ

この章では、データベース エンジンに SQLite を使用して Xamarin.iOS でのデータ アクセスについて説明します。 データベースは、「直接」ADO.NET 構文を使用してアクセスできるまたは SQLite.NET ORM を追加し、c# でのデータの操作を実行できます。

2 つのサンプルを確認しました。 更新と削除機能をテキスト フィールドには、単純なアプリケーションを含む出力の作成、読み取り、非常に単純なデータ アクセス コードを含む 1 つ。 また、スレッドと、あらかじめ設定されている SQLite データベースを使用してアプリケーションをシードする方法を説明しました。

クロス プラットフォームのデータ アクセスの他の例を参照してください、 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケース スタディ。

## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/data-cloud/data/databases.md)
