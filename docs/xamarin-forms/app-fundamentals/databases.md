---
title: "ローカル データベース"
description: "Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。 この記事では、Xamarin.Forms アプリケーションが読み取りを SQLite.Net を使用してローカル SQLite データベースにデータを書き込む方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2017
ms.openlocfilehash: 29686b29a18fe409a1f778d54266cbeedea40eda
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="local-databases"></a>ローカル データベース

_Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。この記事では、Xamarin.Forms アプリケーションが読み取りを SQLite.Net を使用してローカル SQLite データベースにデータを書き込む方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms アプリケーションを使用して、 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/)を参照して共有コードを組み込むデータベースの操作をするためのパッケージ、 `SQLite` NuGet に含まれているクラスです。 ポータブル クラス ライブラリ (PCL) プロジェクト内の Xamarin.Forms ソリューションをデータベースを格納するパスを返すプラットフォーム固有のプロジェクトでは、データベース操作を定義できます。

付随する[サンプル アプリケーション](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo)はシンプルな Todo リスト アプリケーションです。 次のスクリーン ショットは、各プラットフォームでサンプルを表示する方法を示しています。

[![Xamarin.Forms データベースの使用例のスクリーン ショット](databases-images/todo-list-sml.png "TodoList 最初のページのスクリーン ショット")](databases-images/todo-list.png#lightbox "TodoList 最初のページのスクリーン ショット") [ ![Xamarin.Forms データベースの使用例のスクリーン ショット](databases-images/todo-list-sml.png "TodoList 最初のページのスクリーン ショット")](databases-images/todo-list.png#lightbox "TodoList 最初のページのスクリーン ショット")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite の使用

このセクションでは、SQLite.Net NuGet パッケージ、まず Xamarin.Forms ソリューションを追加、データベース操作を実行し、使用するメソッドを記述する方法を示します、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)を各プラットフォームで、データベースを格納する場所を決定します。

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-pcl-project"></a>Xamarins.Forms PCL Project

Xamarin.Forms PCL プロジェクトに SQLite サポートを追加するには、検索に NuGet の検索機能を使用**sqlite net pcl**してパッケージをインストールします。

![](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL パッケージを追加します。")

類似した名前の NuGet パッケージの数が、適切なパッケージがこれらの属性。

- **によって作成された:** Frank A. Krueger
- **Id:** sqlite net pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

参照が追加されると、データベース ファイルの場所を調べるには、プラットフォーム固有の機能を抽象化するインターフェイスを記述します。 このサンプルで使用されるインターフェイスでは、1 つのメソッドを定義します。

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

インターフェイスを定義した後を使用して、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)を実装を取得し、ローカル ファイル パス (メモをこのインターフェイスはまだ実装されていません) を取得します。 次のコードの実装を取得、`App.Database`プロパティ。

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase`コンス トラクターを次に示します。

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

この方法では、開始タグとデータベースの操作を実行するたびに、データベース ファイルを閉じるの経費の回避など、アプリケーションの実行中に開いているとして保持されている 1 つのデータベース接続を作成します。

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

すべてのデータ アクセス コードはすべてのプラットフォーム間で共有するために PCL プロジェクトに書き込まれます。 次のセクションで説明したよう、のみ、データベースのローカル ファイル パスを取得、プラットフォーム固有のコードが必要です。

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS プロジェクト

IOS アプリケーションを構成するには、iOS プロジェクトを使用する同じ NuGet パッケージを追加、 *NuGet*ウィンドウ。

![](databases-images/vsmac-sqlite-nuget.png "NuGet SQLite.NET PCL パッケージを追加します。")

必要な唯一のコードは、`IFileHelper`データ ファイルのパスを決定する実装。 次のコード ファイルを配置、SQLite データベースに、**ライブラリ/データベース**アプリケーションのサンド ボックス内のフォルダーです。 参照してください、 [iOS ファイル システムで作業](~/ios/app-fundamentals/file-system.md)記憶域の使用可能な別のディレクトリの詳細についてはドキュメントです。

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

            if (!Directory.Exists(libFolder))
            {
                Directory.CreateDirectory(libFolder);
            }

            return Path.Combine(libFolder, filename);
        }
    }
}
```

コードを含む注、`assembly:Dependency`属性のこの実装で検出されるよう、`DependencyService`です。

<a name="PCL_Android" />

### <a name="android-project"></a>Android Project

Android アプリケーションを構成するを使用して、Android プロジェクトを同じ NuGet パッケージを追加、 *NuGet*ウィンドウ。

![](databases-images/vsmac-sqlite-nuget.png "NuGet SQLite.NET PCL パッケージを追加します。")

必要な唯一のコードは、この参照が追加されると、`IFileHelper`データ ファイルのパスを決定する実装。

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            return Path.Combine(path, filename);
        }
    }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Windows 10 ユニバーサル Windows プラットフォーム (UWP)

UWP アプリケーションを構成するには、UWP プロジェクトを使用する同じ NuGet パッケージを追加、 *NuGet*ウィンドウ。

![](databases-images/vs2017-sqlite-uwp-nuget.png "NuGet SQLite.NET PCL パッケージを追加します。")

参照が追加されると、実装、`IFileHelper`プラットフォーム固有の使用をインターフェイス`Windows.Storage`データ ファイルのパスを決定する API。

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
        }
    }
}

```

## <a name="summary"></a>まとめ

Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。

この記事の内容に重点を置きます**へのアクセス**Xamarin.Forms を使用して SQLite データベース。 SQLite.Net 自体の扱いの詳細についてを参照してください、[データ アクセス: を使用して SQLite.NET](~/cross-platform/app-fundamentals/index.md)ドキュメント。 ほとんどの SQLite.Net コードは、すべてのプラットフォームで共有可能のみ SQLite データベース ファイルの場所を構成するには、プラットフォーム固有の機能が必要です。


## <a name="related-links"></a>関連リンク

- [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [データベースのブック](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
