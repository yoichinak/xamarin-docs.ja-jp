---
title: Xamarin で ADO.NET を使用する
description: このドキュメントでは、ADO.NET をメソッドとして使用して、Xamarin アプリケーションで SQLite にアクセスする方法について説明します。 ここでは、アセンブリ参照、Mono. Data. Sqlite、および BasicDataAccess サンプルについて説明します。
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d72a9722a9d48ea52932e4fd6516c0712dbd693c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436257"
---
# <a name="using-adonet-with-xamarinios"></a>Xamarin で ADO.NET を使用する

Xamarin には、iOS で利用できる SQLite データベースのサポートが組み込まれており、使い慣れた ADO.NET のような構文を使用して公開されています。 これらの Api を使用するには、、、ステートメントなど、SQLite によって処理される SQL ステートメントを記述する必要があり `CREATE TABLE` `INSERT` `SELECT` ます。

## <a name="assembly-references"></a>アセンブリ参照

ADO.NET 経由で SQLite へのアクセスを使用するには `System.Data` 、次に示すように、iOS プロジェクトを追加して参照する必要があり `Mono.Data.Sqlite` ます (Visual Studio for Mac と Visual Studio のサンプルの場合)。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

 ![アセンブリ参照 Visual Studio for Mac](using-adonet-images/image4.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  ![Visual Studio でのアセンブリ参照](using-adonet-images/image6.png)

-----

[参照の **編集] >** 右クリックし、[参照の編集] をクリックして必要なアセンブリを選択します。

## <a name="about-monodatasqlite"></a>Mono. Data. Sqlite の概要

クラスを使用して、 `Mono.Data.Sqlite.SqliteConnection` 空のデータベースファイルを作成し、その後、 `SqliteCommand` データベースに対して SQL 命令を実行するために使用できるオブジェクトをインスタンス化します。

1. **空のデータベースの作成** - `CreateFile` 有効な (つまり、書き込み可能な) ファイルパスを使用してメソッドを呼び出します。 このメソッドを呼び出す前に、ファイルが既に存在するかどうかを確認する必要があります。そうしないと、新しい (空の) データベースが古いデータベースの先頭に作成され、古いファイルのデータは失われます。

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > 変数は、 `dbPath` このドキュメントで既に説明したルールに従って決定する必要があります。

2. **データベース接続の作成** -SQLite データベースファイルが作成された後、データにアクセスするための接続オブジェクトを作成できます。 接続は、次に示すように、という形式の接続文字列を使用して構築され `Data Source=file_path` ます。

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    既に説明したように、異なるスレッド間で接続を再利用することはできません。 不明な場合は、必要に応じて接続を作成し、完了したら閉じます。しかし、必要以上に多くのことを行うことに注意してください。

3. **データベースコマンドの作成と実行** -接続が確立されたら、それに対して任意の SQL コマンドを実行できます。 次のコードは、実行されている CREATE TABLE ステートメントを示しています。

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

データベースに対して SQL を直接実行する場合は、既に存在するテーブルを作成しようとするなど、無効な要求を行わないようにするための通常の予防措置を講じる必要があります。 データベースの構造を追跡して、"SQLite エラーテーブル [項目] が既に存在しています" などの SqliteException を発生させないようにします。

## <a name="basic-data-access"></a>基本的なデータアクセス

このドキュメントの *DataAccess_Basic* サンプルコードは、iOS で実行されている場合は次のようになります。

 ![iOS ADO.NET のサンプル](using-adonet-images/image9.png)

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
3. データのクエリ

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

SQLite ではデータに対して任意の SQL コマンドを実行できるため、任意の作成、挿入、更新、削除、選択などのステートメントを実行できます。 SQLite でサポートされている SQL コマンドについては、Sqlite の web サイトを参照してください。 SQL ステートメントは、SqliteCommand オブジェクトに対して次の3つのメソッドのいずれかを使用して実行されます。

- **ExecuteNonQuery** –通常、テーブルの作成またはデータの挿入に使用されます。 一部の操作の戻り値は、影響を受ける行の数を示します。それ以外の場合は-1 になります。
- **ExecuteReader** –行のコレクションがとして返される必要がある場合に使用  `SqlDataReader` します。
- **ExecuteScalar** –1つの値 (集計など) を取得します。

### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT、UPDATE、および DELETE ステートメントは、影響を受けた行数を返します。 その他の SQL ステートメントはすべて-1 を返します。

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

次のメソッドは、SELECT ステートメント内の WHERE 句を示しています。 コードは完全な SQL ステートメントを作成するため、文字列の前後の引用符 (') などの予約文字をエスケープする必要があります。

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

ExecuteReader メソッドは、SqliteDataReader オブジェクトを返します。 この例に示されている Read メソッドに加えて、次のような便利なプロパティがあります。

- **RowsAffected** –クエリの影響を受ける行の数。
- **Hasrows** –行が返されたかどうかを指定します。

### <a name="executescalar"></a>EXECUTESCALAR

この値は、1つの値 (集計など) を返す SELECT ステートメントに使用します。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`メソッドの戻り値の型は、 `object` データベースクエリに応じて結果をキャストする必要があります。 結果には、カウントクエリの整数、または1つの列の SELECT クエリの文字列を指定できます。 これは、リーダーオブジェクトを返す他の Execute メソッド、または影響を受ける行の数のカウントとは異なることに注意してください。

## <a name="microsoftdatasqlite"></a>Microsoft.Data.Sqlite

`Microsoft.Data.Sqlite` [NuGet からインストール](https://www.nuget.org/packages/Microsoft.Data.Sqlite)できる別のライブラリがあります。これは機能的にはと同じであり、 `Mono.Data.Sqlite` 同じ種類のクエリを許可します。

[2 つのライブラリ](/dotnet/standard/data/sqlite/compare)と[Xamarin 固有の詳細](/dotnet/standard/data/sqlite/xamarin)を比較しています。 Xamarin iOS アプリで最も重要なのは、初期化呼び出しを含める必要があることです。

```csharp
// required for Xamarin.iOS
SQLitePCL.Batteries_V2.Init();
```

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. フォームデータアクセス](~/xamarin-forms/data-cloud/data/databases.md)