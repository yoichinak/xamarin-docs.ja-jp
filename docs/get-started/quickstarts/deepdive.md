---
title: Xamarin.Forms のクイックスタートの詳細
description: この記事では、Xamarin.Forms を使用したアプリケーション開発の基礎について説明します。 たとえば、Xamarin.Forms アプリケーションの構造、アプリケーションのアーキテクチャ、と基礎、ユーザー インターフェイスについて説明しました。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: 997c9e023a743b8e5128ffc566e50da63652f945
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739006"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin.Forms のクイックスタートの詳細

Xamarin の[クイックスタート](~/get-started/index.yml)では、note アプリケーションがビルドされました。 この記事では、Xamarin.Forms アプリケーションのしくみの基礎を理解するために、構築された内容を確認します。

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio の概要

Visual Studio は、コードを*ソリューション*と*プロジェクト*に分けて整理しています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 メモアプリケーションは、次のスクリーンショットに示すように、4つのプロジェクトを含む1つのソリューションで構成されています。

![](deepdive-images/vs/solution.png "Visual Studio ソリューション エクスプローラー")

プロジェクトの内容:

- 注: このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリプロジェクトです。
- Notes.Android-このプロジェクトは Android 固有のコードを保持し、Android アプリケーションのエントリポイントです。
- Notes.iOS: このプロジェクトは、ios 固有のコードを保持し、iOS アプリケーションのエントリポイントです。
- Notes.UWP: このプロジェクトは、ユニバーサル Windows プラットフォーム (UWP) 固有のコードを保持し、UWP アプリケーションのエントリポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーンショットは、Visual Studio の Notes .NET Standard library プロジェクトの内容を示しています。

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard プロジェクトの内容")

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet**&ndash;プロジェクトに追加された Xamarin.Forms および sqlite-net pcl NuGet パッケージ。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージです。

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Visual Studio for Mac の概要

[Visual Studio for Mac ](/visualstudio/mac/)は、コードを*ソリューション*と*プロジェクト*に分けて整理するという Visual Studio の方法に従っています。 ソリューションとは、1 つまたは複数のプロジェクトを保持できるコンテナーです。 プロジェクトは、アプリケーション、サポートするライブラリ、テスト アプリケーションなどの場合があります。 メモアプリケーションは、次のスクリーンショットに示すように、3つのプロジェクトを含む1つのソリューションで構成されます。

![](deepdive-images/vsmac/solution.png "Visual Studio for Mac ソリューション ウィンドウ")

プロジェクトの内容:

- Notes: このプロジェクトは、すべての共有コードと共有 UI を保持する .NET Standard ライブラリプロジェクトです。
- Notes.Android: このプロジェクトは、android 固有のコードを保持し、Android アプリケーションのエントリポイントです。
- Notes.iOS: このプロジェクトは、ios 固有のコードを保持し、iOS アプリケーションのエントリポイントです。

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの構造

次のスクリーンショットは、Visual Studio for Mac の Notes .NET Standard library プロジェクトの内容を示しています。

![](deepdive-images/vsmac/net-standard-project.png "Phoneword .NET Standard ライブラリ プロジェクトの内容")

このプロジェクトには、**NuGet** ノードと **SDK** ノードを含む **Dependencies** ノードがあります。

- **NuGet**&ndash;プロジェクトに追加された Xamarin.Forms および sqlite-net pcl NuGet パッケージ。
- **SDK** &ndash; .NET Standard を定義する NuGet パッケージの完全なセットを参照する `NETStandard.Library` メタパッケージです。

::: zone-end

このプロジェクトには、以下の複数のファイルも含まれています。

