---
title: CursorAdapters の使用
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/25/2017
ms.openlocfilehash: 81ce3a31a151c8783e09bc42f2dcc2c64168a2e4
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457576"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Xamarin Android でのカーソルの使用

Android には、SQLite データベースクエリのデータを表示するためのアダプタークラスが用意されています。

 **SimpleCursorAdapter** –サブクラス化 `ArrayAdapter` せずに使用できるため、に似ています。 コンストラクターで必要なパラメーター (カーソルやレイアウト情報など) を指定し、に割り当てるだけです `ListView` 。

 **カーソル** : レイアウトコントロールへのデータ値のバインドをより詳細に制御する必要がある場合に継承できる基本クラス (コントロールの表示/非表示、プロパティの変更など)。

カーソルアダプターは、SQLite に格納されているデータの長い一覧をスクロールするための高パフォーマンスな方法を提供します。 コンシューマーコードでは、オブジェクトで SQL クエリを定義 `Cursor` し、各行のビューを作成および設定する方法を記述する必要があります。

## <a name="creating-an-sqlite-database"></a>SQLite データベースの作成

カーソルアダプターを示すには、簡単な SQLite データベース実装が必要です。 **SimpleCursorTableAdapter/VegetableDatabase**のコードには、テーブルを作成してデータを設定するためのコードと SQL が含まれています。
完全な `VegetableDatabase` クラスを次に示します。

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

クラスは、 `VegetableDatabase` アクティビティのメソッドでインスタンス化され `OnCreate` `HomeScreen` ます。 `SQLiteOpenHelper`基本クラスは、データベースファイルのセットアップを管理し、そのメソッドの SQL が `OnCreate` 1 回だけ実行されるようにします。 このクラスは、との次の2つの例で使用し `SimpleCursorAdapter` `CursorAdapter` ます。

を機能させるには、カーソルクエリに整数型の列が *必要* `_id` `CursorAdapter` です。 基になるテーブルにという名前の整数型の列がない場合は `_id` 、カーソルを構成する内の別の一意の整数に対して列の別名を使用し `RawQuery` ます。 詳細については、 [Android のドキュメント](xref:Android.Widget.CursorAdapter) を参照してください。

### <a name="creating-the-cursor"></a>カーソルの作成

この例では、を使用して、 `RawQuery` SQL クエリをオブジェクトに変換し `Cursor` ます。 カーソルから返される列リストによって、カーソルアダプターに表示できるデータ列が定義されます。 **SimpleCursorTableAdapter/HomeScreen**メソッドでデータベースを作成するコード `OnCreate` を次に示します。

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

を呼び出すすべてのコード `StartManagingCursor` は、もを呼び出す必要があり `StopManagingCursor` ます。 この例では、を使用してを開始し、 `OnCreate` `OnDestroy` カーソルを閉じます。 メソッドには次の `OnDestroy` コードが含まれています。

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

アプリケーションで SQLite データベースを使用可能にし、そのオブジェクトを表示されているように作成すると、 `SimpleCursorAdapter` のまたはサブクラスを使用して `CusorAdapter` 、に行を表示でき `ListView` ます。

## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter の使用

`SimpleCursorAdapter` はに似てい `ArrayAdapter` ますが、SQLite での使用に特化しています。 サブクラス化は必要ありません。オブジェクトを作成するときに単純なパラメーターをいくつか設定し、そのオブジェクトをのプロパティに割り当てるだけ `ListView` `Adapter` です。

SimpleCursorAdapter コンストラクターのパラメーターは次のとおりです。

 **Context** –格納しているアクティビティへの参照。

 **Layout** –使用する行ビューのリソース ID。

 **ICursor** –表示するデータの SQLite クエリを含むカーソル。

 **From** string array –カーソル内の列の名前に対応する文字列の配列。

 **To** integer array –行レイアウト内のコントロールに対応するレイアウト id の配列。 配列に指定された列の値は、 `from` 同じインデックスのこの配列で指定されている ControlID にバインドされます。

`from`配列と `to` 配列は、データソースからビュー内のレイアウトコントロールへのマッピングを形成するため、同じ数のエントリを持つ必要があります。

**SimpleCursorTableAdapter/HomeScreen**サンプルコードは次のようになり `SimpleCursorAdapter` ます。

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` は、SQLite データをですばやく簡単に表示する方法です `ListView` 。 主な制限事項として、列の値をバインドできるのは、コントロールの表示のみです。また、行のレイアウトの他の側面 (コントロールの表示/非表示、プロパティの変更など) を変更することはできません。

## <a name="subclassing-cursoradapter"></a>サブクラスカーソルのサブクラス化

`CursorAdapter`サブクラスには、SQLite からデータを表示する場合と同じパフォーマンス上の利点があり `SimpleCursorAdapter` ますが、各行ビューの作成とレイアウトを完全に制御することもできます。 `CursorAdapter`実装は `BaseAdapter` `GetView` 、、 `GetItemId` 、 `Count` またはインデクサーをオーバーライドしないため、サブクラス化とは大きく異なり `this[]` ます。

動作している SQLite データベースの場合、サブクラスを作成するには、次の2つのメソッドをオーバーライドする必要があり `CursorAdapter` ます。

- **Bindview** –ビューを指定して、指定されたカーソルのデータを表示するように更新します。

- **Newview** –で `ListView` 新しいビューを表示する必要がある場合に呼び出されます。 は、 `CursorAdapter` (通常のアダプターのメソッドとは異なり) リサイクルビューを処理し `GetView` ます。

前の例のアダプターサブクラスには、行の数を返し、現在の項目を取得するメソッドがあり `CursorAdapter` ます。この情報はカーソル自体から収集される可能性があるため、ではこれらのメソッドは必要ありません。 各ビューの作成と作成をこれらの2つのメソッドに分割することで、によって `CursorAdapter` ビューの再利用が強制されます。 これは、メソッドのパラメーターを無視できる通常のアダプターとは対照的です `convertView` `BaseAdapter.GetView` 。

### <a name="implementing-the-cursoradapter"></a>カーソルアダプターの実装

**カーソル tableadapter/HomeScreenCursorAdapter**のコードには、サブクラスが含まれてい `CursorAdapter` ます。 このメソッドは、コンストラクターに渡されたコンテキスト参照を格納して、メソッド内のにアクセスできるようにし `LayoutInflater` `NewView` ます。 完成したクラスは次のようになります。

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

### <a name="assigning-the-cursoradapter"></a>カーソルアダプターの割り当て

が `Activity` 表示されるで、 `ListView` カーソルを作成し、 `CursorAdapter` リストビューに割り当てます。

このアクションを実行するコードは、 **カーソル tableadapter/HomeScreen**メソッドで次のようになり `OnCreate` ます。

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

メソッドには、 `OnDestroy` 前に説明したメソッドの呼び出しが含まれてい `StopManagingCursor` ます。

## <a name="related-links"></a>関連リンク

- [SimpleCursorTableAdapter (サンプル)](/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [カーソル Tableadapter (サンプル)](/samples/xamarin/monodroid-samples/cursortableadapter)