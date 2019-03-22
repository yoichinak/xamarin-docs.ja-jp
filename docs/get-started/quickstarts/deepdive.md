---
title: Xamarin.Forms のクイック スタートの詳細情報
description: この記事では、Xamarin.Forms を使用したアプリケーション開発の基礎について説明します。 たとえば、Xamarin.Forms アプリケーションの構造、アプリケーションのアーキテクチャ、と基礎、ユーザー インターフェイスについて説明しました。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: 8674ef47867acf3bca4d05fd6628a58e2f9ad90e
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329365"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin.Forms のクイック スタートの詳細情報

[Xamarin.Forms のクイック スタート](~/get-started/index.yml)、Notes アプリケーションの構築します。 この記事では、Xamarin.Forms アプリケーションのしくみの基礎を理解するために、構築された内容を確認します。

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio の概要

Visual Studio は、コードを*ソリューション*と*プロジェクト*に分けて整理しています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Notes アプリケーションは、次のスクリーン ショットに示すように 4 つのプロジェクトを含む 1 つのソリューションで構成されます。

![](deepdive-images/vs/solution.png "Visual Studio ソリューション エクスプローラー")

プロジェクトの内容:

- メモ: このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリ プロジェクトです。
- Notes.Android – このプロジェクトは、Android 固有のコードを保持して Android アプリケーションのエントリ ポイントです。
- Notes.iOS – このプロジェクトは、iOS 固有のコードを保持して iOS アプリケーションのエントリ ポイントです。
- Notes.UWP – このプロジェクトはユニバーサル Windows プラットフォーム (UWP) 固有のコードを保持および UWP アプリケーションのエントリ ポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーン ショットでは、Visual Studio で、ノート .NET Standard ライブラリ プロジェクトの内容を示しています。

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard プロジェクトの内容")

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet** &ndash; Xamarin.Forms と sqlite-net の pcl の NuGet パッケージをプロジェクトに追加されました。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージです。

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Visual Studio for Mac の概要

[Visual Studio for Mac ](/visualstudio/mac/)は、コードを*ソリューション*と*プロジェクト*に分けて整理するという Visual Studio の方法に従っています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Notes アプリケーションは、次のスクリーン ショットに示すように 3 つのプロジェクトを含む 1 つのソリューションで構成されます。

![](deepdive-images/vsmac/solution.png "Visual Studio for Mac ソリューション ウィンドウ")

プロジェクトの内容:

- メモ: このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリ プロジェクトです。
- Notes.Android – このプロジェクトは、Android 固有のコードを保持して Android アプリケーションのエントリ ポイントです。
- Notes.iOS – このプロジェクトは、iOS の特定のコードを保持して iOS アプリケーションのエントリ ポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーン ショットは、Mac に Visual Studio でノート .NET Standard ライブラリ プロジェクトの内容を示しています。

![](deepdive-images/vsmac/net-standard-project.png "Phoneword .NET Standard ライブラリ プロジェクトの内容")

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet** &ndash; Xamarin.Forms と sqlite-net の pcl の NuGet パッケージをプロジェクトに追加されました。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージです。

::: zone-end

このプロジェクトには、以下の複数のファイルも含まれています。

- **Data\NoteDatabase.cs** – このクラスには、データベースを作成、データを読み取ったり、データを書き込み、およびからデータを削除するコードが含まれています。
- **Models\Note.cs** – このクラスを定義、`Note`モデルがあり、そのインスタンスが、アプリケーションで各メモに関するデータを格納します。
- **App.xaml**: `App` クラスの XAML マークアップ。アプリケーションのリソース ディクショナリを定義します。
- **App.xaml.cs**: `App` クラスの分離コード。各プラットフォーム上のアプリケーションで表示される最初のページのインスタンス化と、アプリケーションのライフサイクル イベント処理を担当します。
- **NotesPage.xaml** –、XAML マークアップを`NotesPage`クラスは、アプリケーションが起動されているページの UI を定義します。
- **NotesPage.xaml.cs** – の分離コード、`NotesPage`クラスは、ユーザーがページと対話するときに実行されるビジネス ロジックが含まれています。
- **NoteEntryPage.xaml** –、XAML マークアップを`NoteEntryPage`クラスは、ユーザーがメモに入ったときに表示するページの UI を定義します。
- **NoteEntryPage.xaml.cs** – の分離コード、`NoteEntryPage`クラスは、ユーザーがページと対話するときに実行されるビジネス ロジックが含まれています。

