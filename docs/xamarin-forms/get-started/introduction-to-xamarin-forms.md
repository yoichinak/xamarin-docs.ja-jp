---
title: "Xamarin.Forms の概要"
description: "Xamarin.Forms は、開発者が Android、iOS、Windows、および Windows Phone 間で共有できるユーザー インターフェイスを簡単に作成できる、クロス プラットフォームのネイティブ サポートの、抽象型 UI ツールキットです。 ユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの外観を維持できます。 この記事では、Xamarin.Forms の概要と、Xamarin.Forms を使用したアプリケーションの記述方法を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: be4b0c907774c33dfcd1818da167acb2dc3b04dd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Xamarin.Forms の概要

_Xamarin.Forms は、開発者が Android、iOS、Windows、および Windows Phone 間で共有できるユーザー インターフェイスを簡単に作成できる、クロス プラットフォームのネイティブ サポートの、抽象型 UI ツールキットです。ユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの外観を維持できます。この記事では、Xamarin.Forms の概要と、Xamarin.Forms を使用したアプリケーションの記述方法を説明します。_

<a name="Overview" />

## <a name="overview"></a>概要

Xamarin.Forms は、開発者がクロス プラットフォームのユーザー インターフェイスをすばやく作成するためのフレームワークです。 ユーザー インターフェイスは独自に抽象化され、iOS、Android、Windows、または Windows Phone のネイティブ コントロールでレンダリングされます。 つまり、ユーザー インターフェイスのコードの大部分は同じでありながら、ターゲット プラットフォームのネイティブな外観を維持できることを意味します。

Xamarin.Forms では、時間と共に複雑なアプリケーションに発展させることが可能なアプリケーションのプロトタイプを迅速に作成できます。 Xamarin.Forms のアプリケーションはネイティブ アプリケーションであるため、ブラウザー サンドボックス、限定的な API、パフォーマンスの低さなど他のツールキットにあるような制限はありません。 Xamarin.Forms を使用して記述されたアプリケーションは、(これらのみに限定されるものではありませんが) iOS の CoreMotion、PassKit、および StoreKit や、Android の NFC および Google Play 開発者サービスや、Windows のタイルなど、基になるプラットフォームの任意の API または機能を使用できます。 さらに、ユーザー インターフェイスの一部を Xamarin.Forms で作成し、他の部分はネイティブ UI ツールキットを使用してアプリケーションを作成することもできます。

Xamarin.Forms アプリケーションは、従来のクロスプラットフォーム アプリケーションと同じ方法で設計されています。 [ポータブル ライブラリ](~/cross-platform/app-fundamentals/pcl.md)または[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)を使用して共有コードを格納し、共有コードを使用するそのプラットフォーム用のアプリケーションを作成する方法が最も一般的です。

Xamarin.Forms でユーザー インターフェイスを作成する技法は 2 つあります。 1 番目の技法では、UI をすべて C# ソース コードで作成します。 2 番目の技法では、ユーザー インターフェイスを表す宣言型マークアップ言語である *Extensible Application Markup Language* (XAML) を使用する方法です。 XAML の詳細については、「[XAML Basics](~/xamarin-forms/xaml/xaml-basics/index.md)」 (XAML の基礎) を参照してください。

この資料は、次のトピックで Xamarin.Forms フレームワークの基礎を説明します。

