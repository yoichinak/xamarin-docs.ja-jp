---
title: ローカルの SQLite.NET データベースにデータを格納する
description: この記事では、ローカルの SQLite.NET データベースにデータを格納する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: ff07af00e1e647255ac56318c0685552823f510c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653505"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>ローカルの SQLite.NET データベースにデータを格納する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

このクイックスタートでは、次の方法について説明します。

- Nuget パッケージマネージャーを使用して、NuGet パッケージをプロジェクトに追加します。
- データを SQLite.NET データベースにローカルに格納します。

クイックスタートでは、ローカルの SQLite.NET データベースにデータを格納する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![](database-images/screenshots1-sml.png "メモページ")](database-images/screenshots1.png#lightbox "メモページ\"")
)[![](database-images/screenshots2-sml.png "メモ入力ページ")](database-images/screenshots2.png#lightbox "メモ入力ページ")

### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試行する前に、[前のクイックスタート](multi-page.md)を正常に完了している必要があります。 または、[前のクイックスタートサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)をダウンロードし、このクイックスタートの出発点として使用してください。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、Note ソリューションを開きます。

2. **ソリューションエクスプローラー**で、**メモ**プロジェクトを選択し、右クリックして **[NuGet パッケージの管理...]** を選択します。

    ![](database-images/vs/add-nuget-packages.png "NuGet パッケージを追加します")    

3. **NuGet パッケージ マネージャー**で、 **[参照]** タブを選択し、**pcl-sqlite-net** NuGet パッケージを検索して選択し、 **[インストール]** ボタンをクリックしてプロジェクトに追加します。

    ![](database-images/vs/add-package.png "パッケージの追加")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **[モデル]** フォルダーの**Note.cs**を開き、既存のコードを次のコードに置き換えます。

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

    このクラスは、 `Note`アプリケーションの各メモに関するデータを格納するモデルを定義します。 プロパティは、SQLite.NET データベース`PrimaryKey`内`AutoIncrement`の各`Note`インスタンスが SQLite.NET によって提供される一意の id を持つように、属性と属性でマークされます。 `ID`

    **CTRL + S**キーを押して**Note.cs**への変更内容を保存し、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

5. **ソリューションエクスプローラー**で、 **Notes**プロジェクトに**Data**という名前の新しいフォルダーを追加します。

6. **ソリューションエクスプローラー**の**Notes**プロジェクトで、note **database**という名前の新しいクラスを**Data**フォルダーに追加します。

7. **NoteDatabase.cs**で、既存のコードを次のコードに置き換えます。

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

    このクラスには、データベースを作成し、そこからデータを読み取り、データを書き込んでからデータを削除するためのコードが含まれています。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは、次の手順`App`でクラスによって提供されます。

    **CTRL + S**キーを押して**NoteDatabase.cs**への変更内容を保存し、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

8. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **App.xaml.cs**をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードは、 `Database`新しい`NoteDatabase`インスタンスをシングルトンとして作成し、データベースのファイル名をコンストラクターの`NoteDatabase`引数として渡すプロパティを定義します。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

9. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **NotesPage.xaml.cs**をダブルクリックして開きます。 次に、 `OnAppearing`メソッドを次のコードに置き換えます。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)データベースに格納されているすべてのメモをに設定します。

    **CTRL + S**キーを押して**NotesPage.xaml.cs**への変更内容を保存し、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

