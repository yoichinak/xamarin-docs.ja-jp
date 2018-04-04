---
title: .storyboard/.xib-less ユーザー インターフェイスの設計
description: この記事では、c# コード、.storyboard ファイル、.xib ファイル、またはインターフェイスのビルダーせずから直接 Xamarin.Mac アプリケーションのユーザー インターフェイスを作成するについて説明します。
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 66725b02d3e351e74fa79ae5336a7db3a9f2b534
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>.storyboard/.xib-less ユーザー インターフェイスの設計

_この記事では、c# コード、.storyboard ファイル、.xib ファイル、またはインターフェイスのビルダーせずから直接 Xamarin.Mac アプリケーションのユーザー インターフェイスを作成するについて説明します。_


## <a name="overview"></a>概要

同じユーザー インターフェイス要素にアクセスし、ツールで作業する開発者 Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき*Objective-C*と*Xcode*はします。 通常、Xamarin.Mac アプリケーションを作成するときに使用する Xcode のインターフェイスのビルダー .storyboard または .xib ファイルで作成および管理するアプリケーションのユーザー インターフェイス。

また、c# コードで直接、Xamarin.Mac アプリケーションの UI の一部またはすべてを作成するオプションがあります。 この記事で c# コードでのユーザー インターフェイスと UI 要素の作成の基本について説明します。

