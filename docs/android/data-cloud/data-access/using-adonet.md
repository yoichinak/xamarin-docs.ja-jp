---
title: Android での ADO.NET の使用
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: 6592bd6d5cf7b78918fa2d020be723d662625e06
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023767"
---
# <a name="using-adonet-with-android"></a>Android での ADO.NET の使用

Xamarin には、Android で利用できる SQLite データベースのサポートが組み込まれており、使い慣れた ADO.NET に似た構文を使用して公開できます。 これらの Api を使用するには、`CREATE TABLE`、`INSERT`、`SELECT` ステートメントなど、SQLite によって処理される SQL ステートメントを記述する必要があります。

## <a name="assembly-references"></a>アセンブリ参照

ADO.NET 経由で SQLite へのアクセスを使用するには、次に示すように、Android プロジェクトに `System.Data` と `Mono.Data.Sqlite` 参照を追加する必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Visual Studio での Android リファレンス](using-adonet-images/image7.png "Visual Studio での Android リファレンス") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos) 

![Visual Studio for Mac での Android リファレンス](using-adonet-images/image5.png "Visual Studio for Mac での Android リファレンス") 

-----

[参照の**編集] >** 右クリックし、[参照の編集] をクリックして必要なアセンブリを選択します。

## <a name="about-monodatasqlite"></a>Mono. Data. Sqlite の概要

`Mono.Data.Sqlite.SqliteConnection` クラスを使用して、空のデータベースファイルを作成し、その後、データベースに対して SQL 命令を実行するために使用できる `SqliteCommand` オブジェクトをインスタンス化します。

**空のデータベースを作成する**場合は、有効な (つまり書き込み可能な) ファイルパスを指定して、`CreateFile` メソッド &ndash; 呼び出します。 このメソッドを呼び出す前に、ファイルが既に存在するかどうかを確認する必要があります。そうしないと、新しい (空の) データベースが古いデータベースの先頭に作成され、古いファイルのデータは失われます。
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` 変数は、このドキュメントで既に説明したルールに従って決定する必要があります。

**データベース接続の作成**SQLite データベースファイルが作成された後 &ndash;、データにアクセスするための接続オブジェクトを作成できます。 接続は、次に示すように、`Data Source=file_path`の形式の接続文字列を使用して構築されます。

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

既に説明したように、異なるスレッド間で接続を再利用することはできません。 不明な場合は、必要に応じて接続を作成し、完了したら閉じます。しかし、必要以上に多くのことを行うことに注意してください。

接続が確立されたら、**データベースコマンドを作成して実行**&ndash;、任意の SQL コマンドを実行できます。 次のコードは、実行されている `CREATE TABLE` ステートメントを示しています。

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

データベースに対して SQL を直接実行する場合は、既に存在するテーブルを作成しようとするなど、無効な要求を行わないようにするための通常の予防措置を講じる必要があります。 **SQLite エラーテーブル [Items] が既に存在**するなどの `SqliteException` が発生しないように、データベースの構造を記録しておきます。

## <a name="basic-data-access"></a>基本的なデータアクセス

このドキュメントの*DataAccess_Basic*サンプルコードは、Android で実行すると次のようになります。

![Android ADO.NET のサンプル](using-adonet-images/image8.png "Android ADO.NET のサンプル")

次のコードは、単純な SQLite 操作を実行し、結果をアプリケーションのメインウィンドウのテキストとして表示する方法を示しています。

次の名前空間を含める必要があります。

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

次のコードサンプルは、データベースとの対話全体を示しています。

1. データベースファイルの作成
2. データの挿入
3. データの照会

これらの操作は通常、コード内の複数の場所に表示されます。たとえば、アプリケーションが最初に起動したときにデータベースファイルとテーブルを作成し、アプリ内の個々の画面でデータの読み取りと書き込みを実行できます。 次の例では、この例の1つのメソッドにグループ化されています。

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

## <a name="more-complex-queries"></a>より複雑なクエリ

SQLite を使用すると、任意の SQL コマンドをデータに対して実行できるため、任意の `CREATE`、`INSERT`、`UPDATE`、`DELETE`、または `SELECT` のステートメントを実行できます。 SQLite でサポートされている SQL コマンドについては、SQLite の web サイトを参照してください。 SQL ステートメントは、`SqliteCommand` オブジェクトに対して次の3つのメソッドのいずれかを使用して実行されます。

- **ExecuteNonQuery** &ndash; 通常、テーブルの作成またはデータの挿入に使用します。 一部の操作の戻り値は、影響を受ける行の数を示します。それ以外の場合は-1 になります。

- **ExecuteReader** &ndash;、行のコレクションを `SqlDataReader`として返す必要がある場合に使用します。

- **ExecuteScalar** &ndash; は、1つの値 (集計など) を取得します。

### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`、`UPDATE`、および `DELETE` ステートメントは、影響を受ける行の数を返します。 その他の SQL ステートメントはすべて-1 を返します。

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

次のメソッドは、`SELECT` ステートメント内の `WHERE` 句を示しています。
コードは完全な SQL ステートメントを作成するため、文字列の前後の引用符 (') などの予約文字をエスケープする必要があります。

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

`ExecuteReader` メソッドは `SqliteDataReader` オブジェクトを返します。 この例に示されている `Read` メソッドに加えて、次のような便利なプロパティもあります。

- **RowsAffected** &ndash; クエリの影響を受ける行の数。

- **Hasrows**は、行が返されたかどうか &ndash; ます。

### <a name="executescalar"></a>EXECUTESCALAR

これは、1つの値 (集計など) を返す `SELECT` ステートメントに使用します。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` メソッドの戻り値の型は `object` &ndash;、データベースクエリに応じて結果をキャストする必要があります。 結果として、`COUNT` クエリの整数、または1つの列 `SELECT` クエリの文字列を指定できます。 これは、リーダーオブジェクトまたは影響を受ける行の数のカウントを返す他の `Execute` メソッドとは異なることに注意してください。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)
