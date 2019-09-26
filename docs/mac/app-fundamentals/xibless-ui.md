---
title: . xib のユーザーインターフェイスの設計 (Xamarin. Mac)
description: この記事では、storyboard ファイル、xib ファイル、または Interface Builder C#を使用せずに、コードから直接 Xamarin. Mac アプリケーションのユーザーインターフェイスを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: bcc176f8d3eb97751e6957039c2a14ed02aad653
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "70770158"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>. xib のユーザーインターフェイスの設計 (Xamarin. Mac)

_この記事では、storyboard ファイル、xib ファイル、または Interface Builder C#を使用せずに、コードから直接 Xamarin. Mac アプリケーションのユーザーインターフェイスを作成する方法について説明します。_

## <a name="overview"></a>概要

同じユーザー インターフェイス要素にアクセスし、ツールで作業する開発者 Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき*Objective-C*と*Xcode*はします。 通常、Xamarin. Mac アプリケーションを作成するときは、Xcode の Interface Builder を使用して、アプリケーションのユーザーインターフェイスを作成して維持するために、storyboard または xib ファイルを使用します。

また、Xamarin. Mac アプリケーションの UI の一部またはすべてをコード内でC#直接作成することもできます。 この記事では、コードでC#ユーザーインターフェイスと UI 要素を作成するための基本について説明します。

