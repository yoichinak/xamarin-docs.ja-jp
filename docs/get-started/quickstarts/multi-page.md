---
title: 複数ページの Xamarin.Forms アプリケーションでナビゲーションを実行する
description: この記事では、単一のメモを格納できるシングルページ アプリケーションを、複数のメモを格納できる複数ページのアプリケーションに変更する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 9ce02b4c6412eab1f4b1003b262573c59940286c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "68653793"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>複数ページの Xamarin.Forms アプリケーションでナビゲーションを実行する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

このクイックスタートでは、次の方法について学習します。

- Xamarin. Forms ソリューションにページを追加する
- ページ間のナビゲーションを実行する
- データ バインディングを使用して、ユーザー インターフェイスの要素とそのデータ ソースとの間でデータを同期する

このクイックスタートでは、単一のメモを格納できるクロスプラットフォームの Xamarin.Forms のシングルページ アプリケーションを、複数のメモを格納できる複数ページのアプリケーションに変更する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![](multi-page-images/screenshots1-sml.png "Notes Page")](multi-page-images/screenshots1.png#lightbox "Notes Page")
[![](multi-page-images/screenshots2-sml.png "Note Entry Page")](multi-page-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試みるには、[前のクイックスタート](single-page.md)を正常に完了しておく必要があります。 または、[前のクイックスタートのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)をダウンロードし、それをこのクイックスタートの出発点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動します。 スタート ウィンドウの最近使ったプロジェクトまたはソリューションのリストで **Notes** ソリューションをクリックするか、 **[プロジェクトやソリューションを開く]** をクリックします。次に、 **[プロジェクト/ソリューションを開く]** ダイアログで、Notes プロジェクトのソリューション ファイルを選択します。

    ![](multi-page-images/vs/open-solution.png "Open Project")

2. **ソリューション エクスプローラー**で、 **[Notes]** プロジェクトを右クリックし、 **[追加]、[新しいフォルダー]** の順に選択します。

    ![](multi-page-images/vs/add-new-item.png "Add New Item")

3. **ソリューション エクスプローラー**で、新しいフォルダーに **Models** という名前を付けます。

    ![](multi-page-images/vs/name-folder.png "Models Folder")

4. **ソリューション エクスプローラー**で、 **[Models]** フォルダーを選択して右クリックし、 **[追加]、[新しい項目]** の順に選択します。

    ![](multi-page-images/vs/add-new-models-file.png "Add New File")

5. **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム]、[クラス]** の順に選択し、新しいファイルに **Note** という名前を付け、 **[追加]** ボタンをクリックします。

    ![](multi-page-images/vs/add-note-class.png "Add Note Class")

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

    **Ctrl + S** キーを押して **Note.cs** への変更内容を保存してから、ファイルを閉じます。

7. **ソリューション エクスプローラー**で、 **[Notes]** プロジェクトを右クリックし、 **[追加]、[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム]、[Xamarin.Forms]、[コンテンツ ページ]** の順に選択し、新しいファイルに **NoteEntryPage** という名前を付け、 **[追加]** ボタンをクリックします。

    ![](multi-page-images/vs/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    これにより、**NoteEntryPage** という名前の新しいページがプロジェクトのルート フォルダーに追加されます。 このページは、アプリケーションの 2 ページ目になります。

8. **NoteEntryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Editor` と `Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 さらに、`Editor` ではデータ バインディングを使用して、`Note` モデルの `Text` プロパティにバインドします。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

      **Ctrl + S** キーを押して **NoteEntryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

9. **NoteEntryPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      このコードにより、単一のメモを表す `Note` インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に格納されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) を押すと `OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` の内容がランダムに生成されたファイル名の新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカル アプリケーションのデータ フォルダーに格納されます。 その後、メソッドによって前のページに戻ります。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、前のページに戻ります。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **Ctrl + S** キーを押して **NoteEntryPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

10. **ソリューション エクスプローラー**で、 **[Notes]** プロジェクトを右クリックし、 **[追加]、[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム]、[Xamarin.Forms]、[コンテンツ ページ]** の順に選択し、新しいファイルに **NotesPage** という名前を付け、 **[追加]** ボタンをクリックします。

      これにより、**NotesPage** という名前のページがプロジェクトのルート フォルダーに追加されます。 このページは、アプリケーションのルート ページになります。

11. **NotesPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    このコードにより、[`ListView`](xref:Xamarin.Forms.ListView) と [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) から構成されるページのユーザー インターフェイスが宣言によって定義されます。 `ListView` では、データ バインディングを使用して、アプリケーションによって取得されたすべてのメモを表示します。メモを選択すると、ノートを変更できる `NoteEntryPage` に移動します。 または、`ToolbarItem` を押して、新しいメモを作成することもできます。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

    **Ctrl + S** キーを押して **NotesPage.xaml** への変更内容を保存してから、ファイルを閉じます。

12. **NotesPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
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

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    このコードにより、`NotesPage` の機能が定義されます。 このページが表示されると、`OnAppearing` メソッドが実行されます。これにより、[`ListView`](xref:Xamarin.Forms.ListView) に、ローカル アプリケーションのデータ フォルダーから取得されたすべてのメモが挿入されます。 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) を押すと、`OnNoteAddedClicked` イベント ハンドラーが実行されます。 このメソッドにより `NoteEntryPage` に移動し、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を新しい `Note` インスタンスに設定します。 `ListView` 内の項目が選択されると、`OnListViewItemSelected` イベント ハンドラーが実行されます。 このメソッドにより `NoteEntryPage` に移動し、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を選択した `Note` インスタンスに設定します。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **CTRL + S** キーを押して **NotesPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

