---
title: カスタム ContentProvider の作成
description: 前のセクションでは、組み込みの ContentProvider 実装からデータを使用する方法を説明しています。 ここでは、カスタム ContentProvider を構築し、そのデータを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3e57e0cd2fa87db8035fa68995b69f231151fa09
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020524"
---
# <a name="creating-a-custom-contentprovider"></a>カスタム ContentProvider の作成

_前のセクションでは、組み込みの ContentProvider 実装からデータを使用する方法を説明しています。ここでは、カスタム ContentProvider を構築し、そのデータを使用する方法について説明します。_

## <a name="about-contentproviders"></a>ContentProviders について

コンテンツプロバイダークラスは `ContentProvider`から継承する必要があります。 これは、クエリに応答するために使用される内部データストアで構成されている必要があります。また、コードを使用してデータの有効な要求を行うために、Uri と MIME の種類を定数として公開する必要があります。

### <a name="uri-authority"></a>URI (機関)

Android では、Uri を使用して `ContentProviders` にアクセスします。 `ContentProvider` を公開するアプリケーションは、 **Androidmanifest .xml**ファイルで応答する uri を設定します。 アプリケーションがインストールされると、他のアプリケーションがアクセスできるように、これらの Uri が登録されます。

Mono for Android では、コンテンツプロバイダークラスには、 **Androidmanifest**に追加する uri (または uri) を指定するための `[ContentProvider]` 属性が必要です。

### <a name="mime-type"></a>Mime の種類

MIME の種類の一般的な形式は、2つの部分で構成されています。 Android `ContentProviders` は、通常、次の2つの文字列を MIME の種類の最初の部分に使用します。

1. 単一行を表す &ndash; `vnd.android.cursor.item` には、コードで `ContentResolver.CursorItemBaseType` 定数を使用します。

1. 複数の行に対して &ndash; を `vnd.android.cursor.dir` には、コードで `ContentResolver.CursorDirBaseType` 定数を使用します。

MIME の種類の2番目の部分はアプリケーションに固有であり、`vnd.` プレフィックスを持つ逆引き DNS 標準を使用する必要があります。 このサンプルコードでは、`vnd.com.xamarin.sample.Vegetables`を使用します。

### <a name="data-model-metadata"></a>データモデルのメタデータ

使用するアプリケーションでは、さまざまな種類のデータにアクセスするために Uri クエリを作成する必要があります。 ベース Uri は、特定のデータテーブルを参照するように拡張できます。また、結果をフィルター処理するためのパラメーターを含めることもできます。 データを表示するために、結果のカーソルで使用される列と句も宣言する必要があります。

有効な Uri クエリのみが構築されるようにするには、有効な文字列を定数値として指定することが慣例です。 これにより、コード補完によって値が探索可能になるため、`ContentProvider` へのアクセスが簡単になります。また、文字列の入力ミスを防ぐことができます。

前の例では、`android.provider.ContactsContract` クラスが連絡先データのメタデータを公開していました。 カスタム `ContentProvider` では、クラス自体に定数を公開するだけです。

## <a name="implementation"></a>実装

カスタム `ContentProvider`を作成して使用するには、次の3つの手順を実行します。

1. `SQLiteOpenHelper`を実装 &ndash;**データベースクラスを作成**します。

2. **`ContentProvider` クラスを作成**し、データベースのインスタンス、定数値として公開されるメタデータ、およびデータにアクセスするメソッドを `ContentProvider` 実装 &ndash; ます。

3. Uri を介してアクセスされる `ContentProvider`を使用して `CursorAdapter` を設定 &ndash; には、その Uri を使用して **`ContentProvider` にアクセスし**ます。

既に説明したように、`ContentProviders` は、アプリケーションが定義されている以外のアプリケーションから使用できます。 この例では、データは同じアプリケーションで使用されますが、スキーマに関する Uri と情報がわかっている限り、他のアプリケーションもアクセスできることに注意してください (通常、定数値として公開されます)。

## <a name="create-a-database"></a>データベースを作成する

ほとんどの `ContentProvider` 実装は、`SQLite` データベースに基づいています。 次に示すように、 **SimpleContentProvider/VegetableDatabase**のデータベースコード例では、非常に単純な2列のデータベースが作成されます。

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

データベース実装自体では、`ContentProvider`で公開する特別な考慮事項は必要ありません。ただし、`ContentProvider's` データを `ListView` コントロールにバインドする場合は、`_id` という名前の一意の整数列を結果セットに含める必要があります。 `ListView` コントロールの使用方法の詳細については、 [Listviews And Adapters](~/android/user-interface/layouts/list-view/index.md)のドキュメントを参照してください。

