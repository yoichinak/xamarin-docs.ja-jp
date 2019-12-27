---
title: Xamarin のコードで iOS ユーザーインターフェイスを作成する
description: このドキュメントでは、コードを使用して Xamarin iOS アプリ用のユーザーインターフェイスを構築する方法について説明します。 ビューコントローラー、ビュー階層の構築、回転の処理などについて説明します。
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: 42a2694239fdd55efa91b7fa30be8a10cafb4cc5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010324"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Xamarin のコードで iOS ユーザーインターフェイスを作成する

iOS アプリのユーザーインターフェイスは、通常は1つのウィンドウに似ていますが、アプリケーションは通常1つのウィンドウを取得しますが、ウィンドウに必要な数のオブジェクトを入力できます。また、オブジェクトや配置は、アプリの表示内容に応じて変更できます。 このシナリオのオブジェクト (ユーザーに表示される物事) はビューと呼ばれます。 アプリケーションで1つの画面を作成するには、コンテンツビュー階層内でビューが相互に積み重ねられ、階層が1つのビューコントローラーによって管理されます。 複数の画面を持つアプリケーションには、複数のコンテンツ ビュー階層、それぞれに独自のビュー コントローラー、およびウィンドウ内のアプリケーションの場所のビューがあり、ユーザーに表示される画面に基づいて異なるコンテンツ ビュー階層を作成します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。