- **Data\NoteDatabase.cs** –このクラスには、データベースを作成し、そこからデータを読み取り、データを書き込んでからデータを削除するためのコードが含まれています。
- **Modelabout** : このクラスは、アプリケーション内の`Note`各メモに関するデータをインスタンスに格納するモデルを定義します。
- **App.xaml**: `App` クラスの XAML マークアップ。アプリケーションのリソース ディクショナリを定義します。
- **App.xaml.cs**: `App` クラスの分離コード。各プラットフォーム上のアプリケーションで表示される最初のページのインスタンス化と、アプリケーションのライフサイクル イベント処理を担当します。
- **AssemblyInfo.cs** –このファイルには、アセンブリレベルで適用されるプロジェクトに関するアプリケーション属性が含まれています。
- **[注釈]** : `NotesPage`クラスの xaml マークアップ。アプリケーションの起動時に表示されるページの UI を定義します。
- **NotesPage.xaml.cs** – `NotesPage`クラスの分離コード。ユーザーがページと対話するときに実行されるビジネスロジックを格納します。
- **NoteEntryPage** –ユーザーがノートを入力した`NoteEntryPage`ときに表示されるページの UI を定義する、クラスの xaml マークアップ。
- **NoteEntryPage.xaml.cs** – `NoteEntryPage`クラスの分離コード。ユーザーがページと対話するときに実行されるビジネスロジックを格納します。

Xamarin.iOS アプリケーションの構造については、「[Anatomy of a Xamarin.iOS Application](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application)」(Xamarin.iOS アプリケーションの構造) を参照してください。 Xamarin.Android アプリケーションの構造については、「[Anatomy of a Xamarin.Android Application](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy)」(Xamarin.Android アプリケーションの構造) を参照してください。

## <a name="architecture-and-application-fundamentals"></a>アーキテクチャとアプリケーションの基礎

Xamarin.Forms アプリケーションは、従来のクロスプラットフォーム アプリケーションと同じ方法で設計されています。 通常、共有コードは .NET Standard ライブラリに配置され、プラットフォーム固有のアプリケーションは共有コードを使用します。 次の図は、Notes アプリケーションのこのリレーションシップの概要を示しています。

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "メモアーキテクチャ")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "メモアーキテクチャ")

::: zone-end

スタートアップ コードを最大限に再利用するために、Xamarin.Forms アプリケーションは `App` という 1 つのクラスがあります。このクラスは、各プラットフォーム上のアプリケーションが表示する最初のページのインスタンス化を担当しています。次にコード例を示します。

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

このコードは、 `MainPage` `App`クラスのプロパティを、コンテンツ[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)が`NotesPage`インスタンスであるインスタンスに設定します。

また、 **AssemblyInfo.cs**ファイルには、アセンブリレベルで適用される1つのアプリケーション属性が含まれています。

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

