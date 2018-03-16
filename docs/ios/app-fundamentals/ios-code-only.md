---
title: "コードで iOS のユーザー インターフェイスの作成"
description: "Xamarin.iOS アプリ – iOS 用、またはコードでの Xamarin デザイナーでは、ユーザー インターフェイスを作成する 2 つの方法を提供します。 この記事では、コードで完全 iOS ユーザー インターフェイスを作成する方法について説明します。 UIKit からビューの階層を作成してコント ローラーでアプリケーションの画面を構築するプロジェクト テンプレートからを起動する方法を示します。 その後、コント ローラーで読み込むことができるカスタム ビューを作成する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d53dea1a46c6b42f901beb217eb00b3a3fa0fd92
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2018
---
# <a name="creating-ios-user-interfaces-in-code"></a>コードで iOS のユーザー インターフェイスの作成

_Xamarin.iOS アプリ – iOS 用、またはコードでの Xamarin デザイナーでは、ユーザー インターフェイスを作成する 2 つの方法を提供します。この記事では、コードで完全 iOS ユーザー インターフェイスを作成する方法について説明します。UIKit からビューの階層を作成してコント ローラーでアプリケーションの画面を構築するプロジェクト テンプレートからを起動する方法を示します。その後、コント ローラーで読み込むことができるカスタム ビューを作成する方法について説明します。_

IOS アプリのユーザー インターフェイス、storefront のように – アプリケーションは、通常、1 つのウィンドウを取得が、そのことができます、ウィンドウでいっぱいに多数のオブジェクトには、必要があるし、オブジェクトと配置を表示するアプリの必要に応じて変更することができます。 このシナリオのオブジェクト (ユーザーに表示される物事) はビューと呼ばれます。 アプリケーションで 1 つの画面をビルドするビューは相互に積み重ねられたコンテンツ ビューを階層では、階層は単一のビュー コント ローラーによって管理します。 複数の画面を持つアプリケーションには、複数のコンテンツ ビュー階層、それぞれに独自のビュー コントローラー、およびウィンドウ内のアプリケーションの場所のビューがあり、ユーザーに表示される画面に基づいて異なるコンテンツ ビュー階層を作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。 