Xamarin.iOS アプリケーションの構造については、「[Anatomy of a Xamarin.iOS Application](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application)」(Xamarin.iOS アプリケーションの構造) を参照してください。 Xamarin.Android アプリケーションの構造については、「[Anatomy of a Xamarin.Android Application](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy)」(Xamarin.Android アプリケーションの構造) を参照してください。

## <a name="architecture-and-application-fundamentals"></a>アーキテクチャとアプリケーションの基礎

Xamarin.Forms アプリケーションは、従来のクロスプラットフォーム アプリケーションと同じ方法で設計されています。 通常、共有コードは .NET Standard ライブラリに配置され、プラットフォーム固有のアプリケーションは共有コードを使用します。 次の図は、Notes アプリケーションのこの関係の概要を示しています。

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "ノートのアーキテクチャ")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "ノートのアーキテクチャ")

::: zone-end

スタートアップ コードを最大限に再利用するために、Xamarin.Forms アプリケーションは `App` という 1 つのクラスがあります。このクラスは、各プラットフォーム上のアプリケーションが表示する最初のページのインスタンス化を担当しています。次にコード例を示します。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new NavigationPage(new NotesPage());
        }
        ...
    }
}
```

このコードを設定、`MainPage`のプロパティ、`App`クラスを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)インスタンスの内容を持つ、`NotesPage`インスタンス。 また、XAML コンパイラで [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 属性が有効なので、XAML は中間言語に直接コンパイルされます。 詳細については、「[XAML Compilation](~/xamarin-forms/xaml/xamlc.md)」(XAML のコンパイル) を参照してください。

## <a name="launching-the-application-on-each-platform"></a>各プラットフォームでアプリケーションを起動します。

### <a name="ios"></a>iOS

IOS で最初の Xamarin.Forms ページを起動する Notes.iOS プロジェクトの定義、`AppDelegate`から継承するクラス、`FormsApplicationDelegate`クラス。

```csharp
namespace Notes.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();
            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }
    }
}
```

`FinishedLaunching` オーバーライドは、`Init` メソッドを呼び出して Xamarin.Forms のフレームワークを初期化します。 その結果、Xamarin.Forms の iOS 固有の実装がアプリケーションに読み込まれ、次に `LoadApplication` メソッドの呼び出しによってルート ビュー コントローラーが設定されます。

### <a name="android"></a>Android

Notes.Android プロジェクトには Android で最初の Xamarin.Forms ページを起動するには、作成するコードが含まれています、`Activity`で、`MainLauncher`属性から継承したアクティビティと、`FormsAppCompatActivity`クラス。

```csharp
namespace Notes.Droid
{
    [Activity(Label = "Notes",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` オーバーライドは、`Init` メソッドを呼び出して Xamarin.Forms のフレームワークを初期化します。 その結果、Xamarin.Forms の Android 固有の実装がアプリケーションに読み込まれ、次に Xamarin.Forms アプリケーションが読み込まれます。

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) アプリケーションでは、Xamarin.Forms フレームワークを初期化する `Init` メソッドが `App` から呼び出されます。

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

その結果、Xamarin.Forms の UWP 固有の実装がアプリケーションに読み込まれます。 最初の Xamarin.Forms ページはによって起動される、`MainPage`クラス。

```csharp
namespace Notes.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Notes.App());
        }
    }
}
```

Xamarin.Forms アプリケーションは `LoadApplication` メソッドを使用して読み込まれます。

> [!NOTE]
> ユニバーサル Windows プラットフォーム アプリには、Xamarin.Forms を使用して構築されたが、Windows で Visual Studio の使用のみを指定できます。

::: zone-end

## <a name="user-interface"></a>ユーザーインターフェイス

これには、Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するために使用する 4 つのメインのコントロール グループがあります。

1. **ページ**: Xamarin.Forms のページは、クロスプラットフォーム モバイル アプリケーション画面を表しています。 Notes アプリケーションを使用して、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)クラスを 1 つの画面を表示します。 ページの詳細については、「[Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md)」(Xamarin.Forms のページ) を参照してください。
1. **ビュー**: Xamarin.Forms のビューは、ユーザー インターフェイスに表示されるコントロールです。たとえば、ラベル、ボタン、テキスト入力ボックスなどです。 完成したノート アプリケーションを使用して、 [ `ListView` ](xref:Xamarin.Forms.ListView)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Button` ](xref:Xamarin.Forms.Button)ビュー。 ビューの詳細については、「[Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md)」(Xamarin.Forms のビュー) を参照してください。
1. **レイアウト**: Xamarin.Forms のレイアウトは、ビューを論理構造にまとめるために使用されるコンテナーです。 Notes アプリケーションを使用して、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)を縦に並べて、ビューを配置するためのクラスと[ `Grid` ](xref:Xamarin.Forms.Grid)ボタンを水平方向に配置するためのクラス。 レイアウトの詳細については、「[Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md)」(Xamarin.Forms のレイアウト) を参照してください。
1. **セル**: Xamarin.Forms セルは、一覧内の項目に使用される特殊な要素です。一覧内の各項目を描画する方法を示しています。 Notes アプリケーションを使用して、 [ `TextCell` ](xref:Xamarin.Forms.TextCell)行ごとに 2 つの項目を一覧に表示します。 セルの詳細については、「[Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md)」(Xamarin.Forms のセル) を参照してください。

