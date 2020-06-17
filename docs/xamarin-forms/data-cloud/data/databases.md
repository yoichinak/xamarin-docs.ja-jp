---
title: " Xamarin.Forms Local Databases" description: " Xamarin.Forms は、SQLite データベースエンジンを使用したデータベース駆動型アプリケーションをサポートします。これにより、共有コードでオブジェクトを読み込んで保存できるようになります。 この記事で Xamarin.Forms は、SQLite.Net を使用して、アプリケーションがローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
ms. 製品: xamarin ms. assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB: xamarin-forms author: profexorgeek ms. author: jusjohns ms. date: 12/05/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-local-databases"></a>Xamarin.Formsローカルデータベース

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

SQLite データベースエンジンを使用 Xamarin.Forms すると、アプリケーションは共有コードでデータオブジェクトを読み込んで保存することができます。 このサンプルアプリケーションでは、SQLite データベーステーブルを使用して todo 項目を格納します。 この記事では、共有コードで SQLite.Net を使用して、ローカルデータベースの情報を格納および取得する方法について説明します。

[![IOS と Android の Todolist アプリのスクリーンショット](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "IOS および Android での Todolist アプリ")

SQLite.NET をモバイルアプリに統合するには、次の手順を実行します。

1. [NuGet パッケージをインストール](#install-the-sqlite-nuget-package)します。
1. [定数を構成](#configure-app-constants)します。
1. [データベースアクセスクラスを作成](#create-a-database-access-class)します。
1. [でデータに Xamarin.Forms アクセス](#access-data-in-xamarinforms)します。
1. [詳細構成](#advanced-configuration)。

## <a name="install-the-sqlite-nuget-package"></a>SQLite NuGet パッケージをインストールする

NuGet パッケージマネージャーを使用して、 **sqlite-pcl**を検索し、共有コードプロジェクトに最新バージョンを追加します。

類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。

- **作成者:** Frank A. Krueger (praeclarum)
- **ID:** sqlite-net-pcl
- **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> パッケージ名に関係なく、**sqlite-net-pcl** NuGet パッケージを .NET Standard プロジェクトでも使用します。

## <a name="configure-app-constants"></a>アプリ定数の構成

サンプルプロジェクトには、一般的な構成データを提供する**Constants.cs**ファイルが含まれています。

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

定数ファイルは、 `SQLiteOpenFlag` データベース接続を初期化するために使用される既定の列挙値を指定します。 列挙型では、次の値がサポートされて `SQLiteOpenFlag` います。

- `Create`: データベースファイルが存在しない場合は、接続によって自動的に作成されます。
- `FullMutex`: シリアル化されたスレッドモードで接続が開かれています。
- `NoMutex`: 接続はマルチスレッドモードで開かれています。
- `PrivateCache`: 接続は、有効になっている場合でも、共有キャッシュに参加しません。
- `ReadWrite`: 接続でデータの読み取りと書き込みを行うことができます。
- `SharedCache`: 接続は、有効になっている場合、共有キャッシュに参加します。
- `ProtectionComplete`: デバイスがロックされている間、ファイルは暗号化され、アクセスできません。
- `ProtectionCompleteUnlessOpen`: ファイルは、開かれるまで暗号化されますが、ユーザーがデバイスをロックしている場合でもアクセスできます。
- `ProtectionCompleteUntilFirstUserAuthentication`: ファイルは、ユーザーがデバイスを起動してロックを解除した後に暗号化されます。
- `ProtectionNone`: データベースファイルは暗号化されていません。

データベースの使用方法に応じて、異なるフラグを指定することが必要になる場合があります。 の詳細については `SQLiteOpenFlags` 、「sqlite.org で[新しいデータベース接続を開く](https://www.sqlite.org/c3ref/open.html)」を参照してください。

## <a name="create-a-database-access-class"></a>データベースアクセスクラスを作成する

データベースラッパークラスは、アプリの他の部分からデータアクセス層を抽象化します。 このクラスは、クエリロジックを一元化し、データベースの初期化の管理を簡略化します。これにより、アプリの拡大に伴ってデータ操作のリファクタリングや拡張が容易になります。 Todo アプリは、 `TodoItemDatabase` この目的のためにクラスを定義します。

### <a name="lazy-initialization"></a>限定的な初期化

は、 `TodoItemDatabase` 最初にアクセスされるまで、.net クラスを使用して `Lazy` データベースの初期化を遅延させます。 遅延初期化を使用すると、データベースの読み込みプロセスによってアプリの起動が遅れることがなくなります。 詳細については、「 [Lazy &lt; T &gt; クラス](xref:System.Lazy`1)」を参照してください。

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

データベース接続は静的なフィールドで、アプリケーションの有効期間中は1つのデータベース接続が使用されます。 永続的な静的接続を使用すると、単一のアプリセッション中に複数回接続を開始したり閉じたりするよりもパフォーマンスが向上します。

`InitializeAsync`メソッドは、オブジェクトを格納するテーブルが既に存在するかどうかを確認し `TodoItem` ます。 このメソッドでは、テーブルが存在しない場合は自動的に作成されます。

### <a name="the-safefireandforget-extension-method"></a>Safe焼討 And忘れる拡張メソッド

`TodoItemDatabase`クラスがインスタンス化されるときは、非同期プロセスであるデータベース接続を初期化する必要があります。 ただし、

- クラスコンストラクターを非同期にすることはできません。
- 待機されていない非同期メソッドは、例外をスローしません。
- メソッドを使用する `Wait` と、スレッド_と_飲み込まよって例外が許可されます。

非同期の初期化を開始するために、実行のブロックを避け、例外をキャッチする機会がある場合、サンプルアプリケーションではという拡張メソッドを使用し `SafeFireAndForget` ます。 `SafeFireAndForget`拡張メソッドは、クラスに追加の機能を提供し `Task` ます。

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

メソッドは、 `SafeFireAndForget` 指定されたオブジェクトの非同期実行を待機 `Task` し、 `Action` 例外がスローされた場合に呼び出されるをアタッチできるようにします。

詳細については、「[タスクベースの非同期パターン (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)」を参照してください。

### <a name="data-manipulation-methods"></a>データ操作方法

クラスには、 `TodoItemDatabase` 作成、読み取り、編集、および削除の4種類のデータ操作のためのメソッドが含まれています。 SQLite.NET ライブラリには、SQL ステートメントを記述せずにオブジェクトを格納および取得するための単純なオブジェクトリレーショナルマップ (ORM) が用意されています。

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

## <a name="access-data-in-xamarinforms"></a>データへのアクセスXamarin.Forms

クラスは、 Xamarin.Forms `App` クラスのインスタンスを公開し `TodoItemDatabase` ます。

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

このプロパティを使用 Xamarin.Forms すると、コンポーネントは、 `Database` ユーザーの操作に応じてインスタンスのデータの取得と操作のメソッドを呼び出すことができます。 次に例を示します。

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

SQLite は、この記事とサンプルアプリに記載されているよりも多くの機能を備えた堅牢な API を提供します。 以下のセクションでは、スケーラビリティにとって重要な機能について説明します。

詳細については、sqlite.org に関する[SQLite のドキュメント](https://www.sqlite.org/docs.html)を参照してください。

### <a name="write-ahead-logging"></a>先行書き込みログ

既定では、SQLite は従来の rollback ジャーナルを使用します。 変更されていないデータベースの内容のコピーが別のロールバックファイルに書き込まれ、変更がデータベースファイルに直接書き込まれます。 このコミットは、ロールバックジャーナルが削除されたときに発生します。

先行書き込みログ (WAL) は、最初に個別の WAL ファイルに変更を書き込みます。 WAL モードでは、コミットは特殊なレコードで、WAL ファイルに追加されます。これにより、1つの WAL ファイルで複数のトランザクションを実行できます。 WAL ファイルは、_チェックポイント_と呼ばれる特殊な操作でデータベースファイルにマージされます。

読み取りと書き込みの操作を同時に行うことができないため、ローカルデータベースでは WAL が高速になります。 ただし、WAL モードでは、_ページサイズ_を変更することはできません。また、データベースにファイルの関連付けを追加し、追加の_チェックポイント_操作を追加します。

SQLite.NET で WAL を有効にするには、 `EnableWriteAheadLoggingAsync` インスタンスでメソッドを呼び出し `SQLiteAsyncConnection` ます。

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

詳細については、「sqlite.org での[SQLite 書き込みログ](https://www.sqlite.org/wal.html)」を参照してください。

### <a name="copying-a-database"></a>データベースのコピー

SQLite データベースのコピーが必要になる場合があります。

- データベースはアプリケーションに付属していましたが、モバイルデバイスの書き込み可能ストレージにコピーまたは移動する必要があります。
- データベースのバックアップまたはコピーを作成する必要があります。
- データベースファイルのバージョン、移動、または名前変更を行う必要があります。

一般に、データベースファイルの移動、名前の変更、またはコピーは、他のいくつかの考慮事項がある他のファイルの種類と同じです。

- データベースファイルの移動を試行する前に、すべてのデータベース接続を閉じる必要があります。
- [先行書き込みログ](#write-ahead-logging)を使用する場合、SQLite は共有メモリアクセス (shm) ファイルと (先行書き込みログ) (wal) ファイルを作成します。 これらのファイルにも変更を適用してください。

詳細については、「 [」 Xamarin.Forms の「ファイル処理](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Todo サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet パッケージ](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite のドキュメント](https://www.sqlite.org/docs.html)
- [Android での SQLite の使用](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [IOS での SQLite の使用](~/ios/data-cloud/data/using-sqlite-orm.md)
- [タスク ベースの非同期パターン (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Lazy &lt; T &gt; クラス](xref:System.Lazy`1)
