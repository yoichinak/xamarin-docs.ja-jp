---
title: Xamarin.Forms ローカルデータベース
description: Xamarin.Forms は、SQLite データベースエンジンを使用したデータベース駆動型アプリケーションをサポートします。これにより、共有コードでオブジェクトを読み込んで保存できるようになります。 この記事で Xamarin.Forms は、SQLite.Net を使用して、アプリケーションがローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 03/01/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a7dd5ea8963fed079c82ac6944d571176002486e
ms.sourcegitcommit: 322e7bcf9fb8c1ad52ab8e929bea95d45e280834
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751445"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms ローカルデータベース

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/todo)

SQLite データベースエンジンを使用 Xamarin.Forms すると、アプリケーションは共有コードでデータオブジェクトを読み込んで保存することができます。 このサンプルアプリケーションでは、SQLite データベーステーブルを使用して todo 項目を格納します。 この記事では、共有コードで SQLite.Net を使用して、ローカルデータベースの情報を格納および取得する方法について説明します。

[![IOS と Android の Todolist アプリのスクリーンショット](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "IOS および Android での Todolist アプリ")

SQLite.NET をモバイルアプリに統合するには、次の手順を実行します。

1. [NuGet パッケージをインストール](#install-the-sqlite-nuget-package)します。
1. [定数を構成](#configure-app-constants)します。
1. [データベースアクセスクラスを作成](#create-a-database-access-class)します。
1. [でデータに Xamarin.Forms アクセス](#access-data-in-xamarinforms)します。
1. [詳細構成](#advanced-configuration)。

## <a name="install-the-sqlite-nuget-package"></a>SQLite NuGet パッケージをインストールする

NuGet パッケージマネージャーを使用して、 **sqlite-pcl** を検索し、共有コードプロジェクトに最新バージョンを追加します。

類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。

- **ID:** sqlite-net-pcl
- **作成者:** SQLite-net
- **所有者:** praeclarum
- **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> パッケージ名に関係なく、**sqlite-net-pcl** NuGet パッケージを .NET Standard プロジェクトでも使用します。

## <a name="configure-app-constants"></a>アプリ定数の構成

サンプルプロジェクトには、一般的な構成データを提供する **Constants.cs** ファイルが含まれています。

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

データベースの使用方法に応じて、異なるフラグを指定することが必要になる場合があります。 の詳細については `SQLiteOpenFlags` 、「sqlite.org で [新しいデータベース接続を開く](https://www.sqlite.org/c3ref/open.html) 」を参照してください。

## <a name="create-a-database-access-class"></a>データベースアクセスクラスを作成する

データベースラッパークラスは、アプリの他の部分からデータアクセス層を抽象化します。 このクラスは、クエリロジックを一元化し、データベースの初期化の管理を簡略化します。これにより、アプリの拡大に伴ってデータ操作のリファクタリングや拡張が容易になります。 Todo アプリは、 `TodoItemDatabase` この目的のためにクラスを定義します。

### <a name="lazy-initialization"></a>限定的な初期化

は、 `TodoItemDatabase` `AsyncLazy<T>` 最初にアクセスされるまでデータベースの初期化を遅延させるために、カスタムクラスによって表される非同期の遅延初期化を使用します。

```csharp
public class TodoItemDatabase
{
    static SQLiteAsyncConnection Database;

    public static readonly AsyncLazy<TodoItemDatabase> Instance = new AsyncLazy<TodoItemDatabase>(async () =>
    {
        var instance = new TodoItemDatabase();
        CreateTableResult result = await Database.CreateTableAsync<TodoItem>();
        return instance;
    });

    public TodoItemDatabase()
    {
        Database = new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    }

    //...
}
```

フィールドは、 `Instance` オブジェクトのデータベーステーブルが存在しない場合に作成するために使用され、 `TodoItem` シングルトンと `TodoItemDatabase` してを返します。 `Instance`型のフィールドは、 `AsyncLazy<TodoItemDatabase>` 最初に待機したときに構築されます。 複数のスレッドが同時にフィールドにアクセスしようとすると、すべてのスレッドで1つの構築が使用されます。 次に、構築が完了すると、すべての `await` 操作が完了します。 また、 `await` 構築が完了した後の操作は、値が使用可能であるため、すぐに続行されます。

> [!NOTE]
> データベース接続は静的なフィールドで、アプリケーションの有効期間中は1つのデータベース接続が使用されます。 永続的な静的接続を使用すると、単一のアプリセッション中に複数回接続を開始したり閉じたりするよりもパフォーマンスが向上します。

### <a name="asynchronous-lazy-initialization"></a>非同期の遅延初期化

データベースの初期化を開始するために、ブロックの実行を避け、例外をキャッチすることができます。サンプルアプリケーションでは、クラスによって表される非同期のレイジー初期化が使用され `AsyncLazy<T>` ます。

```csharp
public class AsyncLazy<T> : Lazy<Task<T>>
{
    readonly Lazy<Task<T>> instance;

    public AsyncLazy(Func<T> factory)
    {
        instance = new Lazy<Task<T>>(() => Task.Run(factory));
    }

    public AsyncLazy(Func<Task<T>> factory)
    {
        instance = new Lazy<Task<T>>(() => Task.Run(factory));
    }

    public TaskAwaiter<T> GetAwaiter()
    {
        return instance.Value.GetAwaiter();
    }
}
```

クラスは、 `AsyncLazy` 型と型を組み合わせて、 `Lazy<T>` `Task<T>` リソースの初期化を表す遅延初期化タスクを作成します。 コンストラクターに渡されるファクトリデリゲートは、同期または非同期のどちらでもかまいません。 ファクトリデリゲートはスレッドプールのスレッドで実行され、複数のスレッドが同時に起動しようとしても、複数回実行されることはありません。 ファクトリデリゲートが完了すると、遅延初期化された値が使用可能になり、インスタンスを待機しているすべてのメソッドが `AsyncLazy<T>` 値を受け取ります。 詳しくは、「[AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/)」をご覧ください。

### <a name="data-manipulation-methods"></a>データ操作方法

クラスには、 `TodoItemDatabase` 作成、読み取り、編集、および削除の4種類のデータ操作のためのメソッドが含まれています。 SQLite.NET ライブラリには、SQL ステートメントを記述せずにオブジェクトを格納および取得するための単純なオブジェクトリレーショナルマップ (ORM) が用意されています。

```csharp
public class TodoItemDatabase
{
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

## <a name="access-data-in-xamarinforms"></a>データへのアクセス Xamarin.Forms

クラスは、 `TodoItemDatabase` `Instance` クラス内のデータアクセス操作を `TodoItemDatabase` 呼び出すことができるフィールドを公開します。

```csharp
async void OnSaveClicked(object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    TodoItemDatabase database = await TodoItemDatabase.Instance;
    await database.SaveItemAsync(todoItem);

    // Navigate backwards
    await Navigation.PopAsync();
}
```

## <a name="advanced-configuration"></a>詳細な構成

SQLite は、この記事とサンプルアプリに記載されているよりも多くの機能を備えた堅牢な API を提供します。 以下のセクションでは、スケーラビリティにとって重要な機能について説明します。

詳細については、sqlite.org に関する [SQLite のドキュメント](https://www.sqlite.org/docs.html) を参照してください。

### <a name="write-ahead-logging"></a>先行書き込みログ

既定では、SQLite は従来の rollback ジャーナルを使用します。 変更されていないデータベースの内容のコピーが別のロールバックファイルに書き込まれ、変更がデータベースファイルに直接書き込まれます。 このコミットは、ロールバックジャーナルが削除されたときに発生します。

Write-Ahead のログ記録 (WAL) では、最初に個別の WAL ファイルに変更を書き込みます。 WAL モードでは、コミットは特殊なレコードで、WAL ファイルに追加されます。これにより、1つの WAL ファイルで複数のトランザクションを実行できます。 WAL ファイルは、 _チェックポイント_ と呼ばれる特殊な操作でデータベースファイルにマージされます。

読み取りと書き込みの操作を同時に行うことができないため、ローカルデータベースでは WAL が高速になります。 ただし、WAL モードでは、 _ページサイズ_ を変更することはできません。また、データベースにファイルの関連付けを追加し、追加の _チェックポイント_ 操作を追加します。

SQLite.NET で WAL を有効にするには、 `EnableWriteAheadLoggingAsync` インスタンスでメソッドを呼び出し `SQLiteAsyncConnection` ます。

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

詳細については、「 [SQLite Write-Ahead Logging](https://www.sqlite.org/wal.html) on sqlite.org」を参照してください。

### <a name="copy-a-database"></a>データベースをコピーする

SQLite データベースのコピーが必要になる場合があります。

- データベースはアプリケーションに付属していましたが、モバイルデバイスの書き込み可能ストレージにコピーまたは移動する必要があります。
- データベースのバックアップまたはコピーを作成する必要があります。
- データベースファイルのバージョン、移動、または名前変更を行う必要があります。

一般に、データベースファイルの移動、名前の変更、またはコピーは、他のいくつかの考慮事項がある他のファイルの種類と同じです。

- データベースファイルの移動を試行する前に、すべてのデータベース接続を閉じる必要があります。
- [先行書き込みログ](#write-ahead-logging)を使用する場合、SQLite は共有メモリアクセス (shm) ファイルと (先行書き込みログ) (wal) ファイルを作成します。 これらのファイルにも変更を適用してください。

詳細については、「 [」 Xamarin.Forms の「ファイル処理](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Todo サンプルアプリケーション](/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet パッケージ](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite のドキュメント](https://www.sqlite.org/docs.html)
- [Android での SQLite の使用](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [IOS での SQLite の使用](~/ios/data-cloud/data/using-sqlite-orm.md)
- [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/)
