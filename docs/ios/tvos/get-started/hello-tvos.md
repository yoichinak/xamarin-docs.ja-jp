---
title: "こんにちは, tvOS クイック スタート ガイド"
description: "このガイドは、初めて Xamarin.tvOS アプリとその開発ツール チェーンを作成する手順について説明します。 コードを UI コントロールを公開し、ビルド、実行、および Xamarin.tvOS アプリケーションをテストする方法を示しています。 Xamarin デザイナーも導入されています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: 5eccb36b3c6a437ddc1ec055e779d8f78460643e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="hello-tvos-quick-start-guide"></a>こんにちは, tvOS クイック スタート ガイド

_このガイドは、初めて Xamarin.tvOS アプリとその開発ツール チェーンを作成する手順について説明します。コードを UI コントロールを公開し、ビルド、実行、および Xamarin.tvOS アプリケーションをテストする方法を示しています。 Xamarin デザイナーも導入されています。_

Apple に Apple TV の Apple TV の 4 K、tvOS 11 を実行する 5 番目の世代がリリースされました。

Apple TV のプラットフォームでは、開発者は、豊富な実体験のアプリを作成し、Apple TV の組み込みアプリ ストアをリリースすることを開いています。

Xamarin.iOS 開発に詳しい場合は、非常に単純な tvOS への移行が検索する必要があります。 Api と機能のほとんどは、同じ、ただし、多くの一般的な Api がご利用いただけません (WebKit) などです。 さらに、操作、Siri リモートと課題がいくつかのデザインがタッチ スクリーン ベースの iOS デバイスではありません。

