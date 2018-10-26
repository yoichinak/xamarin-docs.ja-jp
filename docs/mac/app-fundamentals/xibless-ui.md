---
title: Xamarin.Mac で.storyboard/.xib-less ユーザー インターフェイスの設計
description: この記事から直接、Xamarin.Mac アプリケーションのユーザー インターフェイスの作成ではC#.storyboard ファイル、.xib ファイル、またはインターフェイス ビルダーなしのコード。
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: e41f19c1a2d02537f300ae82b7f3d45bc6571e1b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112466"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>Xamarin.Mac で.storyboard/.xib-less ユーザー インターフェイスの設計

_この記事から直接、Xamarin.Mac アプリケーションのユーザー インターフェイスの作成ではC#.storyboard ファイル、.xib ファイル、またはインターフェイス ビルダーなしのコード。_

## <a name="overview"></a>概要

同じユーザー インターフェイス要素にアクセスし、ツールで作業する開発者 Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき*Objective-C*と*Xcode*はします。 Xamarin.Mac アプリケーションを作成するときに通常、作成およびアプリケーションのユーザー インターフェイスを管理する Xcode の Interface Builder .storyboard または .xib ファイルに使用します。

直接、Xamarin.Mac アプリケーションの UI の一部またはすべてを作成するオプションもあるC#コード。 この記事では、ユーザー インターフェイスと UI 要素の作成の基本を説明しますC#コード。

