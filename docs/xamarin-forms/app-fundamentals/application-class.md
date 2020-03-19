---
title: Xamarin.Forms の App クラス
description: この記事では、既定の App クラスの機能について説明します。このクラスには、アプリの最初のページを設定されるプロパティと、ライフサイクルの状態変化をまたいで格納されるシンプルな値のための永続ディクショナリが含まれます。
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
ms.custom: video
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: aaf2086fd8128d68baa401ab646b31bcbc279545
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303769"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms の App クラス

`Application` 基底クラスでは、プロジェクトの既定の `App` サブクラスで公開される次の機能が提供されています。

* アプリの最初のページが設定される `MainPage` プロパティ。
* ライフサイクルの状態変化をまたいでシンプルな値を格納するための永続的な [`Properties` ディクショナリ](#Properties_Dictionary)。
* 現在のアプリケーション オブジェクトへの参照が含まれている静的な `Current` プロパティ。

また、`OnStart`、`OnSleep`、`OnResume` などの[ライフサイクル メソッド](~/xamarin-forms/app-fundamentals/app-lifecycle.md)と、モーダル ナビゲーション イベントも公開されています。

選択するテンプレートに応じて、2 つの方法のいずれかで `App` クラスを定義できます。

* **C#**
* **XAML と C#**

XAML を使用して **App** クラスを作成するには、次のコード例に示すように、既定の **App** クラスを、XAML の **App** クラスとそれに関連する分離コードに置き換える必要があります。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Photos.App">

</Application>
```

次のコード例では、関連する分離コードを示します。

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティの設定だけでなく、分離コードで `InitializeComponent` メソッドを呼び出して、関連付けられている XAML を読み込んで解析する必要があります。

## <a name="mainpage-property"></a>MainPage プロパティ

`Application` クラスの `MainPage` プロパティでは、アプリケーションのルート ページが設定されます。

たとえば、ユーザーがログインしているかログアウトしているかに応じて異なるページを表示するためのロジックを、`App` クラス内に作成することができます。

`MainPage` プロパティは、`App` のコンストラクターで設定する必要があります。

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Properties ディクショナリ

`Application` サブクラスには静的な `Properties` ディクショナリがあり、データの格納に使用できます。具体的には、`OnStart`、`OnSleep`、および `OnResume` メソッドで使用するためのデータです。 これには、`Application.Current.Properties` を使用して Xamarin.Forms コード内のどこからでもアクセスできます。

`Properties` ディクショナリでは、`string` キーが使用され、`object` 値が格納されます。

たとえば、次のように、永続的な `"id"` プロパティをコードの任意の場所で設定できます (項目が選択されるとき、ページの `OnDisappearing` メソッド内、または `OnSleep` メソッド内)。

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

その後、`OnStart` または `OnResume` メソッドで、この値を使用して何らかの方法でユーザーのエクスペリエンスを再作成できます。 `Properties` ディクショナリには `object` が格納されるので、使用する前にその値をキャストする必要があります。

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

予期しないエラーを防ぐため、常に、アクセスする前にキーの存在を確認します。

> [!NOTE]
> `Properties` ディクショナリでは、ストレージ用のプリミティブ型のみをシリアル化できます。 他の型 (`List<string>` など) を格納しようとすると、明示されずに失敗する可能性があります。

<!-- bugzilla 28657 -->

### <a name="persistence"></a>永続性

`Properties` ディクショナリは、デバイスに自動的に保存されます。
ディクショナリに追加されたデータは、アプリケーションがバックグラウンドから戻るときに、または再起動後でも利用できます。

Xamarin.Forms 1.4 では、`Application` クラスに追加メソッド `SavePropertiesAsync()` が導入されました。このメソッドを呼び出すと、`Properties` ディクショナリを事前に保存できます。 これにより、クラッシュまたは OS による強制終了のためにプロパティがシリアル化されないリスクなしに、重要な更新後にプロパティを保存できます。

`Properties` ディクショナリの使用に関する参考資料については、「**Creating Mobile Apps with Xamarin.Forms**」(Xamarin.Forms でモバイル アプリを作成する) の第 [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf)、[15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf)、[20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) 章、および関連する[サンプル](https://github.com/xamarin/xamarin-forms-book-preview-2)をご覧ください。

## <a name="the-application-class"></a>Application クラス

参考のため、完全な `Application` クラスの実装を次に示します。

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}
```

このクラスは、各プラットフォーム固有プロジェクトでインスタンス化されて、`LoadApplication` メソッドに渡されます。このメソッドでは、`MainPage` が読み込まれて、ユーザーに表示されます。
以下のセクションでは、各プラットフォームのコードを示します。 最新の Xamarin.Forms ソリューション テンプレートには、アプリ用に事前構成されたこのすべてのコードが既に含まれます。

### <a name="ios-project"></a>iOS プロジェクト

iOS の `AppDelegate` クラスは、`FormsApplicationDelegate` を継承します。 次のようになります。

* `App` クラスのインスタンスを使用して `LoadApplication` を呼び出します。

* 常に `base.FinishedLaunching (app, options);` を返します。

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Android プロジェクト

Android の `MainActivity` は、`FormsAppCompatActivity` を継承します。 `OnCreate` のオーバーライド内で、`App` クラスのインスタンスを使用して `LoadApplication` メソッドが呼び出されます。

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 用のユニバーサル Windows プロジェクト (UWP)

UWP プロジェクトのメイン ページでは、`WindowsPage` を継承する必要があります。

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# の分離コードの構築では、`LoadApplication` を呼び出して、Xamarin.Forms の `App` のインスタンスを作成する必要があります。 UWP アプリケーションにも Xamarin.Forms と関係のない独自の `App` クラスがあるため、アプリケーションの名前空間を使用して `App` を明示的に修飾するのがよい方法であることに注意してください。

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

UWP プロジェクトの **App.xaml.cs** から `Forms.Init()` を呼び出す必要があることに注意してください。

詳細については、「[Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md)」(Windows プロジェクトのセットアップ) を参照してください。UWP をターゲットにしていない既存の Xamarin.Forms ソリューションに UWP プロジェクトを追加する手順が説明されています。

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Series/Xamarin-101/Xamarin-Solution-Architecture-4-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
