---
title: "App クラス"
description: "C# コードか XAML のいずれかを指定できる既定 App クラスの機能"
ms.topic: article
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: d7965c5d4d65dd6bf7aa4128f467acd3e2d39e60
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="app-class"></a>App クラス

`Application`基底クラスが、既定のプロジェクトに公開されている次の機能を提供`App`サブクラス。

* A`MainPage`プロパティとは、アプリの初期ページを設定する場所です。
* 永続的な[`Properties`ディクショナリ](#Properties_Dictionary)ライフ サイクルの状態の変更の間にわたって単純な値を格納します。
* 静的な`Current`を現在のアプリケーション オブジェクトへの参照を格納するプロパティです。

場合も公開[ライフ サイクル メソッド](~/xamarin-forms/app-fundamentals/app-lifecycle.md)など`OnStart`、 `OnSleep`、および`OnResume`モーダル ナビゲーション イベントとします。

選択したテンプレートによって、`App`で 2 つの方法のいずれかのクラスを定義する可能性があります。

* **C#**、または
* **XAML と C# の場合**

作成する、**アプリ**クラスの既定の XAML を使用して**アプリ**クラスを XAML で置き換える必要があります**アプリ**クラスと関連付けられている分離コード、コード例を次に示すようにします。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

次のコード例は、関連付けられている分離コードを示しています。

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

設定だけでなく、 [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/)プロパティ、分離コードを呼び出す必要がありますも、`InitializeComponent`を読み込みし、関連する XAML を解析します。

## <a name="mainpage-property"></a>MainPage プロパティ

`MainPage`プロパティを`Application`クラスは、アプリケーションのルート ページを設定します。

ロジックを作成するなど、`App`ユーザーがログインしているかどうかどうかに応じて異なるページを表示するクラス。

`MainPage`プロパティを設定する必要があります、`App`コンス トラクター

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

`Application`サブクラスには、静的な`Properties`辞書で使用するための具体的には、データの格納に使用できる、 `OnStart`、 `OnSleep`、および`OnResume`メソッドです。 この、Xamarin.Forms を使用してコードで任意の場所からアクセスできる`Application.Current.Properties`です。

`Properties`ディクショナリを使用して、`string`キーとストア、`object`値。

固定を設定するなど、 `"id"` 、コード内の任意の場所のプロパティ (項目を選択すると、ページの `OnDisappearing`メソッド、または、`OnSleep`メソッド) 次のように。

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

`OnStart`または`OnResume`メソッドをいくつかの方法で、ユーザーのエクスペリエンスを再作成し、この値を使用できます。 `Properties`ディクショナリ ストア`object`s ために使用する前にその値をキャストする必要があります。

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

常に予期しないエラーを防ぐためにアクセスする前に、キーの有無を確認します。

> [!NOTE]
> **注:** 、`Properties`ディクショナリには、ストレージのプリミティブ型がシリアル化できるだけです。 その他の種類を保存しようとしています (など`List<string>`) サイレント モードで失敗することができます。

<!-- bugzilla 28657 -->

### <a name="persistence"></a>永続性

`Properties`ディクショナリがデバイスに自動的に保存されます。
アプリケーションがバック グラウンドから戻るときに、または再起動された後もに、ディクショナリに追加されたデータは使用可能になります。

Xamarin.Forms 1.4 で追加のメソッドを導入された、`Application`クラス - `SavePropertiesAsync()` -事前対応的に保持すると呼ばれることができます、`Properties`ディクショナリ。 これは、リスクを取得するシリアル化されませんアウト クラッシュまたは OS によって強制終了中のためにするのではなく、重要な更新プログラムの後にプロパティを保存できるようにします。

使用してへの参照を見つけることができます、`Properties`ディクショナリで、 **Xamarin.Forms を使用したモバイル アプリを作成する**チャプターを予約[6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf)、 [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf)、および[20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)、および関連付けられた[サンプル](https://github.com/xamarin/xamarin-forms-book-preview-2)です。



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

このクラスは各プラットフォームに固有のプロジェクトでインスタンス化しに渡される、`LoadApplication`メソッドが、`MainPage`は読み込まれ、ユーザーに表示します。
各プラットフォームのコードは、次のセクションに表示されます。 最新の Xamarin.Forms ソリューション テンプレートには、この構成済みアプリのすべてのコードが含まれています。


### <a name="ios-project"></a>iOS プロジェクト

IOS`AppDelegate`からクラスを今すぐ継承`FormsApplicationDelegate`です。 これを行ってください。

* 呼び出す`LoadApplication`のインスタンスと、`App`クラスです。

* 常に返す`base.FinishedLaunching (app, options);`です。

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

### <a name="android-project"></a>Android Project

Android`MainActivity`を今すぐ継承`FormsApplicationActivity`です。 `OnCreate`オーバーライド、`LoadApplication`のインスタンス メソッドが呼び出された、`App`クラスです。

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> **注:**より新しいがある[ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md)基本 Android マテリアル デザインのサポートを向上させるために使用するクラス。
> これは、既定の Android テンプレートを将来的になりますが、行うことができる[手順](~/xamarin-forms/platform/android/appcompat.md)を既存の Android アプリを更新します。


### <a name="windows-phone-project"></a>Windows Phone プロジェクト

Windows Phone (Silverlight ベースの) プロジェクトのメイン ページを継承する必要があります`FormsApplicationPage`です。 つまり、XAML および c# の`MainPage`参照、`FormsApplicationPage`クラスに示すようにします。

XAML ルート要素が反映されるように、カスタム名前空間を使用して、`FormsApplicationPage`クラス。

```xaml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

C# からの継承、`FormsApplicationPage`クラス、および呼び出し`LoadApplication`、Xamarin.Forms のインスタンスを作成する`App`です。 明示的に修飾するために、アプリケーションの名前空間を使用することをお勧めしてあることに注意してください、 `App` Windows Phone アプリケーションもあるため、独自`App`Xamarin.Forms に関係のないクラスです。

```csharp
public partial class MainPage :
    global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3, use the correct namespace
    }
 }