[![](ios-code-only-images/image9.png "This diagram illustrates the relationships between the Window, Views, Subviews, and View Controller")](ios-code-only-images/image9.png#lightbox)

これらのビュー階層は、Visual Studio の[Xamarin Designer for iOS](~/ios/user-interface/designer/index.md)を使用して作成できますが、完全にコードで作業する方法について理解しておくことをお勧めします。 この記事では、コードのみのユーザーインターフェイスの開発を開始して実行するためのいくつかの基本的なポイントについて説明します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。

[![](ios-code-only-images/image9.png "This diagram illustrates the relationships between the Window, Views, Subviews, and View Controller")](ios-code-only-images/image9.png#lightbox)

これらのビュー階層は、Visual Studio for Mac の[Xamarin Designer for iOS](~/ios/user-interface/designer/index.md)を使用して作成できますが、完全にコードで作業する方法についての基本的な理解が必要です。 この記事では、コードのみのユーザーインターフェイスの開発を開始して実行するためのいくつかの基本的なポイントについて説明します。

-----

## <a name="creating-a-code-only-project"></a>コードのみのプロジェクトの作成

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>iOS の空のプロジェクトテンプレート

まず、次に示すように、visual Studio で、 **visual C# > iPhone & IPad > IOS App (Xamarin) プロジェクト > ファイル > 新しいプロジェクト**を使用して、ios プロジェクトを作成します。

[[新しいプロジェクトの![] ダイアログ](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

次に、 **[空のアプリケーション]** プロジェクトテンプレートを選択します。

[[テンプレートの選択] ダイアログボックス![](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

空のプロジェクトテンプレートは、次の4つのファイルをプロジェクトに追加します。

[![プロジェクトファイル](ios-code-only-images/empty-project.w157-sml.png "プロジェクト ファイル")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.cs** -iOS からのアプリケーションイベントを処理するために使用される `UIApplicationDelegate` サブクラス、`AppDelegate` が含まれています。 アプリケーションウィンドウは、`AppDelegate`の `FinishedLaunching` メソッドで作成されます。
1. **Main.cs** -アプリケーションのエントリポイントが含まれており、`AppDelegate` のクラスを指定します。
1. **Info.plist** -アプリケーションの構成情報を含むプロパティリストファイル。
1. **Entitlements.plist** –アプリケーションの機能およびアクセス許可に関する情報を含むプロパティリストファイル。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="ios-templates"></a>iOS テンプレート

Visual Studio for Mac は空のテンプレートを提供しません。 すべてのテンプレートには、ストーリーボードのサポートが付属しています。これは、UI を作成する主な方法として Apple が推奨しています。 ただし、コードで UI を完全に作成することもできます。

次の手順に従って、アプリケーションからストーリーボードを削除します。

1. 単一ビューアプリテンプレートを使用して、新しい iOS プロジェクトを作成します。

    [![](ios-code-only-images/single-view-app.png "Use the Single View App template")](ios-code-only-images/single-view-app.png#lightbox)

1. `Main.Storyboard` と `ViewController.cs` ファイルを削除します。 `LaunchScreen.Storyboard`**は削除しないでください。** ビューコントローラーは、ストーリーボードに作成されたビューコントローラーの分離コードであるため、削除する必要があります。
1. ポップアップダイアログから **[削除]** を選択してください。

    [![](ios-code-only-images/delete.png "Select Delete from the pop-up dialog")](ios-code-only-images/delete.png#lightbox)

1. 情報 plist で、 **[配置情報 > Main インターフェイス]** オプション内の情報を削除します。

    [![](ios-code-only-images/main-interface.png "Delete the information inside the Main Interface option")](ios-code-only-images/main-interface.png#lightbox)

1. 最後に、AppDelegate クラスの `FinishedLaunching` メソッドに次のコードを追加します。

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

上記の手順 5. で `FinishedLaunching` メソッドに追加されたコードは、iOS アプリケーション用のウィンドウを作成するために必要な最小限のコード量です。

-----

iOS アプリケーションは、 [MVC パターン](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc)を使用して構築されます。 アプリケーションに表示される最初の画面は、ウィンドウのルートビューコントローラーから作成されます。 MVC パターン自体の詳細については、「 [Hello, iOS マルチスクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)ガイド」を参照してください。

テンプレートによって追加された `AppDelegate` の実装により、アプリケーションウィンドウが作成されます。このウィンドウには、iOS アプリケーションごとに1つだけ存在し、次のコードを使用して表示できます。

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

このアプリケーションを今すぐ実行すると、`Application windows are expected to have a root view controller at the end of application launch`を示す例外がスローされる可能性があります。 コントローラーを追加し、それをアプリのルートビューコントローラーにしましょう。

## <a name="adding-a-controller"></a>コントローラーを追加する

アプリには多くのビューコントローラーを含めることができますが、すべてのビューコントローラーを制御するには1つのルートビューコントローラーが必要です。  `UIViewController` インスタンスを作成し、それを `Window.RootViewController` プロパティに設定して、ウィンドウにコントローラーを追加します。

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

すべてのコントローラーには関連付けられたビューがあり、これは `View` プロパティからアクセスできます。 上のコードでは、次に示すように、ビューの `BackgroundColor` プロパティを `UIColor.LightGray` に変更して、表示されるようにしています。

 [![](ios-code-only-images/image1.png "The View's background is a visible light gray")](ios-code-only-images/image1.png#lightbox)

この方法では、`UIViewController` サブクラスを `RootViewController` として設定することもできます。これには、UIKit だけでなく、自分で記述したコントローラーも含まれます。 たとえば、次のコードでは、`RootViewController`として `UINavigationController` を追加しています。

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

 [![](ios-code-only-images/image2.png "The controller nested within the navigation controller")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>ビューコントローラーの作成

これで、ウィンドウの `RootViewController` としてコントローラーを追加する方法を説明しました。次は、コードでカスタムビューコントローラーを作成する方法を見てみましょう。

次に示すように、`CustomViewController` という名前の新しいクラスを追加します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Add a new class named CustomViewController")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](ios-code-only-images/new-file.png "Add a new class named CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

クラスは、次に示すように、`UIKit` 名前空間にある `UIViewController`から継承する必要があります。

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

`UIViewController` には、ビューコントローラーが最初にメモリに読み込まれるときに呼び出される `ViewDidLoad` と呼ばれるメソッドが含まれています。 これは、ビューのプロパティの設定など、ビューの初期化を行うための適切な場所です。

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

このコントローラーをアプリケーションに読み込み、単純なナビゲーションをデモンストレーションするには、`CustomViewController`の新しいインスタンスを作成します。 新しいナビゲーションコントローラーを作成し、ビューコントローラーインスタンスを渡し、前と同じように `AppDelegate` のウィンドウの `RootViewController` に新しいナビゲーションコントローラーを設定します。

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

これで、アプリケーションが読み込まれると、`CustomViewController` がナビゲーションコントローラー内に読み込まれます。

 [![](ios-code-only-images/customvc.png "The CustomViewController is loaded inside a navigation controller")](ios-code-only-images/customvc.png#lightbox)

このボタンをクリックすると、新しいビューコントローラーがナビゲーションスタックに_プッシュ_されます。

[![](ios-code-only-images/customvca.png "A new View Controller pushed onto the navigation stack")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>ビュー階層の構築

上の例では、ビューコントローラーにボタンを追加することによって、コードでユーザーインターフェイスを作成しました。

iOS ユーザーインターフェイスは、ビュー階層で構成されます。 ラベル、ボタン、スライダーなどの追加のビューは、いくつかの親ビューのサブビューとして追加されます。

たとえば、`CustomViewController` を編集して、ユーザーがユーザー名とパスワードを入力できるログイン画面を作成してみましょう。 画面は、2つのテキストフィールドと1つのボタンで構成されます。

### <a name="adding-the-text-fields"></a>テキストフィールドの追加

最初に、「[ビューの初期化](#initializing-the-view)」セクションで追加したボタンとイベントハンドラーを削除します。 

次に示すように、`UITextField` を作成して初期化し、ビュー階層に追加することによって、ユーザー名のコントロールを追加します。

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

`UITextField`を作成するときに、`Frame` プロパティを設定して、その場所とサイズを定義します。 iOS では、0は左上に + x が右、+ y 下にあります。 `Frame` を他のいくつかのプロパティと共に設定した後、`View.AddSubview` を呼び出して、ビュー階層に `UITextField` を追加します。 これにより、`usernameField`、`View` プロパティが参照する `UIView` インスタンスのサブビューになります。 サブビューが親ビューよりも大きい z オーダーで追加されるため、画面の親ビューの前に表示されます。

`UITextField` が含まれているアプリケーションは次のようになります。

 [![](ios-code-only-images/image4.png "The application with the UITextField included")](ios-code-only-images/image4.png#lightbox)

同様の方法でパスワードの `UITextField` を追加できます。次のように `SecureTextEntry` プロパティを true に設定します。

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

`SecureTextEntry = true` 設定すると、次に示すように、ユーザーが `UITextField` に入力したテキストが非表示になります。

 [![](ios-code-only-images/image4a.png "Setting SecureTextEntry true hides the text entered by the user")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>ボタンの追加

次に、ユーザーがユーザー名とパスワードを送信できるように、ボタンを追加します。 ボタンは、他のコントロールと同様に、親ビューの `AddSubview` メソッドに引数として渡すことによって、ビュー階層に追加されます。

次のコードでは、ボタンを追加し、`TouchUpInside` イベントのイベントハンドラーを登録します。

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

 [![](ios-code-only-images/image5.png "The login screen")](ios-code-only-images/image5.png#lightbox)

以前のバージョンの iOS とは異なり、既定のボタンの背景は透明になっています。 ボタンの `BackgroundColor` プロパティを変更すると、次のように変更されます。

```csharp
submitButton.BackgroundColor = UIColor.White;
```

これにより、標準的な丸い角のボタンではなく、正方形のボタンが表示されます。 角丸エッジを取得するには、次のスニペットを使用します。

```csharp
submitButton.Layer.CornerRadius = 5f;
```

これらの変更により、ビューは次のようになります。

[![](ios-code-only-images/image6.png "An example run of the view")](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>ビュー階層への複数のビューの追加

iOS には、`AddSubviews`を使用してビュー階層に複数のビューを追加する機能が用意されています。

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

次に、機能を `TouchUpInside` イベントに追加します。

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

ナビゲーションの例を次に示します。

[![](ios-code-only-images/navigation.png "The navigation is illustrated in this chart")](ios-code-only-images/navigation.png#lightbox)

既定では、ナビゲーションコントローラーを使用すると、iOS によってアプリケーションにナビゲーションバーと [戻る] ボタンが表示され、スタック内を戻ることができます。

## <a name="iterating-through-the-view-hierarchy"></a>ビュー階層の反復処理

サブビュー階層を反復処理し、特定のビューを選択することができます。 たとえば、各 `UIButton` を検索し、そのボタンに別の `BackgroundColor`を指定するには、次のスニペットを使用できます。

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

ただし、反復されるビューが  `UIView` の場合、親ビュー自体に追加されたオブジェクトが `UIView` を継承するので、すべてのビューが `UIView` として返されるため、これは機能しません。

## <a name="handling-rotation"></a>回転の処理

ユーザーがデバイスを横向きに回転させると、次のスクリーンショットに示すように、コントロールのサイズが適切に変更されません。

[![](ios-code-only-images/image7.png "If the user rotates the device to landscape, the controls do not resize appropriately")](ios-code-only-images/image7.png#lightbox)

この問題を解決する1つの方法は、各ビューの `AutoresizingMask` プロパティを設定することです。 この例では、コントロールを水平方向に拡張するため、各 `AutoresizingMask`を設定します。 次の例は `usernameField`を対象としていますが、ビュー階層の各ガジェットにも同様に適用する必要があります。

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

次に示すように、デバイスまたはシミュレーターを回転させると、すべてが追加の領域を占めるようになります。

[![](ios-code-only-images/image8.png "All the controls stretch to fill the additional space")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>カスタムビューの作成

UIKit の一部であるコントロールを使用するだけでなく、カスタムビューを使用することもできます。 カスタムビューを作成するには、`UIView` から継承し、`Draw`をオーバーライドします。 ここでは、カスタムビューを作成し、それをビュー階層に追加して説明します。

### <a name="inheriting-from-uiview"></a>UIView からの継承

まず、カスタムビューのクラスを作成する必要があります。 これを行うには、Visual Studio の**クラス**テンプレートを使用して、`CircleView`という名前の空のクラスを追加します。 基本クラスは `UIView`に設定する必要があります。これは、`UIKit` 名前空間にあることを思い出してください。 また、`System.Drawing` 名前空間も必要になります。 この例では、その他のさまざまな `System.*` 名前空間は使用されないため、削除してもかまいません。

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

すべての `UIView` には、描画する必要があるときにシステムによって呼び出される `Draw` メソッドがあります。 `Draw` を直接呼び出すことはできません。 これは、実行ループ処理中にシステムによって呼び出されます。 ビューがビュー階層に追加された後に run ループを初めて実行すると、その `Draw` メソッドが呼び出されます。 `Draw` の後続の呼び出しは、ビューがビューで `SetNeedsDisplay` または `SetNeedsDisplayInRect` を呼び出すことによって描画する必要があるとマークされている場合に発生します。

次に示すように、オーバーライドされた `Draw` メソッド内にそのようなコードを追加することで、ビューに描画コードを追加できます。

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

`CircleView` は `UIView`であるため、`UIView` プロパティも設定できます。 たとえば、コンストラクターで `BackgroundColor` を設定できます。

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

先ほど作成した `CircleView` を使用するには、既存のコントローラーのビュー階層にサブビューとして追加します。これは、前の `UILabels` や `UIButton` の場合と同じです。また、新しいコントローラーのビューとして読み込むこともできます。 後者について考えてみましょう。

### <a name="loading-a-view"></a>ビューの読み込み

 `UIViewController` には、ビューを作成するためにコントローラーによって呼び出される `LoadView` という名前のメソッドがあります。 これは、ビューを作成してコントローラーの `View` プロパティに割り当てるための適切な場所です。

まず、コントローラーが必要であるため、`CircleController`という名前の新しい空のクラスを作成します。

`CircleController` 次のコードを追加して、`View` を `CircleView` に設定します (オーバーライドで `base` の実装を呼び出すことはできません)。

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

[![](ios-code-only-images/circles.png "The new view with a circle is displayed")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>起動画面の作成

[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)は、アプリが応答していることをユーザーに表示する手段として起動したときに表示されます。 起動画面はアプリの読み込み時に表示されるため、アプリケーションがまだメモリに読み込まれているため、コードで作成することはできません。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で iOS プロジェクトを作成すると、xib ファイルの形式で起動画面が表示されます。これは、プロジェクト内の**Resources**フォルダーにあります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で iOS プロジェクトを作成すると、ストーリーボードファイルの形式で起動画面が表示されます。

-----

これを編集するには、ダブルクリックして iOS Designer で開くことができます。

Apple では、iOS 8 以降を対象とするアプリケーションで xib ファイルまたはストーリーボードファイルを使用することをお勧めします。 iOS デザイナーでいずれかのファイルを起動するときに、サイズクラスと自動レイアウトを使用して、すべてのデバイスで適切に表示され、正しく表示されるようにレイアウトを調整します。白紙. Xib またはストーリーボードに加えて、静的な起動イメージを使用して、以前のバージョンを対象とするアプリケーションをサポートできるようにすることができます。

起動画面の作成の詳細については、次のドキュメントを参照してください。

- [Xib を使用して起動画面を作成する](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [ストーリーボードを使用した起動画面の管理](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> IOS 9 では、起動画面を作成する主な方法としてストーリーボードを使用することをお勧めします。

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>IOS 8 より前のアプリケーションの起動イメージの作成

アプリケーションが iOS 8 より前のバージョンを対象としている場合は、xib またはストーリーボードの起動画面に加えて、静的イメージを使用できます。

この静的イメージは、アプリケーションで、Info.plist ファイルで設定することも、アセットカタログ (iOS 7 の場合) として設定することもできます。 アプリケーションが実行される可能性のあるデバイスのサイズ (320x480、640x960、640x1136) ごとに個別のイメージを提供する必要があります。 起動画面のサイズの詳細については、[起動画面イメージ](~/ios/app-fundamentals/images-icons/launch-screens.md)ガイドを参照してください。

> [!IMPORTANT]
> アプリに起動画面が表示されない場合は、画面に完全に収まらないことがわかります。 この場合は、少なくとも、`Default-568@2x.png` という名前の640x1136 イメージを、Info.plist に含めるようにしてください。

## <a name="summary"></a>まとめ

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

この記事では、Visual Studio でプログラムによって iOS アプリケーションを開発する方法について説明しました。 ここでは、空のプロジェクトテンプレートからプロジェクトを構築する方法について説明し、ルートビューコントローラーを作成してウィンドウに追加する方法について説明しました。 次に、UIKit からコントロールを使用して、アプリケーション画面を開発するコントローラー内にビュー階層を作成する方法を説明しました。 次に、さまざまな方向にビューを適切に配置する方法を確認しました。また、`UIView`をサブクラス化してカスタムビューを作成する方法と、コントローラー内にビューを読み込む方法についても説明しました。 最後に、起動画面をアプリケーションに追加する方法について説明します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

この記事では、Visual Studio for Mac でプログラムで iOS アプリケーションを開発する方法について説明しました。 ここでは、1つのビューテンプレートからプロジェクトをビルドする方法を説明し、ルートビューコントローラーを作成してウィンドウに追加する方法について説明しました。 次に、UIKit からコントロールを使用して、アプリケーション画面を開発するコントローラー内にビュー階層を作成する方法を説明しました。 次に、さまざまな方向にビューを適切に配置する方法を確認しました。また、`UIView`をサブクラス化してカスタムビューを作成する方法と、コントローラー内にビューを読み込む方法についても説明しました。 最後に、起動画面をアプリケーションに追加する方法について説明します。

-----

## <a name="related-links"></a>関連リンク

- [SimpleLogin (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplelogin)
