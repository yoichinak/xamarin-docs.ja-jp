---
title: ローカルの SQLite.NET データベースにデータを格納する
description: この記事では、ローカル SQLite.NET データベースにデータを格納する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: e9aed82ef0db9de35bd4cffe7daebcaa9389e42e
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832137"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>SQLite.NET のローカル データベースにデータを格納します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)

このクイック スタートでは、学習する方法。

- NuGet パッケージをプロジェクトに追加するのにには、NuGet パッケージ マネージャーを使用します。
- SQLite.NET データベースにデータをローカルに格納します。

このクイック スタート SQLite.NET のローカル データベースにデータを格納する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![](database-images/screenshots1-sml.png "ページをノート")](database-images/screenshots1.png#lightbox "ページをノート")
[![](database-images/screenshots2-sml.png "エントリ ページに注意してください")](database-images/screenshots2.png#lightbox "に注意してくださいエントリ ページ")

### <a name="prerequisites"></a>必須コンポーネント

正常に完了する必要があります、[前のクイック スタート](multi-page.md)このクイック スタートを試行する前にします。 または、ダウンロード、[前のクイック スタート サンプル](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)し、このクイック スタートの開始点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、ノートのソリューションを開きます。

2. **ソリューション エクスプ ローラー**を選択、**ノート**プロジェクトを右クリックし、 **NuGet パッケージの管理.** :

    ![](database-images/vs/add-nuget-packages.png "NuGet パッケージを追加します")    

3. **NuGet パッケージ マネージャー**で、 **[参照]** タブを選択し、**pcl-sqlite-net** NuGet パッケージを検索して選択し、 **[インストール]** ボタンをクリックしてプロジェクトに追加します。

    ![](database-images/vs/add-package.png "パッケージを追加します。")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **ソリューション エクスプ ローラー**の**ノート**プロジェクトを開き、 **Note.cs**で、**モデル**フォルダーと置換の既存のコードを次のコード:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    このクラスを定義、`Note`アプリケーションで各メモに関するデータを格納するモデル。 `ID`プロパティが付いた`PrimaryKey`と`AutoIncrement`各ことを確認する属性`Note`SQLite.NET データベース内のインスタンスで SQLite.NET によって提供される一意の id が必要があります。

    変更を保存**Note.cs**キーを押して**CTRL + S**ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

5. **ソリューション エクスプ ローラー**、という名前の新しいフォルダーを追加**データ**を**ノート**プロジェクト。

6. **ソリューション エクスプ ローラー**の**ノート**プロジェクト という名前の新しいクラスを追加 **NoteDatabase**を**データ**フォルダー。

7. **NoteDatabase.cs**、既存のコードを次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    このクラスには、データベースを作成、データを読み取ったり、データを書き込み、およびからデータを削除するコードが含まれています。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスはによって提供される、`App`次の手順でクラス。

    変更を保存**NoteDatabase.cs**キーを押して**CTRL + S**ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

8. **ソリューション エクスプ ローラー**の**ノート**プロジェクトで、ダブルクリックして**App.xaml.cs**を開きます。 既存のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    このコードを定義、`Database`プロパティを作成する`NoteDatabase`、データベースのファイル名を引数として渡して、シングルトンとしてインスタンス、`NoteDatabase`コンス トラクター。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

9. **ソリューション エクスプ ローラー**の**ノート**プロジェクトで、ダブルクリックして**NotesPage.xaml.cs**を開きます。 置換し、`OnAppearing`メソッドを次のコード。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    このコードを生成、 [ `ListView` ](xref:Xamarin.Forms.ListView)データベースに格納されているすべてのノートにします。

    変更を保存**NotesPage.xaml.cs**キーを押して**CTRL + S**ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

10. **ソリューション エクスプ ローラー**、ダブルクリックして**NoteEntryPage.xaml.cs**を開きます。 置換し、`OnSaveButtonClicked`と`OnDeleteButtonClicked`メソッドを次のコードで。

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage`格納、`Note`インスタンスで 1 つの注記を表す、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)ページの。 ときに、`OnSaveButtonClicked`イベント ハンドラーが実行される、`Note`インスタンスがデータベースに保存され、アプリケーションが、前のページに移動します。 ときに、`OnDeleteButtonClicked`イベント ハンドラーが実行される、`Note`インスタンスがデータベースから削除され、アプリケーションが、前のページに移動します。

      変更を保存**NoteEntryPage.xaml.cs**キーを押して**CTRL + S**ファイルを閉じます。

11. 構築し、各プラットフォームで、プロジェクトを実行します。 詳細については、次を参照してください。[クイック スタートを構築](single-page.md#building-the-quickstart)します。

    **NotesPage**キーを押して、 **+** に移動するボタン、 **NoteEntryPage**メモを入力します。 移動は、アプリケーションのメモの保存後、 **NotesPage**します。

    ノートについては、アプリケーションの動作を確認する、さまざまな長さの数を入力します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac 起動し、ノートのプロジェクトを開きます。

2. **Solution Pad**を選択、**ノート**プロジェクトを右クリックし、**追加 > NuGet パッケージを追加しています.** :

    ![](database-images/vsmac/add-nuget-packages.png "NuGet パッケージを追加します")    

3. **[パッケージを追加]** ウィンドウで、**sqlite-net-pcl** NuGet パッケージを検索して選択し、 **[パッケージを追加]** ボタンをクリックしてプロジェクトに追加します。

    ![](database-images/vsmac/add-package.png "パッケージを追加します。")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **Solution Pad**の**ノート**プロジェクトを開き、 **Note.cs**で、**モデル**フォルダーと、既存のコードを次の置換コード:

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    このクラスを定義、`Note`アプリケーションで各メモに関するデータを格納するモデル。 `ID`プロパティが付いた`PrimaryKey`と`AutoIncrement`各ことを確認する属性`Note`SQLite.NET データベース内のインスタンスで SQLite.NET によって提供される一意の id が必要があります。

    変更を保存**Note.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

5. **Solution Pad**、という名前の新しいフォルダーを追加**データ**を**ノート**プロジェクト。

6. **Solution Pad**の**ノート**プロジェクト という名前の新しいクラスを追加 **NoteDatabase**を**データ**フォルダー。

7. **NoteDatabase.cs**、既存のコードを次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    このクラスには、データベースを作成、データを読み取ったり、データを書き込み、およびからデータを削除するコードが含まれています。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスはによって提供される、`App`次の手順でクラス。

    変更を保存**NoteDatabase.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

8. **Solution Pad**の**ノート**プロジェクトで、ダブルクリックして**App.xaml.cs**を開きます。 既存のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    このコードを定義、`Database`プロパティを作成する`NoteDatabase`、データベースのファイル名を引数として渡して、シングルトンとしてインスタンス、`NoteDatabase`コンス トラクター。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

9. **Solution Pad**の**ノート**プロジェクトで、ダブルクリックして**NotesPage.xaml.cs**を開きます。 置換し、`OnAppearing`メソッドを次のコード。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    このコードを生成、 [ `ListView` ](xref:Xamarin.Forms.ListView)データベースに格納されているすべてのノートにします。

    変更を保存**NotesPage.xaml.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

10. **Solution Pad**、ダブルクリックして**NoteEntryPage.xaml.cs**を開きます。 置換し、`OnSaveButtonClicked`と`OnDeleteButtonClicked`メソッドを次のコードで。

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage`格納、`Note`インスタンスで 1 つの注記を表す、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)ページの。 ときに、`OnSaveButtonClicked`イベント ハンドラーが実行される、`Note`インスタンスがデータベースに保存され、アプリケーションが、前のページに移動します。 ときに、`OnDeleteButtonClicked`イベント ハンドラーが実行される、`Note`インスタンスがデータベースから削除され、アプリケーションが、前のページに移動します。

      変更を保存**NoteEntryPage.xaml.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

11. 構築し、各プラットフォームで、プロジェクトを実行します。 詳細については、次を参照してください。[クイック スタートを構築](single-page.md#building-the-quickstart)します。

    **NotesPage**キーを押して、 **+** に移動するボタン、 **NoteEntryPage**メモを入力します。 移動は、アプリケーションのメモの保存後、 **NotesPage**します。

    ノートについては、アプリケーションの動作を確認する、さまざまな長さの数を入力します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイック スタートでは説明した方法。

- NuGet パッケージをプロジェクトに追加するのにには、NuGet パッケージ マネージャーを使用します。
- SQLite.NET データベースにデータをローカルに格納します。

XAML スタイルを使用して、アプリケーションのスタイルを設定するには、次のクイック スタートに進みます。

> [!div class="nextstepaction"]
> [次へ](styling.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)
- [Xamarin.Forms のクイック スタートの詳細情報](deepdive.md)
