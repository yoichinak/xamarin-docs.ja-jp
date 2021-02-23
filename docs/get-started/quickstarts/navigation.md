---
title: Xamarin.Forms アプリケーションでナビゲーションを実行する
description: この記事では、単一のメモを格納できる Xamarin.Forms シェル アプリケーションを、複数のメモを格納できるアプリケーションに変更する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 030204F7-E9E4-4AE3-B9F7-915B697D0171
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/28/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c218c7108ec70ab1ccd1b048e1655dba02c48660
ms.sourcegitcommit: 66ca2680c8fde18a55ce5daa3818edeeb9ba219b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "100569709"
---
# <a name="perform-navigation-in-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションでナビゲーションを実行する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/getstarted-notes-navigation/)

このクイックスタートでは、次の方法について学習します。

- Xamarin.Forms シェル アプリケーションにさらにページを追加する。
- ページ間のナビゲーションを実行する
- データ バインディングを使用して、ユーザー インターフェイスの要素とそのデータ ソースとの間でデータを同期する

このクイックスタートでは、単一のメモを格納できるクロスプラットフォームの Xamarin.Forms シェル アプリケーションを、複数のメモを格納できるアプリケーションに変更する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![Notes ページ](navigation-images/screenshots1-sml.png)](navigation-images/screenshots1.png#lightbox "Notes ページ")
[![Note Entry ページ](navigation-images/screenshots2-sml.png)](navigation-images/screenshots2.png#lightbox "Note Entry ページ")

### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試みるには、[前のクイックスタート](app.md)を正常に完了しておく必要があります。 または、[前のクイックスタートのサンプル](/samples/xamarin/xamarin-forms-samples/getstarted-notes-app/)をダウンロードし、それをこのクイックスタートの出発点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動します。 スタート ウィンドウの最近使ったプロジェクトまたはソリューションのリストで **Notes** ソリューションをクリックするか、 **[プロジェクトやソリューションを開く]** をクリックします。次に、 **[プロジェクト/ソリューションを開く]** ダイアログで、Notes プロジェクトのソリューション ファイルを選択します。

    ![[ソリューションを開く]](navigation-images/vs/open-solution.png)

2. **ソリューション エクスプローラー** で **Notes** プロジェクトを右クリックし、 **[追加] > [新しいフォルダー]** を選択します。

    ![新しいフォルダーの追加](navigation-images/vs/add-new-folder.png)

3. **ソリューション エクスプローラー** で、新しいフォルダーに **Models** という名前を付けます。

    ![Models フォルダー](navigation-images/vs/name-folder.png)

4. **ソリューション エクスプローラー** で、 **[Models]** フォルダーを選択して右クリックし、 **[追加] > [クラス...]** を選択します。

    ![新しいファイルの追加](navigation-images/vs/add-new-models-file.png)

5. **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム]、[クラス]** の順に選択し、新しいファイルに **Note** という名前を付け、 **[追加]** ボタンをクリックします。

    ![Note クラスの追加](navigation-images/vs/add-note-class.png)

    これにより、**Note** という名前のクラスが **Notes** プロジェクトの **Models** フォルダーに追加されます。

6. **Note.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    このクラスでは、アプリケーションでの各メモに関するデータを格納する `Note` モデルが定義されています。

    **Ctrl + S** キーを押して **Note.cs** への変更内容を保存します。  

7. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **[Views]** フォルダーを選択して右クリックし、 **[追加] > [新しい項目...]** を選択します。 **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム]、[Xamarin.Forms]、[コンテンツ ページ]** の順に選択し、新しいファイルに **NoteEntryPage** という名前を付け、 **[追加]** ボタンをクリックします。

    ![Xamarin.Forms の追加ContentPage](navigation-images/vs/add-note-entry-page.png)

    これにより、**NoteEntryPage** という名前の新しいページがプロジェクトの **[Views]** フォルダーに追加されます。 このページはメモの入力に使用されます。

