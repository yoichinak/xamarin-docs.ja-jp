---
title: カスタム ContentProvider の作成
description: 前のセクションでは、組み込みの ContentProvider の実装からデータを使用する方法を説明しました。 このセクションでは、カスタム ContentProvider を構築して、そのデータを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3e57e0cd2fa87db8035fa68995b69f231151fa09
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020524"
---
# <a name="creating-a-custom-contentprovider"></a>カスタム ContentProvider の作成

_前のセクションでは、組み込みの ContentProvider の実装からデータを使用する方法を説明しました。このセクションでは、カスタム ContentProvider を構築して、そのデータを使用する方法について説明します。_

## <a name="about-contentproviders"></a>ContentProviders について

コンテンツ プロバイダー クラスは `ContentProvider` から継承する必要があります。 クエリに応答するために使用される内部データ ストアで構成されている必要があり、使用するコードでデータに対する有効な要求を行うことができるように、URI と MIME の種類を定数として公開する必要があります。

### <a name="uri-authority"></a>URI (機関)

Android では、URI を使用して `ContentProviders` にアクセスします。 `ContentProvider` を公開するアプリケーションでは、それが応答する URI を、**AndroidManifest.xml** ファイルで設定します。 アプリケーションがインストールされると、他のアプリケーションからアクセスできるように、これらの URI が登録されます。

Mono for Android では、**AndroidManifest.xml** に追加する URI (1 つまたは複数) を指定するための `[ContentProvider]` 属性が、コンテンツ プロバイダー クラスに必要です。

### <a name="mime-type"></a>MIME の種類

MIME の種類の一般的な形式は、2 つの部分で構成されます。 Android の `ContentProviders` では、通常、次の 2 つの文字列が MIME の種類の最初の部分に対して使用されます。

1. `vnd.android.cursor.item` &ndash; 1 つの行を表すには、`ContentResolver.CursorItemBaseType` 定数をコードで使用します。

1. `vnd.android.cursor.dir` &ndash; 複数の行の場合は、`ContentResolver.CursorDirBaseType` 定数をコードで使用します。

MIME の種類の 2 番目の部分はアプリケーションに固有であり、`vnd.` プレフィックスの付いた逆引き DNS 標準を使用する必要があります。 サンプル コードでは `vnd.com.xamarin.sample.Vegetables` が使用されています。

### <a name="data-model-metadata"></a>データ モデルのメタデータ

コンシューマー アプリケーションでは、さまざまな種類のデータにアクセスするために URI クエリを作成する必要があります。 基本 URI を拡張して、特定のデータ テーブルを参照することができ、結果をフィルター処理するためのパラメーターを含めることもできます。 データを表示するために結果のカーソルで使用する列と句も宣言する必要があります。

有効な URI クエリのみが構築されるようにするため、有効な文字列を定数値として指定することが慣例です。 このようにすると、コード補完によって値が探索可能になるため `ContentProvider` へのアクセスが簡単になり、文字列の入力ミスを防ぐことができます。

前の例では、`android.provider.ContactsContract` クラスによって連絡先データのメタデータが公開されています。 このカスタム `ContentProvider` では、クラス自体で定数を公開するだけです。

## <a name="implementation"></a>実装

カスタム `ContentProvider` を作成して使用するには、3 つのステップがあります。

1. **データベースクラスを作成する** &ndash; `SQLiteOpenHelper` を実装します。

2. **`ContentProvider` クラスを作成する** &ndash; データベースのインスタンス、定数値として公開されているメタデータ、およびデータにアクセスするためのメソッドを使用して、`ContentProvider` を実装します。

3. **URI を使用して`ContentProvider` にアクセスする** &ndash; URI を介してアクセスされる `ContentProvider` を使用して `CursorAdapter` を設定します。

前に説明したように、`ContentProviders` は、それが定義されている以外のアプリケーションから使用できます。 この例では、データは同じアプリケーションで使用されますが、スキーマに関する URI と情報 (通常、定数値として公開されます) がわかっている限り、他のアプリケーションでもデータにアクセスできることに注意してください。

## <a name="create-a-database"></a>データベースを作成する

ほとんどの `ContentProvider` の実装は、`SQLite` データベースが基になっています。 次に示すように、**SimpleContentProvider/VegetableDatabase.cs** のデータベース コードの例では、非常に簡単な 2 列のデータベースが作成されます。

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

データベースの実装自体については、`ContentProvider` で特別な考慮事項を公開する必要はありませんが、`ContentProvider's` データを `ListView` コントロールにバインドする場合は、`_id` という名前の一意の整数列を結果セットに含める必要があります。 `ListView` コントロールの使用について詳しくは、[ListView とアダプター](~/android/user-interface/layouts/list-view/index.md)に関するドキュメントを参照してください。

## <a name="create-the-contentprovider"></a>ContentProvider を作成する