属性[`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)は xaml コンパイラをオンにして、xaml が中間言語に直接コンパイルされるようにします。 詳細については、「[XAML Compilation](~/xamarin-forms/xaml/xamlc.md)」(XAML のコンパイル) を参照してください。

## <a name="launching-the-application-on-each-platform"></a>各プラットフォームでアプリケーションを起動する

### <a name="ios"></a>iOS

Ios で最初の Xamarin.Forms ページを起動するには、次のように`AppDelegate` 、 `FormsApplicationDelegate`クラスを継承するクラスを定義します。

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

Android で最初の Xamarin.Forms ページを起動するには、 `Activity` `MainLauncher`属性を使用してを作成するコードが含まれています。この`FormsAppCompatActivity`コードには、クラスから継承するアクティビティがあります。

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

その結果、Xamarin.Forms の UWP 固有の実装がアプリケーションに読み込まれます。 最初の Xamarin.Forms ページは、 `MainPage`クラスによって起動されます。

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
> ユニバーサル Windows プラットフォームアプリは、Xamarin を使用してビルドできますが、Windows では Visual Studio のみを使用します。

::: zone-end

## <a name="user-interface"></a>ユーザーインターフェイス

Xamarin.Forms アプリケーションのユーザーインターフェイスを作成するには、次の4つの主要なコントロールグループが使用されます。

1. **ページ**: Xamarin.Forms のページは、クロスプラットフォーム モバイル アプリケーション画面を表しています。 メモアプリケーションは[`ContentPage`](xref:Xamarin.Forms.ContentPage)クラスを使用して、1つの画面を表示します。 ページの詳細については、「[Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md)」(Xamarin.Forms のページ) を参照してください。
1. **ビュー**: Xamarin.Forms のビューは、ユーザー インターフェイスに表示されるコントロールです。たとえば、ラベル、ボタン、テキスト入力ボックスなどです。 完成したノートアプリケーションは[`ListView`](xref:Xamarin.Forms.ListView)、 [`Editor`](xref:Xamarin.Forms.Editor)、、 [`Button`](xref:Xamarin.Forms.Button)およびの各ビューを使用します。 ビューの詳細については、「[Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md)」(Xamarin.Forms のビュー) を参照してください。
1. **レイアウト**: Xamarin.Forms のレイアウトは、ビューを論理構造にまとめるために使用されるコンテナーです。 メモアプリケーションでは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)クラスを使用して、ビューを垂直方向の[`Grid`](xref:Xamarin.Forms.Grid)スタックに配置し、クラスを使用してボタンを水平方向に配置します。 レイアウトの詳細については、「[Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md)」(Xamarin.Forms のレイアウト) を参照してください。
1. **セル**: Xamarin.Forms セルは、一覧内の項目に使用される特殊な要素です。一覧内の各項目を描画する方法を示しています。 メモアプリケーションでは、 [`TextCell`](xref:Xamarin.Forms.TextCell)を使用して、リストの各行に2つの項目を表示します。 セルの詳細については、「[Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md)」(Xamarin.Forms のセル) を参照してください。

実行時に、各コントロールはネイティブの同等のものにマップされます。そしてそれがレンダリングされます。

### <a name="layout"></a>レイアウト

メモアプリケーションでは、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)を使用してクロスプラットフォームアプリケーションの開発を簡略化します。画面のサイズに関係なく、画面にビューを自動的に配置します。 各子要素は、追加した順に、水平または垂直方向に 1 つずつ配置されます。 `StackLayout` が使用する領域の量は、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティの設定によって異なりますが、`StackLayout` は既定で全画面を使用しようとします。

次の XAML コードは、を使用して[`StackLayout`](xref:Xamarin.Forms.StackLayout)を`NoteEntryPage`レイアウトする例を示しています。

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

既定では[`StackLayout`](xref:Xamarin.Forms.StackLayout) 、は垂直方向であることを前提としています。 ただし、 [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)プロパティを[`StackOrientation.Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)列挙メンバーに設定することによって、水平方向に変更することができます。

> [!NOTE]
> ビューのサイズは、プロパティ`HeightRequest`と`WidthRequest`プロパティを使用して設定できます。

[`StackLayout`](xref:Xamarin.Forms.StackLayout) クラスの詳細については、「[StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)」を参照してください。

### <a name="responding-to-user-interaction"></a>ユーザー操作に対する応答

XAML に定義されているオブジェクトによって、分離コード ファイルで処理されるイベントが発生する可能性があります。 次のコード例は、 `OnSaveButtonClicked` `NoteEntryPage`クラスの分離コード内のメソッドを示しています。このメソッドは、 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) [*保存*] ボタンでのイベントの発生に応答して実行されます。

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

メソッド`OnSaveButtonClicked`は、データベースにメモを保存し、前のページに戻ります。

> [!NOTE]
> XAML クラスの分離コード ファイルは、`x:Name` 属性を指定して割り当てられた名前を使用して、XAML に定義されているオブジェクトにアクセスできます。 この属性に割り当てられている値は、C# 変数と同じルールを持っています。つまり、英字またはアンダースコアから始まり、埋め込みスペースが含まれる必要があります。

`OnSaveButtonClicked`メソッドへの [保存] ボタンの配線は、 `NoteEntryPage`クラスの XAML マークアップで発生します。

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>表示内容