```

### <a name="windows-81-project"></a>Windows 8.1 Project

メイン ページに[(WinRT ベース)、Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md)プロジェクトから継承する必要があります`WindowsPage`です。 つまりの XAML`MainPage`参照、`WindowsPage`クラスに示すようにします。

XAML ルート要素が反映されるように、カスタム名前空間を使用して、`FormsApplicationPage`クラス。

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
   ...>
</forms:WindowsPage>
```

C# 分離コードの構築を呼び出す必要があります`LoadApplication`、Xamarin.Forms のインスタンスを作成する`App`です。 明示的に修飾するために、アプリケーションの名前空間を使用することをお勧めしてあることに注意してください、 `App` UWP アプリケーションもあるため、独自`App`Xamarin.Forms に関係のないクラスです。

```csharp
public partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();
        LoadApplication(new YOUR_APP_NAMESPACE.App());
    }
 }
```

なお`Forms.Init()`呼び出す必要があります**App.xaml.cs** 65 行目付近です。

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 用のユニバーサル Windows プロジェクト (UWP)

[ユニバーサル Windows Project](~/xamarin-forms/platform/windows/installation/universal.md) Xamarin.Forms でのサポートは現在プレビュー中です。

UWP プロジェクトのメイン ページを継承する必要があります`WindowsPage`です。 つまり、XAML および c# の`MainPage`参照、`FormsApplicationPage`クラスに示すようにします。

XAML ルート要素が反映されるように、カスタム名前空間を使用して、`FormsApplicationPage`クラス。

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# 分離コードの構築を呼び出す必要があります`LoadApplication`、Xamarin.Forms のインスタンスを作成する`App`です。 明示的に修飾するために、アプリケーションの名前空間を使用することをお勧めしてあることに注意してください、 `App` UWP アプリケーションもあるため、独自`App`Xamarin.Forms に関係のないクラスです。

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

なお`Forms.Init()`呼び出す必要があります**App.xaml.cs** 'system.ftpserver 63 です。
