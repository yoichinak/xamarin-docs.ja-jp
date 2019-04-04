---
title: 複数ページの Xamarin.Forms アプリケーションでナビゲーションを実行する
description: この記事では、複数のノートを格納できる、複数ページのアプリケーションに 1 つの注記を格納できるシングル ページ アプリケーションを有効にする方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 855962560897789dadba535f69c4a7da42bb4742
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854978"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>複数ページの Xamarin.Forms アプリケーションでのナビゲーションを実行します。

[![Download サンプル](~/media/shared/download.png) サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)

このクイック スタートでは、学習する方法。

- Xamarin.Forms ソリューションに追加のページを追加します。
- ページ間の移動を実行します。
- ユーザー インターフェイス要素とそのデータ ソースの間でデータを同期するのにには、データ バインディングを使用します。

このクイック スタート 1 つのページ クロスプラット フォーム-Xamarin.Forms アプリケーション、複数のノートを格納できる、複数ページのアプリケーションに 1 つの注記を格納できるを有効にする方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![](multi-page-images/screenshots1-sml.png "ページをノート")](multi-page-images/screenshots1.png#lightbox "ページをノート")
[![](multi-page-images/screenshots2-sml.png "エントリ ページに注意してください")](multi-page-images/screenshots2.png#lightbox "に注意してくださいエントリ ページ")

### <a name="prerequisites"></a>必須コンポーネント

正常に完了する必要があります、[前のクイック スタート](single-page.md)このクイック スタートを試行する前にします。 または、ダウンロード、[前のクイック スタート サンプル](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)し、このクイック スタートの開始点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動します。 [開始] ウィンドウ、**ノート**最近使ったプロジェクトおよびソリューションの一覧、またはクリックでソリューション**プロジェクトまたはソリューションを開く**、し、**プロジェクト/ソリューションを開く**ダイアログノートのプロジェクトのソリューション ファイルを選択します。

    ![](multi-page-images/vs/open-solution.png "プロジェクトを開く")

2. **ソリューション エクスプ ローラー**を右クリックし、**ノート**順に選択して**追加 > 新しいフォルダー**:

    ![](multi-page-images/vs/add-new-item.png "新しい項目の追加")

3. **ソリューション エクスプ ローラー**、新しいフォルダーの名前**モデル**:

    ![](multi-page-images/vs/name-folder.png "[Models] フォルダー")

4. **ソリューション エクスプ ローラー**を選択、**モデル**フォルダーを右クリックし、**追加 > 新しい項目.**:

    ![](multi-page-images/vs/add-new-models-file.png "新しいファイルの追加")

5. **新しい項目の追加**ダイアログ ボックスで、 **VisualC#項目 > クラス**、新しいファイルに名前**注**、 をクリックし、**追加**ボタン。

    ![](multi-page-images/vs/add-note-class.png "注クラスを追加します。")

    これがという名前のクラスに追加されます**注**を**モデル**のフォルダー、**ノート**プロジェクト。

6. **Note.cs**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

    このクラスを定義、`Note`アプリケーションで各メモに関するデータを格納するモデル。    

    変更を保存**Note.cs**キーを押して**CTRL + S**ファイルを閉じます。

7. **ソリューション エクスプ ローラー**を右クリックし、**ノート**順に選択して**追加 > [新しい項目.**.**新しい項目の追加**ダイアログ ボックスで、 **VisualC#項目 > Xamarin.Forms > コンテンツ ページ**、新しいファイルに名前**NoteEntryPage**、] をクリックし、 **追加**ボタンをクリックします。

    ![](multi-page-images/vs/add-note-entry-page.png "Xamarin.Forms ContentPage を追加します。")

    これはという名前の新しいページを追加**NoteEntryPage**プロジェクトのルート フォルダーにします。 このページは、アプリケーションでは、2 番目のページになります。

8. **NoteEntryPage.xaml**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

      このコードで構成されると、ページのユーザー インターフェイスを宣言によって定義されます、 [ `Editor` ](xref:Xamarin.Forms.Editor)テキストの入力と 2 つの[ `Button` ](xref:Xamarin.Forms.Button)の保存または削除するには、アプリケーション インスタンスファイルです。 2 つ`Button`インスタンスが水平方向にレイアウトを[ `Grid`](xref:Xamarin.Forms.Grid)で、`Editor`と`Grid`に垂直方向にレイアウトされる、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。 さらに、`Editor`にバインドするデータ バインドを使用して、`Text`のプロパティ、`Note`モデル。 データ バインディングの詳細については、[データ バインディング](deepdive.md#data-binding)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

      変更を保存**NoteEntryPage.xaml**キーを押して**CTRL + S**ファイルを閉じます。

9. **NoteEntryPage.xaml.cs**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

      このコードを格納、`Note`インスタンスで 1 つの注記を表す、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のページ。 ときに、**保存** [ `Button` ](xref:Xamarin.Forms.Button)が押された、`OnSaveButtonClicked`イベント ハンドラーが実行の内容を保存するか、 `Editor` 、ランダムに生成されたファイル名を持つ新しいファイルまたはメモを更新します。 存在する場合は、既存のファイルをします。 どちらの場合で、ファイルは、アプリケーションのローカル アプリケーション データ フォルダーに格納されます。 メソッドは、前のページに移動します。 ときに、**削除**`Button`が押された、`OnDeleteButtonClicked`イベント ハンドラーを実行すると、それが存在し、前のページにナビゲートされるファイルを削除します。 ナビゲーションの詳細については、[ナビゲーション](deepdive.md#navigation)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

      変更を保存**NoteEntryPage.xaml.cs**キーを押して**CTRL + S**ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

10. **ソリューション エクスプ ローラー**を右クリックし、**ノート**順に選択して**追加 > [新しい項目.**.**新しい項目の追加**ダイアログ ボックスで、 **VisualC#項目 > Xamarin.Forms > コンテンツ ページ**、新しいファイルに名前**NotesPage**、] をクリックし、**追加**ボタンをクリックします。

      これがという名前のページに追加されます**NotesPage**プロジェクトのルート フォルダーにします。 このページは、アプリケーションのルート ページになります。

11. **NotesPage.xaml**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

    このコードで構成されると、ページのユーザー インターフェイスを宣言によって定義されます、 [ `ListView` ](xref:Xamarin.Forms.ListView)と[ `ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)します。 `ListView`使用データ、アプリケーションによって取得されるメモを表示するバインディングと、メモしてを選択するに移動、`NoteEntryPage`メモを変更できます。 新しいメモを作成して、キーを押しても、`ToolbarItem`します。 データ バインディングの詳細については、[データ バインディング](deepdive.md#data-binding)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

    変更を保存**NotesPage.xaml**キーを押して**CTRL + S**ファイルを閉じます。

12. **NotesPage.xaml.cs**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

    このコードの機能を定義する、`NotesPage`します。 ページが表示されたら、`OnAppearing`メソッドの実行を設定します、 [ `ListView` ](xref:Xamarin.Forms.ListView)メモをローカル アプリケーション データ フォルダーから取得されたとします。 ときに、 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)が押された、`OnNoteAddedClicked`イベント ハンドラーが実行されます。 このメソッドに移動、`NoteEntryPage`で、設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`を新しい`Note`インスタンス。 内の項目のときに、`ListView`が選択されている、`OnListViewItemSelected`イベント ハンドラーが実行されます。 このメソッドに移動、`NoteEntryPage`で、設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`に、選択した`Note`インスタンス。 ナビゲーションの詳細については、[ナビゲーション](deepdive.md#navigation)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

    変更を保存**NotesPage.xaml.cs**キーを押して**CTRL + S**ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

13. **ソリューション エクスプ ローラー**、ダブルクリックして**App.xaml.cs**を開きます。 既存のコードを次のコードに置き換えます。

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
            ...
        }
    }
    ```

    このコードは、名前空間宣言を追加、`System.IO`名前空間の静的な宣言を追加します。`FolderPath`型のプロパティ`string`。 `FolderPath`注データが格納されるデバイス上のパスを格納するプロパティが使用されます。 さらに、コードを初期化します、`FolderPath`プロパティ、`App`コンス トラクター、および初期化、 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage)プロパティを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)をホストする、インスタンス`NotesPage`します。 ナビゲーションの詳細については、[ナビゲーション](deepdive.md#navigation)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **ソリューション エクスプ ローラー**の**ノート**プロジェクトを右クリックして**MainPage.xaml**を選択し、**削除**します。 キーを押して表示されるダイアログで、 **OK**ハード_ディスクからファイルを削除するボタン。

    これには、不要になったために使用されるページが削除されます。

15. 構築し、各プラットフォームで、プロジェクトを実行します。 詳細については、[クイック スタートを構築](single-page.md#building-the-quickstart)を参照してください。

    **NotesPage**キーを押して、 **+** に移動するボタン、 **NoteEntryPage**メモを入力します。 移動は、アプリケーションのメモの保存後、 **NotesPage**します。

    ノートについては、アプリケーションの動作を確認する、さまざまな長さの数を入力します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動します。 開始時間帯でクリックして**オープン**、ダイアログ ボックスで、ノートのプロジェクトのソリューション ファイルを選択します。

    ![](multi-page-images/vsmac/open-solution.png "ソリューションを開く")

2. **Solution Pad**を選択、**ノート**プロジェクトを右クリックし、**追加 > 新しいフォルダー**:

    ![](multi-page-images/vsmac/add-new-folder.png "新しいフォルダーを追加します。")

3. **Solution Pad**、新しいフォルダーの名前**モデル**:

    ![](multi-page-images/vsmac/name-folder.png "[Models] フォルダー")

4. **Solution Pad**を選択、**モデル**フォルダーを右クリックし、**追加 > 新しいファイル.**:

    ![](multi-page-images/vsmac/add-new-models-file.png "新しいファイルの追加")

5. **新しいファイル**ダイアログ ボックスで、**一般的な > 空のクラス**、新しいファイルに名前を**注**、 をクリックし、**新規**ボタン。

    ![](multi-page-images/vsmac/add-note-class.png "注クラスを追加します。")

    これがという名前のクラスに追加されます**注**を**モデル**のフォルダー、**ノート**プロジェクト。

6. **Note.cs**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

    このクラスを定義、`Note`アプリケーションで各メモに関するデータを格納するモデル。

    変更を保存**Note.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

7. **Solution Pad**を選択、**ノート**プロジェクトを右クリックし、**追加 > 新しいファイル.**.**新しいファイル**ダイアログ ボックスで、**フォーム > フォーム ContentPage XAML**、新しいファイルに名前**NoteEntryPage**、 をクリックし、**新規**ボタン。

    ![](multi-page-images/vsmac/add-note-entry-page.png "Xamarin.Forms ContentPage を追加します。")

    これはという名前の新しいページを追加**NoteEntryPage**プロジェクトのルート フォルダーにします。 このページは、アプリケーションでは、2 番目のページになります。

8. **NoteEntryPage.xaml**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

      このコードで構成されると、ページのユーザー インターフェイスを宣言によって定義されます、 [ `Editor` ](xref:Xamarin.Forms.Editor)テキストの入力と 2 つの[ `Button` ](xref:Xamarin.Forms.Button)の保存または削除するには、アプリケーション インスタンスファイルです。 2 つ`Button`インスタンスが水平方向にレイアウトを[ `Grid`](xref:Xamarin.Forms.Grid)で、`Editor`と`Grid`に垂直方向にレイアウトされる、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。 さらに、`Editor`にバインドするデータ バインドを使用して、`Text`のプロパティ、`Note`モデル。 データ バインディングの詳細については、[データ バインディング](deepdive.md#data-binding)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

      変更を保存**NoteEntryPage.xaml**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

9. **NoteEntryPage.xaml.cs**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

      このコードを格納、`Note`インスタンスで 1 つの注記を表す、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のページ。 ときに、**保存** [ `Button` ](xref:Xamarin.Forms.Button)が押された、`OnSaveButtonClicked`イベント ハンドラーが実行の内容を保存するか、 `Editor` 、ランダムに生成されたファイル名を持つ新しいファイルまたはメモを更新します。 存在する場合は、既存のファイルをします。 どちらの場合で、ファイルは、アプリケーションのローカル アプリケーション データ フォルダーに格納されます。 メソッドは、前のページに移動します。 ときに、**削除**`Button`が押された、`OnDeleteButtonClicked`イベント ハンドラーを実行すると、それが存在し、前のページにナビゲートされるファイルを削除します。 ナビゲーションの詳細については、[ナビゲーション](deepdive.md#navigation)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

      変更を保存**NoteEntryPage.xaml.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

      > [!WARNING]
      > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

10. **Solution Pad**を選択、**ノート**プロジェクトを右クリックし、**追加 > 新しいファイル.**.**新しいファイル**ダイアログ ボックスで、**フォーム > フォーム ContentPage XAML**、新しいファイルに名前**NotesPage**、 をクリックし、**新規**ボタンをクリックします。

      これがという名前のページに追加されます**NotesPage**プロジェクトのルート フォルダーにします。 このページは、アプリケーションのルート ページになります。

11. **NotesPage.xaml**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

    このコードで構成されると、ページのユーザー インターフェイスを宣言によって定義されます、 [ `ListView` ](xref:Xamarin.Forms.ListView)と[ `ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)します。 `ListView`使用データ、アプリケーションによって取得されるメモを表示するバインディングと、メモしてを選択するに移動、`NoteEntryPage`メモを変更できます。 新しいメモを作成して、キーを押しても、`ToolbarItem`します。 データ バインディングの詳細については、[データ バインディング](deepdive.md#data-binding)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

    変更を保存**NotesPage.xaml**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

12. **NotesPage.xaml.cs**すべてのテンプレート コードを削除し、次のコードに置き換えます。

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

    このコードの機能を定義する、`NotesPage`します。 ページが表示されたら、`OnAppearing`メソッドの実行を設定します、 [ `ListView` ](xref:Xamarin.Forms.ListView)メモをローカル アプリケーション データ フォルダーから取得されたとします。 ときに、 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)が押された、`OnNoteAddedClicked`イベント ハンドラーが実行されます。 このメソッドに移動、`NoteEntryPage`で、設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`を新しい`Note`インスタンス。 内の項目のときに、`ListView`が選択されている、`OnListViewItemSelected`イベント ハンドラーが実行されます。 このメソッドに移動、`NoteEntryPage`で、設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`に、選択した`Note`インスタンス。 ナビゲーションの詳細については、[ナビゲーション](deepdive.md#navigation)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

    変更を保存**NotesPage.xaml.cs**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

    > [!WARNING]
    > この時点でアプリケーションをビルドしようとすると、以降の手順で修正されるエラーが発生します。

13. **Solution Pad**、ダブルクリックして**App.xaml.cs**を開きます。 既存のコードを次のコードに置き換えます。

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
            ...
        }
    }
    ```

    このコードは、名前空間宣言を追加、`System.IO`名前空間の静的な宣言を追加します。`FolderPath`型のプロパティ`string`。 `FolderPath`注データが格納されるデバイス上のパスを格納するプロパティが使用されます。 さらに、コードを初期化します、`FolderPath`プロパティ、`App`コンス トラクター、および初期化、 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage)プロパティを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)をホストする、インスタンス`NotesPage`します。 ナビゲーションの詳細については、[ナビゲーション](deepdive.md#navigation)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

14. **Solution Pad**の**ノート**プロジェクトを右クリックして**MainPage.xaml**を選択し、**削除**します。 キーを押して表示されるダイアログで、**削除**ハード_ディスクからファイルを削除するボタンをクリックします。

    これには、不要になったために使用されるページが削除されます。

15. 構築し、各プラットフォームで、プロジェクトを実行します。 詳細については、[クイック スタートを構築](single-page.md#building-the-quickstart)を参照してください。

    **NotesPage**キーを押して、 **+** に移動するボタン、 **NoteEntryPage**メモを入力します。 移動は、アプリケーションのメモの保存後、 **NotesPage**します。

    ノートについては、アプリケーションの動作を確認する、さまざまな長さの数を入力します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイック スタートでは説明した方法。

- Xamarin.Forms ソリューションに追加のページを追加します。
- ページ間の移動を実行します。
- ユーザー インターフェイス要素とそのデータ ソースの間でデータを同期するのにには、データ バインディングを使用します。

SQLite.NET のローカル データベースに、データを格納できるように、アプリケーションを変更するには、次のクイック スタートに進みます。

> [!div class="nextstepaction"]
> [次へ](database.md)

## <a name="related-links"></a>関連リンク

- [メモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)
- [Xamarin.Forms のクイック スタートの詳細情報](deepdive.md)
