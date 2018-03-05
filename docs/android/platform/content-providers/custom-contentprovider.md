---
title: "カスタム ContentProvider を作成します。"
description: "前のセクションでは、組み込み ContentProvider 実装からのデータを使用する方法を示しました。 このセクションでは、カスタム ContentProvider をビルドし、そのデータを使用する方法を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 66b956eddc48699c6fd61e9cb52a7fbc3fa70a51
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-custom-contentprovider"></a>カスタム ContentProvider を作成します。

_前のセクションでは、組み込み ContentProvider 実装からのデータを使用する方法を示しました。このセクションでは、カスタム ContentProvider をビルドし、そのデータを使用する方法を説明します。_

## <a name="about-contentproviders"></a>ContentProviders について

コンテンツ プロバイダー クラスは継承する必要があります`ContentProvider`です。 クエリに応答するために使用する内部データ ストアので構成されている必要がありますありようにコードを使用する定数を使用するデータの有効な要求 Uri と MIME の種類を公開する必要があります。

### <a name="uri-authority"></a>URI (機関)

`ContentProviders` Uri を使用して Android でアクセスされます。 公開するアプリケーション、`ContentProvider`内に応答する Uri を設定、 **AndroidManifest.xml**ファイル。 アプリケーションがインストールされているときに、これらの Uri は、他のアプリケーションがアクセスできるようにするに登録されます。

Android 用の Mono でのコンテンツ プロバイダー クラスがあります、 `[ContentProvider]` 、Uri (Uri) を指定する属性に追加する必要がありますを**AndroidManifest.xml**です。

<a name="Mime_Type" />

### <a name="mime-type"></a>Mime の種類

MIME の種類の一般的な形式は、2 つの部分で構成されます。 Android`ContentProviders`一般的な MIME の種類の最初の部分をこれら 2 つの文字列を使用します。

1. `vnd.android.cursor.item` &ndash; 1 つの行を表すを使用して、`ContentResolver.CursorItemBaseType`コード内で一定です。

1. `vnd.android.cursor.dir` &ndash; 複数の行を使用して、`ContentResolver.CursorDirBaseType`コード内で一定です。

MIME の種類の 2 番目の部分は、アプリケーションに固有でありと逆引き DNS 標準を使用する必要があります、`vnd.`プレフィックス。 使用するサンプル コード`vnd.com.xamarin.sample.Vegetables`です。

<a name="Data_Model_Metadata" />

### <a name="data-model-metadata"></a>データ モデルのメタデータ

使用するアプリケーションは、異なる種類のデータにアクセスする Uri のクエリを作成する必要があります。 ベース Uri は、展開すると、データの特定のテーブルを参照してくださいおよび結果をフィルター処理パラメーターを含めることもできます。 列と句の結果として得られるカーソルでデータを表示するために使用も宣言する必要があります。

有効な Uri のクエリのみが構築されることを確認してください、定数値として有効な文字列を指定する一般的なです。 これにより簡単にアクセスする、`ContentProvider`のため、値のコード補完機能を使用して探索可能になり、文字列の入力ミスを防止します。

前の例で、`android.provider.ContactsContract`クラスは、連絡先データのメタデータを公開します。 カスタムの`ContentProvider`クラス自体の定数でのみ公開されます。

<a name="Implementation" />

## <a name="implementation"></a>実装

3 つの手順があるカスタムの作成と`ContentProvider`:

1. **データベース クラスを作成する**&ndash;実装`SQLiteOpenHelper`です。

2. **作成、`ContentProvider`クラス**&ndash;実装`ContentProvider`定数値とデータにアクセスするメソッドとして、データベースのインスタンスのメタデータが公開されています。

3. **アクセス、`ContentProvider`その Uri を使用して** &ndash; Populate、`CursorAdapter`を使用して、 `ContentProvider`、その Uri を使用してアクセスします。

既に説明したよう`ContentProviders`が定義されている以外のアプリケーションから使用できます。 この例では、データが、同じアプリケーションで使用されたが、他のアプリケーションもアクセスできます (これは通常、定数値として公開) スキーマについては、Uri が知っている場合に限り注意してください。

<a name="Create_a_database" />

## <a name="create-a-database"></a>データベースを作成します。

ほとんど`ContentProvider`実装の基になる、`SQLite`データベース。 データベースのコード例**SimpleContentProvider/VegetableDatabase.cs**ようには非常に単純な 2 つの列データベースを作成します。

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

データベースの実装自体には、特別な配慮を公開する必要はありません、 `ContentProvider`、ただしをバインドする場合、`ContentProvider's`にデータを`ListView`という名前の一意の整数列を制御し、`_id`の一部にする必要があります、結果セットです。 参照してください、[の Listview およびアダプター](~/android/user-interface/layouts/list-view/index.md)使用に関する詳細については、ドキュメント、`ListView`コントロール。

<a name="Create_the_ContentProvider" />

## <a name="create-the-contentprovider"></a>ContentProvider を作成します。

このセクションの残りの部分では、手順を説明する方法  **SimpleContentProvider/VegetableProvider.cs**クラスの例でビルドされました。

<a name="Initialize_the_Database" />

### <a name="initialize-the-database"></a>データベースを初期化します。

