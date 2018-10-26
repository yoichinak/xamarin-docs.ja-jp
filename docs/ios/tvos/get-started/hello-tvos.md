---
title: はじめての tvOS クイック スタート ガイド
description: このガイドは、最初の Xamarin.tvOS アプリとその開発ツール チェーンの作成手順について説明します。 コードに UI コントロールを公開し、ビルド、実行、および Xamarin.tvOS アプリケーションをテストする方法を示しています。 Xamarin デザイナーも導入されています。
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 02/02/2018
ms.openlocfilehash: 8c8bf3f86091f49633913b37ef5108ddbae6d276
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115199"
---
# <a name="hello-tvos-quick-start-guide"></a>はじめての tvOS クイック スタート ガイド

_このガイドは、最初の Xamarin.tvOS アプリとその開発ツール チェーンの作成手順について説明します。コードに UI コントロールを公開し、ビルド、実行、および Xamarin.tvOS アプリケーションをテストする方法を示しています。 Xamarin デザイナーも導入されています。_

Apple に Apple TV の Apple TV 4 K、tvOS 11 を実行する 5 世代がリリースされました。

Apple TV プラットフォームは開発者は、多彩で魅力的なアプリを作成し、Apple TV の組み込みアプリ ストアからそれらをリリースすることができます。

Xamarin.iOS の開発に詳しい場合は、非常に単純な tvOS への移行が検索する必要があります。 Api と機能のほとんどは、同じ、ただし、多くの一般的な Api を利用できません (WebKit) など。 さらに、操作、Siri のリモートではタッチ スクリーン ベースの iOS デバイスに存在しないいくつかのデザインの課題をもたらします。

