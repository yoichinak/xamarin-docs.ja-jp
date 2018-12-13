---
title: Xamarin.Forms のローカル データベース
description: Xamarin.Forms では、SQLite データベース エンジンを使ったデータベース駆動型アプリケーションがサポートされています。これにより、共有コードでのオブジェクトの読み込みと保存が可能になります。 この記事では、Xamarin.Forms アプリケーションで SQLite.Net を使用して、ローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 05c77c4c47841a897d0d1de5c3ba004db524ea2d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "36310141"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms のローカル データベース

"_Xamarin.Forms では、SQLite データベース エンジンを使ったデータベース駆動型アプリケーションがサポートされています。これにより、共有コードでのオブジェクトの読み込みと保存が可能になります。この記事では、Xamarin.Forms アプリケーションで SQLite.Net を使用して、ローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。_"

## <a name="overview"></a>概要

Xamarin.Forms アプリケーションでは、[SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) パッケージを使用して、共有コードにデータベース操作を組み込むことができます。これを行うには、`SQLite` NuGet に含まれているクラスを参照します。 Xamarin.Forms ソリューションの .NET Standard ライブラリ プロジェクト内にデータベース操作を定義できます。

付随している[サンプル アプリケーション](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo)は、単純な Todo-list アプリケーションです。 次のスクリーンショットで、各プラットフォーム上でサンプルがどのように表示されるかを示します。

[![Xamarin.Forms データベース サンプルのスクリーンショット](databases-images/todo-list-sml.png "TodoList の 1 ページ目のスクリーンショット")](databases-images/todo-list.png#lightbox "TodoList の 1 ページ目のスクリーンショット") [![Xamarin.Forms データベース サンプルのスクリーンショット](databases-images/todo-list-sml.png "TodoList の 1 ページ目のスクリーンショット")](databases-images/todo-list.png#lightbox "TodoListの 1 ページ目のスクリーンショット")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite の使用

Xamarin.Forms .NET Standard ライブラリに SQLite のサポートを追加するには、NuGet の検索機能を使用して **pcl-sqlite-net** を検索し、最新のパッケージをインストールします。

![NuGet SQLite.NET PCL パッケージの追加](databases-images/vs2017-sqlite-pcl-nuget.png "Add NuGet SQLite.NET PCL パッケージの追加")

類似した名前の NuGet パッケージがいくつかあります。正しいパッケージには、次の属性があります。

- **作成者:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> パッケージ名に関係なく、**pcl-sqlite-net** NuGet パッケージを .NET Standard プロジェクトでも使用します。

参照を追加したら、データベースを格納するためのローカル ファイル パスを返すプロパティを `App` クラスに追加します。

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

引数としてデータベース ファイルのパスを受け取る `TodoItemDatabase` コンストラクターを次に示します。

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

`TodoItemDatabase` クラスの残りの部分には、クロスプラットフォームで実行される SQLite クエリが含まれています。 クエリのコード例を次に示します (構文の詳細については、「[Using SQLite.NET with Xamarin.iOS](~/ios/data-cloud/data/using-sqlite-orm.md)」(Xamarin.iOS で SQLite.NET を使用する) で確認できます)。

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> 非同期 SQLite.Net API を使用することの利点は、データベース操作がバックグラウンドのスレッドに移動することです。 さらに、追加の同時実行処理は API によって処理されるため、そのコードを記述する必要はありません。

## <a name="summary"></a>まとめ

Xamarin.Forms では、SQLite データベース エンジンを使ったデータベース駆動型アプリケーションがサポートされています。これにより、共有コードでのオブジェクトの読み込みと保存が可能になります。

この記事では、Xamarin.Forms を使用した SQLite データベースへの**アクセス**に注目しました。 SQLite.Net 自体の操作方法の詳細については、[Android での SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md) または [iOS での SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md) のドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)

