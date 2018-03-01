---
title: "ADO.NET を使用します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 45d7b3e844501f8aa97d75f2fc8af27a961290b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="using-adonet"></a>ADO.NET を使用します。

Xamarin では、iOS、使い慣れた ADO.NET に似た構文を使用して公開で利用可能な SQLite データベースの組み込みサポートがあります。 など、SQLite、によって処理される SQL ステートメントを記述するこれらの Api を使用する必要があります`CREATE TABLE`、`INSERT`と`SELECT`ステートメントです。

## <a name="assembly-references"></a>アセンブリ参照

アクセスを追加する必要があります ADO.NET を使用して SQLite を使用する`System.Data`と`Mono.Data.Sqlite`(Mac と Visual Studio の Visual Studio でのサンプル) の次のように、iOS プロジェクトへの参照します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Mac 用の Visual Studio でのアセンブリ参照")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Visual Studio でのアセンブリ参照")

-----

右クリック**参照 > 参照を編集しています.**必要なアセンブリを選択するには、をクリックします。

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite について

使用して、`Mono.Data.Sqlite.SqliteConnection`空のデータベース ファイルを作成するクラスをインスタンス化し、`SqliteCommand`オブジェクトをデータベースに対して SQL 命令の実行を使用できます。


1. **空のデータベースを作成する**-呼び出し、`CreateFile`有効なメソッド (ie。 書き込み可能な) ファイルのパス。 このメソッドを呼び出す前にファイルが既に存在するかどうか、古いものの上に新しい (空) のデータベースを作成するそれ以外の場合と、古いファイル内のデータは失われますを確認する必要があります。

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
> **注:** dbPath 変数は、このドキュメントで既に説明したルールに従って決定する必要があります。

2. **データベース接続の作成**SQLite データベース ファイルが作成された後、データにアクセスする接続オブジェクトを作成することができます。 形式での接続文字列に、接続を構築した`Data Source=file_path`次に示すように、します。

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    前述のように、接続しないで再使用する別のスレッド間でします。 不明な場合に必要な接続を作成閉じたりすることです。 完了したらより多くの場合も必須すぎるを考慮します。
    
3. **データベース コマンドの実行の作成と**に対して任意の SQL コマンドを実行して接続を作成したら - です。 次のコードでは、実行中の CREATE TABLE ステートメントを示します。

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

データベースに対して直接 SQL を実行する場合は、既に存在するテーブルを作成しようとしてなどの無効な要求しないように、通常の予防措置を行う必要があります。 追跡データベースの構造体"SQLite エラー テーブル [アイテム] は既に存在"など、SqliteException を発生させないようにします。

## <a name="basic-data-access"></a>基本的なデータ アクセス

*DataAccess_Basic* iOS を実行している場合、このドキュメントのサンプル コードは次のよう。

 ![](using-adonet-images/image9.png "iOS ADO.NET サンプル")

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

SQLite は、データに対して実行する任意の SQL コマンドを許可、ために、内容を作成、挿入、更新、削除または SELECT ステートメントかを実行できます。 Sqlite の web サイトで SQLite でサポートされる SQL コマンドに関するを読み取ることができます。 SqliteCommand オブジェクトの 3 つのメソッドのいずれかを使用して SQL ステートメントが実行されます。

-  **ExecuteNonQuery** – テーブルの作成やデータ挿入のために通常使用されます。 一部の操作の戻り値は、影響を受ける行の数、それ以外の場合は-1。
-  **ExecuteReader** –、行のコレクションとして返されるときに使用する`SqlDataReader`です。
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

次のメソッドは、SELECT ステートメントで WHERE 句を示しています。 コードが完全な SQL ステートメントを作成するため、文字列囲む引用符 (') などの予約文字をエスケープするために慎重を必要があります。

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

ExecuteReader メソッドは、SqliteDataReader オブジェクトを返します。 例に示すように、読み取りメソッドだけでなく他の便利なプロパティは次のとおりです。

-  **RowsAffected** – クエリによって影響を受ける行の数。
-  **HasRows** – 任意の行が返されたかどうか。


### <a name="executescalar"></a>EXECUTESCALAR

これは、(集計) などの 1 つの値を返す SELECT ステートメントを使用します。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`メソッドの戻り値の型は`object`– データベース クエリによって結果をキャストする必要があります。 COUNT クエリからの整数または文字列を 1 つの列の選択クエリおそれがあります。 リーダー オブジェクトまたは影響を受ける行の数のカウントを返す他の Execute メソッドをさまざまなことに注意してください。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