-  [Xamarin.Forms アプリケーションの検証](#Examining_A_Xamarin.Forms_Application).
-  [Xamarin.Forms のページとコントロールの使用方法](#Views_and_Layouts).
-  [データ リストの表示方法](#Lists_in_Xamarin.Forms).
-  [データ バインディングの設定方法](#Data_Binding).
-  [ページ間の移動方法](#Navigation).
-  [次の手順](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Xamarin.Forms アプリケーションの検証

Visual Studio for Mac および Visual Studio の既定の Xamarin.Forms アプリ テンプレートでは、ユーザーにテキストを表示する最も単純な Xamarin.Forms ソリューションを作成できます。 このアプリケーションを実行すると、次のスクリーンショットのような表示があります。

[![](introduction-to-xamarin-forms-images/image05-sml.png "既定の Xamarin.Forms アプリケーション")](introduction-to-xamarin-forms-images/image05.png#lightbox "既定の Xamarin.Forms アプリケーション")

スクリーンショットの各画面は、Xamarin.Forms の *Page* に該当します。 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) は、Android では *Activity*、iOS では *View Controller*、ユニバーサル Windows プラットフォーム (UWP) では *Page* を表します。 上記のスクリーンショットのサンプルは、[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) オブジェクトのインスタンスを作成し、それを使用して [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) を表示します。

スタートアップ コードを最大限に再利用できるようにするために、Xamarin.Forms アプリケーションには `App` という 1 つのクラスがあります。このクラスは、表示される最初の [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) のインスタンス化を担当しています。 `App` クラスのコード例は次のとおりです。

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

このコードは、ページ上で垂直および水平方向の両方に 1 つの [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) を表示する新しい [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) オブジェクトをインスタンス化します。

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>各プラットフォームでの最初の Xamarin.Forms ページの起動

この [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) をアプリケーション内で使用するには、各プラットフォーム アプリケーションで Xamarin.Forms フレームワークを初期化して、起動時に [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) のインスタンスを提供する必要があります。 この初期化手順はプラットフォームによって異なります。詳細については、次のセクションで説明します。

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

iOS で最初の Xamarin.Forms ページを起動するために、プラットフォーム プロジェクトには、`Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` クラスから継承される `AppDelegate` クラスが含まれています。次にコード例を示します。

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

`FinishedLoading` オーバーライドは、`Init` メソッドを呼び出して Xamarin.Forms のフレームワークを初期化します。 その結果、Xamarin.Forms の iOS 固有の実装がアプリケーションに読み込まれ、次に `LoadApplication` メソッドの呼び出しによってルート ビュー コントローラーが設定されます。

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Android で最初の Xamarin.Forms ページを起動するために、プラットフォーム プロジェクトには、`MainLauncher` 属性を指定した `Activity` を作成し、`FormsApplicationActivity` クラスから継承したアクティビティが使用するコードが含まれています。次にコード例を示します。

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

`OnCreate` オーバーライドは、`Init` メソッドを呼び出して Xamarin.Forms のフレームワークを初期化します。 その結果、Xamarin.Forms の Android 固有の実装がアプリケーションに読み込まれ、次に Xamarin.Forms アプリケーションが読み込まれます。

<a name="Launching_in_Windows_Phone" />

#### <a name="windows-phone-81-winrt"></a>Windows Phone 8.1 (WinRT)

Windows ランタイム アプリケーションでは、Xamarin.Forms フレームワークを初期化する `Init` メソッドは `App` から呼び出されます。

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

その結果、Xamarin.Forms の Windows Phone 固有の実装がアプリケーションに読み込まれます。 最初の Xamarin.Forms ページは、次のコード例のように `MainPage` クラスによって起動されます。

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.NavigationCacheMode = NavigationCacheMode.Required;
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms アプリケーションは `LoadApplication` メソッドを使用して読み込まれます。

Windows 8.1 もサポートする Xamarin.Forms このプロジェクト タイプの構成方法の詳細については、「[Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md)」 (Windows プロジェクトの設定) を参照してください。

#### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) アプリケーションでは、Xamarin.Forms フレームワークを初期化する `Init` メソッドが `App` から呼び出されます。

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

その結果、Xamarin.Forms の UWP 固有の実装がアプリケーションに読み込まれます。 最初の Xamarin.Forms ページは、次のコード例のように `MainPage` クラスによって起動されます。

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms アプリケーションは `LoadApplication` メソッドを使用して読み込まれます。

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>ビューおよびレイアウト

Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するために、主に 4 つのコントロール グループが使用されます。

1. **ページ**: Xamarin.Forms のページは、クロスプラットフォーム モバイル アプリケーション画面を表しています。 ページの詳細については、「[Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md)」(Xamarin.Forms のページ) を参照してください。
1. **レイアウト**: Xamarin.Forms のレイアウトは、ビューを論理構造にまとめるために使用されるコンテナーです。 レイアウトの詳細については、「[Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md)」(Xamarin.Forms のレイアウト) を参照してください。
1. **ビュー**: Xamarin.Forms のビューは、ユーザー インターフェイスに表示されるコントロールです。たとえば、ラベル、ボタン、テキスト入力ボックスなどです。 ビューの詳細については、「[Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md)」(Xamarin.Forms のビュー) を参照してください。
1. **セル**: Xamarin.Forms セルは、一覧内の項目に使用される特殊な要素です。一覧内の各項目を描画する方法を示しています。 セルの詳細については、「[Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md)」(Xamarin.Forms のセル) を参照してください。

実行時に、各コントロールはネイティブの同等のものにマップされます。そしてそれがレンダリングされます。

コントロールはレイアウト内でホストされています。 共通に使用されるレイアウトを実装する [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) クラスをこれから検証します。

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) では、クロスプラットフォーム アプリケーションの開発で、画面サイズにかかわらず、画面上のコントロールを簡単に自動的に整えることができるようにします。 各子要素は、追加した順に、水平または垂直方向に 1 つずつ配置されます。 `StackLayout` が使用する領域の量は、[`HorizontalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) プロパティと [`VerticalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) プロパティの設定によって異なりますが、`StackLayout` は既定で全画面を使用しようとします。

次の XAML コードは、3 つ [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) コントロールを整える [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) の使用例を示しています。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

これと同じ C# コードの例は次のとおりです。

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

既定では、[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) は次のスクリーンショットのとおり、垂直方向を前提としています。

[![](introduction-to-xamarin-forms-images/image09-sml.png "垂直方向の StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "垂直方向の StackLayout")

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) の向きは、次の XAML コードのとおり、水平方向に変更することもできます。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

これと同じ C# コードの例は次のとおりです。

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

次のスクリーンショットは、結果のレイアウトです。

[![](introduction-to-xamarin-forms-images/image10-sml.png "水平方向の StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "水平方向の StackLayout")

コントロールのサイズは、次の XAML コードの例のとおり、`HeightRequest` と `WidthRequest` プロパティを使用して設定することも可能です。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

これと同じ C# コードの例は次のとおりです。

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

次のスクリーンショットは、結果のレイアウトです。

[![](introduction-to-xamarin-forms-images/image11-sml.png "LayoutOptions を使用した水平方向の StackLayout")](introduction-to-xamarin-forms-images/image11.png#lightbox "LayoutOptions を使用した水平方向の StackLayout")

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) クラスの詳細については、「[StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)」を参照してください。

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Xamarin.Forms のリスト

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールでは、画面にアイテムのコレクションを表示します。`ListView` の各アイテムは、1 つのセルに入れられます。 既定で `ListView` は組み込みの [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) テンプレートを使用し、1 行のテキストをレンダリングします。

次のコード例は、単純な [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) の例を示しています。

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

次のスクリーンショットは、結果の [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) を示しています。

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールの詳細については、「[ListView](~/xamarin-forms/user-interface/listview/index.md)」を参照してください。

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>カスタム クラスへのバインド

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールは、既定の [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) テンプレートを使用してカスタム オブジェクトを表示することも可能です。

次に示すのは、`TodoItem` クラスのコード例です。

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールは、次のコード例のように入力できます。

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

次のコード例に示すように、[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) でどの `TodoItem` プロパティを表示するか設定するために、バインディングを作成できます。

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

これは、`TodoItem.Name` プロパティへのパスを指定するバインディングを作成します。その結果、前述のスクリーンショットとなります。

カスタム クラスへのバインドの詳細については、「[ListView Data Sources](~/xamarin-forms/user-interface/listview/data-and-databinding.md)」 (ListView データ ソース) を参照してください。

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>ListView での項目の選択

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 内のセルをタッチするユーザーに対応するために、次のコード例のデモのとおり、[`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) イベントが処理される必要があります。

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 内に含まれた場合、[`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) メソッドを使用して組み込みの戻るナビゲーションで新しいページを開くことができます。 [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) イベントは、次のコード例のとおり、[`e.SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/) プロパティを使用してセルに関連付けられたオブジェクトにアクセスし、それを新しいページにバインドし、新しいページを `PushAsync` を使用して表示できます。

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

組み込みの戻るナビゲーションは、各プラットフォームで独自の方法により実装されています。 詳細については、「[ナビゲーション](#Navigation)」を参照してください。

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) のセレクションの詳細については、「[ListView Interactivity](~/xamarin-forms/user-interface/listview/interactivity.md)」 (ListView インタラクティビティ) を参照してください。

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>セルの外観のカスタマイズ

セルの外観は、[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) クラスをサブクラス化し、このクラスのタイプを [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) の [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) プロパティに設定してカスタマイズできます。

次のスクリーンショットのセルは、1 つの [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) と 2 つの [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) コントロールで構成されています。

 ![](introduction-to-xamarin-forms-images/image14.png "ListView カスタム セルの外観")

このカスタム レイアウトを作成するために、次のコード例のとおり、[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) クラスをサブクラス化する必要があります。

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

このコードは次のタスクを実行します。

-  これは、[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) コントロールを追加し、それを `Employee` オブジェクトの `ImageUri` プロパティにバインドします。 データ バインディングの詳細については、「[データ バインディング](#Data_Binding)」を参照してください。
-  これは、2 つの [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) コントロールを保持するために垂直方向の [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) を作成します。 `Label` コントロールは `Employee` オブジェクトの `DisplayName` と `Twitter` プロパティにバインドされています。
-  これは、既存の [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) と `StackLayout` をホストする [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) を作成します。 これは、その子を水平方向で並べます。

一度作成したカスタム セルは、次のコード例のとおり、[`DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) でラップして [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールで使用できます。

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

このコードは、`Employee` の `List` を [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) に提供します。 各セルは `EmployeeCell` クラスを使用してレンダリングされます。 `ListView` は `Employee` オブジェクトを `EmployeeCell` に、その [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) として渡します。

セルの外観のカスタマイズの詳細については、「[Cell Appearance](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)」 (セルの外観) を参照してください。

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>リストの作成およびカスタマイズのための XAML の使用

前のセクションの [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) を XAML で表した場合のコード例は次のとおりです。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

XAML は、[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) を含む [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) を定義します。 `ListView` のデータ ソースは、[`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 属性を使用して設定されます。 `ItemsSource` の各行のレイアウトは、[`ListView.ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 要素内で定義されます。

<a name="Data_Binding" />

## <a name="data-binding"></a>データ バインディング

データ バインディングでは、*ソース*と*ターゲット*と呼ばれる 2 つのオブジェクトを接続します。 *ソース* オブジェクトはデータを提供します。 *ターゲット* オブジェクトは、ソース オブジェクトのデータを使用し (またしばしば表示し) ます。 たとえば、[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (*ターゲット* オブジェクト) は一般的にその [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) プロパティを *ソース* オブジェクトのパブリックの `string` プロパティにバインドします。 次の図では、バインドの関係を示します。

![](introduction-to-xamarin-forms-images/data-binding.png "データ バインディング")

データ バインディングの主な利点は、ビューとデータ ソース間でデータを同期する心配がないことです。 *ソース* オブジェクトの変更は、バインディング フレームワークによって背後で自動的に*ターゲット* オブジェクトにプッシュされます。そして、ターゲット オブジェクトの変更は、オプションで*ソース* オブジェクトに戻されます。

データ バインディングを確立するには、2 つの手順を実行します。

- *ターゲット* オブジェクトの [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) プロパティは、*ソース*に設定する必要があります。
- バインディングは*ターゲット*と*ソース*間で確立する必要があります。 XAML でこれは、[`Binding`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) マークアップ拡張を使用して実現できます。 C# でこれは、[`SetBinding`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) メソッドを使用して実現できます。

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。

### <a name="xaml"></a>XAML

次のコードでは、XAML でデータ バインディングを実行する例を示します。

```xaml
<Entry Text="{Binding FirstName}" ... />
```

*ソース* オブジェクトの [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) プロパティと `FirstName` プロパティ間のバインディングが確立されました。 `Entry` コントロールでの変更は `employeeToDisplay` オブジェクトに自動的に伝達されます。 同様に、`employeeToDisplay.FirstName` プロパティが変更された場合、Xamarin.Forms のバインド エンジンは `Entry` コントロールのコンテンツも更新します。 これは、*両方向のバインド*とも呼ばれています。 両方向のバインドが機能するには、モデル クラスで `INotifyPropertyChanged` インターフェイスを実装する必要があります。

`EmployeeDetailPage` クラスの [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) プロパティは XAML に設定できますが、ここでは、`Employee` オブジェクトのインスタンスのコードビハインドで設定されます。

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

各*ターゲット* オブジェクトの [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) プロパティは個々に設定できますが、これは必ずしも行う必要はありません。 `BindingContext` は、その子がすべて継承する特殊なプロパティです。 したがって、[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) の `BindingContext` が `Employee` インスタンスに設定された場合、`ContentPage` のすべての子は同じ `BindingContext` を持ち、`Employee` オブジェクトのパブリック プロパティにバインドすることができます。

### <a name="c35"></a>C&#35;

次のコードでは、C# でのデータ バインディングの実行例を示します。

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) コンストラクターに `Employee` オブジェクトのインスタンスに渡され、バインドするオブジェクトに [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) が設定されます。 [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) コントロールがインスタンス化され、*ソース* オブジェクトの [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) プロパティと `FirstName` プロパティ間のバインディングが設定されます。 `Entry` コントロールでの変更は `employeeToDisplay` オブジェクトに自動的に伝達されます。 同様に、`employeeToDisplay.FirstName` プロパティが変更された場合、Xamarin.Forms のバインド エンジンは `Entry` コントロールのコンテンツも更新します。 これは、*両方向のバインド*とも呼ばれています。 両方向のバインドが機能するには、モデル クラスで `INotifyPropertyChanged` インターフェイスを実装する必要があります。

`SetBinding` は、次の 2 つのパラメーターを受け取ります。 1 番目のパラメーターはバインディングの種類の情報を指定します。 2 番目のパラメーターは、何をバインドするかまたどのようにバインドするかの情報を提供するために使用します。 多くの場合、2 番目のパラメーターは単に [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) プロパティの名前を保持している文字列です。 次の構文は、`BindingContext` を直接バインドするために使用されます。

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

ドット構文が Xamarin.Forms に `BindingContext` のプロパティの代わりに [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) をデータ ソースとして使用するよう指定します。 これは、`BindingContext` が `string` や `int` などの単純型の場合に便利です。

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>プロパティの変更通知

既定で、*ターゲット* オブジェクトは、バインドの作成時、*ソース* オブジェクトの値のみを受け取ります。 データ ソースと UI の同期を維持するには、*ソース* オブジェクトの変更時に *ターゲット* オブジェクトに通知する手段が必要です。 このメカニズムは、`INotifyPropertyChanged` インターフェイスが提供します。 このインターフェイスを実装すると、基になるプロパティ値が変更されたときに、すべてのデータ バインド コントロールに通知がされます。

`INotifyPropertyChanged` を実装するオブジェクトは、次のコード例のように、そのプロパティの 1 つが新しい値で更新されたときに、`PropertyChanged` イベントを発行する必要があります。

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

`MyObject.FirstName` プロパティが変更になった場合、`OnPropertyChanged` メソッドが起動され、それによって `PropertyChanged` イベントが発生します。 不要なイベントが起動されないようにするには、プロパティ値が変更にならないときに `PropertyChanged` イベントは起動されません。

なお、`OnPropertyChanged` メソッドでは、`propertyName` パラメーターは `CallerMemberName` 属性で修飾されます。 これにより、`null` 値で `OnPropertyChanged` メソッドが呼び出されると、`CallerMemberName` 属性が `OnPropertyChanged` を呼び出したメソッドの名前を提供します。

<a name="Navigation" />

## <a name="navigation"></a>ナビゲーション

Xamarin.Forms は、使用している [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 型に応じて多数のページ ナビゲーション エクスペリエンスを提供します。 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) のインスタンスには、2 つのナビゲーション エクスペリエンスがあります。

- [階層ナビゲーション](#Hierarchical_Navigation)
- [モーダル ナビゲーション](#Modal_Navigation)

[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) クラス、[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) クラスおよび [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) クラスは別のナビゲーション エクスペリエンスを提供します。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>階層ナビゲーション

[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。

階層ナビゲーションでは、[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) クラスは [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) オブジェクトのスタック間をナビゲートするために使用されます。 1 つのページから別のページに移動するには、アプリケーションは新しいページを、そこでアクティブなページとなるナビゲーション スタックにプッシュします。 前のページに戻るには、アプリケーションは現在のページをナビゲーション スタックからポップします。そして新しい最上位のページがアクティブ ページになります。

ナビゲーション スタックに追加された最初のページは、アプリケーションの*ルート* ページとなります。次のコード例に、これを実現する方法を示しています。

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

`LoginPage` にナビゲートするには、次のコード例のとおり、現在のページの [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) プロパティで [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) メソッドを起動する必要があります。

```csharp
await Navigation.PushAsync(new LoginPage());
```

これにより新しい `LoginPage` オブジェクトがナビゲーション スタックにプッシュされ、新しいページとなります。

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。 前のページにプログラムを使用して戻るには、次のコード例のとおり、`LoginPage` インスタンスで [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) メソッドを起動する必要があります。

```csharp
await Navigation.PopAsync();
```

階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>モーダル ナビゲーション

Xamarin.Forms はモーダル ページをサポートしています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。

モーダル ページは、Xamarin.Forms でサポートされる任意の [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 型にできます。 モーダル ページを表示するために、アプリケーションは、そこでアクティブなページになるナビゲーション スタックにページをプッシュします。 前のページに戻るには、アプリケーションは現在のページをナビゲーション スタックからポップし、新しい最上位のページがアクティブ ページになります。

モーダル ナビゲーション メソッドは、任意の [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 派生型の [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) プロパティによって公開されます。 [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) プロパティは、ナビゲーション スタックのモーダル ページを取得する [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) プロパティも公開します。 ただし、モーダル スタックの操作を実行したり、モーダル ナビゲーションで、ルート ページにポップしたりする概念はありません。 これは、これらの操作が基になるプラットフォームで一般にサポートされていないためです。

> [!NOTE]
> [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) インスタンスは、モーダル ページ ナビゲーションの実行には不要です。

`LoginPage` にモーダルを使用してナビゲートするには、次のコード例のとおり、現在のページの [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) プロパティで [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) メソッドを起動する必要があります。

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

これにより、`LoginPage` インスタンスがナビゲーション スタックにプッシュされるようになり、そこがアクティブ ページとなります。

アクティブ ページは、これが物理的なボタンであるか画面上のボタンであるかどうかにかかわらず、デバイスの *[戻る]* ボタンを押すことによってナビゲーション スタックからポップすることができます。 元のページにプログラムを使用して戻るには、`LoginPage` インスタンスが、次のコード例のとおり [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) メソッドを起動する必要があります。

```csharp
await Navigation.PopModalAsync();
```

これにより、ナビゲーション スタックから `LoginPage` インスタンスが削除され、新しい最上位のページがアクティブ ページとなります。

モーダル ナビゲーションの詳細については、「[Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md)」 (モーダル ページ) を参照してください。

<a name="Next_Steps" />

## <a name="next-steps"></a>次の手順

この概要の記事で、Xamarin.Forms アプリケーションを記述し始めることできるようになるはずです。 推奨される次の手順としては、次の機能の説明を読んでください。

- コントロール テンプレートは、実行時にアプリケーション ページを簡単にテーマ設定および再テーマ設定する機能を提供します。 詳細については、「[Control Templates](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)」 (コントロール テンプレート) を参照してください。
- データ テンプレートでは、サポートされているコントロールでデータを表示する機能を提供します。 詳細については、「[Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」 (データ テンプレート) を参照してください。
- 共有コードはネイティブ機能に [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) クラスを介してアクセスできます。 詳細については、「[Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)」 (DependencyService を使用したネイティブ機能へのアクセス) を参照してください。
- Xamarin.Forms にはメッセージを送受信するための単純なメッセージング サービスが含まれおり、これでクラス間のカップリングを削減できます。 詳細については、「[Publish and Subscribe with MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)」 (MessagingCenter を使用したパブリッシュおよびサブスクライブ) を参照してください。
- 各ページ、レイアウトおよびコントロールは各プラットフォームで `Renderer` クラスで異なってレンダリングされます。そして、ネイティブ コントロールが作成され、画面上で並べられ、共有コードで指定された動作が追加されます。 開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。
- 効果では、各プラットフォームのネイティブ コントロールのカスタマイズを可能にします。 効果は、プラットフォーム固有のプロジェクトで [`PlatformEffect`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/) コントロールをサブクラスかすることによって作成されます。そして、適切な Xamarin.Forms コントロールに添付することによって使用されます。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (効果) を参照してください。

または、Charles Petzold 著の「Creating Mobile Apps with Xamarin.Forms」 (Xamarin.Forms でモバイル アプリを作成する) でも Xamarin.Forms の詳細を学習できます。 詳細については、「[Creating Mobile Apps with Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)」 (Xamarin.Forms でモバイル アプリを作成する) を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms の概要と、これを使用したアプリケーションの記述方法について説明しています。 Xamarin.Forms は、開発者が Android、iOS、Windows、および Windows Phone 間で共有できるユーザー インターフェイスを簡単に作成できる、クロス プラットフォームのネイティブ サポートの、抽象型 UI ツールキットです。 ユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの外観を維持できます。


## <a name="related-links"></a>関連リンク

- [XAML の基礎](~/xamarin-forms/xaml/xaml-basics/index.md)
- [コントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
- [ユーザー インターフェイス](~/xamarin-forms/user-interface/index.md)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [入門サンプル](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [無料のセルフ ガイド学習 (ビデオ)](https://university.xamarin.com/self-guided)
- [Hello, Xamarin.Forms iOS ワークブック](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