10. **ソリューションエクスプローラー**で、 **[NoteEntryPage.xaml.cs]** をダブルクリックして開きます。 次に、 `OnSaveButtonClicked`メソッド`OnDeleteButtonClicked`とメソッドを次のコードに置き換えます。

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

      は`NoteEntryPage` `Note` [、`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 1 つのノートを表すインスタンスをページのに格納します。 イベントハンドラーが実行`Note`されると、インスタンスがデータベースに保存され、アプリケーションは前のページに戻ります。 `OnSaveButtonClicked` イベントハンドラーが実行`Note`されると、インスタンスがデータベースから削除され、アプリケーションが前のページに戻ります。 `OnDeleteButtonClicked`

      **CTRL + S**キーを押して**NoteEntryPage.xaml.cs**への変更内容を保存し、ファイルを閉じます。

11. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、 **+** ボタンを押して**NoteEntryPage**に移動し、メモを入力します。 メモを保存すると、アプリケーションは [ノート **] ページ**に戻ります。

    さまざまな長さのメモを入力して、アプリケーションの動作を観察します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動し、Notes プロジェクトを開きます。

2. **Solution Pad**で、**メモ**プロジェクトを選択して右クリックし、[**追加] > [NuGet パッケージの追加**] の順に選択します。

    ![](database-images/vsmac/add-nuget-packages.png "NuGet パッケージを追加します")    

3. **[パッケージを追加]** ウィンドウで、**sqlite-net-pcl** NuGet パッケージを検索して選択し、 **[パッケージを追加]** ボタンをクリックしてプロジェクトに追加します。

    ![](database-images/vsmac/add-package.png "パッケージの追加")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **Solution Pad**の**Notes**プロジェクトで、 **[モデル]** フォルダーの**Note.cs**を開き、既存のコードを次のコードに置き換えます。

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

    このクラスは、 `Note`アプリケーションの各メモに関するデータを格納するモデルを定義します。 プロパティは、SQLite.NET データベース`PrimaryKey`内`AutoIncrement`の各`Note`インスタンスが SQLite.NET によって提供される一意の id を持つように、属性と属性でマークされます。 `ID`

    **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **Note.cs**への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

5. **Solution Pad**で、 **Notes**プロジェクトに**Data**という名前の新しいフォルダーを追加します。

6. **Solution Pad**の Notes プロジェクトで、 **note** **database**という名前の新しいクラスを**Data**フォルダーに追加します。

7. **NoteDatabase.cs**で、既存のコードを次のコードに置き換えます。

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

    このクラスには、データベースを作成し、そこからデータを読み取り、データを書き込んでからデータを削除するためのコードが含まれています。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは、次の手順`App`でクラスによって提供されます。

    **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **NoteDatabase.cs**への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

8. **Solution Pad**の**Notes**プロジェクトで、 **App.xaml.cs**をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードは、 `Database`新しい`NoteDatabase`インスタンスをシングルトンとして作成し、データベースのファイル名をコンストラクターの`NoteDatabase`引数として渡すプロパティを定義します。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

9. **Solution Pad**の**Notes**プロジェクトで、 **NotesPage.xaml.cs**をダブルクリックして開きます。 次に、 `OnAppearing`メソッドを次のコードに置き換えます。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)データベースに格納されているすべてのメモをに設定します。

    **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **NotesPage.xaml.cs**への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

10. **Solution Pad**で、 **[NoteEntryPage.xaml.cs]** をダブルクリックして開きます。 次に、 `OnSaveButtonClicked`メソッド`OnDeleteButtonClicked`とメソッドを次のコードに置き換えます。

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

      は`NoteEntryPage` `Note` [、`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 1 つのノートを表すインスタンスをページのに格納します。 イベントハンドラーが実行`Note`されると、インスタンスがデータベースに保存され、アプリケーションは前のページに戻ります。 `OnSaveButtonClicked` イベントハンドラーが実行`Note`されると、インスタンスがデータベースから削除され、アプリケーションが前のページに戻ります。 `OnDeleteButtonClicked`

      **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **NoteEntryPage.xaml.cs**への変更内容を保存してから、ファイルを閉じます。

11. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、 **+** ボタンを押して**NoteEntryPage**に移動し、メモを入力します。 メモを保存すると、アプリケーションは [ノート **] ページ**に戻ります。

    さまざまな長さのメモを入力して、アプリケーションの動作を観察します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- Nuget パッケージマネージャーを使用して、NuGet パッケージをプロジェクトに追加します。
- データを SQLite.NET データベースにローカルに格納します。

XAML スタイルを使用してアプリケーションのスタイルを適用するには、次のクイックスタートに進みます。

> [!div class="nextstepaction"]
> [次へ](styling.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Xamarin.Forms のクイックスタートの詳細](deepdive.md)