実行時に、各コントロールはネイティブの同等のものにマップされます。そしてそれがレンダリングされます。

### <a name="layout"></a>レイアウト

Notes アプリケーションを使用して、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)を自動的に画面サイズに関係なく、画面のビューを配置してクロスプラット フォーム対応のアプリケーションの開発を簡略化します。 各子要素は、追加した順に、水平または垂直方向に 1 つずつ配置されます。 `StackLayout` が使用する領域の量は、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティの設定によって異なりますが、`StackLayout` は既定で全画面を使用しようとします。

次の XAML コードを使用する例を示しています、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)レイアウトに、 `NoteEntryPage`:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    ...    
    <StackLayout Margin="{StaticResource PageMargin}">
        <Editor Placeholder="Enter your note"
                Text="{Binding Text}"
                HeightRequest="100" />
        <Grid>
            ...
        </Grid>
    </StackLayout>    
</ContentPage>
```

既定では、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)垂直方向を前提としています。 ただし、変更できますを水平方向に設定して、 [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation)プロパティを[ `StackOrientation.Horizontal` ](xref:Xamarin.Forms.StackOrientation.Horizontal)列挙型のメンバー。

> [!NOTE]
> ビューのサイズを使用して設定できます、`HeightRequest`と`WidthRequest`プロパティ。

[`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスの詳細については、「[StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)」を参照してください。

### <a name="responding-to-user-interaction"></a>ユーザー操作に対する応答

XAML に定義されているオブジェクトによって、分離コード ファイルで処理されるイベントが発生する可能性があります。 次のコード例は、`OnSaveButtonClicked`の分離コードでメソッド、`NoteEntryPage`への応答が実行される、クラス、 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)イベントの発生、*保存*ボタン.

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

`OnSaveButtonClicked`メソッドは、データベースにメモを保存し、前のページに戻ります。

