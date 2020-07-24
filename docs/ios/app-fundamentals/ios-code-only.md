---
title: Xamarin のコードで iOS ユーザーインターフェイスを作成する
description: このドキュメントでは、コードを使用して Xamarin iOS アプリ用のユーザーインターフェイスを構築する方法について説明します。 ビューコントローラー、ビュー階層の構築、回転の処理などについて説明します。
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: edd49cc891a86d3323bab319ab811e85f9148640
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997099"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Xamarin のコードで iOS ユーザーインターフェイスを作成する

IOS アプリのユーザーインターフェイスは、通常は1つのウィンドウに似ていますが、アプリケーションは通常1つのウィンドウを取得しますが、ウィンドウに必要な数のオブジェクトを入力できます。また、オブジェクトや配置は、アプリの表示内容に応じて変更できます。 このシナリオのオブジェクト (ユーザーに表示される物事) はビューと呼ばれます。 アプリケーションで 1 つの画面をビルドするには、コンテンツ ビュー階層にビューが相互に積み重ねられ、階層が単一のビュー コントローラーによって管理されます。 複数の画面を持つアプリケーションには、複数のコンテンツ ビュー階層、それぞれに独自のビュー コントローラー、およびウィンドウ内のアプリケーションの場所のビューがあり、ユーザーに表示される画面に基づいて異なるコンテンツ ビュー階層を作成します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。

