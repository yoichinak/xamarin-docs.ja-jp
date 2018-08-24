---
title: Xamarin.iOS で ADO.NET を使用します。
description: このドキュメントでは、Xamarin.iOS アプリケーションで SQLite にアクセスする方法として、ADO.NET を使用する方法について説明します。 これは、アセンブリ参照、Mono.Data.Sqlite、および BasicDataAccess サンプルについて説明します。
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 83f6059c405b2156270f4359cbba33177861af02
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241239"
---
# <a name="using-adonet-with-xamarinios"></a>Xamarin.iOS で ADO.NET を使用します。

Xamarin は、iOS、使い慣れた ADO.NET に似た構文を使用して公開で利用可能な SQLite データベースの組み込みサポートしています。 など、SQLite などによって処理される SQL ステートメントを記述するこれらの Api を使用する必要があります`CREATE TABLE`、`INSERT`と`SELECT`ステートメント。

## <a name="assembly-references"></a>アセンブリ参照

アクセスする必要がありますを追加する ADO.NET を使用して SQLite を使用する`System.Data`と`Mono.Data.Sqlite`(Visual Studio for Mac と Visual Studio でのサンプル) についてはここで示すように、iOS プロジェクトへの参照します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Visual Studio for Mac でのアセンブリ参照")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Visual Studio でアセンブリ参照")

-----

右クリックして**参照 > 参照の編集.** 必要なアセンブリを選択するには、をクリックします。

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite について

使用、`Mono.Data.Sqlite.SqliteConnection`空のデータベース ファイルを作成するクラスをインスタンス化にし、`SqliteCommand`オブジェクト、データベースに対して SQL 命令の実行に使用することができます。


1. **空のデータベースを作成する**-呼び出す、`CreateFile`有効なメソッド (ie。 書き込み可能な) ファイルのパス。 このメソッドを呼び出す前に、ファイルが既に存在するかどうか、新しい (空) のデータベースが作成して、古いものの上にそれ以外の場合と、古いファイル内のデータは失われますを確認する必要があります。

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath`変数は、このドキュメントで前述したルールに従って決定する必要があります。

2. **データベース接続を作成する**- SQLite データベースのファイルが作成された後、データにアクセスする接続オブジェクトを作成することができます。 接続が接続文字列の形式で構築された`Data Source=file_path`ここに示すように。

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    前述のように、接続べきでは再利用される異なるスレッド間で。 確認してみて、必要に応じて接続を作成および; 完了すると閉じますより多くの場合も必要すぎるを考慮してください。
    
3. **データベース コマンドの実行の作成と**- 行ってから、接続に対して任意の SQL コマンドを実行します。 次のコードでは、実行中の CREATE TABLE ステートメントを示します。

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

データベースに対して直接 SQL を実行するときに、既に存在するテーブルを作成しようとしてなどの無効な要求しないようにすることで、通常の予防措置を行う必要があります。 追跡データベースの構造を SqliteException は、「既に SQLite エラー テーブル [項目] が存在します」などが発生しないようにします。

## <a name="basic-data-access"></a>基本的なデータ アクセス

*DataAccess_Basic* iOS で実行されているときに、このドキュメントのサンプル コードがこのような検索します。

 ![](using-adonet-images/image9.png "iOS の ADO.NET のサンプル")

次のコードでは、SQLite の単純な操作を実行する方法を示していて、アプリケーションのメイン ウィンドウでテキストとしての結果を示します。

これらの名前空間を含める必要があります。

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

次のコード サンプルは、データベース全体の相互作用を示しています。

1.  データベース ファイルの作成
2.  一部のデータを挿入します。
3.  データの照会

これらの操作は通常に表示されます、コード全体で複数の場所など、アプリケーションが初めて起動したときに、データベース ファイルとテーブルを作成し、個々 の画面で、アプリでデータの読み取りと書き込みを実行する可能性があります。 次の例でがこの例の 1 つのメソッドに分類されています。

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}
```

## <a name="more-complex-queries"></a>複雑なクエリ

SQLite は、任意の SQL コマンドのデータに対して実行することで、あるため、ものを作成、挿入、更新、削除または SELECT ステートメントなどを実行できます。 Sqlite の web サイトでは SQLite でサポートする SQL コマンドの詳細について確認できます。 SqliteCommand オブジェクトの 3 つのメソッドのいずれかを使用して SQL ステートメントが実行されます。

-  **ExecuteNonQuery** – テーブルの作成やデータの挿入に通常使用されます。 一部の操作の戻り値は影響を受ける行の数、それ以外の場合は-1。
-  **ExecuteReader** -行のコレクションとして返されるときに使用する`SqlDataReader`します。
-  **ExecuteScalar** – 1 つの値 (集計など) を取得します。


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT、UPDATE および DELETE ステートメントでは、影響を受ける行の数を返します。 その他のすべての SQL ステートメントでは、-1 を返します。

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

次のメソッドでは、SELECT ステートメントで WHERE 句を示しています。 コードが完全な SQL ステートメントを作成するために文字列を囲む引用符 (') などの予約文字をエスケープように注意する必要があります。

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

ExecuteReader メソッドは、SqliteDataReader オブジェクトを返します。 だけでなく、Read メソッドの例に示すように、その他の便利なプロパティは次のとおりです。

-  **RowsAffected** – クエリによって影響を受ける行の数。
-  **HasRows** – すべての行が返されたかどうか。


### <a name="executescalar"></a>EXECUTESCALAR

これは、(集計) などの 1 つの値を返す SELECT ステートメントを使用します。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`メソッドの戻り値の型は`object`– データベース クエリによって結果をキャストする必要があります。 結果は、COUNT クエリからの整数または 1 つの列の SELECT クエリからの文字列。 リーダー オブジェクトまたは影響を受ける行の数を返す他の Execute メソッドに異なることに注意してください。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
