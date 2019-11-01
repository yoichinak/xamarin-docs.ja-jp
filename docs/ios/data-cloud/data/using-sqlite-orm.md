---
title: Xamarin で SQLite.NET を使用する
description: SQLite.NET PCL NuGet ライブラリは、Xamarin iOS アプリ用の単純なデータアクセス機構を提供します。 このドキュメントでは、このライブラリの使用方法の概要について説明します。
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
ms.openlocfilehash: e229ad37e8cd5ff940fb5abece7b782b84336d50
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008160"
---
# <a name="using-sqlitenet-with-xamarinios"></a>Xamarin で SQLite.NET を使用する

Xamarin で推奨される SQLite.NET ライブラリは、iOS デバイス上のローカルの SQLite データベースにオブジェクトを格納および取得できる基本的な ORM です。
ORM は、オブジェクトリレーショナルマッピングを表します。これは、SQL ステートメントを記述せずにデータベースから "オブジェクト" を保存および取得できる API です。

<a name="Usage"/>

## <a name="usage"></a>使用方法

Xamarin アプリに SQLite.NET ライブラリを追加するには、次の NuGet パッケージをプロジェクトに追加します。

- **パッケージ名:** sqlite-net-pcl
- **作成者:** Frank A.
- **ID:** sqlite-net-pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet パッケージ](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet パッケージ")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> さまざまな SQLite パッケージを使用できます。適切なものを選択してください (検索の結果が上位ではない可能性があります)。

SQLite.NET ライブラリを使用できるようになったら、次の3つの手順に従ってデータベースにアクセスします。

1. **Using ステートメントを追加し**ます。データアクセスが必要C#なファイルに次のステートメントを追加します。

    ```csharp
    using SQLite;
    ```

1. **空のデータベースを作成**する-SQLiteConnection クラスコンストラクターのファイルパスを渡すことによって、データベース参照を作成できます。 ファイルが既に存在するかどうかを確認する必要はありません。必要に応じて自動的に作成されます。それ以外の場合は、既存のデータベースファイルが開きます。

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    DbPath 変数は、このドキュメントで既に説明したルールに従って決定する必要があります。

1. **データの保存**-SQLiteConnection オブジェクトを作成したら、次のように CreateTable や Insert などのメソッドを呼び出して、データベースコマンドを実行します。

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **データの取得**-オブジェクト (またはオブジェクトの一覧) を取得するには、次の構文を使用します。

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>基本的なデータアクセスのサンプル

このドキュメントの*DataAccess_Basic*サンプルコードは、iOS で実行されている場合は次のようになります。 このコードは、単純な SQLite.NET 操作を実行し、結果をアプリケーションのメインウィンドウのテキストとして表示する方法を示しています。

**Android**

 [![iOS SQLite.NET サンプル](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

次のコードサンプルは、SQLite.NET ライブラリを使用して、基になるデータベースアクセスをカプセル化するデータベース全体の相互作用を示しています。 次のように表示されます。

1. データベースファイルの作成
1. オブジェクトを作成して保存することによってデータを挿入する
1. データの照会

次の名前空間を含める必要があります。

```csharp
using SQLite; // from the github SQLite.cs class
```

これを行うに[は、ここで](#Usage)強調表示されているように、SQLite をプロジェクトに追加する必要があります。 SQLite データベーステーブルは、CREATE TABLE コマンドではなく、クラス (`Stock` クラス) に属性を追加することによって定義されることに注意してください。

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

テーブル名パラメーターを指定せずに `[Table]` 属性を使用すると、基になるデータベーステーブルの名前がクラス (この場合は "Stock") と同じになります。 ORM データアクセスメソッドを使用するのではなく、データベースに対して SQL クエリを直接記述する場合は、実際のテーブル名が重要になります。 同様に `[Column("_id")]` 属性は省略可能であり、存在しない場合は、クラスのプロパティと同じ名前のテーブルに列が追加されます。

## <a name="sqlite-attributes"></a>SQLite 属性

クラスに適用して、基になるデータベースへの格納方法を制御できる共通属性には、次のようなものがあります。

- **[PrimaryKey]** –この属性を整数プロパティに適用して、基になるテーブルの主キーにすることができます。 複合主キーはサポートされていません。
- **[AutoIncrement]** –この属性により、データベースに挿入された新しいオブジェクトごとに整数プロパティの値が自動インクリメントされます。
- **[列 (名前)]** –省略可能な `name` パラメーターを指定すると、基になるデータベース列の名前 (プロパティと同じ) の既定値が上書きされます。
- **[テーブル (名前)]** –クラスを、基になる SQLite テーブルに格納できるものとしてマークします。 省略可能な name パラメーターを指定すると、基になるデータベーステーブルの名前 (クラス名と同じ) の既定値が上書きされます。
- **[MaxLength (値)]** –データベースの挿入を試みたときに、テキストプロパティの長さを制限します。 コードを使用するには、オブジェクトを挿入する前にこれを検証する必要があります。この属性は、データベースの挿入操作または更新操作を実行しようとしたときにのみ ' checked ' になります。
- **[無視]** – SQLite.NET がこのプロパティを無視するようにします。 これは、データベースに格納できない型を持つプロパティや、自動的に解決できないモデルコレクションを使用するプロパティを SQLite として使用する場合に特に便利です。
- **[Unique]** –基になるデータベース列の値が一意であることを確認します。

これらの属性のほとんどは省略可能で、SQLite はテーブル名と列名に既定値を使用します。 データに対して選択と削除のクエリを効率的に実行できるように、常に整数の主キーを指定する必要があります。

## <a name="more-complex-queries"></a>より複雑なクエリ

`SQLiteConnection` の次のメソッドは、他のデータ操作を実行するために使用できます。

- **Insert** –新しいオブジェクトをデータベースに追加します。
- **Get\<t >** –主キーを使用してオブジェクトを取得しようとします。
- **Table\<t >** –テーブル内のすべてのオブジェクトを返します。
- **Delete** –主キーを使用してオブジェクトを削除します。
- **クエリ\<t >** -複数の行 (オブジェクトとして) を返す SQL クエリを実行します。
- **Execute** – SQL からの行が返されない場合 (INSERT、UPDATE、DELETE の各命令など) に、このメソッドを使用します (`Query` ではありません)。

### <a name="getting-an-object-by-the-primary-key"></a>主キーによるオブジェクトの取得

SQLite.Net は、主キーに基づいて1つのオブジェクトを取得する Get メソッドを提供します。

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq を使用したオブジェクトの選択

コレクションを返すメソッドでは、IEnumerable\<T > がサポートされるため、Linq を使用してテーブルの内容を照会または並べ替えることができます。 次のコードは、Linq を使用して、文字 "A" で始まるすべてのエントリをフィルターで除外する例を示しています。

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL を使用したオブジェクトの選択

SQLite.Net はデータへのオブジェクトベースのアクセスを提供できますが、Linq で許可されているよりも複雑なクエリを実行することが必要になる場合があります (または、より高速なパフォーマンスが必要になる場合があります)。 次に示すように、クエリメソッドで SQL コマンドを使用できます。

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> SQL ステートメントを直接記述する場合は、クラスとその属性から生成された、データベース内のテーブルと列の名前に依存関係を作成します。 コード内でこれらの名前を変更する場合は、手動で記述した SQL ステートメントを必ず更新してください。

### <a name="deleting-an-object"></a>オブジェクトの削除

主キーは、次に示すように、行を削除するために使用されます。

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

`rowcount` を確認して、影響を受けた行数 (この場合は削除されたもの) を確認できます。

## <a name="using-sqlitenet-with-multiple-threads"></a>複数のスレッドでの SQLite.NET の使用

SQLite は、*シングルスレッド*、*マルチスレッド*、*シリアル化*の3種類のスレッド処理モードをサポートしています。 制限なく複数のスレッドからデータベースにアクセスする場合は、**シリアル化**されたスレッドモードを使用するように SQLite を構成できます。 このモードは、アプリケーションの早い段階で設定することが重要です (たとえば、`OnCreate` メソッドの先頭にあります)。

スレッド処理モードを変更するには、`Mono.Data.Sqlite` 名前空間にある `SqliteConnection.SetConfig` を呼び出します。 たとえば、次のコード行では、**シリアル化**モード用に SQLite を構成しています。

```csharp
using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
