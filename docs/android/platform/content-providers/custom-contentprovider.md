---
title: カスタム ContentProvider の作成
description: 前のセクションでは、組み込みの ContentProvider 実装からデータを使用する方法を示しました。 このセクションでは、カスタム ContentProvider の作成し、し、そのデータを利用する方法について説明します。
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: da8aacac1f282fefb6b8d0e84cae168cf3a7148b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108016"
---
# <a name="creating-a-custom-contentprovider"></a>カスタム ContentProvider の作成

_前のセクションでは、組み込みの ContentProvider 実装からデータを使用する方法を示しました。このセクションでは、カスタム ContentProvider の作成し、し、そのデータを利用する方法について説明します。_

## <a name="about-contentproviders"></a>ContentProviders について

コンテンツ プロバイダー クラスを継承する必要があります`ContentProvider`します。 クエリに応答するために使用する内部データ ストアで構成されている必要があり、ようにコードを使用する定数を使用するデータの有効な要求 Uri と MIME の種類を公開する必要があります。

### <a name="uri-authority"></a>URI (機関)

`ContentProviders` Uri を使用した Android でアクセスされます。 アプリケーションを公開する、`ContentProvider`への応答 Uri を設定、 **AndroidManifest.xml**ファイル。 アプリケーションがインストールされている場合、これらの Uri は、他のアプリケーションがアクセスできるように登録されます。

Mono for Android でのコンテンツ プロバイダー クラスがあります、 `[ContentProvider]` Uri (または Uri) を指定する属性を追加すること**AndroidManifest.xml**します。


### <a name="mime-type"></a>Mime の種類

MIME の種類の一般的な形式は、2 つの部分で構成されます。 Android `ContentProviders` MIME の種類の最初の部分に、これら 2 つの文字列をよく使用します。

1. `vnd.android.cursor.item` &ndash; 1 つの行を表すを使用して、`ContentResolver.CursorItemBaseType`コードで定数。

1. `vnd.android.cursor.dir` &ndash; 複数の行を使用して、`ContentResolver.CursorDirBaseType`コードで定数。

MIME の種類の 2 番目の部分は、アプリケーションに固有でありの逆引き DNS の標準を使用する必要があります、`vnd.`プレフィックス。 サンプル コードを使用して`vnd.com.xamarin.sample.Vegetables`します。


### <a name="data-model-metadata"></a>データ モデルのメタデータ

コンシューマー側アプリケーションでは、さまざまな種類のデータにアクセスする Uri のクエリを作成する必要があります。 ベース Uri では、展開すると、データの特定のテーブルを参照してくださいし、結果をフィルター処理するパラメーターを含めることもできます。 列と句のカーソルが結果として得られるデータを表示するために使用も宣言する必要があります。

有効な Uri のクエリのみが構築されることを確認するには、定数値として有効な文字列を提供する慣習があります。 これにより、アクセスしやすく、`ContentProvider`コード補完機能を使用して探索可能な値は、文字列の入力ミスを防止するためです。

前の例では、`android.provider.ContactsContract`クラスは、連絡先データのメタデータを公開します。 カスタムの`ContentProvider`クラス自体の定数が公開されます。


## <a name="implementation"></a>実装

3 つの手順があるカスタムの作成と`ContentProvider`:

1. **データベース クラスを作成**&ndash;実装`SQLiteOpenHelper`します。

2. **作成、`ContentProvider`クラス**&ndash;実装`ContentProvider`データベースのインスタンスのメタデータを定数値、およびデータにアクセスするメソッドとして公開します。

3. **アクセス、`ContentProvider`その Uri を使用して** &ndash; Populate、`CursorAdapter`を使用して、 `ContentProvider`、その Uri を使用してアクセスします。

前述のように、`ContentProviders`が定義されている以外のアプリケーションから利用できます。 この例では、データが、同じアプリケーションで使用されるが、他のアプリケーションもアクセスできる (これは通常、定数値として公開) スキーマについては、Uri がわかっている限りに注意してください。


## <a name="create-a-database"></a>データベースを作成します。

ほとんど`ContentProvider`実装の基になる、`SQLite`データベース。 データベース コードの例で**SimpleContentProvider/VegetableDatabase.cs**に示すように、非常に単純な 2 つの列のデータベースを作成します。

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

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
  {
    throw new NotImplementedException();
  }
}
```

データベースの実装自体には、特別な配慮を公開する必要はありません、 `ContentProvider`、ただしをバインドする場合、`ContentProvider's`データを`ListView`という名前の一意の整数列を制御し、`_id`の一部にする必要があります、結果セット。 参照してください、 [Listview と Adapter](~/android/user-interface/layouts/list-view/index.md)ドキュメントを使用しての詳細については、`ListView`コントロール。


## <a name="create-the-contentprovider"></a>ContentProvider を作成します。

