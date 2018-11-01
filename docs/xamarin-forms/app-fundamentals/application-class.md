---
title: Xamarin.Forms アプリ クラス
description: この記事では、アプリの最初のページを設定するプロパティが含まれている既定のアプリのクラスの機能を説明しの永続的なディクショナリは、状態変更のライフ サイクル全体での単純な値を格納します。
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 9acd1b8f25696267578f5cc269eb1b0c738be571
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675095"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms アプリ クラス

`Application`基底クラスは、既定のプロジェクトに公開されている次の機能を提供`App`サブクラスです。

* A`MainPage`プロパティで、アプリの最初のページを設定する場所です。
* 永続的な[`Properties`ディクショナリ](#Properties_Dictionary)ライフ サイクルの状態の変更の間での単純な値を格納します。
* 静的な`Current`プロパティを現在のアプリケーション オブジェクトへの参照が含まれています。

また、公開[ライフ サイクル メソッド](~/xamarin-forms/app-fundamentals/app-lifecycle.md)など`OnStart`、`OnSleep`と`OnResume`モーダル ナビゲーション イベントと。

選択したテンプレートによっては、`App`クラスは、2 つの方法のいずれかで定義できます。

* **C#**、または
* **XAML と C# のコード**

作成する、**アプリ**クラスの XAML では、既定値を使用して**アプリ**クラスを XAML で置き換える必要があります**アプリ**クラスと関連付けられている分離コードを次のコード例に示すようにします。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

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

設定と、 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage)プロパティ、分離コードを呼び出す必要がありますも、`InitializeComponent`読み込み、関連付けられている XAML を解析するメソッド。

## <a name="mainpage-property"></a>MainPage プロパティ

`MainPage`プロパティを`Application`クラスは、アプリケーションのルート ページを設定します。

内のロジックを作成するなど、`App`かどうかにでユーザーがログインしているかどうかに応じてさまざまなページを表示するクラス。

`MainPage`でプロパティを設定する必要があります、`App`コンス トラクター

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

## <a name="properties-dictionary"></a>プロパティ ディクショナリ

`Application`サブクラスには、静的な`Properties`ディクショナリで使用するため、特にデータの格納に使用できる、 `OnStart`、 `OnSleep`、および`OnResume`メソッド。 これは、Xamarin.Forms を使用してコードの任意の場所からアクセスできる`Application.Current.Properties`します。

`Properties`ディクショナリを使用して、`string`キーとストア、`object`値。

永続的な設定など`"id"`、コードの任意の場所でプロパティ (ページの項目を選択すると`OnDisappearing`メソッド、または、`OnSleep`メソッド) このような。

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

`OnStart`または`OnResume`メソッドをいくつかの方法でユーザーのエクスペリエンスを再作成し、この値を使用できます。 `Properties`ディクショナリ ストア`object`s ために使用する前にその値をキャストする必要があります。

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

常に予期しないエラーを防ぐために、アクセスする前に、キーの存在を確認します。

> [!NOTE]
> `Properties`ディクショナリは、ストレージのプリミティブ型のみをシリアル化します。 その他の種類を格納しようとしています (など`List<string>`) サイレント モードで失敗することができます。

<!-- bugzilla 28657 -->

### <a name="persistence"></a>永続性

`Properties`ディクショナリは、デバイスに自動的に保存します。
ディクショナリに追加されたデータは、アプリケーションがバック グラウンドから戻るときに、または再起動後も提供されます。

Xamarin.Forms 1.4 で追加のメソッドを導入された、`Application`クラス - `SavePropertiesAsync()` -事前に永続化を呼び出すことができ、`Properties`ディクショナリ。 これは、リスクのないシリアル化をクラッシュまたは OS によって強制終了中のためによりも重要な更新プログラムの後にプロパティを保存するようにします。

使用してへの参照を見つけることができます、`Properties`ディクショナリで、**を Xamarin.Forms での Mobile Apps の作成**書籍の章[6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf)、 [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf)、および[20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)、および関連付けられている[サンプル](https://github.com/xamarin/xamarin-forms-book-preview-2)します。



## <a name="the-application-class"></a>Application クラス

完全な`Application`参照クラスの実装を次に示します。

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

このクラスは、各プラットフォーム固有プロジェクトでインスタンス化されに渡される、`LoadApplication`されているメソッド、`MainPage`が読み込まれ、ユーザーに表示されます。
次のセクションでは、各プラットフォームのコードに表示されます。 最新の Xamarin.Forms ソリューション テンプレートには、このアプリの構成済みのすべてのコードが含まれています。


### <a name="ios-project"></a>iOS プロジェクト

IOS`AppDelegate`クラスから継承`FormsApplicationDelegate`します。 必要があります。

* 呼び出す`LoadApplication`のインスタンスと、`App`クラス。

* 常に返す`base.FinishedLaunching (app, options);`します。

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

Android`MainActivity`継承`FormsAppCompatActivity`します。 `OnCreate`オーバーライド、`LoadApplication`のインスタンス メソッドを呼び出すと、`App`クラス。

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

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 のユニバーサル Windows プロジェクト (UWP)

参照してください[セットアップの Windows プロジェクト](~/xamarin-forms/platform/windows/installation/index.md)Xamarin.Forms での UWP サポートについてはします。

UWP プロジェクトのメイン ページを継承する必要があります`WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C#構築の背後にあるコードを呼び出す必要があります`LoadApplication`、Xamarin.Forms のインスタンスを作成する`App`します。 明示的に修飾するために、アプリケーション名前空間を使用することをお勧めです、 `App` UWP アプリケーションもあるため、独自`App`Xamarin.Forms と無関係なクラスです。

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

なお`Forms.Init()`呼び出す必要があります**App.xaml.cs** 63 行目にします。