このセクションの残りの部分では、**SimpleContentProvider/VegetableProvider.cs** の例のクラスが作成された方法について、手順を追って説明します。

### <a name="initialize-the-database"></a>データベースを初期化する

最初のステップでは、`ContentProvider` をサブクラス化し、それが使用するデータベースを追加します。

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

コードの残りの部分では、データの検出とクエリの実行を可能にする実際のコンテンツ プロバイダーの実装が形成されます。

## <a name="add-metadata-for-consumers"></a>コンシューマーのためのメタデータを追加する

`ContentProvider` クラスでは、4 種類のメタデータを公開します。 機関のみが必須です。残りは慣例によって行われます。

- **機関** &ndash; アプリケーションのインストール時に Android に登録されるように、`ContentProvider` 属性をクラスに追加する "*必要があります*"。

- **URI** &ndash; コードで使いやすいように、`CONTENT_URI` は定数として公開されます。 証明機関と一致している必要がありますが、スキームとベース パスが含まれます。

- **MIME の種類** &ndash; 結果のリストと 1 つの結果は異なるコンテンツの種類として扱われるので、2 つの MIME の種類を定義します。

- **InterfaceConsts** &ndash; 使用するコードで入力ミスのおそれなしに簡単に検出して参照できるように、データ列名ごとに定数値を指定します。

次のコードでは、前のステップのデータベース定義に追加して、これらの各項目の実装方法を示します。

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

使用するコードでは、URI を使用して `ContentProvider` の要求を行うため、返すデータを決定するためにそれらの要求を解析できる必要があります。 `UriMatcher` クラスは、`ContentProvider` でサポートされる URI パターンで初期化すると、URI を解析するのに役立ちます。

例の `UriMatcher` は、2 つの URI で初期化されます。

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; 野菜の完全な一覧を返すよう要求します。

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; \# は数値パラメーターのプレースホルダーです (データベース内の行の `_id`)。 アスタリスク プレースホルダー ("\*") を使用して、テキスト パラメーターと一致させることもできます。

コードでは、定数を使用して、AUTHORITY や BASE\_PATH などのメタデータ値を参照します。 リターン コードは、返すデータを決定するために、URI の解析を行うメソッドで使用されます。

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

このコードはすべて、`ContentProvider` クラスに対してプライベートです。 詳しくは、[Google の UriMatcher のドキュメント](xref:Android.Content.UriMatcher)を参照してください。

## <a name="implement-the-querymethod"></a>QueryMethod を実装する

実装するのが最も簡単な `ContentProvider` メソッドは、`Query` メソッドです。 次の実装では、`UriMatcher` を使用して `uri` パラメーターを解析し、正しいデータベース メソッドを呼び出しています。 `uri` に ID パラメーターが含まれている場合は、整数が (`LastPathSegment` を使用して) 解析され、データベース クエリで使用されます。

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

`GetType` メソッドもオーバーライドする必要があります。 このメソッドを呼び出して、指定された URI に対して返されるコンテンツの種類を決定できます。
これにより、そのデータの処理方法がコンシューマー アプリケーションに伝えられます。

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

## <a name="implement-the-other-overrides"></a>他のオーバーライドを実装する

この簡単な例では、データを編集または削除することはできませんが、Insert、Update、Delete の各メソッドを実装して、実装せずに追加する必要があります。

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

これで、基本的な `ContentProvider` の実装が完了します。 アプリケーションがインストールされると、アプリケーションで公開されているデータが、アプリケーション内と、参照するための URI がわかっている他のアプリケーションの両方で、使用できるようになります。

## <a name="access-the-contentprovider"></a>ContentProvider にアクセスする

`VegetableProvider` を実装したら、このドキュメントの冒頭にある Contacts プロバイダーと同じ方法でアクセスできます。指定された URI を使用してカーソルを取得し、アダプターを使用してデータにアクセスします。

## <a name="bind-a-listview-to-a-contentprovider"></a>ListView を ContentProvider にバインドする

`ListView` にデータを設定するには、野菜のフィルター処理されていない一覧に対応する URI を使用します。 コードでは、`com.xamarin.sample.vegetableprovider/vegetables` に解決されることがわかっている `VegetableProvider.CONTENT_URI` 定数値を使用します。 `VegetableProvider.Query` の実装では、`ListView` にバインドできるカーソルが返されます。

`SimpleContentProvider/HomeScreen.cs` のコードでは、`ContentProvider` のデータを簡単に表示できることがわかります。

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

[![Vegetables、Fruits、Flower Buds、Legumes、Bulbs、Tubers が一覧表示されているアプリのスクリーンショット](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider から 1 つの項目を取得する

コンシューマー アプリケーションでは、1 行のデータにアクセスしたいことがあります。これは、特定の行を参照する別の URI を作成することによって行うことができます。

必要な `Id` を含む URI を作成することにより、`ContentResolver` を直接使用して 1 つの項目にアクセスします。

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

完全なメソッドは次のようになります。

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