サブクラスには、まず`ContentProvider`が使用するデータベースを追加します。

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

コードの残りの部分は、データが検出され、クエリを実行できるようにする実際のコンテンツ プロバイダーの実装を形成します。

<a name="Add_Metadata_for_Consumers" />


## <a name="add-metadata-for-consumers"></a>コンシューマーのメタデータを追加します。

4 つの異なる種類のメタデータで公開しようとしているが、`ContentProvider`クラスです。 だけで、権限が必要な残りの部分は、規約によって実行されます。

- **機関** &ndash; 、`ContentProvider`属性*必要があります*アプリケーションがインストールされているときに、Android での登録できるように、クラスに追加します。

- **Uri** &ndash; 、`CONTENT_URI`コードで簡単に使用されるように、定数として公開します。 機関が一致がスキームと基本パスが含まれて、必要があります。

- **MIME の種類**&ndash;結果と 1 つの結果の一覧が、さまざまなコンテンツの種類として扱われるので、それらを表す 2 つの MIME の種類を定義します。

- **InterfaceConsts** &ndash;使用コードを簡単に検出し、誤字を危険にさらすに参照できるように、各データ列名の定数値を指定します。

このコードでは、これらの各項目の実装方法を示しています、前の手順からデータベースの定義を追加します。

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

<a name="Implement_the_URI_Parsing_Helper" />

## <a name="implement-the-uri-parsing-helper"></a>ヘルパーを解析する URI を実装します。

要求を行うに Uri を使用するコードを実行するため、 `ContentProvider`、返されるデータを決定するには、その要求を解析できる必要があります。 `UriMatcher`クラスは、Uri の解析に役立つことが、後で、初期化されている場合は、Uri パターン、`ContentProvider`をサポートしています。

`UriMatcher`の例では、2 つの Uri で初期化されます。

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). アスタリスクのプレース ホルダー ("\*")、テキスト パラメーターに一致するようにも使用できます。

コードでお定数を使用して、参照するように、証明機関とベース メタデータ値\_パス。 これらのリターン コードは、Uri の解析を行って、返されるデータを決定を行うメソッドで使用されます。

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

このコードはすべてのプライベート、`ContentProvider`クラスです。 参照してください[Google の UriMatcher ドキュメント](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)についてさらにします。

<a name="Implement_the_QueryMethod" />

## <a name="implement-the-querymethod"></a>実装、QueryMethod

最も単純な`ContentProvider`メソッドを実装するのには、`Query`メソッドです。 実装、`UriMatcher`を解析、`uri`パラメーターと、適切なデータベース メソッドを呼び出します。 場合、 `uri` out の整数を解析し、ID パラメーターが含まれています (を使用して`LastPathSegment`) であり、データベース クエリに使用します。

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

`GetType`メソッドをオーバーライドもする必要があります。 指定した Uri の返されるコンテンツの種類を特定するこのメソッドを呼び出すことがあります。
そのデータを処理する方法を利用するアプリケーションに通知この可能性があります。

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

<a name="Implement_the_Other_Overrides" />

## <a name="implement-the-other-overrides"></a>その他のオーバーライドを実装します。

編集またはデータの削除、単純な例ができませんが、Insert、Update および Delete メソッドを実装する必要がありますので、実装しなくても追加します。

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

基本的なを完了する`ContentProvider`実装します。 公開データが使用できるアプリケーションをインストールすると、アプリケーションの内部は、参照する Uri を知っているその他のアプリケーションにも両方です。

<a name="Access_the_ContentProvider" />

## <a name="access-the-contentprovider"></a>アクセス、ContentProvider

1 回、`VegetableProvider`が実装されるにアクセスするときはこのドキュメントの先頭のアドレス帳プロバイダーと同じ方法: 指定した Uri を使用してカーソルを取得し、アダプターを使用してデータにアクセスします。

<a name="Bind_a_ListView_to_a_ContentProvider" />

## <a name="bind-a-listview-to-a-contentprovider"></a>ListView を ContentProvider にバインドします。

設定する、`ListView`野菜のフィルター選択されていない一覧に対応する Uri を使用しておデータを使用します。 定数値を使用するコードでお`VegetableProvider.CONTENT_URI`に解決されることが分かっている`com.xamarin.sample.vegetableprovider/vegetables`です。 当社`VegetableProvider.Query`にバインドすることができますし、カーソルの実装を返します、`ListView`です。

内のコード`SimpleContentProvider/HomeScreen.cs`からデータを表示するは簡単な方法を示しています、 `ContentProvider`:

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

次のような結果として得られるアプリケーションが表示。

[![野菜、くだもの、花バッド、Legumes、電球、Tubers を一覧表示するアプリのスクリーン ショット](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png)


<a name="Retrieve_a_Single_Item_from_a_ContentProvider" />

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider から 1 つの項目を取得します。

処理を行うアプリケーションすることも (たとえば) 特定の行を参照する別の Uri を構築することによって行うことが可能には、データの 1 つの行にアクセスします。

使用して`ContentResolver`、必須の Uri を構築して 1 つの項目にアクセスするには、直接`Id`です。

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

完全なメソッドは、次のようになります。

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

- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