[![この図は、ウィンドウ、ビュー、サブビュー、ビューコントローラーの間の関係を示しています。](ios-code-only-images/image9.png)](ios-code-only-images/image9.png#lightbox)

これらのビュー階層は、Visual Studio の[Xamarin Designer for iOS](~/ios/user-interface/designer/index.md)を使用して作成できますが、完全にコードで作業する方法について理解しておくことをお勧めします。 この記事では、コードのみのユーザーインターフェイスの開発を開始して実行するためのいくつかの基本的なポイントについて説明します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。

[![この図は、ウィンドウ、ビュー、サブビュー、ビューコントローラーの間の関係を示しています。](ios-code-only-images/image9.png)](ios-code-only-images/image9.png#lightbox)

これらのビュー階層は、Visual Studio for Mac の[Xamarin Designer for iOS](~/ios/user-interface/designer/index.md)を使用して作成できますが、完全にコードで作業する方法についての基本的な理解が必要です。 この記事では、コードのみのユーザーインターフェイスの開発を開始して実行するためのいくつかの基本的なポイントについて説明します。

-----

## <a name="creating-a-code-only-project"></a>コードのみのプロジェクトの作成

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>iOS の空のプロジェクトテンプレート

まず、次に示すように、visual **C# > iPhone & iPad > IOS App (Xamarin) プロジェクト > ファイル > 新しいプロジェクト**を使用して、visual Studio で iOS プロジェクトを作成します。

[![[新しいプロジェクト] ダイアログ](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

次に、[**空のアプリケーション**] プロジェクトテンプレートを選択します。

[![[テンプレートの選択] ダイアログ](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

空のプロジェクトテンプレートは、次の4つのファイルをプロジェクトに追加します。

[![プロジェクトファイル](ios-code-only-images/empty-project.w157-sml.png "プロジェクト ファイル")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.cs** -サブクラスが含まれてい `UIApplicationDelegate` `AppDelegate` ます。これは、iOS からのアプリケーションイベントを処理するために使用されます。 アプリケーションウィンドウは、のメソッドで作成され `AppDelegate` `FinishedLaunching` ます。
1. **Main.cs** -アプリケーションのエントリポイントが含まれます。これは、のクラスを指定し `AppDelegate` ます。
1. **情報. plist** -アプリケーションの構成情報を含むプロパティリストファイル。
1. **権利. plist** –アプリケーションの機能およびアクセス許可に関する情報を含むプロパティリストファイル。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="ios-templates"></a>iOS テンプレート

Visual Studio for Mac は空のテンプレートを提供しません。 すべてのテンプレートには、ストーリーボードのサポートが付属しています。これは、UI を作成する主な方法として Apple が推奨しています。 ただし、コードで UI を完全に作成することもできます。

次の手順に従って、アプリケーションからストーリーボードを削除します。

1. 単一ビューアプリテンプレートを使用して、新しい iOS プロジェクトを作成します。

    [![単一ビューアプリテンプレートを使用する](ios-code-only-images/single-view-app.png)](ios-code-only-images/single-view-app.png#lightbox)

1. `Main.Storyboard`ファイルとファイルを削除し `ViewController.cs` ます。 を**削除しない**で `LaunchScreen.Storyboard` ください。 ビューコントローラーは、ストーリーボードに作成されたビューコントローラーの分離コードであるため、削除する必要があります。
1. ポップアップダイアログから [**削除**] を選択してください。

    [![ポップアップダイアログから [削除] を選択します。](ios-code-only-images/delete.png)](ios-code-only-images/delete.png#lightbox)

1. 情報 plist で、[**配置情報 > Main インターフェイス**] オプション内の情報を削除します。

    [![メインインターフェイスオプション内の情報を削除します。](ios-code-only-images/main-interface.png)](ios-code-only-images/main-interface.png#lightbox)

1. 最後に、AppDelegate クラスのメソッドに次のコードを追加し `FinishedLaunching` ます。

    ```csharp
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
    ```

上記の手順 5. でメソッドに追加されたコード `FinishedLaunching` は、iOS アプリケーション用のウィンドウを作成するために必要な最小限のコードの量です。

-----

iOS アプリケーションは、 [MVC パターン](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc)を使用して構築されます。 アプリケーションに表示される最初の画面は、ウィンドウのルートビューコントローラーから作成されます。 MVC パターン自体の詳細については、「 [Hello, IOS マルチスクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)ガイド」を参照してください。

`AppDelegate`テンプレートによって追加されたの実装によって、アプリケーションウィンドウが作成されます。このウィンドウには、iOS アプリケーションごとに1つだけが存在し、次のコードで表示されます。

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

このアプリケーションを今すぐ実行すると、そのことを示す例外がスローされる可能性があり `Application windows are expected to have a root view controller at the end of application launch` ます。 コントローラーを追加し、それをアプリのルートビューコントローラーにしましょう。

## <a name="adding-a-controller"></a>コントローラーを追加する

アプリには多くのビューコントローラーを含めることができますが、すべてのビューコントローラーを制御するには1つのルートビューコントローラーが必要です。  インスタンスを作成し、プロパティに設定して、コントローラーをウィンドウに追加 `UIViewController` し `Window.RootViewController` ます。

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

すべてのコントローラーには、プロパティからアクセスできるビューが関連付けられてい `View` ます。 上のコードでは、次のようにビューの `BackgroundColor` プロパティをに変更して `UIColor.LightGray` 、表示されるようにしています。

 [![ビューの背景が明るい灰色で表示されます。](ios-code-only-images/image1.png)](ios-code-only-images/image1.png#lightbox)

`UIViewController`この方法では、任意のサブクラスをとして設定することもでき `RootViewController` ます。これには、uikit だけでなく、自分で記述したコントローラーも含まれます。 たとえば、次のコードでは、をとして追加し `UINavigationController` てい `RootViewController` ます。

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

これにより、次に示すように、ナビゲーションコントローラー内に入れ子になったコントローラーが生成されます。

 [![ナビゲーションコントローラー内に入れ子になっているコントローラー](ios-code-only-images/image2.png)](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>ビューコントローラーの作成

これで、ウィンドウのとしてコントローラーを追加する方法がわかりました。次は、 `RootViewController` コードでカスタムビューコントローラーを作成する方法を見てみましょう。

次に示すように、という名前の新しいクラスを追加 `CustomViewController` します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![CustomViewController という名前の新しいクラスを追加します。](ios-code-only-images/customviewcontroller.w157-sml.png)](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![CustomViewController という名前の新しいクラスを追加します。](ios-code-only-images/new-file.png)](ios-code-only-images/new-file.png#lightbox)

-----

クラスは、 `UIViewController` 次に示すように、名前空間内のから継承する必要があり `UIKit` ます。

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

## <a name="initializing-the-view"></a>ビューの初期化

`UIViewController`には、 `ViewDidLoad` ビューコントローラーが最初にメモリに読み込まれるときに呼び出されるというメソッドが含まれています。 これは、ビューのプロパティの設定など、ビューの初期化を行うための適切な場所です。

たとえば、次のコードでは、ボタンが押されたときに新しいビューコントローラーをナビゲーションスタックにプッシュするためのボタンとイベントハンドラーを追加しています。

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

アプリケーションにこのコントローラーを読み込み、単純なナビゲーションのデモンストレーションを行うには、の新しいインスタンスを作成し `CustomViewController` ます。 新しいナビゲーションコントローラーを作成し、ビューコントローラーインスタンスを渡し、 `RootViewController` 前と同じように、新しいナビゲーションコントローラーをのウィンドウに設定し `AppDelegate` ます。

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

これで、アプリケーションが読み込まれると、 `CustomViewController` がナビゲーションコントローラー内に読み込まれます。

 [![CustomViewController がナビゲーションコントローラー内に読み込まれる](ios-code-only-images/customvc.png)](ios-code-only-images/customvc.png#lightbox)

このボタンをクリックすると、新しいビューコントローラーがナビゲーションスタックに_プッシュ_されます。

[![新しいビューコントローラーがナビゲーションスタックにプッシュされました](ios-code-only-images/customvca.png)](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>ビュー階層の構築

上の例では、ビューコントローラーにボタンを追加することによって、コードでユーザーインターフェイスを作成しました。

iOS ユーザーインターフェイスは、ビュー階層で構成されます。 ラベル、ボタン、スライダーなどの追加のビューは、いくつかの親ビューのサブビューとして追加されます。

たとえば、を編集して、 `CustomViewController` ユーザーがユーザー名とパスワードを入力できるログイン画面を作成してみましょう。 画面は、2つのテキストフィールドと1つのボタンで構成されます。

### <a name="adding-the-text-fields"></a>テキストフィールドの追加

最初に、「[ビューの初期化](#initializing-the-view)」セクションで追加したボタンとイベントハンドラーを削除します。

次に示すように、を作成して初期化 `UITextField` し、ビュー階層に追加することによって、ユーザー名のコントロールを追加します。

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

を作成するときに、プロパティを設定して `UITextField` `Frame` 場所とサイズを定義します。 IOS では、0は左上に + x が右、+ y 下にあります。 を `Frame` 他のいくつかのプロパティと共に設定した後、を呼び出して `View.AddSubview` 、を `UITextField` ビュー階層に追加します。 これにより、 `usernameField` プロパティが参照するインスタンスのサブビューが作成され `UIView` `View` ます。 サブビューが親ビューよりも大きい z オーダーで追加されるため、画面の親ビューの前に表示されます。

を含むアプリケーションは `UITextField` 次のようになります。

 [![UITextField を含むアプリケーション](ios-code-only-images/image4.png)](ios-code-only-images/image4.png#lightbox)

`UITextField`同様の方法でパスワードのを追加できます。次に示すように、この時点では、 `SecureTextEntry` プロパティを true に設定します。

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField);
      View.AddSubview(passwordField);
   }
}

```

`SecureTextEntry = true`次に示すように、を設定すると、ユーザーが入力したテキストが非表示に `UITextField` なります。

 [![SecureTextEntry true を設定すると、ユーザーが入力したテキストが非表示になります](ios-code-only-images/image4a.png)](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>ボタンの追加

次に、ユーザーがユーザー名とパスワードを送信できるように、ボタンを追加します。 ボタンは、他のコントロールと同様に、親ビューのメソッドに引数として渡すことによって、ビュー階層に追加され `AddSubview` ます。

次のコードでは、ボタンを追加し、イベントのイベントハンドラーを登録し `TouchUpInside` ます。

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

これを使用すると、次のようにログイン画面が表示されるようになります。

 [![ログイン画面](ios-code-only-images/image5.png)](ios-code-only-images/image5.png#lightbox)

以前のバージョンの iOS とは異なり、既定のボタンの背景は透明になっています。 ボタンのプロパティを変更すると、次のように変更され `BackgroundColor` ます。

```csharp
submitButton.BackgroundColor = UIColor.White;
```

これにより、標準的な丸い角のボタンではなく、正方形のボタンが表示されます。 角丸エッジを取得するには、次のスニペットを使用します。

```csharp
submitButton.Layer.CornerRadius = 5f;
```

これらの変更により、ビューは次のようになります。

[![ビューの実行例](ios-code-only-images/image6.png)](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>ビュー階層への複数のビューの追加

iOS には、を使用してビュー階層に複数のビューを追加する機能が用意されて `AddSubviews` います。

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton });
```

## <a name="adding-button-functionality"></a>ボタン機能の追加

ボタンがクリックされると、ユーザーが何かの処理を行うことが期待されます。 たとえば、アラートが表示されたり、別の画面に移動したりします。

2つ目のビューコントローラーをナビゲーションスタックにプッシュするコードを追加してみましょう。

最初に、2つ目のビューコントローラーを作成します。

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

次に、機能をイベントに追加し `TouchUpInside` ます。

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

ナビゲーションの例を次に示します。

[![このグラフでは、ナビゲーションを示しています。](ios-code-only-images/navigation.png)](ios-code-only-images/navigation.png#lightbox)

既定では、ナビゲーションコントローラーを使用すると、iOS によってアプリケーションにナビゲーションバーと [戻る] ボタンが表示され、スタック内を戻ることができます。

## <a name="iterating-through-the-view-hierarchy"></a>ビュー階層の反復処理

サブビュー階層を反復処理し、特定のビューを選択することができます。 たとえば、各を検索してそのボタンを別のものにするには、 `UIButton` `BackgroundColor` 次のスニペットを使用できます。

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

ただし、これは、の反復処理対象のビューがである場合には機能しません。これは、 `UIView` `UIView` 親ビュー自体に追加されたオブジェクトがを継承するため、すべてのビューがとして返されるためです `UIView` 。

## <a name="handling-rotation"></a>回転の処理

ユーザーがデバイスを横向きに回転させると、次のスクリーンショットに示すように、コントロールのサイズが適切に変更されません。

[![ユーザーがデバイスを横向きに回転させると、コントロールのサイズが適切に変更されません。](ios-code-only-images/image7.png)](ios-code-only-images/image7.png#lightbox)

この問題を解決する1つの方法は、 `AutoresizingMask` 各ビューでプロパティを設定することです。 ここでは、コントロールを水平方向に拡張し、それぞれを設定 `AutoresizingMask` します。 次の例は用です `usernameField` が、ビュー階層の各ガジェットにも同じことを適用する必要があります。

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

次に示すように、デバイスまたはシミュレーターを回転させると、すべてが追加の領域を占めるようになります。

[![すべてのコントロールが拡張され、追加の領域がいっぱいになります。](ios-code-only-images/image8.png)](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>カスタムビューの作成

UIKit の一部であるコントロールを使用するだけでなく、カスタムビューを使用することもできます。 を継承し、をオーバーライドすることによって、カスタムビューを作成でき `UIView` `Draw` ます。 ここでは、カスタムビューを作成し、それをビュー階層に追加して説明します。

### <a name="inheriting-from-uiview"></a>UIView からの継承

まず、カスタムビューのクラスを作成する必要があります。 これを行うには、Visual Studio の**クラス**テンプレートを使用して、という名前の空のクラスを追加し `CircleView` ます。 基底クラスをに設定する必要があり `UIView` ます。これは、 `UIKit` 名前空間にあります。 また、名前空間も必要になり `System.Drawing` ます。 この例では、他のさまざまな `System.*` 名前空間は使用されないため、削除してもかまいません。

クラスは次のようになります。

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>UIView での描画

すべてのには、 `UIView` `Draw` 描画する必要があるときにシステムによって呼び出されるメソッドがあります。 `Draw`を直接呼び出すことはできません。 これは、実行ループ処理中にシステムによって呼び出されます。 ビューがビュー階層に追加された後に run ループを初めて実行すると、その `Draw` メソッドが呼び出されます。 後続のの呼び出しは、ビュー `Draw` にまたはのいずれかを呼び出すことによって描画する必要があるとしてマークされている場合に発生し `SetNeedsDisplay` `SetNeedsDisplayInRect` ます。

次に示すように、オーバーライドされたメソッド内にそのようなコードを追加することで、ビューに描画コードを追加でき `Draw` ます。

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

はであるため `CircleView` `UIView` 、プロパティも設定でき `UIView` ます。 たとえば、コンストラクターでを設定でき `BackgroundColor` ます。

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

先ほど作成したを使用するには、以前と同じように、 `CircleView` 既存のコントローラーのビュー階層にサブビューとして追加するか、 `UILabels` `UIButton` 新しいコントローラーのビューとして読み込むことができます。 後者について考えてみましょう。

### <a name="loading-a-view"></a>ビューの読み込み

 `UIViewController`には、という名前のメソッドがあり `LoadView` ます。このメソッドは、そのビューを作成するためにコントローラーによって呼び出されます。 これは、ビューを作成し、コントローラーのプロパティに割り当てるための適切な場所です `View` 。

まず、コントローラーが必要であるため、という名前の新しい空のクラスを作成 `CircleController` します。

で `CircleController` 、をに設定する次のコードを追加し `View` `CircleView` ます (オーバーライドで実装を呼び出すことはできません `base` )。

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

最後に、実行時にコントローラーを提示する必要があります。 これを行うには、前に追加した [送信] ボタンに次のようにイベントハンドラーを追加します。

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

これで、アプリケーションを実行して [送信] ボタンをタップすると、円付きの新しいビューが表示されます。

[![円付きの新しいビューが表示されます。](ios-code-only-images/circles.png)](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>起動画面の作成

[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)は、アプリが応答していることをユーザーに表示する手段として起動したときに表示されます。 起動画面はアプリの読み込み時に表示されるため、アプリケーションがまだメモリに読み込まれているため、コードで作成することはできません。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio で iOS プロジェクトを作成すると、xib ファイルの形式で起動画面が表示されます。これは、プロジェクト内の**Resources**フォルダーにあります。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で iOS プロジェクトを作成すると、ストーリーボードファイルの形式で起動画面が表示されます。

-----

これを編集するには、ダブルクリックして iOS Designer で開くことができます。

Apple では、iOS 8 以降を対象とするアプリケーションに対して xib ファイルまたはストーリーボードファイルを使用することをお勧めします。 iOS デザイナーでいずれかのファイルを起動するときに、サイズクラスと自動レイアウトを使用してレイアウトを調整し、適切に表示され、すべてのデバイスサイズに正しく表示されるようにします。 Xib またはストーリーボードに加えて、静的な起動イメージを使用して、以前のバージョンを対象とするアプリケーションをサポートできるようにすることができます。

起動画面の作成の詳細については、次のドキュメントを参照してください。

- [Xib を使用して起動画面を作成する](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [ストーリーボードを使用した起動画面の管理](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> IOS 9 では、起動画面を作成する主な方法としてストーリーボードを使用することをお勧めします。

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>IOS 8 より前のアプリケーションの起動イメージの作成

アプリケーションが iOS 8 より前のバージョンを対象としている場合は、xib またはストーリーボードの起動画面に加えて、静的イメージを使用できます。

この静的イメージは、アプリケーションで、情報 plist ファイルで設定することも、アセットカタログ (iOS 7 の場合) として設定することもできます。 アプリケーションが実行される可能性のあるデバイスのサイズ (320x480、640x960、640x1136) ごとに個別のイメージを提供する必要があります。 起動画面のサイズの詳細については、[起動画面イメージ](~/ios/app-fundamentals/images-icons/launch-screens.md)ガイドを参照してください。

> [!IMPORTANT]
> アプリに起動画面が表示されない場合は、画面に完全に収まらないことがわかります。 この場合は、少なくとも、という名前の640x1136 イメージを `Default-568@2x.png` 情報 plist に含めてください。

## <a name="summary"></a>まとめ

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

この記事では、Visual Studio でプログラムによって iOS アプリケーションを開発する方法について説明しました。 ここでは、空のプロジェクトテンプレートからプロジェクトを構築する方法について説明し、ルートビューコントローラーを作成してウィンドウに追加する方法について説明しました。 次に、UIKit からコントロールを使用して、アプリケーション画面を開発するコントローラー内にビュー階層を作成する方法を説明しました。 次に、さまざまな方向にビューを適切に配置する方法と、サブクラス化することによってカスタムビューを作成する方法、およびコントローラー内にビューを読み込む方法について説明しました `UIView` 。 最後に、起動画面をアプリケーションに追加する方法について説明します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

この記事では、Visual Studio for Mac でプログラムで iOS アプリケーションを開発する方法について説明しました。 ここでは、1つのビューテンプレートからプロジェクトをビルドする方法を説明し、ルートビューコントローラーを作成してウィンドウに追加する方法について説明しました。 次に、UIKit からコントロールを使用して、アプリケーション画面を開発するコントローラー内にビュー階層を作成する方法を説明しました。 次に、さまざまな方向にビューを適切に配置する方法と、サブクラス化することによってカスタムビューを作成する方法、およびコントローラー内にビューを読み込む方法について説明しました `UIView` 。 最後に、起動画面をアプリケーションに追加する方法について説明します。

-----

## <a name="related-links"></a>関連リンク

- [SimpleLogin (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplelogin)