このガイドでは、Xamarin アプリで tvOS の操作の概要を提供します。 TvOS の詳細については、Apple を参照してください[Apple TV 4 K に備える](https://developer.apple.com/tvos/)ドキュメント。

## <a name="overview"></a>概要

Xamarin.tvOS で完全にネイティブ Apple TV のアプリを開発できますC#と同じ OS X ライブラリとで開発するときに使用されるインターフェイスのコントロールを使用して .NET *Swift* (または*Objective C*) と。*Xcode*します。

Xamarin.tvOS でアプリを記述からさらに、 C# Xamarin.iOS、Xamarin.Android、Xamarin.Mac アプリでは; を使用して .NET では、一般的なバックエンド コードを共有することができます同時に、各プラットフォームでネイティブ エクスペリエンスを提供します。

この記事で紹介する主要な概念を基本的な構築するためのプロセスを用いて Xamarin.tvOS と Visual Studio を使用して Apple TV のアプリを作成するために必要な**はじめての tvOS**回数がカウントされるアプリのボタンがありますクリックしてされました。

[![](hello-tvos-images/run05.png "例のアプリの実行")](hello-tvos-images/run05.png#lightbox)

次の概念について説明します。

-  **Visual Studio for Mac** – Visual Studio for Mac とられて Xamarin.tvOS アプリケーションを作成する方法の概要。
-  **Xamarin.tvOS アプリの構造**– どのような Xamarin.tvOS アプリで構成されます。
-  **ユーザー インターフェイスを作成する**– Xamarin iOS デザイナーを使用してユーザー インターフェイスを作成する方法。
-  **展開とテスト**– を実行し、実際の tvOS ハードウェアおよび tvOS シミュレーターでアプリをテストする方法。

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Visual studio for Mac 新しい Xamarin.tvOS アプリを起動します。

前述のようと呼ばれる、Apple TV のアプリを作成します`Hello-tvOS`メイン画面に 1 つのボタンとラベルを追加します。 ボタンがクリックされたとき、クリックされた回数がラベルに表示されます。

最初に、次の操作してみましょう。

1. Visual Studio for Mac を起動します。

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. をクリックして、**新しいソリューション.** リンクを開く画面の左上隅で、**新しいプロジェクト** ダイアログ ボックス。
3. 選択**tvOS** > **アプリ** > **単一ビュー アプリ** をクリックし、**次**ボタン。

    [![](hello-tvos-images/setup02.png "単一ビュー アプリを選択します。")](hello-tvos-images/setup02.png#lightbox)
4. 入力`Hello, tvOS`の**アプリ名**、入力、**組織 Id**  をクリックし、 **次へ**ボタン。

    [![](hello-tvos-images/setup04.png "はじめての tvOS を入力します。")](hello-tvos-images/setup04.png#lightbox)
5. 入力`Hello_tvOS`の**プロジェクト名** をクリックし、**作成**ボタン。

    [![](hello-tvos-images/setup03.png "HellotvOS を入力します。")](hello-tvos-images/setup03.png#lightbox)

Visual Studio for Mac は、新しい Xamarin.tvOS アプリを作成し、アプリケーションのソリューションに追加される既定のファイルが表示されます。

 [![](hello-tvos-images/project01.png "既定のファイルを表示します。")](hello-tvos-images/project01.png#lightbox)

Visual Studio for Mac で**ソリューション**と**プロジェクト**、Visual Studio がまったく同じ方法でします。 ソリューションは 1 つまたは複数のプロジェクトを保持できるコンテナーです。プロジェクトには、アプリケーション、サポート ライブラリ、テスト アプリケーションなどを含めることができます。この場合は、Visual Studio for Mac が作成、ソリューションとアプリケーション プロジェクトの両方をします。

たい場合は、1 つまたは複数のコードに共通の共有コードが含まれているライブラリ プロジェクトを作成できます。 これらのライブラリ プロジェクトをアプリケーション プロジェクトで使用または Xamarin.tvOS アプリの他のプロジェクトと共有できます (またはコードの種類に基づいて、Xamarin.iOS、Xamarin.Android、Xamarin.Mac)、標準の .NET アプリケーションを構築する場合と同様です。

## <a name="anatomy-of-a-xamarintvos-app"></a>Xamarin.tvOS アプリの構造

IOS プログラミングに慣れている場合、多くの類似点は、ここがわかります。 実際、tvOS 9 は、ここに多くの概念が交差するよう、iOS 9 のサブセットです。

プロジェクト内のファイルを見てみましょう。

-   `Main.cs` – このファイルには、アプリのメイン エントリ ポイントが含まれています。 アプリが起動した時点では、実行される最初のクラスとメソッドが含まれています。
-   `AppDelegate.cs` – このファイルには、オペレーティング システムからイベントをリッスンしているアプリケーションのメイン クラスが含まれています。
-   `Info.plist` – このファイルには、アプリケーション名、アイコンなどのアプリケーション プロパティが含まれています。
-   `ViewController.cs` – これは、メイン ウィンドウを表し、そのライフ サイクルを制御するクラスです。
-   `ViewController.designer.cs` – このファイルには、メイン画面のユーザー インターフェイスと統合するのに役立つプラミング コードが含まれています。
-  `Main.storyboard` – メイン ウィンドウの UI。 このファイルを作成、iOS 用の Xamarin デザイナーによって管理されることができます。

次のセクションでは、これらのファイルの一部を簡単に確認をみましょう。 さらに詳しく後で、それらについて説明しますが、これで、まず基本を理解することをお勧めします。

### <a name="maincs"></a>Main.cs

`Main.cs`ファイルには、静的なが含まれている`Main`新しい Xamarin.tvOS アプリ インスタンスを作成し、クラスの名前を渡すメソッドは OS のイベントを処理するこの例では、`AppDelegate`クラス。

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

`AppDelegate.cs`ファイルが含まれています、`AppDelegate`クラスは、これは、ウィンドウを作成して、OS イベントをリッスンする責任を負います。

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

このコードは、する前に、iOS アプリケーションを構築したが、非常に単純ですが、おなじみでしょうです。 重要な行を調べてみましょう。

最初に、クラスレベルの変数宣言を見てみましょう。

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window`プロパティは、メイン ウィンドウへのアクセスを提供します。 tvOS と呼ばれるものを使用して、*モデル ビュー コント ローラー* (MVC) パターンです。 一般に、すべてのウィンドウを作成する (およびその他の多くのことを windows 内で) はなど、新しいビュー (コントロール) を追加、表示するなど、ウィンドウのライフ サイクルは、コント ローラー。

次に、私たちが、`FinishedLaunching`メソッド。 このメソッドは、アプリケーションがインスタンス化し、は実際には、アプリケーション ウィンドウを作成すると、ビューを表示することでプロセスを開始後に実行されます。 アプリは、その UI を定義するストーリー ボードを使用するため、追加のコードは必要ありませんここで。

など、テンプレートに用意されている他の多くのメソッドがある`DidEnterBackground`と`WillEnterForeground`します。 これらを安全に取り外せるアプリケーション イベントは、アプリで使用されていない場合。

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController`クラスは、メイン ウィンドウのコント ローラー。 つまり、メイン ウィンドウのライフ サイクルを担います。 ここで詳細にこれを後で調査、ここではみましょうだけを簡単に見て。

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

今が空のメイン ウィンドウ クラスのデザイナー ファイルですは自動的に設定されます Visual Studio for Mac ように iOS Designer でのユーザー インターフェイスを作成します。

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

によって自動的に管理されている Visual studio for Mac とデザイナー ファイルは、通常は関係はありませんし任意のウィンドウに追加したり表示したりするアプリケーションでは、コントロールへのアクセスを許可するために必要なプラミング コードを指定します。

Xamarin.tvOS アプリを作成しましたし、そのコンポーネントの基本的な理解がある、次の UI の作成を見てみましょう。

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>ユーザー インターフェイスの作成

IOS 用の Xamarin のデザイナーを使用して Xamarin.tvOS アプリのユーザー インターフェイスを作成する必要はありません、UI から直接作成できますC#がコードはこの記事の範囲外です。 わかりやすく、使用する iOS Designer をこのチュートリアルの残りの部分で UI を作成します。

UI の作成を開始するをダブルクリックしましょう、`Main.storyboard`ファイル、**ソリューション エクスプ ローラー** iOS Designer での編集用に開きます。

[![](hello-tvos-images/designer01.png "ソリューション エクスプローラーの Main.storyboard ファイル")](hello-tvos-images/designer01.png#lightbox)

デザイナーを起動し、次のようになりますこのする必要があります。

[![](hello-tvos-images/designer02.png "デザイナー")](hello-tvos-images/designer02.png#lightbox)

詳細については、iOS デザイナーとそのしくみを参照してください、 [iOS 用の Xamarin のデザイナーの概要](~/ios/user-interface/designer/introduction.md)ガイド。

Xamarin.tvOS アプリのデザイン サーフェイスにコントロールを追加することを開始できます。

次の手順で行います。

1. 検索、**ツールボックス**、デザイン画面の右側にある必要があります。

    [![](hello-tvos-images/designer03.png "ツールボックス")](hello-tvos-images/designer03.png#lightbox)

    ここで検索することはできません場合を参照して**ビュー > パッド > ツールボックス**を表示します。
2. ドラッグ、**ラベル**から、**ツールボックス**デザイン サーフェイスに。

    [![](hello-tvos-images/designer04.png "ツールボックスからラベルをドラッグします。")](hello-tvos-images/designer04.png#lightbox)
3. をクリックして、**タイトル**プロパティ、**プロパティ パッド**ボタンのタイトルを変更し、`Hello, tvOS`設定と、**フォント サイズ**128 に。

    [![](hello-tvos-images/designer05.png "はじめての tvOS にタイトルを設定して、フォント サイズを 128 に設定します。")](hello-tvos-images/designer05.png#lightbox)
4. すべての単語が表示され、ウィンドウの上部中央に配置できるように、ラベルのサイズを変更します。

    [![](hello-tvos-images/designer06.png "サイズを変更し、ラベルの中央")](hello-tvos-images/designer06.png#lightbox)
5. ラベルを意図したとおりに表示されるように、ここでの位置に制約される必要があります。 画面のサイズに関係なく までのラベルには、クリックして、 *T の星形のハンドル*が表示されます。

    [![](hello-tvos-images/designer07.png "T 型ハンドル")](hello-tvos-images/designer07.png#lightbox)
6. ラベルを水平方向に制限するには、中央の正方形を選択し、垂直方向の点線にドラッグします。

    [![](hello-tvos-images/designer08.png "中央の正方形を選択します。")](hello-tvos-images/designer08zoom.png#lightbox)

     ラベルには、オレンジ色が有効にする必要があります。
7. ラベルの上部にある T ハンドルを選択し、ウィンドウの上端にドラッグします。

    [![](hello-tvos-images/designer09.png "ウィンドウの上端にハンドルをドラッグします。")](hello-tvos-images/designer09.png#lightbox)
8. 次に、幅と高さのをクリックして*ボーン ハンドル*下図のようにします。

    [![](hello-tvos-images/designer10.png "幅と高さのボーン ハンドル")](hello-tvos-images/designer10.png#lightbox)

     ときに各*ボーン ハンドル*がクリックすると、幅と高さの両方それぞれを固定サイズの設定を選択します。
9. 完了すると、制約する必要があります Properties pad の [レイアウト] タブと同様になります。

    [![](hello-tvos-images/designer11.png "例の制約")](hello-tvos-images/designer11.png#lightbox)
8. ドラッグ、**ボタン**から、**ツールボックス**し、ラベルの下に配置します。
9. をクリックして、**タイトル**プロパティ、**プロパティ パッド**ボタンのタイトルを変更および`Click Me`:

    [![](hello-tvos-images/designer12.png "ここをクリックするボタンのタイトルを変更します。")](hello-tvos-images/designer12.png#lightbox)
10. 5 ~ 8 tvOS ウィンドウで、ボタンを制限する上記の手順を繰り返します。 ただし、(手順 7) のように、ウィンドウの上部に T ハンドルをドラッグする代わりにドラッグ、ラベルの下。

    [![](hello-tvos-images/designer14.png "ボタンを制限します。")](hello-tvos-images/designer14.png#lightbox)
11. ボタンの下に別のラベルをドラッグして、最初のラベルとセットと同じ幅にサイズの**配置**に**Center**:

    [![](hello-tvos-images/designer15.png "ボタンの下に別のラベルをドラッグして、サイズを幅の最初のラベルと同じであり、アラインメントを中心に設定するには")](hello-tvos-images/designer15.png#lightbox)
12. 最初のラベルやボタンなどの中心や位置とサイズにピン留めするには、このラベルを設定します。

    [![](hello-tvos-images/designer16.png "位置とサイズにラベルをピン留め")](hello-tvos-images/designer16.png#lightbox)
13. ユーザー インターフェイスに変更を保存します。

サイズを変更するでコントロールが移動された、する必要がありますにお気付きこと、デザイナーを使用するスナップインを便利なヒントに基づく[Apple TV のヒューマン インターフェイス ガイドライン](https://developer.apple.com/tvos/human-interface-guidelines/)します。 次のガイドラインを高品質なアプリケーションを Apple TV のユーザーの使い慣れたルック アンド フィールを作成できます。

検索する場合、**ドキュメント アウトライン**セクションで、レイアウトと、ユーザー インターフェイスを構成する要素の階層を表示する方法に注意してください。

[![](hello-tvos-images/designer17.png "ドキュメント アウトライン セクション")](hello-tvos-images/designer17.png#lightbox)

ここからは、編集、または UI 要素の順序を変更して必要な場合にドラッグする項目を選択できます。 たとえば、UI 要素は、別の要素で覆われているが、ウィンドウの最上位の項目を一覧の一番下にはドラッグでした。

Xamarin.tvOS でアクセスしてで操作できるように UI 項目を公開する必要があります、ユーザー インターフェイスを作成したら、C#コード。

### <a name="accessing-the-controls-in-the-code-behind"></a>分離コード内のコントロールへのアクセス

コードから iOS デザイナーで追加したコントロールにアクセスする 2 つの主な方法はあります。

* コントロールのイベント ハンドラーを作成します。
* 後で参照できるように、名前、コントロールを提供します。

これらのいずれかが追加されたとき、内の部分クラス、`ViewController.designer.cs`変更を反映するように更新されます。 これによって、ビュー コント ローラー内のコントロールにアクセスできます。

### <a name="creating-an-event-handler"></a>イベント ハンドラーの作成

このサンプル アプリケーションで、ボタンがクリックされたときに必要_もの_起こるため、イベント ハンドラーが ボタンを特定のイベントに追加する必要があります。 これを設定するには、次の操作を行います。

1. Xamarin ios デザイナーでビュー コント ローラー上のボタンを選択します。
2. プロパティ パッドで、選択、**イベント** タブ。

    [![](hello-tvos-images/event1.png "[イベント] タブ")](hello-tvos-images/event1.png#lightbox)
3. TouchUpInside イベントを検索して、イベント ハンドラーをという名前を付けます`Clicked`:

    [![](hello-tvos-images/event2.png "TouchUpInside イベント")](hello-tvos-images/event2.png#lightbox)
4. 押したとき**Enter**、 **ViewController**.cs ファイルが開き、コードでイベント ハンドラーの場所を提案します。 キーボードの矢印キーを使用して、場所を設定します。

    [![](hello-tvos-images/event3.png "場所の設定")](hello-tvos-images/event3.png#lightbox)
5. これにより、次に示す部分メソッドが作成されます。

    [![](hello-tvos-images/event4.png "部分メソッド")](hello-tvos-images/event4.png#lightbox)

私たちは、ボタンの関数を許可するコードの追加を開始する準備ができました。

### <a name="naming-a-control"></a>コントロールの名前を付ける

ラベルを更新する必要があります、ボタンがクリックされたときに数回のクリックの数に基づきます。 これを行うには、コード内のラベルにアクセスする必要があります。 これは、名前を指定することによって行います。 次の手順で行います。

1. ストーリー ボードを開き、ビュー コント ローラーの下部にラベルを選択します。
2. プロパティ パッドで、選択、**ウィジェット** タブ。

    [![](hello-tvos-images/name1.png "[ウィジェット] タブを選択します。")](hello-tvos-images/name1.png#lightbox)
3. **Identity > 名前**、追加`ClickedLabel`:

    [![](hello-tvos-images/name2.png "ClickedLabel を設定します。")](hello-tvos-images/name2.png#lightbox)

ラベルの更新を開始する準備ができました!

### <a name="how-controls-are-accessed"></a>コントロールへのアクセス方法

選択した場合、`ViewController.designer.cs`で、**ソリューション エクスプ ローラー**表示することができる方法、`ClickedLabel`ラベルと`Clicked`イベント ハンドラーにマップされた、**アウトレット**と**アクション**でC#:

[![](hello-tvos-images/accesscontrol.png "Outlet と Action")](hello-tvos-images/accesscontrol.png#lightbox)

場合もあります`ViewController.designer.cs`部分クラスは、Visual Studio for Mac は、変更する必要があるないように`ViewController.cs`クラスに加え変更を上書きします。

この方法で UI 要素を公開するビュー コント ローラーでそれらにアクセスすることができます。

通常必要はありませんを開く、 `ViewController.designer.cs` 、自分でこれがここで紹介教育目的のみ。

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>コードの記述

作成された、ユーザー インターフェイスと UI 要素を使用してコードに公開される**Outlet**と**アクション**、最後に、プログラムの機能を提供するコードを記述する準備ができました。

アプリケーションでは、最初のボタンをクリックするたびにここ、ボタンがクリックしてされた回数を表示する、ラベルを更新します。 これを実現する必要がありますを開く、`ViewController.cs`ファイルをダブルクリックして編集、 **Solution Pad**:

[![](hello-tvos-images/code01.png "Solution Pad")](hello-tvos-images/code01.png#lightbox)

最初に、クラス レベル変数を作成する必要があります、`ViewController`クラスで発生したクリックの回数を追跡します。 クラス定義を編集し、次のようにします。

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

次に、同じクラス (`ViewController`)、オーバーライドする必要があります、 **ViewDidLoad**メソッドと、ラベルの初期メッセージを設定するコードを追加。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

使用する必要があります`ViewDidLoad `などの別のメソッドではなく`Initialize`ため、`ViewDidLoad `呼びます*後*、OS が読み込まれ、ユーザー インターフェイスをインスタンス化、`.storyboard`ファイル。 ラベル コントロールにする前にアクセスしようとした場合、`.storyboard`なります、ファイルが完全に読み込まれ、インスタンス化、`NullReferenceException`エラー ラベル コントロールはまだ作成されていないためです。

次に、ユーザーがボタンをクリックしてに応答するコードを追加する必要があります。 次の部分に追加クラスを作成しました。

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

このコードはいつでも、ボタンをクリックすると呼ばれます。

すべての準備をしますと、ビルド Xamarin.tvOS アプリケーションをテストして準備ができました。

## <a name="testing-the-application"></a>アプリケーションのテスト

時間をビルドして予想どおりに実行されるかどうかを確認するには、次のようにアプリケーションを実行することをお勧めします。 ビルドをすべて 1 つの手順で実行または実行せずに構築できます。

アプリケーションを構築しますたびにするビルドの種類を選択できます。

-   **デバッグ**– デバッグ ビルドをコンパイル、' (アプリケーション) ファイル、アプリケーションの実行中に何が起こっているかをデバッグできるようにする追加のメタデータ。
-   **リリース**– リリース ビルドを作成も、' のファイルに含まれないデバッグについては、ためサイズが小さく、高速化を実行しますに。  

ビルドの種類を選択することができます、**構成セレクター** Visual Studio for Mac 画面の左上隅にあります。

[![](hello-tvos-images/run01.png "ビルドの種類を選択します。")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>アプリケーションのビルド

ここではデバッグ ビルドのみが必要ではことを確認**デバッグ**が選択されています。 押すか、アプリケーションの最初にビルドしましょう**⌘ B**、または、**ビルド**] メニューの [選択**すべてビルド**します。

エラーがなければ場合が表示されます、**ビルドが成功しました**Mac のステータス バーの Visual Studio でのメッセージ。 エラーがあった場合は、プロジェクトをレビューし、手順が正常に従っているかどうかを確認します。 コード (Xcode と Visual Studio for Mac の両方) が、チュートリアルでは、コードと一致することを確認することによって開始します。

### <a name="running-the-application"></a>アプリケーションの実行

アプリケーションを実行するには、3 つのオプションがあります。

-  **⌘+Enter** を押します。
-  **[実行]** メニューの **[デバッグ]** を選択します。
-  Visual Studio for Mac ツール バー (**ソリューション エクスプローラー**のすぐ上) の **[再生]** ボタンをクリックします。

(既にビルドされていない) 場合、アプリケーションがビルド、デバッグ モードでは、シミュレーターの tvOS で start を開始、およびアプリが起動し、メイン インターフェイス ウィンドウを表示します。

[![サンプル アプリのホーム画面](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

**ハードウェア**メニューの **表示 Apple TV リモート**シミュレーターを制御できるようにします。

[![](hello-tvos-images/run04.png "Apple TV リモートの表示を選択します。")](hello-tvos-images/run04.png#lightbox)

シミュレーターのリモートを使用して何回かラベルのボタンをクリックする場合は、カウントで更新する必要があります。

[![](hello-tvos-images/run05.png "更新された数のラベル")](hello-tvos-images/run05.png#lightbox)

おめでとうございます! ここでは、さまざまな機能について説明しましたが、Xamarin.tvOS アプリとその作成に使用するツールのコンポーネントの理解がはず最初から最後までこのチュートリアルを実行した場合。

## <a name="where-to-next"></a>次の場所ですか。

Apple TV のアプリのユーザーと (ユーザーの手ではなく、部屋は) インターフェイスの制限事項の tvOS と切断のための課題をいくつかの開発は、アプリのサイズとストレージに配置します。

その結果、強くお勧めすること、読み取り、次を文書 Xamarin.tvOS アプリのデザインに進む前に。

- [TvOS 9 の概要](~/ios/tvos/platform/tvos9.md)– この記事では、すべての新規および変更した Api と tvOS 9 で使用できる機能を紹介 Xamarin.tvOS 開発者向け。
- [ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)– Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として ios デバイスの画面で、間接的にからイメージをタップした場所 Siri リモコンを使用してルーム。 この記事では、フォーカスと Xamarin.tvOS アプリのユーザー インターフェイスでのナビゲーションを処理するために使用される方法の概念について説明します。
- [Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md) – ユーザーの対話、Apple TV と Xamarin.tvOS アプリは、含まれる Siri のリモートを使用する主な方法です。 アプリがゲームの場合は、必要に応じてで構築できますサポートに加え、サード パーティ用に iOS (MFI) ゲームの Bluetooth コント ローラーもアプリ。 この記事では、Xamarin.tvOS アプリで、新しい Siri のリモート、および Bluetooth ゲーム コント ローラーのサポートについて説明します。
- [リソースとデータの記憶域](~/ios/tvos/app-fundamentals/resources-data-storage.md)– 新しい Apple TV の iOS デバイスとは異なり永続的でローカル ストレージが tvOS アプリの提供されません。 その結果、Xamarin.tvOS アプリがユーザー設定) などの情報を保持する必要がある場合を格納し、iCloud からそのデータを取得する必要があります。 この記事では、リソースと Xamarin.tvOS アプリでの永続的なデータ記憶域の使用について説明します。
- [アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)作成 – 魅惑的なアイコンと画像は、Apple TV のアプリの没入型のユーザー エクスペリエンスの開発の重要な部分です。 このガイドでは、作成し、Xamarin.tvOS アプリに必要なグラフィックス アセットを含めるに必要な手順を説明します。
- [ユーザー インターフェイス](~/ios/tvos/user-interface/index.md)– ユーザー インターフェイス (UI) コントロールなどの一般的なユーザー エクスペリエンス (UX) カバレッジを使用して、Xcode の Interface Builder と UX デザインの原則 Xamarin.tvOS を使用する場合。
- [展開とテスト](~/ios/tvos/deploy-test/index.md)– このセクションのトピックは、配布する方法についても、アプリをテストするために使用します。 このトピックでは、デバッグ、テスト担当者と、Apple TV App Store にアプリケーションを公開する方法へのデプロイに使用されるツールなどが含まれます。

Xamarin.tvOS で正常に実行する場合を参照してください、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ドキュメントの一覧を既知の問題とソリューションです。

## <a name="summary"></a>まとめ

この記事では、Mac の簡単なこんにちは, tvOS アプリを作成して Visual Studio での tvOS アプリを開発するクイック スタートを提供します。 TvOS デバイス インターフェイスの作成、プロビジョニング、tvOS のコーディングおよび tvOS シミュレーターでのテストの基本を説明しました。


## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [(ビデオ) Xamarin で tvOS のアプリの構築](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