このセクションの残りの部分でどの手順について説明**SimpleContentProvider/VegetableProvider.cs**クラスの例をビルドします。


### <a name="initialize-the-database"></a>データベースを初期化します。

サブクラスには、まず`ContentProvider`のために使用するデータベースを追加します。

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

コードの残りの部分は、データが検出され、クエリを実行できる実際のコンテンツ プロバイダーの実装を形成します。



## <a name="add-metadata-for-consumers"></a>コンシューマーのメタデータを追加します。

次の 4 つの異なる種類のメタデータを公開しようとしているが、`ContentProvider`クラス。 権限が必要なだけで、残りの部分は、規則によって実行されます。

- **機関** &ndash; 、`ContentProvider`属性*する必要があります*アプリケーションがインストールされている場合に、Android に登録されているように、クラスに追加します。

- **Uri** &ndash; 、`CONTENT_URI`コードで簡単に使用されるように、定数として公開されます。 これにより、機関が一致する必要がありますがスキームと基本パスが含まれます。

- **MIME の種類**&ndash;結果および 1 つの結果の一覧は、さまざまなコンテンツの種類として扱われるので、それらを表す 2 つの MIME の種類を定義します。

- **InterfaceConsts** &ndash;が、簡単に検出して入力ミスのリスクなしに参照コードを実行できるように、各データ列名の定数値を指定します。

このコードでは、これらの各項目の実装方法、データベース定義に、前の手順を追加します。

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```


## <a name="implement-the-uri-parsing-helper"></a>ヘルパーを解析する URI を実装します。

要求を Uri を使用するコードを実行するため、 `ContentProvider`、どのデータを返すを判別するには、その要求を解析できる必要があります。 `UriMatcher`クラスが Uri の解析に役立つ、Uri パターンにで初期化した後、`ContentProvider`をサポートしています。

`UriMatcher`の例では、2 つの Uri で初期化されます。

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). アスタリスクのプレース ホルダー ("\*") テキスト パラメーターに一致するようにも使用できます。

コードを使用して、定数、証明機関とベースなどのメタデータ値を参照してください\_パス。 これらのリターン コードは、Uri を返すデータを決定する、解析を行うメソッドで使用されます。

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

このコードはすべてのプライベート、`ContentProvider`クラス。 参照してください[Google の UriMatcher ドキュメント](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)についてさらにします。


## <a name="implement-the-querymethod"></a>実装、QueryMethod

最も単純な`ContentProvider`メソッドを実装、`Query`メソッド。 実装、`UriMatcher`を解析、`uri`パラメーターを適切なデータベース メソッドを呼び出します。 場合、 `uri` 、整数を解析し、ID パラメーターが含まれています (を使用して`LastPathSegment`)、データベース クエリで使用します。

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

`GetType`メソッドをオーバーライドすることも必要があります。 指定した Uri の返されるコンテンツの種類を決定するこのメソッドを呼び出すことがあります。
そのデータを処理する方法をコンシューマー側アプリケーションに通知この可能性があります。

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```


## <a name="implement-the-other-overrides"></a>その他のオーバーライドを実装します。

単純な例は、編集またはデータの削除できませんが、Insert、Update および Delete メソッドを実装する必要がありますので、実装を持たない追加。

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

基本的な完了`ContentProvider`実装します。 公開されるデータが利用するアプリケーションをインストールすると、アプリケーション内でどちらも参照する Uri を認識している他のアプリケーションにもします。


## <a name="access-the-contentprovider"></a>アクセス、ContentProvider

1 回、`VegetableProvider`された実装へのアクセスはこのドキュメントの先頭にある連絡先のプロバイダーと同じ方法: 指定した Uri を使用してカーソルを取得し、アダプターを使用してデータにアクセスします。


## <a name="bind-a-listview-to-a-contentprovider"></a>ContentProvider に、ListView をバインドします。

設定する、`ListView`データ野菜のフィルター処理されていない一覧に対応する Uri を使用します。 定数の値を使用してコードで`VegetableProvider.CONTENT_URI`にわかるように解決される`com.xamarin.sample.vegetableprovider/vegetables`します。 この`VegetableProvider.Query`の実装にし、バインド可能なカーソルを返します、`ListView`します。

コードでは、`SimpleContentProvider/HomeScreen.cs`からデータを表示するが簡単な方法を示しています、 `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

作成されたアプリケーションのようになります。

[![野菜、くだもの、花バッド、Legumes、電球、Tubers を一覧表示するアプリのスクリーン ショット](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider から 1 つの項目を取得します。

コンシューマー側アプリケーションは、(たとえば) を参照する特定の行を別の Uri の構築を行う、データの 1 つの行へのアクセスにもかまいません。

使用`ContentResolver`を必須の Uri を構築することにより、1 つの項目にアクセスするには、直接`Id`します。

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

完全なメソッドのようになります。

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```


## <a name="related-links"></a>関連リンク

- [SimpleContentProvider (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