13. **ソリューション エクスプローラー**で、**App.xaml.cs** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    このコードにより、`System.IO` 名前空間の名前空間宣言が追加され、`string`型の静的な `FolderPath` プロパティの宣言が追加されます。 `FolderPath` プロパティは、メモ データが格納されるデバイス上のパスを格納するために使用されます。 さらに、このコードにより `App` コンストラクターの `FolderPath` プロパティが初期化され、`NotesPage` のインスタンスをホストする [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) にする [`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティが初期化されます。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **ソリューション エクスプローラー**で、 **[Notes]** プロジェクトの **[MainPage.xaml]** を右クリックし、 **[削除]** を選択します。 表示されたダイアログで、 **[OK]** ボタンを押して、ハード ディスクからファイルを削除します。

    これにより、使用されなくなったページが削除されます。

15. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[+]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動します。 スタート ウィンドウで **[開く]** をクリックし、ダイアログで Notes プロジェクトのソリューション ファイルを選択します。

    ![](multi-page-images/vsmac/open-solution.png "Open Solution")

2. **Solution Pad** で **[Notes]** プロジェクトを選択して右クリックし、 **[追加]、[新しいフォルダー]** の順に選択します。

    ![](multi-page-images/vsmac/add-new-folder.png "Add New Folder")

3. **Solution Pad** で、新しいフォルダーに **Models** という名前を付けます。

    ![](multi-page-images/vsmac/name-folder.png "Models Folder")

4. **Solution Pad** で、 **[Models]** フォルダーを選択して右クリックし、 **[追加]、[新しいファイル]** の順に選択します。

    ![](multi-page-images/vsmac/add-new-models-file.png "Add New File")

5. **[新しいファイル]** ダイアログで **[全般]、[空のクラス]** の順に選択して、新しいファイルに **Note** という名前を付け **[新規]** ボタンをクリックします。

    ![](multi-page-images/vsmac/add-note-class.png "Add Note Class")

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

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**Note.cs** への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad** で **[Notes]** プロジェクトを選択して右クリックし、 **[追加]、[新しいファイル]** の順に選択します。 **[新しいファイル]** ダイアログで、 **[フォーム]、[フォーム ContentPage XAML]** の順に選択して、新しいファイルに **NoteEntryPage** という名前を付け、 **[新規]** ボタンをクリックします。

    ![](multi-page-images/vsmac/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    これにより、**NoteEntryPage** という名前の新しいページがプロジェクトのルート フォルダーに追加されます。 このページは、アプリケーションの 2 ページ目になります。

8. **NoteEntryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Editor` と `Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 さらに、`Editor` ではデータ バインディングを使用して、`Note` モデルの `Text` プロパティにバインドします。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

      **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NoteEntryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

9. **NoteEntryPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      このコードにより、単一のメモを表す `Note` インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に格納されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) を押すと `OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` の内容がランダムに生成されたファイル名の新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカル アプリケーションのデータ フォルダーに格納されます。 その後、メソッドによって前のページに戻ります。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、前のページに戻ります。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NoteEntryPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

10. **Solution Pad** で **[Notes]** プロジェクトを選択して右クリックし、 **[追加]、[新しいファイル]** の順に選択します。 **[新しいファイル]** ダイアログで、 **[フォーム]、[フォーム ContentPage XAML]** の順に選択して、新しいファイルに **NotesPage** という名前を付け、 **[新規]** ボタンをクリックします。

      これにより、**NotesPage** という名前のページがプロジェクトのルート フォルダーに追加されます。 このページは、アプリケーションのルート ページになります。

11. **NotesPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    このコードにより、[`ListView`](xref:Xamarin.Forms.ListView) と [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) から構成されるページのユーザー インターフェイスが宣言によって定義されます。 `ListView` では、データ バインディングを使用して、アプリケーションによって取得されたすべてのメモを表示します。メモを選択すると、ノートを変更できる `NoteEntryPage` に移動します。 または、`ToolbarItem` を押して、新しいメモを作成することもできます。 データ バインディングの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[データ バインディング](deepdive.md#data-binding)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NotesPage.xaml** への変更内容を保存してから、ファイルを閉じます。

12. **NotesPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
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

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    このコードにより、`NotesPage` の機能が定義されます。 このページが表示されると、`OnAppearing` メソッドが実行されます。これにより、[`ListView`](xref:Xamarin.Forms.ListView) に、ローカル アプリケーションのデータ フォルダーから取得されたすべてのメモが挿入されます。 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) を押すと、`OnNoteAddedClicked` イベント ハンドラーが実行されます。 このメソッドにより `NoteEntryPage` に移動し、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を新しい `Note` インスタンスに設定します。 `ListView` 内の項目が選択されると、`OnListViewItemSelected` イベント ハンドラーが実行されます。 このメソッドにより `NoteEntryPage` に移動し、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を選択した `Note` インスタンスに設定します。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**NotesPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、これは以降の手順で修正します。

13. **Solution Pad** で **App.xaml.cs** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    このコードにより、`System.IO` 名前空間の名前空間宣言が追加され、`string`型の静的な `FolderPath` プロパティの宣言が追加されます。 `FolderPath` プロパティは、メモ データが格納されるデバイス上のパスを格納するために使用されます。 さらに、このコードにより `App` コンストラクターの `FolderPath` プロパティが初期化され、`NotesPage` のインスタンスをホストする [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) にする [`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティが初期化されます。 ナビゲーションの詳細については、「[Xamarin.Forms クイックスタートの詳細](deepdive.md)」の「[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **Solution Pad** で、 **[Notes]** プロジェクトの **[MainPage.xaml]** を右クリックし、 **[削除]** を選択します。 表示されたダイアログで、 **[削除]** ボタンを押して、ハード ディスクからファイルを削除します。

    これにより、使用されなくなったページが削除されます。

15. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[+]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 メモを保存すると、アプリケーションは **NotesPage** に戻ります。

    さまざまな長さの複数のメモを入力して、アプリケーションの動作を観察します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- Xamarin. Forms ソリューションにページを追加する
- ページ間のナビゲーションを実行する
- データ バインディングを使用して、ユーザー インターフェイスの要素とそのデータ ソースとの間でデータを同期する

ローカルの SQLite.NET データベースにデータを格納するようにアプリケーションを変更するには、次のクイックスタートに進みます。

> [!div class="nextstepaction"]
> [次へ](database.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Xamarin.Forms クイックスタートの詳細](deepdive.md)
