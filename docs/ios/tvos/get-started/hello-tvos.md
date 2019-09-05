---
title: Hello, tvOS クイックスタートガイド
description: このガイドでは、最初の tvOS アプリとその開発ツールチェーンを作成する手順について説明します。 また、UI コントロールをコードに公開し、tvOS アプリケーションをビルド、実行、およびテストする方法を示す Xamarin デザイナーについても説明します。
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 30fcf586a280688834e1ae9af61630c2611964a5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281821"
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvOS クイックスタートガイド

_このガイドでは、最初の tvOS アプリとその開発ツールチェーンを作成する手順について説明します。また、UI コントロールをコードに公開し、tvOS アプリケーションをビルド、実行、およびテストする方法を示す Xamarin デザイナーについても説明します。_

Apple は tvOS 11 を実行する apple tv 4 番目の世代をリリースしました。

Apple TV platform は開発者にオープンであり、充実したイマーシブアプリを作成し、Apple TV の組み込みの App Store でリリースできます。

Xamarin の開発に慣れている場合は、tvOS が非常に単純なものに移行することをお勧めします。 Api と機能のほとんどは同じですが、多くの一般的な Api は使用できません (WebKit など)。 さらに、Siri リモコンでを使用すると、タッチスクリーンベースの iOS デバイスには存在しない設計上の課題が生じます。