[![Mac コード エディター用の Visual Studio](xibless-ui-images/intro01.png "Visual Studio for Mac コード エディター")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>コードを使用するウィンドウの切り替え

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、 **Main.storyboard** (または従来、 **MainWindow.xib**) プロジェクトに自動的に含まれるファイル。 これも含まれています、 **ViewController.cs**アプリのメイン ビューを管理するファイル (または再度従来、 **MainWindow.cs**と**MainWindowController.cs**ファイル)。

アプリケーションの Xibless ウィンドウを切り替えるには、次の操作を行います。

1. 使用を停止するアプリケーションを開く`.stroyboard`または Visual studio for mac ユーザー インターフェイスを定義する .xib ファイル
2. **ソリューション パッド**を右クリックし、 **Main.storyboard**または**MainWindow.xib**ファイルおよび選択した**削除**: 

    ![メインのストーリー ボードまたはウィンドウを削除する](xibless-ui-images/switch01.png "メインのストーリー ボードまたはウィンドウを削除します。")
3. **削除 ダイアログ**をクリックして、**削除**.storyboard または .xib をプロジェクトから完全に削除するボタン。 

    ![削除を確認する](xibless-ui-images/switch02.png "削除を確認します。")

これで、変更する必要があります、 **MainWindow.cs**ウィンドウのレイアウトを定義および変更するファイル、 **ViewController.cs**または**MainWindowController.cs**ファイルを作成します。インスタンス、`MainWindow`クラスお .storyboard または .xib ファイルを使用していないためです。

ストーリー ボードを使用して、ユーザー インターフェイスは自動的に使用できませんの最新の Xamarin.Mac アプリ、 **MainWindow.cs**、 **ViewController.cs**または**MainWindowController.cs**ファイル。 必要に応じて、単に新しいクラスを追加空 c# プロジェクト (**追加** > **新しいファイル**  > **全般** > **空のクラス**) し、不足しているファイルと同じ名前です。 


### <a name="defining-the-window-in-code"></a>コード ウィンドウを定義します。

次に、編集、 **MainWindow.cs**ファイルし、次のようになります。

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

いくつかの主要な要素について説明します。

最初に、いくつか追加されました_プロパティの計算_(ウィンドウを .storyboard または .xib ファイルで作成した) 場合とコンセントと同様に機能します。

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

これらは us アクセスを与えるウィンドウに表示しようとしている UI 要素。 .Storyboard または .xib ファイルから、ウィンドウが大きくされていない、ためおにそれをインスタンス化する方法を必要があります (後でおわかりのよう、`MainWindowController`クラス)。 この新しいメソッドをコンス トラクターが何を行っています。

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

これは、ここで、ウィンドウのレイアウトをデザインおよび必要なユーザー インターフェイスを作成するために必要なすべての UI 要素に配置されます。 必要なウィンドウに追加できる UI 要素があれば、前に、_コンテンツ ビュー_要素を含める必要。

```csharp
ContentView = new NSView (Frame);
```

これは、ウィンドウに収まるコンテンツ ビューを作成します。 これで、この最初の UI 要素を追加、`NSButton`ウィンドウに。

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

最初にここで注意することは、iOS とは異なり macOS を使用して数学表記のウィンドウの座標系を定義します。 したがって原点は、値が右方向と、ウィンドウの右上隅に向かってを増やすことで、ウィンドウの左下内がします。 作成するとき、新しい`NSButton`はこの点を考慮ように画面上の位置とサイズを定義します。

`AutoresizingMask = NSViewResizingMask.MinYMargin`プロパティは、そのウィンドウが垂直方向にサイズ変更されると、ウィンドウの上部から同じ場所に保存することをボタンに指示します。 ここでも、これが必要な (0, 0) は、ウィンドウの左下にあります。

最後に、`ContentView.AddSubview (ClickMeButton)`メソッドを追加、`NSButton`実行とウィンドウが表示され、アプリケーションはときにそのことが画面に表示されるように、コンテンツ ビューにします。

回数を表示するウィンドウにラベルを次に追加する、`NSButton`がクリックしてされました。 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

MacOS は、固有の仕様があるないため_ラベル_UI 要素が追加されました。 特別にスタイル設定された編集不可能な`NSTextField`ラベルとして機能します。 ウィンドウの左下には、(0, 0) を前に、アカウントに、サイズと位置が、ボタンと同様です。 `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin`プロパティを使用して、**または**2 つを結合する演算子を`NSViewResizingMask`機能します。 これは、ウィンドウの垂直方向にサイズとウィンドウの上部から同じ場所に状態を維持し、圧縮し、につれて幅で、ウィンドウの水平方向にサイズ変更がラベルになります。

もう一度、`ContentView.AddSubview (ClickMeLabel)`メソッドを追加、`NSTextField`アプリケーションが実行され、ウィンドウが開かれたときにそのことが画面に表示されるように、コンテンツ ビューにします。


### <a name="adjusting-the-window-controller"></a>ウィンドウのコント ローラーを調整します。

以降のデザイン、`MainWindow`が読み込まれている不要になった、.storyboard や .xib ファイルから、ウィンドウのコント ローラーをいくつか調整する必要があります。 編集、 **MainWindowController.cs**ファイルし、次のようになります。

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

この変更の主要な要素について話し合うことができます。

新しいインスタンスを最初に、定義は、`MainWindow`クラスを基本ウィンドウ コント ローラーに割り当てる`Window`プロパティ。

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

画面でのウィンドウの場所を定義して、`CGRect`です。 ウィンドウの座標系と同じようには、画面は、左下隅として (0, 0) を定義します。 次に、ウィンドウのスタイルを使用して定義、**または**を 2 つ以上の結合演算子`NSWindowStyle`機能。

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

次`NSWindowStyle`機能を利用します。

- **ふちなし**のウィンドウには枠線はありません。
- **「** -、ウィンドウ タイトル バーになります。
- **Closable**のウィンドウを閉じる ボタンがあり、閉じることができます。
- **Miniaturizable** -ウィンドウ Miniaturize ボタンがあり、最小限に抑えることができます。
- **サイズ変更可能な**-、ウィンドウのサイズを変更 ボタンがある、サイズ変更できます。
- **ユーティリティ**-ユーティリティ スタイル ウィンドウ (パネル)。
- **DocModal**のウィンドウが表示するパネルの場合は、ドキュメント モーダル モーダル システムではなくなります。 
- **NonactivatingPanel**のウィンドウが表示するパネルの場合は、これは行われませんのメイン ウィンドウです。
- **TexturedBackground**のウィンドウが、テクスチャの背景が付きます。
- **スケールなし**のウィンドウは縮小されません。
- **UnifiedTitleAndToolbar** -は、ウィンドウのタイトルとツールバーの領域を結合します。
- **Hud** -ヘッドアップ ディスプレイ パネルとウィンドウが表示されます。
- **FullScreenWindow**のウィンドウが全画面表示モードに切り替わることができます。
- **FullSizeContentView**のウィンドウのコンテンツ ビューが、タイトルとツールバー領域の背後にします。

最後の 2 つのプロパティを定義、_バッファリング型_ウィンドウのウィンドウの描画が遅延される場合とします。 詳細については`NSWindows`、Apple を参照してください[Introduction to Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)ドキュメント。

最後に、.storyboard または .xib ファイルから、ウィンドウが大きくされていない、ので必要がありますでシミュレートするために、 **MainWindowController.cs** 、windows を呼び出すことによって`AwakeFromNib`メソッド。

```csharp
Window.AwakeFromNib ();
```

これにより、.storyboard または .xib ファイルから読み込まれる標準ウィンドウと同様にすると、ウィンドウに対してコードが許可されます。


### <a name="displaying-the-window"></a>ウィンドウの表示

.Storyboard または .xib ファイルと削除、および**MainWindow.cs**と**MainWindowController.cs**変更、ファイルが使用されるウィンドウと同じように作成した通常のウィンドウ.Xib ファイルと Xcode のインターフェイスのビルダー。

次はウィンドウとそのコント ローラーの新しいインスタンスを作成し、ウィンドウを表示する画面に。

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

この時点では、アプリケーションを実行して、ボタンが 2 ~ 3 回クリックした場合は、次が表示されます。

![実行のサンプル アプリ](xibless-ui-images/run01.png "実行のサンプル アプリ")


## <a name="adding-a-code-only-window"></a>コードのみ ウィンドウを追加します。

プロジェクトを右クリックし、既存の Xamarin.Mac アプリケーションにコードでのみ、xibless ウィンドウを追加する場合、**ソリューション パッド**選択**追加** > **新しいファイル.**.**新しいファイル**ダイアログ選択**Xamarin.Mac** > **コント ローラーと Cocoa ウィンドウ**下図のように。

![新しいウィンドウのコント ローラーを追加する](xibless-ui-images/add01.png "新しいウィンドウ コント ローラーの追加") 

同様に、既定の .storyboard または .xib ファイルをプロジェクトから削除されます (ここでは**SecondWindow.xib**)」の手順に従うと、[コードを使用するウィンドウを切り替える](#Switching_a_Window_to_use_Code)をカバーする前のセクション、コード ウィンドウの定義。


## <a name="adding-a-ui-element-to-a-window-in-code"></a>コード内のウィンドウに、UI 要素を追加します。

ウィンドウはコードで作成したか、.storyboard または .xib ファイルから読み込まれた、かありますコードからウィンドウへの UI 要素を追加する場所です。 例えば:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

上記のコードが新たに作成`NSButton`に追加し、`MyWindow`表示ウィンドウのインスタンス。 基本的に Xcode のインターフェイスのビルダー .storyboard または .xib ファイル内で定義できる UI 要素をコードで作成や、ウィンドウに表示されます。


## <a name="defining-the-menu-bar-in-code"></a>コードでメニュー バーの定義

現在の制限によって Xamarin.Mac は行わないでを作成すること、Xamarin.Mac アプリケーションのメニュー バー –`NSMenuBar`– でコードが、引き続き使用する、 **Main.storyboard**または**MainMenu.xib**ファイルを定義します。 ただし、追加および c# コードでメニューおよびメニュー項目を削除できます。

たとえば、編集、 **<code>appdelegate.cs</code>**ファイルし、`DidFinishLaunching`次のようなメソッドの検索。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

上記は、コードからのステータス バーのメニューを作成し、アプリケーションを起動するときに、それを表示します。 メニューと操作の詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)ドキュメント。


## <a name="summary"></a>まとめ

この記事では、c# .storyboard または .xib ファイルで Xcode のインターフェイスのビルダーを使用してではなくコードで Xamarin.Mac アプリケーションのユーザー インターフェイスの作成についての詳細を取得しました。



## <a name="related-links"></a>関連リンク

- [MacXibless (サンプル)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [メニュー](~/mac/user-interface/menu.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows の概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
