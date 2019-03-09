---
title: Xamarin.iOS でストーリー ボードの概要
description: このドキュメントでは、Xamarin.iOS でストーリー ボードの概要を示します。 ストーリー ボードを使用してユーザー インターフェイスを定義する方法について説明します。 これは、セグエ、および iOS Designer を使用して、ストーリー ボード ファイルを編集する方法。
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
---

# <a name="introduction-to-storyboards-in-xamarinios"></a>Xamarin.iOS でストーリー ボードの概要

このガイドでは、どのようなストーリー ボードを説明しましたが、主要なコンポーネント – Segues などのいくつかを確認します。 ストーリー ボードを作成して、使用する方法について説明しますおよび開発者がどのような利点があります。

ストーリー ボード ファイルの形式は、iOS アプリケーションの UI の視覚的表現として、Apple によって導入された、前に、開発者は各ビュー コント ローラー用の XIB ファイルを作成し、それぞれのビュー間のナビゲーションのプログラムを手動で作成します。  ストーリー ボードを使用して、ビュー コント ローラーと、デザイン サーフェイスにそれらの間のナビゲーションを定義する、開発者し、により、アプリケーションのユーザー インターフェイスの WYSIWYG 編集します。

ストーリー ボードの作成、開くおよび Xamarin iOS Designer で編集できます。 このガイドもチュートリアル デザイナーを使用して c# プログラムのナビゲーションを使用しているときに、ストーリー ボードを作成する方法。


## <a name="requirements"></a>必要条件

ストーリー ボードは、Xamarin のワークロードがインストールされている iOS Visual Studio for Mac で Designer と Visual Studio 2015 と 2017 使用できます。

## <a name="what-is-a-storyboard"></a>ストーリー ボードとは何ですか。