このガイドでは、Xamarin アプリでの tvOS の使用方法について説明します。 TvOS の詳細については、apple の apple [TV 4k ドキュメントの準備](https://developer.apple.com/tvos/)に関するドキュメントを参照してください。

## <a name="overview"></a>概要

TvOS を使用すると、 C#と .net で完全にネイティブな Apple TV アプリを開発できます。これは、 *Swift* (または) および*Xcode*での開発時に使用されるものと同じ OS X ライブラリおよびインターフェイスコントロールを使用して行います。

さらに、tvOS アプリはと .NET でC#記述されているため、一般的なバックエンドコードを xamarin、Xamarin、Android、および Xamarin. Mac アプリと共有できます。すべてのプラットフォームでネイティブエクスペリエンスを提供します。

この記事では、tvOS と Visual Studio を使用して Apple TV アプリを作成するために必要な主要な概念について説明します。これにより、ボタンがクリックされた回数をカウントする基本的な**Hello, tvOS**アプリを構築するプロセスについて説明します。

[![](hello-tvos-images/run05.png "アプリの実行例")](hello-tvos-images/run05.png#lightbox)

ここでは、次の概念について説明します。

- **Visual Studio for Mac** – Visual Studio for Mac の概要と、それを使用して tvOS アプリケーションを作成する方法について説明します。
- **TvOS アプリの構造**-tvOS アプリが構成されています。
- **ユーザーインターフェイスの作成**–を使用してユーザーインターフェイスを作成する方法については、「」を Xamarin Designer for iOS。
- **デプロイとテスト**– tvOS シミュレーターと real tvOS ハードウェアでアプリを実行およびテストする方法について説明します。

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Visual Studio for Mac で新しい Xamarin. tvOS アプリを開始する

前述のように、メイン画面に1つのボタン`Hello-tvOS`とラベルを追加するという Apple TV アプリを作成します。 ボタンがクリックされたとき、クリックされた回数がラベルに表示されます。

使用を開始するには、次の手順を実行します。

1. Visual Studio for Mac を起動します。

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. 画面の左上隅にある **[新しいソリューション...]** リンクをクリックして、 **[新しいプロジェクト]** ダイアログボックスを開きます。
3. [ **TvOS** > **app** Single View app] を選択し、[次へ] ボタンをクリックします。 > 

    [![](hello-tvos-images/setup02.png "単一ビューアプリを選択する")](hello-tvos-images/setup02.png#lightbox)
4. `Hello, tvOS` **アプリ名**として「」と入力し、**組織の識別子**を入力して、 **[次へ]** ボタンをクリックします。

    [![](hello-tvos-images/setup04.png "「Hello, tvOS」と入力します。")](hello-tvos-images/setup04.png#lightbox)
5. `Hello_tvOS` **プロジェクト名**として「」と入力し、 **[作成]** ボタンをクリックします。

    [![](hello-tvos-images/setup03.png "「HellotvOS」と入力します。")](hello-tvos-images/setup03.png#lightbox)

Visual Studio for Mac によって新しい tvOS アプリが作成され、アプリケーションのソリューションに追加される既定のファイルが表示されます。

 [![](hello-tvos-images/project01.png "既定のファイルビュー")](hello-tvos-images/project01.png#lightbox)

Visual Studio for Mac は、Visual Studio とまったく同じ方法で**ソリューション**と**プロジェクト**を使用します。 ソリューションは 1 つまたは複数のプロジェクトを保持できるコンテナーです。プロジェクトには、アプリケーション、サポート ライブラリ、テスト アプリケーションなどを含めることができます。この場合、Visual Studio for Mac によってソリューションとアプリケーションプロジェクトの両方が作成されます。

必要に応じて、共通の共有コードを含む1つまたは複数のコードライブラリプロジェクトを作成できます。 これらのライブラリプロジェクトは、標準の .NET アプリケーションを構築している場合と同様に、アプリケーションプロジェクトで使用することも、tvOS アプリプロジェクト (または、コードの種類に基づいて xamarin、Xamarin、Android、Xamarin. Mac) と共有することもできます。

## <a name="anatomy-of-a-xamarintvos-app"></a>TvOS アプリの構造

IOS プログラミングに精通している場合は、ここによく似ている点があります。 実際、tvOS 9 は iOS 9 のサブセットなので、ここでは多数の概念を説明します。

プロジェクト内のファイルを見てみましょう。

- `Main.cs` – このファイルには、アプリのメイン エントリ ポイントが含まれています。 アプリが起動した時点では、実行される最初のクラスとメソッドが含まれています。
- `AppDelegate.cs`–このファイルには、オペレーティングシステムからのイベントのリッスンを担当するメインアプリケーションクラスが含まれています。
- `Info.plist`–このファイルには、アプリケーション名、アイコンなどのアプリケーションプロパティが含まれています。
- `ViewController.cs`–これは、メインウィンドウを表すクラスであり、そのライフサイクルを制御します。
- `ViewController.designer.cs`–このファイルには、メイン画面のユーザーインターフェイスとの統合に役立つプラミングコードが含まれています。
- `Main.storyboard`–メインウィンドウの UI。 このファイルは、Xamarin Designer for iOS によって作成および管理できます。

以下のセクションでは、これらのファイルのいくつかを簡単に見ていきます。 これらの詳細については後で詳しく説明しますが、ここでは基本を理解しておくことをお勧めします。

### <a name="maincs"></a>Main.cs

この`Main.cs`ファイルには、 `Main`新しい tvOS app インスタンスを作成し、OS イベントを処理するクラスの名前を渡す静的メソッドが含まれています。ここ`AppDelegate`では、クラスを使用します。

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

この`AppDelegate.cs`ファイルには`AppDelegate` 、ウィンドウを作成し、OS イベントをリッスンするクラスが含まれています。

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

このコードは、前に iOS アプリケーションをビルドしている場合を除いてよく知られていますが、非常に簡単です。 重要な行を確認してみましょう。

まず、クラスレベル変数の宣言を見てみましょう。

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

プロパティ`Window`は、メインウィンドウへのアクセスを提供します。 tvOS は、*モデルビューコントローラー* (MVC) パターンと呼ばれるものを使用します。 一般に、作成するすべてのウィンドウ (およびウィンドウ内の他の多くのもの) には、コントローラーがあります。これは、ウィンドウの表示、新しいビュー (コントロール) の追加など、ウィンドウのライフサイクルを担当します。

次に、 `FinishedLaunching`メソッドを用意しました。 このメソッドは、アプリケーションがインスタンス化された後に実行され、実際にアプリケーションウィンドウを作成し、そこにビューを表示するプロセスを開始します。 アプリではストーリーボードを使用して UI を定義するため、ここで追加のコードは必要ありません。

テンプレートには、や`DidEnterBackground` `WillEnterForeground`などの他にも多くのメソッドが用意されています。 アプリでアプリケーションイベントが使用されていない場合は、これらを安全に削除できます。

### <a name="viewcontrollercs"></a>ViewController.cs

クラス`ViewController`は、メインウィンドウのコントローラーです。 これは、メインウィンドウのライフサイクルを担当することを意味します。 これについては後で詳しく説明しますが、ここでは簡単に見てみましょう。

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

メインウィンドウクラスのデザイナーファイルは現在空ですが、iOS Designer を使用してユーザーインターフェイスを作成すると、Visual Studio for Mac によって自動的に設定されます。

```csharp
using Foundation;

namespace HellotvOS
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

通常、デザイナーファイルは、Visual Studio for Mac によって自動的に管理され、アプリケーションのウィンドウまたはビューに追加されるコントロールへのアクセスを許可する必要なプラミングコードを提供するだけなので、デザイナーファイルには関係ありません。

TvOS アプリを作成したので、そのコンポーネントについての基本的な理解ができたので、UI の作成について見てみましょう。

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>ユーザー インターフェイスの作成

TvOS アプリのユーザーインターフェイスを作成するために Xamarin Designer for iOS を使用する必要はありません。コードからC#直接 UI を作成できますが、この記事では説明しません。 わかりやすくするために、このチュートリアルの残りの部分では、iOS Designer を使用して UI を作成します。

UI の作成を開始するには、**ソリューションエクスプローラー**内の`Main.storyboard`ファイルをダブルクリックして、iOS Designer で編集するために開きます。

[![](hello-tvos-images/designer01.png "ソリューション エクスプローラーの Main.storyboard ファイル")](hello-tvos-images/designer01.png#lightbox)

これにより、デザイナーが起動し、次のように表示されます。

[![](hello-tvos-images/designer02.png "デザイナー")](hello-tvos-images/designer02.png#lightbox)

IOS Designer とそのしくみの詳細については、「Xamarin Designer for iOS ガイド[の概要](~/ios/user-interface/designer/introduction.md)」を参照してください。

これで、tvOS アプリのデザイン画面にコントロールを追加できるようになりました。

次の手順で行います。

1. デザインサーフェイスの右側にある**ツールボックス**を見つけます。

    [![](hello-tvos-images/designer03.png "ツールボックス")](hello-tvos-images/designer03.png#lightbox)

    ここで見つからない場合は、表示されている **> パッド > ツールボックス**を参照して表示します。
2. **ツールボックス**からデザイン画面に**ラベル**をドラッグします。

    [![](hello-tvos-images/designer04.png "ツールボックスからラベルをドラッグする")](hello-tvos-images/designer04.png#lightbox)
3. **プロパティパッド** `Hello, tvOS`で**title**プロパティをクリックし、ボタンのタイトルをに変更し、**フォントサイズ**を128に設定します。

    [![](hello-tvos-images/designer05.png "タイトルを Hello, tvOS に設定し、フォントサイズを128に設定します。")](hello-tvos-images/designer05.png#lightbox)
4. すべての単語が表示されるようにラベルのサイズを変更し、ウィンドウの上部に中央に配置します。

    [![](hello-tvos-images/designer06.png "ラベルのサイズを変更して中央揃えにする")](hello-tvos-images/designer06.png#lightbox)
5. ラベルは、意図したとおりに表示されるように、その位置に制約される必要があります。 画面のサイズに関係なく。 これを行うには、 *T 形状のハンドル*が表示されるまでラベルをクリックします。

    [![](hello-tvos-images/designer07.png "T 形状のハンドル")](hello-tvos-images/designer07.png#lightbox)
6. ラベルを水平方向に制限するには、中央の四角形を選択し、垂直線にドラッグします。

    [![](hello-tvos-images/designer08.png "中央の四角形を選択します")](hello-tvos-images/designer08zoom.png#lightbox)

     ラベルをオレンジ色にする必要があります。
7. ラベルの上部にある T ハンドルを選択し、ウィンドウの上端にドラッグします。

    [![](hello-tvos-images/designer09.png "ハンドルをウィンドウの上端にドラッグします。")](hello-tvos-images/designer09.png#lightbox)
8. 次に、幅をクリックし、次に示すように、高さの*ボーンハンドル*をクリックします。

    [![](hello-tvos-images/designer10.png "幅と高さのボーンが処理する")](hello-tvos-images/designer10.png#lightbox)

     各*ボーンハンドル*をクリックしたら、幅と高さの両方を選択して固定の寸法を設定します。
9. 完了すると、制約は、プロパティパッドの [レイアウト] タブに表示されるようになります。

    [![](hello-tvos-images/designer11.png "制約の例")](hello-tvos-images/designer11.png#lightbox)
10. [**ツールボックス** **] からボタン**をドラッグして、ラベルの下に配置します。
11. **プロパティパッド**で **[タイトル]** プロパティをクリックし、ボタンのタイトルを次`Click Me`のように変更します。

    [![](hello-tvos-images/designer12.png "ボタンのタイトルを変更してクリックします")](hello-tvos-images/designer12.png#lightbox)
12. 上記の手順 5. ~ 8. を繰り返して、[tvOS] ウィンドウのボタンを制限します。 ただし、(手順 #7 のように) ウィンドウの上部に T ハンドルをドラッグするのではなく、ラベルの一番下にドラッグします。

    [![](hello-tvos-images/designer14.png "ボタンを制限する")](hello-tvos-images/designer14.png#lightbox)
13. ボタンの下に別のラベルをドラッグし、最初のラベルと同じ幅になるようにサイズを変更し、その**配置**を [**中央**揃え] に設定します。

    [![](hello-tvos-images/designer15.png "ボタンの下に別のラベルをドラッグし、最初のラベルと同じ幅になるようにサイズを変更して、その配置を [中央] に設定します。")](hello-tvos-images/designer15.png#lightbox)
14. 最初のラベルとボタンと同じように、このラベルを [center] に設定し、[位置とサイズ] にピン留めします。

    [![](hello-tvos-images/designer16.png "ラベルを位置とサイズにピン留めする")](hello-tvos-images/designer16.png#lightbox)
15. ユーザーインターフェイスへの変更を保存します。

コントロールのサイズ変更や移動を行っているときに、 [APPLE TV のヒューマンインターフェイスガイドライン](https://developer.apple.com/tvos/human-interface-guidelines/)に基づく便利なスナップヒントがデザイナーに表示されていることに注意してください。 これらのガイドラインは、Apple TV ユーザーの使い慣れたルックアンドフィールを備えた高品質のアプリケーションを作成するのに役立ちます。

**[ドキュメントアウトライン]** セクションを確認すると、ユーザーインターフェイスを構成する要素のレイアウトと階層がどのように表示されるかがわかります。

[![](hello-tvos-images/designer17.png "ドキュメントアウトラインセクション")](hello-tvos-images/designer17.png#lightbox)

ここから、必要に応じて、編集またはドラッグする項目を選択して、UI 要素を並べ替えることができます。 たとえば、UI 要素が別の要素によってカバーされていた場合は、それをリストの一番下にドラッグして、ウィンドウ上の一番上の項目にすることができます。

ユーザーインターフェイスを作成したので、UI 項目を公開して、tvOS がコード内でC#それらにアクセスして対話できるようにする必要があります。

### <a name="accessing-the-controls-in-the-code-behind"></a>分離コード内のコントロールへのアクセス

IOS デザイナーでコードから追加したコントロールにアクセスするには、主に次の2つの方法があります。

- コントロールにイベントハンドラーを作成する。
- コントロールに名前を付けて、後で参照できるようにします。

これらのいずれかが追加されると、内`ViewController.designer.cs`の部分クラスが更新され、変更が反映されます。 これにより、ビューコントローラー内のコントロールにアクセスできるようになります。

### <a name="creating-an-event-handler"></a>イベントハンドラーの作成

このサンプルアプリケーションでは、ボタンをクリックすると_何らか_の処理が行われます。そのため、ボタンの特定のイベントにイベントハンドラーを追加する必要があります。 これを設定するには、次の手順を実行します。

1. Xamarin iOS Designer で、ビューコントローラーのボタンを選択します。
2. プロパティ パッドで、**イベント** タブを選択します。

    [![](hello-tvos-images/event1.png "[イベント] タブ")](hello-tvos-images/event1.png#lightbox)
3. TouchUpInside イベントを見つけて、という名前`Clicked`のイベントハンドラーを指定します。

    [![](hello-tvos-images/event2.png "TouchUpInside イベント")](hello-tvos-images/event2.png#lightbox)
4. **Enter キーを**押すと、イベントハンドラーの場所がコードに提示され、 **viewcontroller**.cs ファイルが開きます。 キーボードの方向キーを使用して、場所を設定します。

    [![](hello-tvos-images/event3.png "場所の設定")](hello-tvos-images/event3.png#lightbox)
5. これにより、次のような部分メソッドが作成されます。

    [![](hello-tvos-images/event4.png "部分メソッド")](hello-tvos-images/event4.png#lightbox)

これで、ボタンを機能させるためのコードの追加を開始する準備ができました。

### <a name="naming-a-control"></a>コントロールの名前付け

このボタンをクリックすると、クリック数に基づいてラベルが更新されます。 これを行うには、コードでラベルにアクセスする必要があります。 これは、名前を付けることによって行われます。 次の手順で行います。

1. ストーリーボードを開き、ビューコントローラーの下部にあるラベルを選択します。
2. プロパティ パッドで、**ウィジェット** タブを選択します。

    [![](hello-tvos-images/name1.png "[ウィジェット] タブを選択します。")](hello-tvos-images/name1.png#lightbox)
3. **[Id > 名]** に`ClickedLabel`、次のように追加します。

    [![](hello-tvos-images/name2.png "ClickedLabel の設定")](hello-tvos-images/name2.png#lightbox)

これで、ラベルの更新を開始する準備ができました。

### <a name="how-controls-are-accessed"></a>コントロールにアクセスする方法

`ViewController.designer.cs` **ソリューションエクスプローラー**でを選択する`ClickedLabel`と、ラベルと`Clicked`イベントハンドラーがのC#**アウトレット**と**アクション**にどのようにマップされているかを確認できます。

[![](hello-tvos-images/accesscontrol.png "アウトレットとアクション")](hello-tvos-images/accesscontrol.png#lightbox)

また、 `ViewController.designer.cs`は部分クラスであるため、クラスに加えられた変更を`ViewController.cs`上書きするように Visual Studio for Mac を変更する必要がないことにも注意してください。

このように UI 要素を公開すると、ビューコントローラーでそれらにアクセスできるようになります。

通常、を開く必要はありませ`ViewController.designer.cs`ん。ここでは、教育目的でのみ提供されていました。

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>コードの記述

ユーザーインターフェイスが作成され、その UI 要素が**コンセント**と**アクション**を使用してコードに公開されると、最終的には、プログラムの機能を提供するコードを記述する準備が整います。

アプリケーションでは、最初のボタンがクリックされるたびに、ボタンがクリックされた回数を表示するようにラベルを更新します。 これを行うには、 `ViewController.cs` **Solution Pad**でファイルをダブルクリックして、ファイルを編集用に開く必要があります。

[![](hello-tvos-images/code01.png "Solution Pad")](hello-tvos-images/code01.png#lightbox)

まず、クラスの`ViewController`クラスレベルの変数を作成して、発生したクリックの回数を追跡する必要があります。 クラス定義を編集し、次のようにします。

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

次に、同じクラス (`ViewController`) で、 **ViewDidLoad**メソッドをオーバーライドし、ラベルの初期メッセージを設定するためのコードを追加する必要があります。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

`ViewDidLoad`は、 `ViewDidLoad` OSが`Initialize`読み込まれ、 ファイルからユーザーインターフェイスがインスタンス化された後に呼び出されるため、などの別のメソッドではなく、を使用する必要があり`.storyboard`ます。 `.storyboard`ファイルが完全に読み込まれてインスタンス化される前にラベルコントロールにアクセスしようとすると、 `NullReferenceException`ラベルコントロールがまだ作成されていないため、エラーが発生します。

次に、ボタンをクリックしたユーザーに応答するコードを追加する必要があります。 次の部分クラスを、作成したに追加します。

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

このコードは、ユーザーがボタンをクリックするたびに呼び出されます。

すべてが整ったら、tvOS アプリケーションをビルドしてテストする準備ができました。

## <a name="testing-the-application"></a>アプリケーションのテスト

アプリケーションをビルドして実行し、期待どおりに実行されることを確認します。 すべてを1回の手順でビルドして実行することも、実行せずにビルドすることもできます。

アプリケーションをビルドするたびに、必要なビルドの種類を選択できます。

- **デバッグ**–デバッグビルドは、追加のメタデータを含む ' ' (アプリケーション) ファイルにコンパイルされ、アプリケーションの実行中に何が起こっているかをデバッグできます。
- **リリース**–リリースビルドでは ' ' ファイルも作成されますが、デバッグ情報は含まれていないため、サイズが小さくなり、実行速度が速くなります。  

Visual Studio for Mac 画面の左上隅にある**構成セレクター**からビルドの種類を選択できます。

[![](hello-tvos-images/run01.png "ビルドの種類を選択します")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>アプリケーションのビルド

ここでは、デバッグビルドのみが必要なので、 **[デバッグ]** が選択されていることを確認してみましょう。 まず、 **⌘ B**を押すか、 **[ビルド]** メニューの **[すべてビルド]** をクリックして、アプリケーションをビルドします。

エラーがない場合は、Visual Studio for Mac のステータスバーに**ビルド成功**メッセージが表示されます。 エラーが発生した場合は、プロジェクトを確認し、手順に従っていることを確認してください。 まず、コード (Xcode と Visual Studio for Mac 内の両方) がチュートリアルのコードと一致することを確認します。

### <a name="running-the-application"></a>アプリケーションの実行

アプリケーションを実行するには、次の3つのオプションがあります。

- **⌘+Enter** を押します。
- **[実行]** メニューの **[デバッグ]** を選択します。
- Visual Studio for Mac ツール バー (**ソリューション エクスプローラー**のすぐ上) の **[再生]** ボタンをクリックします。

アプリケーションがビルドされ (まだビルドされていない場合)、デバッグモードで起動すると、tvOS シミュレーターが起動し、アプリが起動し、メインインターフェイスウィンドウが表示されます。

[![サンプルアプリのホーム画面](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

**[ハードウェア]** メニューの **[Apple TV リモコンの表示]** を選択して、シミュレーターを制御できるようにします。

[![](hello-tvos-images/run04.png "[Apple TV リモコンを表示する] を選択します。")](hello-tvos-images/run04.png#lightbox)

シミュレーターのリモートを使用して、ボタンを数回クリックすると、ラベルがカウントで更新されます。

[![](hello-tvos-images/run05.png "カウントが更新されたラベル")](hello-tvos-images/run05.png#lightbox)

おめでとうございます! ここでは、このチュートリアルについて詳しく説明しましたが、このチュートリアルを最初から最後まで実行した場合は、tvOS アプリのコンポーネントと、それらを作成するためのツールを十分に理解する必要があります。

## <a name="where-to-next"></a>次の場所

Apple TV アプリの開発では、ユーザーとインターフェイス (ユーザーの手に含まれていない部屋にある) との接続が切断されているため、いくつかの課題があります。また、アプリのサイズとストレージには tvOS の制限があります。

そのため、tvOS アプリの設計に進む前に、次のドキュメントを読むことを強くお勧めします。

- [TvOS 9 の概要](~/ios/tvos/platform/tvos9.md)–この記事では、tvOS 9 で使用できるすべての新しい api と変更された api と機能について、tvOS 開発者向けに紹介します。
- [ナビゲーションとフォーカスの操作](~/ios/tvos/app-fundamentals/navigation-focus.md)– tvOS アプリのユーザーは、iOS と直接やり取りするのではなく、デバイスの画面上のイメージをタップしますが、Siri リモートを使用してルームから間接的に使用します。 この記事では、フォーカスの概念と、tvOS アプリのユーザーインターフェイスでのナビゲーションの処理に使用する方法について説明します。
- [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md) : ユーザーが Apple TV と tvOS アプリを操作する主な方法は、付属の Siri リモートを使用することです。 アプリがゲームの場合は、必要に応じて、アプリで iOS (MFI) Bluetooth ゲームコントローラー用に作成されたサードパーティのサポートを組み込むこともできます。 この記事では、tvOS アプリでの新しい Siri リモートおよび Bluetooth ゲームコントローラーのサポートについて説明します。
- [リソースとデータストレージ](~/ios/tvos/app-fundamentals/resources-data-storage.md)-iOS デバイスとは異なり、新しい Apple TV は tvOS アプリ用の永続的なローカルストレージを提供しません。 その結果、tvOS アプリで情報を保持する必要がある場合 (ユーザー設定など)、iCloud からそのデータを格納して取得する必要があります。 この記事では、tvOS アプリでのリソースと永続データストレージの使用について説明します。
- [アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)–魅了のアイコンと画像の作成は、Apple TV アプリのイマーシブユーザーエクスペリエンスを開発するうえで重要な部分です。 このガイドでは、tvOS アプリに必要なグラフィック資産を作成して含めるために必要な手順について説明します。
- [ユーザーインターフェイス](~/ios/tvos/user-interface/index.md)–ユーザーインターフェイス (UI) コントロールを含む一般的なユーザーエクスペリエンス (ux) には、tvOS を操作するときに Xcode の INTERFACE BUILDER と UX の設計原則を使用します。
- [デプロイとテスト](~/ios/tvos/deploy-test/index.md): このセクションでは、アプリのテストと配布方法に関するトピックについて説明します。 このトピックには、デバッグに使用するツール、テスト担当者へのデプロイ、Apple TV App Store にアプリケーションを発行する方法などが含まれます。

TvOS の使用に関する問題が発生した場合は、[トラブルシューティング](~/ios/tvos/troubleshooting.md)のドキュメントを参照して、既知の問題と解決策の一覧を確認してください。

## <a name="summary"></a>Summary

この記事では、簡単な Hello, tvOS アプリを作成することによって、Visual Studio for Mac を使用して tvOS 用のアプリを開発する方法を簡単に説明しました。 TvOS デバイスのプロビジョニング、インターフェイスの作成、tvOS シミュレーターでの tvOS とテストのコーディングの基本について説明しています。


## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin を使用した tvOS 用アプリの構築 (ビデオ)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
