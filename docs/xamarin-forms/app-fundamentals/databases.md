---
title: Xamarin.Forms のローカル データベース
description: Xamarin.Forms は、負荷を共有コードでオブジェクトを保存することができます、SQLite データベース エンジンを使用してデータベース駆動型アプリケーションをサポートします。 この記事では、Xamarin.Forms アプリケーションが読み取るし、SQLite.Net を使用して、ローカルの SQLite データベースにデータを書き込む方法について説明します。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 05c77c4c47841a897d0d1de5c3ba004db524ea2d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "36310141"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms のローカル データベース

_Xamarin.Forms は、負荷を共有コードでオブジェクトを保存することができます、SQLite データベース エンジンを使用してデータベース駆動型アプリケーションをサポートします。この記事では、Xamarin.Forms アプリケーションが読み取るし、SQLite.Net を使用して、ローカルの SQLite データベースにデータを書き込む方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms アプリケーションで使用できます、 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/)にデータベースの操作を組み込むことのパッケージを参照してコードを共有する、 `SQLite` NuGet に含まれているクラス。 Xamarin.Forms ソリューションの .NET Standard ライブラリ プロジェクトでは、データベース操作を定義できます。

付随する[サンプル アプリケーション](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo)は単純な Todo リスト アプリケーションです。 次のスクリーン ショットでは、各プラットフォームで、サンプルを表示する方法を示します。

[![Xamarin.Forms のデータベースの例のスクリーン ショット](databases-images/todo-list-sml.png "TodoList 最初のページのスクリーン ショット")](databases-images/todo-list.png#lightbox "TodoList 最初のページのスクリーン ショット") [ ![Xamarin.Forms のデータベースの例のスクリーン ショット](databases-images/todo-list-sml.png "TodoList 最初のページのスクリーン ショット")](databases-images/todo-list.png#lightbox "TodoList 最初のページのスクリーン ショット")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite を使用します。

Xamarin.Forms .NET Standard ライブラリには、SQLite のサポートを追加するを検索する NuGet の検索機能を使用して、 **pcl-sqlite-net**して最新のパッケージをインストールします。

![NuGet SQLite.NET PCL のパッケージを追加](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL のパッケージの追加")

類似した名前の NuGet パッケージがいくつか、適切なパッケージがこれらの属性。

- **作成者:** Frank A. Krueger
- **Id:** sqlite-net-pcl
- **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 使用して、パッケージ名に関係なく、 **pcl-sqlite-net** .NET Standard プロジェクトであっても、NuGet パッケージ。

参照を追加すると、追加のプロパティを`App`クラスをデータベースを格納するためのローカル ファイル パスを返します。

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

`TodoItemDatabase`を引数としてのデータベース ファイルのパスを受け取るコンス トラクターを次に示します。

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

アプリケーションの中に開いたままでは、シングルトンは、1 つのデータベース接続が作成されるように、データベースを公開するという利点が実行される、データベース操作のたびにデータベース ファイルを閉じて開き経費を回避するため、実行されます。

残りの部分、`TodoItemDatabase`クラスには、クロス プラットフォームを実行している SQLite クエリが含まれています。 クエリのコード例を次に示します (構文の詳細が記載されて[Xamarin.iOS でを使用して SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md)します。

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
> 非同期 SQLite.Net API を使用する利点は、操作はバック グラウンド スレッドに移動するデータベースです。 さらに、その他の同時実行処理では、その API が自動的に処理をコードを記述する必要はありません。

## <a name="summary"></a>まとめ

Xamarin.Forms は、負荷を共有コードでオブジェクトを保存することができます、SQLite データベース エンジンを使用してデータベース駆動型アプリケーションをサポートします。

この記事に重点を置いて**にアクセスする**Xamarin.Forms を使用した SQLite データベース。 SQLite.Net 自体の操作方法の詳細についてを参照してください、 [android SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md)または[ios SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md)ドキュメント。

## <a name="related-links"></a>関連リンク

- [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)