このガイドでは、tvOS Xamarin アプリでの操作の概要が提供されます。 TvOS の詳細については、Apple を参照してください[Apple tv 4 K 準備](https://developer.apple.com/tvos/)ドキュメント。

## <a name="overview"></a>概要

Xamarin.tvOS では、C# の場合と同じ OS X ライブラリおよびで開発するときに使用されるインターフェイスのコントロールを使用して .NET で完全にネイティブ Apple TV のアプリを開発することができます*Swift* (または*OBJECTIVE-C*) および*Xcode*です。

さらに、Xamarin.tvOS アプリは、c# と .NET で記述された、ため、一般的なバックエンド コードと共有できる Xamarin.iOS、Xamarin.Android Xamarin.Mac アプリです。同時に、各プラットフォームでネイティブのエクスペリエンスを提供します。

この記事の導入の基本的な作成処理を用いて Xamarin.tvOS と Visual Studio を使用して Apple TV アプリケーションを作成するために必要な重要な概念、**こんにちは、tvOS**回数をカウントするアプリのボタンにはクリックしてされました。

[![](hello-tvos-images/run05.png "例のアプリの実行")](hello-tvos-images/run05.png#lightbox)

次の概念を説明します。

-  **Visual Studio for Mac** – Mac とそれに Xamarin.tvOS アプリケーションを作成する方法の Visual Studio の概要です。
-  **Xamarin.tvOS アプリの構造は**– 新機能、Xamarin.tvOS アプリから成ります。
-  **ユーザー インターフェイスを作成する**– iOS 用の Xamarin デザイナーを使用してユーザー インターフェイスを作成する方法です。
-  **配置およびテスト**– を実行し、tvOS の実際のハードウェアおよび tvOS シミュレーターでアプリをテストします。

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Mac 用 Visual Studio で新しい Xamarin.tvOS アプリの開始

呼ばれる Apple TV アプリケーションを作成した前述のように、`Hello-tvOS`メイン画面に 1 つのボタンとラベルを追加します。 ボタンがクリックされたとき、クリックされた回数がラベルに表示されます。

はじめに、次の操作してみましょう。

1. Visual Studio for Mac を起動します。

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. をクリックして、**新しいソリューションをしています.**リンクを開くには、画面の左上隅で、**新しいプロジェクト** ダイアログ ボックス。
3. 選択**tvOS** > **アプリ** > **1 つのアプリの表示** をクリックし、**次**ボタン。

    [![](hello-tvos-images/setup02.png "アプリの 1 つのビューを選択します")](hello-tvos-images/setup02.png#lightbox)
4. 入力`Hello, tvOS`の**アプリ名**、入力、**組織 Id**  をクリック、 **次へ**ボタン。

    [![](hello-tvos-images/setup04.png "こんにちは、tvOS を入力します。")](hello-tvos-images/setup04.png#lightbox)
5. 入力`Hello_tvOS`の**プロジェクト名** をクリックし、**作成**ボタン。

    [![](hello-tvos-images/setup03.png "Enter HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Visual Studio for Mac は、新しい Xamarin.tvOS アプリが作成され、アプリケーションのソリューションに追加される既定のファイルを表示します。

 [![](hello-tvos-images/project01.png "既定のファイルを表示します。")](hello-tvos-images/project01.png#lightbox)

Visual Studio for Mac では使用**ソリューション**と**プロジェクト**、Visual Studio は、まったく同じ方法でします。 ソリューションは 1 つまたは複数のプロジェクトを保持できるコンテナーです。プロジェクトには、アプリケーション、サポート ライブラリ、テスト アプリケーションなどを含めることができます。この場合、Visual Studio for Mac が作成、ソリューションと、アプリケーション プロジェクトの両方。

場合は、1 つまたは複数のコードに、共通の共有コードを含むライブラリ プロジェクトを作成できます。 これらのライブラリ プロジェクトをアプリケーション プロジェクトで使用または他の Xamarin.tvOS アプリ プロジェクトと共有できます (またはコードの種類に基づいて、Xamarin.iOS、Xamarin.Android、および Xamarin.Mac)、標準の .NET アプリケーションを構築する場合と同様です。

## <a name="anatomy-of-a-xamarintvos-app"></a>Xamarin.tvOS アプリの構造

IOS プログラミングに慣れている場合は、多数の類似点は、ここかがわかります。 実際には、tvOS 9 は、ここに多数の概念が交差するよう、iOS 9 のサブセットです。

プロジェクト内のファイルを見てみましょう。

-   `Main.cs` – このファイルには、アプリのメイン エントリ ポイントが含まれています。 アプリが起動した時点では、実行される最初のクラスとメソッドが含まれています。
-   `AppDelegate.cs` – このファイルには、オペレーティング システムからのイベントをリッスンしているは、アプリケーションのメイン クラスが含まれています。
-   `Info.plist` – このファイルには、アプリケーション名、アイコンなどのアプリケーションのプロパティが含まれています。
-   `ViewController.cs` – これは、メイン ウィンドウを表し、そのライフ サイクルを制御するクラスです。
-   `ViewController.designer.cs` – このファイルには、メイン画面のユーザー インターフェイスと統合するのに役立つ組み込みコードが含まれています。
-  `Main.storyboard` – メイン ウィンドウの UI。 このファイルは、作成、iOS 用の Xamarin デザイナーで保持されていることができます。

次のセクションで、これらのファイルの一部を簡単に見てをみましょう。 見ていきましょうにさらに詳しく後が、今すぐその基本を理解することをお勧めします。

### <a name="maincs"></a>Main.cs

`Main.cs`ファイルには、静的なが含まれている`Main`新しい Xamarin.tvOS アプリ インスタンスを作成し、クラスの名前を渡しますメソッドは OS イベントを処理するこの例では、`AppDelegate`クラス。

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

`AppDelegate.cs`ファイルが含まれています、`AppDelegate`クラスは、このウィンドウを作成して、OS イベントの待機を担当します。

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

前に、iOS アプリケーションを構築しましたが、非常に簡単な場合を除き、このコードは既にご存じではありません。 重要な行を調べてみましょう。

最初に、クラス レベルの変数宣言を見てみましょう。

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window`プロパティは、メイン ウィンドウへのアクセスを提供します。 tvOS と呼ばれるものを使用して、*モデル ビュー コント ローラー* (MVC) パターン。 一般に、ウィンドウを作成するごとに (および windows 内で他の多くの要素) はなどへ (コントロール) の新しいビューの追加、表示などのウィンドウのライフ サイクルは、コント ローラーです。

次がある、`FinishedLaunching`メソッドです。 このメソッドは、アプリケーションがインスタンス化され、は、実際には、アプリケーション ウィンドウを作成して、ビューを表示することでプロセスを開始を後で実行されます。 アプリでは、ストーリー ボードを使用して、その UI を定義する、ため追加のコードは必要ありませんここで。

ように、テンプレートに用意されている他の多くの方法がある`DidEnterBackground`と`WillEnterForeground`です。 これらを安全に削除アプリケーションのイベントは、アプリで使用されていない場合。

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController`クラスは、メイン ウィンドウのコント ローラー。 つまり、これは、メイン ウィンドウのライフ サイクルを担当します。 ここで詳細に調べるこの後で、ここではだけを見てみましょうクイック見て。

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

メイン ウィンドウ クラスをデザイナー ファイルが今すぐ空が自動的に入力 Visual Studio によって Mac の iOS デザイナーを使用して、ユーザー インターフェイスを作成。

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

通常に関係していてデザイナー ファイルは、すぐに自動的に管理されています Visual Studio によって Mac 用としておだけは任意のウィンドウに追加したり表示したりするアプリケーションでは、コントロールへのアクセスを許可するために必要なプラミング コードを提供いたします。

これで、Xamarin.tvOS アプリを作成したとそのコンポーネントの基本的な知識がある、次の UI を作成するを見てみましょう。

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>ユーザー インターフェイスの作成

IOS 用の Xamarin デザイナーを使用して、Xamarin.tvOS アプリ用のユーザー インターフェイスを作成する必要はありませんが、c# コードから直接、UI を作成することができます、この記事の範囲外です。 わかりやすくするため、使用する iOS デザイナーは、このチュートリアルの残りの部分で UI を作成します。

UI の作成を開始する をダブルクリックしてみましょう、`Main.storyboard`ファイルで、**ソリューション エクスプ ローラー**ファイルを開いて、iOS デザイナーで編集します。

[![](hello-tvos-images/designer01.png "ソリューション エクスプ ローラーで Main.storyboard ファイル")](hello-tvos-images/designer01.png#lightbox)

これは、デザイナーを起動し、次のように必要があります。

[![](hello-tvos-images/designer02.png "デザイナー")](hello-tvos-images/designer02.png#lightbox)

詳細については、iOS デザイナーでそのしくみを参照してください、 [iOS 用の Xamarin デザイナーの概要](~/ios/user-interface/designer/introduction.md)ガイドです。

Xamarin.tvOS アプリのデザイン サーフェイスにコントロールを追加して開始できます。

次の手順で行います。

1. 検索、**ツールボックス**、デザイン画面の右側にある必要があります。

    [![](hello-tvos-images/designer03.png "ツールボックス")](hello-tvos-images/designer03.png#lightbox)

    見つからない場合、ここで、参照**ビュー > パッド > ツールボックス**を表示します。
2. ドラッグ、**ラベル**から、**ツールボックス**デザイン画面に。

    [![](hello-tvos-images/designer04.png "ラベルをツールボックスからドラッグします。")](hello-tvos-images/designer04.png#lightbox)
3. をクリックして、**タイトル**プロパティに、**プロパティ パッド**ボタンのタイトルを変更および`Hello, tvOS`設定と、**フォント サイズ**128 に。

    [![](hello-tvos-images/designer05.png "128 こんにちは、tvOS にタイトルを設定し、フォント サイズ")](hello-tvos-images/designer05.png#lightbox)
4. ラベルのサイズを変更して、すべての単語が表示され、ウィンドウの上部中央に配置できるようにします。

    [![](hello-tvos-images/designer06.png "サイズを変更し、ラベルの中心")](hello-tvos-images/designer06.png#lightbox)
5. ラベルが必要になりますの位置に制限することを意図したとおりに表示されるようにします。 画面のサイズに関係なく これを行うまでラベルをクリックして、 *T ハンドル*が表示されます。

    [![](hello-tvos-images/designer07.png "T 型ハンドル")](hello-tvos-images/designer07.png#lightbox)
6. ラベルを水平方向に制限するには、中央の四角形を選択し、垂直方向に破線にドラッグします。

    [![](hello-tvos-images/designer08.png "中央の四角形を選択します。")](hello-tvos-images/designer08zoom.png#lightbox)

     ラベルは、オレンジ色にする必要があります。
7. ラベルの上部にある T ハンドルを選択し、ウィンドウの上端にドラッグします。

    [![](hello-tvos-images/designer09.png "ハンドルをウィンドウの上端にドラッグします。")](hello-tvos-images/designer09.png#lightbox)
8. 次に、クリックして、幅と高さを*骨ハンドル*下図のようにします。

    [![](hello-tvos-images/designer10.png "幅と高さ骨ハンドル")](hello-tvos-images/designer10.png#lightbox)

     ときに各*骨ハンドル*は幅と高さの両方それぞれを固定サイズの設定を選択 をクリックします。
9. 完了すると、制約はパッドのプロパティの [レイアウト] タブのようになります。

    [![](hello-tvos-images/designer11.png "例の制約")](hello-tvos-images/designer11.png#lightbox)
8. ドラッグ、**ボタン**から、**ツールボックス**し、ラベルの下に配置します。
9. をクリックして、**タイトル**プロパティに、**プロパティ パッド**ボタンのタイトルを変更および`Click Me`:

    [![](hello-tvos-images/designer12.png "ここをクリックするボタン タイトルを変更します。")](hello-tvos-images/designer12.png#lightbox)
10. 5 ~ 8 tvOS ウィンドウで、ボタンを制限する上記の手順を繰り返します。 ただし、T ハンドルを (手順 7) とウィンドウの上部にドラッグすると、代わりにドラッグ、ラベルの最下位に。

    [![](hello-tvos-images/designer14.png "ボタンを制限します。")](hello-tvos-images/designer14.png#lightbox)
11. 別のラベル、ボタンの下にドラッグ、サイズは、最初のラベルとセットと同じ幅にその**配置**に**Center**:

    [![](hello-tvos-images/designer15.png "別のラベル、ボタンの下にドラッグ、サイズの最初のラベルと同じ幅ありセンターにその配置を設定します。")](hello-tvos-images/designer15.png#lightbox)
12. 最初のラベルやボタンなどの中心や位置とサイズにピン留めするには、このラベルを設定します。

    [![](hello-tvos-images/designer16.png "位置とサイズにラベルをピン留め")](hello-tvos-images/designer16.png#lightbox)
13. ユーザー インターフェイスに、変更を保存します。

サイズを変更するでコントロールを移動した、する必要がありますにお気付きこと、デザイナーを使用する便利なスナップ ヒントに基づく[Apple TV のヒューマン インターフェイス ガイドライン](https://developer.apple.com/tvos/human-interface-guidelines/)です。 次のガイドラインを使用すると、Apple TV のユーザーの使い慣れたルック アンド フィールを持っている高品質なアプリケーションを作成できます。

参照する場合、 **ドキュメント アウトライン**セクションで、レイアウトと、ユーザー インターフェイスを構成する要素の階層の表示方法に注意してください。

[![](hello-tvos-images/designer17.png "ドキュメント アウトライン セクション")](hello-tvos-images/designer17.png#lightbox)

ここからは、編集、または UI 要素の順序を変更して必要な場合にドラッグする項目を選択できます。 たとえば、UI 要素は、別の要素で説明するが、ウィンドウの最上位の項目を一覧の一番下にはドラッグでした。

これでが作成される、ユーザー インターフェイス、Xamarin.tvOS がアクセスして、c# コードでこれらと対話できるように、UI 項目を公開する必要があります。

### <a name="accessing-the-controls-in-the-code-behind"></a>分離コード内のコントロールへのアクセス

コードから iOS デザイナーで追加したコントロールにアクセスする主な 2 つの方法があります。

* コントロールのイベント ハンドラーを作成しています。
* お後で参照できるように、名前、コントロールを提供します。

これらのいずれかが追加されたとき、内の部分クラス、`ViewController.designer.cs`変更を反映するように更新されます。 これによって、ビュー コント ローラー内のコントロールにアクセスできます。

### <a name="creating-an-event-handler"></a>イベント ハンドラーの作成

このサンプル アプリケーションで、ボタンがクリックされたときに必要_もの_起こるため、イベント ハンドラーは、ボタン上の特定のイベントに追加する必要があります。 これを設定するには、次の操作を行います。

1. Xamarin iOS デザイナーでは、ビュー コント ローラー上のボタンを選択します。
2. パッドでは、プロパティ、選択、**イベント** タブ。

    [![](hello-tvos-images/event1.png "[イベント] タブ")](hello-tvos-images/event1.png#lightbox)
3. TouchUpInside イベントを見つけて、イベント ハンドラーをという名前を付けます`Clicked`:

    [![](hello-tvos-images/event2.png "TouchUpInside イベント")](hello-tvos-images/event2.png#lightbox)
4. 押すと**Enter**、 **ViewController**.cs ファイルを開くには、コードでイベント ハンドラーの場所を提案します。 場所を設定するのにには、キーボード、方向キーを使用します。

    [![](hello-tvos-images/event3.png "場所の設定")](hello-tvos-images/event3.png#lightbox)
5. 部分メソッドは、次のようにこれ作成されます。

    [![](hello-tvos-images/event4.png "部分メソッド")](hello-tvos-images/event4.png#lightbox)

これで関数をボタンを使用するためのコードの追加を開始する準備が整いました。

### <a name="naming-a-control"></a>コントロールの名前を付ける

ラベルを更新する必要があります、ボタンがクリックされたときにクリックの回数に基づいて。 これを行うには、コード内のラベルにアクセスする必要があります。 これは、名前を付けることによって行います。 次の手順で行います。

1. ストーリー ボードを開き、ビュー コント ローラーの下部にラベルを選択します。
2. パッドでは、プロパティ、選択、**ウィジェット** タブ。

    [![](hello-tvos-images/name1.png "ウィジェット タブを選択します。")](hello-tvos-images/name1.png#lightbox)
3. **Identity > 名前**、追加`ClickedLabel`:

    [![](hello-tvos-images/name2.png "ClickedLabel を設定します。")](hello-tvos-images/name2.png#lightbox)

ラベルの更新を開始する準備が整いました。

### <a name="how-controls-are-accessed"></a>コントロールへのアクセス方法

選択した場合、`ViewController.designer.cs`で、**ソリューション エクスプ ローラー**を表示することができる方法、`ClickedLabel`ラベルと`Clicked`イベント ハンドラーにマップされた、**コンセント**と**アクション**C# の場合。

[![](hello-tvos-images/accesscontrol.png "コンセントとアクション")](hello-tvos-images/accesscontrol.png#lightbox)

場合もあります`ViewController.designer.cs`部分クラスでは、Visual Studio for Mac は、変更する必要があるないように`ViewController.cs`クラスに加え変更を上書きします。

これにより、UI 要素を公開するには、ビュー コント ローラーにアクセスすることができます。

通常必要はありませんを開くには、 `ViewController.designer.cs` 、自分でこれがここに示す教育する目的でのみです。

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>コードの記述

作成された、ユーザー インターフェイスとその UI 要素を使用してコードに公開されている**コンセント**と**アクション**、最後に、プログラムの機能を提供するコードを記述する準備ができました。

アプリケーションでは、最初のボタンをクリックするたびにお見せ、ボタンがクリックしてされた回数を表示する、ラベルを更新します。 これを実現する必要がありますを開くには、`ViewController.cs`ファイル内をダブルクリックして編集するため、**ソリューション パッド**:

[![](hello-tvos-images/code01.png "ソリューションのパッド")](hello-tvos-images/code01.png#lightbox)

クラス レベルの変数を作成する必要があります最初に、当社`ViewController`で発生したクリックの回数を追跡するクラス。 クラス定義を編集し、次のようにします。

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

次に、同じクラス (`ViewController`)、オーバーライドする必要があります、 **ViewDidLoad**メソッドをラベルの最初のメッセージを設定するコードを追加および。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

使用する必要があります`ViewDidLoad `などの別の方法ではなく`Initialize`ので、`ViewDidLoad `が呼び出された*後*OS が読み込まれ、ユーザー インターフェイスからインスタンス化、`.storyboard`ファイル。 前に、ラベル コントロールにアクセスしようとした場合、`.storyboard`お得られる、ファイルが完全に読み込まれ、インスタンス化、`NullReferenceException`エラー ラベル コントロールはまだ作成されないためです。

次に、ユーザーがボタンをクリックしてに応答するコードを追加する必要があります。 次の部分に追加クラスを作成しました。

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

このコードは、いつでも、ユーザーがこのボタンをクリックしたときに呼び出されます。

配置内のすべてのことをビルドして Xamarin.tvOS アプリケーションをテストする準備ができました。

## <a name="testing-the-application"></a>アプリケーションのテスト

ビルド、および期待どおりに実行を確認するようにアプリケーションを実行することをお勧めします。 構築、すべて 1 つのステップで実行したことができますか、実行せずに構築できます。

アプリケーションを構築するたびにするビルドの種類を選択できます。

-   **デバッグ**– にデバッグ ビルドをコンパイル、' デバッグ アプリケーションの実行中に何が起こっているかにより、追加のメタデータで (アプリケーション) ファイル。
-   **リリース**: また、リリース ビルドを作成、' のファイルに含まれていませんデバッグ情報が小さいことと、速く実行します。  

ビルドの種類を選択することができます、**構成セレクター** Visual Studio for Mac 画面の左上隅にあります。

[![](hello-tvos-images/run01.png "ビルドの種類を選択します。")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>アプリケーションのビルド

ここではデバッグ ビルドを希望ではことを確認**デバッグ**が選択されています。 いずれかのキーを押してアプリケーションを最初に構築してみましょう**⌘B**、または、**ビルド**] メニューの [選択**すべてビルド**です。

すべてのエラーがなかったため、わかります、**ビルドに成功した**Mac のステータス バーの Visual Studio でのメッセージ。 エラーがあった場合は、プロジェクトを確認し、手順が正しくに従っていることを確認してください。 コード (Mac 用の Visual Studio と Xcode で両方) が、チュートリアルでのコードと一致することを確認することによって開始します。

### <a name="running-the-application"></a>アプリケーションの実行

実行するには、アプリケーションは、3 つのオプションがあります。

-  **⌘+Enter** を押します。
-  **[実行]** メニューの **[デバッグ]** を選択します。
-  Visual Studio for Mac ツール バー (**ソリューション エクスプローラー**のすぐ上) の **[再生]** ボタンをクリックします。

(既にビルドされていない) 場合、アプリケーションのビルドは、開始してデバッグ モードでは、tvOS シミュレーターが起動し、アプリは起動して、インターフェイスのメイン ウィンドウを表示します。

[![サンプル アプリのホーム画面](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

**ハードウェア**メニュー選択**Apple TV のリモートの表示**シミュレーターを管理することができます。

[![](hello-tvos-images/run04.png "Apple テレビのリモコンの番組を選択します。")](hello-tvos-images/run04.png#lightbox)

何回かラベルのボタンをクリックした場合、シミュレーターのリモートを使用しては、カウントで更新する必要があります。

[![](hello-tvos-images/run05.png "更新された数のラベル")](hello-tvos-images/run05.png#lightbox)

おめでとうございます!  たくさんのここで説明したけれども、このチュートリアルを開始から終了までに従っている場合、Xamarin.tvOS アプリだけでなくその作成に使用できるツールのコンポーネントの理解が作成されました

## <a name="where-to-next"></a>次にどこですか。

Apple TV のアプリをいくつかの課題をユーザーと (ユーザーの手のひらではなく、部屋の向こう側の) のインターフェイスの制限事項 tvOS と切断のための開発は、アプリのサイズとストレージに配置します。

その結果、強くお勧めこと、読み取り、次を文書 Xamarin.tvOS アプリの設計にジャンプする前に。

- [TvOS 9 の概要](~/ios/tvos/platform/tvos9.md)– この記事では、すべての新しいまたは変更された Api と tvOS 9 で使用できる機能を紹介 Xamarin.tvOS 開発者向けです。
- [ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)– Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として ios 間でからでは間接的に、デバイスの画面で画像をタップした場所 Siri リモコンを使用してルーム。 この記事では、フォーカス イベントとその使用 Xamarin.tvOS アプリのユーザー インターフェイスでのナビゲーションを処理する方法の概念について説明します。
- [Siri のリモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md) – ユーザーが Apple TV Xamarin.tvOS アプリと対話は含まれる Siri リモコンを使用するための主要手段です。 アプリがゲームの場合は、必要に応じてで構築できます作成用のサード パーティのサポートの Bluetooth ゲーム コント ローラーの iOS (MFI) アプリにもします。 この記事では、Xamarin.tvOS アプリで新しい Siri リモート コンピューターと Bluetooth ゲーム コント ローラーのサポートについて説明します。
- [リソースとデータ記憶域](~/ios/tvos/app-fundamentals/resources-data-storage.md)– 新しい Apple TV の iOS デバイスとは異なり永続的でローカル ストレージが tvOS アプリの提供されません。 その結果、Xamarin.tvOS アプリがユーザーの設定) などの情報を保持する必要がある場合を格納し、iCloud からそのデータを取得する必要があります。 この記事では、リソースと Xamarin.tvOS アプリ内の永続的なデータ ストレージの操作について説明します。
- [アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)作成 – 魅了のアイコンと画像は、Apple TV のアプリの実体験のユーザー エクスペリエンスの開発の重要な部分です。 このガイドでは、作成し、Xamarin.tvOS アプリに必要なグラフィック アセットを挿入するに必要な手順を説明します。
- [ユーザー インターフェイス](~/ios/tvos/user-interface/index.md)– Xamarin.tvOS を操作するとき Xcode のインターフェイスのビルダーとユーザー エクスペリエンスの設計の原則をユーザー インターフェイス (UI) コントロールを含む一般的なユーザー エクスペリエンス (UX) カバレッジが使用されます。
- [配置およびテスト](~/ios/tvos/deploy-test/index.md)– このセクションのトピックが、配布する方法としてアプリをテストするために使用します。 トピックでは、デバッグ、テスト担当者および Apple TV のアプリ ストアへのアプリケーションを公開する方法への展開に使用するツールなどが含まれます。

Xamarin.tvOS で正常に実行する場合を参照してください、[トラブルシューティング](~/ios/tvos/troubleshooting.md)の一覧についてはドキュメントが問題と解決方法を把握します。

## <a name="summary"></a>まとめ

この記事では、簡単なこんにちは、tvOS のアプリを作成することで、Mac の tvOS の Visual Studio でのアプリを開発するクイック スタートが用意されています。 TvOS デバイスのプロビジョニング、インターフェイスの作成、tvOS のコーディングおよび tvOS シミュレーターでのテストの基礎を説明します。


## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (ビデオ) を使用した tvOS 用アプリの構築](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
