---
title: カスタム ContentProvider の作成
description: 前のセクションでは、組み込みの ContentProvider 実装からデータを使用する方法を説明しています。 ここでは、カスタム ContentProvider を構築し、そのデータを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: e16aa1b96749047554b4f8e6887791d8ed4ff63b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643690"
---
# <a name="creating-a-custom-contentprovider"></a>カスタム ContentProvider の作成

_前のセクションでは、組み込みの ContentProvider 実装からデータを使用する方法を説明しています。ここでは、カスタム ContentProvider を構築し、そのデータを使用する方法について説明します。_

## <a name="about-contentproviders"></a>ContentProviders について

コンテンツプロバイダークラスは、から`ContentProvider`継承する必要があります。 これは、クエリに応答するために使用される内部データストアで構成されている必要があります。また、コードを使用してデータの有効な要求を行うために、Uri と MIME の種類を定数として公開する必要があります。

### <a name="uri-authority"></a>URI (機関)

`ContentProviders`Android では、Uri を使用してアクセスします。 を`ContentProvider`公開するアプリケーションは、 **androidmanifest .xml**ファイルで応答する uri を設定します。 アプリケーションがインストールされると、他のアプリケーションがアクセスできるように、これらの Uri が登録されます。

Mono for Android の場合、コンテンツプロバイダークラスには、 `[ContentProvider]` **androidmanifest**に追加する uri (または uri) を指定する属性が必要です。


### <a name="mime-type"></a>Mime の種類

MIME の種類の一般的な形式は、2つの部分で構成されています。 Android `ContentProviders`では、通常、次の2つの文字列を MIME の種類の最初の部分に使用します。

1. `vnd.android.cursor.item`1つの行を表すには`ContentResolver.CursorItemBaseType` 、コードで定数を使用します。 &ndash;

1. `vnd.android.cursor.dir`複数の行の場合は`ContentResolver.CursorDirBaseType` 、コードで定数を使用します。 &ndash;

MIME の種類の2番目の部分はアプリケーションに固有であり、 `vnd.`プレフィックスを持つ逆引き DNS 標準を使用する必要があります。 このサンプルコードで`vnd.com.xamarin.sample.Vegetables`は、を使用します。


### <a name="data-model-metadata"></a>データモデルのメタデータ

使用するアプリケーションでは、さまざまな種類のデータにアクセスするために Uri クエリを作成する必要があります。 ベース Uri は、特定のデータテーブルを参照するように拡張できます。また、結果をフィルター処理するためのパラメーターを含めることもできます。 データを表示するために、結果のカーソルで使用される列と句も宣言する必要があります。

有効な Uri クエリのみが構築されるようにするには、有効な文字列を定数値として指定することが慣例です。 これにより、コード補完に`ContentProvider`よって値が探索可能になるため、に簡単にアクセスできるようになり、文字列の入力ミスを防ぐことができます。

前の例では`android.provider.ContactsContract` 、クラスが連絡先データのメタデータを公開していました。 カスタム`ContentProvider`の場合は、クラス自体の定数を公開するだけです。


## <a name="implementation"></a>実装

カスタム`ContentProvider`を作成して使用するには、次の3つの手順を実行します。

1. **データベースクラスを作成する**を実装`SQLiteOpenHelper`します。 &ndash;

2. **`ContentProvider`** データベースのインスタンス、 `ContentProvider`定数値として公開されるメタデータ、およびデータにアクセスするためのメソッドを使用して実装クラス&ndash;を作成します。

3. **`ContentProvider`**  Uri を`CursorAdapter`使用してにアクセスし&ndash; 、 `ContentProvider`uri を使用してにアクセスします。

既に説明し`ContentProviders`たように、は、それが定義されている以外のアプリケーションから使用できます。 この例では、データは同じアプリケーションで使用されますが、スキーマに関する Uri と情報がわかっている限り、他のアプリケーションもアクセスできることに注意してください (通常、定数値として公開されます)。


## <a name="create-a-database"></a>データベースを作成する

ほとんど`ContentProvider`の実装は、 `SQLite`データベースに基づいて行われます。 次に示すように、 **SimpleContentProvider/VegetableDatabase**のデータベースコード例では、非常に単純な2列のデータベースが作成されます。

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

データベースの実装自体では`ContentProvider`、には特別な考慮事項は必要ありませんが、 `ContentProvider's`データを`ListView`コントロールにバインドする場合は、という`_id`一意の整数型の列が結果セット。 コントロールの`ListView`使用方法の詳細については、 [listviews とアダプター](~/android/user-interface/layouts/list-view/index.md)のドキュメントを参照してください。


## <a name="create-the-contentprovider"></a>ContentProvider を作成する

このセクションの残りの部分では、 **SimpleContentProvider/VegetableProvider**クラスの作成方法について、手順を追って説明します。


