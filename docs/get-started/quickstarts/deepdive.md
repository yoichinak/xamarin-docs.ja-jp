---
title: Xamarin.Forms クイック スタート Deep Dive
description: この記事では、Xamarin.Forms を使用したアプリケーション開発の基礎について説明します。 取り上げたテーマには、Xamarin.Forms アプリケーションの構造、アーキテクチャとアプリケーションの基礎、ユーザー インターフェイスが含まれていました。
zone_pivot_groups: ''
ms.topic: ''
ms.prod: ''
ms.custom: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1bfb76f71a2ac9d8bc9ae84152501909000b9623
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84132523"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin.Forms クイック スタート Deep Dive

[Xamarin.Forms クイック スタート](~/get-started/index.yml)では、Notes アプリケーションをビルドしました。 この記事では、Xamarin.Forms アプリケーションのしくみの基礎を理解するために、構築された内容を確認します。

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio の概要

Visual Studio は、コードを*ソリューション*と*プロジェクト*に分けて整理しています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Notes アプリケーションは、次のスクリーンショットに示されているように、4 つのプロジェクトを含む 1 つのソリューションで構成されています。

![](deepdive-images/vs/solution.png "Visual Studio Solution Explorer")

プロジェクトの内容:

- Notes - このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリ プロジェクトです。
- Notes.Android: このプロジェクトは、Android 固有のコードを保持しており、Android アプリケーションのエントリ ポイントです。
- Notes.iOS: このプロジェクトは、iOS 固有のコードを保持しており、iOS アプリケーションのエントリ ポイントです。
- Notes.UWP: このプロジェクトは、ユニバーサル Windows プラットフォーム (UWP) 固有のコードを保持保持しており、UWP アプリケーションのエントリ ポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーンショットは、Visual Studio の Notes .NET Standard プロジェクトの内容です。

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard Project Contents")

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet** &ndash; プロジェクトに追加されている Xamarin.Forms と sqlite-net-pcl NuGet パッケージ。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージ。

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Visual Studio for Mac の概要

[Visual Studio for Mac ](/visualstudio/mac/)は、コードを*ソリューション*と*プロジェクト*に分けて整理するという Visual Studio の方法に従っています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Notes アプリケーションは、次のスクリーンショットに示されているように、3 つのプロジェクトを含む 1 つのソリューションで構成されています。

![](deepdive-images/vsmac/solution.png "Visual Studio for Mac Solution Pane")

プロジェクトの内容:

- Notes - このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリ プロジェクトです。
- Notes.Android: このプロジェクトは、Android 固有のコードを保持した、Android アプリケーションのエントリ ポイントです。
- Notes.iOS: このプロジェクトは、iOS 固有のコードを保持しており、iOS アプリケーションのエントリ ポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーンショットは、Visual Studio for Mac の Notes .NET Standard プロジェクトの内容を示しています。

![](deepdive-images/vsmac/net-standard-project.png "Phoneword .NET Standard Library Project Contents")

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet** &ndash; プロジェクトに追加されている Xamarin.Forms と sqlite-net-pcl NuGet パッケージ。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージ。

::: zone-end

このプロジェクトには、以下の複数のファイルも含まれています。

- **Data\NoteDatabase.cs** – このクラスには、データベースを作成し、それに対してデータの読み込み、データの書き込み、データの削除を行うためのコードが含まれます。
- **Models\Note.cs** – このクラスにより、`Note` モデルが定義されます。このインスタンスにアプリケーション内の各メモに関するデータが格納されます。
- **App.xaml**: `App` クラスの XAML マークアップ。アプリケーションのリソース ディクショナリを定義します。
- **App.xaml.cs**: `App` クラスの分離コード。各プラットフォーム上のアプリケーションで表示される最初のページのインスタンス化と、アプリケーションのライフサイクル イベント処理を担当します。
- **AssemblyInfo.cs** – このファイルには、アセンブリ レベルで適用されるプロジェクトに関するアプリケーション属性が含まれています。
- **NotesPage.xaml**: `NotesPage` クラスの XAML マークアップ。アプリケーションの起動時に表示されるページの UI を定義します。
- **NotesPage.xaml.cs**: `NotesPage` クラスの分離コード。ユーザーがページを操作したときに実行されるビジネス ロジックが含まれています。
- **NoteEntryPage.xaml**: `NoteEntryPage` クラスの XAML マークアップ。ユーザーがメモを入力するときに表示されるページの UI を定義します。
- **NoteEntryPage.xaml.cs**: `NoteEntryPage` クラスの分離コード。ユーザーがページを操作するときに実行されるビジネス ロジックが含まれています。