8. **NoteEntryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.Views.NoteEntryPage"
                   Title="Note Entry">
          <!-- Layout children vertically -->
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <!-- Layout children in two columns -->
              <Grid ColumnDefinitions="*,*">
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor) と、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) オブジェクトで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Editor` と `Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 さらに、`Editor` ではデータ バインディングを使用して、`Note` モデルの `Text` プロパティにバインドします。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

      **Ctrl + S** キーを押して **NoteEntryPage.xaml** への変更内容を保存します。

9. **NoteEntryPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```csharp
      using System;
      using System.IO;
      using Notes.Models;
      using Xamarin.Forms;

      namespace Notes.Views
      {
          [QueryProperty(nameof(ItemId), nameof(ItemId))]
          public partial class NoteEntryPage : ContentPage
          {
              public string ItemId
              {
                  set
                  {
                      LoadNote(value);
                  }
              }

              public NoteEntryPage()
              {
                  InitializeComponent();

                  // Set the BindingContext of the page to a new Note.
                  BindingContext = new Note();
              }

              void LoadNote(string filename)
              {
                  try
                  {
                      // Retrieve the note and set it as the BindingContext of the page.
                      Note note = new Note
                      {
                          Filename = filename,
                          Text = File.ReadAllText(filename),
                          Date = File.GetCreationTime(filename)
                      };
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

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save the file.
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update the file.
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  // Delete the file.
                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }
          }
      }
      ```

      このコードにより、単一のメモを表す `Note` インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に格納されます。 クラスは、ナビゲーション中にクエリ パラメーターを使用してページにデータを渡すことができるように、`QueryPropertyAttribute` で修飾されています。 `QueryPropertyAttribute` の最初の引数には、データを受信するプロパティの名前を指定し、2 番目の引数にはクエリ パラメータ― ID を指定します。そのため、上のコードの `QueryParameterAttribute` において、`GoToAsync` メソッドの呼び出しで指定されている URI から `ItemId` クエリ パラメーターで渡されたデータを `ItemId` プロパティが受け取るように指定されています。 その後、`ItemId` プロパティによって `LoadNote` メソッドが呼び出され、デバイス上のファイルから `Note` オブジェクトが作成されて、ページの `BindingContext` が `Note` オブジェクトに設定されます。

      **[保存]** [`Button`](xref:Xamarin.Forms.Button) を押すと `OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` の内容がランダムに生成されたファイル名の新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカル アプリケーションのデータ フォルダーに格納されます。 その後、メソッドによって前のページに戻ります。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、前のページに戻ります。 ナビゲーションの詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **Ctrl + S** キーを押して **NoteEntryPage.xaml.cs** への変更を保存します。

      > [!WARNING]
      > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

10. **ソリューション エクスプローラー** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **NotesPage.xaml** を開きます。

11. **NotesPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="20"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium"/>
                        <Label Text="{Binding Date}"
                               TextColor="Silver"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    このコードにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) と [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) から構成されるページのユーザー インターフェイスが宣言によって定義されます。 `CollectionView` により、データ バインディングを使用して、アプリケーションによって取得されたすべてのメモが表示されます。 メモを選択すると、ノートを変更できる `NoteEntryPage` に移動します。 または、`ToolbarItem` を押して、新しいメモを作成することもできます。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

    **Ctrl + S** キーを押して **NotesPage.xaml** への変更を保存します。

12. **ソリューション エクスプローラー** の **Notes** プロジェクトで、 **[Views]** フォルダーの **NotesPage.xaml** を展開し、**NotesPage.xaml.cs** を開きます。