[![Visual Studio for Mac コードエディター](xibless-ui-images/intro01.png "Visual Studio for Mac コードエディター")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>コードを使用するためのウィンドウの切り替え

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは、プロジェクトに自動的に含まれる**メインの storyboard** (または**xib**) ファイルで定義されています。 これには、アプリのメインビューを管理する**ViewController.cs**ファイル (または、従来は**MainWindow.cs**と**MainWindowController.cs**ファイル) も含まれています。

アプリケーションの Xibless ウィンドウに切り替えるには、次の手順を実行します。

1. または xib ファイルを使用して`.storyboard`停止するアプリケーションを開き、Visual Studio for Mac でユーザーインターフェイスを定義します。
2. **Solution Pad**で、 **mainwindow.xaml**ファイルまたは xib ファイルを右クリック**し、[** **削除**] を選択します。

    ![メインのストーリーボードまたはウィンドウの削除](xibless-ui-images/switch01.png "メインのストーリーボードまたはウィンドウの削除")
3. [**削除] ダイアログボックス**で、 **[削除]** ボタンをクリックして、プロジェクトから完全に storyboard または xib を削除します。

    ![削除の確認](xibless-ui-images/switch02.png "削除の確認")

次に、 **MainWindow.cs**ファイルを変更してウィンドウのレイアウトを定義し、 **ViewController.cs**または**MainWindowController.cs**ファイルを変更して`MainWindow`クラスのインスタンスを作成する必要があります。これは、を使用しなくなったためです。storyboard または xib ファイル。

ユーザーインターフェイスにストーリーボードを使用する最新の Xamarin. Mac アプリには、 **MainWindow.cs**、 **ViewController.cs** 、 **MainWindowController.cs**の各ファイルが自動的に含まれない場合があります。 必要にC#応じて、新しい空のクラスをプロジェクトに追加します (新しいファイルの**追加** >  **...** 一般的な**空のクラス**) で、見つからないファイルと同じ名前を付けます。  >  > 

### <a name="defining-the-window-in-code"></a>コードでのウィンドウの定義

次に、 **MainWindow.cs**ファイルを編集し、次のように表示します。

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

いくつかの重要な要素について説明します。

まず、(ウィンドウが storyboard または xib ファイルで作成されたかのように) コンセントのように機能するいくつかの計算される_プロパティ_を追加しました。

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

これにより、ウィンドウに表示する UI 要素にアクセスできるようになります。 ウィンドウは、storyboard または xib ファイルから大きくなっていないため、インスタンス化する方法が必要です ( `MainWindowController`クラスで後ほど説明します)。 この新しいコンストラクターメソッドの機能を次に示します。

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

ここでは、ウィンドウのレイアウトをデザインし、必要なユーザーインターフェイスを作成するために必要な UI 要素を配置します。 UI 要素をウィンドウに追加する前に、要素を格納する_コンテンツビュー_が必要です。

```csharp
ContentView = new NSView (Frame);
```

これにより、ウィンドウを埋めるコンテンツビューが作成されます。 ここで、最初の UI 要素である`NSButton`をウィンドウに追加します。

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

ここで最初に注意すべき点は、iOS とは異なり、macOS は数学表記を使用して、ウィンドウの座標系を定義することです。 そのため、元のポイントはウィンドウの左下隅にあり、値はウィンドウの右上隅と右上隅に向かって大きくなります。 新しい`NSButton`を作成するときは、画面上での位置とサイズを定義するときに、このことを考慮に入れます。

この`AutoresizingMask = NSViewResizingMask.MinYMargin`プロパティは、ウィンドウのサイズを垂直方向に変更するときに、ウィンドウの上部と同じ位置に移動するようにボタンに指示します。 ここでも、(0, 0) がウィンドウの左下にあるため、これが必要です。

最後に、 `ContentView.AddSubview (ClickMeButton)`メソッドは、 `NSButton`をコンテンツビューに追加します。これにより、アプリケーションの実行時とウィンドウの表示時に画面に表示されるようになります。

次に、 `NSButton`がクリックされた回数を表示するラベルがウィンドウに追加されます。

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

MacOS には特定の_ラベル_の UI 要素がないため、ラベルとして機能するよう`NSTextField`に特別にスタイル設定された編集不可のを追加しました。 前のボタンと同じように、サイズと場所によって、ウィンドウの左下にある (0, 0) が考慮されます。 プロパティは、or 演算子を使用して 2 `NSViewResizingMask`つの機能を結合します。 `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` これにより、ウィンドウのサイズを垂直方向に変更したときに、ウィンドウの上部と同じ場所にラベルが表示されるようになります。また、ウィンドウのサイズを水平方向に変更したときに、幅を縮小して拡大します。

ここでも`ContentView.AddSubview (ClickMeLabel)` 、メソッドは`NSTextField` 、をコンテンツビューに追加して、アプリケーションの実行時とウィンドウの開閉時に画面に表示されるようにします。

### <a name="adjusting-the-window-controller"></a>ウィンドウコントローラーの調整

の`MainWindow`デザインは、storyboard または xib ファイルから読み込まれなくなったため、ウィンドウコントローラーに何らかの調整を加える必要があります。 **MainWindowController.cs**ファイルを編集し、次のように表示します。

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

この変更の主な要素について説明します。

まず、 `MainWindow`クラスの新しいインスタンスを定義し、それを基本ウィンドウコントローラーの`Window`プロパティに割り当てます。

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

ここでは、を`CGRect`使用して画面のウィンドウの場所を定義します。 ウィンドウの座標系と同じように、画面の左下隅に (0, 0) が定義されています。 次に、 **or**演算子を使用して2つ`NSWindowStyle`以上の機能を結合することで、ウィンドウのスタイルを定義します。

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
```

次`NSWindowStyle`の機能を使用できます。

- **フチなし**-ウィンドウには境界線がありません。
- **タイトル-ウィンドウ**にはタイトルバーが表示されます。
- **Closable** -ウィンドウには [閉じる] ボタンがあり、閉じることができます。
- **Miniaturizable** -ウィンドウには [Miniaturize] ボタンがあり、最小化することができます。
- [サイズ**変更**可能]: ウィンドウにサイズ変更ボタンが表示され、サイズを変更できます。
- **ユーティリティ**-ウィンドウはユーティリティスタイルウィンドウ (パネル) です。
- **Docmodal** -ウィンドウがパネルの場合、システムモーダルではなくドキュメントモーダルになります。
- **非アクティブ化パネル**-ウィンドウがパネルの場合は、メインウィンドウに表示されません。
- **Texturedbackground** -ウィンドウの背景がテクスチャになります。
- **スケールなし**-ウィンドウは拡大縮小されません。
- **UnifiedTitleAndToolbar** -ウィンドウのタイトルとツールバー領域が結合されます。
- **Hud** -ウィンドウがヘッドアップ表示パネルとして表示されます。
- **Fullscreenwindow** -ウィンドウを全画面表示モードにすることができます。
- **FullSizeContentView** -ウィンドウのコンテンツビューは、タイトルとツールバー領域の背後にあります。

最後の2つのプロパティは、ウィンドウの_バッファーの種類_を定義します。ウィンドウの描画が遅延される場合もあります。 の詳細につい`NSWindows`ては、Apple の[Windows の概要に](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)関するドキュメントを参照してください。

最後に、ウィンドウが storyboard または xib ファイルから大きくなっていないため、windows `AwakeFromNib`のメソッドを呼び出して、 **MainWindowController.cs**でシミュレートする必要があります。

```csharp
Window.AwakeFromNib ();
```

これにより、storyboard または xib ファイルから読み込まれた標準ウィンドウと同様に、ウィンドウに対してコードを作成できます。

### <a name="displaying-the-window"></a>ウィンドウの表示

Xib ファイルまたは**MainWindow.cs**ファイルと**MainWindowController.cs**ファイルを削除し、Xcode の Interface Builder で作成された通常のウィンドウと同様に、xib ファイルを使用してウィンドウを使用します。

次の例では、ウィンドウとそのコントローラーの新しいインスタンスを作成し、画面にウィンドウを表示します。

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

この時点で、アプリケーションが実行され、ボタンが数回クリックされると、次のように表示されます。

![アプリの実行例](xibless-ui-images/run01.png "アプリの実行例")

## <a name="adding-a-code-only-window"></a>コードのみのウィンドウの追加

既存の xibless アプリケーションにコードのみを追加する場合は、 **Solution Pad**でプロジェクトを右クリックし、**新しいファイル**の**追加** >  を選択します。次の図に示すように、**新しいファイル** ダイアログボックスで、コントローラーを使用して  **Xamarin. Mac** > **cocoa ウィンドウ**を選択します。

![新しいウィンドウコントローラーの追加](xibless-ui-images/add01.png "新しいウィンドウコントローラーの追加")

前と同様に、プロジェクト (この例では**xib**) から既定のストーリーボードまたは xib ファイルを削除し、上のコードセクションを使用した[ウィンドウの切り替え](#Switching_a_Window_to_use_Code)に関するセクションの手順に従って、ウィンドウの定義をコードに適用します。

## <a name="adding-a-ui-element-to-a-window-in-code"></a>コード内のウィンドウに UI 要素を追加する

ウィンドウがコードで作成されたか、または storyboard または xib ファイルから読み込まれたかにかかわらず、コードからウィンドウに UI 要素を追加することが必要になる場合があります。 次に例を示します。

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

上のコードでは、 `NSButton`新しいを作成し、 `MyWindow`それをウィンドウインスタンスに追加して表示します。 基本的には、Xcode の Interface Builder で定義できるすべての UI 要素を、コードで作成し、ウィンドウに表示することができます。

## <a name="defining-the-menu-bar-in-code"></a>コードでのメニューバーの定義

Xamarin. mac の現在の制限により、コードでは xamarin アプリケーションのメニューバー`NSMenuBar`を作成することは推奨されていませんが、 **xib**ファイルを使用して定義することはできません。 ただし、コードでC#メニューやメニュー項目を追加および削除することはできます。

たとえば、 **AppDelegate.cs**ファイルを編集し、 `DidFinishLaunching`メソッドを次のように設定します。

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

上の例では、コードからステータスバーメニューを作成し、アプリケーションの起動時に表示します。 メニューの操作の詳細については、[メニュー](~/mac/user-interface/menu.md)のドキュメントを参照してください。

## <a name="summary"></a>まとめ

この記事では、Xcode の Interface Builder を storyboard または xib ファイルで使用するのC#ではなく、コードで Xamarin アプリケーションのユーザーインターフェイスを作成する方法について詳しく説明しました。

## <a name="related-links"></a>関連リンク

- [MacXibless (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macxibless)
- [Windows](~/mac/user-interface/window.md)
- [メニュー](~/mac/user-interface/menu.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows の概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
