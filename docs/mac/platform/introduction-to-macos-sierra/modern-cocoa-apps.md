---
title: 最新の macOS アプリの構築
description: この記事では、開発者が Xamarin. Mac で最新の macOS アプリを構築するために使用できる、いくつかのヒント、機能、および手法について説明します。
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 0ddf6cfc26e811505a50c2d89596f830658f0c1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029900"
---
# <a name="building-modern-macos-apps"></a>最新の macOS アプリの構築

_この記事では、開発者が Xamarin. Mac で最新の macOS アプリを構築するために使用できる、いくつかのヒント、機能、および手法について説明します。_

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>最新のビューで最新の外観を構築する

最新の外観には、次に示すアプリ例のような最新のウィンドウとツールバーの外観が含まれます。

[![](modern-cocoa-apps-images/content08.png "An example of a modern Mac app UI")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>全サイズのコンテンツビューの有効化

これを実現するために、開発者は_フルサイズのコンテンツビュー_を使用することをお勧めします。つまり、コンテンツはツール領域とタイトルバー領域の下に拡張され、macOS によって自動的に不鮮明になります。

この機能をコードで有効にするには、`NSWindowController` のカスタムクラスを作成し、次のようにします。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

この機能は、ウィンドウを選択し、**フルサイズのコンテンツビュー**をチェックすることで、Xcode の Interface Builder で有効にすることもできます。

[![](modern-cocoa-apps-images/content01.png "Editing the main storyboard in Xcode's Interface Builder")](modern-cocoa-apps-images/content01.png#lightbox)

フルサイズのコンテンツビューを使用する場合、開発者はタイトルとツールバー領域の下にあるコンテンツをオフセットして、特定のコンテンツ (ラベルなど) がその下にスライドしないようにする必要があります。

この問題をより複雑にするために、ユーザーが現在実行している操作、ユーザーがインストールした macOS のバージョン、またはアプリが実行されている Mac ハードウェアに基づいて、タイトルとツールバーの領域の高さを動的に設定できます。

その結果、ユーザーインターフェイスをレイアウトするときにオフセットをハードコーディングするだけでは動作しなくなります。 開発者は動的なアプローチを行う必要があります。

Apple は、コード内の現在のコンテンツ領域を取得するために、`NSWindow` クラスの[キー値観測](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes)可能な `ContentLayoutRect` プロパティを含めました。 開発者は、この値を使用して、コンテンツ領域が変更されたときに必要な要素を手動で配置できます。

より適切な解決策は、Auto Layout クラスと Size クラスを使用して、コードまたは Interface Builder に UI 要素を配置することです。

次の例のようなコードを使用すると、アプリのビューコントローラーで、オートレイアウトとサイズのクラスを使用して UI 要素を配置できます。

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

このコードは、ラベル (`ItemTitle`) に適用される top 制約のストレージを作成して、タイトルとツールバー領域の下にスリップしないようにします。

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

ビューコントローラーの `UpdateViewConstraints` メソッドをオーバーライドすることにより、開発者は必要な制約が既に構築されているかどうかをテストし、必要に応じて作成することができます。

新しい制約を構築する必要がある場合、制約が必要なコントロールがウィンドウの [`ContentLayoutGuide`] プロパティにアクセスされ、`NSLayoutGuide`にキャストされます。

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

`NSLayoutGuide` の TopAnchor プロパティがアクセスされ、使用可能な場合は、目的のオフセット量を使用して新しい制約を作成するために使用されます。新しい制約がアクティブになり、適用されます。

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>簡素化したツールバーの有効化

通常の macOS ウィンドウには、ウィンドウの上端に沿って実行される標準のタイトルバーが含まれています。 ウィンドウにツールバーも含まれている場合は、次のタイトルバー領域の下に表示されます。

[![](modern-cocoa-apps-images/content02.png "A standard Mac Toolbar")](modern-cocoa-apps-images/content02.png#lightbox)

簡素化されたツールバーを使用すると、タイトル領域が消え、ツールバーがウィンドウの [閉じる]、[最小化]、[最大化] の各ボタンを使用して、タイトルバーの位置に移動します。

[![](modern-cocoa-apps-images/content03.png "A streamlined Mac Toolbar")](modern-cocoa-apps-images/content03.png#lightbox)

合理化されたツールバーを有効にするには、`NSViewController` の `ViewWillAppear` メソッドをオーバーライドし、次のようにします。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

通常、この効果は、マップ、カレンダー、メモ、システム設定などの_Shoebox アプリケーション_(1 つのウィンドウアプリ) に使用されます。 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>アクセサリビューコントローラーの使用

アプリの設計によっては、開発者はタイトルバー領域を補完する必要がある場合もあります。アクセサリビューコントローラーは、タイトル/ツールバー領域のすぐ下に表示され、アクティビティに基づいてユーザーにコンテキストに依存するコントロールを提供します。現在の参加:

[![](modern-cocoa-apps-images/content04.png "An example Accessory View Controller")](modern-cocoa-apps-images/content04.png#lightbox)

アクセサリビューコントローラーは、開発者の関与なしで、システムによって自動的にぼやけてサイズが変更されます。

アクセサリビューコントローラーを追加するには、次の手順を実行します。

1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
2. **カスタムビューコントローラー**をウィンドウの階層にドラッグします。 

    [![](modern-cocoa-apps-images/content05.png "Adding a new Custom View Controller")](modern-cocoa-apps-images/content05.png#lightbox)
3. アクセサリビューの UI のレイアウト: 

    [![](modern-cocoa-apps-images/content06.png "Designing the new view")](modern-cocoa-apps-images/content06.png#lightbox)
4. アクセサリビューを、その UI の**アウトレット**とその他の**アクション**または**アウトレット**として公開します。 

    [![](modern-cocoa-apps-images/content07.png "Adding the required OUtlet")](modern-cocoa-apps-images/content07.png#lightbox)
5. 変更を保存します。
6. Visual Studio for Mac に戻り、変更を同期します。

`NSWindowController` を編集して、次のようにします。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

このコードの重要な点は、Interface Builder で定義され、**アウトレット**として公開されたカスタムビューにビューが設定される場所です。

```csharp
accessoryView.View = AccessoryViewGoBar;
```

アクセサリが表示される_場所_を定義する `LayoutAttribute` は次のとおりです。

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

MacOS は完全にローカライズされているため、`Left` および `Right` の `NSLayoutAttribute` プロパティは非推奨とされており、`Leading` および `Trailing`に置き換える必要があります。

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>タブ付きウィンドウの使用

さらに、macOS システムは、アクセサリビューコントローラーをアプリのウィンドウに追加する場合があります。 たとえば、いくつかのアプリのウィンドウが1つの仮想ウィンドウにマージされるタブ付きウィンドウを作成するには、次のようにします。

[![](modern-cocoa-apps-images/content08.png "An example of a tabbed Mac Window")](modern-cocoa-apps-images/content08.png#lightbox)

通常、開発者は Xamarin. Mac アプリで [タブ付きウィンドウを使用する] の操作を制限する必要があります。システムは次のように自動的に処理します。

- `OrderFront` メソッドが呼び出されると、Windows は自動的にタブになります。
- `OrderOut` メソッドが呼び出されると、Windows は自動的にタブ解除されます。
- コードでは、すべてのタブ付きウィンドウは依然として "visible" と見なされますが、すべての非表示タブは、CoreGraphics を使用してシステムによって非表示になります。
- `NSWindow` の `TabbingIdentifier` プロパティを使用して、ウィンドウを複数のタブにグループ化します。
- `NSDocument` ベースのアプリである場合、これらの機能のいくつかは、開発者の操作なしで自動的に有効になります (たとえば、タブバーに追加されるプラスボタン)。
- `NSDocument` ベースでないアプリでは、タブグループの "プラス" ボタンを使用して、`NSWindowsController`の `GetNewWindowForTab` 方法を上書きすることによって新しいドキュメントを追加できます。

システムベースのタブ付きウィンドウを使用する必要があるアプリの `AppDelegate` は、すべての要素をまとめて、次のようになります。

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

`NewDocumentNumber` プロパティは、作成された新しいドキュメントの数を追跡し、`NewDocument` メソッドは新しいドキュメントを作成して表示します。

`NSWindowController` は次のようになります。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

ここで、静的な `App` プロパティは、`AppDelegate`にアクセスするためのショートカットを提供します。 `SetDefaultDocumentTitle` メソッドは、新しく作成されたドキュメントの数に基づいて、新しいドキュメントのタイトルを設定します。

次のコードは、アプリがタブを使用することを推奨し、アプリのウィンドウをタブにグループ化するための文字列を提供することを macOS に指示します。

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

次のオーバーライドメソッドを実行すると、ユーザーがクリックしたときに新しいドキュメントを作成するためのプラスボタンがタブバーに追加されます。

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>コアアニメーションの使用

コアアニメーションは、macOS に組み込まれている高電力グラフィックスレンダリングエンジンです。 コアアニメーションは、CPU 上でグラフィックス操作を実行するのではなく、最新の macOS ハードウェアで使用できる GPU (グラフィックス処理ユニット) を活用するように最適化されています。これにより、コンピューターの速度が低下する可能性があります。

コアアニメーションによって提供される `CALayer`は、高速で滑らかなスクロールやアニメーションなどのタスクに使用できます。 アプリのユーザーインターフェイスは、コアアニメーションを完全に活用するために、複数のサブビューとレイヤーで構成されている必要があります。

`CALayer` オブジェクトは、次のように、ユーザーに表示される情報を開発者が制御できるようにするいくつかのプロパティを提供します。

- `Content`-レイヤーのコンテンツを提供する `NSImage` または `CGImage` を指定できます。
- `BackgroundColor`-レイヤーの背景色を `CGColor` として設定します
- `BorderWidth`-境界線の幅を設定します。
- `BorderColor`-境界線の色を設定します。

アプリの UI で主要なグラフィックスを利用するには、_レイヤーがサポート_されたビューを使用する必要があります。 Apple は、ウィンドウのコンテンツビューで常にを有効にすることをお勧めします。 これにより、すべての子ビューでレイヤーのバッキングも自動的に継承されます。

さらに、Apple では、新しい `CALayer` をサブレイヤーとして追加するのではなく、レイヤーでサポートされているビューを使用することを提案しています。これは、必要な設定 (Retina ディスプレイで必要な設定など) がシステムによって自動的に処理されるためです。

レイヤーのバッキングを有効にするには、次のように**コアアニメーションレイヤー**を確認して、**ビュー効果インスペクター**の下にある Xcode の Interface Builder の `true` またはその内部に `NSView` の `WantsLayer` を設定します。

[![](modern-cocoa-apps-images/content09.png "The View Effects Inspector")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>レイヤーを使用したビューの再描画

Xamarin. Mac アプリでレイヤーによってサポートされるビューを使用する場合のもう1つの重要な手順は、`NSViewController`で `OnSetNeedsDisplay` に `NSView` の `LayerContentsRedrawPolicy` を設定することです。 (例:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

開発者がこのプロパティを設定していない場合は、そのフレームの元の変更が発生するたびにビューが再描画されます。これは、パフォーマンス上の理由からは不要です。 このプロパティをに設定すると `OnSetNeedsDisplay` 開発者は手動で `NeedsDisplay` を `true` に設定して、コンテンツを強制的に再描画する必要があります。

ビューがダーティとマークされている場合、システムはビューの `WantsUpdateLayer` プロパティを確認します。 `true` が返された場合は、`UpdateLayer` メソッドが呼び出されます。それ以外の場合は、ビューの内容を更新するためにビューの `DrawRect` メソッドが呼び出されます。

Apple には、必要に応じてビューの内容を更新するための次の提案があります。

- Apple では、パフォーマンスが大幅に向上するため、可能な限り `DrawRect` に `UpdateLater` を使用することを推奨しています。
- 似たような UI 要素には、同じ `layer.Contents` を使用します。
- また、開発者は、可能な限り、`NSTextField`などの標準ビューを使用して UI を作成することも推奨されます。

`UpdateLayer`を使用するには、`NSView` のカスタムクラスを作成し、コードを次のようにします。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>最新のドラッグアンドドロップを使用する

ユーザーに最新のドラッグアンドドロップエクスペリエンスを提供するには、開発者はアプリのドラッグアンドドロップ操作で_ドラッグ Flocking_を採用する必要があります。 ドラッグ Flocking は、ユーザーがドラッグ操作を継続している間に、最初にドラッグされる個々のファイルまたは項目が、1つの要素として flocks (項目数のカウントでカーソルの下にグループ化される) 個々の要素として表示されます。

ユーザーがドラッグ操作を終了すると、個々の要素が Unflock され、元の場所に戻ります。

次のコード例では、カスタムビューでのドラッグ Flocking を有効にします。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

Flocking 効果は、ドラッグされている各項目を、配列内の個別の要素として `NSView` の `BeginDraggingSession` メソッドに送信することによって実現されました。

`NSTableView` または `NSOutlineView`を使用する場合は、`NSTableViewDataSource` クラスの `PastboardWriterForRow` メソッドを使用してドラッグ操作を開始します。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

これにより、開発者は、すべての行を1つのグループとしてペーストボードに書き込む前の `WriteRowsWith` 方法ではなく、ドラッグするテーブル内のすべての項目に対して個別の `NSDraggingItem` を提供できます。

`NSCollectionViews`を使用する場合は、ドラッグの開始時に `WriteItemsAt` メソッドではなく、`PasteboardWriterForItemAt` メソッドを使用します。

開発者は、クリップボードに大きなファイルを配置することは常に避ける必要があります。 MacOS Sierra を初めて_使用する_と、開発者は、新しい `NSFilePromiseProvider` クラスおよび `NSFilePromiseReceiver` クラスを使用してユーザーがドロップ操作を終了したときに後で処理されるように、指定されたファイルへの参照を、後で実行できるようにすることができます。

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>最新のイベント追跡の使用

タイトルまたはツールバー領域に追加されたユーザーインターフェイス要素 (`NSButton`など) の場合、ユーザーは要素をクリックして、通常どおりにイベントを発生させることができます (ポップアップウィンドウの表示など)。 ただし、項目はタイトルまたはツールバー領域にもあるため、ユーザーは要素をクリックしてドラッグし、ウィンドウを移動できる必要があります。

これをコードで実現するには、要素のカスタムクラス (`NSButton`など) を作成し、次のように `MouseDown` イベントをオーバーライドします。

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

このコードでは、UI 要素がアタッチされている `NSWindow` の `TrackEventsMatching` メソッドを使用して、`LeftMouseUp` および `LeftMouseDragged` イベントをインターセプトします。 `LeftMouseUp` イベントの場合、UI 要素は通常どおりに応答します。 `LeftMouseDragged` イベントの場合、イベントが `NSWindow`の `PerformWindowDrag` メソッドに渡され、画面上のウィンドウが移動します。

`NSWindow` クラスの `PerformWindowDrag` メソッドを呼び出すと、次のような利点があります。

- これにより、アプリがハングしている場合 (ディープループを処理する場合など) でも、ウィンドウを移動できます。
- スペースの切り替えは想定どおりに動作します。
- 空白バーは通常どおり表示されます。
- ウィンドウのスナップと配置は通常どおりに動作します。

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>最新のコンテナービューコントロールの使用

macOS Sierra には、以前のバージョンの OS で使用できる既存のコンテナービューコントロールに対する最新の機能強化が多数用意されています。

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>テーブルビューの機能強化

開発者は、常に新しい `NSView` ベースのバージョンのコンテナービューコントロール (`NSTableView`など) を使用する必要があります。 (例:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

これにより、テーブル内の特定の行にカスタムテーブル行アクションをアタッチできます (行を削除するためのスワイプ権限など)。 この動作を有効にするには、`NSTableViewDelegate`の `RowActions` メソッドをオーバーライドします。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

静的 `NSTableViewRowAction.FromStyle` は、次のスタイルの新しいテーブル行アクションを作成するために使用されます。

- `Regular`-行の内容の編集など、非破壊的な標準のアクションを実行します。
- `Destructive`-テーブルから行を削除するなどの破壊的なアクションを実行します。 これらのアクションは、背景が赤で表示されます。

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>スクロールビューの機能強化 

スクロールビュー (`NSScrollView`) を直接使用する場合、または別のコントロール (`NSTableView`など) の一部として使用する場合、スクロールビューの内容は、最新の外観とビューを使用して、Xamarin. Mac アプリのタイトルとツールバーの領域の下にスライドできます。

その結果、スクロールビューコンテンツ領域の最初の項目は、タイトルとツールバー領域によって部分的に隠される可能性があります。

この問題を修正するために、Apple は `NSScrollView` クラスに2つの新しいプロパティを追加しました。

- `ContentInsets`-スクロールビューの上部に適用されるオフセットを定義する `NSEdgeInsets` オブジェクトを開発者が指定できるようにします。
- `AutomaticallyAdjustsContentInsets`-`true` スクロールビューを使用すると、開発者の `ContentInsets` が自動的に処理されます。

開発者は `ContentInsets` を使用することによって、次のようなアクセサリを含めることができるように、スクロールビューの開始を調整できます。

- メールアプリに示されているような並べ替えインジケーター。
- 検索フィールド。
- [更新] または [更新] ボタン。

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>モダンアプリでの自動レイアウトとローカリゼーション

Apple には、開発者が国際化 macOS アプリを簡単に作成できるようにする Xcode にいくつかのテクノロジが含まれています。 Xcode を使用すると、開発者は、ストーリーボードファイル内のアプリのユーザーインターフェイスデザインからユーザー向けのテキストを分離し、UI が変更された場合にこの分離を維持するためのツールを提供できるようになりました。

詳細については、「Apple の[国際化とローカリゼーションのガイド](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)」を参照してください。

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>基本の国際化の実装 

基本の国際化を実装することにより、開発者は、アプリの UI を表す1つのストーリーボードファイルを提供し、ユーザー向けのすべての文字列を分離することができます。 

開発者が、アプリのユーザーインターフェイスを定義する初期ストーリーボードファイル (またはファイル) を作成すると、基本の国際化 (開発者が話す言語) でビルドされます。

次に、開発者は、複数の言語に翻訳できるローカライズと基本の国際化文字列 (ストーリーボード UI デザイン) をエクスポートできます。

その後、これらのローカライズをインポートし、Xcode によってストーリーボード用の言語固有の文字列ファイルが生成されます。

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>ローカライズをサポートするための自動レイアウトの実装

文字列値のローカライズされたバージョンは、サイズや読み取り方向が大きく異なる場合があるため、開発者は自動レイアウトを使用して、ストーリーボードファイル内のアプリのユーザーインターフェイスを配置およびサイズ設定する必要があります。

Apple では、次のことを提案しています。

- **固定幅制約を削除**する-テキストベースのすべてのビューのサイズをコンテンツに基づいて調整できます。 固定幅ビューでは、特定の言語でコンテンツをトリミングすることができます。
- **組み込みのコンテンツサイズを使用**する-既定では、テキストベースのビューがコンテンツに合わせて自動的にサイズ変更されます。 サイズが正しく設定されていないテキストベースのビューの場合は、Xcode の Interface Builder でそれらを選択し、[**編集** > **サイズをコンテンツに合わせる**] を選択します。
- **先頭と末尾の属性の適用**-テキストの方向はユーザーの言語に基づいて変化する可能性があるため、既存の `Right` および `Left` 属性とは対照的に、新しい `Leading` と `Trailing` の制約属性を使用します。 `Leading` と `Trailing` は、言語の方向に基づいて自動的に調整されます。
- **隣接するビューにビューをピン留め**する-選択した言語に応じてビューを変更するビューの位置を調整したり、サイズを変更したりできます。
- **Windows の最小サイズと最大サイズのいずれかまたは両方を設定しない**-選択した言語がコンテンツ領域のサイズを変更したときに、windows によるサイズ変更を許可します。
- アプリでの開発中に行われる**テストレイアウトの変更**は、常に異なる言語でテストする必要があります。 詳細については、Apple の[国際化アプリのテストに](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1)関するドキュメントを参照してください。
- **NSStackViews を使用する**と、ビューを - `NSStackViews` にピン留めして、予測可能な方法でコンテンツをシフトしたり拡大したりできます。また、選択した言語に基づいてコンテンツのサイズが変更されます。

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode の Interface Builder でのローカライズ

Apple では、Xcode の Interface Builder にいくつかの機能が用意されており、ローカリゼーションをサポートするアプリの UI を設計または編集するときに開発者が使用できます。 **属性インスペクター**の **[テキストの方向]** セクションでは、開発者は、選択したテキストベースのビュー (`NSTextField`など) で方向を使用および更新する方法に関するヒントを提供できます。

[![](modern-cocoa-apps-images/content10.png "The Text Direction options")](modern-cocoa-apps-images/content10.png#lightbox)

**テキストの方向**には、次の3つの値を指定できます。

- **自然**-レイアウトは、コントロールに割り当てられた文字列に基づいています。
- **左から右**-レイアウトは常に左から右に強制的に適用されます。
- **Right To left** -レイアウトは常に右から左に強制的に適用されます。

**レイアウト**には、次の2つの値を指定できます。

- **左から右**-レイアウトは常に左から右に配置されます。
- **Right To left** -レイアウトは常に右から左になります。

通常、特定のアラインメントが必要な場合を除き、これらを変更しないでください。

**Mirror**プロパティは、特定のコントロールのプロパティ (セルイメージの位置など) を反転するようシステムに指示します。 次の3つの値があります。

- **自動**-選択された言語の方向に基づいて、位置が自動的に変更されます。
- **右から左**へのインターフェイス-位置は右から左に記述された言語でのみ変更されます。
- **なし**-この位置は変更されません。

開発者がテキストベースのビューのコンテンツに対して**中心**、**ジャスティファイ**、または**完全な**配置を指定している場合は、選択した言語に基づいてこれらが反転されることはありません。

MacOS Sierra 前は、コードで作成されたコントロールは自動的にはミラー化されません。 開発者は、次のようなコードを使用してミラーリングを処理する必要がありました。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

`Alignment` と `ImagePosition` が、コントロールの `UserInterfaceLayoutDirection` に基づいて設定されています。

macOS Sierra は、いくつかのパラメーター (Title、Image、Action など) を受け取る新しい便利な `CreateButton` コンストラクターをいくつか追加し、自動的に適切にミラー化します。 (例:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>システムの外観の使用

最新の macOS アプリでは、イメージの作成、編集、プレゼンテーションアプリに適した新しい濃いインターフェイスの外観を採用できます。

[![](modern-cocoa-apps-images/content11.png "An example of a dark Mac Window UI")](modern-cocoa-apps-images/content11.png#lightbox)

これを行うには、ウィンドウが表示される前に1行のコードを追加します。 (例:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

`NSAppearance` クラスの静的 `GetAppearance` メソッドを使用して、システムから名前付きの外観を取得します (この場合は `NSAppearance.NameVibrantDark`)。

システムの外観を使用する場合、Apple には次のような推奨事項があります。

- ハードコードされた値 (`LabelColor` や `SelectedControlColor`など) に対して名前付きの色を優先します。
- 可能な場合は、システム標準コントロールスタイルを使用します。

システムの外観を使用する macOS アプリは、システム環境設定アプリのユーザー補助機能を有効にしたユーザーに対して自動的に正しく動作します。 その結果、Apple は、開発者が macOS アプリでシステムの外観を常に使用する必要があることを示唆しています。

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>ストーリーボードを使用した Ui のデザイン

ストーリーボードを使用すると、開発者は、アプリのユーザーインターフェイスを構成する個々の要素をデザインするだけでなく、特定の要素の UI フローと階層をデザインすることもできます。

開発者は、コントローラーを使用して、要素をコンポジションの単位に収集し、セグエ abstract を実行し、ビュー階層全体で移動するために必要な一般的な "グルーコード" を削除できます。

[![](modern-cocoa-apps-images/content12.png "Editing the UI in Xcode's Interface Builder")](modern-cocoa-apps-images/content12.png#lightbox)

詳細については、[ストーリーボードの概要に](~/mac/platform/storyboards/index.md)関するドキュメントを参照してください。

ストーリーボードに定義されている特定のシーンには、ビュー階層の前のシーンからのデータが必要になる場合があります。 Apple では、次のような情報をシーン間で渡すための推奨事項があります。

- データ dependancies は、常に階層内を下方向にカスケードする必要があります。
- Ui の柔軟性を制限するため、UI 構造的 dependancies をハードコーディングしないようにします。
- インターフェイスC#を使用して、汎用データ dependancies を提供します。

セグエのソースとして動作するビューコントローラーは、メソッドを `PrepareForSegue` オーバーライドし、ターゲットビューコントローラーを表示するためにセグエが実行される前に必要な初期化 (データの受け渡しなど) を実行できます。 (例:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

詳細については、[セグエ](~/mac/platform/storyboards/indepth.md#Segues)のドキュメントを参照してください。

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>アクションの伝達

MacOS アプリの設計によっては、ui コントロールのアクションに最適なハンドラーが UI 階層内の別の場所にある場合があります。 これは、通常、アプリの UI の他の部分とは別に、独自のシーンに存在するメニューおよびメニュー項目に当てはまります。

この状況に対処するために、開発者はカスタムアクションを作成し、そのアクションを応答側チェーンに渡すことができます。 詳細については、[カスタムウィンドウの操作に](~/mac/user-interface/menu.md)関するドキュメントを参照してください。

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>最新の Mac 機能

Apple には、次のように、開発者が最も多くの Mac プラットフォームを作成できるようにするために、macOS Sierra にユーザー向けの機能がいくつか組み込まれています。

- **Nsuseractivity** -これにより、アプリは、ユーザーが現在使用しているアクティビティを記述できます。 `NSUserActivity` は、最初はハンドオフをサポートするように作成されています。この場合、ユーザーのデバイスの1つで開始されたアクティビティを、別のデバイスで取得して続行することができます。 `NSUserActivity` は、iOS の場合と同様に macOS でも動作します。詳細については、ハンドオフ iOS のドキュメントの[概要に](~/ios/platform/handoff.md)関する記事をご覧ください。
- **Siri On Mac** -siri は、現在のアクティビティ (`NSUserActivity`) を使用して、ユーザーが発行できるコマンドのコンテキストを提供します。
- **状態の復元**-ユーザーが macOS でアプリを終了し、後で relaunches すると、アプリは自動的に以前の状態に戻ります。 開発者は、状態の復元 API を使用して、ユーザーインターフェイスがユーザーに表示される前に、一時的な UI の状態をエンコードおよび復元できます。 アプリが `NSDocument` 基づいている場合、状態の復元は自動的に処理されます。 `NSDocument` ベースでないアプリの状態の復元を有効にするには、`NSWindow` クラスの `Restorable` を `true`に設定します。
- **クラウド内のドキュメント**-macOS Sierra する前に、アプリはユーザーの iCloud ドライブにあるドキュメントを使用することを明示的に選択する必要がありました。 MacOS Sierra、ユーザーの**デスクトップ**フォルダーと**ドキュメント**フォルダーは、システムによって自動的に iCloud ドライブと同期される場合があります。 その結果、ユーザーのコンピューター上の領域を解放するために、ドキュメントのローカルコピーが削除される可能性があります。 この変更は、`NSDocument` ベースのアプリによって自動的に処理されます。 他のすべてのアプリの種類では、ドキュメントの読み取りと書き込みを同期するために `NSFileCoordinator` を使用する必要があります。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、開発者が Xamarin. Mac で最新の macOS アプリを構築するために使用できるいくつかのヒント、機能、および手法について説明しました。

## <a name="related-links"></a>関連リンク

- [macOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
