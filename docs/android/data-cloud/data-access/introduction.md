---
title: Android 上のデータ ストレージの概要
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: 26576fe31919822237022572a4e490cf6fc19d65
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241445"
---
# <a name="introduction"></a>はじめに

## <a name="when-to-use-a-database"></a>データベースを使用する場合

モバイル デバイスのストレージと処理能力を増やすと、中に携帯電話とタブレットでも、デスクトップおよびラップトップの対応するより遅延します。 このためだけと仮定すると、データベースが常に正しい答えではなく、アプリのデータ ストレージ アーキテクチャを計画するいくつかの時間がかかってお勧めします。 など、要件に合わせてさまざまなオプションを数多くあります。

-  **基本設定**– Android には、データの単純なキーと値のペアを格納するための組み込みメカニズムが提供しています。 シンプルなユーザー設定または小規模なパーソナル化情報) などのデータを格納する場合は、この種の情報を格納するため、プラットフォームのネイティブ機能を使用します。
-  **テキスト ファイル**– ユーザー入力またはキャッシュのダウンロードされたコンテンツ (例。 HTML) は、ファイル システムに直接格納できます。 適切なファイルの名前付け規則を使用して、ファイルを整理し、データを検索します。
-  **データ ファイルをシリアル化**– オブジェクトは XML または JSON としてファイル システムに永続化することができます。 .NET framework には、シリアル化して、簡単なオブジェクトをシリアル化解除を行うライブラリが含まれています。 適切な名前を使用すると、データ ファイルを整理できます。
-  **データベース**:、SQLite データベース エンジンは、Android のプラットフォームで利用し、格納するための構造を照会、並べ替え、またはそれ以外の場合を操作する必要があるデータ。 データベース ストレージは、多くのプロパティを使用してデータの一覧に適しています。
-  **イメージ ファイル**– モバイル デバイス上のデータベースにバイナリ データを格納することはできますは、ファイル システムに直接格納することをお勧めします。 必要な場合に、イメージを他のデータに関連付けるデータベースのファイル名を格納できます。 大きいイメージは、または多数のイメージを扱う場合は、すべてのユーザーの記憶域スペースを消費しないようにする必要がなくなったファイルを削除するキャッシュ戦略を計画することをお勧めします。

データベースがアプリの適切なストレージ メカニズムである場合は、このドキュメントの残りの部分には、Xamarin プラットフォームでの SQLite を使用する方法について説明します。

## <a name="advantages-of-using-a-database"></a>データベースを使用する利点

モバイル アプリで、SQL database を使用する利点を数多くあります。

-  SQL データベースでは、構造化データの効率的な記憶域を許可します。
-  複雑なクエリでは、特定のデータを抽出できます。
-  クエリの結果を並べ替えることができます。
-  クエリの結果を集計することができます。
-  データベースの既存のスキルを持つ開発者は、データベースとデータ アクセス コードを設計するためを利用できます。
-  接続されているアプリケーションのサーバー コンポーネントからのデータ モデル再利用する (全体または一部)、モバイル アプリケーションにします。


## <a name="sqlite-database-engine"></a>SQLite データベース エンジン

SQLite とは、Google によって、モバイル プラットフォームを採用しているオープン ソースのデータベース エンジンです。 SQLite データベース エンジンは、活用するために開発者向けの追加の作業はありません両方のオペレーティング システムに組み込まれています。 SQLite は、ため、クロス プラットフォーム モバイル開発に適してします。

-  データベース エンジンは、小規模で高速かつ簡単に移植可能なです。
-  データベースは、モバイル デバイス管理が容易には、1 つのファイルに格納されます。
-  プラットフォーム間でファイル形式が使いやすい: かどうか 32 ビットまたは 64 ビット、およびビッグ エンディアンまたはリトル エンディアン システム。
-  ほとんどの SQL92 標準を実装します。


SQLite は、小規模で高速に設計されている、ためにいくつかの注意事項の使用方法はあります。

-  OUTER join 構文がサポートされていません。
-  名前の変更にのみテーブルおよび ADDCOLUMN がサポートされます。 スキーマに他の変更を行うことはできません。
-  ビューは、読み取り専用です。


SQLite の詳細 - web サイトについては、 [SQLite.org](http://SQLite.org) - SQLite を使用して、Xamarin を使用する必要があるすべての情報は、このドキュメントに含まれているため、サンプルに関連付けられています。 SQLite データベース エンジンは、Android 2 以降、Android でサポートされています。
この章でカバーされていません、SQLite は Windows Phone と Windows アプリケーションで使用できるもします。

## <a name="windows-and-windows-phone"></a>Windows および Windows Phone

これらのプラットフォームはこのドキュメントでは説明しませんが、SQLite はまた、Windows プラットフォームで使用できます。
詳細については、 [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)と[Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケース スタディ、および確認[Tim Heuer のブログ](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx)します。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android のデータのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
