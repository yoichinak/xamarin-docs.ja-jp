---
title: ローカルの SQLite.NET データベースにデータを格納する
description: この記事では、Xamarin.Forms シェル アプリケーションからローカル環境の SQLite.NET データベースにデータを格納する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: F669CE4E-1E1B-409F-BC1C-8364633EB7EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/28/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cfc479e6211692f3de7d78e2fd52cd80a2630660
ms.sourcegitcommit: 1f391667869a4541dd9b42d78862dc01d69ed160
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2021
ms.locfileid: "99818405"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>ローカルの SQLite.NET データベースにデータを格納する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

このクイックスタートでは、次の方法について学習します。

- データを SQLite.NET データベースにローカルに格納する。

このクイックスタートでは、Xamarin.Forms シェル アプリケーションからローカル環境の SQLite.NET データベースにデータを格納する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![Notes ページ](database-images/screenshots1-sml.png)](database-images/screenshots1.png#lightbox)
[![Note Entry ページ](database-images/screenshots2-sml.png)](database-images/screenshots2.png#lightbox)

## <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試みるには、[前のクイックスタート](navigation.md)を正常に完了しておく必要があります。 または、[前のクイックスタートのサンプル](/samples/xamarin/xamarin-forms-samples/getstarted-notes-navigation/)をダウンロードし、それをこのクイックスタートの出発点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、Notes ソリューションを開きます。

2. **ソリューション エクスプローラー** で **Notes** ソリューションを右クリックし、 **[ソリューションの NuGet パッケージの管理...]** を選択します。

    ![NuGet パッケージの管理](database-images/vs/manage-nuget-packages.png)    

3. **NuGet パッケージ マネージャー** で **[参照]** タブを選択して、**sqlite-net-pcl** NuGet パッケージを検索します。

    > [!WARNING]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** SQLite-net
    > - **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    **NuGet パッケージ マネージャー** で、正しい **sqlite-net-pcl** パッケージを選択し、 **[プロジェクト]** チェック ボックスをオンにしてから、 **[インストール]** ボタンをクリックしてソリューションに追加します。

    ![sqlite-net-pcl を選択する](database-images/vs/select-package.png)

    このパッケージは、データベース操作をアプリケーションに組み込むために使用され、ソリューション内のすべてのプロジェクトに追加されます。

    **NuGet パッケージ マネージャー** を閉じます。

4. **ソリューション エクスプローラー** の **[Notes]** プロジェクトで、 **[Models]** フォルダーにある **[Note.cs]** を開き、既存のコードを次のコードに置き換えます。

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

    **Ctrl + S** キーを押して **Note.cs** への変更内容を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

5. **ソリューション エクスプローラー** で、 **[Data]** という名前の新しいフォルダーを **[Notes]** プロジェクトに追加します。

6. **ソリューション エクスプローラー** の **[Note]** プロジェクトで、 **[Data]** フォルダーに **[NoteDatabase]** という名前の新しいクラスを追加します。

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
            readonly SQLiteAsyncConnection database;

            public NoteDatabase(string dbPath)
            {
                database = new SQLiteAsyncConnection(dbPath);
                database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                //Get all notes.
                return database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                // Get a specific note.
                return database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    // Update an existing note.
                    return database.UpdateAsync(note);
                }
                else
                {
                    // Save a new note.
                    return database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                // Delete a note.
                return database.DeleteAsync(note);
            }
        }
    }
    ```

    このクラスには、データベースを作成し、それに対してデータの読み込み、データの書き込み、データの削除を行うためのコードが含まれます。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは次の手順で `App` クラスによって提供されます。

    **Ctrl + S** キーを押して **NoteDatabase.cs** への変更を保存します。  

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

8. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **App.xaml** を展開し、**App.xaml.cs** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Notes.Data;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            // Create the database connection as a singleton.
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
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    このコードでは `Database` プロパティが定義されています。これによって、新しい `NoteDatabase` インスタンスがシングルトンとして作成され、データベースのファイル名が `NoteDatabase` コンストラクターに引数として渡されます。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **Ctrl + S** キーを押して **App.xaml.cs** への変更を保存します。  

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

9. **ソリューション エクスプローラー** の **Notes** プロジェクトで、 **[Views]** フォルダーの **NotesPage.xaml** を展開し、**NotesPage.xaml.cs** を開きます。 次に、`OnAppearing` と `OnSelectionChanged` のメソッドを次のコードに置き換えます。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        // Retrieve all the notes from the database, and set them as the
        // data source for the CollectionView.
        collectionView.ItemsSource = await App.Database.GetNotesAsync();
    }

    async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        if (e.CurrentSelection != null)
        {
            // Navigate to the NoteEntryPage, passing the ID as a query parameter.
            Note note = (Note)e.CurrentSelection.FirstOrDefault();
            await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.ID.ToString()}");
        }
    }    
    ```    

    `OnAppearing` メソッドによって、データベースに格納されているメモが、[`CollectionView`](xref:Xamarin.Forms.CollectionView) に設定されます。 `OnSelectionChanged` メソッドによって、`NoteEntryPage` への移動が行われ、選択されている `Note` オブジェクトの `ID` プロパティがクエリ パラメーターとして渡されます。

    **Ctrl + S** キーを押して **NotesPage.xaml.cs** への変更を保存します。  

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

