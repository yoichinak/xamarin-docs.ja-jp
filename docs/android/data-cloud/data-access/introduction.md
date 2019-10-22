---
title: Android でのデータストレージの概要
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 69d5222bb6c50870d0c42bea6ff71236e3d1580c
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70754564"
---
# <a name="introduction"></a>概要

## <a name="when-to-use-a-database"></a>データベースを使用する場合

モバイルデバイスの記憶域と処理能力は増加していますが、携帯電話やタブレットは、対応するデスクトップやノート pc に遅れています。 このため、データベースが常に適切な回答であると想定するだけではなく、アプリのデータストレージアーキテクチャを計画するには時間がかかります。 要件に応じて、次のようなさまざまなオプションがあります。

- **基本設定**– Android には、単純なキーと値のペアを格納するための組み込みメカニズムが用意されています。 単純なユーザー設定または少量のデータ (パーソナル化情報など) を格納する場合は、プラットフォームのネイティブ機能を使用して、この種類の情報を格納します。
- **テキストファイル**-ダウンロードしたコンテンツのユーザー入力またはキャッシュ (例: HTML) は、ファイルシステムに直接格納できます。 ファイルの整理とデータの検索には、適切なファイルの名前付け規則を使用します。
- **シリアル化**されたデータファイル-ファイルシステム上でオブジェクトを XML または JSON として永続化できます。 .NET framework には、オブジェクトのシリアル化と逆シリアル化を簡単にするためのライブラリが用意されています。 適切な名前を使用して、データファイルを整理します。
- **データベース**– SQLite データベースエンジンは、Android プラットフォームで使用でき、クエリ、並べ替え、またはその他の操作に必要な構造化データを格納する場合に便利です。 データベースストレージは、多くのプロパティを持つデータの一覧に適しています。
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

SQLite は、モバイルプラットフォーム用に Google によって採用されたオープンソースのデータベースエンジンです。 SQLite データベースエンジンは両方のオペレーティングシステムに組み込まれているため、開発者はこれを利用することはできません。 SQLite は、次の理由により、クロスプラットフォームモバイル開発に適しています。

- データベースエンジンは、高速で簡単に移植できます。
- データベースは1つのファイルに格納され、モバイルデバイスで簡単に管理できます。
- ファイル形式は、32-または64ビット、ビッグエンディアン、リトルエンディアンのいずれのシステムでも、プラットフォーム間で簡単に使用できます。
- ほとんどの SQL92 標準が実装されています。

SQLite は小規模で高速なので、使用に関していくつかの注意事項があります。

- 一部の外部結合構文はサポートされていません。
- Table RENAME と ADDCOLUMN のみがサポートされています。 スキーマに対して他の変更を行うことはできません。
- ビューは読み取り専用です。

SQLite の詳細については、 [SQLite.org](http://SQLite.org)を参照してください。ただし、Xamarin で sqlite を使用するために必要なすべての情報は、このドキュメントおよび関連するサンプルに含まれています。 Android 2 以降では、SQLite データベースエンジンが Android でサポートされています。
この章では説明しませんが、SQLite は Windows Phone および Windows アプリケーションでも使用できます。

## <a name="windows-and-windows-phone"></a>Windows および Windows Phone

SQLite は Windows プラットフォームでも使用できますが、このドキュメントでは説明しません。
詳細については、「 [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)と[tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケーススタディ」と「 [Tim heuer のブログ](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx)」を参照してください。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