は[`ListView`](xref:Xamarin.Forms.ListView) 、リスト内の項目のコレクションを垂直方向に表示する役割を担います。 内の各項目`ListView`は、1つのセルに格納されます。

次のコード例は、 [`ListView`](xref:Xamarin.Forms.ListView) `NotesPage`からのを示しています。

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

の[`ListView`](xref:Xamarin.Forms.ListView)各行のレイアウトは、 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)要素内で定義され、データバインディングを使用して、アプリケーションによって取得されるすべてのメモを表示します。 プロパティは、次のように`NotesPage.xaml.cs`データソースに設定されます。 [`ListView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)データベースに格納されているすべてのメモをに設定します。

で[`ListView`](xref:Xamarin.Forms.ListView)行を選択すると、イベントが[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)発生します。 イベントが発生すると`OnListViewItemSelected`、という名前のイベントハンドラーが実行されます。

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

イベント[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)は、プロパティを[`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem)使用して、セルに関連付けられているオブジェクトにアクセスできます。

クラスの[`ListView`](xref:Xamarin.Forms.ListView)詳細については、「 [ListView](~/xamarin-forms/user-interface/listview/index.md)」を参照してください。

## <a name="navigation"></a>ナビゲーション

Xamarin.Forms は、使用している [`Page`](xref:Xamarin.Forms.Page) 型に応じて多数のページ ナビゲーション エクスペリエンスを提供します。 インスタンス[`ContentPage`](xref:Xamarin.Forms.ContentPage)の場合、ナビゲーションは階層構造またはモーダルにできます。 モーダルナビゲーションの詳細については、「 [Xamarin のモーダルページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)」を参照してください。

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) クラス、[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) クラスおよび [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) クラスは別のナビゲーション エクスペリエンスを提供します。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。

階層ナビゲーション[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)では、クラスを使用して、必要に応じ[`ContentPage`](xref:Xamarin.Forms.ContentPage)て、オブジェクトのスタック間を移動したり、順方向に移動したりします。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。 1 つのページから別のページに移動するには、アプリケーションは新しいページを、そこでアクティブなページとなるナビゲーション スタックにプッシュします。 前のページに戻るには、アプリケーションは現在のページをナビゲーション スタックからポップします。そして新しい最上位のページがアクティブ ページになります。

`NavigationPage` クラスはまた、ページの最上部にナビゲーション バーを追加します。このバーには、タイトルと、前にページに戻るための **[戻る]** ボタンが表示されます。このボタンはプラットフォーム固有です。

ナビゲーションスタックに追加された最初のページは、アプリケーションの*ルート*ページと呼ばれます。次のコード例は、この方法をメモアプリケーションで実現する方法を示しています。

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

これにより、 `NoteEntryPage`新しいオブジェクトがナビゲーションスタックにプッシュされ、それがアクティブページになります。

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。 元のページにプログラムを使用して戻るには、`NoteEntryPage` オブジェクトが次のコード例のように [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) メソッドを呼び出す必要があります。

```csharp
await Navigation.PopAsync();
```

階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

## <a name="data-binding"></a>データ バインディング

Xamarin.Forms アプリケーションがそのデータを表示し、相互作用するしくみを簡単にするためにデータ バインディングが使用されます。 データ バインディングはユーザー インターフェイスと基礎アプリケーションの間で接続を確立します。 [`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスには、データ バインディングをサポートするためのインフラストラクチャの大部分が含まれています。

データ バインディングでは、*ソース*と*ターゲット*と呼ばれる 2 つのオブジェクトを接続します。 *ソース* オブジェクトはデータを提供します。 *ターゲット* オブジェクトは、ソース オブジェクトのデータを使用し (またしばしば表示し) ます。 たとえば、 [`Editor`](xref:Xamarin.Forms.Editor) (*ターゲット*オブジェクト) は、通常、*ソース*オブジェクト[`Text`](xref:Xamarin.Forms.Editor.Text)のパブリック`string`プロパティにプロパティをバインドします。 次の図では、バインドの関係を示します。

![](deepdive-images/data-binding.png "データ バインディング")

データ バインディングの主な利点は、ビューとデータ ソース間でデータを同期する心配がないことです。 *ソース* オブジェクトの変更は、バインディング フレームワークによって背後で自動的に*ターゲット* オブジェクトにプッシュされます。そして、ターゲット オブジェクトの変更は、オプションで*ソース* オブジェクトに戻されます。

データバインディングを確立するには、次の2段階の手順を実行します。

- *ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは、*ソース*に設定する必要があります。
- バインディングは*ターゲット*と*ソース*間で確立する必要があります。 XAML でこれは、[`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) マークアップ拡張を使用して実現できます。