### <a name="initialize-the-database"></a>データベースを初期化します

最初の手順では、 `ContentProvider`をサブクラス化して、使用するデータベースを追加します。

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

コードの残りの部分では、データの検出とクエリの実行を可能にする実際のコンテンツプロバイダーの実装を形成します。



## <a name="add-metadata-for-consumers"></a>コンシューマーのメタデータを追加する

`ContentProvider`クラスで公開するメタデータには、4種類あります。 証明機関のみが必要です。残りは規約によって行われます。

- **権限**クラスに属性を追加して、アプリケーションのインストール時に Android に登録されるように*する必要があり*ます。 `ContentProvider` &ndash;

- **Uri**は、`CONTENT_URI`コードで簡単に使用できるように定数として公開されています。 &ndash; 証明機関と一致している必要がありますが、スキームとベースパスが含まれている必要があります。

- **MIME の種類**&ndash;結果の一覧と1つの結果は異なる種類のコンテンツとして扱われるので、2つの MIME タイプを定義してそれらを表現します。

- **InterfaceConsts**&ndash;データ列名ごとに定数値を指定します。これにより、コードを使用すると、入力ミスを損なうことなく簡単に検出して参照できます。

次のコードは、これらの各項目がどのように実装されているかを示しています。これは、前の手順でデータベース定義に追加したものです。

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


## <a name="implement-the-uri-parsing-helper"></a>URI 解析ヘルパーを実装する

コードを使用する場合は uri を使用し`ContentProvider`ての要求を行うため、返されるデータを決定するためにこれらの要求を解析できる必要があります。 クラス`UriMatcher`は、 `ContentProvider`がサポートする uri パターンで初期化された後、uri の解析に役立ちます。

この`UriMatcher`例のは、次の2つの uri で初期化されます。

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). アスタリスクプレースホルダー ("\*") を使用して、テキストパラメーターを照合することもできます。

コードでは、定数を使用して、オーソリティや基本\_パスなどのメタデータ値を参照します。 リターンコードは、返されるデータを決定するために、Uri 解析を行うメソッドで使用されます。

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

このコードは、すべてクラスに`ContentProvider`対してプライベートです。 詳細については、 [Google の UriMatcher のドキュメント](xref:Android.Content.UriMatcher)を参照してください。


## <a name="implement-the-querymethod"></a>QueryMethod を実装する

実装する`ContentProvider`最も簡単`Query`な方法は、メソッドです。 次の実装では`UriMatcher` 、を使用`uri`してパラメーターを解析し、正しいデータベースメソッドを呼び出します。 に`uri` ID パラメーターが含まれている場合は、(を使用`LastPathSegment`して) 整数が解析され、データベースクエリで使用されます。

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

`GetType`メソッドもオーバーライドする必要があります。 このメソッドは、指定された Uri に対して返されるコンテンツの種類を決定するために呼び出すことができます。
これにより、そのデータの処理方法を使用しているアプリケーションがわかります。

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


## <a name="implement-the-other-overrides"></a>その他のオーバーライドを実装する

単純な例では、データの編集や削除は許可されていませんが、Insert、Update、Delete の各メソッドを実装して実装せずに追加する必要があります。

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

これで、基本的`ContentProvider`な実装が完了します。 アプリケーションがインストールされると、アプリケーション内で公開されているデータは、アプリケーション内でも、参照する Uri を認識している他のアプリケーションにも使用できるようになります。


## <a name="access-the-contentprovider"></a>ContentProvider にアクセスする

`VegetableProvider`が実装されると、このドキュメントの冒頭にある Contacts プロバイダーと同じ方法でアクセスできます。指定した Uri を使用してカーソルを取得し、アダプターを使用してデータにアクセスします。


## <a name="bind-a-listview-to-a-contentprovider"></a>ListView を ContentProvider にバインドする

にデータを`ListView`設定するには、フィルター処理されていない野菜の一覧に対応する Uri を使用します。 コードでは、に`VegetableProvider.CONTENT_URI` `com.xamarin.sample.vegetableprovider/vegetables`解決される定数値を使用します。 この実装では、 `ListView`にバインドできるカーソルが返されます。 `VegetableProvider.Query`

のコード`SimpleContentProvider/HomeScreen.cs`は、 `ContentProvider`からデータを簡単に表示する方法を示しています。

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

結果のアプリケーションは次のようになります。

[![アプリ一覧の野菜、果物、花 Buds、Legumes、電球、Tubers のスクリーンショット](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider から1つの項目を取得する

コンシューマーアプリケーションでは、1行のデータにアクセスすることもできます。これは、特定の行を参照する別の Uri (たとえば) を作成することによって行うことができます。

必要な`Id`を使用して Uri を構築することによって、1つの項目に直接アクセスします。`ContentResolver`

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

完成したメソッドは次のようになります。

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

- [SimpleContentProvider (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