## <a name="create-the-contentprovider"></a>ContentProvider を作成する

このセクションの残りの部分では、 **SimpleContentProvider/VegetableProvider**クラスの作成方法について、手順を追って説明します。

### <a name="initialize-the-database"></a>データベースを初期化します

最初の手順では、`ContentProvider` をサブクラス化し、使用するデータベースを追加します。

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

`ContentProvider` クラスで公開するメタデータには、4種類あります。 証明機関のみが必要です。残りは規約によって行われます。

- `ContentProvider` 属性 &ndash;**機関**は、アプリケーションのインストール時に Android に登録されるように、クラスに追加する*必要があり*ます。

- **Uri** &ndash; `CONTENT_URI` は、コードで簡単に使用できるように定数として公開されます。 証明機関と一致している必要がありますが、スキームとベースパスが含まれている必要があります。

- 結果の一覧と1つの結果のリスト &ndash; **mime の種類**は、異なるコンテンツの種類として扱われるので、2つの mime タイプを定義します。

- **InterfaceConsts** &ndash; は、データ列名ごとに定数値を指定します。これにより、コードを使用することによって、入力ミスのエラーを発生させることなく簡単に検出して参照できます。

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

コードを使用する場合は、Uri を使用して `ContentProvider`の要求を行うため、返されるデータを決定するためにこれらの要求を解析できる必要があります。 `UriMatcher` クラスは、`ContentProvider` がサポートする Uri パターンで初期化された Uri を解析するのに役立ちます。

この例の `UriMatcher` は、次の2つの Uri で初期化されます。

1. *"VegetableProvider/野菜"* は、野菜の完全なリストを返すように要求 &ndash; ます。

2. *"VegetableProvider/野菜/\#"* &ndash;、\# が数値パラメーター (データベース内の行の `_id`) のプレースホルダーになっています。 アスタリスクプレースホルダー ("\*") を使用して、テキストパラメーターに一致させることもできます。

コードでは、定数を使用して、AUTHORITY や BASE\_PATH などのメタデータ値を参照します。 リターンコードは、返されるデータを決定するために、Uri 解析を行うメソッドで使用されます。

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

このコードは、すべて `ContentProvider` クラスに対してプライベートです。 詳細については、 [Google の UriMatcher のドキュメント](xref:Android.Content.UriMatcher)を参照してください。

## <a name="implement-the-querymethod"></a>QueryMethod を実装する

実装する最も簡単な `ContentProvider` メソッドは、`Query` メソッドです。 次の実装では、`UriMatcher` を使用して `uri` パラメーターを解析し、正しいデータベースメソッドを呼び出します。 `uri` に ID パラメーターが含まれている場合、整数は (`LastPathSegment`を使用して) 解析され、データベースクエリで使用されます。

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

`GetType` メソッドもオーバーライドする必要があります。 このメソッドは、指定された Uri に対して返されるコンテンツの種類を決定するために呼び出すことができます。
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

これで、基本的な `ContentProvider` の実装が完了します。 アプリケーションがインストールされると、アプリケーション内で公開されているデータは、アプリケーション内でも、参照する Uri を認識している他のアプリケーションにも使用できるようになります。

## <a name="access-the-contentprovider"></a>ContentProvider にアクセスする

`VegetableProvider` が実装されると、このドキュメントの冒頭にある Contacts プロバイダーと同じ方法でアクセスできます。指定した Uri を使用してカーソルを取得し、アダプターを使用してデータにアクセスします。

## <a name="bind-a-listview-to-a-contentprovider"></a>ListView を ContentProvider にバインドする

`ListView` にデータを設定するには、フィルター処理されていない野菜の一覧に対応する Uri を使用します。 コードでは、`com.xamarin.sample.vegetableprovider/vegetables`に解決されることがわかっている `VegetableProvider.CONTENT_URI`定数値を使用します。 `VegetableProvider.Query` の実装では、`ListView`にバインドできるカーソルが返されます。

`SimpleContentProvider/HomeScreen.cs` のコードは、`ContentProvider`のデータを簡単に表示する方法を示しています。

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

[アプリ一覧の野菜、果物、花 Buds、Legumes、電球、Tubers のスクリーンショット![](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider から1つの項目を取得する

コンシューマーアプリケーションでは、1行のデータにアクセスすることもできます。これは、特定の行を参照する別の Uri (たとえば) を作成することによって行うことができます。

必要な `Id`を含む Uri を構築することによって、`ContentResolver` を直接使用して1つの項目にアクセスします。

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
