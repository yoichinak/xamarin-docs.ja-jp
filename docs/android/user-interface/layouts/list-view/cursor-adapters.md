---
title: CursorAdapters の使用
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/25/2017
ms.openlocfilehash: 42b9bd528459d8ee941cc293372bf5662a493342
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827616"
---
# <a name="using-cursoradapters"></a>CursorAdapters の使用


## <a name="overview"></a>概要

Android は、SQLite データベース クエリからのデータを表示するには、具体的には、アダプター クラスを提供します。

 **SimpleCursorAdapter** – と同様に、`ArrayAdapter`サブクラス化せずに使用できるためです。 単に、コンス トラクターで (カーソルとレイアウト情報) などの必須のパラメーターを指定し、割り当てる、`ListView`します。

 **CursorAdapter** – レイアウト コントロール (たとえば、コントロールの表示/非表示またはそのプロパティを変更) するときに必要な詳細に制御するデータのバインディングを介してから継承できる基底クラスの値します。

カーソルのアダプターは、長い SQLite に格納されているデータの一覧をスクロールするパフォーマンスの高い方法を提供します。 使用側コードでの SQL クエリを定義する必要があります、`Cursor`オブジェクトし、作成し、各行のビューを設定する方法について説明します。


## <a name="creating-an-sqlite-database"></a>SQLite データベースを作成します。

カーソルのアダプターを説明するために単純な SQLite データベースの実装が必要です。 コードでは、 **SimpleCursorTableAdapter/VegetableDatabase.cs**コードとテーブルを作成し、データを入力する SQL が含まれています。
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

`VegetableDatabase`でクラスをインスタンス化は、`OnCreate`のメソッド、`HomeScreen`アクティビティ。 `SQLiteOpenHelper`基底クラスがデータベース ファイルの設定を管理し、確実に SQL では、その`OnCreate`メソッドは一度だけ実行します。 このクラスの次の 2 つの例で使用されます`SimpleCursorAdapter`と`CursorAdapter`します。

カーソル クエリ*する必要があります*整数型の列がある`_id`の`CursorAdapter`させる。 基になるテーブルがという名前の整数型の列を持たないかどうか`_id`で一意の整数を別の列の別名を使用し、`RawQuery`カーソルを構成します。 参照してください、 [Android docs](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)についてさらにします。


### <a name="creating-the-cursor"></a>カーソルを作成します。

例を使用して、`RawQuery`に SQL クエリを有効にする、`Cursor`オブジェクト。 カーソルから返される列の一覧は、カーソルのアダプターでの表示に使用できるデータ列を定義します。 データベースを作成するコード、 **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate`メソッドを次に示します。

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

そのコードを呼び出す`StartManagingCursor`も呼び出す必要があります`StopManagingCursor`します。 例を使用して`OnCreate`を開始して`OnDestroy`カーソルを閉じます。 `OnDestroy`メソッドには、このコードが含まれています。

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

アプリケーションでは、SQLite データベースが利用可能にしが示すようにカーソル オブジェクトを作成、いずれかを活用できます、`SimpleCursorAdapter`またはそのサブクラス`CusorAdapter`内の行を表示する、`ListView`します。


## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter を使用します。

`SimpleCursorAdapter` ようには、 `ArrayAdapter`SQLite で使用するため、特殊化されました。 これはサブクラス化が必要 – だけオブジェクトを作成するときにいくつかの単純なパラメーターを設定されずに割り当てる、`ListView`の`Adapter`プロパティ。

SimpleCursorAdapter コンス トラクターのパラメーターは次のとおりです。

 **コンテキスト**–、含まれるアクティビティへの参照。

 **レイアウト**– 使用する、行ビューのリソース ID。

 **ICursor** – SQLite クエリを表示するデータを含むカーソル。

 **から**文字列配列 – カーソル内の列の名前に対応する文字列の配列。

 **整数配列へ** – 行レイアウト内のコントロールに対応するレイアウト Id の配列。 指定された列の値、`from`配列は同じインデックスには、この配列で指定された ControlID にバインドされます。

`from`と`to`ビューで、データ ソースからレイアウト コントロールへのマッピングを形成するため、配列は同じ数のエントリをいる必要があります。

**SimpleCursorTableAdapter/HomeScreen.cs**をコード ワイヤをサンプル、`SimpleCursorAdapter`次のように。

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` SQLite のデータを表示する高速で簡単な方法は、`ListView`します。 主要な制限は、コントロールを表示する列の値をバインドすることができますのみが、行のレイアウト (たとえば、コントロールの表示/非表示またはプロパティを変更する) の他の側面を変更することはできませんです。


## <a name="subclassing-cursoradapter"></a>CursorAdapter をサブクラス化

A`CursorAdapter`サブクラスには、同じパフォーマンスの利点として、`SimpleCursorAdapter`作成し、それぞれの行ビューのレイアウトを完全に制御を提供する SQLite などからデータを表示するもの。 `CursorAdapter`実装はサブクラス化と大きく異なる`BaseAdapter`オーバーライドしないため、 `GetView`、 `GetItemId`、`Count`または`this[]`インデクサーです。

作業中の指定した SQLite データベースを作成する 2 つのメソッドをオーバーライドするだけでかまいません、`CursorAdapter`サブクラスです。

- **BindView** – 指定されたカーソルでデータを表示するようにビューを指定すると、更新します。

- **NewView** – ときに呼び出されます、`ListView`新しいビューを表示する必要があります。 `CursorAdapter`ビューのリサイクルが取得されます (とは異なり、`GetView`標準アダプター上のメソッド)。

前の例のアダプターのサブクラスは、行の数を取得し、– 現在の項目を取得するメソッドを持つ、`CursorAdapter`カーソル自体からその情報を得られるためこれらのメソッドは必要ありません。 作成およびこれら 2 つのメソッドには、各ビューの設定を分割することで、`CursorAdapter`ビューを再使用を強制します。 これは、これに対し、通常のアダプターを無視すること、`convertView`のパラメーター、`BaseAdapter.GetView`メソッド。


### <a name="implementing-the-cursoradapter"></a>CursorAdapter を実装します。

コードでは、 **CursorTableAdapter/HomeScreenCursorAdapter.cs**が含まれています、`CursorAdapter`サブクラスです。 アクセスできるように、コンス トラクターに渡されるコンテキストの参照を格納する`LayoutInflater`で、`NewView`メソッド。 完全なクラスのようになります。

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

`Activity`を表示する、 `ListView`、カーソルを作成し、`CursorAdapter`リスト ビューに割り当てます。

この操作を実行するコード、 **CursorTableAdapter/HomeScreen.cs** `OnCreate`メソッドを次に示します。

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy`メソッドが含まれています、`StopManagingCursor`前に説明したメソッド呼び出し。



## <a name="related-links"></a>関連リンク

- [SimpleCursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/monodroid/SimpleCursorTableAdapter/)
- [CursorTableAdapter (サンプル)](https://developer.xamarin.com/samples/monodroid/CursorTableAdapter/)