> [!NOTE]
> XAML クラスの分離コード ファイルは、`x:Name` 属性を指定して割り当てられた名前を使用して、XAML に定義されているオブジェクトにアクセスできます。 この属性に割り当てられている値は、C# 変数と同じルールを持っています。つまり、英字またはアンダースコアから始まり、埋め込みスペースが含まれる必要があります。

保存の配線がボタンを`OnSaveButtonClicked`の XAML マークアップでメソッドが発生した、`NoteEntryPage`クラス。

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>表示内容

[ `ListView` ](xref:Xamarin.Forms.ListView)はリストの項目のコレクションを垂直方向に表示する責任を負います。 内の各項目、 `ListView` 1 つのセルに含まれます。

次のコード例は、 [ `ListView` ](xref:Xamarin.Forms.ListView)から、 `NotesPage`:

```xaml
<ListView x:Name="listView"
          Margin="{StaticResource PageMargin}"
          ItemSelected="OnListViewItemSelected">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Text}"
                      Detail="{Binding Date}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

内の各行のレイアウト、 [ `ListView` ](xref:Xamarin.Forms.ListView)内で定義されて、 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)要素、およびアプリケーションによって取得されるメモを表示するバインドのデータを使用します。 [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティでのデータ ソースに設定されて`NotesPage.xaml.cs`:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

このコードを生成、 [ `ListView` ](xref:Xamarin.Forms.ListView)データベースに格納されているすべてのノートにします。

内の行を選択すると、 [ `ListView` ](xref:Xamarin.Forms.ListView)、 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)イベントが発生します。 名前付きイベント ハンドラーを`OnListViewItemSelected`イベントが発生したときに実行されます。

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

[ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)してセルに関連付けられているオブジェクトにアクセスできるイベント、 [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem)プロパティ。

詳細については、 [ `ListView` ](xref:Xamarin.Forms.ListView)クラスを参照してください[ListView](~/xamarin-forms/user-interface/listview/index.md)します。

## <a name="navigation"></a>ナビゲーション

Xamarin.Forms は、使用している [`Page`](xref:Xamarin.Forms.Page) 型に応じて多数のページ ナビゲーション エクスペリエンスを提供します。 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)インスタンスのナビゲーションは、階層、またはモーダルを指定できます。 モーダル ナビゲーションについては、次を参照してください。 [Xamarin.Forms のモーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)します。

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) クラス、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) クラスおよび [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) クラスは別のナビゲーション エクスペリエンスを提供します。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。

階層ナビゲーションで、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)の履歴をナビゲートするクラスが使用される[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) forwards と backwards、必要に応じて、オブジェクトします。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。 1 つのページから別のページに移動するには、アプリケーションは新しいページを、そこでアクティブなページとなるナビゲーション スタックにプッシュします。 前のページに戻るには、アプリケーションは現在のページをナビゲーション スタックからポップします。そして新しい最上位のページがアクティブ ページになります。

`NavigationPage` クラスはまた、ページの最上部にナビゲーション バーを追加します。このバーには、タイトルと、前にページに戻るための **[戻る]** ボタンが表示されます。このボタンはプラットフォーム固有です。

ナビゲーション スタックに追加された最初のページとして参照されます、*ルート*Notes アプリケーションでこれを実現する方法、アプリケーションと、次のコード例のページに表示されます。

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

すべての [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスに、ページ スタックを変更するメソッドを公開する [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティがあります。 このメソッドは、アプリケーションに [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) が含まれる場合にのみ呼び出します。 `NoteEntryPage` に移動するには、下のコード例のように、[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) メソッドを呼び出す必要があります。

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

これにより、新しい`NoteEntryPage`アクティブなページがナビゲーション スタックにプッシュするオブジェクト。

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。 元のページにプログラムを使用して戻るには、`NoteEntryPage` オブジェクトが次のコード例のように [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドを呼び出す必要があります。

```csharp
await Navigation.PopAsync();
```

階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

## <a name="data-binding"></a>データ バインディング

Xamarin.Forms アプリケーションがそのデータを表示し、相互作用するしくみを簡単にするためにデータ バインディングが使用されます。 データ バインディングはユーザー インターフェイスと基礎アプリケーションの間で接続を確立します。 [`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスには、データ バインディングをサポートするためのインフラストラクチャの大部分が含まれています。

