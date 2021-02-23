---
title: Xamarin.Forms クイック スタート Deep Dive
description: この記事では、Xamarin.Forms シェルを使用したアプリケーション開発の基礎について説明します。 Xamarin.Forms シェル アプリケーションの構造、アーキテクチャとアプリケーションの基礎、ユーザー インターフェイスなどのトピックが含まれます。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.custom: video
ms.assetid: F65C83B7-BC9F-425F-8662-931B652A2946
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/25/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 723ea0f3c6703824bfbfca51f4ecfc8c5ab0b83a
ms.sourcegitcommit: 0a6b19004932c1ac82e16c95d5d3d5eb35a5b17f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2021
ms.locfileid: "100255299"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin.Forms クイック スタート Deep Dive

[Xamarin.Forms クイック スタート](~/get-started/index.yml)では、Notes アプリケーションをビルドしました。 この記事では、Xamarin.Forms シェル アプリケーションのしくみの基礎を理解するために構築された内容を確認します。

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio の概要

Visual Studio は、コードを *ソリューション* と *プロジェクト* に分けて整理しています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Notes アプリケーションは、次のスクリーンショットに示されているように、3 つのプロジェクトを含む 1 つのソリューションで構成されています。

![Visual Studio ソリューション エクスプローラー](deepdive-images/vs/solution.png)

プロジェクトの内容:

- Notes - このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリ プロジェクトです。
- Notes.Android: このプロジェクトは、Android 固有のコードを保持しており、Android アプリケーションのエントリ ポイントです。
- Notes.iOS: このプロジェクトは、iOS 固有のコードを保持しており、iOS アプリケーションのエントリ ポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーンショットは、Visual Studio の Notes .NET Standard プロジェクトの内容です。

![Phoneword .NET Standard プロジェクトの内容](deepdive-images/vs/net-standard-project.png)

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet** &ndash; プロジェクトに追加されている Xamarin.Forms、Xamarin.Essentials、Newtonsoft.Json、sqlite-net-pcl の各 NuGet パッケージ。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージ。

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Visual Studio for Mac の概要

[Visual Studio for Mac](/visualstudio/mac/)は、コードを *ソリューション* と *プロジェクト* に分けて整理するという Visual Studio の方法に従っています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 Notes アプリケーションは、次のスクリーンショットに示されているように、3 つのプロジェクトを含む 1 つのソリューションで構成されています。

![Visual Studio for Mac ソリューション ウィンドウ](deepdive-images/vsmac/solution.png)

プロジェクトの内容:

- Notes - このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリ プロジェクトです。
- Notes.Android: このプロジェクトは、Android 固有のコードを保持した、Android アプリケーションのエントリ ポイントです。
- Notes.iOS: このプロジェクトは、iOS 固有のコードを保持しており、iOS アプリケーションのエントリ ポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーンショットは、Visual Studio for Mac の Notes .NET Standard プロジェクトの内容を示しています。

![Phoneword .NET Standard ライブラリ プロジェクトの内容](deepdive-images/vsmac/net-standard-project.png)

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet** &ndash; プロジェクトに追加されている Xamarin.Forms、Xamarin.Essentials、Newtonsoft.Json、sqlite-net-pcl の各 NuGet パッケージ。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージ。

::: zone-end

このプロジェクトには、複数のファイルも含まれています。

