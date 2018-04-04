---
title: CursorAdapters を使用します。
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: 20311cc50c87638391d8b078c405bc61baceb373
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="using-cursoradapters"></a>CursorAdapters を使用します。


## <a name="overview"></a>概要

Android には、SQLite データベース クエリからデータを表示するには、具体的には、アダプター クラスが用意されています。

 **SimpleCursorAdapter** -と同様に、`ArrayAdapter`サブクラス化せずに使用できるためです。 単に、コンス トラクターで、必須パラメーター (、カーソルとレイアウト情報など) を提供し、割り当てる、`ListView`です。

 **CursorAdapter** – 値とする必要がある詳細に制御データのバインドから継承できる基底クラスのレイアウト コントロール (たとえば、コントロールの表示/非表示またはプロパティの変更)。

カーソルのアダプターは、SQLite に格納されているデータの長い一覧をスクロールするパフォーマンスの高い方法を提供します。 コンシューマー コードでの SQL クエリを定義する必要があります、`Cursor`オブジェクトを作成し、各行のビューを設定する方法を説明します。


## <a name="creating-an-sqlite-database"></a>SQLite データベースを作成します。

カーソルのアダプターを示すために単純な SQLite データベースの実装が必要です。 内のコード**SimpleCursorTableAdapter/VegetableDatabase.cs**コードと、テーブルを作成し、一部のデータを設定する SQL が含まれています。
完全な`VegetableDatabase`クラスを示します。

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
   public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
   public override void OnCreate(SQLiteDatabase db)
   {
       db.ExecSQL(create_table_sql);
       // seed with data
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
   }
   public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

`VegetableDatabase`でクラスをインスタンス化は、`OnCreate`のメソッド、`HomeScreen`アクティビティ。 `SQLiteOpenHelper`基底クラスが、データベース ファイルの設定を管理し、確実に内の SQL の`OnCreate`メソッドは一度だけ実行します。 このクラスは、の次の 2 つの例では使用`SimpleCursorAdapter`と`CursorAdapter`です。

カーソル クエリ*必要があります*整数型の列がある`_id`の`CursorAdapter`動作をします。 かどうか、基になるテーブルが名前付き整数型の列`_id`で一意の整数を別の列の別名を使用して、`RawQuery`カーソルを構成します。 参照してください、 [Android docs](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)についてさらにします。


### <a name="creating-the-cursor"></a>カーソルを作成します。

例を使用して、`RawQuery`に SQL クエリを有効にする、`Cursor`オブジェクト。 カーソルから返される列リストでは、カーソルのアダプターでの表示に使用できるデータ列を定義します。 データベースを作成するコード、 **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate`メソッドを次に示します。

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

そのコードを呼び出す`StartManagingCursor`呼び出す必要もあります`StopManagingCursor`です。 例を使用して`OnCreate`を開始して`OnDestroy`カーソルを閉じます。 `OnDestroy`メソッドには、このコードが含まれています。

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

いずれかの利用できるアプリケーションでは、SQLite データベースが使用されが示すようにカーソル オブジェクトを作成、`SimpleCursorAdapter`またはそのサブクラスの`CusorAdapter`内の行を表示する、`ListView`です。


## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter を使用します。

`SimpleCursorAdapter` 同様には、 `ArrayAdapter`SQLite で使用する、特殊なです。 これはサブクラス化を必要と – だけオブジェクトを作成するときにいくつかの単純なパラメーターを設定されず、単に割り当てる、`ListView`の`Adapter`プロパティです。

SimpleCursorAdapter コンス トラクターのパラメーターは次のとおりです。

 **コンテキスト**–、含まれるアクティビティへの参照。

 **レイアウト**– 使用する行の表示のリソース ID です。

 **ICursor** – SQLite クエリを表示するデータを含むカーソル。

 **から**文字列配列 – カーソル内の列の名前に対応する文字列の配列。

 **整数配列へ** – 行レイアウト内のコントロールに対応するレイアウト Id の配列。 指定された列の値、`from`配列は同じインデックスには、この配列で指定された ControlID にバインドされます。

`from`と`to`ビューでデータ ソースからレイアウト コントロールへのマッピングを形成するため、配列は同じ数のエントリをいる必要があります。

**SimpleCursorTableAdapter/HomeScreen.cs**をコード ワイヤのサンプル、`SimpleCursorAdapter`次のようにします。

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` SQLite のデータを表示するための高速で単純なは、`ListView`です。 大きな限界は、あるメッセージがコントロールを表示する列の値のみバインドできます (たとえば、コントロールの表示/非表示またはプロパティを変更する) の行のレイアウトの他の側面を変更することはできません。


## <a name="subclassing-cursoradapter"></a>CursorAdapter をサブクラス化

A`CursorAdapter`サブクラスは、パフォーマンス上の利点として、 `SimpleCursorAdapter` SQLite からデータを表示することができますを作成し、それぞれのビューの行のレイアウトを完全に制御をします。 `CursorAdapter`実装とは大きく異なりますサブクラス化`BaseAdapter`オーバーライドしないため`GetView`、 `GetItemId`、`Count`または`this[]`インデクサーです。

作業中の指定 SQLite データベースを作成する 2 つのメソッドをオーバーライドするだけでかまいません、`CursorAdapter`サブクラス。

- **BindView** – ビューを指定するには、指定されたカーソルにデータを表示するように更新します。

- **NewView** – ときに呼び出されます、`ListView`を表示する新しいビューが必要です。 `CursorAdapter`ビューを再利用の注意 (とは異なり、`GetView`標準アダプター上のメソッド)。

前の例のアダプター サブクラスは、– 現在の項目を取得して行の数を返すメソッドを持つ、`CursorAdapter`自体、カーソルからその情報を収集できるためこれらのメソッドは必要ありません。 作成およびこれら 2 つのメソッドには、各ビューの設定を分割して、`CursorAdapter`ビューを再使用を強制します。 これはこれに対し、通常のアダプターを無視する可能性がある、`convertView`のパラメーター、`BaseAdapter.GetView`メソッドです。


### <a name="implementing-the-cursoradapter"></a>CursorAdapter を実装します。

内のコード**CursorTableAdapter/HomeScreenCursorAdapter.cs**が含まれています、`CursorAdapter`サブクラスです。 アクセスできるように、コンス トラクターに渡されるコンテキストの参照を格納、`LayoutInflater`で、`NewView`メソッドです。 完全なクラスは、次のようになります。

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```


### <a name="assigning-the-cursoradapter"></a>CursorAdapter を割り当てる

`Activity`を表示する、 `ListView`、カーソルを作成し、`CursorAdapter`し、リスト ビューに割り当てます。

この操作を実行するコードを**CursorTableAdapter/HomeScreen.cs** `OnCreate`メソッドを次に示します。

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy`メソッドが含まれています、`StopManagingCursor`前に説明したメソッドの呼び出しです。



## <a name="related-links"></a>関連リンク

- [SimpleCursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/CursorTableAdapter/)
