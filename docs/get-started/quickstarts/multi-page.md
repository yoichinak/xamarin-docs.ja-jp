---
title: 複数ページの Xamarin.Forms アプリケーションでナビゲーションを実行する
description: この記事では、1つのメモを複数ページアプリケーションに格納できるシングルページアプリケーションを、複数のメモを格納できるようにする方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 9ce02b4c6412eab1f4b1003b262573c59940286c
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68653793"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>複数ページの Xamarin. フォームアプリケーションでナビゲーションを実行する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

このクイックスタートでは、次の方法について説明します。

- Xamarin. Forms ソリューションにページを追加します。
- ページ間のナビゲーションを実行します。
- ユーザーインターフェイス要素とそのデータソースとの間でデータを同期するには、データバインディングを使用します。

このクイックスタートでは、1つのノートを複数ページアプリケーションに格納できる単一ページクロスプラットフォーム Xamarin. Forms アプリケーションを、複数のメモを格納できるようにする方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![](multi-page-images/screenshots1-sml.png "Notes Page")](multi-page-images/screenshots1.png#lightbox "Notes Page")
[![](multi-page-images/screenshots2-sml.png "Note Entry Page")](multi-page-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>必要条件

このクイックスタートを試行する前に、[前のクイックスタート](single-page.md)を正常に完了している必要があります。 または、[前のクイックスタートサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)をダウンロードし、このクイックスタートの出発点として使用してください。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動します。 スタート ウィンドウで、最近使ったプロジェクト/ソリューション の一覧で  **note** ソリューションをクリックするか、**プロジェクトまたはソリューション**を開く をクリックします。次に、**プロジェクト/ソリューションを開く** ダイアログで、メモプロジェクトのソリューションファイルを選択します。

    ![](multi-page-images/vs/open-solution.png "Open Project")

2. **ソリューションエクスプローラー**で、**メモ**プロジェクトを右クリックし、 **[> 新しいフォルダーの追加]** を選択します。

    ![](multi-page-images/vs/add-new-item.png "Add New Item")

3. **ソリューションエクスプローラー**で、新しいフォルダー**モデル**にという名前を指定します。

    ![](multi-page-images/vs/name-folder.png "Models Folder")

4. **ソリューションエクスプローラー**で、 **[モデル]** フォルダーを選択し、右クリックして、 **[新しい項目の追加 >]** を選択します。

    ![](multi-page-images/vs/add-new-models-file.png "Add New File")

5. **[新しい項目の追加]** ダイアログで、 **[ビジュアルC#項目 > クラス]** を選択し、新しいファイルに「 **Note**」という名前を指定して、 **[追加]** ボタンをクリックします。

    ![](multi-page-images/vs/add-note-class.png "Add Note Class")

    これにより、 **note**という名前のクラスが**Notes**プロジェクトの **[モデル]** フォルダーに追加されます。

6. **Note.cs**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

    このクラスは、アプリケーションの各メモに関するデータを格納する `Note` モデルを定義します。    

    **CTRL + S**キーを押して**Note.cs**への変更内容を保存し、ファイルを閉じます。

7. **ソリューションエクスプローラー**で、**メモ**プロジェクトを右クリックし、 **[> 新しい項目の追加]** を選択します。 **[新しい項目の追加]** ダイアログで、[**ビジュアルC#項目 > Xamarin. Forms > コンテンツ] ページ**を選択し、新しいファイルに**NoteEntryPage**という名前を指定して、 **[追加]** ボタンをクリックします。

    ![](multi-page-images/vs/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    これにより、 **NoteEntryPage**という名前の新しいページがプロジェクトのルートフォルダーに追加されます。 このページは、アプリケーションの2番目のページになります。

8. **NoteEntryPage**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

      このコードは、宣言によってページのユーザーインターフェイスを定義します。これは、テキスト入力の[`Editor`](xref:Xamarin.Forms.Editor)と、アプリケーションにファイルの保存または削除を指示する2つの[`Button`](xref:Xamarin.Forms.Button)インスタンスで構成されます。 2つの `Button` インスタンスは[`Grid`](xref:Xamarin.Forms.Grid)に水平方向に配置され、`Editor` と `Grid` は[`StackLayout`](xref:Xamarin.Forms.StackLayout)に垂直方向にレイアウトされます。 さらに、`Editor` はデータバインディングを使用して、`Note` モデルの `Text` プロパティにバインドします。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

      **CTRL + S**キーを押して**NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

9. **NoteEntryPage.xaml.cs**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

      このコードでは、1つのメモを表す `Note` インスタンスがページの[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)に格納されます。 **保存** [`Button`](xref:Xamarin.Forms.Button)が押されると `OnSaveButtonClicked` イベントハンドラーが実行されます。これにより、`Editor` の内容がランダムに生成されたファイル名を持つ新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカルアプリケーションデータフォルダーに格納されます。 次に、メソッドは前のページに戻ります。 **Delete** `Button` が押されると `OnDeleteButtonClicked` イベントハンドラーが実行され、ファイルが存在する場合はそのファイルが削除され、前のページに戻ります。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **CTRL + S**キーを押して**NoteEntryPage.xaml.cs**への変更内容を保存し、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

10. **ソリューションエクスプローラー**で、**メモ**プロジェクトを右クリックし、 **[> 新しい項目の追加]** を選択します。 **[新しい項目の追加]** ダイアログで、[**ビジュアルC#項目 > Xamarin. Forms > コンテンツ] ページ**を選択し、新しいファイルに「**ノート**」という名前を指定して、 **[追加]** ボタンをクリックします。

      これにより、プロジェクトのルートフォルダーに [**ノート] ページ**という名前のページが追加されます。 このページは、アプリケーションのルートページになります。

11. [ファイル **] [.xaml**] で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)と[`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)で構成されるページのユーザーインターフェイスを宣言によって定義します。 @No__t_0 は、データバインディングを使用して、アプリケーションによって取得されるすべてのメモを表示します。メモを選択すると、ノートを変更できる `NoteEntryPage` に移動します。 または、`ToolbarItem` を押して新しいメモを作成することもできます。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

    **CTRL + S**キーを押して、変更内容を [**ノート] ページ**に保存し、ファイルを閉じます。

12. **NotesPage.xaml.cs**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

    このコードでは、`NotesPage` の機能を定義します。 このページが表示されると、`OnAppearing` メソッドが実行され、ローカルアプリケーションデータフォルダーから取得されたすべてのメモが[`ListView`](xref:Xamarin.Forms.ListView)に設定されます。 [@No__t_1](xref:Xamarin.Forms.ToolbarItem)が押されると `OnNoteAddedClicked` イベントハンドラーが実行されます。 このメソッドは、`NoteEntryPage` に移動し、`NoteEntryPage` の[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)を新しい `Note` インスタンスに設定します。 @No__t_0 内の項目が選択されると、`OnListViewItemSelected` イベントハンドラーが実行されます。 このメソッドは、`NoteEntryPage` に移動し、選択した `Note` インスタンスに `NoteEntryPage` の[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)を設定します。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **CTRL + S**キーを押して**NotesPage.xaml.cs**への変更内容を保存し、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

13. **ソリューションエクスプローラー**で、 **[App.xaml.cs]** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードは `System.IO` 名前空間の名前空間宣言を追加し、`string` 型の静的な `FolderPath` プロパティの宣言を追加します。 @No__t_0 プロパティは、ノートデータが格納されるデバイス上のパスを格納するために使用されます。 さらに、このコードは `App` コンストラクターの `FolderPath` プロパティを初期化し、 [`MainPage`](xref:Xamarin.Forms.Application.MainPage)プロパティを初期化して、`NotesPage` のインスタンスをホストする[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)にします。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **mainpage.xaml**を右クリックし、 **[削除]** を選択します。 表示されるダイアログで、 **[OK** ] をクリックして、ハードディスクからファイルを削除します。

    これにより、使用されなくなったページが削除されます。

15. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、[ **+** ] ボタンをクリックして**NoteEntryPage**に移動し、メモを入力します。 メモを保存すると、アプリケーションは [ノート **] ページ**に戻ります。

    さまざまな長さのメモを入力して、アプリケーションの動作を観察します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動します。 スタート ウィンドウで **開く** をクリックし、ダイアログボックスで、メモプロジェクトのソリューションファイルを選択します。

    ![](multi-page-images/vsmac/open-solution.png "Open Solution")

2. **Solution Pad**で、**メモ**プロジェクトを選択して右クリックし、 **[> 新しいフォルダーの追加]** を選択します。

    ![](multi-page-images/vsmac/add-new-folder.png "Add New Folder")

3. **Solution Pad**で、新しいフォルダー**モデル**にという名前を指定します。

    ![](multi-page-images/vsmac/name-folder.png "Models Folder")

4. **Solution Pad**で、 **[モデル]** フォルダーを選択し、右クリックして、 **[新しいファイルを追加 >]** を選択します。

    ![](multi-page-images/vsmac/add-new-models-file.png "Add New File")

5. **[新しいファイル]** ダイアログで、 **[全般 > 空のクラス**] を選択し、新しいファイルに「 **Note**」という名前を指定して、 **[新規]** ボタンをクリックします。

    ![](multi-page-images/vsmac/add-note-class.png "Add Note Class")

    これにより、 **note**という名前のクラスが**Notes**プロジェクトの **[モデル]** フォルダーに追加されます。

6. **Note.cs**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

    このクラスは、アプリケーションの各メモに関するデータを格納する `Note` モデルを定義します。

    **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **Note.cs**への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad**で、**メモ**プロジェクトを選択し、右クリックして、 **[> 新しいファイルの追加]** を選択します。 **[新しいファイル]** ダイアログで、 **[フォーム > フォーム ContentPage XAML**] を選択し、新しいファイルに**NoteEntryPage**という名前を指定して、 **[新規]** ボタンをクリックします。

    ![](multi-page-images/vsmac/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    これにより、 **NoteEntryPage**という名前の新しいページがプロジェクトのルートフォルダーに追加されます。 このページは、アプリケーションの2番目のページになります。

8. **NoteEntryPage**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

      このコードは、宣言によってページのユーザーインターフェイスを定義します。これは、テキスト入力の[`Editor`](xref:Xamarin.Forms.Editor)と、アプリケーションにファイルの保存または削除を指示する2つの[`Button`](xref:Xamarin.Forms.Button)インスタンスで構成されます。 2つの `Button` インスタンスは[`Grid`](xref:Xamarin.Forms.Grid)に水平方向に配置され、`Editor` と `Grid` は[`StackLayout`](xref:Xamarin.Forms.StackLayout)に垂直方向にレイアウトされます。 さらに、`Editor` はデータバインディングを使用して、`Note` モデルの `Text` プロパティにバインドします。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

      **[ファイル > 保存]** を選択し (または、  **&#8984; + S**キーを押して)、 **NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

9. **NoteEntryPage.xaml.cs**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

      このコードでは、1つのメモを表す `Note` インスタンスがページの[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)に格納されます。 **保存** [`Button`](xref:Xamarin.Forms.Button)が押されると `OnSaveButtonClicked` イベントハンドラーが実行されます。これにより、`Editor` の内容がランダムに生成されたファイル名を持つ新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカルアプリケーションデータフォルダーに格納されます。 次に、メソッドは前のページに戻ります。 **Delete** `Button` が押されると `OnDeleteButtonClicked` イベントハンドラーが実行され、ファイルが存在する場合はそのファイルが削除され、前のページに戻ります。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **NoteEntryPage.xaml.cs**への変更内容を保存してから、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

10. **Solution Pad**で、**メモ**プロジェクトを選択し、右クリックして、 **[> 新しいファイルの追加]** を選択します。 **[新しいファイル]** ダイアログで、 **[フォーム > フォーム ContentPage XAML**] を選択し、新しいファイルに「**ノート**」という名前を指定して、 **[新規]** 作成 ボタンをクリックします。

      これにより、プロジェクトのルートフォルダーに [**ノート] ページ**という名前のページが追加されます。 このページは、アプリケーションのルートページになります。

11. [ファイル **] [.xaml**] で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)と[`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)で構成されるページのユーザーインターフェイスを宣言によって定義します。 @No__t_0 は、データバインディングを使用して、アプリケーションによって取得されるすべてのメモを表示します。メモを選択すると、ノートを変更できる `NoteEntryPage` に移動します。 または、`ToolbarItem` を押して新しいメモを作成することもできます。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

    [ファイル **]** **> [保存**] を選択して (または **&#8984; + S**キーを押して)、ファイルを閉じて、変更内容を保存します。

12. **NotesPage.xaml.cs**で、すべてのテンプレートコードを削除し、次のコードに置き換えます。

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

    このコードでは、`NotesPage` の機能を定義します。 このページが表示されると、`OnAppearing` メソッドが実行され、ローカルアプリケーションデータフォルダーから取得されたすべてのメモが[`ListView`](xref:Xamarin.Forms.ListView)に設定されます。 [@No__t_1](xref:Xamarin.Forms.ToolbarItem)が押されると `OnNoteAddedClicked` イベントハンドラーが実行されます。 このメソッドは、`NoteEntryPage` に移動し、`NoteEntryPage` の[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)を新しい `Note` インスタンスに設定します。 @No__t_0 内の項目が選択されると、`OnListViewItemSelected` イベントハンドラーが実行されます。 このメソッドは、`NoteEntryPage` に移動し、選択した `Note` インスタンスに `NoteEntryPage` の[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)を設定します。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **[ファイル > 保存]** を選択し (または **&#8984; + S**キーを押して)、 **NotesPage.xaml.cs**への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

13. **Solution Pad**で、 **[App.xaml.cs]** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードは `System.IO` 名前空間の名前空間宣言を追加し、`string` 型の静的な `FolderPath` プロパティの宣言を追加します。 @No__t_0 プロパティは、ノートデータが格納されるデバイス上のパスを格納するために使用されます。 さらに、このコードは `App` コンストラクターの `FolderPath` プロパティを初期化し、 [`MainPage`](xref:Xamarin.Forms.Application.MainPage)プロパティを初期化して、`NotesPage` のインスタンスをホストする[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)にします。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **Solution Pad**の**Notes**プロジェクトで、 **mainpage.xaml**を右クリックし、 **[削除]** を選択します。 表示されたダイアログで、 **[削除]** ボタンを押して、ハードディスクからファイルを削除します。

    これにより、使用されなくなったページが削除されます。

15. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、[ **+** ] ボタンをクリックして**NoteEntryPage**に移動し、メモを入力します。 メモを保存すると、アプリケーションは [ノート **] ページ**に戻ります。

    さまざまな長さのメモを入力して、アプリケーションの動作を観察します。

::: zone-end

## <a name="next-steps"></a>次のステップ

このクイックスタートでは、次の方法について学習しました。

- Xamarin. Forms ソリューションにページを追加します。
- ページ間のナビゲーションを実行します。
- ユーザーインターフェイス要素とそのデータソースとの間でデータを同期するには、データバインディングを使用します。

ローカルの SQLite.NET データベースにデータを格納するようにアプリケーションを変更するには、次のクイックスタートに進みます。

> [!div class="nextstepaction"]
> [次へ](database.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Xamarin. フォームのクイックスタートの詳細](deepdive.md)