メモアプリケーションでは、バインディングターゲットは[`Editor`](xref:Xamarin.Forms.Editor) 、メモ`Note`を表示するですが、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)の`NoteEntryPage`として設定されたインスタンスはバインディングソースです。

のは、次のコード例に示すように、ページナビゲーション中に設定されます。`NoteEntryPage` `BindingContext`

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

アプリケーションに新しいメモが追加されたときに実行される`NoteEntryPage` `Note`メソッドでは、[のが新しいインスタンスに設定されます。`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) `OnNoteAddedClicked` `Note` `NoteEntryPage` [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) `OnListViewItemSelected` で`BindingContext`既存のメモを選択したときに実行されるメソッドでは、のは、プロパティを通じてアクセスされる、選択されたインスタンスに設定されます。 [`ListView`](xref:Xamarin.Forms.ListView)

> [!IMPORTANT]
> 各*ターゲット* オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティは個々に設定できますが、これは必ずしも行う必要はありません。 `BindingContext` は、その子がすべて継承する特殊なプロパティです。 したがって、の`BindingContext`が[`ContentPage`](xref:Xamarin.Forms.ContentPage) `Note` `BindingContext`インスタンスに`Note`設定されている場合、のすべての子は同じであり、オブジェクトのパブリックプロパティにバインドできます。`ContentPage`

のは[`Editor`](xref:Xamarin.Forms.Editor) `Text` 、オブジェクト`Note`のプロパティにバインドされます。 `NoteEntryPage`

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

*ソース* オブジェクトの [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) プロパティと `Text` プロパティ間のバインディングが確立されました。 に加えら`Editor`れた変更は、自動的に`Note`オブジェクトに反映されます。 同様に、 `Note.Text`プロパティに変更が加えられた場合は、Xamarin のバインドエンジンによって`Editor`のコンテンツも更新されます。 これは、*両方向のバインド*とも呼ばれています。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="styling"></a>[スタイル]

多くの場合、Xamarin アプリケーションには、同じ外観を持つ複数のビジュアル要素が含まれています。 各ビジュアル要素の外観を設定すると、繰り返しやすく、エラーが発生しやすくなります。 代わりに、外観を定義し、必要なビジュアル要素に適用するスタイルを作成できます。

クラス[`Style`](xref:Xamarin.Forms.Style)は、プロパティ値のコレクションを1つのオブジェクトにグループ化して、複数のビジュアル要素インスタンスに適用できるようにします。 スタイルは、アプリケーションレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)、ページレベル、またはビューレベルでに格納されます。 使用可能な影響を`Style`定義する場所の選択:

- [`Style`](xref:Xamarin.Forms.Style)アプリケーションレベルで定義されたインスタンスは、アプリケーション全体で適用できます。
- [`Style`](xref:Xamarin.Forms.Style)ページレベルで定義されたインスタンスは、ページとその子に適用できます。
- [`Style`](xref:Xamarin.Forms.Style)ビューレベルで定義されたインスタンスは、ビューとその子に適用できます。

> [!IMPORTANT]
> アプリケーション全体で使用されるすべてのスタイルは、重複を避けるために、アプリケーションのリソースディクショナリに格納されます。 ただし、ページに固有の XAML は、アプリケーションのリソースディクショナリに含めないでください。リソースは、ページで要求されるときではなく、アプリケーションの起動時に解析されます。

各[ `Style` ](xref:Xamarin.Forms.Style)インスタンスには、1 つまたは複数のコレクションが含まれています[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクトは、各`Setter`を持つ、 [ `Property` ](xref:Xamarin.Forms.Setter.Property)および[`Value`](xref:Xamarin.Forms.Setter.Value)。 は、スタイルが適用される要素のバインド可能なプロパティの名前です。は、 `Value`プロパティに適用される値です。 `Property` 次のコード例は、からの`NoteEntryPage`スタイルを示しています。

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

このスタイルは、ページ上[`Editor`](xref:Xamarin.Forms.Editor)の任意のインスタンスに適用されます。

を作成[`Style`](xref:Xamarin.Forms.Style)する場合[`TargetType`](xref:Xamarin.Forms.Style.TargetType) 、プロパティは常に必須です。

> [!NOTE]
> Xamarin.Forms アプリケーションのスタイル設定は、従来、XAML スタイルを使用して実現されています。 ただし、カスケードスタイルシート (CSS) を使用してビジュアル要素のスタイルを設定することもできます。 詳細については、「[カスケードスタイルシートを使用した Xamarin.Forms アプリのスタイル設定 (CSS)](~/xamarin-forms/user-interface/styles/css/index.md)」を参照してください。

XAML のスタイルの詳細については、次を参照してください。[XAML スタイルを使った Xamarin.Forms アプリのスタイリング](~/xamarin-forms/user-interface/styles/xaml/index.md)。

### <a name="providing-platform-specific-styles"></a>プラットフォーム固有のスタイルの提供

マーク`OnPlatform`アップ拡張機能を使用すると、プラットフォームごとに UI の外観をカスタマイズできます。

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

これ[`Style`](xref:Xamarin.Forms.Style)により、使用されている[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)プラットフォームに応じて、のプロパティ[`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)と[`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)プロパティに異なる[`Color`](xref:Xamarin.Forms.Color)値が設定されます。

XAML マークアップ拡張機能の詳細については、「[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。 マークアップ拡張機能の詳細については、「 [onplatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」を参照してください。 `OnPlatform`

## <a name="testing-and-deployment"></a>テストと展開

Visual Studio for Mac と Visual Studio のいずれも、アプリケーションをテストおよび展開するためのオプションを多数用意しています。 アプリケーションのデバッグは、アプリケーション開発ライフサイクルの一般的な部分であり、コードの問題を診断するときに役立ちます。 詳細については、「[Set a Breakpoint](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)」(ブレークポイントの設定)、「[Step Through Code](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)」(コードのステップ スルー)、「[Output Information to the Log Window](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)」(ログ ウィンドウへの出力情報) を参照してください。

シミュレーターは、アプリケーションの展開とテストを始めるにはおすすめの場所です。また、テスト アプリケーションに役立つ機能があります。 ただし、ユーザーは完成したアプリケーションをシミュレーター上では使用しないので、早期に、そして何度も実際のデバイス上でアプリケーションをテストすることをお勧めします。 iOS デバイスのプロビジョニングの詳細については、「[Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)」(デバイスのプロビジョニング) を参照してください。 Android デバイスのプロビジョニングの詳細については、「[Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)」(開発用のデバイスの設定) を参照してください。

## <a name="next-steps"></a>次の手順

この詳細については、Xamarin を使用したアプリケーション開発の基礎について説明しました。 推奨される次の手順としては、次の機能の説明を読んでください。

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

- [拡張可能なアプリケーションマークアップ言語 (XAML)](~/xamarin-forms/xaml/index.yml)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [コントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [入門サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms%20get%20started)
- [Xamarin.Forms API リファレンス](xref:Xamarin.Forms)
- [無料のセルフ ガイド学習 (ビデオ)](https://university.xamarin.com/self-guided/)