Xamarin.iOS アプリケーションの構造については、「[Anatomy of a Xamarin.iOS Application](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application)」(Xamarin.iOS アプリケーションの構造) を参照してください。 Xamarin.Android アプリケーションの構造については、「[Anatomy of a Xamarin.Android Application](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy)」(Xamarin.Android アプリケーションの構造) を参照してください。

## <a name="architecture-and-application-fundamentals"></a>アーキテクチャとアプリケーションの基礎

Xamarin.Forms アプリケーションは、従来のクロスプラットフォーム アプリケーションと同じ方法で設計されています。 通常、共有コードは .NET Standard ライブラリに配置され、プラットフォーム固有のアプリケーションは共有コードを使用します。 次の図は、Notes アプリケーションのこの関係の概要を示しています。

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "Notes Architecture")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "Notes Architecture")

::: zone-end

スタートアップ コードを最大限に再利用するために、Xamarin.Forms アプリケーションには `App` という名前の 1 つのクラスがあります。このクラスは、各プラットフォーム上のアプリケーションが表示する最初のページのインスタンス化を担当しています。次にコード例を示します。

```csharp
using Xamarin.Forms;

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

このコードは、`App` クラスの `MainPage` プロパティを、コンテンツが `NotesPage` インスタンスの [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) インスタンスに設定します。

さらに、**AssemblyInfo.cs** ファイルには、アセンブリ レベルで適用される単一のアプリケーション属性が含まれています。

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

[`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 属性により XAML コンパイラが有効になるため、XAML は中間言語に直接コンパイルされます。 詳細については、「[XAML Compilation](~/xamarin-forms/xaml/xamlc.md)」(XAML のコンパイル) を参照してください。

## <a name="launching-the-application-on-each-platform"></a>各プラットフォームでのアプリケーションの起動

### <a name="ios"></a>iOS

iOS で最初の Xamarin.Forms ページを起動するため、Notes.iOS プロジェクトでは `FormsApplicationDelegate` クラスを継承する `AppDelegate` クラスが定義されます。

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

`FinishedLaunching` オーバーライドは、`Init` メソッドを呼び出すことで Xamarin.Forms フレームワークを初期化します。 その結果、Xamarin.Forms の iOS 固有の実装がアプリケーションに読み込まれ、次に `LoadApplication` メソッドの呼び出しによってルート ビュー コントローラーが設定されます。

### <a name="android"></a>Android

Android で最初の Xamarin.Forms ページを起動するために、Notes.Android プロジェクトには、`MainLauncher` 属性が指定され、`FormsAppCompatActivity` クラスから継承したアクティビティがある `Activity` を作成するコードが含まれています。

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

`OnCreate` オーバーライドは、`Init` メソッドを呼び出すことで Xamarin.Forms フレームワークを初期化します。 それにより、Xamarin.Forms の Android 固有の実装がアプリケーションに読み込まれ、次に Xamarin.Forms アプリケーションが読み込まれます。

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) アプリケーションでは、`Init` フレームワークを初期化する Xamarin.Forms メソッドが `App` クラスから呼び出されます。

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

それにより、Xamarin.Forms の UWP 固有の実装がアプリケーションに読み込まれます。 最初の Xamarin.Forms ページは `MainPage` クラスによって起動されます。

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
> ユニバーサル Windows プラットフォーム アプリは Xamarin.Forms を使用してビルドできますが、Windows で Visual Studio を使用する必要があります。

::: zone-end

## <a name="user-interface"></a>ユーザー インターフェイス

Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するために、主に 4 つのコントロール グループが使用されます。

