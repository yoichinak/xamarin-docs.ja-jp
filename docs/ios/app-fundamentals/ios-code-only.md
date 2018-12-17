---
title: Xamarin.iOS でのコードで iOS ユーザー インターフェイスの作成
description: このドキュメントでは、コードを使用して Xamarin.iOS アプリのユーザー インターフェイスを構築する方法について説明します。 回転をその他の処理、ビュー階層を構築、ビュー コント ローラーがについて説明します。
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 2fa554264578ec626567ef7d28377ac80bde21d3
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53060175"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Xamarin.iOS でのコードで iOS ユーザー インターフェイスの作成

iOS アプリのユーザー インターフェイスは、ガラス張りの店先のようなものです。– アプリケーションは通常 1 つのウィンドウを持ちますが、必要に応じてオブジェクトでウィンドウをいっぱいにすることができます。また、アプリが何を表示したいかに応じて、オブジェクトとその配置を変更できます。 このシナリオのオブジェクト (ユーザーが見る画面) はビューと呼ばれます。 アプリケーションで 1 つの画面を構築するために、コンテンツ ビューの階層内でビューは相互に積み重ねられ、それらの階層は単一のビュー コントローラーによって管理されます。 複数の画面を持つアプリケーションには、複数のコンテンツ ビューの階層があり、それぞれが独自のビュー コントローラーを持ちます。また、アプリケーションはウィンドウ内にビューを配置し、ユーザーに表示される画面に基づいて異なるコンテンツ ビューの階層を作成します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。 

