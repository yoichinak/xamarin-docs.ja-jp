---
title: Android と ADO.NET を使用します。
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 7ecf7244fb2ccbe0e4163c89941f9de5138ba713
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674952"
---
# <a name="using-adonet-with-android"></a>Android と ADO.NET を使用します。

Xamarin は、Android で使用できるは、使い慣れた ADO.NET に似た構文を使用して公開できます SQLite データベースの組み込みサポートしています。 など、SQLite などによって処理される SQL ステートメントを記述するこれらの Api を使用する必要があります`CREATE TABLE`、`INSERT`と`SELECT`ステートメント。

## <a name="assembly-references"></a>アセンブリ参照

アクセスする必要がありますを追加する ADO.NET を使用して SQLite を使用する`System.Data`と`Mono.Data.Sqlite`次に示すように、Android プロジェクトに参照します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Visual Studio での android のリファレンス](using-adonet-images/image7.png "Visual Studio で Android の参照") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos) 

![Visual Studio for Mac での android のリファレンス](using-adonet-images/image5.png "Mac 用の Visual Studio で Android を参照") 

-----


右クリックして**参照 > 参照の編集.** 必要なアセンブリを選択するには、をクリックします。

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite について

使用、`Mono.Data.Sqlite.SqliteConnection`空のデータベース ファイルを作成するクラスをインスタンス化にし、`SqliteCommand`オブジェクト、データベースに対して SQL 命令の実行に使用することができます。

**空のデータベースを作成する**&ndash;呼び出し、`CreateFile`有効な (つまり書き込み可能な) ファイルのパスを持つメソッド。 このメソッドを呼び出す前に、ファイルが既に存在するかどうか、新しい (空) のデータベースが作成して、古いものの上にそれ以外の場合と、古いファイル内のデータは失われますをチェックする必要があります。
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath`変数は、このドキュメントで前述したルールに従って決定する必要があります。

**データベース接続を作成する** &ndash; SQLite データベースのファイルが作成された後は、データにアクセスする接続オブジェクトを作成することができます。 接続が接続文字列の形式で構築された`Data Source=file_path`ここに示すように。

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

前述のように、接続べきでは再利用される異なるスレッド間で。 確認してみて、必要に応じて接続を作成および; 完了すると閉じますより多くの場合も必要すぎるを考慮してください。

**データベース コマンドの実行の作成と**&ndash;の接続を取得したら、それに対して任意の SQL コマンドを実行することができます。 次のコードを`CREATE TABLE`ステートメントが実行されています。

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

データベースに対して直接 SQL を実行するときに、既に存在するテーブルを作成しようとしてなどの無効な要求しないようにすることで、通常の予防措置を行う必要があります。 追跡データベースの構造が発生しないように、`SqliteException`など**SQLite エラー テーブル [項目] は既に存在**します。

## <a name="basic-data-access"></a>基本的なデータ アクセス

*DataAccess_Basic* Android で実行されているときに、このドキュメントのサンプル コードがこのような検索します。

![Android の ADO.NET サンプル](using-adonet-images/image8.png "Android ADO.NET のサンプル")

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

SQLite は、任意の SQL コマンドのデータに対して実行することで、あるため、すべてを実行できます`CREATE`、 `INSERT`、 `UPDATE`、 `DELETE`、または`SELECT`たいステートメント。 SQLite の web サイトでは SQLite でサポートする SQL コマンドの詳細について確認できます。 3 つのメソッドのいずれかを使用して SQL ステートメントを実行する`SqliteCommand`オブジェクト。

-   **ExecuteNonQuery** &ndash;通常、テーブルの作成やデータの挿入に使用します。 一部の操作の戻り値は影響を受ける行の数、それ以外の場合は-1。

-   **ExecuteReader** &ndash;行のコレクションとして返されるときに使用する`SqlDataReader`します。

-   **ExecuteScalar** &ndash; (集計など) の 1 つの値を取得します。


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`、 `UPDATE`、および`DELETE`ステートメントでは、影響を受ける行の数を返します。 その他のすべての SQL ステートメントでは、-1 を返します。

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

次のメソッドに示す、`WHERE`句、`SELECT`ステートメント。
コードが完全な SQL ステートメントを作成するために文字列を囲む引用符 (') などの予約文字をエスケープように注意する必要があります。

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

`ExecuteReader` メソッドは `SqliteDataReader` オブジェクトを返します。 加え、`Read`メソッドに示す例では、その他の便利なプロパティが含まれます。

-   **RowsAffected** &ndash;クエリによって影響を受ける行の数。

-   **HasRows** &ndash;行が返されたかどうか。


### <a name="executescalar"></a>EXECUTESCALAR

これを使用して`SELECT`(集計) などの 1 つの値を返すステートメント。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`メソッドの戻り値の型は`object`&ndash;データベース クエリによって結果をキャストする必要があります。 結果から整数である可能性があります、`COUNT`クエリまたは 1 つの列から文字列を`SELECT`クエリ。 これは他のさまざまなことに注意してください。`Execute`リーダー オブジェクトまたは影響を受ける行の数のカウントを返すメソッド。



## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android のデータのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
