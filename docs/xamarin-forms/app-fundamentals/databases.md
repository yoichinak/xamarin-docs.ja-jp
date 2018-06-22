---
title: Xamarin.Forms ローカル データベース
description: Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。 この記事では、Xamarin.Forms アプリケーションが読み取りを SQLite.Net を使用してローカル SQLite データベースにデータを書き込む方法について説明します。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310141"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms ローカル データベース

_Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。この記事では、Xamarin.Forms アプリケーションが読み取りを SQLite.Net を使用してローカル SQLite データベースにデータを書き込む方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms アプリケーションを使用して、 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/)を参照して共有コードを組み込むデータベースの操作をするためのパッケージ、 `SQLite` NuGet に含まれているクラスです。 データベース操作は、まず Xamarin.Forms ソリューションの標準の .NET ライブラリ プロジェクトで定義できます。

付随する[サンプル アプリケーション](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo)はシンプルな Todo リスト アプリケーションです。 次のスクリーン ショットは、各プラットフォームでサンプルを表示する方法を示しています。

[![Xamarin.Forms データベースの使用例のスクリーン ショット](databases-images/todo-list-sml.png "TodoList 最初のページのスクリーン ショット")](databases-images/todo-list.png#lightbox "TodoList 最初のページのスクリーン ショット") [ ![Xamarin.Forms データベースの使用例のスクリーン ショット](databases-images/todo-list-sml.png "TodoList 最初のページのスクリーン ショット")](databases-images/todo-list.png#lightbox "TodoList 最初のページのスクリーン ショット")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite の使用

Xamarin.Forms .NET 標準ライブラリに SQLite サポートを追加するには、検索する NuGet の検索機能を使用**sqlite net pcl**し、最新のパッケージをインストールします。

![NuGet SQLite.NET PCL パッケージを追加](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL パッケージの追加")

類似した名前の NuGet パッケージの数が、適切なパッケージがこれらの属性。

- **によって作成された:** Frank A. Krueger
- **Id:** sqlite net pcl
- **NuGet リンク:** [sqlite net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 使用して、パッケージ名であるにもかかわらず、 **sqlite net pcl**も標準の .NET プロジェクトでの NuGet パッケージです。

参照が追加されると、追加のプロパティを`App`データベースを格納するためのローカル ファイル パスを表すクラス。

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

`TodoItemDatabase`を引数として、データベース ファイルのパスを受け取るコンス トラクターを次に示します。

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

アプリケーションのときに開いてとして保持されているシングルトンが 1 つのデータベース接続を作成すると、データベースを公開することの利点が実行される、データベース操作を開いたり閉じたり、データベース ファイルごとにコストを回避するため使用されます。

残りの部分、`TodoItemDatabase`クラスには、プラットフォーム間を実行している SQLite クエリが含まれています。 クエリのコード例を次に示します (構文の詳細については含まれて、[を使用して SQLite.NET](~/cross-platform/app-fundamentals/index.md)資料)。

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
> 非同期 SQLite.Net API を使用する利点は、操作はバック グラウンド スレッドに移動するデータベースです。 さらに、その他の同時実行 API はその処理のための処理コードを記述する必要はありません。

## <a name="summary"></a>まとめ

Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。

この記事の内容に重点を置きます**へのアクセス**Xamarin.Forms を使用して SQLite データベース。 SQLite.Net 自体の扱いの詳細についてを参照してください、 [Android で SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md)または[iOS で SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md)ドキュメント。

## <a name="related-links"></a>関連リンク

- [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [データベースのブック](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
