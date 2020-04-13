---
title: Xamarin.Forms のローカル データベース
description: Xamarin.Forms では、SQLite データベース エンジンを使ったデータベース駆動型アプリケーションがサポートされています。これにより、共有コードでのオブジェクトの読み込みと保存が可能になります。 この記事では、Xamarin.Forms アプリケーションで SQLite.Net を使用して、ローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 2369afc693940d83971a43877da363e2c66b31b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992397"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms のローカル データベース

[![サンプルの](~/media/shared/download.png)ダウンロード サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

SQLite データベース エンジンを使用すると、Xamarin.Forms アプリケーションは、共有コード内のデータ オブジェクトを読み込み、保存できます。 サンプル アプリケーションでは、SQLite データベース テーブルを使用して todo 項目を格納します。 この資料では、共有コードでSQLite.Netを使用してローカル データベースに情報を格納および取得する方法について説明します。

[![iOS と Android の Todolist アプリのスクリーンショット](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "iOSとアンドロイド上のトドリストアプリ")

次の手順に従って、SQLite.NETをモバイル アプリに統合します。

1. [NuGet パッケージをインストール](#install-the-sqlite-nuget-package)する 。
1. [定数を構成する](#configure-app-constants):
1. [データベース アクセス クラス を作成する](#create-a-database-access-class)。
1. [Xamarin.Forms でデータにアクセスします](#access-data-in-xamarinforms)。
1. [詳細な構成](#advanced-configuration):

## <a name="install-the-sqlite-nuget-package"></a>SQLite の NuGet パッケージをインストールします。

NuGet パッケージ マネージャーを使用して**sqlite-net-pcl**を検索し、最新バージョンを共有コード プロジェクトに追加します。

類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。

- **作成者:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> パッケージ名に関係なく、**sqlite-net-pcl** NuGet パッケージを .NET Standard プロジェクトでも使用します。

## <a name="configure-app-constants"></a>アプリの定数を構成する

サンプル プロジェクトには、共通の構成データを提供する**Constants.cs**ファイルが含まれています。

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

定数ファイルは、データベース接続`SQLiteOpenFlag`の初期化に使用される既定の列挙値を指定します。 列挙`SQLiteOpenFlag`型では、次の値がサポートされます。

- `Create`: データベース ファイルが存在しない場合、接続によって自動的に作成されます。
- `FullMutex`: 接続はシリアル化されたスレッド モードで開かれます。
- `NoMutex`: 接続がマルチスレッド モードで開かれます。
- `PrivateCache`: 接続が有効な場合でも、接続は共有キャッシュに参加しません。
- `ReadWrite`: 接続はデータの読み取りと書き込みが可能です。
- `SharedCache`: 接続が有効な場合、接続は共有キャッシュに参加します。
- `ProtectionComplete`: デバイスがロックされている間、ファイルは暗号化され、アクセスできません。
- `ProtectionCompleteUnlessOpen`: ファイルは開かれるまで暗号化されますが、ユーザーがデバイスをロックしてもアクセス可能です。
- `ProtectionCompleteUntilFirstUserAuthentication`: ユーザーがデバイスを起動してロックを解除するまで、ファイルは暗号化されます。
- `ProtectionNone`: データベース ファイルは暗号化されていません。

データベースの使用方法に応じて、異なるフラグを指定する必要がある場合があります。 詳細については、「sqlite.org`SQLiteOpenFlags`[で新しいデータベース接続を開く](https://www.sqlite.org/c3ref/open.html)」を参照してください。

## <a name="create-a-database-access-class"></a>データベース アクセス クラスの作成

データベース ラッパー クラスは、アプリの残りの部分からデータ アクセス層を抽象化します。 このクラスは、クエリ ロジックを一元化し、データベースの初期化の管理を簡略化します。 Todo アプリは、`TodoItemDatabase`この目的のためにクラスを定義します。

### <a name="lazy-initialization"></a>限定的な初期化

は`TodoItemDatabase`、.NET`Lazy`クラスを使用して、データベースが最初にアクセスされるまで、データベースの初期化を遅延します。 遅延初期化を使用すると、データベースの読み込みプロセスがアプリの起動を遅らせなくなります。 詳細については、「[遅延&lt;T&gt;クラス](xref:System.Lazy`1)」を参照してください。

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
                initialized = true;
            }
        }
    }

    //...
}
```

データベース接続は静的フィールドであり、単一のデータベース接続がアプリの存続時間に使用されることを保証します。 永続的な静的接続を使用すると、1 つのアプリ セッション中に接続を複数回開いたり閉じたりするよりもパフォーマンスが向上します。

メソッド`InitializeAsync`は、オブジェクトを格納`TodoItem`するためのテーブルが既に存在するかどうかをチェックします。 このメソッドは、テーブルが存在しない場合は自動的に作成されます。

### <a name="the-safefireandforget-extension-method"></a>セーフファイアアンドフォーゲット拡張メソッド

クラスが`TodoItemDatabase`インスタンス化されるときに、非同期プロセスであるデータベース接続を初期化する必要があります。 ただし、

- クラス コンストラクターを非同期にすることはできません。
- 待ち受けていない非同期メソッドは例外をスローしません。
- このメソッド`Wait`を使用すると、スレッドがブロック_され、_ 例外が取り込まれます。

非同期初期化を開始し、実行をブロックしないようにし、例外をキャッチする機会を得るために、サンプル アプリケーションは という`SafeFireAndForget`拡張メソッドを使用します。 拡張`SafeFireAndForget`メソッドは、クラスに追加機能`Task`を提供します。

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

メソッド`SafeFireAndForget`は、指定された`Task`オブジェクトの非同期実行を待機し、例外`Action`がスローされた場合に呼び出されるをアタッチできます。

詳しくは、[タスク・ベースの非同期パターン (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)を参照してください。

### <a name="data-manipulation-methods"></a>データ操作メソッド

この`TodoItemDatabase`クラスには、作成、読み取り、編集、および削除の 4 種類のデータ操作のメソッドが含まれています。 SQLite.NETライブラリは、SQL ステートメントを記述せずにオブジェクトを格納および取得できる単純なオブジェクト リレーショナル マップ (ORM) を提供します。

```csharp
public class TodoItemDatabase {

    // ...

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## <a name="access-data-in-xamarinforms"></a>Xamarin.Forms でデータにアクセスする

Xamarin.Forms`App`クラスは、クラスのインスタンスを`TodoItemDatabase`公開します。

```csharp
static TodoItemDatabase database;
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

このプロパティを使用すると、Xamarin.Forms コンポーネントは、ユーザーの操作`Database`に応答して、インスタンスのデータ取得および操作メソッドを呼び出すことができます。 次に例を示します。

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>詳細な構成

SQLite は、この記事とサンプル アプリで説明されている機能よりも多くの機能を備えた堅牢な API を提供します。 以下のセクションでは、スケーラビリティにとって重要な機能について説明します。

詳細については、sqlite.orgの[SQLite ドキュメントを参照してください](https://www.sqlite.org/docs.html)。

### <a name="write-ahead-logging"></a>先行書き込みログ

デフォルトでは、SQLite は従来のロールバック・ジャーナルを使用します。 変更されていないデータベースコンテンツのコピーが別のロールバックファイルに書き込まれ、その後、変更がデータベースファイルに直接書き込まれます。 COMMIT は、ロールバック・ジャーナルが削除されるときに発生します。

先行書き込みログ (WAL) は、最初に別の WAL ファイルに変更を書き込みます。 WAL モードでは、COMMIT は特別なレコードであり、WAL ファイルに追加され、単一の WAL ファイルで複数のトランザクションが発生することを可能にします。 WAL ファイルは _、チェックポイント_と呼ばれる特別な操作でデータベース ファイルにマージされます。

WAL は、読み取り操作と書き込み操作を同時に実行できるように、リーダーとライターが互いにブロックしないため、ローカル データベースの方が高速です。 ただし、WAL モードでは_ページ サイズ_の変更が許可されず、ファイルの関連付けがデータベースに追加され、_チェックポイント操作が_追加されます。

SQLite.NETで WAL を有効にするには`EnableWriteAheadLoggingAsync`、インスタンスで`SQLiteAsyncConnection`メソッドを呼び出します。

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

詳細については[、「sqlite.orgでの SQLite 書き込み先書きログ」](https://www.sqlite.org/wal.html)を参照してください。

### <a name="copying-a-database"></a>データベースのコピー

SQLite データベースのコピーが必要になる場合があります。

- データベースはアプリケーションに付属していますが、モバイル デバイス上の書き込み可能ストレージにコピーまたは移動する必要があります。
- データベースのバックアップまたはコピーを作成する必要があります。
- データベース ファイルのバージョン、移動、または名前の変更を行う必要があります。

一般に、データベース ファイルの移動、名前の変更、またはコピーは、他のファイルの種類と同じプロセスで、さらにいくつかの考慮事項があります。

- データベース ファイルを移動する前に、すべてのデータベース接続を閉じる必要があります。
- [先行書き込みログ](#write-ahead-logging)を使用する場合、SQLite は共有メモリ アクセス (.shm) ファイルと (先行ログ) (.wal) ファイルを作成します。 これらのファイルにも変更を適用してください。

詳細については、「 [Xamarin.Forms でのファイル処理](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Todo サンプル アプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet パッケージ](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite のドキュメント](https://www.sqlite.org/docs.html)
- [アンドロイドでのSQLiteの使用](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [iOS での SQLite の使用](~/ios/data-cloud/data/using-sqlite-orm.md)
- [タスク・ベースの非同期パターン (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [レイ&lt;ジー&gt; Tクラス](xref:System.Lazy`1)
