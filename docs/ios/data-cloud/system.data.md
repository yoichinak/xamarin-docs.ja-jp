---
title: System.Data
ms.topic: article
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 145a0692ed9761944eec4c7ece1f098a584f2d54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="systemdata"></a>System.Data

サポートを追加する Xamarin.iOS 8.10 [System.Data](https://developer.xamarin.com/api/namespace/System.Data/)など、 `Mono.Data.Sqlite.dll` ADO.NET プロバイダー。 サポートには、次の追加が含まれています[アセンブリ](~/cross-platform/internals/available-assemblies.md):。

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>例

次のプログラムでデータベースを作成する`Documents/mydb.db3`かどうか、データベースが以前存在しないこととは、サンプル データを設定します。 書き込まれた出力を含む、データベースは、照会`stderr`です。

### <a name="add-references"></a>参照を追加します。

最初を右クリックし、**参照**ノードを選択して**参照の編集.**を選択し、`System.Data`と`Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "新しい参照の追加")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>サンプル コード

次のコードは、テーブルを作成すると、埋め込まれた SQL コマンドを使用して行を挿入する簡単な例を示しています。

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
> コードを受けやすくなりますので、SQL コマンドに文字列を埋め込むには不適切な手法はのように上記のコード例で、 [SQL インジェクション](http://en.wikipedia.org/wiki/SQL_injection)です。


### <a name="using-command-parameters"></a>コマンド パラメーターを使用します。

次のコードは、コマンドのパラメーターを使用して (テキストには、単一アポストロフィなどの特別な SQL 文字が含まれています) 場合でも、データベースに安全にユーザーが入力したテキストを挿入する方法を示しています。

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

両方**System.Data**と**Mono.Data.Sqlite**一部の機能がありません。

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

機能が表示されない**System.Data.dll**で構成されます。

-  何が必要である[System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (例。 [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  XML 構成ファイル サポート (例。 [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (XML 構成ファイルのサポートによって異なります)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  `System.EnterpriseServices.dll`依存関係が*削除*から`System.Data.dll`の削除にその結果、 [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction))メソッドです。


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

一方、 **Mono.Data.Sqlite.dll**ソース コードの変更があっていませんが、代わりに、ホストの数を可能性があります*ランタイム*以降発行`Mono.Data.Sqlite.dll`SQLite 3.5 をバインドします。 iOS 8 は、その一方で、SQLite 3.8.5 に付属します。 あえてみることですが、いくつかの点は、2 つのバージョン間で変更されました。

IOS の古いバージョンは、SQLite の次のバージョンに付属します。

- **iOS 7** -バージョン 3.7.13 します。
- **iOS 6** -バージョン 3.7.13 します。
- **iOS 5** -バージョン 3.7.7 します。
- **iOS 4** -バージョン 3.6.22 します。

データベース スキーマ、クエリを実行に関連する最も一般的な問題が表示されるなど、特定のテーブルに列が存在する実行時に決定する例: `Mono.Data.Sqlite.SqliteConnection.GetSchema` (オーバーライド[DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) および`Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable`(オーバーライドする[DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/))。 いるものを使用して簡単に言えば、よう[DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/)動作する可能性はほとんどありません。

<a name="Data_Binding" />

## <a name="data-binding"></a>データ バインディング

この時点では、データ バインディングがサポートされていません。

