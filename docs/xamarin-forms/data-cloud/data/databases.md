---
title: Xamarin.Forms のローカル データベース
description: Xamarin.Forms では、SQLite データベース エンジンを使ったデータベース駆動型アプリケーションがサポートされています。これにより、共有コードでのオブジェクトの読み込みと保存が可能になります。 この記事では、Xamarin.Forms アプリケーションで SQLite.Net を使用して、ローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 52b227b0244a83ec4a7466cca7591c6b712f1c76
ms.sourcegitcommit: dde593cf9dedf4a056ffef86bcf2fa0640412a4d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794668"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms のローカル データベース

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

SQLite データベースエンジンを使用すると、Xamarin アプリケーションは、共有コードでデータオブジェクトを読み込んで保存することができます。 このサンプルアプリケーションでは、SQLite データベーステーブルを使用して todo 項目を格納します。 この記事では、共有コードで SQLite.Net を使用して、ローカルデータベースの情報を格納および取得する方法について説明します。

[iOS および Android での Todolist アプリのスクリーンショットの ![](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "IOS および Android での Todolist アプリ")

SQLite.NET をモバイルアプリに統合するには、次の手順を実行します。

1. [NuGet パッケージをインストール](#install-the-sqlite-nuget-package)します。
1. [定数を構成](#configure-app-constants)します。
1. [データベースアクセスクラスを作成](#create-a-database-access-class)します。
1. [Xamarin. フォームのデータにアクセス](#access-data-in-xamarinforms)します。
1. [詳細構成](#advanced-configuration)。

## <a name="install-the-sqlite-nuget-package"></a>SQLite NuGet パッケージをインストールする

NuGet パッケージマネージャーを使用して、 **sqlite-pcl**を検索し、共有コードプロジェクトに最新バージョンを追加します。

類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。

- **作成者:** Frank A. Krueger
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

定数ファイルは、データベース接続を初期化するために使用される既定の `SQLiteOpenFlag` 列挙値を指定します。 `SQLiteOpenFlag` 列挙型では、次の値がサポートされます。

- `Create`: データベースファイルが存在しない場合、接続によって自動的に作成されます。
- `FullMutex`: 接続はシリアル化されたスレッドモードで開かれています。
- `NoMutex`: 接続はマルチスレッドモードで開かれています。
- `PrivateCache`: 接続は、有効になっている場合でも、共有キャッシュに参加しません。
- `ReadWrite`: 接続でデータの読み取りと書き込みを行うことができます。
- `SharedCache`: 接続は、有効になっている場合、共有キャッシュに参加します。
- `ProtectionComplete`: デバイスがロックされている間、ファイルは暗号化され、アクセスできません。
- `ProtectionCompleteUnlessOpen`: ファイルは開かれるまで暗号化されますが、ユーザーがデバイスをロックしている場合でもアクセスできます。
- `ProtectionCompleteUntilFirstUserAuthentication`: ファイルは、ユーザーがデバイスを起動してロックを解除した後に暗号化されます。
- `ProtectionNone`: データベースファイルが暗号化されていません。

データベースの使用方法に応じて、異なるフラグを指定することが必要になる場合があります。 `SQLiteOpenFlags`の詳細については、「sqlite.org で[新しいデータベース接続を開く](https://www.sqlite.org/c3ref/open.html)」を参照してください。

## <a name="create-a-database-access-class"></a>データベースアクセスクラスを作成する

データベースラッパークラスは、アプリの他の部分からデータアクセス層を抽象化します。 このクラスは、クエリロジックを一元化し、データベースの初期化の管理を簡略化します。これにより、アプリの拡大に伴ってデータ操作のリファクタリングや拡張が容易になります。 Todo アプリは、この目的のために `TodoItemDatabase` クラスを定義します。

### <a name="lazy-initialization"></a>限定的な初期化

`TodoItemDatabase` は、最初にアクセスされるまで、.NET `Lazy` クラスを使用してデータベースの初期化を遅延させます。 遅延初期化を使用すると、データベースの読み込みプロセスによってアプリの起動が遅れることがなくなります。 詳細については、「 [Lazy&lt;t&gt; クラス](xref:System.Lazy`1)」を参照してください。

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

`InitializeAsync` メソッドは、`TodoItem` オブジェクトを格納するテーブルが既に存在するかどうかを確認します。 このメソッドでは、テーブルが存在しない場合は自動的に作成されます。

### <a name="the-safefireandforget-extension-method"></a>Safe焼討 And忘れる拡張メソッド

`TodoItemDatabase` クラスがインスタンス化されるときには、データベース接続を初期化する必要があります。これは、非同期プロセスです。 ただし、

- クラスコンストラクターを非同期にすることはできません。
- 待機されていない非同期メソッドは、例外をスローしません。
- `Wait` メソッドを使用する_と_、スレッドがブロックされ、飲み込ま例外が許可されます。

非同期初期化を開始するために、実行をブロックしないようにして、例外をキャッチできるようにするために、サンプルアプリケーションは `SafeFireAndForget`と呼ばれる拡張メソッドを使用します。 `SafeFireAndForget` 拡張メソッドは、`Task` クラスに追加機能を提供します。

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

`SafeFireAndForget` メソッドは、指定された `Task` オブジェクトの非同期実行を待機し、例外がスローされた場合に呼び出される `Action` をアタッチできるようにします。

詳細については、「[タスクベースの非同期パターン (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)」を参照してください。

### <a name="data-manipulation-methods"></a>データ操作方法

`TodoItemDatabase` クラスには、作成、読み取り、編集、および削除の4種類のデータ操作のためのメソッドが含まれています。 SQLite.NET ライブラリには、SQL ステートメントを記述せずにオブジェクトを格納および取得するための単純なオブジェクトリレーショナルマップ (ORM) が用意されています。

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

## <a name="access-data-in-xamarinforms"></a>Xamarin のデータにアクセスする

Xamarin `App` クラスは、`TodoItemDatabase` クラスのインスタンスを公開します。

```csharp
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

このプロパティを使用すると、ユーザーの操作に応じて、`Database` インスタンスでデータの取得と操作のメソッドを呼び出すことができます。 例:

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>詳細構成

SQLite は、この記事とサンプルアプリに記載されているよりも多くの機能を備えた堅牢な API を提供します。 以下のセクションでは、スケーラビリティにとって重要な機能について説明します。

詳細については、sqlite.org に関する[SQLite のドキュメント](https://www.sqlite.org/docs.html)を参照してください。

### <a name="write-ahead-logging"></a>先行書き込みログ

既定では、SQLite は従来の rollback ジャーナルを使用します。 変更されていないデータベースの内容のコピーが別のロールバックファイルに書き込まれ、変更がデータベースファイルに直接書き込まれます。 このコミットは、ロールバックジャーナルが削除されたときに発生します。

先行書き込みログ (WAL) は、最初に個別の WAL ファイルに変更を書き込みます。 WAL モードでは、コミットは特殊なレコードで、WAL ファイルに追加されます。これにより、1つの WAL ファイルで複数のトランザクションを実行できます。 WAL ファイルは、_チェックポイント_と呼ばれる特殊な操作でデータベースファイルにマージされます。

読み取りと書き込みの操作を同時に行うことができないため、ローカルデータベースでは WAL が高速になります。 ただし、WAL モードでは、_ページサイズ_を変更することはできません。また、データベースにファイルの関連付けを追加し、追加の_チェックポイント_操作を追加します。

SQLite.NET で WAL を有効にするには、`SQLiteAsyncConnection` インスタンスで `EnableWriteAheadLoggingAsync` メソッドを呼び出します。

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

詳細については、「 [Xamarin. Forms でのファイル処理](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Todo サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet パッケージ](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite のドキュメント](https://www.sqlite.org/docs.html)
- [Android での SQLite の使用](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [IOS での SQLite の使用](~/ios/data-cloud/data/using-sqlite-orm.md)
- [タスクベースの非同期パターン (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Lazy&lt;T&gt; クラス](xref:System.Lazy`1)