13. **NotesPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Notes.Models;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                // Create a Note object from each file.
                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                // Set the data source for the CollectionView to a
                // sorted collection of notes.
                collectionView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnAddClicked(object sender, EventArgs e)
            {
                // Navigate to the NoteEntryPage, without passing any data.
                await Shell.Current.GoToAsync(nameof(NoteEntryPage));
            }

            async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
            {
                if (e.CurrentSelection != null)
                {
                    // Navigate to the NoteEntryPage, passing the filename as a query parameter.
                    Note note = (Note)e.CurrentSelection.FirstOrDefault();
                    await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.Filename}");
                }
            }
        }
    }
    ```

    このコードにより、`NotesPage` の機能が定義されます。 このページが表示されると、`OnAppearing` メソッドが実行されます。これにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) に、ローカル アプリケーションのデータ フォルダーから取得されたすべてのメモが挿入されます。 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) を押すと、`OnAddClicked` イベント ハンドラーが実行されます。 このメソッドにより、`NoteEntryPage` に移動します。 `CollectionView` 内の項目が選択されると、`OnSelectionChanged` イベント ハンドラーが実行されます。 [`CollectionView`](xref:Xamarin.Forms.CollectionView) で項目が選択されている場合、このメソッドにより `NoteEntryPage` に移動し、選択されている `Note` の `Filename` プロパティがクエリ パラメーターとしてページに渡されます。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **Ctrl + S** キーを押して **NotesPage.xaml.cs** への変更を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

14. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **AppShell.xaml** を展開し、**AppShell.xaml.cs** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```csharp
    using Notes.Views;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
                Routing.RegisterRoute(nameof(NoteEntryPage), typeof(NoteEntryPage));
            }
        }
    }
    ```

    このコードにより、`NoteEntryPage` へのルートが登録されます。これは、シェルのビジュアル階層 (**AppShell.xaml**) では表されません。 その後、このページには、`GoToAsync` メソッドを使用する URI ベースのナビゲーションを使用して移動できます。

    **Ctrl + S** キーを押して **AppShell.xaml.cs** への変更を保存します。

15. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **App.xaml** を展開し、**App.xaml.cs** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
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

    このコードにより、`System.IO` 名前空間の名前空間宣言が追加され、`string`型の静的な `FolderPath` プロパティの宣言が追加されます。 `FolderPath` プロパティは、メモ データが格納されるデバイス上のパスを格納するために使用されます。 さらに、このコードにより `App` コンストラクターの `FolderPath` プロパティが初期化され、[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティがサブクラス化された `Shell` オブジェクトに初期化されます。

    **Ctrl + S** キーを押して **App.xaml.cs** への変更を保存します。

16. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](app.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[追加]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。 アプリケーションを閉じて再起動し、入力したメモがデバイスに保存されていることを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動します。 スタート ウィンドウで **[開く]** をクリックし、ダイアログで Notes プロジェクトのソリューション ファイルを選択します。

    ![[ソリューションを開く]](navigation-images/vsmac/open-solution.png)

2. **Solution Pad** で **Notes** プロジェクトを右クリックし、 **[追加] > [新しいフォルダー]** を選択します。

    ![新しいフォルダーの追加](navigation-images/vsmac/add-new-folder.png)

3. **[新しいフォルダー]** ダイアログで、新しいフォルダーに「**Models**」という名前を設定します。

    ![Models フォルダー](navigation-images/vsmac/name-folder.png)

4. **Solution Pad** で **[Models]** フォルダーを選択して右クリックし、 **[追加] > [新しいクラス...]** を選択します。

    ![新しいファイルの追加](navigation-images/vsmac/add-new-models-file.png)

5. **[新しいファイル]** ダイアログで **[全般]、[空のクラス]** の順に選択して、新しいファイルに **Note** という名前を付け **[新規]** ボタンをクリックします。

    ![Note クラスの追加](navigation-images/vsmac/add-note-class.png)

    これにより、**Note** という名前のクラスが **Notes** プロジェクトの **Models** フォルダーに追加されます。

6. **Note.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    このクラスでは、アプリケーションでの各メモに関するデータを格納する `Note` モデルが定義されています。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**Note.cs** への変更を保存します。

7. **Solution Pad** で **[Notes]** プロジェクトを選択して右クリックし、 **[追加]、[新しいファイル]** の順に選択します。 **[新しいファイル]** ダイアログで、 **[フォーム]、[フォーム ContentPage XAML]** の順に選択して、新しいファイルに **NoteEntryPage** という名前を付け、 **[新規]** ボタンをクリックします。

    ![Xamarin.Forms の追加ContentPage](navigation-images/vsmac/add-note-entry-page.png)

    これにより、**NoteEntryPage** という名前の新しいページがプロジェクトの **[Views]** フォルダーに追加されます。 このページはメモの入力に使用されます。

8. **NoteEntryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.Views.NoteEntryPage"
                   Title="Note Entry">
          <!-- Layout children vertically -->
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <!-- Layout children in two columns -->
              <Grid ColumnDefinitions="*,*">
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor) と、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) オブジェクトで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Editor` と `Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 さらに、`Editor` ではデータ バインディングを使用して、`Note` モデルの `Text` プロパティにバインドします。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

      **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NoteEntryPage.xaml** への変更を保存します。

9. **NoteEntryPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```csharp
      using System;
      using System.IO;
      using Notes.Models;
      using Xamarin.Forms;

      namespace Notes.Views
      {
          [QueryProperty(nameof(ItemId), nameof(ItemId))]
          public partial class NoteEntryPage : ContentPage
          {
              public string ItemId
              {
                  set
                  {
                      LoadNote(value);
                  }
              }

              public NoteEntryPage()
              {
                  InitializeComponent();

                  // Set the BindingContext of the page to a new Note.
                  BindingContext = new Note();
              }

              void LoadNote(string filename)
              {
                  try
                  {
                      // Retrieve the note and set it as the BindingContext of the page.
                      Note note = new Note
                      {
                          Filename = filename,
                          Text = File.ReadAllText(filename),
                          Date = File.GetCreationTime(filename)
                      };
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

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save the file.
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update the file.
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  // Delete the file.
                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  // Navigate backwards
                  await Shell.Current.GoToAsync("..");
              }
          }
      }
      ```

      このコードにより、単一のメモを表す `Note` インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に格納されます。 クラスは、ナビゲーション中にクエリ パラメーターを使用してページにデータを渡すことができるように、`QueryPropertyAttribute` で修飾されています。 `QueryPropertyAttribute` の最初の引数には、データを受信するプロパティの名前を指定し、2 番目の引数にはクエリ パラメータ― ID を指定します。そのため、上のコードの `QueryParameterAttribute` において、`GoToAsync` メソッドの呼び出しで指定されている URI から `ItemId` クエリ パラメーターで渡されたデータを `ItemId` プロパティが受け取るように指定されています。 その後、`ItemId` プロパティによって `LoadNote` メソッドが呼び出され、デバイス上のファイルから `Note` オブジェクトが作成されて、ページの `BindingContext` が `Note` オブジェクトに設定されます。

      **[保存]** [`Button`](xref:Xamarin.Forms.Button) を押すと `OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` の内容がランダムに生成されたファイル名の新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカル アプリケーションのデータ フォルダーに格納されます。 その後、メソッドによって前のページに戻ります。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、前のページに戻ります。 ナビゲーションの詳細については、[Xamarin.Forms シェル クイックスタート Deep Dive](deepdive.md) に関するページの「[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NoteEntryPage.xaml.cs** への変更を保存します。

      > [!WARNING]
      > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

10. **Solution Pad** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **NotesPage.xaml** を開きます。

11. **NotesPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="20"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium"/>
                        <Label Text="{Binding Date}"
                               TextColor="Silver"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    このコードにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) と [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) から構成されるページのユーザー インターフェイスが宣言によって定義されます。 `CollectionView` により、データ バインディングを使用して、アプリケーションによって取得されたすべてのメモが表示されます。 メモを選択すると、ノートを変更できる `NoteEntryPage` に移動します。 または、`ToolbarItem` を押して、新しいメモを作成することもできます。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NotesPage.xaml** への変更を保存します。

12. **Solution Pad** の **Notes** プロジェクトで、 **[Views]** フォルダーの **NotesPage.xaml** を展開し、**NotesPage.xaml.cs** を開きます。

13. **NotesPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Notes.Models;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                // Create a Note object from each file.
                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                // Set the data source for the CollectionView to a
                // sorted collection of notes.
                collectionView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnAddClicked(object sender, EventArgs e)
            {
                // Navigate to the NoteEntryPage, without passing any data.
                await Shell.Current.GoToAsync(nameof(NoteEntryPage));
            }

            async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
            {
                if (e.CurrentSelection != null)
                {
                    // Navigate to the NoteEntryPage, passing the filename as a query parameter.
                    Note note = (Note)e.CurrentSelection.FirstOrDefault();
                    await Shell.Current.GoToAsync($"{nameof(NoteEntryPage)}?{nameof(NoteEntryPage.ItemId)}={note.Filename}");
                }
            }
        }
    }
    ```

    このコードにより、`NotesPage` の機能が定義されます。 このページが表示されると、`OnAppearing` メソッドが実行されます。これにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) に、ローカル アプリケーションのデータ フォルダーから取得されたすべてのメモが挿入されます。 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) を押すと、`OnAddClicked` イベント ハンドラーが実行されます。 このメソッドにより、`NoteEntryPage` に移動します。 `CollectionView` 内の項目が選択されると、`OnSelectionChanged` イベント ハンドラーが実行されます。 [`CollectionView`](xref:Xamarin.Forms.CollectionView) で項目が選択されている場合、このメソッドにより `NoteEntryPage` に移動し、選択されている `Note` の `Filename` プロパティがクエリ パラメーターとしてページに渡されます。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NotesPage.xaml.cs** への変更を保存します。

    > [!WARNING]
    > 以降の手順で修正されるエラーのため、アプリケーションは現在はビルドされません。