[![Visual Studio for Mac のコード エディター](xibless-ui-images/intro01.png "Visual Studio for Mac のコード エディター")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>コードを使用して、ウィンドウの切り替え

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、 **Main.storyboard** (または従来、 **MainWindow.xib**) プロジェクトに自動的に含まれるファイル。 これも含まれています、 **ViewController.cs**アプリのメイン ビューを管理するファイル (またはこれまでもう一度、 **MainWindow.cs**と**MainWindowController.cs**ファイル)。

アプリケーションの Xibless ウィンドウに切り替えるには、次の操作を行います。

1. 使用を停止したいアプリケーションを開く`.stroyboard`または .xib ファイルで Visual Studio for mac ユーザー インターフェイスを定義するには
2. **Solution Pad**を右クリックし、 **Main.storyboard**または**MainWindow.xib**ファイルおよび選択**削除**: 

    ![メインのストーリー ボードまたはウィンドウを削除する](xibless-ui-images/switch01.png "メインのストーリー ボードまたはウィンドウを削除します。")
3. **削除ダイアログ**、 をクリックして、**削除**.storyboard または .xib をプロジェクトから完全に削除するボタン。 

    ![削除を確認する](xibless-ui-images/switch02.png "削除を確認します。")

変更する必要がありますので、 **MainWindow.cs**ウィンドウのレイアウトを定義および変更するファイル、 **ViewController.cs**または**MainWindowController.cs**ファイルを作成する、インスタンス、`MainWindow`クラス不要になった .storyboard または .xib ファイルを使用しているためです。

最新の Xamarin.Mac アプリのユーザー インターフェイスが自動的に含まれないために、ストーリー ボードを使用する、 **MainWindow.cs**、 **ViewController.cs**または**MainWindowController.cs**ファイル。 新しい空を単に、必要に応じて追加C#クラスをプロジェクト (**追加** > **新しいファイル...**  > **全般** > **空のクラス**) し、不足しているファイルと同じ名前です。 


### <a name="defining-the-window-in-code"></a>コードでウィンドウを定義します。

次に、編集、 **MainWindow.cs**ファイルを開き、次のようになります。

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

最初に、いくつか追加されました_プロパティの計算_(.storyboard または .xib ファイルでウィンドウを作成した) 場合、コンセントのように機能します。

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

これらは私たちにアクセスできるように、ウィンドウに表示する UI 要素。 .Storyboard または .xib ファイルから、ウィンドウが大きくされていない、ためにインスタンス化する方法が必要 (後で表示されるよう、`MainWindowController`クラス)。 この新しいコンス トラクター メソッドを行っています。

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

これはウィンドウのレイアウトのデザインし、必要なユーザー インターフェイスを作成するために必要な UI 要素を配置する場所です。 必要なウィンドウに UI 要素を追加するため、_コンテンツ ビュー_要素を格納します。

```csharp
ContentView = new NSView (Frame);
```

これは、ウィンドウによって設定されるコンテンツ ビューを作成します。 最初の UI 要素を追加しましたので、`NSButton`ウィンドウに。

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

最初にここで注意することは、iOS とは異なり macOS 数学的表記を使用、ウィンドウの座標システムの定義です。 そのため、原点は、値が右方向と、ウィンドウの右上隅に向かってを増やすことで、ウィンドウの左側にある下隅が。 作成するとき、新しい`NSButton`この点を考慮に画面上の位置とサイズを定義します。

`AutoresizingMask = NSViewResizingMask.MinYMargin`プロパティは、ウィンドウが垂直方向にサイズ変更されると、ウィンドウの上部から同じ場所に維持するように、ボタンを指示します。 ここでも、これは必要なためです (0, 0) は、ウィンドウの左下にあります。

最後に、`ContentView.AddSubview (ClickMeButton)`メソッドを追加、`NSButton`のため、アプリケーションが実行して表示されるウィンドウと画面に表示されることは、コンテンツ ビューにします。

回数の合計を表示するウィンドウにラベルを次に追加する、`NSButton`がクリックしてされました。 

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

MacOS は特定があるないため_ラベル_UI 要素の場合は、特別にスタイルが追加されましたが編集可能な`NSTextField`ラベルとして機能します。 その (0, 0) は、ウィンドウの左下にあるアカウントにサイズと位置は前に、ボタンと同様です。 `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin`プロパティを使用して、**または**2 つを結合する演算子を`NSViewResizingMask`機能します。 これにより、ウィンドウの上部から同じ場所に常に、ウィンドウの垂直方向にサイズと縮小し、ウィンドウの水平方向にサイズ変更は、幅が大きくなったりラベル。

もう一度、`ContentView.AddSubview (ClickMeLabel)`メソッドを追加、`NSTextField`のため、アプリケーションが実行され、ウィンドウを開くときに画面に表示されることは、コンテンツ ビューにします。


### <a name="adjusting-the-window-controller"></a>ウィンドウ コント ローラーを調整します。

以降のデザイン、`MainWindow`は読み込まれなく .storyboard または .xib ファイルからウィンドウ コント ローラーをいくつか調整する必要があります。 編集、 **MainWindowController.cs**ファイルを開き、次のようになります。

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

この変更の主要な要素を説明することができます。

まずの新しいインスタンスを定義します、`MainWindow`クラスを基本ウィンドウ コント ローラーに割り当てる`Window`プロパティ。

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

画面のウィンドウの場所を定義、`CGRect`します。 ウィンドウの座標系と同様、画面左下隅に (0, 0) を定義します。 次に、ウィンドウのスタイルを使用して定義、**または**を 2 つ以上の結合演算子`NSWindowStyle`機能。

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

次`NSWindowStyle`機能を利用します。

- **ボーダーレス**-、ウィンドウには境界線はありません。
- **「** -ウィンドウのタイトル バーになります。
- **Closable** -、ウィンドウが閉じるボタンがあるし、閉じることができます。
- **Miniaturizable** -ウィンドウ Miniaturize ボタンとして表示され、最小限に抑えることができます。
- **サイズ変更可能な**-は、ウィンドウのサイズを変更するボタンがあるし、サイズを変更できます。
- **ユーティリティ**-ユーティリティ スタイル ウィンドウ (パネル)。
- **DocModal** -ウィンドウがパネルの場合は、ドキュメント モーダル システム モーダルではなくなります。 
- **NonactivatingPanel** -ウィンドウがパネルの場合は、それにしない場合、メイン ウィンドウ。
- **TexturedBackground**のテクスチャの背景、ウィンドウが必要があります。
- **スケールなし**-ウィンドウはスケールされなくなります。
- **UnifiedTitleAndToolbar** -は、ウィンドウのタイトルとツールバーの領域を結合します。
- **Hud** -ヘッドアップ ディスプレイ パネルとして、ウィンドウが表示されます。
- **FullScreenWindow** -ウィンドウが全画面表示モードを開始できます。
- **FullSizeContentView** -ウィンドウのコンテンツ ビューは、ツールバー領域、タイトルの背後にあります。

最後の 2 つのプロパティを定義、_バッファリング型_ウィンドウの場合は、ウィンドウの描画を遅延とします。 詳細については`NSWindows`、Apple を参照してください[Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)ドキュメント。

最後に、ウィンドウは、.storyboard または .xib ファイルから拡張されているはありません、ために必要がありますでシミュレートするために、 **MainWindowController.cs** 、windows を呼び出すことによって`AwakeFromNib`メソッド。

```csharp
Window.AwakeFromNib ();
```

これにより、だけで、ウィンドウに対してコードをなどの標準の .storyboard または .xib ファイルから読み込まれたウィンドウが許可されます。


### <a name="displaying-the-window"></a>ウィンドウの表示

.Storyboard または .xib ファイルを削除し、 **MainWindow.cs**と**MainWindowController.cs** 、変更されたファイルで作成した通常のウィンドウと同様に、そのウィンドウを使っているします.Xib ファイルを使用して Xcode の Interface Builder です。

次がウィンドウとそのコント ローラーの新しいインスタンスを作成し、ウィンドウを画面に表示します。

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

この時点で、アプリケーションを実行、ボタンが何回かクリックされた場合は、次が表示されます。

![アプリの実行例](xibless-ui-images/run01.png "アプリの実行例")


## <a name="adding-a-code-only-window"></a>コード ウィンドウを追加します。

プロジェクトを右クリックし、既存の Xamarin.Mac アプリケーションにコードのみ、xibless ウィンドウを追加する場合、 **Solution Pad**選択**追加** > **新しいファイル.**.**新しいファイル**ダイアログ選択**Xamarin.Mac** > **Cocoa ウィンドウ コント ローラーと**以下に示すように。

![新しいウィンドウ コント ローラーの追加](xibless-ui-images/add01.png "新しいウィンドウ コント ローラーの追加") 

同様に、既定の .storyboard または .xib ファイル、プロジェクトから削除されます (ここで**SecondWindow.xib**) での手順に従います、[コードを使用して、ウィンドウの切り替え](#Switching_a_Window_to_use_Code)をカバーする前のセクション、コード ウィンドウの定義。


## <a name="adding-a-ui-element-to-a-window-in-code"></a>コード内のウィンドウに UI 要素を追加します。

ウィンドウをコードで作成または .storyboard または .xib ファイルから読み込まれた、かどうかがあります、ウィンドウをコードから UI 要素を追加します。 例えば:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

上記のコードが新たに作成`NSButton`にそれを追加し、`MyWindow`表示のウィンドウのインスタンス。 基本的に .storyboard または .xib ファイルに Xcode の Interface Builder で定義できる任意の UI 要素をコードで作成し、ウィンドウに表示されます。


## <a name="defining-the-menu-bar-in-code"></a>コードでメニュー バーの定義

Xamarin.Mac での現在の制限によっては行わないで作成すること、Xamarin.Mac アプリケーションのメニュー バー –`NSMenuBar`– でコードしますが、引き続き使用する、 **Main.storyboard**または**MainMenu.xib**ファイルを定義します。 追加およびメニューとメニュー項目を削除することができます、つまりC#コード。

たとえば、編集、 **AppDelegate.cs**ファイル、`DidFinishLaunching`メソッドの次のようになります。

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

上記のコードからのステータス バー メニューを作成し、アプリケーションの起動時が表示されます。 メニューの操作方法の詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)ドキュメント。


## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションのユーザー インターフェイスの作成について詳しく説明しましたC#.storyboard または .xib ファイルと Xcode の Interface Builder を使用してではなくコード。



## <a name="related-links"></a>関連リンク

- [MacXibless (サンプル)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [メニュー](~/mac/user-interface/menu.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows の概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