データ バインディングでは、*ソース*と*ターゲット*と呼ばれる 2 つのオブジェクトを接続します。 *ソース* オブジェクトはデータを提供します。 *ターゲット* オブジェクトは、ソース オブジェクトのデータを使用し (またしばしば表示し) ます。 など、 [ `Editor` ](xref:Xamarin.Forms.Editor) (*ターゲット*オブジェクト) バインドは通常その[ `Text` ](xref:Xamarin.Forms.Editor.Text)プロパティをパブリック`string`プロパティ*ソース*オブジェクト。 次の図では、バインドの関係を示します。

![](deepdive-images/data-binding.png "データ バインディング")

データ バインディングの主な利点は、ビューとデータ ソース間でデータを同期する心配がないことです。 *ソース* オブジェクトの変更は、バインディング フレームワークによって背後で自動的に*ターゲット* オブジェクトにプッシュされます。そして、ターゲット オブジェクトの変更は、オプションで*ソース* オブジェクトに戻されます。

データを確立するバインディングは、2 段階のプロセスです。

- *ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは、*ソース*に設定する必要があります。
- バインディングは*ターゲット*と*ソース*間で確立する必要があります。 XAML でこれは、[`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) マークアップ拡張を使用して実現できます。

バインディング ターゲットは、ノートのアプリケーションで、 [ `Editor` ](xref:Xamarin.Forms.Editor) 、ノートを表示するときに、`Note`インスタンスに設定、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`バインディングは、ソース。

`BindingContext`の`NoteEntryPage`の次のコード例に示すように、ページ ナビゲーション、中に設定されます。

```csharp
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
```

`OnNoteAddedClicked`メソッドは、アプリケーションに新しいメモが追加されたときに実行される、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`新しいに設定されている`Note`インスタンス。 `OnListViewItemSelected`メソッドで、既存のノートがで選択したときに実行される、 [ `ListView` ](xref:Xamarin.Forms.ListView)、`BindingContext`の`NoteEntryPage`が、選択したセット`Note`インスタンスを通じてアクセスされる、[ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem)プロパティ。

> [!IMPORTANT]
> 各*ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは個々に設定できますが、これは必ずしも行う必要はありません。 `BindingContext` は、その子がすべて継承する特殊なプロパティです。 そのため、`BindingContext`で、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)に設定されている、`Note`インスタンス、すべての子の`ContentPage`が同じである`BindingContext`、し、のパブリックプロパティにバインドできます`Note`オブジェクト。

[ `Editor` ](xref:Xamarin.Forms.Editor)で`NoteEntryPage`にバインドし、`Text`のプロパティ、`Note`オブジェクト。

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

*ソース* オブジェクトの [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) プロパティと `Text` プロパティ間のバインディングが確立されました。 行われた変更、`Editor`に自動的に伝達されます、`Note`オブジェクト。 同様に、変更された場合、`Note.Text`プロパティ、Xamarin.Forms のバインド エンジンによっての内容更新も、`Editor`します。 これは、*両方向のバインド*とも呼ばれています。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="styling"></a>[スタイル]

Xamarin.Forms アプリケーションには、複数のビジュアル要素同一の外観を持つには多くの場合が含まれます。 各ビジュアル要素の外観の設定は繰り返し発生することがあります、エラーが発生します。 代わりに、スタイル作成できる外観を定義し、必要なビジュアル要素に適用されます。

[ `Style` ](xref:Xamarin.Forms.Style)クラスにグループ化し、複数のビジュアル要素のインスタンスに適用できる 1 つのオブジェクトにプロパティ値のコレクション。 スタイルが格納されている、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)のいずれかで、アプリケーション レベル、ページ レベル、またはレベルを表示します。 定義する場所を選択する、`Style`に及ぼす影響を使用できます。

- [`Style`](xref:Xamarin.Forms.Style) アプリケーション レベルで定義されているインスタンスは、アプリケーション全体で適用できます。
- [`Style`](xref:Xamarin.Forms.Style) ページ レベルで定義されているインスタンスは、ページとその子に適用できます。
- [`Style`](xref:Xamarin.Forms.Style) ビュー レベルで定義されているインスタンスは、ビューとその子に適用できます。

> [!IMPORTANT]
> アプリケーション全体で使用されている任意のスタイルは、重複を避けるために、アプリケーションのリソース ディクショナリに格納されます。 ただし、ページで必要なときに、リソースの代わりに、アプリケーションの起動時に解析されますは、特定のページには、XAML をアプリケーションのリソース ディクショナリに含めることはできません。

各[ `Style` ](xref:Xamarin.Forms.Style)インスタンスには、1 つまたは複数のコレクションが含まれています[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクトは、各`Setter`を持つ、 [ `Property` ](xref:Xamarin.Forms.Setter.Property)および[`Value`](xref:Xamarin.Forms.Setter.Value)。 `Property`にスタイルを適用するのには、要素のバインド可能なプロパティの名前を指定し、`Value`プロパティに適用される値です。 次のコード例からスタイルを示しています`NoteEntryPage`:。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    <ContentPage.Resources>
        <!-- Implicit styles -->
        <Style TargetType="{x:Type Editor}">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>
        ...
    </ContentPage.Resources>
    ...
</ContentPage>
```

このスタイルを適用する[ `Editor` ](xref:Xamarin.Forms.Editor)ページ上のインスタンス。

作成するときに、 [ `Style` ](xref:Xamarin.Forms.Style)、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティは常に必要です。

> [!NOTE]
> Xamarin.Forms アプリケーションのスタイル設定は XAML スタイルを使用してこれまで行われます。 ただし、Xamarin.Forms では、カスケード スタイル シート (CSS) を使用して視覚的要素をスタイルもサポートしています。 詳細については、次を参照してください。[カスケード スタイル シート (CSS) を使用してスタイルを設定する Xamarin.Forms アプリ](~/xamarin-forms/user-interface/styles/css/index.md)します。

XAML のスタイルの詳細については、次を参照してください。[XAML スタイルを使った Xamarin.Forms アプリのスタイリング](~/xamarin-forms/user-interface/styles/xaml/index.md)。

### <a name="providing-platform-specific-styles"></a>プラットフォーム固有のスタイルを提供します。

`OnPlatform`マークアップ拡張機能では、プラットフォームごとに UI の外観をカスタマイズできます。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.App">
    <Application.Resources>
        ...
        <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
        <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
        <Color x:Key="iOSNavigationBarTextColor">Black</Color>
        <Color x:Key="AndroidNavigationBarTextColor">White</Color>

        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                       Android={StaticResource AndroidNavigationBarColor}}" />
             <Setter Property="BarTextColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                       Android={StaticResource AndroidNavigationBarTextColor}}" />           
        </Style>
        ...
    </Application.Resources>
