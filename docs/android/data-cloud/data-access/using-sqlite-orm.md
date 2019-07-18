---
title: Android での SQLite.NET の使用
description: SQLite.NET PCL NuGet ライブラリでは、Xamarin.Android アプリの単純なデータ アクセス メカニズムを提供します。
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/18/2018
ms.openlocfilehash: 27d2d68243c5b25a3ecfc0ce333dca183ba090ec
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649536"
---
# <a name="using-sqlitenet-with-android"></a>Android での SQLite.NET の使用

Xamarin で推奨される SQLite.NET ライブラリとは、簡単に格納および Android デバイス上のローカルの SQLite データベース内のオブジェクトを取得できる非常に基本的な ORM です。 ORM のオブジェクト リレーショナル マッピング略&ndash;API を保存し、「オブジェクト」を SQL ステートメントを記述することがなく、データベースから取得することができます。

SQLite.NET ライブラリを Xamarin アプリに含めるには、プロジェクトに次の NuGet パッケージを追加します。

- **パッケージ名:** sqlite-net-pcl
- **作成者:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet パッケージ](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet パッケージ")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> 多くの異なる SQLite パッケージが使用可能な – (検索で上位の検索結果は可能性がありますないあります) のいずれかを適切に選択してください。

SQLite.NET ライブラリを使用すると、これらを使用してデータベースにアクセスする 3 つの手順に従います。

1.  **使用して、追加ステートメント**&ndash;次のステートメントを追加、C#ファイル データ アクセスが必要な場合。

    ```csharp
    using SQLite;
    ```

2.  **空のデータベース作成**&ndash;ファイルのパスを SQLiteConnection クラスのコンス トラクターに渡すことによってデータベースの参照を作成できます。 ファイルが既に存在するかどうかを確認する必要はありません&ndash;必要な場合は作成自動的には、それ以外の場合、既存のデータベース ファイルが開かれます。 `dbPath`変数は、このドキュメントで前述したルールに従って決定する必要があります。

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **データの保存** &ndash; CreateTable やこのような挿入など、そのメソッドを呼び出すことによって、データベース コマンドが実行される SQLiteConnection オブジェクトを作成するとします。

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **データの取得**&ndash;を取得するオブジェクト (またはオブジェクトの一覧) で次の構文が使用します。

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>基本的なデータ アクセス サンプル

*DataAccess_Basic* Android で実行されているときに、このドキュメントのサンプル コードがこのような検索します。 このコードでは、SQLite.NET の単純な操作を実行する方法を示していて、アプリケーションのメイン ウィンドウでテキストとしての結果が表示されます。


**Android**

![Android の SQLite.NET サンプル](using-sqlite-orm-images/image3.png "Android SQLite.NET サンプル")

次のコードを基になるデータベースへのアクセスをカプセル化する SQLite.NET ライブラリを使用して、データベース全体の相互作用を示しています。
表示されます。

1.  データベース ファイルの作成

2.  オブジェクトを作成し、それらを保存して一部のデータを挿入する.

3.  データの照会

これらの名前空間を含める必要があります。

```csharp
using SQLite; // from the github SQLite.cs class
```

最後の 1 つは、プロジェクトに SQLite を追加することが必要です。 属性クラスに追加して、SQLite データベースのテーブルが定義されていることに注意してください (、`Stock`クラス) CREATE TABLE コマンドではなく。

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

使用して、`[Table]`せず、基になるデータベース テーブル (この例では、"Stock") では、クラスと同じ名前を指定すると、テーブル名のパラメーターを指定する属性します。 なくする場合、データベースに対して直接 SQL クエリを記述、ORM データ アクセス メソッドを使用して、実際のテーブル名が重要です。 同様に、`[Column("_id")]`属性は省略可能で、かどうか、列の存在しない追加クラスのプロパティと同じ名前のテーブルにします。

## <a name="sqlite-attributes"></a>SQLite 属性

基になるデータベースの格納方法を制御するクラスに適用できる共通の属性は次のとおりです。

-   **[主キー]** &ndash;強制的に基になるテーブルの主キーに整数のプロパティにこの属性を適用できます。 複合主キーがサポートされていません。

-   **[増分]** &ndash;をデータベースに挿入された新しい各オブジェクトの自動インクリメント整数プロパティの値により、この属性

-   **[Column(name)]** &ndash;省略可能な指定`name`パラメーターは、基になるデータベース列の名前 (プロパティと同じ) の既定値をオーバーライドします。

-   **[Table(name)]** &ndash; SQLite の基盤のテーブルに格納することとクラスをマークします。 省略可能な名前のパラメーターを指定すると、基になるデータベース テーブルの名前 (クラス名と同じ) の既定値が上書きされます。

-   **[MaxLength(value)]** &ndash;データベース挿入が試行されたときに、text プロパティの長さを制限します。 コードを使用すると、この属性は '' ときにだけチェック、データベースの insert または update 操作が試行されたオブジェクトを挿入する前にこれ検証する必要があります。

-   **[無視]** &ndash;をこのプロパティを無視すると、SQLite.NET します。
    これは、データベースに格納できない型を持つプロパティまたは SQLite では自動的に解決できないコレクションをモデル化するプロパティに特に便利です。

-   **[Unique]** &ndash;により、基になるデータベース列の値が一意であります。


これらの属性のほとんどは省略可能な SQLite は、テーブルおよび列名の既定値を使用します。 データでクエリの選択と削除を効率的に実行できるように常に整数の主キーを指定する必要があります。

## <a name="more-complex-queries"></a>複雑なクエリ

次のメソッドで`SQLiteConnection`他のデータ操作を実行するために使用できます。

-   **挿入**&ndash;データベースに新しいオブジェクトを追加します。

-   **取得&lt;T&gt;**  &ndash;主キーを使用して、オブジェクトの取得を試みます。

-   **テーブル&lt;T&gt;**  &ndash;テーブルのすべてのオブジェクトを返します。

-   **削除**&ndash;の主キーを使用してオブジェクトを削除します。

-   **クエリ&lt;T&gt;**  &ndash; (オブジェクト) としての行の数を返す SQL クエリを実行します。

-   **実行**&ndash;このメソッドを使用して (および not `Query`) (INSERT、UPDATE および DELETE の命令) などの SQL から行を予定がない場合。


### <a name="getting-an-object-by-the-primary-key"></a>主キーによって、オブジェクトの取得

SQLite.Net では、1 つの主キーに基づいてオブジェクトを取得する Get メソッドを提供します。

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq を使用してオブジェクトを選択します。

コレクションを返すメソッドをサポートして`IEnumerable<T>`クエリまたはテーブルの内容を並べ替える Linq を使用できるようにします。 次のコードでは、Linq を使用して、文字"A"で始まるすべてのエントリをフィルター処理する例を示します。

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL を使用してオブジェクトを選択します。

SQLite.Net には、データにオブジェクト ベースのアクセスできるように、にもかかわらずはことがありますにより、Linq (またはパフォーマンスを向上させる必要があります) よりもさらに複雑なクエリを実行する必要があります。 次に示すように、クエリ メソッドでは、SQL コマンドを使用できます。

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> SQL ステートメントを直接作成するときは、テーブルと、クラスとその属性から生成した、データベース内の列の名前に依存関係を作成します。 コードでこれらの名前を変更する場合は、手動で書き込まれた SQL ステートメントの更新を忘れないでください。

### <a name="deleting-an-object"></a>オブジェクトを削除します。

次のように、主キーは、行を削除する使用します。

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

チェックすることができます、 `rowcount` (ここで削除された) 行の数が影響を受けたことを確認します。

## <a name="using-sqlitenet-with-multiple-threads"></a>複数のスレッドで SQLite.NET の使用

SQLite は、次の 3 つの異なるスレッド処理モードをサポートしています。*シングル スレッド*、*マルチ スレッド*、および*シリアル化*します。 制限を適用せずに複数のスレッドからデータベースにアクセスする場合は、SQLite を使用するを構成することができます、**シリアル化**モードのスレッドを処理します。 アプリケーションの初期段階でこのモードを設定することが重要 (などの先頭に、`OnCreate`メソッド)。

スレッド処理モードを変更するには、呼び出す`SqliteConnection.SetConfig`します。 たとえば、SQLite for の構成のコード行**シリアル化**モード。

```csharp
using using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Android のバージョンの SQLite では、さらに、いくつかの手順が必要な制限があります。 場合への呼び出し`SqliteConnection.SetConfig`など SQLite 例外を生成`library used incorrectly`、次の回避策を使用する必要があります。

1.  ネイティブへのリンク**libsqlite.so**ライブラリように、`sqlite3_shutdown`と`sqlite3_initialize`Api は、アプリに提供されます。

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```

2.  先頭に、`OnCreate`メソッド、SQLite をシャット ダウンに次のコードを追加、構成の**シリアル化**モード、および SQLite を再初期化します。

    ```csharp
    using using Mono.Data.Sqlite;
    ...
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

この回避策にも適して、`Mono.Data.Sqlite`ライブラリ。 SQLite とマルチ スレッドの詳細については、次を参照してください。 [SQLite と複数のスレッド](https://www.sqlite.org/threadsafe.html)します。

## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/data-cloud/data/databases.md)