ストーリー ボードとは、アプリケーションでのすべての画面のビジュアル表現です。 各シーンを表すと、シーンのシーケンスが含まれている、*ビュー コント ローラー*とその*ビュー*します。 これらのビューは、オブジェクトを含めることができますと[コントロール](~/ios/user-interface/controls/index.md)ユーザー、アプリケーションとの対話を行うことができます。 このビューとコントロールのコレクション (または*サブビュー*) と呼ばれますが、*コンテンツ ビュー階層*します。 シーンが接続されている、セグエ ビュー コント ローラーの間の遷移を表すオブジェクト。 通常は初期のビューのオブジェクトと接続するビューの間のセグエを作成して実現します。 デザイン サーフェイス上の関係は、次の図に示します。

 [![](images/storyboardsview.png "この図に、デザイン サーフェイス上の関係を示します")](images/storyboardsview.png#lightbox)

に示すように、ストーリー ボードは既にレンダリングされたコンテンツで、シーンの各をレイアウトし、両者間の接続を示しています。  Iphone のシーンについて説明、するも安全であることを想定するこの時点では、注目に値します*シーン*ストーリー ボードが 1 に等しい*画面*デバイス上のコンテンツ。 ただし、一度に – 表示する複数のシーンを含めることは可能が iPad でなどポップ オーバー ビュー コント ローラーを使用します。

ストーリー ボードを使用して、Xamarin を使用する場合に特にアプリケーションの UI を作成する多くの利点があります。 含む – すべてのオブジェクトとして、UI のビジュアル表現はまず、[カスタム コントロール](~/ios/user-interface/designer/ios-designable-controls-overview.md): デザイン時に表示されます。 つまり、構築またはその外観とフローを視覚化するアプリケーションをデプロイする前にします。 たとえば、上の画像を考えてみましょう。 デザイン サーフェス数では、各ビューのレイアウトおよびすべての関係を簡単に見てわかります。 これは、ストーリー ボードを非常に強力な点です。

イベントは、特に iOS Designer を使用する場合、ストーリー ボードより管理しやすい。 ほとんどの UI コントロール プロパティ パッドで使用できるイベントの一覧になります。 イベント ハンドラーをここに追加し、ビュー コント ローラー クラス内の部分メソッドが完了しました.

ストーリー ボードのコンテンツは、XML ファイルとして格納されます。 ビルド時にすべて`.storyboard`nibs と呼ばれるバイナリ ファイルにファイルがコンパイルされます。 実行時にこれら nibs が初期化され、新しいビューを作成するインスタンス化します。

## <a name="segues"></a>セグエ

A*セグエ*、または*セグエ オブジェクト*シーンの間の遷移を表すために iOS 開発に使用します。 セグエを作成するキーを押し、 **Ctrl**キーと別に 1 つのシーンからクリック ドラッグします。 マウスをドラッグしたのとは、次の図に示すように、セグエが潜在顧客場所を示す青色コネクタが表示されます。

 [![](images/createsegue.png "青色コネクタが表示されたら、この画像に示すように、セグエが潜在顧客場所を示す")](images/createsegue.png#lightbox)

時に、マウス、セグエのアクションを選択させて、メニューが表示されます。 次の画像のように表示可能性があります。 

**前の iOS 8 とサイズ クラス**:

[![](images/segue1.png "サイズ クラスなしアクション セグエ ドロップダウン")](images/segue1.png#lightbox)

**サイズ クラスとアダプティブ セグエを使用する場合**:

[![](images/16new.png "サイズ クラスを含むアクション セグエ ドロップダウン")](images/16new.png#lightbox)

> [!IMPORTANT]
> Ctrl-クリックとしてマップを使用する VMWare の Windows 仮想マシンの場合、_を右クリックして_既定でマウス ボタンをクリックします。 セグエを作成するには、を介して、キーボード設定を編集**設定** > **キーボードおよびマウス** > **マウスのショートカット**再マップして、**セカンダリ ボタン**下図のようにします。
> 
> [![](images/image22.png "キーボードとマウスの設定")](images/image22.png#lightbox)
> 
> 通常どおり、ビュー コント ローラー間のセグエを追加できるようになりましたにします。

さまざまな種類の遷移、各ユーザーに新しいビュー コント ローラーを表示する方法と、ストーリー ボードでその他のビュー コント ローラーとの対話方法に与えること制御があります。 これらは、以下について説明します。 カスタム遷移を実装するために、セグエ オブジェクトをサブクラス化することもします。

-  **表示/プッシュ**– プッシュ セグエ ビュー コント ローラーをナビゲーション スタックに追加します。 元のプッシュ ビュー コント ローラーは、スタックに追加されているビュー コント ローラーとして同じナビゲーション コント ローラーの一部と見なします。 これと同じもの`pushViewController`、通常、画面上のデータ間のリレーションシップがあるときに使用するとします。 セグエ、プッシュを使用して、ナビゲーション バー [戻る] ボタンとナビゲーション ビュー階層をドリルダウンできるように、スタック上の各ビューに追加するタイトルを使用する余裕ができます。
-  **モーダル**– モーダル セグエは、表示されているアニメーションの遷移のオプションを使用して、プロジェクト内の任意の 2 つのビュー コント ローラー間のリレーションシップを作成します。 親ビューのコント ローラーのビューに読み込まれると完全に子ビュー コント ローラーの邪魔になります。 セグエをプッシュとは異なりを使用します。 [戻る] ボタンを追加します。いつ、セグエ モーダルを使用して`DismissViewController`前のビュー コント ローラーに返すために使用する必要があります。
-  **カスタム**– custom 任意のサブクラスとしてセグエを作成できます` UIStoryboardSegue`します。
-  **アンワインド**– アンワインド セグエ プッシュまたはモーダルを後方に移動することできますセグエ – たとえば、モーダルとして表示されるビュー コント ローラーを閉じる。 さらに、1 つだけでなくをアンワインドできますが、一連のプッシュとモーダル セグエしアクションのアンワインドを 1 つのナビゲーション階層内の複数の手順を戻っています。 読み取り、iOS でアンワインド セグエを使用する方法を理解する、[セグエ アンワインド作成](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue)レシピです。
-  **ソースレス**– ソースレス セグエ初期ビュー コント ローラーを含むシーンを示し、したがってユーザーがどのビューが最初に表示します。 次に示すセグエによって表されます。  

    [![](images/sourcelesssegue.png "ソースレス セグエ")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>アダプティブ セグエの種類

 iOS 8 の導入[サイズ クラス](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes)iOS ストーリー ボード ファイルすべての iOS デバイスの 1 つの UI を作成する開発者を有効にすると、すべての利用可能な画面サイズに対応するようにします。 既定では、すべての新しい Xamarin.iOS アプリケーションはサイズ クラスを使用します。 以前のプロジェクトからサイズ クラスを使用して、参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ガイド。 
 
サイズ クラスを使用するアプリケーションは、新しい使用も[*アダプティブ セグエ*](~/ios/user-interface/storyboards/unified-storyboards.md)します。 サイズ クラスを使用する場合と、されるかどうかは直接指定することに注意してください、iPhone または iPad を使用しています。 つまりを使用する必要がある不動産量に関係なく、同じになります常に 1 つの UI 作成しています。 環境を審査してコンテンツを提示する最善の方法を決定するアダプティブ Segues 作業します。 アダプティブ セグエは、以下に示します。 

[![](images/adaptivesegue.png "アダプティブ セグエ ドロップダウン")](images/adaptivesegue.png#lightbox)

|セグエ|説明|
|--- |--- |
|[表示]|非常に似ています、プッシュがセグエがアカウントに画面のコンテンツを取得します。|
|詳細の表示|アプリには、(たとえば、iPad で分割ビュー コント ローラー) でマスター/詳細ビューが表示されている場合、コンテンツは詳細の表示を置き換えます。 アプリでは、マスターまたは詳細を表示する場合、コンテンツはビュー コント ローラーのスタックの先頭を置き換えます。|
|Presentation|これはモーダルのセグエに似ており、プレゼンテーションと遷移のスタイルを選択できるようにします。|
|ポップ オーバー プレゼンテーション|コンテンツは、ポップ オーバーとして表示します|

### <a name="transferring-data-with-segues"></a>セグエを使用してデータを転送します。

セグエの利点は、遷移で終了しません。 また、ビュー コント ローラー間でデータの転送を管理することも使用できます。 オーバーライドすることでこれは、`PrepareForSegue`初期ビュー コント ローラー コンピューターと自分たちのデータを処理するメソッド。 セグエがトリガーされると – などで、ボタンを押す – アプリケーションはメソッドを呼び出すこの、新しいビュー コント ローラーを準備する機会を提供する*する前に*ナビゲーションが発生します。 次のコードから、 [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)サンプルをこの例を示します。 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
}
```

この例で、`PrepareForSegue`セグエは、ユーザーによってトリガーされたときにメソッドが呼び出されます。 まず、'受信' のビュー コント ローラーのインスタンスを作成し、これをセグエの対象ビュー コント ローラーとして設定する必要があります。 これは、次のコード行で行います。

```csharp
var callHistoryController = segue.DestinationViewController as CallHistoryController;
```

メソッドには、プロパティを設定する機能、`DestinationViewController`します。 この例と呼ばれるリストを渡すことでこの方法の利点を思い出させて`PhoneNumbers`を`CallHistoryController`と同じ名前のオブジェクトに割り当てます。

```csharp
if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
```

移行が完了すると、ユーザーが表示されます、`CallHistoryController`で表示されたリスト。

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>非ストーリー ボードのプロジェクトをストーリー ボードを追加します。

場合によっては、以前ストーリー ボード ファイルにストーリー ボードを追加する必要があります。 次の手順に従って 1 回でこれを行う Visual Studio for Mac を効率化されることができます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 参照して新しいストーリー ボード ファイルを作成**ファイル > 新規ファイル > iOS > ストーリー ボード**以下に示すように。 
    
    [![](images/new-storyboard-xs.png "新しいファイル ダイアログ")](images/new-storyboard-xs.png#lightbox)

2. ストーリー ボード名を追加、**メイン インターフェイス**のセクション、 **Info.plist**以下に示すように。
    
    [![](images/infoplist.png "Info.plist エディター")](images/infoplist.png#lightbox)
    
    これにより、初期のビュー コント ローラーでインスタンス化するのと同じ、`FinishedLaunching`アプリ デリゲート内のメソッド。 このオプションを設定して、アプリケーション ウィンドウ (下記参照) をインスタンス化、メインのストーリー ボードを読み込みますおよびとしてストーリー ボードの Initial View Controller (ソースレス セグエの横にある 1 つ) のインスタンスを割り当てます、`RootViewController`ウィンドウはのプロパティ画面に表示するウィンドウです。

3. `AppDelegate`、既定のオーバーライド`Window`をウィンドウのプロパティを実装する次のコードのメソッド。
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 新しいストーリー ボード ファイルを作成するプロジェクトを右クリックして**追加 > 新しいファイル > iOS > の空の Storyboard**以下に示すように。 
    
    [![](images/new-storyboard-vs.png "新しい項目 ダイアログ ボックス")](images/new-storyboard-vs.png#lightbox)

2. ストーリー ボード名を追加、**メイン インターフェイス**の iOS アプリケーションを次に示すセクション。
    
    [![](images/ios-app.png "Info.plist エディター")](images/ios-app.png#lightbox)
    
    これにより、初期のビュー コント ローラーでインスタンス化するのと同じ、`FinishedLaunching`アプリ デリゲート内のメソッド。 このオプションを設定して、アプリケーション ウィンドウ (下記参照) をインスタンス化、メインのストーリー ボードを読み込みますおよびとしてストーリー ボードの Initial View Controller (ソースレス セグエの横にある 1 つ) のインスタンスを割り当てます、`RootViewController`ウィンドウはのプロパティ画面に表示するウィンドウです。

3. `AppDelegate`、既定のオーバーライド`Window`をウィンドウのプロパティを実装する次のコードのメソッド。

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>IOS Designer でストーリー ボードを作成します。

IOS では、統合されたシームレスに Visual studio for Mac と Visual Studio の Xamarin デザイナーを使用して、ストーリー ボードを作成できます。

次のストーリー ボードを作成する iOS Designer を使用して開始するには[こんにちは, iOS マルチ スクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)ガイド。 このチュートリアルでは、Segues と、コントロールのイベントを処理する方法を使用してビュー コント ローラー間でナビゲーションを説明します。

## <a name="instantiate-storyboards-manually"></a>ストーリー ボードを手動でインスタンス化します。

ストーリー ボードはプロジェクトで、個々 の XIB ファイルを完全に置換を使用して、ストーリー ボードで個別のビュー コント ローラーをインスタンス化できますが`Storyboard.InstantiateViewController`します。

場合によってアプリケーションには、デザイナーによって提供される組み込みのストーリー ボードの遷移を処理することはできません特別な要件がないです。 など、アプリケーションの現在の状態に応じて、同じボタンから別の画面を起動するアプリケーションを作成する場合、ビュー コント ローラーを手動でインスタンス化し、自分たちの移行をプログラミングするたい場合があります。

次のスクリーン ショットでは、それらの間のセグエをデザイン サーフェイス上の 2 つのビュー コントなしでは示しています。 次のセクションは、コードでその遷移設定する方法を説明します。

 [![](images/viewcontrollerspink.png "このスクリーン ショットは、それらの間のセグエをデザイン サーフェイス上の 2 つのビュー コントなしで")](images/viewcontrollerspink.png#lightbox)

1. 追加、_空の iPhone ストーリー ボード_既存プロジェクトのプロジェクトに。
    
    [![](images/add-storyboard1.png "ストーリー ボードを追加します。")](images/add-storyboard1.png#lightbox)

2. 開き、新しく作成されたストーリー ボードをダブルクリックし、新しい追加**ナビゲーション コント ローラー**デザイン サーフェイスにします。 ナビゲーション コント ローラーは、次のように、ルート ビュー コント ローラーとなる既定では、UI のないです。

    [![](images/uinavigationcontroller.png "ビュー コント ローラーをセグエします。")](images/uinavigationcontroller.png#lightbox)

3. 選択、_ビュー コント ローラー_を下部にある黒いバーをクリックします。 デザイナーの**プロパティ パッド** **Identity**ビュー コント ローラーのカスタム クラスと一意の ID を指定できます。 設定、**クラス名**と**ストーリー ボード ID**に`MainViewController`します。

    [![](images/identitypanelnew.png "カスタム クラスを指定します。")](images/identitypanelnew.png#lightbox)

4. ストーリー ボードから、ビュー コント ローラーをインスタンス化する必要がありますが、後でし、ストーリー ボード ID を使用して、コードでそれらを参照します。 ストーリー ボード ID と一致する復元の ID を設定すると、状態を復元する必要があるビュー コント ローラーが正しく再作成取得ことによってください。

5. 現在のみある 1 つのビュー コント ローラー。 別のビュー コント ローラーをデザイン画面にドラッグします。 **プロパティ パッド**、Id を クラスとストーリー ボード ID を設定`PinkViewController`以下に示すように。

    [![](images/pinkvcnew.png "プロパティ パッド")](images/pinkvcnew.png#lightbox)
    
    IDE では、これらのビュー コント ローラーのカスタム クラスを作成します。 これらで表示できます、 **Solution Pad**、次のスクリーン ショットに示すようにします。
    
    [![](images/solution-pad.png "Solution Pad")](images/solution-pad.png#lightbox)

6. `PinkViewController`、コント ローラーのフレームの中心をクリックして、ビューを選択します。 プロパティ パッドで、[表示] を変更、**バック グラウンド**マゼンタに。
    
    [![](images/pinkcontroller.png "背景色を設定")](images/pinkcontroller.png#lightbox)

7. 最後に、ボタンをドラッグして、**ツールボックス**上に、`MainViewController`します。 プロパティ パッドで名前を付けます`PinkButton`とタイトル GoToPink、下図のようにします。

    [![](images/pinkbutton.png "ボタンの名前を設定します。")](images/pinkbutton.png#lightbox)

ストーリー ボードが完了するには、空の画面をいたす場合は、プロジェクトをすぐに展開します。 最初のビューとして使用するルート ビュー コント ローラーを設定して、ストーリー ボードを使用する IDE に指示する必要があるためにです。 通常これ行う、プロジェクトのオプションの上に示すようにします。 次のコードを追加することで、コードでは、同じ結果を得ることがこの例ではただし、 **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

多くのコードしますが、数行のみに慣れていません。 最初に、登録、ストーリー ボードに、 **AppDelegate**ストーリー ボードの名前を渡すことによって**MainStoryboard**します。 次に、呼び出すことで、ストーリー ボードから、最初のビュー コント ローラーをインスタンス化するアプリケーションに伝えます`InstantiateInitialViewController`、ストーリー ボードで、アプリケーションのルート ビュー コント ローラーとしてそのビュー コント ローラーを設定します。 このメソッドは、ユーザーに表示される最初の画面を決定し、そのビュー コント ローラーの新しいインスタンスを作成します。

IDE が作成されたソリューション ウィンドウで、`MainViewcontroller.cs`クラス、およびその`corresponding designer.cs`手順 4. で Properties Pad にクラス名を追加しました。 このクラスの基底クラスを含む特殊なコンス トラクターを作成することがわかります。

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


IDE が自動的に追加されているデザイナーを使用してストーリー ボードを作成するときに、 [[登録]](xref:Foundation.RegisterAttribute)の上部にある属性、`designer.cs`クラスし、で指定されたストーリー ボード ID と同じですが、文字列の識別子を渡す、前の手順。 C# はリンクこのストーリー ボードに関連するシーンにします。

ある時点であった既存のクラスを追加したい場合があります**いない**デザイナーで作成します。 ここではこのクラスを通常どおり登録。

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

クラスとメソッドを登録する方法の詳細についてを参照してください、[型レジストラー](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/)ドキュメント。

このクラスの最後の手順で、ボタンとピンクのビュー コント ローラーへの移行を結び付けます。 インスタンスを作成しますが、 `PinkViewController` ; ストーリー ボードから、私たちはプログラムを実行、プッシュのセグエ`PushViewController`以下のコード例に示しますように。

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

アプリケーションを実行するには、2 画面アプリケーションが生成されます。

![](images/finishedstoryboard.png "サンプル アプリの画面の実行")

## <a name="conditional-segues"></a>条件付きをセグエします。

多くの場合、1 つのビュー コント ローラーから、[次へ] への移行は、特定の条件によって異なります。 たとえば、単純なログイン画面を実行した場合はだけ使用します次の画面に移動する*場合*ユーザー名とパスワードが確認されました。

次の例では、パスワード フィールド上のサンプルを追加します。 ユーザーがアクセスできる、 *PinkViewController*場合、正しいパスワードを入力すると、それ以外の場合、エラーが表示されます。

始める前に、1 ~ 8 の上記の手順に従います。 次の手順での ストーリー ボードを作成、UI の作成し、アプリケーション デリゲートを使用するビュー コント ローラーの通知を開始しました RootViewController です。

1. 次に、UI を構築して追加するには、追加のビュー、`MainViewController`次のスクリーン ショットのような外観にすること。

    - UITextField
        - 名前:PasswordTextField
        - プレース ホルダー:' シークレットのパスワードを入力 '
    - UILabel
        - テキスト:' エラー。間違ったパスワード。 渡さないでください '。
        - 色：赤
        - 配置:中央揃え
        - 行:2
        - 'Hidden' チェック ボックスはオン 
        
    [![](images/passwordvc.png "中心線")](images/passwordvc.png#lightbox)
    
2. ピンクに移動 ボタンと Ctrl - からドラッグして、ビュー コント ローラーの間のセグエを作成、 *PinkButton*を*PinkViewController*を選択して**プッシュ**マウスに. 

3. セグエをクリックし、*識別子* `SegueToPink`:

    [![](images/namesegue.png "セグエをクリックし、識別子 SegueToPink を付けます")](images/namesegue.png#lightbox)  
    

4. 最後には、次の ShouldPerformSegue メソッドを追加、`MainViewController`クラス。

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

このコードに segueIdentifier が一致しましたが、`SegueToPink`セグエ、条件をテストできるようにここでは有効なパスワード。 場合は、条件の戻り値`true`、セグエは実行し、表示されます、`PinkViewController`します。 場合`false`、新しいビュー コント ローラーは表示されません。

SegueIdentifier ShouldPerformSegue メソッドの引数をチェックして、このビュー コント ローラー上のすべてのセグエにこのアプローチを適用できます。 ここで 1 つのセグエ識別子 – あるのみ`SegueToPink`します。

Storyboards.Conditional ソリューションを参照してください、[手動のストーリー ボードのサンプル](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/)実施例についてはします。

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>ストーリー ボードの参照の使用

ストーリー ボードの参照を使用すると、大規模で複雑なストーリー ボードのデザインを受け取り、小さいストーリー ボード、元の参照を取得するため複雑さを削除して、結果として得られる個別のストーリー ボードを簡単に設計および管理に分割することができます。

また、ストーリー ボードの参照を提供できます、_アンカー_同じストーリー ボードまたは別の特定のシーン内の別のシーンにします。

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>外部のストーリー ボードを参照します。

外部のストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**プロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**  >  **iOS** > **ストーリー ボード**します。 入力、**名前**新しいストーリー ボードをクリックして、**新規**ボタン。
    
    [![](images/ref01.png "新しいファイル ダイアログ")](images/ref01.png#lightbox)
    
2. 通常は、変更を保存、新しいストーリー ボードのシーンのレイアウトをデザインします。 
    
    [![](images/ref02.png "新しいシーンのレイアウト")](images/ref02.png#lightbox)
    
3. IOS Designer への参照を追加すること、ストーリー ボードを開きます。

4. ドラッグ、**の参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 
    
    [![](images/ref03.png "ストーリー ボードの参照")](images/ref03.png#lightbox)
    
5. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**の名前を選択、**ストーリー ボード**上記で作成します。 

    [![](images/ref04.png "[ウィジェット] タブ")](images/ref04.png#lightbox)
    
6. (ボタン) のように既存のシーンでの UI ウィジェットをコントロール クリックしを新しいセグエを作成、**ストーリー ボードにリファレンス**作成しました。 

    [![](images/ref05.png "セグエを作成します。")](images/ref05.png#lightbox) 
    
7. ポップアップ メニューから選択**表示**セグエを完了します。 

    [![](images/ref06.png "セグエを完了するの表示 を選択")](images/ref06.png#lightbox) 
    
8. ストーリー ボードに変更を保存します。

アプリが実行され、セグエをストーリー ボードの参照で指定された外部のストーリー ボードから初期ビュー コント ローラーから作成した UI 要素に、ユーザーがクリックしたときに表示されます。

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>外部のストーリー ボードで特定のシーンを参照します。

特定のシーンへの参照を追加するには、外部のストーリー ボード (および最初のビュー コント ローラー以外) が、次、実行します。

1. **ソリューション エクスプ ローラー**、外部のストーリー ボードを開き、編集をダブルクリックします。

2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [![](images/ref07.png "新しいシーンのレイアウト")](images/ref07.png#lightbox)
    
3. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**、入力、**ストーリー ボード ID**新しいシーンのビュー コント ローラー。 

    [![](images/ref08.png "新しいシーン ビュー コント ローラーのストーリー ボード ID を入力します。")](images/ref08.png#lightbox)
    
3. IOS Designer への参照を追加すること、ストーリー ボードを開きます。

4. ドラッグ、**の参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 

    [![](images/ref03.png "ストーリー ボードの参照")](images/ref03.png#lightbox)
    
5. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**の名前を選択、**ストーリー ボード**と**参照 ID** (ストーリー ボード ID) の上記で作成したシーンの場合: 

    [![](images/ref09.png "[ウィジェット] タブ ")](images/ref09.png#lightbox)
    
6. (ボタン) のように既存のシーンでの UI ウィジェットをコントロール クリックしを新しいセグエを作成、**ストーリー ボードにリファレンス**作成しました。 

    [![](images/ref10.png "セグエを作成します。")](images/ref10.png#lightbox) 
    
7. ポップアップ メニューから選択**表示**セグエを完了します。 

    [![](images/ref06.png "セグエを完了するの表示 を選択")](images/ref06.png#lightbox) 
    
8. ストーリー ボードに変更を保存します。

セグエをシーンにから作成した UI 要素をクリックする、アプリが実行され、ユーザー、特定**ストーリー ボード ID**からストーリー ボードの参照で指定された外部のストーリー ボードが表示されます。

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>同じストーリー ボードの特定のシーンを参照します。

特定のシーンと同じストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ストーリー ボードを開き、編集をダブルクリックします。

2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [![](images/ref11.png "新しいシーンのレイアウト")](images/ref11.png#lightbox)

3. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**、入力、**ストーリー ボード ID**新しいシーンのビュー コント ローラー。 

    [![](images/ref12.png "[ウィジェット] タブ")](images/ref12.png#lightbox)
    
3. ドラッグ、**の参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 

    [![](images/ref03.png "ストーリー ボードの参照")](images/ref03.png#lightbox)
    
5. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を選択します**参照 ID** (ストーリー ボード ID) 上で作成したシーンの。 

    [![](images/ref13.png "[ウィジェット] タブ")](images/ref13.png#lightbox)
    
6. (ボタン) のように既存のシーンでの UI ウィジェットをコントロール クリックしを新しいセグエを作成、**ストーリー ボードにリファレンス**作成しました。 

    [![](images/ref14.png "セグエを作成します。")](images/ref14.png#lightbox) 
    
7. ポップアップ メニューから選択**表示**セグエを完了します。 

    [![](images/ref06.png "セグエを完了するの表示 を選択")](images/ref06.png#lightbox) 
    
8. ストーリー ボードに変更を保存します。

セグエをシーンにから作成した UI 要素をクリックする、アプリが実行され、ユーザー、特定**ストーリー ボード ID**でストーリー ボードの参照で指定された同じストーリー ボードが表示されます。

## <a name="summary"></a>まとめ

この記事では、ストーリー ボードと方法が有益である iOS アプリケーションの開発の概念について説明します。 これは、シーン、ビュー コント ローラー、ビューとビュー階層、およびさまざまな種類の Segues と共にシーンをリンクする方法について説明します。  についても検討ビュー コント ローラーをインスタンス化する手動でストーリー ボード、および作成の条件付き Segues から。



## <a name="related-links"></a>関連リンク

- [手動のストーリー ボード (サンプル)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [IOS Designer の概要](~/ios/user-interface/designer/introduction.md)
- [ストーリー ボードへの変換](https://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard クラスのリファレンス](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