</Application>
```

これは、 [ `Style` ](xref:Xamarin.Forms.Style)設定別[ `Color` ](xref:Xamarin.Forms.Color)の値を[ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)と[ `BarTextColor` ](xref:Xamarin.Forms.NavigationPage.BarTextColor)プロパティの[ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)使用されているプラットフォームに応じて、します。

XAML マークアップ拡張機能の詳細については、「[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。 については、`OnPlatform`マークアップ拡張機能を参照してください[OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)します。

## <a name="testing-and-deployment"></a>テストと展開

Visual Studio for Mac と Visual Studio のいずれも、アプリケーションをテストおよび展開するためのオプションを多数用意しています。 アプリケーションのデバッグは、アプリケーション開発ライフサイクルの一般的な部分であり、コードの問題を診断するときに役立ちます。 詳細については、「[Set a Breakpoint](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)」(ブレークポイントの設定)、「[Step Through Code](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)」(コードのステップ スルー)、「[Output Information to the Log Window](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)」(ログ ウィンドウへの出力情報) を参照してください。

シミュレーターは、アプリケーションの展開とテストを始めるにはおすすめの場所です。また、テスト アプリケーションに役立つ機能があります。 ただし、ユーザーは完成したアプリケーションをシミュレーター上では使用しないので、早期に、そして何度も実際のデバイス上でアプリケーションをテストすることをお勧めします。 iOS デバイスのプロビジョニングの詳細については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)」(デバイスのプロビジョニング) を参照してください。 Android デバイスのプロビジョニングの詳細については、「[Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)」(開発用のデバイスの設定) を参照してください。

## <a name="next-steps"></a>次の手順

この詳細情報には、Xamarin.Forms を使用したアプリケーション開発の基礎が調べします。 推奨される次の手順としては、次の機能の説明を読んでください。

- Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するために、主に 4 つのコントロール グループが使用されます。 詳細については、「[Controls Reference](~/xamarin-forms/user-interface/controls/index.md)」 (コントロールのリファレンス) を参照してください。
- データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティでの変更が自動的にもう片方のプロパティに反映されるようにする手法です。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。
- Xamarin.Forms では、使用されるページの種類に応じて、さまざまなページ ナビゲーションのエクスペリエンスが提供されます。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。
- スタイルは、繰り返されるマークアップを減らすのに役立ち、アプリケーションの外観をより簡単に変更できるようにします。 詳しくは、「[Styling Xamarin.Forms Apps](~/xamarin-forms/user-interface/styles/index.md)」 (Xamarin.Forms アプリのスタイル設定) をご覧ください。
- XAML マークアップ拡張では、要素属性をリテラル テキスト文字列ではなく、ソースから設定できるようにすることで、XAML をより強力かつ柔軟なものにします。 詳細については、「[XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/index.md)」 (XAML マークアップ拡張) を参照してください。
- データ テンプレートでは、サポートされているビューでのデータの表現方法を定義する機能が提供されます。 詳細については、「[Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」 (データ テンプレート) を参照してください。
- 各ページ、レイアウト、およびビューは `Renderer` クラスを使用して、プラットフォームごとに異なる方法でレンダリングされます。その後、ネイティブ コントロールが作成され、画面に配置され、共有コードで指定された動作が追加されます。 開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。
- 効果では、各プラットフォームのネイティブ コントロールのカスタマイズを可能にします。 効果は、プラットフォーム固有のプロジェクトで [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) クラスをサブクラス化することによって作成され、適切な Xamarin.Forms コントロールに添付することによって使用されます。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (効果) を参照してください。
- 共有コードはネイティブ機能に [`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスを介してアクセスできます。 詳細については、「[Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)」 (DependencyService を使用したネイティブ機能へのアクセス) を参照してください。

または、Charles Petzold 著の『[_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)』 (Xamarin.Forms でモバイル アプリを作成する) でも Xamarin.Forms の詳細を学習できます。 この書籍は、PDF またはさまざまな形式の電子ブックとして入手可能です。

## <a name="related-links"></a>関連リンク

- [eXtensible Application Markup Language (XAML)](~/xamarin-forms/xaml/index.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [コントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [入門サンプル](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/)
- [Xamarin.Forms API リファレンス](xref:Xamarin.Forms)
- [無料のセルフ ガイド学習 (ビデオ)](https://university.xamarin.com/self-guided/)