- **Data\NoteDatabase.cs** – このクラスには、データベースを作成し、それに対してデータの読み込み、データの書き込み、データの削除を行うためのコードが含まれます。
- **Models\Note.cs** – このクラスにより、`Note` モデルが定義されます。このインスタンスにアプリケーション内の各メモに関するデータが格納されます。
- **Views\AboutPage.xaml** – `AboutPage` クラスの XAML マークアップ。About ページの UI が定義されています。
- **Views\AboutPage.xaml.cs** – `AboutPage` クラスのコードビハインド。ユーザーがページを操作するときに実行されるビジネス ロジックが含まれています。
- **Views\NotesPage.xaml** – `NotesPage` クラスの XAML マークアップ。アプリケーションの起動時に表示されるページの UI が定義されています。
- **Views\NotesPage.xaml.cs** – `NotesPage` クラスのコードビハインド。ユーザーがページを操作したときに実行されるビジネス ロジックが含まれています。
- **Views\NoteEntryPage.xaml** – `NoteEntryPage` クラスの XAML マークアップ。ユーザーがメモを入力するときに表示されるページの UI が定義されています。
- **Views\NoteEntryPage.xaml.cs** – `NoteEntryPage` クラスのコードビハインド。ユーザーがページを操作するときに実行されるビジネス ロジックが含まれています。
- **App.xaml**: `App` クラスの XAML マークアップ。アプリケーションのリソース ディクショナリを定義します。
- **App.xaml.cs** – `App` クラスのコードビハインド。シェル アプリケーションのインスタンス化と、アプリケーションのライフサイクル イベントの処理が行われます。
- **AppShell.xaml** – `AppShell` クラスの XAML マークアップ。アプリケーションのビジュアル階層が定義されています。
- **AppShell.xaml.cs** – `AppShell` クラスのコードビハインド。プログラムによって移動できるように、`NoteEntryPage` のルートが作成されます。
- **AssemblyInfo.cs** – このファイルには、アセンブリ レベルで適用されるプロジェクトに関するアプリケーション属性が含まれています。

Xamarin.iOS アプリケーションの構造については、「[Anatomy of a Xamarin.iOS Application](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application)」(Xamarin.iOS アプリケーションの構造) を参照してください。 Xamarin.Android アプリケーションの構造については、「[Anatomy of a Xamarin.Android Application](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy)」(Xamarin.Android アプリケーションの構造) を参照してください。

## <a name="architecture-and-application-fundamentals"></a>アーキテクチャとアプリケーションの基礎

Xamarin.Forms アプリケーションは、従来のクロスプラットフォーム アプリケーションと同じ方法で設計されています。 通常、共有コードは .NET Standard ライブラリに配置され、プラットフォーム固有のアプリケーションは共有コードを使用します。 次の図は、Notes アプリケーションのこの関係の概要を示しています。

![Notes アーキテクチャ](deepdive-images/architecture.png)

スタートアップ コードを最大限に再利用するため、Xamarin.Forms アプリケーションには `App` という名前の 1 つのクラスがあり、各プラットフォームでのアプリケーションのインスタンス化が行われます。次にコード例を示します。

```csharp
using Xamarin.Forms;

namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        // ...
    }
}
```

