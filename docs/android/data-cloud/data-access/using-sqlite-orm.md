---
title: Android での SQLite.NET の使用
description: SQLite.NET PCL NuGet ライブラリは、Xamarin Android アプリ用の単純なデータアクセス機構を提供します。
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/18/2018
ms.openlocfilehash: 65c8466e2649c6d48cf5651f25d14c073dbcf5e3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754433"
---
# <a name="using-sqlitenet-with-android"></a>Android での SQLite.NET の使用

Xamarin で推奨されている SQLite.NET ライブラリは、Android デバイス上のローカルの SQLite データベースにオブジェクトを簡単に格納および取得できる基本的な ORM です。 ORM は、SQL ステートメントを&ndash;記述せずにデータベースから "オブジェクト" を保存および取得できる API のオブジェクトリレーショナルマッピングを表します。

Xamarin アプリに SQLite.NET ライブラリを追加するには、次の NuGet パッケージをプロジェクトに追加します。

- **パッケージ名:** sqlite-net-pcl
- **作成者:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet パッケージ](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet パッケージ")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> さまざまな SQLite パッケージを使用できます。適切なものを選択してください (検索の結果が上位ではない可能性があります)。

SQLite.NET ライブラリを使用できるようになったら、次の3つの手順に従ってデータベースにアクセスします。

1. **Using ステートメントを追加**するデータアクセスが必要なC#ファイルに次のステートメントを追加します。 &ndash;

    ```csharp
    using SQLite;
    ```

2. **空のデータベースを作成する**&ndash;データベース参照を作成するには、ファイルパスを SQLiteConnection クラスコンストラクターに渡します。 ファイルが既に存在する&ndash;かどうかを確認する必要はありません。必要に応じて自動的に作成されます。それ以外の場合は、既存のデータベースファイルが開きます。 変数`dbPath`は、このドキュメントで既に説明したルールに従って決定する必要があります。

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3. **データの保存**&ndash; SQLiteConnection オブジェクトを作成したら、次のように CreateTable や Insert などのメソッドを呼び出して、データベースコマンドを実行します。

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4. **データの取得**&ndash;オブジェクト (またはオブジェクトの一覧) を取得するには、次の構文を使用します。

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>基本的なデータアクセスのサンプル

このドキュメントの*DataAccess_Basic*サンプルコードは、Android で実行すると次のようになります。 このコードは、単純な SQLite.NET 操作を実行し、結果をアプリケーションのメインウィンドウのテキストとして表示する方法を示しています。

**Android**

![Android SQLite.NET のサンプル](using-sqlite-orm-images/image3.png "Android SQLite.NET のサンプル")

次のコードサンプルは、SQLite.NET ライブラリを使用して、基になるデータベースアクセスをカプセル化するデータベース全体の相互作用を示しています。
次のように表示されます。

1. データベースファイルの作成

2. オブジェクトを作成して保存することによってデータを挿入する

3. データの照会

次の名前空間を含める必要があります。

```csharp
using SQLite; // from the github SQLite.cs class
```

最後の1つでは、SQLite をプロジェクトに追加しておく必要があります。 SQLite データベーステーブルは、CREATE TABLE コマンドではなくクラス ( `Stock`クラス) に属性を追加することによって定義されることに注意してください。

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

テーブル名パラメーターを指定せずに属性を使用すると、基になるデータベーステーブルの名前がクラス(この場合は"Stock")と同じになります。`[Table]` ORM データアクセスメソッドを使用するのではなく、データベースに対して SQL クエリを直接記述する場合は、実際のテーブル名が重要になります。 同様に、属性は省略可能であり、存在しない場合は、クラスのプロパティと同じ名前のテーブルに列が追加されます。 `[Column("_id")]`

## <a name="sqlite-attributes"></a>SQLite 属性

クラスに適用して、基になるデータベースへの格納方法を制御できる共通属性には、次のようなものがあります。

- **[PrimaryKey]** &ndash;この属性を整数プロパティに適用して、基になるテーブルの主キーにすることができます。 複合主キーはサポートされていません。

- **[自動インクリメント]** &ndash;この属性を指定すると、データベースに挿入される新しいオブジェクトごとに整数のプロパティの値が自動インクリメントされます。

- **[列 (名前)]** &ndash;省略可能`name`なパラメーターを指定すると、基になるデータベース列の名前 (プロパティと同じ) の既定値が上書きされます。

- **[テーブル (名前)]** &ndash;基になる SQLite テーブルに格納できるようにクラスをマークします。 省略可能な name パラメーターを指定すると、基になるデータベーステーブルの名前 (クラス名と同じ) の既定値が上書きされます。

- **[MaxLength (値)]** &ndash;データベースの挿入が試行されたときに、text プロパティの長さを制限します。 コードを使用するには、オブジェクトを挿入する前にこれを検証する必要があります。この属性は、データベースの挿入操作または更新操作を実行しようとしたときにのみ ' checked ' になります。

- **[無視]** &ndash; SQLite.NET によってこのプロパティが無視されます。
    これは、データベースに格納できない型を持つプロパティや、SQLite によって自動的に解決できないコレクションをモデル化するプロパティの場合に特に便利です。

- **[一意]** &ndash;基になるデータベース列の値が一意であることを確認します。

これらの属性のほとんどは省略可能で、SQLite はテーブル名と列名に既定値を使用します。 データに対して選択と削除のクエリを効率的に実行できるように、常に整数の主キーを指定する必要があります。

## <a name="more-complex-queries"></a>より複雑なクエリ

次のメソッド`SQLiteConnection`を使用すると、他のデータ操作を実行できます。

- **挿入**&ndash;新しいオブジェクトをデータベースに追加します。

- **Get&lt;T&gt;は、主キー** を使用してオブジェクトを取得しようとします。&ndash;

- **テーブル&lt;T&gt;は、テーブル内**のすべてのオブジェクトを返します&ndash; 。

- **削除**&ndash;主キーを使用してオブジェクトを削除します。

- **クエリ&lt;T&gt;では、複数**の行(オブジェクトとして)を返すSQLクエリを実行します。&ndash;

- **実行**SQL から行が返され`Query`ない場合 (INSERT、UPDATE、DELETE など) は、このメソッドを使用します。 &ndash;

### <a name="getting-an-object-by-the-primary-key"></a>主キーによるオブジェクトの取得

SQLite.Net は、主キーに基づいて1つのオブジェクトを取得する Get メソッドを提供します。

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq を使用したオブジェクトの選択

コレクション`IEnumerable<T>`を返すメソッドは、Linq を使用してテーブルの内容を照会または並べ替えることができます。 次のコードは、Linq を使用して、文字 "A" で始まるすべてのエントリをフィルターで除外する例を示しています。

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

> [!NOTE]
> SQL ステートメントを直接記述する場合は、クラスとその属性から生成された、データベース内のテーブルと列の名前に依存関係を作成します。 コード内でこれらの名前を変更する場合は、手動で記述した SQL ステートメントを必ず更新してください。

### <a name="deleting-an-object"></a>オブジェクトの削除

主キーは、次に示すように、行を削除するために使用されます。

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

を確認して`rowcount` 、影響を受けた行の数を確認できます (この場合は削除されます)。

## <a name="using-sqlitenet-with-multiple-threads"></a>複数のスレッドでの SQLite.NET の使用

SQLite は、3つの異なるスレッド処理モードをサポートしています。*シングルスレッド*、*マルチスレッド*、および*シリアル化*されます。 制限なく複数のスレッドからデータベースにアクセスする場合は、**シリアル化**されたスレッドモードを使用するように SQLite を構成できます。 アプリケーションの早い段階でこのモードを設定することが重要です (たとえば、 `OnCreate`メソッドの先頭で)。

スレッドモードを変更するには`SqliteConnection.SetConfig`、を呼び出します。 たとえば、次のコード行では、**シリアル化**モード用に SQLite を構成しています。

```csharp
using using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

SQLite の Android バージョンには、さらにいくつかの手順が必要な制限があります。 の呼び出し`SqliteConnection.SetConfig`によって`library used incorrectly`SQLite 例外 (など) が生成される場合は、次の回避策を使用する必要があります。

1. Api `sqlite3_shutdown`と apiをアプリで使用できるように、ネイティブlibsqlite.soライブラリにリンクし`sqlite3_initialize`ます。

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```

2. `OnCreate`メソッドの先頭で、sqlite をシャットダウンするために次のコードを追加し、**シリアル化**モード用に構成して、sqlite を再初期化します。

    ```csharp
    using using Mono.Data.Sqlite;
    ...
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

この回避策は、 `Mono.Data.Sqlite`ライブラリにも使用できます。 SQLite とマルチスレッドの詳細については、「 [sqlite と複数のスレッド](https://www.sqlite.org/threadsafe.html)」を参照してください。

## <a name="related-links"></a>関連リンク

- [このような場合の基本 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [詳細設定 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms データアクセス](~/xamarin-forms/data-cloud/data/databases.md)
