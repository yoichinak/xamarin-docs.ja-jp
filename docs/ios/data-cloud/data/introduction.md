---
title: Xamarin. iOS アプリのデータストレージの概要
description: このドキュメントでは、Xamarin iOS アプリケーションでのデータストレージのさまざまな方法について説明し、SQLite の利点に関する特定の情報を提供します。
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: eefe57abd4ebf4986411a1d717aebd131ebf408f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008343"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Xamarin. iOS アプリのデータストレージの概要

## <a name="when-to-use-a-database"></a>データベースを使用する場合

モバイルデバイスの記憶域と処理能力が向上していますが、携帯電話やタブレットは、デスクトップ &amp; 対応するノート pc に遅れます。 このため、データベースが常に適切な回答であると想定するだけではなく、アプリのデータストレージアーキテクチャを計画するには時間がかかります。 要件に応じて、次のようなさまざまなオプションがあります。

- **基本設定**– iOS には、データの単純なキーと値のペアを格納するための組み込みメカニズムが用意されています。 単純なユーザー設定または少量のデータ (パーソナル化情報など) を格納する場合は、プラットフォームのネイティブ機能を使用して、この種類の情報を格納します。 IOS の場合は、複数のデバイスを持つユーザーのバックアップと同期の両方で、このデータに対して iCloud 同期を利用することもできます。
- **テキストファイル**-ダウンロードしたコンテンツのユーザー入力またはキャッシュ (例: HTML) は、ファイルシステムに直接格納できます。 ファイルの整理とデータの検索には、適切なファイルの名前付け規則を使用します。
- **シリアル化**されたデータファイル-ファイルシステム上でオブジェクトを XML または JSON として永続化できます。 .NET framework には、オブジェクトのシリアル化と逆シリアル化を簡単にするためのライブラリが用意されています。 適切な名前を使用して、データファイルを整理します。
- **データベース**– SQLite データベースエンジンは iOS で使用でき、クエリ、並べ替え、またはその他の操作に必要な構造化データを格納する場合に便利です。 データベースストレージは、多くのプロパティを持つデータの一覧に適しています。
- **イメージファイル**–バイナリデータをモバイルデバイスのデータベースに格納することはできますが、ファイルシステムに直接格納することをお勧めします。 必要に応じて、データベースにファイル名を格納して、画像を他のデータに関連付けることができます。 大きなイメージまたは多くのイメージを扱う場合は、ユーザーの記憶領域をすべて消費しないようにするために不要になったファイルを削除するキャッシュ戦略を計画することをお勧めします。

データベースがアプリの適切なストレージメカニズムである場合、このドキュメントの残りの部分では、Xamarin プラットフォームで SQLite を使用する方法について説明します。

## <a name="advantages-of-using-a-database"></a>データベースを使用する利点

モバイルアプリで SQL データベースを使用することには、いくつかの利点があります。

- SQL データベースを使用すると、構造化データを効率的に保存できます。
- 複雑なクエリを使用して、特定のデータを抽出することができます。
- クエリ結果を並べ替えることができます。
- クエリ結果は集計できます。
- 既存のデータベーススキルを持つ開発者は、その知識を活用してデータベースとデータアクセスコードを設計できます。
- 接続されたアプリケーションのサーバーコンポーネントからのデータモデルは、モバイルアプリケーションで (全体または一部で) 再利用される場合があります。

## <a name="sqlite-database-engine"></a>SQLite データベースエンジン

SQLite は、モバイルプラットフォーム用に Apple によって採用されたオープンソースのデータベースエンジンです。 SQLite データベースエンジンは iOS に組み込まれているため、開発者はこれを利用することはできません。 SQLite は、次の理由により、クロスプラットフォームモバイル開発に適しています。

- データベースエンジンは、高速で簡単に移植できます。
- データベースは1つのファイルに格納され、モバイルデバイスで簡単に管理できます。
- ファイル形式は、32-または64ビット、ビッグエンディアン、リトルエンディアンのいずれのシステムでも、プラットフォーム間で簡単に使用できます。
- ほとんどの SQL92 標準が実装されています。

SQLite は小規模で高速なので、使用に関していくつかの注意事項があります。

- 一部の外部結合構文はサポートされていません。
- Table RENAME と ADDCOLUMN のみがサポートされています。 スキーマに対して他の変更を行うことはできません。
- ビューは読み取り専用です。

SQLite の詳細については、 [SQLite.org](https://SQLite.org)を参照してください。ただし、Xamarin で sqlite を使用するために必要なすべての情報は、このドキュメントおよび関連するサンプルに含まれています。 SQLite データベースエンジンは、すべてのバージョンの iOS に組み込まれています。
この章では説明しませんが、SQLite は Windows Phone および Windows アプリケーションでも使用できます。

## <a name="windows-and-windows-phone"></a>Windows および Windows Phone

SQLite は Windows プラットフォームでも使用できますが、このドキュメントでは説明しません。
詳細については、「 [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)のケーススタディ」と「 [Tim heuer のブログ](https://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx)」を参照してください。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
