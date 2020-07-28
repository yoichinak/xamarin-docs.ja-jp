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
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a28e391c6bacd460f095c94e30b2d9433a5191e4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931795"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>ローカルの SQLite.NET データベースにデータを格納する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

このクイックスタートでは、次の方法について学習します。

- Nuget パッケージ マネージャーを使用して、NuGet パッケージをプロジェクトに追加する。
- データを SQLite.NET データベースにローカルに格納する。

このクイック スタートでは、ローカルの SQLite.NET データベースにデータを格納する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![Notes ページ](database-images/screenshots1-sml.png)](database-images/screenshots1.png#lightbox "Notes ページ")
[![Note Entry ページ](database-images/screenshots2-sml.png)](database-images/screenshots2.png#lightbox "Note Entry ページ")

## <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試みるには、[前のクイックスタート](multi-page.md)を正常に完了しておく必要があります。 または、[前のクイックスタートのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)をダウンロードし、それをこのクイックスタートの出発点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、Notes ソリューションを開きます。

2. **ソリューション エクスプローラー**で、 **[Notes]** プロジェクトを選択し、右クリックして **[NuGet パッケージの管理]** を選択します。

    ![NuGet パッケージの追加](database-images/vs/add-nuget-packages.png)    

3. **NuGet パッケージ マネージャー**で、 **[参照]** タブを選択し、**pcl-sqlite-net** NuGet パッケージを検索して選択し、 **[インストール]** ボタンをクリックしてプロジェクトに追加します。

    ![パッケージの追加](database-images/vs/add-package.png)

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **ソリューション エクスプローラー**の **[Notes]** プロジェクトで、 **[Models]** フォルダーにある **[Note.cs]** を開き、既存のコードを次のコードに置き換えます。

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

    このクラスでは、アプリケーションでの各メモに関するデータを格納する `Note` モデルが定義されています。 SQLite.NET データベース内の各 `Note` インスタンスが SQLite.NET によって付与される一意の ID を確実に持つようにするため、`ID` プロパティが `PrimaryKey` と `AutoIncrement` の属性によってマークされます。

    **Ctrl + S** キーを押して **Note.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

5. **ソリューション エクスプローラー**で、 **[Data]** という名前の新しいフォルダーを **[Notes]** プロジェクトに追加します。

6. **ソリューション エクスプローラー**の **[Note]** プロジェクトで、 **[Data]** フォルダーに **[NoteDatabase]** という名前の新しいクラスを追加します。

7. **[NoteDatabase.cs]** で、既存のコードを次のコードに置き換えます。

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

    このクラスには、データベースを作成し、それに対してデータの読み込み、データの書き込み、データの削除を行うためのコードが含まれます。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは次の手順で `App` クラスによって提供されます。

    **CTRL + S** キーを押して **NoteDatabase.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

8. **ソリューション エクスプローラー**の **[Notes]** プロジェクトで、 **[App.xaml.cs]** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードでは `Database` プロパティが定義されています。これによって、新しい `NoteDatabase` インスタンスがシングルトンとして作成され、データベースのファイル名が `NoteDatabase` コンストラクターに引数として渡されます。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

9. **ソリューション エクスプローラー**の **[Notes]** プロジェクトで、 **[NotesPage.xaml.cs]** をダブルクリックして開きます。 次に、`OnAppearing` メソッドを次のコードに置き換えます。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    このコードにより、データベースに格納されているすべてのメモが [`ListView`](xref:Xamarin.Forms.ListView) に取り込まれます。

    **CTRL + S** キーを押して **NotesPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

10. **ソリューション エクスプローラー**で、 **[NoteEntryPage.xaml.cs]** をダブルクリックして開きます。 次に、`OnSaveButtonClicked` と `OnDeleteButtonClicked` のメソッドを次のコードに置き換えます。

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

      `NoteEntryPage` では 1 つのメモを表す `Note` インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に格納されます。 `OnSaveButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースに保存され、アプリケーションは前のページに戻ります。 `OnDeleteButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースから削除され、アプリケーションは前のページに戻ります。

      **CTRL + S** キーを押して **NoteEntryPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

11. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[+]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動し、Notes プロジェクトを開きます。

2. **Solution Pad** で、 **[Notes]** プロジェクトを選択し、右クリックして **[追加] > [NuGet パッケージの追加]** の順に選択します。

    ![NuGet パッケージの追加](database-images/vsmac/add-nuget-packages.png)    

3. **[パッケージを追加]** ウィンドウで、**sqlite-net-pcl** NuGet パッケージを検索して選択し、 **[パッケージを追加]** ボタンをクリックしてプロジェクトに追加します。

    ![パッケージの追加](database-images/vsmac/add-package.png)

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **Solution Pad**の **[Notes]** プロジェクトで、 **[Models]** フォルダーにある **[Note.cs]** を開き、既存のコードを次のコードに置き換えます。

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

    このクラスでは、アプリケーションでの各メモに関するデータを格納する `Note` モデルが定義されています。 SQLite.NET データベース内の各 `Note` インスタンスが SQLite.NET によって付与される一意の ID を確実に持つようにするため、`ID` プロパティが `PrimaryKey` と `AutoIncrement` の属性によってマークされます。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**Note.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

5. **Solution Pad** で、 **[Data]** という名前の新しいフォルダーを **[Notes]** プロジェクトに追加します。

6. **Solution Pad** の **[Note]** プロジェクトで、 **[Data]** フォルダーに **[NoteDatabase]** という名前の新しいクラスを追加します。

7. **[NoteDatabase.cs]** で、既存のコードを次のコードに置き換えます。

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

    このクラスには、データベースを作成し、それに対してデータの読み込み、データの書き込み、データの削除を行うためのコードが含まれます。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは次の手順で `App` クラスによって提供されます。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NoteDatabase.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

8. **Solution Pad** の **[Notes]** プロジェクトで、 **[App.xaml.cs]** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードでは `Database` プロパティが定義されています。これによって、新しい `NoteDatabase` インスタンスがシングルトンとして作成され、データベースのファイル名が `NoteDatabase` コンストラクターに引数として渡されます。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

9. **Solution Pad** の **[Notes]** プロジェクトで、 **[NotesPage.xaml.cs]** をダブルクリックして開きます。 次に、`OnAppearing` メソッドを次のコードに置き換えます。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    このコードにより、データベースに格納されているすべてのメモが [`ListView`](xref:Xamarin.Forms.ListView) に取り込まれます。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NotesPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

10. **Solution Pad** で **[NoteEntryPage.xaml.cs]** をダブルクリックして開きます。 次に、`OnSaveButtonClicked` と `OnDeleteButtonClicked` のメソッドを次のコードに置き換えます。

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

      `NoteEntryPage` では 1 つのメモを表す `Note` インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に格納されます。 `OnSaveButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースに保存され、アプリケーションは前のページに戻ります。 `OnDeleteButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースから削除され、アプリケーションは前のページに戻ります。

      **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NoteEntryPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

11. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[+]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- Nuget パッケージ マネージャーを使用して、NuGet パッケージをプロジェクトに追加する。
- データを SQLite.NET データベースにローカルに格納する。

XAML スタイルを使用してアプリケーションのスタイルを設定するには、次のクイックスタートに進みます。

> [!div class="nextstepaction"]
> [次へ](styling.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Xamarin.Forms クイックスタート Deep Dive](deepdive.md)