10. **ソリューション エクスプローラー** で、 **[Views]** フォルダーの **NoteEntryPage.xaml** を展開し、**NoteEntryPage.xaml.cs** を開きます。 次に、`LoadNote`、`OnSaveButtonClicked`、`OnDeleteButtonClicked` メソッドを次のコードに置き換えます。

      ```csharp
      async void LoadNote(string itemId)
      {
          try
          {
              int id = Convert.ToInt32(itemId);
              // Retrieve the note and set it as the BindingContext of the page.
              Note note = await App.Database.GetNoteAsync(id);
              BindingContext = note;
          }
          catch (Exception)
          {
              Console.WriteLine("Failed to load note.");
          }
      }

      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          if (!string.IsNullOrWhiteSpace(note.Text))
          {
              await App.Database.SaveNoteAsync(note);
          }

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }
      ```    

      `NoteEntryPage` によって、`LoadNote` メソッドを使用して、ID がクエリ パラメーターとしてページに渡されたメモがデータベースから取得され、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に `Note` オブジェクトとして格納されます。 `OnSaveButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースに保存され、アプリケーションは前のページに戻ります。 `OnDeleteButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースから削除され、アプリケーションは前のページに戻ります。

      **Ctrl + S** キーを押して **NoteEntryPage.xaml.cs** への変更を保存します。  

11. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](app.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[追加]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。 アプリケーションを閉じて再起動し、入力したメモがデータベースに保存されていることを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動し、Notes ソリューションを開きます。

2. **Solution Pad** で、**Notes** ソリューションを右クリックし、 **[NuGet パッケージの管理...]** を選択します。

    ![NuGet パッケージの管理](database-images/vsmac/manage-nuget-packages.png)    

3. **[NuGet パッケージの管理]** ダイアログで **[参照]** タブを選択して、**sqlite-net-pcl** NuGet パッケージを検索します。

    > [!WARNING]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** SQLite-net
    > - **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    **[NuGet パッケージの管理]** ダイアログで **sqlite-net-pcl** パッケージを選択し、 **[パッケージの追加]** ボタンをクリックしてソリューションに追加します。

      ![sqlite-net-pcl を選択する](database-images/vsmac/select-package.png)

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

4. **[プロジェクトの選択]** ダイアログで、すべてのチェック ボックスがオンになっていることを確認して、 **[OK]** ボタンをクリックします。

    ![すべてのプロジェクトにパッケージを追加する](database-images/vsmac/add-package.png)

    これにより、ソリューション内のすべてのプロジェクトに NuGet パッケージが追加されます。

5. **Solution Pad** の **[Notes]** プロジェクトで、 **[Models]** フォルダーにある **[Note.cs]** を開き、既存のコードを次のコードに置き換えます。

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

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**Note.cs** への変更を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

6. **Solution Pad** で、 **[Data]** という名前の新しいフォルダーを **[Notes]** プロジェクトに追加します。

7. **Solution Pad** の **[Note]** プロジェクトで、 **[Data]** フォルダーに **[NoteDatabase]** という名前の新しいクラスを追加します。

8. **[NoteDatabase.cs]** で、既存のコードを次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection database;

            public NoteDatabase(string dbPath)
            {
                database = new SQLiteAsyncConnection(dbPath);
                database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                //Get all notes.
                return database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                // Get a specific note.
                return database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    // Update an existing note.
                    return database.UpdateAsync(note);
                }
                else
                {
                    // Save a new note.
                    return database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                // Delete a note.
                return database.DeleteAsync(note);
            }
        }
    }
    ```

    このクラスには、データベースを作成し、それに対してデータの読み込み、データの書き込み、データの削除を行うためのコードが含まれます。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`NoteDatabase` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは次の手順で `App` クラスによって提供されます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NoteDatabase.cs** への変更を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

9. **Solution Pad** で、**Notes** プロジェクトの **App.xaml** を展開し、**App.xaml.cs** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Notes.Data;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            // Create the database connection as a singleton.
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
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    このコードでは `Database` プロパティが定義されています。これによって、新しい `NoteDatabase` インスタンスがシングルトンとして作成され、データベースのファイル名が `NoteDatabase` コンストラクターに引数として渡されます。 データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**App.xaml.cs** への変更を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

10. **Solution Pad** の **Notes** プロジェクトで、 **[Views]** フォルダーの **NotesPage.xaml** を展開し、**NotesPage.xaml.cs** を開きます。 次に、`OnAppearing` と `OnSelectionChanged` のメソッドを次のコードに置き換えます。

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        // Retrieve all the notes from the database, and set them as the
        // data source for the CollectionView.
        collectionView.ItemsSource = await App.Database.GetNotesAsync();
    }

    async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        if (e.CurrentSelection != null)
        {
            // Navigate to the NoteEntryPage, passing the ID as a query parameter.
            Note note = (Note)e.CurrentSelection.FirstOrDefault();
            await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.ID.ToString()}");
        }
    }    
    ```    

    `OnAppearing` メソッドによって、データベースに格納されているメモが、[`CollectionView`](xref:Xamarin.Forms.CollectionView) に設定されます。 `OnSelectionChanged` メソッドによって、`NoteEntryPage` への移動が行われ、選択されている `Note` オブジェクトの `ID` プロパティがクエリ パラメーターとして渡されます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NotesPage.xaml.cs** への変更を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

11. **Solution Pad** で、 **[Views]** フォルダーの **NoteEntryPage.xaml** を展開し、**NoteEntryPage.xaml.cs** を開きます。 次に、`LoadNote`、`OnSaveButtonClicked`、`OnDeleteButtonClicked` メソッドを次のコードに置き換えます。

      ```csharp
      async void LoadNote(string itemId)
      {
          try
          {
              int id = Convert.ToInt32(itemId);
              // Retrieve the note and set it as the BindingContext of the page.
              Note note = await App.Database.GetNoteAsync(id);
              BindingContext = note;
          }
          catch (Exception)
          {
              Console.WriteLine("Failed to load note.");
          }
      }

      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          if (!string.IsNullOrWhiteSpace(note.Text))
          {
              await App.Database.SaveNoteAsync(note);
          }

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);

          // Navigate backwards
          await Shell.Current.GoToAsync("..");
      }
      ```    

      `NoteEntryPage` によって、`LoadNote` メソッドを使用して、ID がクエリ パラメーターとしてページに渡されたメモがデータベースから取得され、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に `Note` オブジェクトとして格納されます。 `OnSaveButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースに保存され、アプリケーションは前のページに戻ります。 `OnDeleteButtonClicked` イベント ハンドラーが実行されると、`Note` インスタンスがデータベースから削除され、アプリケーションは前のページに戻ります。

      **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NoteEntryPage.xaml.cs** への変更を保存します。

12. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](app.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[追加]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。 アプリケーションを閉じて再起動し、入力したメモがデータベースに保存されていることを確認します。

::: zone-end

## <a name="next-steps"></a>次のステップ

このクイックスタートでは、次の方法について学習しました。

- データを SQLite.NET データベースにローカルに格納する。

XAML スタイルを使用してアプリケーションのスタイルを設定するには、次のクイックスタートに進みます。

> [!div class="nextstepaction"]
> [次へ](styling.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Xamarin.Forms シェル クイックスタート Deep Dive](deepdive.md)