[![](ios-code-only-images/image9.png "この図では、ウィンドウ、ビュー、サブビュー、およびビュー コント ローラー間の関係を示しています。")](ios-code-only-images/image9.png#lightbox)

使用してこれらのビュー階層を構築することができます、 [iOS 用の Xamarin デザイナー](~/ios/user-interface/designer/index.md) Visual Studio で、ただし方が便利なコードでまったく作業する方法の基本的な知識があります。 この記事は、一部の基本的なポイント取得して実行コードのみのユーザー インターフェイスの開発の手順について説明します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

次の図は、デバイスの画面にユーザー インターフェイスを表示するウィンドウ、ビュー、サブビュー、およびビュー コントローラー間の関係を示しています。 

[![](ios-code-only-images/image9.png "この図では、ウィンドウ、ビュー、サブビュー、およびビュー コント ローラー間の関係を示しています。")](ios-code-only-images/image9.png#lightbox)


使用してこれらのビュー階層を構築することができます、 [iOS 用の Xamarin デザイナー](~/ios/user-interface/designer/index.md) Mac 用 Visual Studio でただし方が便利なコードでまったく作業する方法の基本的な知識があります。 この記事は、一部の基本的なポイント取得して実行コードのみのユーザー インターフェイスの開発の手順について説明します。


-----

## <a name="creating-a-code-only-project"></a>コード専用のプロジェクトを作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS 空のプロジェクト テンプレート

最初に、iPhone を使用して Visual Studio で、iOS プロジェクトを作成**空のプロジェクト**以下に示すテンプレートをコント ローラーとビューを追加する拡張します。


[![](ios-code-only-images/blankapp-vs.png "新しいプロジェクト ダイアログ ボックス")](ios-code-only-images/blankapp-vs.png#lightbox)


空のプロジェクト テンプレートは、4 つのファイルをプロジェクトに追加します。


[![](ios-code-only-images/empty-project.png "プロジェクト ファイル")](ios-code-only-images/empty-project.png#lightbox)


1. **<code>appdelegate.cs</code>** -が含まれています、`UIApplicationDelegate`サブクラスは、 `AppDelegate` 、iOS からのアプリケーション イベントの処理に使用されます。 アプリケーション ウィンドウを作成、`AppDelegate`の`FinishedLaunching`メソッドです。
1. **Main.cs** -クラスを指定すると、アプリケーションのエントリ ポイントを含むため、`AppDelegate`です。
1. **Info.plist** -アプリケーションの構成情報を含むプロパティ一覧ファイル。
1. **Entitlements.plist** – プロパティ一覧ファイルの機能と、アプリケーションのアクセス許可に関する情報を格納します。


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS テンプレート


Mac 用の visual Studio では、空のテンプレートは提供されません。 すべてのテンプレートは、ストーリー ボードのサポートは、Apple が UI を作成する主な方法として推奨が付属します。 ただし、コードで UI を完全に作成することができます。 

次の手順に従ってアプリケーションからストーリー ボードを削除します。 


1. 新しい iOS プロジェクトを作成するのにには、1 つのアプリの表示テンプレートを使用します。
    
    [![](ios-code-only-images/single-view-app.png "1 つのアプリの表示テンプレートを使用します。")](ios-code-only-images/single-view-app.png#lightbox)

1. 削除、`Main.Storyboard`と`ViewController.cs`ファイル。 **いない**削除、`LaunchScreen.Storyboard`です。 ストーリー ボードで作成されるビュー コント ローラーの分離コードがあるために、ビュー コント ローラーを削除する必要があります。
1. 選択することを確認**削除**ポップアップ ダイアログ ボックス。
    
    [![](ios-code-only-images/delete.png "ポップアップ ダイアログ ボックスから削除 を選択します。")](ios-code-only-images/delete.png#lightbox)

1. Info.plist で内の情報を削除、**展開情報 > Main インターフェイス**オプション。
    
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


-----

## <a name="creating-a-window"></a>ウィンドウを作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

追加されたコード、 `FinishedLaunching` 、上記の手順 3 のメソッドは、iOS アプリケーションに対するウィンドウ作成に必要なコードの最小量。  

-----

iOS アプリケーションの構築を使用して、 [MVC パターン](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller)です。 アプリケーションが表示される最初の画面がウィンドウのルート ビュー コント ローラーから作成されます。 参照してください、[こんにちは, iOS マルチ スクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)自体の詳細については、MVC パターンを説明します。

実装、`AppDelegate`によって追加されたテンプレートを作成、アプリケーション ウィンドウの iOS アプリケーションごとに 1 つだけは、次のコードの表示にします。

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

スローされることを示す例外が得られる可能性がありますを今すぐこのアプリケーションを実行する場合は、`Application windows are expected to have a root view controller at the end of application launch`です。 コント ローラーを追加し、アプリのルート ビュー コント ローラーを作成してみましょう。

## <a name="adding-a-controller"></a>コントローラーを追加する

アプリは、多くのビュー コント ローラーを含めることができますが、1 つのルート ビュー コント ローラーをすべて、コント ローラーの表示を制御する必要があります。  コント ローラー ウィンドウを追加作成することで、`UIViewController`インスタンスに設定して、`window.RootViewController`プロパティ。

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

すべてのコント ローラーからアクセス可能な関連のビューがあります、`View`プロパティです。 上記のコードの変更、ビューの`BackgroundColor`プロパティを`UIColor.LightGray`表示されるように、次のようにできるようにします。

 [![](ios-code-only-images/image1.png "表示の明るい灰色では、ビューの背景")](ios-code-only-images/image1.png#lightbox)

いずれかの設定でした`UIViewController`サブクラスとして、`RootViewController`この方法で UIKit だけでなくおを自分で書くからコント ローラーを含む同様に、します。 たとえば、次のコードを追加、`UINavigationController`として、 `RootViewController`:

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

## <a name="creating-a-view-controller"></a>ビューのコント ローラーを作成します。

これでとしてコント ローラーを追加する方法を説明しました、`RootViewController`ウィンドウのコードでカスタム ビュー コント ローラーを作成する方法を見てみましょう。

という名前の新しいクラスを追加`CustomViewController`次のようにします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.png "CustomViewController をという名前の新しいクラスを追加します。")](ios-code-only-images/customviewcontroller.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "CustomViewController をという名前の新しいクラスを追加します。")](ios-code-only-images/new-file.png#lightbox)

-----

クラスを継承する必要があります`UIViewController`には、`UIKit`ように、名前空間。

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

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>ビューの初期化

`UIViewController` 呼び出されるメソッドを含む`ViewDidLoad`ビュー コント ローラーがメモリに読み込まれたときに呼び出されます。 これは、そのプロパティを設定するなど、ビューの初期化を行う適切な場所です。

たとえば、次のコードには、ボタンとボタンが押されたときに、新しいビュー コント ローラーをナビゲーション スタックをプッシュするイベント ハンドラーが追加されます。

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

新しいインスタンスを作成して、アプリケーションでこのコント ローラーを読み込む簡単なナビゲーションを示す、`CustomViewController`です。 新しいナビゲーション コント ローラーを作成、ビュー コント ローラーのインスタンスに渡すし、ウィンドウの新しいナビゲーション コント ローラーを設定`RootViewController`で、`AppDelegate`までと同様。

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

これで読み込むときに、アプリケーション、`CustomViewController`ナビゲーション コント ローラー内に読み込まれます。

 [![](ios-code-only-images/customvc.png "ナビゲーションのコント ローラー内で読み込まれる、CustomViewController")](ios-code-only-images/customvc.png#lightbox)
 
ボタンをクリックすると_プッシュ_ナビゲーション スタックに新しいビューのコント ローラー。

[![](ios-code-only-images/customvca.png "新しいビュー コント ローラーがナビゲーション スタックにプッシュされます。")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>ビューの階層構造の構築

上記の例では、ビュー コント ローラーにボタンを追加して、コードにユーザー インターフェイスを作成する開始しました。

iOS ユーザー インターフェイスは、階層の表示で構成されます。 ラベル、ボタン、スライダーなどの他のビューは、いくつかの親ビューのサブビューとして追加されます。

たとえば、編集、`CustomViewController`ユーザーがユーザー名とパスワードを入力できるログイン画面を作成します。 画面は、ボタンと 2 つのテキスト フィールドで構成されます。

### <a name="adding-the-text-fields"></a>テキスト フィールドを追加します。

追加したボタンをクリックし、イベント ハンドラーを最初に、削除、[ビューの初期化](#Initializing_the_View)セクションです。 

ユーザー名のコントロールを追加するには、作成と初期化、`UITextField`追加し、階層の表示を次のようにします。

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

作成するとき、 `UITextField`、設定、`Frame`その場所とサイズを定義するプロパティです。 0, 0 座標の左上に + x 右側に、iOS でおよび + y ダウンします。 設定した後、`Frame`とその他のいくつかのプロパティを呼び出して`View.AddSubview`を追加する、`UITextField`階層の表示にします。 これにより、`usernameField`のサブビュー、`UIView`インスタンスが、`View`プロパティの参照。 画面上の親ビューの前に、表示されるように、親ビューにより高いを z オーダーでサブビューが追加されます。

使用してアプリケーションを`UITextField`含まれているを次に示します。

 [![](ios-code-only-images/image4.png "アプリケーションに含まれている UITextField")](ios-code-only-images/image4.png#lightbox)

追加できる、`UITextField`同様の方法でパスワードをこの時点のみ設定、`SecureTextEntry`プロパティを true、次に示すようにします。

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

設定`SecureTextEntry = true`で入力したテキストを非表示に、`UITextField`次に示すように、ユーザーによって。

 [![](ios-code-only-images/image4a.png "True は、SecureTextEntry を設定、ユーザーが入力したテキストを非します。")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>ボタンの追加

次に、ユーザーがユーザー名とパスワードを送信できるように、ボタンを追加します。 親ビューの引数として渡すことによって、他のコントロールのような階層の表示に、ボタンが追加`AddSubview`メソッドを再度です。

次のコードは、ボタンを追加し、イベント ハンドラーを登録、`TouchUpInside`イベント。

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

こうすると、ログイン画面が表示されます次に示すよう。

 [![](ios-code-only-images/image5.png "ログイン画面")](ios-code-only-images/image5.png#lightbox)

異なり、iOS の以前のバージョンで既定のボタンの背景は透過的です。 変更、ボタンの`BackgroundColor`プロパティに変更します。

```csharp
submitButton.BackgroundColor = UIColor.White;
```

これが、一般的な丸め枠付きのボタンではなく、正方形のボタンに発生します。 丸い端を取得するには、次のスニペットを使用します。

```csharp
submitButton.Layer.CornerRadius = 5f;
```

これらの変更、ビューは次のようになります。

[![](ios-code-only-images/image6.png "ビューの例の実行")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>ビュー階層に複数のビューを追加します。

iOS を使用して、ビュー階層に複数のビューを追加する機能を提供する`AddSubviews`です。

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>ボタンの機能を追加します。

ボタンがクリックされたときに、ユーザーは、求める動作が発生します。 たとえば、アラートが表示されるか、ナビゲーションが別の画面に実行します。 

2 つ目のビュー コント ローラーをナビゲーション スタックにプッシュするコードを追加してみましょう。

最初に、2 つ目のビュー コント ローラーを作成します。

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

次に、機能を追加して、`TouchUpInside`イベント。

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

ナビゲーションは、次に示します。

[![](ios-code-only-images/navigation.png "このグラフで、ナビゲーションを示します")](ios-code-only-images/navigation.png#lightbox)

既定では、ナビゲーションのコント ローラーを使用する場合に iOS アプリケーションに提供、ナビゲーション バーとスタック後方へ移動できるようにするには、[戻る] ボタンに注意してください。

## <a name="iterating-through-the-view-hierarchy"></a>階層の表示を反復処理します。

サブビュー階層を反復処理し、特定の任意のビューを選択することができます。 たとえば、それぞれを検索する`UIButton`し、そのボタンを異なる`BackgroundColor`、次のスニペットを使用することができます

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

これには、ただしは動作しませんの反復処理されているビューの場合は、`UIView`のすべてのビューとして返される中に、`UIView`親ビューに追加されたオブジェクトとしてそれ自体を継承`UIView`です。

## <a name="handling-rotation"></a>回転の処理

ユーザーが横向きにデバイスを回転させる場合、コントロール サイズを変更しないで適切に、次のスクリーン ショットに示すように。

 [![](ios-code-only-images/image7.png "ユーザーが横向きにデバイスを回転させる場合、コントロール サイズを変更しないで適切に")](ios-code-only-images/image7.png#lightbox)

これを解決する方法の 1 つは、設定して、`AutoresizingMask`各ビューのプロパティです。 このケースで必要なコントロールを水平方向にストレッチするため、各設定`AutoresizingMask`です。 次の例は、`usernameField`が、同じ階層の表示では、各ガジェットに適用する必要があります。

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

今すぐおは、デバイスまたはシミュレーターを回転するときにすべて全体に引き伸ばす追加の領域では、次のように。

 [![](ios-code-only-images/image8.png "すべてのコントロールを追加の領域がいっぱいに拡大します。")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>カスタム ビューを作成します。

UIKit の一部であるコントロールを使用するだけでなく、カスタム ビューを使用もできます。 継承することで、カスタム ビューを作成できます`UIView`とオーバーライド`Draw`です。 カスタム ビューを作成し、階層の表示を示すために追加してみましょう。

### <a name="inheriting-from-uiview"></a>UIView から継承します。

まずを行うには必要なは、クラス、カスタム ビューを作成します。 それではを使用して、**クラス**という空のクラスを追加する Visual Studio でテンプレート`CircleView`です。 基本クラスに設定する必要があります`UIView`が内にあることに注意してください、`UIKit`名前空間。 必要、`System.Drawing`名前空間もします。 その他のさまざまな`System.*`名前空間はできませんこの例で使用されているため、自由にそれらを削除します。

クラスは、次のようになります。

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>における、UIView の描画

各`UIView`が、`Draw`それが描画されなければならないときに、システムによって呼び出されるメソッド。 `Draw` 絶対に呼び出さないで直接です。 実行ループ処理中に、システムによって呼び出されます。 最初に、ループの実行、ビューが階層の表示に追加された後、`Draw`メソッドが呼び出されます。 後続の呼び出し`Draw`を呼び出して、描画する必要があると、ビューが付いているときに発生する`SetNeedsDisplay`または`SetNeedsDisplayInRect`ビュー。

描画コード オーバーライド内でこのようなコードを追加することで、ビューに追加できる`Draw`メソッドを次のようにします。

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

`CircleView`は、`UIView`も設定できます`UIView`プロパティもします。 たとえば、設定できる、`BackgroundColor`コンス トラクターで。

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

使用する、`CircleView`先ほど作成した、おできますいずれかに追加してサブビューとして、既存のコント ローラーの階層の表示で行ったように、`UILabels`と`UIButton`前、または新しいコント ローラーのビューとして読み込みことができます。 後者を実行してみましょう。

### <a name="loading-a-view"></a>ビューの読み込み

 `UIViewController` という名前のメソッドを持つ`LoadView`をそのビューを作成するコント ローラーによって呼び出されます。 これは、適切な場所をビューを作成し、コント ローラーに割り当てる`View`プロパティです。

コント ローラーは、必要があります最初に、名前付き、新しい空のクラスを作成して`CircleController`です。

`CircleController`を設定する次のコードを追加、`View`を`CircleView`(呼び出す必要はありません、`base`オーバーライドでの実装)。

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

最後に、実行時に、コント ローラーを提示する必要があります。 しましょうこれ以前、次のように追加した送信ボタンのイベント ハンドラーを追加することで。

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

ここで、[アプリケーションを実行して送信] ボタンをタップして、円に新しいビューが表示されます。

 [![](ios-code-only-images/circles.png "円に新しいビューが表示されます。")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>起動画面を作成します。

A[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)応答であるユーザーに表示する方法として、アプリの起動時に表示されます。 起動画面が表示されるので、アプリを読み込むときに、アプリケーションがメモリに読み込まれているがまだ、コードで作成できません。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ときに、画面を起動して、Visual Studio でプロジェクトは含まれている .xib ファイルの形式で自動的に表示される iOS の作成、**リソース**プロジェクト内のフォルダーです。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

ときに、Mac、起動画面は、ストーリー ボード ファイルの形式で自動的に表示されるは、Visual Studio で iOS プロジェクトを作成します。 

-----

これは、二重 をクリックして、iOS デザイナーで開いて編集できます。

Apple は、.xib またはストーリー ボード ファイルを使用して iOS 8 を対象とするアプリケーションか、後で、iOS デザイナー内のいずれかのファイルを起動するときに使用するサイズ クラスと自動レイアウトで見栄えの良い、し、すべてのデバイスが正しく、表示できるように、レイアウトを調整することをお勧めサイズ。 以前のバージョンを対象とするアプリケーションをサポートするために、.xib またはストーリー ボードに加え、静的な起動イメージを使用できます。

起動画面を作成する方法の詳細については、次のドキュメントを参照してください。

- [使用して、.xib 起動画面を作成します。](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [ストーリー ボードでの起動画面を管理します。](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> **注:** Apple iOS 9、時点での起動画面を作成する主な方法としてストーリー ボードを使用する必要があることをお勧めします。

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>起動イメージの作成前の iOS 8 アプリケーション

アプリケーションが iOS 8 より前のバージョンを対象とする場合、.xib またはストーリー ボードの起動画面に加えて静的イメージを使用できます。 

この静的イメージは、Info.plist のファイルまたは (用、iOS 7)、アプリケーションでの資産カタログとして設定できます。 各デバイス サイズ (320 x 480、640 x 960、640 x 1136) をアプリケーションが実行される個別の画像を提供する必要があります。 起動画面サイズの詳細については、表示、[起動画面イメージ](~/ios/app-fundamentals/images-icons/launch-screens.md)ガイドです。

> [!IMPORTANT]
> **注:**画面を起動して、アプリがない場合は、画面を完全に一致しないことを確認可能性があります。 大文字と小文字の場合を確認してください含めるには、少なくとも、という名前の 640 x 1136 イメージ`Default-568@2x.png`Info.plist にします。 



## <a name="summary"></a>まとめ

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

この記事では、Visual Studio でプログラムで iOS アプリケーションを開発する方法について説明します。 お見て、空のプロジェクト テンプレートからプロジェクトをビルドする方法を作成し、ルート ビュー コント ローラー ウィンドウを追加する方法について説明します。 UIKit からコントロールを使用して、アプリケーションの画面を開発するコント ローラー内のビュー階層を作成する方法を紹介します。 次に調査方法、ビューを作成するレイアウト適切にさまざまな方向にサブクラス化して、カスタム ビューを作成する方法を説明しました`UIView`方法と同様、コント ローラー内のビューを読み込めません。 最後に起動画面をアプリケーションに追加する方法を説明します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

この記事には、Visual Studio でプログラムから for mac の iOS アプリケーションを開発する方法が説明されています。 について説明しました、1 つのビュー テンプレートからプロジェクトをビルドする方法を作成し、ルート ビュー コント ローラー ウィンドウを追加する方法について説明します。 UIKit からコントロールを使用して、アプリケーションの画面を開発するコント ローラー内のビュー階層を作成する方法を紹介します。 次に調査方法、ビューを作成するレイアウト適切にさまざまな方向にサブクラス化して、カスタム ビューを作成する方法を説明しました`UIView`方法と同様、コント ローラー内のビューを読み込めません。 最後に起動画面をアプリケーションに追加する方法を説明します。

-----




## <a name="related-links"></a>関連リンク

- [SimpleLogin (サンプル)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
