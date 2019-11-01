---
title: CursorAdapters の使用
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/25/2017
ms.openlocfilehash: d0b5845036ab2981a4aa06d2a01ed6b13d094bef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028909"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Xamarin Android でのカーソルの使用

Android には、SQLite データベースクエリのデータを表示するためのアダプタークラスが用意されています。

 **SimpleCursorAdapter** –サブクラス化せずに使用できるため、`ArrayAdapter` に似ています。 コンストラクターで必要なパラメーター (カーソルやレイアウト情報など) を指定し、`ListView`に割り当てるだけです。

 **カーソル**: レイアウトコントロールへのデータ値のバインドをより詳細に制御する必要がある場合に継承できる基本クラス (コントロールの表示/非表示、プロパティの変更など)。

カーソルアダプターは、SQLite に格納されているデータの長い一覧をスクロールするための高パフォーマンスな方法を提供します。 コンシューマーコードでは、`Cursor` オブジェクトで SQL クエリを定義し、各行のビューを作成および設定する方法を記述する必要があります。

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

`VegetableDatabase` クラスは、`HomeScreen` アクティビティの `OnCreate` メソッドでインスタンス化されます。 `SQLiteOpenHelper` 基本クラスは、データベースファイルのセットアップを管理し、`OnCreate` メソッド内の SQL が1回だけ実行されるようにします。 このクラスは、次の2つの例では `SimpleCursorAdapter` と `CursorAdapter`に使用されています。

`CursorAdapter` を機能させるには、カーソルクエリに整数列 `_id` が*必要*です。 基になるテーブルに `_id` という名前の整数型の列がない場合は、カーソルを構成する `RawQuery` 内の別の一意の整数に対して列の別名を使用します。 詳細については、 [Android のドキュメント](xref:Android.Widget.CursorAdapter)を参照してください。

### <a name="creating-the-cursor"></a>カーソルの作成

この例では、`RawQuery` を使用して、SQL クエリを `Cursor` オブジェクトに変換します。 カーソルから返される列リストによって、カーソルアダプターに表示できるデータ列が定義されます。 **SimpleCursorTableAdapter/HomeScreen** `OnCreate` メソッドにデータベースを作成するコードを次に示します。

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

`StartManagingCursor` を呼び出すコードでも `StopManagingCursor`を呼び出す必要があります。 この例では、`OnCreate` を使用してを開始し、`OnDestroy` してカーソルを閉じます。 `OnDestroy` メソッドには、次のコードが含まれています。

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

アプリケーションで SQLite データベースが使用可能になっていて、そのオブジェクトが示されているように作成されると、`SimpleCursorAdapter` または `CusorAdapter` のサブクラスを使用して、`ListView`に行を表示できます。

## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter の使用

`SimpleCursorAdapter` は `ArrayAdapter`に似ていますが、SQLite での使用に特化しています。 サブクラス化は必要ありません。オブジェクトを作成するときに単純なパラメーターを設定し、それを `ListView`の `Adapter` プロパティに割り当てるだけです。

SimpleCursorAdapter コンストラクターのパラメーターは次のとおりです。

 **Context** –格納しているアクティビティへの参照。

 **Layout** –使用する行ビューのリソース ID。

 **ICursor** –表示するデータの SQLite クエリを含むカーソル。

 **From** string array –カーソル内の列の名前に対応する文字列の配列。

 **To** integer array –行レイアウト内のコントロールに対応するレイアウト id の配列。 `from` 配列に指定された列の値は、同じインデックスのこの配列で指定されている ControlID にバインドされます。

`from` と `to` の配列は、データソースからビュー内のレイアウトコントロールへのマッピングを形成するため、同じ数のエントリを持つ必要があります。

**SimpleCursorTableAdapter/HomeScreen**サンプルコードは、次のように `SimpleCursorAdapter` を作成します。

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` は、`ListView`に SQLite データをすばやく簡単に表示する方法です。 主な制限事項として、列の値をバインドできるのは、コントロールの表示のみです。また、行のレイアウトの他の側面 (コントロールの表示/非表示、プロパティの変更など) を変更することはできません。

## <a name="subclassing-cursoradapter"></a>サブクラスカーソルのサブクラス化

`CursorAdapter` サブクラスには、SQLite からデータを表示するための `SimpleCursorAdapter` と同じパフォーマンス上の利点がありますが、各行ビューの作成とレイアウトを完全に制御することもできます。 `CursorAdapter` の実装は、`GetView`、`GetItemId`、`Count`、または `this[]` インデクサーをオーバーライドしないため、`BaseAdapter` のサブクラス化とは大きく異なります。

動作する SQLite データベースがある場合は、次の2つのメソッドをオーバーライドして `CursorAdapter` サブクラスを作成するだけで済みます。

- **Bindview** –ビューを指定して、指定されたカーソルのデータを表示するように更新します。

- **Newview** – `ListView` が新しいビューを表示する必要がある場合に呼び出されます。 `CursorAdapter` は、通常のアダプターの `GetView` 方法とは異なり、リサイクルビューを処理します。

前の例のアダプターサブクラスには、行の数を返し、現在の項目を取得するメソッドがあります。この情報はカーソル自体から収集される可能性があるため、`CursorAdapter` ではこれらのメソッドは必要ありません。 各ビューの作成と作成をこれらの2つのメソッドに分割すると、`CursorAdapter` によってビューの再利用が適用されます。 これは、`BaseAdapter.GetView` メソッドの `convertView` パラメーターを無視できる通常のアダプターとは対照的です。

### <a name="implementing-the-cursoradapter"></a>カーソルアダプターの実装

**カーソル tableadapter/HomeScreenCursorAdapter**のコードには、`CursorAdapter` サブクラスが含まれています。 このメソッドは、コンストラクターに渡されたコンテキスト参照を格納し、`NewView` メソッド内の `LayoutInflater` にアクセスできるようにします。 完成したクラスは次のようになります。

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

`ListView`を表示する `Activity` で、カーソルを作成し `CursorAdapter`、リストビューに割り当てます。

このアクションを実行するコードは、**カーソル**の `OnCreate` メソッドで、次のようになります。

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy` メソッドには、前に説明した `StopManagingCursor` メソッドの呼び出しが含まれています。

## <a name="related-links"></a>関連リンク

- [SimpleCursorTableAdapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [カーソル Tableadapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
