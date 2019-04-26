---
title: Xamarin.iOS で System.Data
description: このドキュメントでは、System.Data、および Mono.Data.Sqlite.dll を使用して Xamarin.iOS アプリケーションでの SQLite のデータにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: e6df2d9d45eb2f898bb3c4957ec7960956a184e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61240220"
---
# <a name="systemdata-in-xamarinios"></a>Xamarin.iOS で System.Data

Xamarin.iOS 8.10 のサポートが追加[System.Data](xref:System.Data)など、 `Mono.Data.Sqlite.dll` ADO.NET プロバイダー。 サポートには、次の追加が含まれています[アセンブリ](~/cross-platform/internals/available-assemblies.md):。

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>例

次のプログラムでデータベースを作成する`Documents/mydb.db3`、し、データベースが以前存在しないかどうかにはサンプル データが設定されます。 書き込まれた出力で、データベースは、照会`stderr`します。

### <a name="add-references"></a>参照を追加します。

最初に、右クリックして、**参照**ノード選択**参照の編集.** 選び`System.Data`と`Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "新しい参照の追加")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>サンプル コード

次のコードでは、テーブルを作成すると、埋め込まれた SQL コマンドを使用して行を挿入する単純な例を示します。

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> コードを受けやすくために、SQL コマンド文字列を埋め込むには不適切な手法は前述のように上記のコード サンプル、 [SQL インジェクション](https://en.wikipedia.org/wiki/SQL_injection)します。


### <a name="using-command-parameters"></a>コマンド パラメーターの使用

次のコードでは、(テキストには、単一アポストロフィなどの特別な SQL 文字が含まれている) 場合でも、データベースに安全にユーザーが入力したテキストを挿入するコマンドのパラメーターを使用する方法を示します。

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>不足している機能

両方**System.Data**と**Mono.Data.Sqlite**機能の一部が欠落しています。

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

機能が表示されない**System.Data.dll**で構成されます。

-  ものが必要な[System.CodeDom](xref:System.CodeDom) (例。 [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  XML 構成ファイルのサポートの (例。 [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (XML 構成ファイルのサポートによって異なります)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  `System.EnterpriseServices.dll`依存関係が*削除*から`System.Data.dll`、削除の結果として得られる、 [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*)メソッド。


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

一方、 **Mono.Data.Sqlite.dll**ソース コードの変更があっていませんが、代わりに、ホストの数を可能性があります*ランタイム*から発行`Mono.Data.Sqlite.dll`SQLite 3.5 をバインドします。 iOS 8 には SQLite 3.8.5 一方で、含まれています。 これをあえてと事項は、2 つのバージョン間で変更されました。

IOS の以前のバージョンは、次のバージョンの SQLite に付属します。

- **iOS 7** -3.7.13 のバージョン。
- **iOS 6** -3.7.13 のバージョン。
- **iOS 5** -3.7.7 のバージョン。
- **iOS 4** -3.6.22 のバージョン。

データベース スキーマのクエリに関連する最も一般的な問題が表示されるなど、特定のテーブルに列が存在する実行時に決定する例: `Mono.Data.Sqlite.SqliteConnection.GetSchema` (オーバーライド[DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema)と`Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable`(オーバーライド[DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable)します。 よっているものを使用して簡単に言えば、 [DataTable](xref:System.Data.DataTable)動作する可能性はほとんどありません。

<a name="Data_Binding" />

## <a name="data-binding"></a>データ バインディング

この時点では、データ バインディングはサポートされていません。

