---
title: Xamarin. iOS データアクセス
description: このドキュメントでは、Xamarin iOS アプリケーションでローカルデータベースを操作する方法について説明しているガイドにリンクしています。 リンクされたコンテンツについては、SQLite.NET、ADO.NET などを参照してください。
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: 110d6c95c1bcc93d541b908807fe26c700eb656e
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526591"
---
# <a name="xamarinios-data-access"></a>Xamarin. iOS データアクセス

Xamarin iOS は、次のようなデータベースアクセス Api をサポートしています。

- ADO.NET フレームワーク。
- SQLite-NET サードパーティライブラリ。

このガイドでは、ADO.NET と SQLite.NET を設定して Xamarin アプリケーションの SQLite データベースにアクセスする方法を説明する前に、一般的なデータベースの概要について説明します。 

このドキュメントのコードの大部分は完全にクロスプラットフォームで、iOS または Android では変更なしで実行されます。 ここでは、次の2つのサンプルアプリについて説明します。

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) –単純データ操作は、結果をテキスト表示コントロールに書き込みます。
- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) –データ操作を、単純なデータ構造の一覧表示と編集を行う小さな動作アプリケーションに統合します。

どちらのサンプルソリューションにも、iOS と Android のサンプルアプリケーションプロジェクトが含まれています。

Xamarin のアプリケーションについては、「[データベースの操作](~/xamarin-forms/data-cloud/data/databases.md)」を参照してください。これは、xamarin を使用した PCL ライブラリで SQLite を操作する方法について説明しています。

## <a name="sections"></a>セクション

- [はじめに](introduction.md)
- [構成](configuration.md)
- [SQLite.NET ORM の使用](using-sqlite-orm.md)
- [ADO.NET の使用](using-adonet.md)
- [アプリでのデータの使用](using-data-in-an-app.md)

## <a name="summary"></a>まとめ

この章では、データベースエンジンとして SQLite を使用した Xamarin. iOS でのデータアクセスについて説明しました。 ADO.NET 構文を使用してデータベースに直接アクセスできます。また、SQLite.NET ORM を含め、でC#データ操作を実行することもできます。

2つのサンプルを確認しています。1つは、テキストフィールドに出力する非常に単純なデータアクセスコードを含むサンプルと、作成、読み取り、更新、および削除の機能を含む単純なアプリケーションです。 また、スレッド処理と、事前に設定された SQLite データベースを使用してアプリケーションをシード処理する方法についても説明しました。

クロスプラットフォームデータアクセスのその他の例については、 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)のケーススタディを参照してください。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms データアクセス](~/xamarin-forms/data-cloud/data/databases.md)
