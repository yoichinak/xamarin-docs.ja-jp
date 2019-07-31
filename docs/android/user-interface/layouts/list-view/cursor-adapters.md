---
title: CursorAdapters の使用
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/25/2017
ms.openlocfilehash: ce2f62869057fc83b04b58af37d6ffffd5ad7fb8
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646472"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Xamarin Android でのカーソルの使用

Android には、SQLite データベースクエリのデータを表示するためのアダプタークラスが用意されています。

 **SimpleCursorAdapter** –サブクラス化`ArrayAdapter`せずに使用できるため、に似ています。 コンストラクターで必要なパラメーター (カーソルやレイアウト情報など) を指定し、に`ListView`割り当てるだけです。

 **カーソル**: レイアウトコントロールへのデータ値のバインドをより詳細に制御する必要がある場合に継承できる基本クラス (コントロールの表示/非表示、プロパティの変更など)。

カーソルアダプターは、SQLite に格納されているデータの長い一覧をスクロールするための高パフォーマンスな方法を提供します。 コンシューマーコードでは、 `Cursor`オブジェクトで SQL クエリを定義し、各行のビューを作成および設定する方法を記述する必要があります。


## <a name="creating-an-sqlite-database"></a>SQLite データベースの作成

カーソルアダプターを示すには、簡単な SQLite データベース実装が必要です。 **SimpleCursorTableAdapter/VegetableDatabase**のコードには、テーブルを作成してデータを設定するためのコードと SQL が含まれています。
完全な`VegetableDatabase`クラスを次に示します。

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

クラス`VegetableDatabase`は、 `HomeScreen`アクティビティの`OnCreate`メソッドでインスタンス化されます。 基本`SQLiteOpenHelper`クラスは、データベースファイルのセットアップを管理し、その`OnCreate`メソッドの SQL が1回だけ実行されるようにします。 このクラスは、と`SimpleCursorAdapter` `CursorAdapter`の次の2つの例で使用します。

を機能させるには、 `CursorAdapter`カーソル`_id`クエリに整数型の列が*必要*です。 基になるテーブルにという名前`_id`の整数型の列がない場合は、 `RawQuery`カーソルを構成する内の別の一意の整数に対して列の別名を使用します。 詳細については、 [Android のドキュメント](xref:Android.Widget.CursorAdapter)を参照してください。


### <a name="creating-the-cursor"></a>カーソルの作成

この例では`RawQuery` 、を使用して、SQL `Cursor`クエリをオブジェクトに変換します。 カーソルから返される列リストによって、カーソルアダプターに表示できるデータ列が定義されます。 **SimpleCursorTableAdapter/HomeScreen** `OnCreate`メソッドでデータベースを作成するコードを次に示します。

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

を呼び出す`StartManagingCursor`すべてのコードは、 `StopManagingCursor`もを呼び出す必要があります。 この例で`OnCreate`は、を使用`OnDestroy`してを開始し、カーソルを閉じます。 メソッド`OnDestroy`には次のコードが含まれています。

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

アプリケーションで SQLite データベースを使用可能にし、そのオブジェクトを表示されているように作成する`SimpleCursorAdapter`と、の`CusorAdapter`またはサブクラスを使用`ListView`して、に行を表示できます。


## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter の使用

`SimpleCursorAdapter`はに似`ArrayAdapter`ていますが、SQLite での使用に特化しています。 サブクラス化は必要ありません。オブジェクトを作成するときに単純なパラメーターをいくつか`ListView`設定`Adapter`し、そのオブジェクトをのプロパティに割り当てるだけです。

SimpleCursorAdapter コンストラクターのパラメーターは次のとおりです。

 **Context** –格納しているアクティビティへの参照。

 **Layout** –使用する行ビューのリソース ID。

 **ICursor** –表示するデータの SQLite クエリを含むカーソル。

 **から**文字列配列 – カーソル内の列の名前に対応する文字列の配列。

 **整数配列へ** – 行レイアウト内のコントロールに対応するレイアウト Id の配列。 `from`配列に指定された列の値は、同じインデックスのこの配列で指定されている ControlID にバインドされます。

配列`from` と`to`配列は、データソースからビュー内のレイアウトコントロールへのマッピングを形成するため、同じ数のエントリを持つ必要があります。

**SimpleCursorTableAdapter/HomeScreen**サンプルコードは次のように`SimpleCursorAdapter`なります。

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter`は、SQLite データをで`ListView`すばやく簡単に表示する方法です。 主な制限事項として、列の値をバインドできるのは、コントロールの表示のみです。また、行のレイアウトの他の側面 (コントロールの表示/非表示、プロパティの変更など) を変更することはできません。


## <a name="subclassing-cursoradapter"></a>サブクラスカーソルのサブクラス化

サブクラスには、 `SimpleCursorAdapter` SQLite からデータを表示する場合と同じパフォーマンス上の利点がありますが、各行ビューの作成とレイアウトを完全に制御することもできます。 `CursorAdapter` `GetItemId` `GetView`実装は`this[]` 、、、 `BaseAdapter` また`Count`はインデクサーをオーバーライドしないため、サブクラス化とは大きく異なります。 `CursorAdapter`

動作している SQLite データベースの場合、サブクラスを`CursorAdapter`作成するには、次の2つのメソッドをオーバーライドする必要があります。

- **Bindview** –ビューを指定して、指定されたカーソルのデータを表示するように更新します。

- **Newview** –で新しいビュー `ListView`を表示する必要がある場合に呼び出されます。 は`CursorAdapter` 、(通常のアダプターの`GetView`メソッドとは異なり) リサイクルビューを処理します。

前の例のアダプターサブクラスには、行の数を返し、現在の項目を取得する`CursorAdapter`メソッドがあります。この情報はカーソル自体から収集される可能性があるため、ではこれらのメソッドは必要ありません。 各ビューの作成と作成をこれらの2つのメソッドに分割`CursorAdapter`することで、によってビューの再利用が強制されます。 これは、 `convertView` `BaseAdapter.GetView`メソッドのパラメーターを無視できる通常のアダプターとは対照的です。


### <a name="implementing-the-cursoradapter"></a>カーソルアダプターの実装

**カーソル tableadapter/HomeScreenCursorAdapter**のコードには`CursorAdapter` 、サブクラスが含まれています。 このメソッドは`NewView` 、コンストラクターに渡されたコンテキスト参照を格納して`LayoutInflater` 、メソッド内のにアクセスできるようにします。 完成したクラスは次のようになります。

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

が表示されるで、カーソルを作成し`CursorAdapter` 、リストビューに割り当てます。 `Activity` `ListView`

このアクションを実行するコードは、**カーソル tableadapter/HomeScreen** `OnCreate`メソッドで次のようになります。

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

メソッド`OnDestroy`には、 `StopManagingCursor`前に説明したメソッドの呼び出しが含まれています。



## <a name="related-links"></a>関連リンク

- [SimpleCursorTableAdapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [カーソル Tableadapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
