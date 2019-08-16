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
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653793"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>複数ページの Xamarin.Forms アプリケーションでナビゲーションを実行する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

このクイックスタートでは、次の方法について説明します。

- Xamarin.Forms ソリューションにページを追加します。
- ページ間のナビゲーションを実行します。
- ユーザーインターフェイス要素とそのデータソースとの間でデータを同期するには、データバインディングを使用します。

このクイックスタートでは、1つのノートを複数ページアプリケーションに格納できる単一ページクロスプラットフォーム Xamarin.Forms アプリケーションを、複数のメモを格納できるようにする方法について説明します。 最終的なアプリケーションは、次のとおりです。

[メモ] ページ[ ![(multi-page-images/screenshots1-sml.png " ")]][(multi-page-images/screenshots1.png#lightbox "メモ] ページ")メモ入力ページの(multi-page-images/screenshots2.png#lightbox "メモ入力ページ") [ ![(multi-page-images/screenshots2-sml.png " ")]] 


### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試行する前に、[前のクイックスタート](single-page.md)を正常に完了している必要があります。 または、[前のクイックスタートサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)をダウンロードし、このクイックスタートの出発点として使用してください。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動します。 [スタート] ウィンドウで、[最近使ったプロジェクト/ソリューション] の一覧で [ **note** ] ソリューションをクリックするか、[**プロジェクトまたはソリューション**を開く] をクリックします。次に、[**プロジェクト/ソリューションを開く**] ダイアログで、メモプロジェクトのソリューションファイルを選択します。

    ![](multi-page-images/vs/open-solution.png "プロジェクトを開く")

2. **ソリューションエクスプローラー**で、**メモ**プロジェクトを右クリックし、[ **> 新しいフォルダーの追加**] を選択します。

    ![](multi-page-images/vs/add-new-item.png "新しい項目の追加")

3. **ソリューションエクスプローラー**で、新しいフォルダー**モデル**にという名前を指定します。

    ![](multi-page-images/vs/name-folder.png "モデルフォルダー")

4. **ソリューションエクスプローラー**で、[**モデル**] フォルダーを選択し、右クリックして、[**新しい項目の追加 >** ] を選択します。

    ![](multi-page-images/vs/add-new-models-file.png "新しいファイルの追加")

5. [**新しい項目の追加**] ダイアログで、[**ビジュアルC#項目 > クラス**] を選択し、新しいファイルに「 **Note**」という名前を指定して、[**追加**] ボタンをクリックします。

    ![](multi-page-images/vs/add-note-class.png "Note クラスの追加")

    これにより、 **note**という名前のクラスが**Notes**プロジェクトの [**モデル**] フォルダーに追加されます。

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

    このクラスは、 `Note`アプリケーションの各メモに関するデータを格納するモデルを定義します。    

    **CTRL + S**キーを押して**Note.cs**への変更内容を保存し、ファイルを閉じます。

7. **ソリューションエクスプローラー**で、**メモ**プロジェクトを右クリックし、[ **> 新しい項目の追加**] を選択します。[**新しい項目の追加**] ダイアログで、[**ビジュアルC#項目 > Xamarin.Forms > コンテンツ] ページ**を選択し、新しいファイルに**NoteEntryPage**という名前を指定して、[**追加**] ボタンをクリックします。

    ![](multi-page-images/vs/add-note-entry-page.png "Xamarin.Forms ContentPage の追加")

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

      このコードは、ページのユーザーインターフェイスを宣言[`Editor`](xref:Xamarin.Forms.Editor)によって定義します。これは、テキスト入力用のと、アプリケーションにファイルの保存または削除を指示する2つ[`Button`](xref:Xamarin.Forms.Button)のインスタンスで構成されます。 2つ`Button`のインスタンスは、で水平方向[`Grid`](xref:Xamarin.Forms.Grid)にレイアウト`Editor` `Grid`され、で水平方向[`StackLayout`](xref:Xamarin.Forms.StackLayout)にレイアウトされます。 さらに、は`Editor` 、データバインディングを使用して`Text` 、 `Note`モデルのプロパティにバインドします。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

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

      このコードは、 `Note`ページ[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の内に1つのノートを表すインスタンスを格納します。 `Editor` **保存** [`Button`](xref:Xamarin.Forms.Button)を押す`OnSaveButtonClicked`と、イベントハンドラーが実行されます。これにより、の内容がランダムに生成されたファイル名を持つ新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカルアプリケーションデータフォルダーに格納されます。 次に、メソッドは前のページに戻ります。 **削除** `Button`が押さ`OnDeleteButtonClicked`れると、イベントハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、前のページに戻ります。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

      **CTRL + S**キーを押して**NoteEntryPage.xaml.cs**への変更内容を保存し、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

10. **ソリューションエクスプローラー**で、**メモ**プロジェクトを右クリックし、[ **> 新しい項目の追加**] を選択します。[**新しい項目の追加**] ダイアログで、[**ビジュアルC#項目 > Xamarin.Forms > コンテンツ] ページ**を選択し、新しいファイルに「**ノート**」という名前を指定して、[**追加**] ボタンをクリックします。

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

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)とで構成されるページのユーザーインターフェイスを宣言によって定義します。 は`ListView` 、データバインディングを使用して、アプリケーションによって取得されるすべてのメモを表示します`NoteEntryPage` 。メモを選択すると、ノートを変更できる場所に移動します。 または、 `ToolbarItem`を押して新しいメモを作成することもできます。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

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

    このコードは、 `NotesPage`の機能を定義します。 このページが表示さ`OnAppearing`れると、メソッドが実行され、ローカルアプリケーションデータフォルダーから取得したすべての[`ListView`](xref:Xamarin.Forms.ListView)メモがに設定されます。 が押されると、 `OnNoteAddedClicked`イベントハンドラーが実行されます。 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) このメソッドは、に`NoteEntryPage`移動し、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`を新しい`Note`インスタンスに設定します。 の`ListView`項目が選択されると、 `OnListViewItemSelected`イベントハンドラーが実行されます。 このメソッドは、に`NoteEntryPage`移動し、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`を選択した`Note`インスタンスに設定します。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **CTRL + S**キーを押して**NotesPage.xaml.cs**への変更内容を保存し、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

13. **ソリューションエクスプローラー**で、[ **App.xaml.cs** ] をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードは、 `System.IO`名前空間の名前空間宣言を追加し、型`string`の静的`FolderPath`プロパティの宣言を追加します。 `FolderPath`プロパティは、ノートデータが格納されるデバイス上のパスを格納するために使用されます。 さらに、この`FolderPath`コードは`App` [`MainPage`](xref:Xamarin.Forms.Application.MainPage)コンストラクターでプロパティを初期化し、プロパティを初期化して、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)の`NotesPage`インスタンスをホストするとします。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **mainpage.xaml**を右クリックし、[**削除**] を選択します。 表示されるダイアログで、 **[OK** ] をクリックして、ハードディスクからファイルを削除します。

    これにより、使用されなくなったページが削除されます。

15. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、 **+** ボタンを押して**NoteEntryPage**に移動し、メモを入力します。 メモを保存すると、アプリケーションは [ノート **] ページ**に戻ります。

    さまざまな長さのメモを入力して、アプリケーションの動作を観察します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動します。 [スタート] ウィンドウで [**開く**] をクリックし、ダイアログボックスで、メモプロジェクトのソリューションファイルを選択します。

    ![](multi-page-images/vsmac/open-solution.png "ソリューションを開く")

2. **Solution Pad**で、**メモ**プロジェクトを選択して右クリックし、[ **> 新しいフォルダーの追加**] を選択します。

    ![](multi-page-images/vsmac/add-new-folder.png "新しいフォルダーの追加")

3. **Solution Pad**で、新しいフォルダー**モデル**にという名前を指定します。

    ![](multi-page-images/vsmac/name-folder.png "モデルフォルダー")

4. **Solution Pad**で、[**モデル**] フォルダーを選択し、右クリックして、[**新しいファイルを追加 >** ] を選択します。

    ![](multi-page-images/vsmac/add-new-models-file.png "新しいファイルの追加")

5. [**新しいファイル**] ダイアログで、 **[全般 > 空のクラス**] を選択し、新しいファイルに「 **Note**」という名前を指定して、[**新規**] ボタンをクリックします。

    ![](multi-page-images/vsmac/add-note-class.png "Note クラスの追加")

    これにより、 **note**という名前のクラスが**Notes**プロジェクトの [**モデル**] フォルダーに追加されます。

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

    このクラスは、 `Note`アプリケーションの各メモに関するデータを格納するモデルを定義します。

    [**ファイル > 保存**] を選択し (または **&#8984; + S**キーを押して)、 **Note.cs**への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad**で、**メモ**プロジェクトを選択し、右クリックして、[ **> 新しいファイルの追加**] を選択します。[**新しいファイル**] ダイアログで、 **[フォーム > フォーム ContentPage XAML**] を選択し、新しいファイルに**NoteEntryPage**という名前を指定して、[**新規**] ボタンをクリックします。

    ![](multi-page-images/vsmac/add-note-entry-page.png "Xamarin.Forms ContentPage の追加")

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

      このコードは、ページのユーザーインターフェイスを宣言[`Editor`](xref:Xamarin.Forms.Editor)によって定義します。これは、テキスト入力用のと、アプリケーションにファイルの保存または削除を指示する2つ[`Button`](xref:Xamarin.Forms.Button)のインスタンスで構成されます。 2つ`Button`のインスタンスは、で水平方向[`Grid`](xref:Xamarin.Forms.Grid)にレイアウト`Editor` `Grid`され、で水平方向[`StackLayout`](xref:Xamarin.Forms.StackLayout)にレイアウトされます。 さらに、は`Editor` 、データバインディングを使用して`Text` 、 `Note`モデルのプロパティにバインドします。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

      [**ファイル > 保存**] を選択し (または、  **&#8984; + S**キーを押して)、 **NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

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

      このコードは、 `Note`ページ[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の内に1つのノートを表すインスタンスを格納します。 `Editor` **保存** [`Button`](xref:Xamarin.Forms.Button)を押す`OnSaveButtonClicked`と、イベントハンドラーが実行されます。これにより、の内容がランダムに生成されたファイル名を持つ新しいファイルに保存されるか、またはメモが更新されている場合は既存のファイルに保存されます。 どちらの場合も、ファイルはアプリケーションのローカルアプリケーションデータフォルダーに格納されます。 次に、メソッドは前のページに戻ります。 **削除** `Button`が押さ`OnDeleteButtonClicked`れると、イベントハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、前のページに戻ります。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

      [**ファイル > 保存**] を選択し (または **&#8984; + S**キーを押して)、 **NoteEntryPage.xaml.cs**への変更内容を保存してから、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

10. **Solution Pad**で、**メモ**プロジェクトを選択し、右クリックして、[ **> 新しいファイルの追加**] を選択します。[**新しいファイル**] ダイアログで、 **[フォーム > フォーム ContentPage XAML**] を選択し、新しいファイルに「**ノート**」という名前を指定して、[**新規**作成] ボタンをクリックします。

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

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)とで構成されるページのユーザーインターフェイスを宣言によって定義します。 は`ListView` 、データバインディングを使用して、アプリケーションによって取得されるすべてのメモを表示します`NoteEntryPage` 。メモを選択すると、ノートを変更できる場所に移動します。 または、 `ToolbarItem`を押して新しいメモを作成することもできます。 データバインディングの詳細については、「 [Xamarin](deepdive.md)の概要」の「[データバインディング](deepdive.md#data-binding)」を参照してください。

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

    このコードは、 `NotesPage`の機能を定義します。 このページが表示さ`OnAppearing`れると、メソッドが実行され、ローカルアプリケーションデータフォルダーから取得したすべての[`ListView`](xref:Xamarin.Forms.ListView)メモがに設定されます。 が押されると、 `OnNoteAddedClicked`イベントハンドラーが実行されます。 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) このメソッドは、に`NoteEntryPage`移動し、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`を新しい`Note`インスタンスに設定します。 の`ListView`項目が選択されると、 `OnListViewItemSelected`イベントハンドラーが実行されます。 このメソッドは、に`NoteEntryPage`移動し、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`を選択した`Note`インスタンスに設定します。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    [**ファイル > 保存**] を選択し (または **&#8984; + S**キーを押して)、 **NotesPage.xaml.cs**への変更内容を保存してから、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、後続の手順で修正されるエラーが発生します。

13. **Solution Pad**で、[ **App.xaml.cs** ] をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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

    このコードは、 `System.IO`名前空間の名前空間宣言を追加し、型`string`の静的`FolderPath`プロパティの宣言を追加します。 `FolderPath`プロパティは、ノートデータが格納されるデバイス上のパスを格納するために使用されます。 さらに、この`FolderPath`コードは`App` [`MainPage`](xref:Xamarin.Forms.Application.MainPage)コンストラクターでプロパティを初期化し、プロパティを初期化して、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)の`NotesPage`インスタンスをホストするとします。 ナビゲーションの詳細については、「 [Xamarin のクイックスタート](deepdive.md)の[ナビゲーション](deepdive.md#navigation)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **Solution Pad**の**Notes**プロジェクトで、 **mainpage.xaml**を右クリックし、[**削除**] を選択します。 表示されたダイアログで、[**削除**] ボタンを押して、ハードディスクからファイルを削除します。

    これにより、使用されなくなったページが削除されます。

15. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、 **+** ボタンを押して**NoteEntryPage**に移動し、メモを入力します。 メモを保存すると、アプリケーションは [ノート **] ページ**に戻ります。

    さまざまな長さのメモを入力して、アプリケーションの動作を観察します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- Xamarin.Forms ソリューションにページを追加します。
- ページ間のナビゲーションを実行します。
- ユーザーインターフェイス要素とそのデータソースとの間でデータを同期するには、データバインディングを使用します。

ローカルの SQLite.NET データベースにデータを格納するようにアプリケーションを変更するには、次のクイックスタートに進みます。

> [!div class="nextstepaction"]
> [次へ](database.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Xamarin.Forms のクイックスタートの詳細](deepdive.md)
