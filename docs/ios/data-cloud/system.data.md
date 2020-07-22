---
title: Xamarin. iOS のシステムデータ
description: このドキュメントでは、system.string と Mono を使用して、Xamarin. iOS アプリケーションの SQLite データにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: 736d70aebcf861b5557d5f076a42ff0a3dcfc043
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569955"
---
# <a name="systemdata-in-xamarinios"></a>Xamarin. iOS のシステムデータ

ADO.NET プロバイダー[を含む、system.string のサポート](xref:System.Data)が追加さ8.10 れます。 `Mono.Data.Sqlite.dll` サポートには、次の[アセンブリ](~/cross-platform/internals/available-assemblies.md)の追加が含まれます。

- `System.Data.dll`
- `System.Data.Service.Client.dll`
- `System.Transactions.dll`
- `Mono.Data.Tds.dll`
- `Mono.Data.Sqlite.dll`

<a name="Example"></a>

## <a name="example"></a>例

次のプログラムでは、にデータベースを作成し `Documents/mydb.db3` ます。データベースが以前に存在していない場合は、サンプルデータが設定されます。 次に、データベースに対してクエリが作成され、出力がに書き込まれ `stderr` ます。

### <a name="add-references"></a>参照の追加

最初に、[**参照**] ノードを右クリックし、[**参照の編集**] を選択します。次に、[and] を選択します。 `System.Data` `Mono.Data.Sqlite`

[![](system.data-images/edit-references-sml.png "Adding new references")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>サンプル コード

次のコードは、テーブルを作成し、埋め込み SQL コマンドを使用して行を挿入する簡単な例を示しています。

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
> 上記のコードサンプルで説明したように、sql[インジェクション](https://en.wikipedia.org/wiki/SQL_injection)によってコードが脆弱になるため、sql コマンドに文字列を埋め込むことは不適切です。

### <a name="using-command-parameters"></a>コマンド パラメーターの使用

次のコードは、コマンドパラメーターを使用して、ユーザーが入力したテキストをデータベースに安全に挿入する方法を示しています (テキストに単一引用符などの特別な SQL 文字が含まれている場合でも)。

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

<a name="Missing_Functionality"></a>

## <a name="missing-functionality"></a>不足している機能

**システムデータ**と**Mono. Sqlite**の両方に、いくつかの機能がありません。

<a name="System.Data"></a>

### <a name="systemdata"></a>System.Data

System.string にない機能は次のもので構成さ**れ**ます。

- [システム CodeDom](xref:System.CodeDom)が必要なもの ( [TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
- XML 構成ファイルのサポート (例: [DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) ) のようになります。
- [DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (XML 構成ファイルのサポートに依存)
- [System.Data.OleDb](xref:System.Data.OleDb)
- [System.Data.Odbc](xref:System.Data.Odbc)
- `System.EnterpriseServices.dll`依存関係がから*削除*されたため `System.Data.dll` 、 [EnlistDistributedTransaction (ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*)メソッドが削除されました。

<a name="Mono.Data.Sqlite"></a>

### <a name="monodatasqlite"></a>Mono. Data. Sqlite

一方、 **Mono**は、ソースコードの変更はありませんが、代わりに Sqlite 3.5 をバインドしているため、多くの*実行時*の問題が発生する可能性があります。 `Mono.Data.Sqlite.dll` 一方、iOS 8 には SQLite 3.8.5 が付属しています。 ここでは、2つのバージョンの間で変更されたものがあるとします。

以前のバージョンの iOS には、次のバージョンの SQLite が付属しています。

- **iOS 7** -バージョン3.7.13。
- **iOS 6** -バージョン3.7.13。
- **iOS 5** -バージョン3.7.7。
- **iOS 4** -バージョン3.6.22。

最も一般的な問題は、データベーススキーマのクエリに関連しているように見えます。たとえば、特定のテーブルに存在する列が実行時に決定されます `Mono.Data.Sqlite.SqliteConnection.GetSchema` ( [DbConnection](xref:System.Data.Common.DbConnection.GetSchema)とのオーバーライド `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` ( [dbdatareader の](xref:System.Data.Common.DbDataReader.GetSchemaTable)オーバーライド) などです。 つまり、 [DataTable](xref:System.Data.DataTable)を使用しているものはほとんど動作しないように思えます。

<a name="Data_Binding"></a>

## <a name="data-binding"></a>データ バインディング

現時点では、データバインディングはサポートされていません。