[![](ios-code-only-images/image9.png "この図では、ウィンドウ、ビュー、サブビュー、およびビュー コント ローラー間のリレーションシップを示しています。")](ios-code-only-images/image9.png#lightbox)

Visual Studio では、[iOS 用の Xamarin デザイナー](~/ios/user-interface/designer/index.md)　を使用してこれらのビュー階層を構築することができますが、全てをコードで操作する方法を根本的に理解しておくとよいでしょう。  この記事では、コードのみのユーザー インターフェイス開発を始めるためのいくつかの基本的なポイントについて説明します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。 

[![](ios-code-only-images/image9.png "この図では、ウィンドウ、ビュー、サブビュー、およびビュー コント ローラー間のリレーションシップを示しています。")](ios-code-only-images/image9.png#lightbox)

Visual studio for Mac では、[iOS 用の Xamarin デザイナー](~/ios/user-interface/designer/index.md) を使用してこれらのビュー階層を構築することができますが、全てをコードで操作する方法を根本的に理解しておくとよいでしょう。 この記事では、コードのみのユーザー インターフェイス開発を始めるためのいくつかの基本的なポイントについて説明します。

-----

## <a name="creating-a-code-only-project"></a>コードのみのプロジェクトを作成します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>iOS の空のプロジェクト テンプレート

最初に、Visual Studio を使用して、iOS プロジェクトを作成、**ファイル > 新しいプロジェクト > Visual c# > iPhone と iPad > iOS アプリ (Xamarin)** プロジェクトは、次に示します。

[![新しいプロジェクト ダイアログ ボックス](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

選択し、**空のアプリ**プロジェクト テンプレート。

[![テンプレート ダイアログ ボックスを選択します。](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

空のプロジェクト テンプレートには、プロジェクトに 4 つのファイルが追加されます。

[![プロジェクト ファイル](ios-code-only-images/empty-project.w157-sml.png "プロジェクト ファイル")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.cs** -が含まれています、`UIApplicationDelegate`サブクラスでは、 `AppDelegate` 、iOS からのアプリケーション イベントの処理に使用されます。 アプリケーション ウィンドウが作成された、`AppDelegate`の`FinishedLaunching`メソッド。
1. **Main.cs** -クラスを指定すると、アプリケーションのエントリ ポイントを含むため、 `AppDelegate` 。
1. **Info.plist** -アプリケーションの構成情報を含むプロパティ一覧ファイル。
1. **Entitlements.plist** – 機能と、アプリケーションのアクセス許可に関する情報を含むプロパティ一覧ファイル。


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="ios-templates"></a>iOS のテンプレート


Visual Studio for Mac では、空のテンプレートが提供されません。 すべてのテンプレートが付属してストーリー ボードのサポートは、Apple は、UI を作成する主な方法としてお勧めします。 ただし、コードで完全に UI を作成することができます。 

次の手順に従って、アプリケーションからストーリー ボードを削除します。 


1. 新しい iOS プロジェクトを作成するのにには、単一ビュー アプリ テンプレートを使用します。

    [![](ios-code-only-images/single-view-app.png "単一ビュー アプリ テンプレートを使用します。")](ios-code-only-images/single-view-app.png#lightbox)

1. 削除、`Main.Storyboard`と`ViewController.cs`ファイル。 **いない**削除、`LaunchScreen.Storyboard`します。 ストーリー ボードで作成したビュー コント ローラーの分離コードは、ビュー コント ローラーを削除する必要があります。
1. 選択することを確認**削除**ポップアップ ダイアログ ボックス。

    [![](ios-code-only-images/delete.png "ポップアップ ダイアログ ボックスから削除 を選択します。")](ios-code-only-images/delete.png#lightbox)

1. Info.plist の内の情報を削除、**展開情報 > メイン インターフェイス**オプション。

    [![](ios-code-only-images/main-interface.png "メイン インターフェイス オプション内の情報を削除します。")](ios-code-only-images/main-interface.png#lightbox)

1. 最後に、次のコードを追加、 `FinishedLaunching` AppDelegate クラスのメソッド。

        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

コードに追加された、`FinishedLaunching`メソッドは、上記の手順 5 では、iOS アプリケーションに対するウィンドウ作成に必要なコードの最小量。


-----

使用して iOS アプリケーションの構築、 [MVC パターン](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc)します。 アプリケーションが表示される最初の画面は、ウィンドウのルート ビュー コント ローラーから作成されます。 参照してください、[こんにちは, iOS マルチ スクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)の詳細については、MVC パターン自体をガイドします。

実装、`AppDelegate`によって追加されたテンプレートを作成、アプリケーション ウィンドウのすべての iOS アプリケーションの 1 つだけであり、次のコードで可視化します。

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
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

このアプリケーションを今すぐ実行する場合は、ことを示すスローされる例外に得可能性`Application windows are expected to have a root view controller at the end of application launch`します。 コント ローラーを追加し、アプリのルート ビュー コント ローラーになります。

## <a name="adding-a-controller"></a>コントローラーを追加する

アプリは、多くのビュー コント ローラーを含めることができますが、すべてのビュー コント ローラーを制御する 1 つのルート ビュー コント ローラーが必要です。  コント ローラーを作成して、ウィンドウを追加、`UIViewController`インスタンスに設定して、`window.RootViewController`プロパティ。

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

すべてのコント ローラーに関連付けられたビューからアクセス可能な`View`プロパティ。 上記のコードの変更、ビューの`BackgroundColor`プロパティを`UIColor.LightGray`は、表示される、次に示すようにします。

 [![](ios-code-only-images/image1.png "ビューの背景が明るいグレーの表示")](ios-code-only-images/image1.png#lightbox)

いずれかに設定して`UIViewController`サブクラスとして、 `RootViewController` UIKit にも、自分で書くことからコント ローラーを同様に、この方法でなど。 たとえば、次のコードを追加、`UINavigationController`として、 `RootViewController`:

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

これには、次に示すように、ナビゲーション コント ローラー内で入れ子になったコント ローラーが生成されます。

 [![](ios-code-only-images/image2.png "ナビゲーション コント ローラー内で入れ子になったコント ローラー")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>ビュー コント ローラーの作成

これでとしてコント ローラーを追加する方法を見てきました、`RootViewController`ウィンドウのコードでカスタム ビュー コント ローラーを作成する方法を見てみましょう。

という名前の新しいクラスを追加`CustomViewController`次に示すよう。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "CustomViewController をという名前の新しいクラスを追加します。")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](ios-code-only-images/new-file.png "CustomViewController をという名前の新しいクラスを追加します。")](ios-code-only-images/new-file.png#lightbox)

-----

クラスを継承する必要があります`UIViewController`、これは、`UIKit`ように、名前空間。

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

`UIViewController` 呼び出されたメソッドが含まれています`ViewDidLoad`ビュー コント ローラーがメモリに読み込まれたときに呼び出されます。 これは、そのプロパティを設定するなど、ビューの初期化を実行する適切な場所です。

たとえば、次のコードには、ボタンとボタンが押されたときに、ナビゲーション スタックに新しいビュー コント ローラーをプッシュするイベント ハンドラーが追加されます。

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

新しいインスタンスを作成するアプリケーションでは、このコント ローラーを読み込むし、単純なナビゲーションを示す、`CustomViewController`します。 新しいナビゲーション コント ローラーを作成、ビュー コント ローラー インスタンスを渡すし、ウィンドウの新しいナビゲーション コント ローラーを設定`RootViewController`で、`AppDelegate`以前と同様。

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

これで、アプリケーションが読み込まれたら、`CustomViewController`ナビゲーション コント ローラー内で読み込まれます。

 [![](ios-code-only-images/customvc.png "ナビゲーション コント ローラー内で、CustomViewController が読み込まれる")](ios-code-only-images/customvc.png#lightbox)
 
ボタンをクリックして_プッシュ_ナビゲーション スタックに新しいビュー コント ローラー。

[![](ios-code-only-images/customvca.png "新しいビュー コント ローラーをナビゲーション スタックにプッシュ")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>ビュー階層を構築

上記の例では、コードでビュー コント ローラーへのボタンを追加することで、ユーザー インターフェイスを作成する開始しました。

iOS ユーザー インターフェイスは、ビュー階層で構成されます。 ラベル、ボタン、スライダーなど、追加のビューは、いくつかの親ビューのサブビューとして追加されます。

たとえば、編集、`CustomViewController`ユーザーがユーザー名とパスワードを入力できるログイン画面を作成します。 画面は、2 つのテキスト フィールドとボタンで構成されます。

### <a name="adding-the-text-fields"></a>テキスト フィールドを追加します。

最初で追加したボタンをクリックし、イベント ハンドラーを削除、[ビューの初期化](#Initializing_the_View)セクション。 

ユーザー名のコントロールを追加するには、作成と初期化、`UITextField`追加ビュー階層を次に示すよう。

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

作成するとき、 `UITextField`、設定、`Frame`その場所とサイズを定義するプロパティ。 IOS で 0, 0 の座標は右に画面の左 + x と + y ダウンします。 設定した後、`Frame`呼び出す他のいくつかのプロパティと共に`View.AddSubview`を追加する、`UITextField`ビュー階層にします。 これにより、`usernameField`のサブビュー、`UIView`インスタンスが、`View`プロパティの参照。 サブビューは、画面上の親ビューの前に表示されるように、親ビューによりも高いを z オーダーで追加されます。

使用してアプリケーションを`UITextField`に含まれる次のとおりです。

 [![](ios-code-only-images/image4.png "アプリケーションに含まれている UITextField")](ios-code-only-images/image4.png#lightbox)

追加できる、`UITextField`と同様の方法でパスワードに、この時点でのみ設定します、`SecureTextEntry`プロパティを true、次に示すように。

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

設定`SecureTextEntry = true`に入力したテキストを非表示に、`UITextField`次に示すように、ユーザー。

 [![](ios-code-only-images/image4a.png "True で、ユーザーが入力したテキストを非 SecureTextEntry を設定")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>ボタンの追加

次に、ユーザーは、ユーザー名とパスワードを送信できるように、ボタンを追加します。 親ビューの引数として渡すことによって、その他のコントロールと同様の階層を表示するボタンが追加`AddSubview`メソッドを再度します。

次のコードは、ボタンが追加されのイベント ハンドラーを登録、`TouchUpInside`イベント。

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

この場所は、ログイン画面が表示されます次に示すよう。

 [![](ios-code-only-images/image5.png "ログイン画面")](ios-code-only-images/image5.png#lightbox)

異なり、iOS の以前のバージョンで、既定のボタンの背景は透過的です。 変更ボタンの`BackgroundColor`プロパティに変更します。

```csharp
submitButton.BackgroundColor = UIColor.White;
```

これが、一般的な四捨五入枠付きのボタンではなく正方形のボタンで発生します。 丸い端を取得するには、次のスニペットを使用します。

```csharp
submitButton.Layer.CornerRadius = 5f;
```

これらの変更、ビューを次のようになります。

[![](ios-code-only-images/image6.png "ビューの例の実行")](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>ビュー階層に複数のビューを追加します。

iOS を使用して、ビュー階層に複数のビューを追加する機能を提供する`AddSubviews`します。

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>ボタンの機能を追加します。

ボタンがクリックされたときに何かが起こる、ユーザーが期待されます。 たとえば、アラートが表示されます。 または別の画面にナビゲーションが実行されます。

2 つ目のビュー コント ローラーをナビゲーション スタックにプッシュするコードを追加してみましょう。

まず、2 つ目のビュー コント ローラーを作成します。

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

機能を追加し、`TouchUpInside`イベント。

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

ナビゲーションは、次に示します。

[![](ios-code-only-images/navigation.png "ナビゲーションは、このグラフに示します")](ios-code-only-images/navigation.png#lightbox)

既定では、ナビゲーション コント ローラーを使用するときに iOS アプリケーションに提供、ナビゲーション バーとスタック後方へ移動できるように、[戻る] ボタンに注目してください。

## <a name="iterating-through-the-view-hierarchy"></a>ビューの階層構造を反復処理します。

サブビューの階層を反復処理し、特定のビューを選択することになります。 たとえば、それぞれを検索する`UIButton`し、そのボタンを別`BackgroundColor`、次のスニペットを使用できます

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

これには、ただしは動作しませんの反復処理されるビューの場合は、`UIView`すべてのビューがされているとして返されるよう、`UIView`継承自体を親ビューに追加されたオブジェクトとして`UIView`。

## <a name="handling-rotation"></a>回転の処理

ユーザーが横にデバイスを回転させる場合、コントロール サイズを変更しないで適切には、次のスクリーン ショットに示すように。

[![](ios-code-only-images/image7.png "コントロールは適切にサイズ変更されない場合は、ユーザーは、横にデバイスを回転、")](ios-code-only-images/image7.png#lightbox)

これを解決する方法の 1 つは、設定して、`AutoresizingMask`それぞれのビューのプロパティ。 そこで各設定は水平方向に拡張するコントロールをするここで`AutoresizingMask`します。 次の例は、`usernameField`が、同じ階層の表示では、各ガジェットに適用する必要があります。

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

今すぐデバイスまたはシミュレーターを回転ときすべて全体に引き伸ばす追加の領域では、次に示すよう。

[![](ios-code-only-images/image8.png "すべてのコントロールにストレッチする追加の領域の塗りつぶし")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>カスタム ビューの作成

UIKit の一部であるコントロールを使用するだけでなく、カスタム ビューを使用こともできます。 継承することによって、カスタム ビューを作成できます`UIView`とオーバーライド`Draw`します。 カスタム ビューを作成し、階層の表示を示すために追加してみましょう。

### <a name="inheriting-from-uiview"></a>UIView から継承します。

最初の手順を行うには、カスタム ビューのクラスを作成します。 これを使用して、**クラス**という名前の空のクラスを追加する Visual Studio でテンプレート`CircleView`します。 基本クラスに設定する必要があります`UIView`、ことを思い出してくださいこれは、`UIKit`名前空間。 必要があります、`System.Drawing`も名前空間。 その他の各種`System.*`名前空間はできませんを削除するこの例で使用されているので自由です。

クラスは、このようになります。

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>UIView の描画

すべて`UIView`が、`Draw`を描画する必要があるときに、システムによって呼び出されるメソッド。 `Draw` 絶対に呼び出さないで直接します。 実行ループの処理中に、システムによって呼び出されます。 ビューがビューの階層構造に追加された後に実行ループを最初にその`Draw`メソッドが呼び出されます。 後続の呼び出し`Draw`ビューがいずれかを呼び出すことで描画する必要があるとマークされている場合に発生する`SetNeedsDisplay`または`SetNeedsDisplayInRect`ビュー。

描画コード内でオーバーライドされたこのようなコードを追加することで、ビューに追加できます`Draw`メソッドを次に示すよう。

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

`CircleView`は、`UIView`も設定できます`UIView`プロパティもします。 たとえば、設定できます、`BackgroundColor`コンス トラクターで。

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

使用する、`CircleView`先ほど作成したことができますかに追加してくださいサブビューとして既存のコント ローラーの階層の表示と同様、`UILabels`と`UIButton`前、または新しいコント ローラーのビューとして読み込むことができます。 後者を実行してみましょう。

### <a name="loading-a-view"></a>ビューの読み込み

 `UIViewController` という名前のメソッドを持つ`LoadView`をそのビューを作成するコント ローラーによって呼び出されます。 これは、適切な場所にビューを作成し、コント ローラーに割り当てる`View`プロパティ。

コント ローラーは、最初に、必要がありますのでという名前の新しい空のクラスを作成`CircleController`です。

`CircleController`設定するのには、次のコードを追加、`View`を`CircleView`(呼び出す必要はありません、`base`オーバーライドでの実装)。

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

最後に、実行時に、コント ローラーを提示する必要があります。 次のように、先ほど追加した送信ボタンのイベント ハンドラーを追加することでこれを行ってしましょう。

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

ここで、アプリケーションを実行して [送信] ボタンをタップして、円で新しいビューが表示されます。

[![](ios-code-only-images/circles.png "円に新しいビューが表示されます。")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>起動画面を作成します。

A[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)応答性があるユーザーに表示する方法として、アプリの起動時に表示されます。 アプリの読み込み時に、起動画面が表示される、ため、アプリケーションはまだメモリに読み込み中にコードで作成できません。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

起動画面がで見つかる .xib ファイルの形式で提供される Visual Studio で iOS プロジェクトを作成するときに、**リソース**プロジェクト内のフォルダー。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac、iOS プロジェクトを作成するときに、起動画面は、ストーリー ボード ファイルの形式で提供されます。

-----

これは、二重にそれをクリックし、iOS Designer で開いて編集できます。

Apple は、.xib またはストーリー ボード ファイルを使用して iOS 8 を対象とするアプリケーションか、後で、iOS Designer のいずれかのファイルを起動するときに使用するサイズ クラスと自動レイアウト問題なさそうし、すべてのデバイスが正しく、表示するように、レイアウトを適応ことお勧めします。サイズ。 以前のバージョンを対象とするアプリケーションのサポートを許可する、.xib またはストーリー ボードだけでなく、静的な起動イメージを使用できます。

起動画面を作成する方法の詳細については、次のドキュメントを参照してください。

- [.Xib を使用して起動画面を作成します。](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [ストーリー ボードの起動画面を管理します。](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> IOS 9、時点では、Apple は、ストーリー ボードを起動画面の作成の主な方法として使用することお勧めします。

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>前の iOS の起動イメージ 8 アプリケーションを作成します。

アプリケーションは iOS 8 より前のバージョンを対象とする場合、.xib またはストーリー ボードの起動画面だけでなく静的なイメージを使用できます。 

この静的イメージは、Info.plist ファイル、または、アプリケーションでのアセット カタログ (iOS 7) を設定できます。 各デバイス サイズ (320 x 480、640 x 960、640 x 1136) で、アプリケーションを実行できる個別のイメージを提供する必要があります。 起動画面のサイズに関する詳細については、表示、[起動画面イメージ](~/ios/app-fundamentals/images-icons/launch-screens.md)ガイド。

> [!IMPORTANT]
> 起動画面アプリがない場合は、画面を十分に適合しないことに注意してください可能性があります。 大文字と小文字の場合は、含めるには、少なくとも、640 x 1136 イメージがという名前を確認する必要があります`Default-568@2x.png`Info.plist にします。 

## <a name="summary"></a>まとめ

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

この記事では、Visual Studio でプログラムで iOS アプリケーションを開発する方法について説明します。 見たので、空のプロジェクト テンプレートからプロジェクトをビルドする方法を作成し、ウィンドウをルート ビュー コント ローラーを追加する方法について説明します。 UIKit からコントロールを使用して、アプリケーションの画面を開発するコント ローラー内のビュー階層を作成する方法を紹介します。 調べる方法、ビューを適切に異なる向きで配置とをサブクラス化して、カスタム ビューを作成する方法を説明しました次`UIView`方法と同様、コント ローラー内のビューを読み込めません。 最後に、アプリケーションを起動画面を追加する方法を説明します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

この記事では、プログラムで Visual Studio for mac の iOS アプリケーションを開発する方法を説明しました。 見たので、1 つのビュー テンプレートからプロジェクトをビルドする方法を作成し、ウィンドウをルート ビュー コント ローラーを追加する方法について説明します。 UIKit からコントロールを使用して、アプリケーションの画面を開発するコント ローラー内のビュー階層を作成する方法を紹介します。 調べる方法、ビューを適切に異なる向きで配置とをサブクラス化して、カスタム ビューを作成する方法を説明しました次`UIView`方法と同様、コント ローラー内のビューを読み込めません。 最後に、アプリケーションを起動画面を追加する方法を説明します。

-----

## <a name="related-links"></a>関連リンク

- [SimpleLogin (サンプル)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