このコードにより、`App` クラスの `MainPage` プロパティが `AppShell` オブジェクトに設定されます。 `AppShell` クラスにより、アプリケーションのビジュアル階層が定義されます。 シェルによってこのビジュアル階層が取得されて、そのユーザー インターフェイスが生成されます。 アプリケーションのビジュアル階層を定義する方法の詳細については、「[アプリケーションのビジュアル階層](#application-visual-hierarchy)」を参照してください。

さらに、**AssemblyInfo.cs** ファイルには、アセンブリ レベルで適用される単一のアプリケーション属性が含まれています。

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

[`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 属性により XAML コンパイラが有効になるため、XAML は中間言語に直接コンパイルされます。 詳細については、「[XAML Compilation](~/xamarin-forms/xaml/xamlc.md)」(XAML のコンパイル) を参照してください。

## <a name="launch-the-application-on-each-platform"></a>各プラットフォームでアプリケーションを起動する

各プラットフォームでのアプリケーションの起動方法は、プラットフォームによって異なります。

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

## <a name="application-visual-hierarchy"></a>アプリケーションのビジュアル階層

Xamarin.Forms シェル アプリケーションでは、`Shell` クラスをサブクラス化するクラスで、アプリケーションのビジュアル階層が定義されています。 Notes アプリケーションの場合、これは `Appshell` クラスです。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Notes.Views"
       x:Class="Notes.AppShell">
    <TabBar>
        <ShellContent Title="Notes"
                      Icon="icon_feed.png"
                      ContentTemplate="{DataTemplate views:NotesPage}" />
        <ShellContent Title="About"
                      Icon="icon_about.png"
                      ContentTemplate="{DataTemplate views:AboutPage}" />
    </TabBar>
</Shell>
```

この XAML は、次の 2 つの主要なオブジェクトで構成されます。

- `TabBar`. `TabBar` は下部のタブ バーを表し、アプリケーションのナビゲーション パターンで下部のタブを使用する場合は、これを使用する必要があります。 `TabBar` オブジェクトは、`Shell` オブジェクトの子です。
- `ShellContent` は、`TabBar` の各タブの `ContentPage` オブジェクトを表します。 各 `ShellContent` オブジェクトは、`TabBar` オブジェクトの子です。

これらのオブジェクトは、ユーザー インターフェイスではなく、アプリケーションのビジュアル階層の編成を表しています。 シェルでは、これらのオブジェクトを取得して、コンテンツのナビゲーション ユーザー インターフェイスを生成します。 したがって、`AppShell` クラスにより、下部のタブから移動できる 2 つのページが定義されています。 ページは、ユーザーのナビゲーションに応じて、オンデマンドで作成されます。

シェル アプリケーションの詳細については、「[Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

## <a name="user-interface"></a>ユーザー インターフェイス

Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するには、複数のコントロール グループが使用されます。

1. **ページ**: Xamarin.Forms のページは、クロスプラットフォーム モバイル アプリケーション画面を表しています。 Notes アプリケーションでは、1 つの画面を表示するために [`ContentPage`](xref:Xamarin.Forms.ContentPage) クラスが使用されます。 ページの詳細については、「[Xamarin.Forms のページ](~/xamarin-forms/user-interface/controls/pages.md)」を参照してください。
1. **ビュー**: Xamarin.Forms のビューは、ユーザー インターフェイスに表示されるコントロールで、ラベル、ボタン、テキスト入力ボックスなどです。 完成した Notes アプリケーションでは、[`CollectionView`](xref:Xamarin.Forms.CollectionView)、[`Editor`](xref:Xamarin.Forms.Editor)、[`Button`](xref:Xamarin.Forms.Button) のビューが使用されます。 ビューの詳細については、「[Xamarin.Forms のビュー](~/xamarin-forms/user-interface/controls/views.md)」を参照してください。
1. **レイアウト**: Xamarin.Forms のレイアウトは、ビューを論理構造にまとめるために使用されるコンテナーです。 Notes アプリケーションでは、ビューを垂直方向に並べて配置するために [`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスが使用され、ボタンを水平方向に配置するために [`Grid`](xref:Xamarin.Forms.Grid) クラスが使用されます。 レイアウトの詳細については、「[Xamarin.Forms のレイアウト](~/xamarin-forms/user-interface/controls/layouts.md)」を参照してください。

実行時に、各コントロールはネイティブの同等のものにマップされます。そしてそれがレンダリングされます。

### <a name="layout"></a>レイアウト

Notes アプリケーションでは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) を使用して、画面サイズにかかわらず、画面上のビューを自動的に配置することで、クロスプラットフォーム アプリケーションの開発が簡素化されます。 各子要素は、追加した順に、水平または垂直方向に 1 つずつ配置されます。 `StackLayout` が使用する領域の量は、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティの設定によって異なりますが、`StackLayout` は既定で全画面を使用しようとします。

次の XAML コードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) を使用して `NoteEntryPage` をレイアウトする例を示しています。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.Views.NoteEntryPage"
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

[`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスの詳細については、「[Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md)」を参照してください。

### <a name="responding-to-user-interaction"></a>ユーザー操作に対する応答

XAML に定義されているオブジェクトによって、分離コード ファイルで処理されるイベントが発生する可能性があります。 次のコード例は、 *[保存]* ボタンによって発生する [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントに応答して実行される、`NoteEntryPage` クラスの分離コードの `OnSaveButtonClicked` メソッドを示しています。

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    if (!string.IsNullOrWhiteSpace(note.Text))
    {
        await App.Database.SaveNoteAsync(note);
    }
    await Shell.Current.GoToAsync("..");
}
```

`OnSaveButtonClicked` メソッドは、データベースにメモを保存し、前のページに戻ります。 ナビゲーションの詳細については、「[ナビゲーション](#navigation)」を参照してください。

> [!NOTE]
> XAML クラスの分離コード ファイルは、`x:Name` 属性を指定して割り当てられた名前を使用して、XAML に定義されているオブジェクトにアクセスできます。 この属性に割り当てられている値は、C# 変数と同じルールを持っています。つまり、英字またはアンダースコアから始まり、埋め込みスペースが含まれる必要があります。

保存ボタンから `OnSaveButtonClicked` メソッドに接続する処理は、`NoteEntryPage` クラスの XAML マークアップで発生します。

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>表示内容

[`CollectionView`](xref:Xamarin.Forms.CollectionView) により、項目のコレクションがリストに表示されます。 既定では、リスト項目は垂直方向に表示され、各項目が 1 つの行に表示されます。

次のコード例は、`NotesPage` からの [`CollectionView`](xref:Xamarin.Forms.CollectionView) を示しています。

```xaml
<CollectionView x:Name="collectionView"
                Margin="{StaticResource PageMargin}"
                SelectionMode="Single"
                SelectionChanged="OnSelectionChanged">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="10" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Label Text="{Binding Text}"
                       FontSize="Medium" />
                <Label Text="{Binding Date}"
                       TextColor="{StaticResource TertiaryColor}"
                       FontSize="Small" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

[`CollectionView`](xref:Xamarin.Forms.CollectionView) の各行のレイアウトは [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 要素内で定義され、データ バインディングを使用して、アプリケーションによって取得されたすべてのメモが表示されます。 [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティは、**NotesPage.xaml.cs** でデータ ソースに設定されています。

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    collectionView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

このコードにより、データベースに格納されているすべてのメモが [`CollectionView`](xref:Xamarin.Forms.CollectionView) に設定され、これはページが表示されるときに実行されます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView) で項目が選択されると、`SelectionChanged` イベントが発生します。 イベントが発生すると、`OnSelectionChanged` という名前のイベント ハンドラーが実行されます。

```csharp
async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection != null)
    {
        // ...
    }
}
```

`SelectionChanged` イベントでは、`e.CurrentSelection` プロパティを使用することで、項目に関連付けられているオブジェクトにアクセスできます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView) クラスの詳細については、「[Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)」を参照してください。

## <a name="navigation"></a>ナビゲーション

ナビゲーションは、移動先の URI を指定して、シェル アプリケーション内で実行されます。 ナビゲーション URI には、3 つのコンポーネントがあります。

- "*ルート*"。シェルの視覚階層の一部として存在するコンテンツへのパスを定義します。
- "*ページ*"。 シェルの視覚階層に存在しないページは、シェル アプリケーション内の任意の場所からナビゲーション スタック上にプッシュできます。 たとえば、`NoteEntryPage` は、シェルのビジュアル階層では定義されていませんが、必要に応じてナビゲーション スタックにプッシュできます。
- 1 つまたは複数の "*クエリ パラメーター*"。 クエリ パラメーターは、ナビゲーション中に移動先ページに渡すことができるパラメーターです。

ナビゲーション URI に 3 つのコンポーネントがすべて含まれる必要はありませんが、含まれている場合は //route/page?queryParameters という構造になります

> [!NOTE]
> ルートは、`Route` プロパティを使用して、シェルのビジュアル階層内の要素で定義できます。 ただし、`Route` プロパティが設定されていない場合は (Notes アプリケーションなど)、実行時にルートが生成されます。

シェルのナビゲーションの詳細については、「[Xamarin.Forms シェルのナビゲーション](~/xamarin-forms/app-fundamentals/shell/navigation.md)」を参照してください。

### <a name="register-routes"></a>ルートを登録する

シェルのビジュアル階層に存在しないページに移動するには、最初にシェルのルーティング システムに登録する必要があります。 `Routing.RegisterRoute` メソッドを使用します。 Notes アプリケーションの場合、これは `AppShell` コンストラクターで行われます。

```csharp
public partial class AppShell : Shell
{
    public AppShell()
    {
        // ...
        Routing.RegisterRoute(nameof(NoteEntryPage), typeof(NoteEntryPage));
    }
}
```

この例では、`NoteEntryPage` という名前のルートが、`NoteEntryPage` 型に登録されています。 このページには、URI ベースのナビゲーションを使用して、アプリケーション内の任意の場所から移動できます。

### <a name="perform-navigation"></a>ナビゲーションを実行する

ナビゲーションは、移動先のルートを表す引数を受け取る `GoToAsync` メソッドによって実行されます。

```csharp
await Shell.Current.GoToAsync("NoteEntryPage");
```

この例では、`NoteEntryPage` に移動します。

> [!IMPORTANT]
> シェルのビジュアル階層にないページに移動するときは、ナビゲーション スタックが作成されます。

ページに移動するときに、クエリ パラメーターとしてデータをページに渡すことができます。

```csharp
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

この例では、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で現在選択されている項目を取得し、`NoteEntryPage` に移動します。`Note` オブジェクトの `ID` プロパティの値が、クエリ パラメーターとして `NoteEntryPage.ItemId` プロパティに渡されます。

渡されたデータを受け取るため、`NoteEntryPage` クラスは `QueryPropertyAttribute` で修飾されています

```csharp
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
    // ...
}
```

`QueryPropertyAttribute` の最初の引数では、渡されたデータを `ItemId` プロパティで受け取ることが指定され、2 番目の引数ではクエリ パラメーター ID が指定されています。そのため、上の例にある `QueryPropertyAttribute`では、`GoToAsync` メソッドの呼び出しにおいて URI から `ItemId`クエリ パラメータ―に渡されたデータを `ItemId` プロパティが受信するように、指定しています。 その後、`ItemId` プロパティによって `LoadNote` メソッドが呼び出されて、デバイスからメモが取得されます。

".." を `GoToAsync` メソッドへの引数として指定すると、後方ナビゲーションが実行されます。

```csharp
await Shell.Current.GoToAsync("..");
```

後方ナビゲーションの詳細については、「[後方ナビゲーション](~/xamarin-forms/app-fundamentals/shell/navigation.md#backwards-navigation)」を参照してください。

## <a name="data-binding"></a>データ バインディング

Xamarin.Forms アプリケーションがデータを表示し、相互作用するしくみを簡素化するために、データ バインディングが使用されます。 データ バインディングはユーザー インターフェイスと基礎アプリケーションの間で接続を確立します。 [`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスには、データ バインディングをサポートするためのインフラストラクチャの大部分が含まれています。

データ バインディングでは、*ソース* と *ターゲット* と呼ばれる 2 つのオブジェクトを接続します。 *ソース* オブジェクトはデータを提供します。 *ターゲット* オブジェクトは、ソース オブジェクトのデータを使用し (またしばしば表示し) ます。 たとえば、[`Editor`](xref:Xamarin.Forms.Editor) ("*ターゲット*" オブジェクト) は一般的にその [`Text`](xref:Xamarin.Forms.InputView.Text) プロパティを "*ソース*" オブジェクトのパブリック プロパティ `string` にバインドします。 次の図では、バインドの関係を示します。

![データ バインディング](deepdive-images/data-binding.png)

データ バインディングの主な利点は、ビューとデータ ソース間でデータを同期する心配がないことです。 *ソース* オブジェクトの変更は、バインディング フレームワークによって背後で自動的に *ターゲット* オブジェクトにプッシュされます。そして、ターゲット オブジェクトの変更は、オプションで *ソース* オブジェクトに戻されます。

データ バインディングを確立するには、次の 2 つの手順を実行します。

- *ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは、*ソース* に設定する必要があります。
- バインディングは *ターゲット* と *ソース* 間で確立する必要があります。 XAML でこれは、[`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) マークアップ拡張を使用して実現できます。

Notes アプリケーションでは、`NoteEntryPage` の [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) として設定された `Note` インスタンスがバインディング ソースであるのに対し、バインディング ターゲットはメモを表示する [`Editor`](xref:Xamarin.Forms.Editor) です。 最初、ページ コンストラクターが実行されるときに、`NoteEntryPage` の `BindingContext` が設定されます。

```csharp
public NoteEntryPage()
{
    // ...
    BindingContext = new Note();
}
```

この例では、`NoteEntryPage` が作成されるときに、ページの `BindingContext` に新しい `Note` が設定されます。 これにより、アプリケーションに新しいメモを追加するシナリオが処理されます。

さらに、`NotesPage` で既存のメモが選択されている場合は、`NoteEntryPage` へのナビゲーションが発生したときにも、ページの `BindingContext` を設定できます。

```csharp
[QueryProperty(nameof(ItemId), nameof(ItemId))]
public partial class NoteEntryPage : ContentPage
{
    public string ItemId
    {
        set
        {
            LoadNote(value);
        }

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
        // ...    
    }
}
```

この例では、ページ ナビゲーションが発生すると、選択されている `Note` オブジェクトがデータベースから取得されて、ページの `BindingContext` に設定されます。

> [!IMPORTANT]
> 各 *ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは個々に設定できますが、これは必ずしも行う必要はありません。 `BindingContext` は、その子がすべて継承する特殊なプロパティです。 したがって、[`ContentPage`](xref:Xamarin.Forms.ContentPage) の `BindingContext` が `Note` インスタンスに設定された場合、`ContentPage` のすべての子は同じ `BindingContext` を持ち、`Note` オブジェクトのパブリック プロパティにバインドすることができます。

その後、`NoteEntryPage` 内の [`Editor`](xref:Xamarin.Forms.Editor) が `Note` オブジェクトの `Text` プロパティにバインドされます。

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}" />
```

*ソース* オブジェクトの [`Editor.Text`](xref:Xamarin.Forms.InputView.Text) プロパティと `Text` プロパティ間のバインディングが確立されました。 `Editor` での変更は `Note` オブジェクトに自動的に伝達されます。 同様に、`Note.Text` プロパティが変更された場合、Xamarin.Forms のバインド エンジンによって `Editor` のコンテンツも更新されます。 これは、*双方向のバインド* とも呼ばれています。

データ バインディングの詳細については、「[Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

## <a name="styling"></a>スタイル

Xamarin.Forms アプリケーションには、多くの場合、同じ外観を持つ複数のビジュアル要素が含まれています。 各ビジュアル要素の外観を設定すると、繰り返し使用しやすく、エラーが発生しやすくなります。 代わりに、外観を定義し、必要なビジュアル要素に適用するスタイルを作成できます。

[`Style`](xref:Xamarin.Forms.Style) クラスは、プロパティ値のコレクションを 1 つのオブジェクトにグループ化して、複数のビジュアル要素インスタンスに適用できるようにします。 スタイルは、アプリケーション レベル、ページ レベル、ビュー レベルのいずれかで [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に格納されます。 `Style` を定義する場所の選択は、それを使用できる場所に影響します。

- アプリケーション レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、アプリケーション全体に適用できます。
- ページ レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、ページとその子に適用できます。
- ビュー レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、ビューとその子に適用できます。

> [!IMPORTANT]
> アプリケーション全体で使用されるスタイルは、重複を回避するために、アプリケーションのリソース ディクショナリに保存されます。 ただし、あるページに固有の XAML は、アプリケーションのリソース ディクショナリに含めるべきではありません。アプリのリソースは、ページが必要とするときではなく、アプリケーションの起動時に解析されるためです。 詳細については、「[アプリケーション リソース ディクショナリのサイズを減らす](~/xamarin-forms/deploy-test/performance.md#reduce-the-application-resource-dictionary-size)」を参照してください。

各 [`Style`](xref:Xamarin.Forms.Style) インスタンスには、1 つ以上の [`Setter`](xref:Xamarin.Forms.Setter) オブジェクトのコレクションが含まれています。各 `Setter` には、[`Property`](xref:Xamarin.Forms.Setter.Property)と[`Value`](xref:Xamarin.Forms.Setter.Value)があります。 `Property` は、スタイルが適用される要素のバインド可能なプロパティの名前で、`Value` はプロパティに適用される値です。 次のコード例は、`NoteEntryPage` のスタイルを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.Views.NoteEntryPage"
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

## <a name="test-and-deployment"></a>テストと展開

Visual Studio for Mac と Visual Studio のいずれも、アプリケーションをテストおよび展開するためのオプションを多数用意しています。 アプリケーションのデバッグは、アプリケーション開発ライフサイクルの一般的な部分であり、コードの問題を診断するときに役立ちます。 詳細については、「[Set a Breakpoint](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)」(ブレークポイントの設定)、「[Step Through Code](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)」(コードのステップ スルー)、「[Output Information to the Log Window](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)」(ログ ウィンドウへの出力情報) を参照してください。

シミュレーターは、アプリケーションの展開とテストを始めるにはおすすめの場所です。また、テスト アプリケーションに役立つ機能があります。 ただし、ユーザーは完成したアプリケーションをシミュレーター上では使用しないので、早期に、そして何度も実際のデバイス上でアプリケーションをテストすることをお勧めします。 iOS デバイスのプロビジョニングの詳細については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)」(デバイスのプロビジョニング) を参照してください。 Android デバイスのプロビジョニングの詳細については、「[Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)」(開発用のデバイスの設定) を参照してください。

## <a name="next-steps"></a>次の手順

この詳細では、Xamarin.Forms シェルを使用したアプリケーション開発の基礎について説明しました。 推奨される次の手順としては、次の機能の説明を読んでください。

- Xamarin.Forms シェルでは、ほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さが軽減されます。 詳細は、「[Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。
- Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するには、複数のコントロール グループが使用されます。 詳細については、「[Controls Reference](~/xamarin-forms/user-interface/controls/index.md)」 (コントロールのリファレンス) を参照してください。
- データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティでの変更が自動的にもう片方のプロパティに反映されるようにする手法です。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。
- Xamarin.Forms には、使用されるページの種類に応じて、複数のページ ナビゲーション エクスペリエンスが用意されています。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。
- スタイルは、繰り返されるマークアップを減らすのに役立ち、アプリケーションの外観をより簡単に変更できるようにします。 詳細については、「[Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/index.md)」を参照してください。
- データ テンプレートでは、サポートされているビューでのデータの表現方法を定義する機能が提供されます。 詳細については、「[Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」 (データ テンプレート) を参照してください。
- エフェクトでは、各プラットフォームのネイティブ コントロールのカスタマイズを可能にします。 エフェクトは、プラットフォーム固有のプロジェクトで [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) クラスをサブクラス化することによって作成され、適切な Xamarin.Forms コントロールに添付することによって使用されます。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (エフェクト) を参照してください。
- 各ページ、レイアウト、およびビューは `Renderer` クラスを使用して、プラットフォームごとに異なる方法でレンダリングされます。その後、ネイティブ コントロールが作成され、画面に配置され、共有コードで指定された動作が追加されます。 開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。
- 共有コードはネイティブ機能に [`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスを介してアクセスできます。 詳細については、「[Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)」 (DependencyService を使用したネイティブ機能へのアクセス) を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms のシェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [eXtensible Application Markup Language (XAML)](~/xamarin-forms/xaml/index.yml)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [コントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
- [入門サンプル](/samples/browse/?products=xamarin&term=Xamarin.Forms%2bget%2bstarted)
- [Xamarin.Forms サンプル](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API リファレンス](xref:Xamarin.Forms)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/Xamarin-Solution-Architecture-4-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
