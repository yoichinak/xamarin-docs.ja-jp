---
title: Android でのデータ ストレージの概要
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: 42af02d2ed2a679d89425257d92f03a3764675ce
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2018
---
# <a name="introduction"></a>はじめに

## <a name="when-to-use-a-database"></a>データベースを使用する場合

モバイル デバイスのストレージおよび処理能力を増やすと、中に携帯電話とタブレットが遅れ、デスクトップとラップトップの対応します。 このために値しますだけがある場合、データベース、適切な回答、すべての時間よりも、アプリのデータ ストレージ アーキテクチャを計画するには、いくつか時間がかかっています。 いくつかのように、要件に合わせてさまざまなオプション。

-  **環境設定**– Android には、データの単純なキーと値のペアを格納するための組み込みメカニズムが提供しています。 シンプルなユーザー設定または小さな (パーソナル化情報など) のデータを格納する場合は、この種の情報を格納するため、プラットフォームのネイティブ機能を使用します。
-  **テキスト ファイル**– ユーザー入力やキャッシュのダウンロード (コンテンツ HTML) は、ファイル システムに直接格納できます。 適切なファイルの名前付け規則を使用して、ファイルを整理し、データを検索します。
-  **データ ファイルをシリアル化**– ファイル システムに、オブジェクトを XML または JSON として永続化できます。 .NET framework には、シリアル化して、簡単なオブジェクトをシリアル化解除を構成するライブラリが含まれています。 適切な名前を使用すると、データ ファイルを整理できます。
-  **データベース**–、SQLite データベース エンジンは、Android プラットフォームで使用できると、格納するのに役立ちます構造体は、クエリ、並べ替え、またはそれ以外の場合を操作する必要のあるデータ。 データベース ストレージは、多くのプロパティを使用してデータの一覧に適しています。
-  **イメージ ファイル**– モバイル デバイス上のデータベースにバイナリ データを格納することはできますは、ファイル システムに直接格納することをお勧めします。 必要な場合は、データに関連付けるイメージの他のデータベースにファイル名を格納できます。 大きなイメージは、または多数のイメージを扱う場合は、不要になったすべてのユーザーの記憶域スペースを消費しないようにファイルを削除するキャッシュ戦略を計画することをお勧めします。

データベースが、適切なストレージ メカニズムをアプリの場合は、このドキュメントの残りの部分は、Xamarin プラットフォームで SQLite を使用する方法について説明します。

## <a name="advantages-of-using-a-database"></a>データベースを使用する利点

モバイル アプリで、SQL データベースを使用する利点を次の数があります。

-  SQL データベースでは、構造化データの効率的な記憶域を許可します。
-  複雑なクエリでは、特定のデータを抽出できます。
-  クエリの結果を並べ替えることができます。
-  クエリの結果を集計できます。
-  データベースの既存のスキルを持つ開発者には、データベースとデータ アクセス コードをデザインするには、知識を利用できます。
-  接続型アプリケーションのサーバー コンポーネントからのデータ モデル再利用する (全体または一部) モバイル アプリケーションにします。


## <a name="sqlite-database-engine"></a>SQLite データベース エンジン

SQLite は、Google によって、モバイル プラットフォームを採用しているオープン ソースのデータベース エンジンです。 SQLite データベース エンジンは、両方のオペレーティング システムに組み込まれてために活用するために開発者のための追加の作業はありません。 SQLite ではクロス プラットフォーム モバイル開発に適してします。

-  データベース エンジンは、小さな、高速で簡単に移動します。
-  データベースは、モバイル デバイスで管理しやすい、1 つのファイルに格納されます。
-  プラットフォーム間でファイルの形式が使いやすい: かどうか 32 ビットまたは 64 ビット、およびビッグ エンディアンまたはリトル エンディアン システムです。
-  標準的な SQL92 のほとんどを実装します。


SQLite は、小規模で高速にするよう設計されていますが、ため上にあるいくつかの注意事項の使用です。

-  OUTER join 構文がサポートされていません。
-  のみテーブルの名前を変更して、ADDCOLUMN がサポートされます。 その他のスキーマを変更したりを実行することはできません。
-  ビューは、読み取り専用です。


-Web サイトで SQLite 方法について学習できる[SQLite.org](http://SQLite.org) SQLite を使用して、Xamarin を使用する必要があります。 すべての情報は、このドキュメントに含まれてし、サンプルに関連付けられている - です。 SQLite データベース エンジンは、Android 2 以降、Android でサポートされています。
この章で説明されていない、SQLite が Windows Phone や Windows アプリケーションで使用します。

## <a name="windows-and-windows-phone"></a>Windows および Windows Phone

これらのプラットフォームは、このドキュメントでは説明しませんが、SQLite も Windows プラットフォームでは、使用できます。
詳細については、 [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)と[Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケース スタディ、および確認[Tim Heuer ブログ](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx)です。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データ レシピ](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