1. **ページ**: Xamarin.Forms のページは、クロスプラットフォーム モバイル アプリケーション画面を表しています。 Notes アプリケーションでは、1 つの画面を表示するために [`ContentPage`](xref:Xamarin.Forms.ContentPage) クラスが使用されます。 ページの詳細については、「[Xamarin.Forms のページ](~/xamarin-forms/user-interface/controls/pages.md)」を参照してください。
1. **ビュー**: Xamarin.Forms のビューは、ユーザー インターフェイスに表示されるコントロールで、ラベル、ボタン、テキスト入力ボックスなどです。 完成した Notes アプリケーションでは、[`ListView`](xref:Xamarin.Forms.ListView)、[`Editor`](xref:Xamarin.Forms.Editor)、[`Button`](xref:Xamarin.Forms.Button) のビューが使用されます。 ビューの詳細については、「[Xamarin.Forms のビュー](~/xamarin-forms/user-interface/controls/views.md)」を参照してください。
1. **レイアウト**: Xamarin.Forms のレイアウトは、ビューを論理構造にまとめるために使用されるコンテナーです。 Notes アプリケーションでは、ビューを垂直方向に並べて配置するために [`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスが使用され、ボタンを水平方向に配置するために [`Grid`](xref:Xamarin.Forms.Grid) クラスが使用されます。 レイアウトの詳細については、「[Xamarin.Forms のレイアウト](~/xamarin-forms/user-interface/controls/layouts.md)」を参照してください。
1. **セル**: Xamarin.Forms のセルは、一覧内の項目に使用される特殊な要素です。一覧内の各項目を描画する方法を示しています。 Notes アプリケーションでは、リスト内の行ごとに 2 つの項目を表示するために [`TextCell`](xref:Xamarin.Forms.TextCell) が使用されます。 セルの詳細については、「[Xamarin.Forms のセル](~/xamarin-forms/user-interface/controls/cells.md)」を参照してください。

実行時に、各コントロールはネイティブの同等のものにマップされます。そしてそれがレンダリングされます。

### <a name="layout"></a>レイアウト

Notes アプリケーションでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) を使用して、画面サイズにかかわらず、画面上のビューを自動的に配置することで、クロスプラットフォーム アプリケーションの開発が簡素化されます。 各子要素は、追加した順に、水平または垂直方向に 1 つずつ配置されます。 `StackLayout` が使用する領域の量は、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティの設定によって異なりますが、`StackLayout` は既定で全画面を使用しようとします。

次の XAML コードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) を使用して `NoteEntryPage` をレイアウトする例を示しています。

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

既定では、[`StackLayout`](xref:Xamarin.Forms.StackLayout) は垂直方向を前提としています。 ただし、[`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) プロパティを [`StackOrientation.Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) 列挙メンバーに設定することによって、水平方向に変更することができます。

> [!NOTE]
> ビューのサイズは、`HeightRequest` と `WidthRequest` のプロパティを使用して設定できます。

[`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスの詳細については、「[StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md)」を参照してください。

### <a name="responding-to-user-interaction"></a>ユーザー操作に対する応答

XAML に定義されているオブジェクトによって、分離コード ファイルで処理されるイベントが発生する可能性があります。 次のコード例は、 *[保存]* ボタンによって発生する [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントに応答して実行される、`NoteEntryPage` クラスの分離コードの `OnSaveButtonClicked` メソッドを示しています。

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

`OnSaveButtonClicked` メソッドは、データベースにメモを保存し、前のページに戻ります。

> [!NOTE]
> XAML クラスの分離コード ファイルは、`x:Name` 属性を指定して割り当てられた名前を使用して、XAML に定義されているオブジェクトにアクセスできます。 この属性に割り当てられている値は、C# 変数と同じルールを持っています。つまり、英字またはアンダースコアから始まり、埋め込みスペースが含まれる必要があります。

保存ボタンから `OnSaveButtonClicked` メソッドに接続する処理は、`NoteEntryPage` クラスの XAML マークアップで発生します。

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>表示内容

[`ListView`](xref:Xamarin.Forms.ListView) は、リストに項目のコレクションを垂直方向に表示します。 `ListView` の各項目が 1 つのセルに含まれます。

次のコード例は、`NotesPage` からの [`ListView`](xref:Xamarin.Forms.ListView) を示しています。

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

[`ListView`](xref:Xamarin.Forms.ListView) の各行のレイアウトは [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 要素内で定義され、データ バインディングを使用して、アプリケーションによって取得されたすべてのメモが表示されます。 [`ListView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティは、`NotesPage.xaml.cs` でデータ ソースに設定されます。

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

このコードにより、データベースに格納されているすべてのメモが [`ListView`](xref:Xamarin.Forms.ListView) に取り込まれます。

[`ListView`](xref:Xamarin.Forms.ListView) で行を選択すると、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントが発生します。 イベントが発生すると、`OnListViewItemSelected` という名前のイベント ハンドラーが実行されます。

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

[`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) プロパティを使用することで、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントがセルに関連付けられているオブジェクトにアクセスできます。

[`ListView`](xref:Xamarin.Forms.ListView) クラスの詳細については、「[ListView](~/xamarin-forms/user-interface/listview/index.md)」を参照してください。

## <a name="navigation"></a>ナビゲーション

Xamarin.Forms では、使用されている [`Page`](xref:Xamarin.Forms.Page) 型に応じて、多数の異なるページ ナビゲーション エクスペリエンスが提供されます。 [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスでは、ナビケーションを階層的、またはモーダルにすることができます。 モーダル ナビゲーションの詳細については、「[Xamarin.Forms のモーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)」を参照してください。

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) クラス、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) クラスおよび [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) クラスは別のナビゲーション エクスペリエンスを提供します。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。

階層ナビゲーションでは、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトのスタック間を (必要に応じて前後に) ナビゲートするために使用されます。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。 1 つのページから別のページに移動するには、アプリケーションは新しいページを、そこでアクティブなページとなるナビゲーション スタックにプッシュします。 前のページに戻るには、アプリケーションは現在のページをナビゲーション スタックからポップします。そして新しい最上位のページがアクティブ ページになります。

`NavigationPage` クラスはまた、ページの最上部にナビゲーション バーを追加します。このバーには、タイトルと、前にページに戻るための **[戻る]** ボタンが表示されます。このボタンはプラットフォーム固有です。

ナビゲーション スタックに追加された最初のページが、アプリケーションの "*ルート*" ページとなります。次のコード例は、Notes アプリケーションでこれを実現する方法を示しています。

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

すべての [`ContentPage`](xref:Xamarin.Forms.ContentPage) インスタンスに、ページ スタックを変更するメソッドを公開する [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティがあります。 このメソッドは、アプリケーションに [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) が含まれる場合にのみ呼び出します。 `NoteEntryPage` に移動するには、下のコード例で示すように、[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) メソッドを呼び出す必要があります。

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

これにより新しい `NoteEntryPage` オブジェクトがナビゲーション スタックにプッシュされ、アクティブ ページとなります。

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。 元のページにプログラムを使用して戻るには、`NoteEntryPage` オブジェクトが次のコード例のように [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドを呼び出す必要があります。

```csharp
await Navigation.PopAsync();
```

階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

## <a name="data-binding"></a>データ バインディング

Xamarin.Forms アプリケーションがデータを表示し、相互作用するしくみを簡素化するために、データ バインディングが使用されます。 データ バインディングはユーザー インターフェイスと基礎アプリケーションの間で接続を確立します。 [`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスには、データ バインディングをサポートするためのインフラストラクチャの大部分が含まれています。

データ バインディングでは、*ソース*と*ターゲット*と呼ばれる 2 つのオブジェクトを接続します。 *ソース* オブジェクトはデータを提供します。 *ターゲット* オブジェクトは、ソース オブジェクトのデータを使用し (またしばしば表示し) ます。 たとえば、[`Editor`](xref:Xamarin.Forms.Editor) ("*ターゲット*" オブジェクト) は一般的にその [`Text`](xref:Xamarin.Forms.InputView.Text) プロパティを "*ソース*" オブジェクトのパブリック プロパティ `string` にバインドします。 次の図では、バインドの関係を示します。

![](deepdive-images/data-binding.png "Data Binding")

データ バインディングの主な利点は、ビューとデータ ソース間でデータを同期する心配がないことです。 *ソース* オブジェクトの変更は、バインディング フレームワークによって背後で自動的に*ターゲット* オブジェクトにプッシュされます。そして、ターゲット オブジェクトの変更は、オプションで*ソース* オブジェクトに戻されます。

データ バインディングを確立するには、次の 2 つの手順を実行します。

- *ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは、*ソース*に設定する必要があります。
- バインディングは*ターゲット*と*ソース*間で確立する必要があります。 XAML でこれは、[`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) マークアップ拡張を使用して実現できます。

Notes アプリケーションでは、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) として設定された `Note` インスタンスがバインディング ソースであるのに対し、バインディング ターゲットはメモを表示する [`Editor`](xref:Xamarin.Forms.Editor) です。

`NoteEntryPage` の `BindingContext` は、次のコード例に示すように、ページ ナビゲーション中に設定されます。

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

アプリケーションに新しいメモが追加されたときに実行される `OnNoteAddedClicked` メソッドでは、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) が新しい `Note` インスタンスに設定されます。 [`ListView`](xref:Xamarin.Forms.ListView) で既存のメモが選択されたときに実行される `OnListViewItemSelected` メソッドでは、`NoteEntryPage` の `BindingContext` が選択された `Note` インスタンスに設定されます。これには、[`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) プロパティを通じてアクセスします。

> [!IMPORTANT]
> 各*ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは個々に設定できますが、これは必ずしも行う必要はありません。 `BindingContext` は、その子がすべて継承する特殊なプロパティです。 したがって、[`ContentPage`](xref:Xamarin.Forms.ContentPage) の `BindingContext` が `Note` インスタンスに設定された場合、`ContentPage` のすべての子は同じ `BindingContext` を持ち、`Note` オブジェクトのパブリック プロパティにバインドすることができます。

その後、`NoteEntryPage` 内の [`Editor`](xref:Xamarin.Forms.Editor) が `Note` オブジェクトの `Text` プロパティにバインドされます。

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

*ソース* オブジェクトの [`Editor.Text`](xref:Xamarin.Forms.InputView.Text) プロパティと `Text` プロパティ間のバインディングが確立されました。 `Editor` での変更は `Note` オブジェクトに自動的に伝達されます。 同様に、`Note.Text` プロパティが変更された場合、Xamarin.Forms のバインド エンジンによって `Editor` のコンテンツも更新されます。 これは、*双方向のバインド*とも呼ばれています。

データ バインディングの詳細については、「[Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

## <a name="styling"></a>スタイル

Xamarin.Forms アプリケーションには、多くの場合、同じ外観を持つ複数のビジュアル要素が含まれています。 各ビジュアル要素の外観を設定すると、繰り返し使用しやすく、エラーが発生しやすくなります。 代わりに、外観を定義し、必要なビジュアル要素に適用するスタイルを作成できます。

[`Style`](xref:Xamarin.Forms.Style) クラスは、プロパティ値のコレクションを 1 つのオブジェクトにグループ化して、複数のビジュアル要素インスタンスに適用できるようにします。 スタイルは、アプリケーション レベル、ページ レベル、ビュー レベルのいずれかで [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に格納されます。 `Style` を定義する場所の選択は、それを使用できる場所に影響します。

- アプリケーション レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、アプリケーション全体に適用できます。
- ページ レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、ページとその子に適用できます。
- ビュー レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、ビューとその子に適用できます。

> [!IMPORTANT]
> アプリケーション全体で使用されるスタイルは、重複を回避するために、アプリケーションのリソース ディクショナリに保存されます。 ただし、あるページに固有の XAML は、アプリケーションのリソース ディクショナリに含めるべきではありません。アプリのリソースは、ページが必要とするときではなく、アプリケーションの起動時に解析されるためです。

各 [`Style`](xref:Xamarin.Forms.Style) インスタンスには、1 つ以上の [`Setter`](xref:Xamarin.Forms.Setter) オブジェクトのコレクションが含まれています。各 `Setter` には、[`Property`](xref:Xamarin.Forms.Setter.Property)と[`Value`](xref:Xamarin.Forms.Setter.Value)があります。 `Property` は、スタイルが適用される要素のバインド可能なプロパティの名前で、`Value` はプロパティに適用される値です。 次のコード例は、`NoteEntryPage` のスタイルを示しています。

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

このスタイルは、ページ上の任意の [`Editor`](xref:Xamarin.Forms.Editor) インスタンスに適用されます。

[`Style`](xref:Xamarin.Forms.Style) を作成する場合は、[`TargetType`](xref:Xamarin.Forms.Style.TargetType) プロパティが常に必要です。

> [!NOTE]
> Xamarin.Forms アプリケーションのスタイル設定は、これまで XAML スタイルを使用して行われていました。 しかし Xamarin.Forms では、カスケード スタイル シート (CSS) を使用したビジュアル要素のスタイル設定もサポートされています。 詳細については、「[カスケード スタイル シート (CSS) を使用した ](~/xamarin-forms/user-interface/styles/css/index.md) アプリのスタイル設定Xamarin.Forms」を参照してください。

XAML スタイルの詳細については、「[XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)」を参照してください。

### <a name="providing-platform-specific-styles"></a>プラットフォーム固有のスタイルの提供

`OnPlatform` マークアップ拡張では、プラットフォームごとに UI の外観をカスタマイズできます。

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

この [`Style`](xref:Xamarin.Forms.Style) により、[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) の [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) プロパティと [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor) プロパティに、使用されているプラットフォームに応じて異なる [`Color`](xref:Xamarin.Forms.Color) 値が設定されます。

XAML マークアップ拡張機能の詳細については、「[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。 `OnPlatform` マークアップ拡張機能の詳細については、「[OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」を参照してください。

## <a name="testing-and-deployment"></a>テストと展開

Visual Studio for Mac と Visual Studio のいずれも、アプリケーションをテストおよび展開するためのオプションを多数用意しています。 アプリケーションのデバッグは、アプリケーション開発ライフサイクルの一般的な部分であり、コードの問題を診断するときに役立ちます。 詳細については、「[Set a Breakpoint](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)」(ブレークポイントの設定)、「[Step Through Code](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)」(コードのステップ スルー)、「[Output Information to the Log Window](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)」(ログ ウィンドウへの出力情報) を参照してください。

シミュレーターは、アプリケーションの展開とテストを始めるにはおすすめの場所です。また、テスト アプリケーションに役立つ機能があります。 ただし、ユーザーは完成したアプリケーションをシミュレーター上では使用しないので、早期に、そして何度も実際のデバイス上でアプリケーションをテストすることをお勧めします。 iOS デバイスのプロビジョニングの詳細については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)」(デバイスのプロビジョニング) を参照してください。 Android デバイスのプロビジョニングの詳細については、「[Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)」(開発用のデバイスの設定) を参照してください。

## <a name="next-steps"></a>次の手順

この詳細では、Xamarin.Forms を使用したアプリケーション開発の基礎について説明しました。 推奨される次の手順としては、次の機能の説明を読んでください。

- Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するために、主に 4 つのコントロール グループが使用されます。 詳細については、「[Controls Reference](~/xamarin-forms/user-interface/controls/index.md)」 (コントロールのリファレンス) を参照してください。
- データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティでの変更が自動的にもう片方のプロパティに反映されるようにする手法です。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。
- Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。
- スタイルは、繰り返されるマークアップを減らすのに役立ち、アプリケーションの外観をより簡単に変更できるようにします。 詳細については、「[Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/index.md)」を参照してください。
- XAML マークアップ拡張では、要素属性をリテラル テキスト文字列ではなく、ソースから設定できるようにすることで、XAML をより強力かつ柔軟なものにします。 詳細については、「[XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/index.md)」 (XAML マークアップ拡張) を参照してください。
- データ テンプレートでは、サポートされているビューでのデータの表現方法を定義する機能が提供されます。 詳細については、「[Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」 (データ テンプレート) を参照してください。
- 各ページ、レイアウト、およびビューは `Renderer` クラスを使用して、プラットフォームごとに異なる方法でレンダリングされます。その後、ネイティブ コントロールが作成され、画面に配置され、共有コードで指定された動作が追加されます。 開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。
- エフェクトでは、各プラットフォームのネイティブ コントロールのカスタマイズを可能にします。 エフェクトは、プラットフォーム固有のプロジェクトで [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) クラスをサブクラス化することによって作成され、適切な Xamarin.Forms コントロールに添付することによって使用されます。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (エフェクト) を参照してください。
- 共有コードはネイティブ機能に [`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスを介してアクセスできます。 詳細については、「[Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)」 (DependencyService を使用したネイティブ機能へのアクセス) を参照してください。

または、「[_Xamarin.Forms でモバイル アプリを作成する_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)」 (Charles Petzold 著) でも Xamarin.Forms の詳細を学習できます。 この書籍は、PDF またはさまざまな形式の電子ブックとして入手可能です。

## <a name="related-links"></a>関連リンク

- [eXtensible Application Markup Language (XAML)](~/xamarin-forms/xaml/index.yml)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [コントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [入門サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms%20get%20started)
- [Xamarin.Forms API リファレンス](xref:Xamarin.Forms)
- [無料のセルフ ガイド学習 (ビデオ)](https://university.xamarin.com/self-guided/)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/Xamarin-Solution-Architecture-4-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
