---
title: ADO.NET を使用する Android と
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 29e81afdf2c46cdefc68e2c2fae4e6e47999a346
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2018
---
# <a name="using-adonet-with-android"></a>ADO.NET を使用する Android と

Xamarin では、Android で使用できるは、使い慣れた ADO.NET に似た構文を使用して公開できます SQLite データベース用の組み込みサポートがあります。 など、SQLite、によって処理される SQL ステートメントを記述するこれらの Api を使用する必要があります`CREATE TABLE`、`INSERT`と`SELECT`ステートメントです。

## <a name="assembly-references"></a>アセンブリ参照

アクセスを追加する必要があります ADO.NET を使用して SQLite を使用する`System.Data`と`Mono.Data.Sqlite`次のように、Android プロジェクトへの参照します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Visual Studio での android 参照](using-adonet-images/image7.png "Visual Studio で Android を参照") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac) 

![Mac 用の Visual Studio での android 参照](using-adonet-images/image5.png "Mac 用 Visual Studio で Android を参照") 

-----


右クリック**参照 > 参照を編集しています.**必要なアセンブリを選択するには、をクリックします。

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite について

使用して、`Mono.Data.Sqlite.SqliteConnection`空のデータベース ファイルを作成するクラスをインスタンス化し、`SqliteCommand`オブジェクトをデータベースに対して SQL 命令の実行を使用できます。

**空のデータベースを作成する**&ndash;を呼び出す、`CreateFile`有効なメソッド (ie。 書き込み可能な) ファイルのパス。 このメソッドを呼び出す前にファイルが既に存在するかどうか、古いものの上に新しい (空) のデータベースを作成するそれ以外の場合と、古いファイル内のデータは失われますをチェックする必要があります。
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath`変数は、このドキュメントで既に説明したルールに従って決定する必要があります。

**データベース接続の作成** &ndash; SQLite データベース ファイルが作成された後は、データにアクセスする接続オブジェクトを作成することができます。 形式での接続文字列に、接続を構築した`Data Source=file_path`次に示すように、します。

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

前述のように、接続しないで再使用する別のスレッド間でします。 不明な場合に必要な接続を作成閉じたりすることです。 完了したらより多くの場合も必須すぎるを考慮します。

**データベース コマンドの実行の作成と**&ndash;接続を作成したらそれに対して任意の SQL コマンド実行できます。 次のコード、`CREATE TABLE`実行されるステートメントです。

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

データベースに対して直接 SQL を実行する場合は、既に存在するテーブルを作成しようとしてなどの無効な要求しないように、通常の予防措置を行う必要があります。 追跡データベースの構造が発生しないように、`SqliteException`など**SQLite エラー テーブル [アイテム] が既に存在する**です。

## <a name="basic-data-access"></a>基本的なデータ アクセス

*DataAccess_Basic* Android で実行されている場合、このドキュメントのサンプル コードは次のよう。

![Android の ADO.NET サンプル](using-adonet-images/image8.png "Android ADO.NET サンプル")

次のコードでは、SQLite の単純な操作を実行する方法について説明し、アプリケーションのメイン ウィンドウでテキストとしての結果を示します。

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

これらの操作が通常で表示されます、コード全体で複数の場所など、アプリケーションが初めて起動したときに、データベース ファイルとテーブルを作成して、個々 の画面で、アプリのデータ読み取りと書き込みを実行することがします。 次の例に分類されたこの例の 1 つのメソッド。

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

SQLite は、データに対して実行する任意の SQL コマンドを許可、実行するあらゆる`CREATE`、 `INSERT`、 `UPDATE`、 `DELETE`、または`SELECT`ステートメントか。 Sqlite の web サイトで SQLite でサポートされる SQL コマンドに関するを読み取ることができます。 次の 3 つの方法のいずれかを使用して SQL ステートメントを実行、`SqliteCommand`オブジェクト。

-   **ExecuteNonQuery** &ndash;通常テーブルの作成やデータの挿入に使用します。 一部の操作の戻り値は、影響を受ける行の数、それ以外の場合は-1。

-   **ExecuteReader** &ndash;行のコレクションとして返されるときに使用する`SqlDataReader`です。

-   **ExecuteScalar** &ndash; (集計など) の 1 つの値を取得します。


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`、 `UPDATE`、および`DELETE`ステートメントの影響を受ける行の数が返されます。 その他のすべての SQL ステートメントでは、-1 を返します。

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

メソッドを示します、`WHERE`句、`SELECT`ステートメントです。
コードが完全な SQL ステートメントを作成するため、文字列囲む引用符 (') などの予約文字をエスケープするために慎重を必要があります。

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

`ExecuteReader` メソッドは `SqliteDataReader` オブジェクトを返します。 加え、`Read`メソッドの例のように、その他の便利なプロパティが含まれます。

-   **RowsAffected** &ndash;クエリによって影響を受ける行の数。

-   **HasRows** &ndash;任意の行が返されたかどうか。


### <a name="executescalar"></a>EXECUTESCALAR

これを使用して`SELECT`(集計) などの 1 つの値を返すステートメントです。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`メソッドの戻り値の型は`object`&ndash;データベース クエリによって結果をキャストする必要があります。 結果の整数である可能性があります、`COUNT`クエリまたは 1 つの列から文字列`SELECT`クエリ。 これは、他のさまざまな`Execute`リーダー オブジェクトまたは影響を受ける行の数のカウントを返すメソッド。



## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android データ レシピ](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
