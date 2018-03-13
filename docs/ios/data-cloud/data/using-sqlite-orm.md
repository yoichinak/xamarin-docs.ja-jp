---
title: "SQLite.NET を使用します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2018
ms.openlocfilehash: 2b29826f1a7acc6bb8dcd0594c69a5cb7ff6db42
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="using-sqlitenet"></a>SQLite.NET を使用します。

Xamarin で推奨される SQLite.NET ライブラリとは、格納および iOS デバイス上のローカル SQLite データベース内のオブジェクトを取得できる基本的な ORM です。
オブジェクト リレーショナル マッピング用 -保存し、「オブジェクト」を SQL ステートメントを記述することがなく、データベースから取得できる API ORM を意味します。

<a name="Usage"/>

## <a name="usage"></a>使用法

追加、 [SQLite.net PCL NuGet パッケージ](https://www.nuget.org/packages/sqlite-net-pcl/)、- プロジェクトにプラットフォームが iOS、Android、Windows などのさまざまなをサポートします。

  [![](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet パッケージ")](using-sqlite-orm-images/image1a.png#lightbox)

SQLite.NET ライブラリを使用した後は、データベースへのアクセスに使用するこれら 3 つの手順に従います。


1. **使用して、追加ステートメント**-データ アクセスが必要な c# のファイルに次のステートメントを追加します。

        using SQLite;

1. **空のデータベースを作成する**-SQLiteConnection クラス コンス トラクターは、ファイルのパスを渡すことによってデータベースの参照を作成することができます。 かどうか必要に応じて、それ以外の場合、既存のデータベース ファイルが開きますが自動的に作成、ファイルが既に存在する場合を確認する必要はありません。

        var db = new SQLiteConnection (dbPath);

    DbPath 変数は、このドキュメントで既に説明したルールに基づいて決定してください。

1. **データを保存**- SQLiteConnection オブジェクト、コマンドが次のように Insert CreateTable など、そのメソッドを呼び出すことによって実行されたデータベースを作成した後。

        db.CreateTable<Stock> ();
        db.Insert (newStock); // after creating the newStock object

1. **データの取得**: 取得するオブジェクト (またはオブジェクトの一覧) は、次の構文を使用します。

        var stock = db.Get<Stock>(5); // primary key id of 5
        var stockList = db.Table<Stock>();

## <a name="basic-data-access-sample"></a>基本的なデータ アクセス サンプル

*DataAccess_Basic* iOS を実行している場合、このドキュメントのサンプル コードは次のようです。 コードでは、単純な SQLite.NET 操作を実行する方法を示していて、アプリケーションのメイン ウィンドウでテキストとしての結果を示します。

**iOS**

 ![](using-sqlite-orm-images/image2.png "iOS SQLite.NET サンプル")

次のコードは、基になるデータベースへのアクセスをカプセル化する SQLite.NET ライブラリを使用してデータベース全体のやり取りを示しています。 これを示しています。

1.  データベース ファイルの作成
1.  オブジェクトを作成して、それらを保存して一部のデータを挿入する.
1.  データの照会


これらの名前空間を含める必要があります。

```csharp
using SQLite; // from the github SQLite.cs class
```

強調表示されているプロジェクトに SQLite を追加する必要があります[ここ](#Usage)です。 SQLite データベース テーブルがクラスに属性を追加することによって定義されることに注意してください (、`Stock`クラス) CREATE TABLE コマンドではなくです。

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

使用して、`[Table]`テーブル名のパラメーターと、基になるデータベースは、テーブルに同じ名前のクラス (ここでは、"Stock") を指定せずに属性します。 実際のテーブル名が使用する場合、データベースに対して直接 SQL クエリを記述ではなく、ORM データ アクセス手法重要です。 同様に、`[Column("_id")]`属性は省略可能なされかどうか不在であれば、列が追加されます、クラスのプロパティと同じ名前のテーブルにします。

## <a name="sqlite-attributes"></a>SQLite 属性

基になるデータベースに格納する方法を制御するクラスに適用できる共通の属性は次のとおりです。

-  **[主キー]** – この属性を強制する、基になるテーブルの主キーにするには整数のプロパティに適用できます。 複合主キーはサポートされていません。
-  **[自動増分]** – この属性は、データベースに挿入された新しいオブジェクトごとに自動的にインクリメントする整数のプロパティの値になります
-  **[Column(name)]** – 省略可能な指定`name`パラメーターには、基になるデータベース列の名前 (プロパティと同じ) の既定値がよりも優先されます。
-  **[Table(name)]** – SQLite の基になるテーブルに格納することとクラスをマークします。 省略可能な name パラメーターを指定すると、基になるデータベース テーブルの名前 (これは、クラス名と同じ) の既定値が上書きされます。
-  **[MaxLength(value)]** – データベースの挿入が試行されたときに、テキスト プロパティの長さを制限します。 コードを実行と、オブジェクトを挿入するようにこの属性は '' ときにのみチェック、データベースの挿入または更新操作が試行する前にこの検証必要があります。
-  **[無視]** – このプロパティを無視すると、SQLite.NET です。 これは、データベースに格納できない型を持つプロパティまたはプロパティが自動的に解決できないモデルのコレクションである SQLite に特に便利です。
-  **[Unique]** – 基になるデータベース列の値が一意であることを確認します。


これらの属性のほとんどは、省略可能な SQLite は、テーブルおよび列名の既定値を使用します。 データにクエリの選択と削除を効率的に処理できるように、整数型の主キーを必ず指定する必要があります。

## <a name="more-complex-queries"></a>複雑なクエリ

次のメソッドで`SQLiteConnection`他のデータ操作を実行するために使用できます。

-  **挿入**– データベースに新しいオブジェクトを追加します。
-  **取得<T>** – プライマリ キーを使用してオブジェクトを取得しようとしています。
-  **テーブル<T>** – テーブル内のすべてのオブジェクトを返します。
-  **削除**– その主キーを使用してオブジェクトを削除します。
-  **クエリ<T>** -(オブジェクト) として行の数を返す SQL クエリを実行します。
-  **実行**– このメソッドを使用して (および not `Query` ) (INSERT、UPDATE および DELETE の命令) などの SQL からの行を必要がない場合。


### <a name="getting-an-object-by-the-primary-key"></a>主キーによって、オブジェクトの取得

SQLite.Net は、その主キーに基づく 1 つのオブジェクトを取得する Get メソッドを提供します。

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq を使用してオブジェクトを選択します。

コレクションを返すメソッドをサポートして IEnumerable<T>をクエリまたはテーブルの内容を並べ替える Linq を使用できるようにします。 次のコードでは、Linq を使用して、文字"A"で始まるすべてのエントリを除外する例を示します。

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL を使用してオブジェクトを選択します。

SQLite.Net には、オブジェクト ベース、データにアクセスを提供できます、でもは場合もあります Linq で (またはパフォーマンスが向上する必要があります) よりも複雑なクエリを実行する必要があります。 次のようにクエリ メソッドを使用して SQL コマンドを使用できます。

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> **注**: テーブルと、クラスとその属性から生成されて、データベース内の列の名前に依存関係を作成して直接 SQL ステートメントを記述する場合。 コードでそれらの名前を変更する場合、手動で記述された SQL ステートメントを更新することを忘れないでください。

### <a name="deleting-an-object"></a>オブジェクトを削除します。

次のように、主キーは、行を削除する使用します。

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

チェックすることができます、 `rowcount` (ここでは削除された) 行の数が影響を確認します。

## <a name="using-sqlitenet-with-multiple-threads"></a>複数のスレッドで SQLite.NET の使用

SQLite は、次の 3 つの異なるスレッド モードをサポートしています:*シングル スレッド*、*マルチ スレッド*、および*シリアル化*です。 使用する SQLite を構成するには制限がまったくない複数のスレッドからデータベースにアクセスする場合、**シリアル化**モードをスレッドです。 アプリケーションの早い段階でこのモードを設定することが重要 (の先頭の位置などで、`OnCreate`メソッド)。

スレッド処理モードを変更するには、呼び出す`SqliteConnection.SetConfig`です。 たとえば、次のコード行は構成に対して SQLite**シリアル化**モード。

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS データ レシピ](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms データ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