14. **Solution Pad** で、**Notes** プロジェクトの **AppShell.xaml** を展開し、**AppShell.xaml.cs** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```csharp
    using Notes.Views;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
                Routing.RegisterRoute(nameof(NoteEntryPage), typeof(NoteEntryPage));
            }
        }
    }
    ```

    このコードにより、`NoteEntryPage` へのルートが登録されます。これは、シェルのビジュアル階層では表されません。 その後、このページには、`GoToAsync` メソッドを使用する URI ベースのナビゲーションを使用して移動できます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AppShell.xaml.cs** への変更を保存します。

15. **Solution Pad** で、**Notes** プロジェクトの **App.xaml** を展開し、**App.xaml.cs** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
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

    このコードにより、`System.IO` 名前空間の名前空間宣言が追加され、`string`型の静的な `FolderPath` プロパティの宣言が追加されます。 `FolderPath` プロパティは、メモ データが格納されるデバイス上のパスを格納するために使用されます。 さらに、このコードにより `App` コンストラクターの `FolderPath` プロパティが初期化され、[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティがサブクラス化された `Shell` オブジェクトに初期化されます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**App.xaml.cs** への変更を保存します。

16. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](app.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[追加]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。 アプリケーションを閉じて再起動し、入力したメモがデバイスに保存されていることを確認します。

::: zone-end

## <a name="next-steps"></a>次のステップ

このクイックスタートでは、次の方法について学習しました。

- Xamarin.Forms シェル アプリケーションにさらにページを追加する。
- ページ間のナビゲーションを実行する
- データ バインディングを使用して、ユーザー インターフェイスの要素とそのデータ ソースとの間でデータを同期する

次のクイックスタートに進み、ローカル環境の SQLite.NET データベースにデータを格納するようにアプリケーションを変更します。

> [!div class="nextstepaction"]
> [次へ](database.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-navigation/)
- [Xamarin.Forms シェル クイックスタート Deep Dive](deepdive.md)
