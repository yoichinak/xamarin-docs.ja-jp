---
title: 最新 macOS アプリの構築
description: この記事では、いくつかのヒント、機能と手法、開発者が Xamarin.Mac で最新 macOS アプリの構築に使用できるについて説明します。
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4eb4ff4a9e4784d816e2cbe8734e0422573cad92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="building-modern-macos-apps"></a>最新 macOS アプリの構築

_この記事では、いくつかのヒント、機能と手法、開発者が Xamarin.Mac で最新 macOS アプリの構築に使用できるについて説明します。_

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>最新のビューを最新の外観の構築

最新の外観には、次に示す例のアプリなど最新のウィンドウとツールバーの表示が含まれます。

[![](modern-cocoa-apps-images/content08.png "最新の Mac アプリ UI の例")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>コンテンツ ビューをサイズ完全を有効にします。

Xamarin.Mac アプリでこの検索を実現するために、開発者が使用する、_フル サイズのコンテンツ ビュー_、つまりとツールのタイトル バー領域の下にあるコンテンツを拡張および macOS によって自動的にあいまいされます。

コードでは、この機能を有効にするのカスタム クラスを作成、`NSWindowController`され次のようになります。

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

この機能も有効にする Xcode のインターフェイスのビルダーで、ウィンドウを選択し、チェック**フル サイズのコンテンツ ビュー**:

[![](modern-cocoa-apps-images/content01.png "Xcode のインターフェイスのビルダーでメインのストーリー ボードの編集")](modern-cocoa-apps-images/content01.png#lightbox)

完全なサイズのコンテンツ ビューを使用する場合は、開発者が (ラベルなど) の特定のコンテンツがその下にスライドしないように、タイトルとツール バー領域の下にコンテンツをオフセットする必要があります。

この問題が複雑になるに、タイトルとツール バーの領域が、ユーザーが実行中のアクションに基づく動的な高さを持つことができます、ユーザーがインストールされている macOS や Mac のハードウェアでアプリが実行されているのバージョン。

その結果、ユーザー インターフェイスのレイアウトに使用するときに、オフセットを単にハード コードは機能しません。 開発者は、動的な方法を取る必要があります。

Apple が含まれる、[監視可能なキーと値](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes)`ContentLayoutRect`のプロパティ、`NSWindow`クラスをコード内の現在のコンテンツ領域を取得します。 開発者は、コンテンツ領域が変更されたときに必要な要素を手動で配置するのにこの値を使用できます。

コードまたはインターフェイスのビルダー内の UI 要素の配置に自動レイアウトとサイズのクラスを使用することをお勧めします。

次の例のようなコードは、アプリのビューのコント ローラーに AutoLayout とサイズのクラスを使用して UI 要素の位置を使用できます。

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

このコードは、ラベルに適用される最上位の制約のストレージを作成 (`ItemTitle`) にタイトルとツール バーの下にスリップしないことを確認してください。

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

ビュー コント ローラーをオーバーライドすることで`UpdateViewConstraints`メソッド、開発者が必要な制約が既に構築されたかをテストおよび必要な場合は、それを作成します。

新しい制約を作成する必要がある場合、`ContentLayoutGuide`制約が適用される必要のあるコントロールのアクセスやをキャストして、ウィンドウのプロパティ、 `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

TopAnchor プロパティ、`NSLayoutGuide`がアクセスされ、使用可能な場合は、目的のオフセットの容量を持つ新しい制約の作成に使用され、新しい制約が適用することをアクティブになります。

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>簡素化されたツールバーを有効にします。

通常 macOS ウィンドウには、ウィンドウの上端実行のタイトル バー、標準が含まれています。 ウィンドウには、ツール バーも含まれている場合は、このタイトル バー領域の下に表示されます。

[![](modern-cocoa-apps-images/content02.png "標準 Mac ツールバー")](modern-cocoa-apps-images/content02.png#lightbox)

簡素化されたツールバーを使用して、タイトル領域は表示されなくなります、タイトル バーの位置に、上方向へ移動 ツールバー、インラインでウィンドウを閉じる、最小および最大化ボタンを使用。

[![](modern-cocoa-apps-images/content03.png "簡素化された Mac ツールバー")](modern-cocoa-apps-images/content03.png#lightbox)

オーバーライドすることで、簡素化されたツールバーが有効になっている、`ViewWillAppear`のメソッド、`NSViewController`され、次のようになります。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

この効果は、通常の使用_靴箱アプリケーション_(1 つのウィンドウ アプリ) は、マップ、予定表、ノート、システム環境設定と同様にします。 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>コント ローラーの付属品の表示を使用します。

アプリのデザイン、によって、開発者にすることも状況依存コントロールをアクティビティに基づく、ユーザーが提供するツール/タイトル バー領域の下のすぐアクセサリ ビュー コント ローラーで、領域は、タイトル バーを補完現在進行中で。

[![](modern-cocoa-apps-images/content04.png "例のアクセサリ ビュー コント ローラー")](modern-cocoa-apps-images/content04.png#lightbox)

アクセサリ ビュー コント ローラーに自動的にあいまいし、する、システムの開発者の介入なしでのサイズを変更します。

アクセサリ ビュー コント ローラーを追加するには、次の操作を行います。

1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
2. ドラッグ、**カスタム ビューのコント ローラー**をウィンドウの階層にします。 

    [![](modern-cocoa-apps-images/content05.png "新しいカスタム ビュー コント ローラーを追加します。")](modern-cocoa-apps-images/content05.png#lightbox)
3. アクセサリのビューをレイアウト UI: 

    [![](modern-cocoa-apps-images/content06.png "新しいビューの設計")](modern-cocoa-apps-images/content06.png#lightbox)
4. として付属品のビューを公開、**コンセント**およびその他の**アクション**または**コンセント**その ui: 

    [![](modern-cocoa-apps-images/content07.png "必要なコンセントを追加します。")](modern-cocoa-apps-images/content07.png#lightbox)
5. 変更を保存します。
6. Visual Studio for Mac の変更を同期に戻ります。

編集、`NSWindowController`され次のようになります。

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

このコードの重要な点は、ビューをインターフェイスのビルダーで定義されとして公開されたカスタム ビューに設定されている、**コンセント**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

および`LayoutAttribute`を定義する_場所_アクセサーが表示されます。

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

MacOS が完全にローカライズされていますので、`Left`と`Right``NSLayoutAttribute`プロパティは廃止され、置き換えられます`Leading`と`Trailing`です。

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>タブ付きウィンドウを使用します。

さらに、macOS システムでは、アプリのウィンドウにアクセサリ ビュー コント ローラーを追加する場合があります。 たとえば、タブ付きウィンドウが 1 つの仮想ウィンドウにマージされます、アプリの Windows のいくつかの場所を作成するには、します。

[![](modern-cocoa-apps-images/content08.png "Mac のタブ付きウィンドウの例")](modern-cocoa-apps-images/content08.png#lightbox)

通常は、開発者が制限付きのアクションが Xamarin.Mac アプリでのタブ付きウィンドウを使用する必要がありますをシステムは処理するように自動的に。

- Windows が自動的にタブ付きときに、`OrderFront`メソッドが呼び出されます。
- Windows が自動的に Untabbed ときに、`OrderOut`メソッドが呼び出されます。
- コードではすべてのタブ付きウィンドウと見なされます「表示」、ただし、最前面以外のタブでは表示されません CoreGraphics を使用して、システムです。
- 使用して、`TabbingIdentifier`プロパティ`NSWindow`タブにウィンドウをグループ化します。
- ある場合、`NSDocument`開発者操作をしなくても自動的に (など、タブ バーに追加されている [+] ボタン) ベースのアプリでこれらの機能のいくつかが有効にします。
- 非`NSDocument`ベースのアプリには、をオーバーライドすることで、新しいドキュメントを追加するタブ グループに「+」ボタンが有効にすることができます、`GetNewWindowForTab`のメソッド、`NSWindowsController`です。

すべての部分を統合することで、`AppDelegate`を使用する、アプリのシステムに基づくタブ付きウィンドウの次のようになります。

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

ここで、`NewDocumentNumber`プロパティの追跡作成された新規ドキュメントの数と`NewDocument`メソッドは、新しい文書を作成し、それを表示します。

`NSWindowController`次のようになります。

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

ここで、静的な`App`プロパティを取得するショートカットを提供する、`AppDelegate`です。 `SetDefaultDocumentTitle`メソッドが作成された新規ドキュメントの数に基づく新しいドキュメントのタイトルを設定します。

次のコードでは、タブを使用するアプリが推奨する macOS に指示し、アプリの Windows でのタブにグループ化できるようにする文字列を提供します。

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

次のメソッドをオーバーライドが、ユーザーがクリックされたときに、新しいドキュメントを作成するタブ バーに、[+] ボタンを追加します。

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>コア アニメーションを使用します。

コア アニメーションは、macOS に組み込まれている高電源グラフィックス レンダリング エンジンです。 コア アニメーションは、コンピューターの速度が低下すると、CPU 上でのグラフィックス操作を実行しているのではなく最新 macOS ハードウェアで使用できる GPU (グラフィックス処理装置) を活用するために最適化されています。

`CALayer`、コア アニメーションによって提供される、高速で滑らかなスクロールやアニメーションなどのタスクに使用できます。 アプリのユーザー インターフェイスは、複数のサブビューとコア アニメーションを有効に活用するレイヤーで構成する必要があります。

A`CALayer`オブジェクトが使用すると、開発者提示された内容を制御するユーザーに画面に表示されるなど、いくつかのプロパティを提供します。

- `Content` は、`NSImage`または`CGImage`レイヤーの内容を提供します。
- `BackgroundColor` -レイヤーの背景色を設定します。 `CGColor`
- `BorderWidth` 境界線の幅を設定します。
- `BorderColor` 境界線の色を設定します。

アプリの UI にコア グラフィックスを利用するには、その必要がありますを使用している_レイヤー バックアップ_ビューで、Apple 開発者をする必要がありますウィンドウのコンテンツ ビューに常に有効にすることを提案します。 これにより、すべての子ビューが自動的に継承レイヤーのバックアップもします。

さらに、Apple は新しいを追加するのではなくレイヤー バックアップ ビューの使用を提案`CALayer`副層として、システムで自動的に処理 (Retina ディスプレイで必要なもの) など、必要な設定のいくつかがあるためです。

設定して、バックアップのレイヤーを有効にすることができます、`WantsLayer`の`NSView`に`true`または下にある Xcode のインターフェイスのビルダー内で、**表示効果インスペクター**をチェックして**コア アニメーション レイヤー**:

[![](modern-cocoa-apps-images/content09.png "表示効果インスペクター")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>レイヤーにビューを再描画

他の重要な手順のレイヤー バックアップ ビュー Xamarin.Mac アプリの使用を設定する場合、`LayerContentsRedrawPolicy`の`NSView`に`OnSetNeedsDisplay`で、`NSViewController`です。 例えば:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

フレームの原点が変わるたびに、開発者は、このプロパティを設定しない場合、ビューが再描画されますこれは、パフォーマンス上の理由が必要ないです。 このプロパティ設定して`OnSetNeedsDisplay`、開発者が設定する必要が手動で`NeedsDisplay`に`true`を再描画する、ただし、コンテンツを強制的にします。

ビューは、ダーティとマークは、システムによって確認、`WantsUpdateLayer`ビューのプロパティです。 返された場合`true`、`UpdateLayer`メソッドは、それ以外の場合、`DrawRect`ビューの内容を更新するビューのメソッドが呼び出されます。

Apple では、必要なときにビューの内容を更新の次の方法があります。

- Apple に適したものを使用して`UpdateLater`経由で`DrawRect`として、可能なが大幅なパフォーマンスの向上を提供するたびにします。
- 同じ使用`layer.Contents`のような UI 要素。
- Apple では、標準的なビューを使用して、UI を作成する開発者もが推奨`NSTextField`、もう一度限りです。

使用する`UpdateLayer`、カスタム クラスを作成、`NSView`し、次のように、コードします。

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

## <a name="using-modern-drag-and-drop"></a>最新のドラッグ アンド ドロップを使用します。

開発者が採用する必要がありますが、ユーザーにとって、最新のドラッグ アンド ドロップ機能を表示_ドラッグ群雄_自社のアプリでは、ドラッグ アンド ドロップ操作です。 ドラッグ Flocking は、それぞれ個別のファイルまたは最初にドラッグされている項目を群れ (項目の数のカウントと共にカーソルの下にグループ)、ユーザーがドラッグ操作を継続する個々 の要素として表示されます。

ユーザーは、ドラッグ操作を終了する個々 の要素は Unflock し、元の場所に戻ります。

次のコード例は、カスタム ビューでのドラッグ群雄を有効にします。

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

ドラッグされている各項目を送信することによって実現されました群れ効果、`BeginDraggingSession`のメソッド、`NSView`として、配列で個別の要素。

使用する場合、`NSTableView`または`NSOutlineView`を使用して、`PastboardWriterForRow`のメソッド、`NSTableViewDataSource`ドラッグ操作を開始するクラス。

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

これにより、開発者は、個人を提供する`NSDraggingItem`旧式の方法ではなくドラッグされているテーブル内のすべての項目の`WriteRowsWith`を書き込むすべての行 1 つのグループとしてペーストします。

使用する場合`NSCollectionViews`、もう一度使用して、`PasteboardWriterForItemAt`メソッドはなく、`WriteItemsAt`ドラッグしたときにメソッドを開始します。

開発者はペースト ボード上の大きなファイルを配置することに常にしないでください。 MacOS Sierra を新しい_ファイル Promise_開発者が参照を配置できるようにするには、ユーザー、new を使用して、Drop 操作の終了時に満たされた後でペースト上のファイルを指定`NSFilePromiseProvider`と`NSFilePromiseReceiver`クラス。

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>最新のイベントの追跡を使用します。

ユーザー インターフェイス要素の (など、 `NSButton`) に追加されたタイトルまたはツール バー領域では、ユーザーは、要素をクリックして、それを (ポップアップ ウィンドウを表示する) などの通常の方法でイベントを発生させることにする必要があります。 ただし、ため、アイテムは、タイトルまたはツール バー領域でもある、ユーザーは をクリックしても、ウィンドウを移動する要素をドラッグできるようにする必要があります。

これを実現するコードで、要素のカスタム クラスを作成します (など`NSButton`) をオーバーライドし、`MouseDown`次のようにイベント。

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

このコードを使用して、`TrackEventsMatching`のメソッド、`NSWindow`をインターセプトする UI 要素にアタッチされている、`LeftMouseUp`と`LeftMouseDragged`イベント。 `LeftMouseUp`イベント、通常どおり応答を UI 要素。 `LeftMouseDragged`イベント、イベントに渡される、`NSWindow`の`PerformWindowDrag`ウィンドウを画面に移動します。

呼び出す、`PerformWindowDrag`のメソッド、`NSWindow`クラスは、次の利点を提供します。

- アプリが停止した場合でも、ウィンドウを移動するには、その (場合など、詳細なループを処理)。
- 領域の切り替えは正常に動作します。
- 通常どおり、スペース バーが表示されます。
- スナップ ウィンドウと位置合わせ通常どおりに動作します。

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>最新のコンテナーのビュー コントロールを使用します。

macOS Sierra は OS の以前のバージョンで利用可能な既存のコンテナーのビュー コントロールに多くの最新の機能強化を提供します。

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>テーブル ビューの機能強化

開発者は、必ず使用して、新しい`NSView`などのバージョンのコンテナーのビュー コントロールをベース`NSTableView`です。 例えば:

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

これにより、特定の行を削除するには、右側をスワイプ) など、テーブル内の行にアタッチされているカスタムのテーブル行のアクション。 この動作を有効にするには、オーバーライド、`RowActions`のメソッド、 `NSTableViewDelegate`:

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

静的な`NSTableViewRowAction.FromStyle`は、次のスタイルの新しいテーブルの行アクションの作成に使用します。

- `Regular` -"は標準の非破壊的なアクションの編集など、行の内容"を実行します。
- `Destructive` -アクションを実行、破壊的な [削除] など、行、テーブルからです。 これらのアクションは、背景が赤で表示されます。

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>ビューの機能強化をスクロールします。 

スクロール可能なビューを使用する場合 (`NSScrollView`) 直接、または別のコントロールの一部として (など`NSTableView`)、スクロール ビューの内容が Xamarin.Mac とでアプリケーションを最新の検索ビューで、タイトルとツール バーの領域の下にスライドできます。

その結果、Scroll View コンテンツ領域の最初の項目にタイトルとツール バー領域で部分的に隠されることです。

この問題を修正する Apple が 2 つの新しいプロパティを追加、`NSScrollView`クラス。

- `ContentInsets` -により、開発者は提供する、`NSEdgeInsets`スクロール ビューの最上位に適用されるオフセットを定義するオブジェクト。
- `AutomaticallyAdjustsContentInsets` If`true`スクロール ビューが自動的に処理、`ContentInsets`開発者用です。

使用して、`ContentInsets`開発者などのアクセサリを含めることを許可するスクロール ビューの先頭を調整できます。

- 並べ替えインジケーターなどのメール アプリに示されています。
- 検索フィールドです。
- 更新または更新プログラムのボタンです。

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>自動レイアウトと最新のアプリでのローカライズ

Apple には、開発者が簡単に国際化 macOS アプリを作成できるように Xcode にいくつかのテクノロジが含まれます。 Xcode は今すぐにより、開発者はアプリのユーザー インターフェイスでのデザインのストーリー ボード ファイル ユーザー向けのテキストを区切るし、UI が変更された場合、この分離を維持するためのツールを提供します。

詳細については、Apple を参照してください[国際化とローカリゼーション ガイド](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)です。

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>基本の国際化を実装します。 

ベースの国際化を実装すると、開発者は、アプリの UI を表し、すべてのユーザー向けの文字列を分離する 1 つのストーリー ボード ファイルを提供できます。 

初期のストーリー ボード ファイル (またはファイル) に、開発者が作成するとき、アプリのユーザー インターフェイスを定義する、ベースの国際化 (開発者が話す間の言語) でビルドされます。

次に、開発者には、ローカライズ版と複数の言語に翻訳することができます (ストーリー ボード UI のデザイン) ベースの国際化文字列をエクスポートできます。

後で、これらのローカライズ版をインポートして、Xcode は、ストーリー ボードの言語に固有の文字列のファイルが生成されます。

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>ローカリゼーションをサポートするために自動レイアウトを実装します。

ローカライズ版であるため、文字列の値が大幅に異なるサイズを持つことができますや方向を読み取り、開発者必要がありますを使用して自動レイアウト位置およびサイズ、ストーリー ボード ファイルに、アプリのユーザー インターフェイスをします。

Apple では、次の操作を勧めします。

- **固定幅の制約を削除する**-テキスト ベースのすべてのビューを許可するかサイズを変更するコンテンツに基づいて。 固定幅ビューは特定の言語でコンテンツにトリミングすることがあります。
- **組み込みのコンテンツ サイズを使用する**- 既定テキスト ベースのビューが自動-サイズによって、コンテンツに合わせてにします。 正しく調整されないテキスト ベースのビュー、Xcode のインターフェイスのビルダーでそれを選択し、選択**編集** > **サイズに合わせてコンテンツ**です。
- **先頭および末尾の属性を適用**- テキストの方向を変更できるため、new を使用して、ユーザーの言語に基づき、`Leading`と`Trailing`既存ではなく制約属性`Right`と`Left`属性。 `Leading` および`Trailing`言語方向に基づいて自動的に調整されます。
- **暗証番号 (pin) のビューを横にあるビュー** -これによりの位置を変更し、その周囲ビューを選択した言語に応じて変更に応じてサイズを変更するビュー。
- **Windows の最小値および最大サイズに設定されていない**のサイズを変更するように選択した言語のコンテンツ領域のサイズを変更できるように Windows です。
- **変更を継続的にレイアウトをテスト**- 中にアプリでの開発は、さまざまな言語で常にテストする必要があります。 Apple を参照してください[、国際化のテスト アプリ](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1)詳細についてはドキュメントです。
- **NSStackViews にビューを一緒に暗証番号 (pin) を使用して** -  `NSStackViews`シフトし、予測可能な方法で拡張するには、そのコンテンツとコンテンツは、選択した言語に基づいてサイズを変更できます。

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode のインターフェイスのビルダーでのローカライズ

Apple は、開発者は、使用できることを設計またはアプリの UI を編集するときのローカリゼーションをサポートするために Xcode のインターフェイスのビルダーでのいくつかの機能が提供がされます。 **テキストの方向**のセクションで、**属性インスペクター**により、ヒントを指定して 方法の方向を使用して、テキスト ベースのビューの select で更新する必要がある developer (など`NSTextField`)。

[![](modern-cocoa-apps-images/content10.png "テキストの方向オプション")](modern-cocoa-apps-images/content10.png#lightbox)

3 つの値がある、**テキストの方向**:

- **自然な**-、レイアウト コントロールに割り当てられた文字列に基づいています。
- **左右からに向かって**-レイアウトは左右からに強制的に常にします。
- **右から左へ**-レイアウトは右左からへ常に強制します。

2 つの値がある、**レイアウト**:

- **左右からに向かって**-レイアウトは常に左右からです。
- **右から左へ**-レイアウトは右から左に常にします。

通常これらが変更されない特定の対応が必要ない場合。

**ミラー**プロパティ (セル画像の位置) などの特定のコントロールのプロパティを反転するシステムに指示します。 3 つの値があります。

- **自動的に**-位置が選択されている言語の方向をに基づいて自動的に変更されます。
- **左側のインターフェイスを右に**-位置は右左のベース言語からでのみ変更されます。
- **決して**-位置は変更されません。

開発者が指定した場合**Center**、 **Justify**または**完全**テキスト ベースのビューのコンテンツを配置、これらは決して反転に基づいて言語を選択します。

MacOS Sierra 前と、コードで作成されたコントロールが自動的にミラー化されません。 開発者は、次のようにコードを使用してミラーリングを処理する必要があります。

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

ここで、`Alignment`と`ImagePosition`に基づいて設定される、`UserInterfaceLayoutDirection`コントロールのです。

macOS Sierra がいくつかの新しい便利なコンス トラクターを追加します (静的を介して`CreateButton`メソッド) を (タイトル、イメージ、およびアクション) などのいくつかのパラメーターを受け取るし、自動的に正しく反映されます。 例えば:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>システムの外観を使用します。

最新 macOS アプリでは、イメージの作成、編集、またはプレゼンテーション アプリに対して適切に機能する新しい濃いインターフェイス外観を採用できます。

[![](modern-cocoa-apps-images/content11.png "濃い Mac ウィンドウ UI の例")](modern-cocoa-apps-images/content11.png#lightbox)

これは、ウィンドウが表示される前に、1 行のコードを追加することで実行できます。 例えば:

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

静的な`GetAppearance`のメソッド、`NSAppearance`クラスを使用して、システムから名前付きの外観を取得 (ここでは`NSAppearance.NameVibrantDark`)。

Apple では、システムの外観を使用するための以下の推奨事項があります。

- 名前付きの色をハードコーディングされた値よりも優先 (など`LabelColor`と`SelectedControlColor`)。
- 可能な場合は、システム標準のコントロールのスタイルを使用します。

システム環境設定アプリからのユーザー補助機能を有効にしているユーザーは、システムの外観を使用する macOS アプリが正しく機能に自動的にします。 その結果、macOS apps で場合、開発者で常にシステムの外観を使用する Apple を提案します。

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>ストーリー ボードで Ui の設計

ストーリー ボードを使用すると、開発者だけでなく、アプリのユーザー インターフェイスを構成が、視覚化およびする UI を設計する個々 の要素のフローのデザインや特定の要素の階層です。

コント ローラーを使用すると、開発者が合成と Segues abstract の単位に要素を収集し、一般的な「接着コード」ビュー階層間を移動するために必要な削除します。

[![](modern-cocoa-apps-images/content12.png "Xcode のインターフェイスのビルダーで、UI の編集")](modern-cocoa-apps-images/content12.png#lightbox)

詳細についてを参照してください、[ストーリー ボードの概要](~/mac/platform/storyboards/index.md)ドキュメント。

ストーリー ボードで定義されている所定のシーンはビュー階層内で前のシーンからデータを必要と、多くのインスタンスが生じます。 Apple では、シーンの間で情報を渡すための以下の推奨事項があります。

- データ依存関係は必要があります、階層を常に下方向に重ねて表示します。
- これにより UI の柔軟性が制限としてハードコーディングする UI 構造の依存関係を避けてください。
- C# のインターフェイスを使用すると、依存関係を汎用的なデータを提供します。

Segue のソースとして動作しているビュー コント ローラーがオーバーライドできる、`PrepareForSegue`メソッドと、Segue 前に (データを渡す) など、任意の初期化が必要な操作は実行されている場合、ターゲット ビュー コント ローラーを表示します。 例えば:

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

詳細についてを参照してください、 [Segues](~/mac/platform/storyboards/indepth.md#Segues)ドキュメント。

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>操作を反映します。

MacOS アプリの設計に基づき、あるときに UI コントロールに対するアクションの最適なハンドラーは、可能性があります内で他の場所で指定 UI 階層。 これは通常のメニューおよびメニュー項目、アプリの UI の残りの部分から独立した独自のシーンでライブに当てはまります。

このような状況を処理するには、開発者はカスタム アクションを作成し、レスポンダーのチェーンの上のアクションを渡します。 詳細情報を参照してください、[カスタム ウィンドウの動作を扱う](~/mac/user-interface/menu.md)ドキュメント。

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Mac の最新の機能

Apple には、開発者がなど Mac プラットフォームを最大限に活用できるように macOS Sierra にいくつかのユーザー向けの機能が含まれます。

- **NSUserActivity** -これにより、アプリをユーザーが現在に関連するアクティビティを記述します。 `NSUserActivity` ハンドオフ、ユーザーのデバイスの 1 つで開始されたアクティビティをピックアップし、別のデバイスに続くでしたをサポートするために最初に作成されました。 `NSUserActivity` iOS では、そのとして macOS で同じ動作を参照してくださいようにしてください。 この[ハンドオフの概要](~/ios/platform/handoff.md)詳細については、iOS ドキュメント。
- **Mac で Siri** -Siri が現在のアクティビティを使用して (`NSUserActivity`)、ユーザーが発行できるコマンドにコンテキストを提供します。
- **状態の復元**- macos し、その後、アプリが自動的には、アプリは relaunches 終了のユーザーが以前の状態に返される場合。 開発者は、エンコードして、ユーザー インターフェイスがユーザーに表示される前に、一時的な UI 状態を復元状態の復元の API を使用できます。 場合は、アプリが`NSDocument`ベース、状態の復元は自動的に処理します。 状態の復元を有効にする非`NSDocument`ベースのアプリの設定、`Restorable`の`NSWindow`クラスを`true`です。
- **クラウド内のドキュメント**-macOS Sierra、以前のアプリは明示的にオプトインするユーザーの iCloud ドライブ内のドキュメントを使用する必要があります。 MacOS ユーザーを Sierra で**デスクトップ**と**ドキュメント**フォルダーに可能性があります、システムによって自動的にその iCloud ドライブと同期します。 その結果、ドキュメントのローカル コピーを削除して、ユーザーのコンピューター上の領域を解放する可能性があります。 `NSDocument` ベースのアプリは、この変更を自動的に処理されます。 その他のすべてのアプリの種類が使用する必要があります、`NSFileCoordinator`をドキュメントの読み取りと書き込みを同期します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事には、いくつかのヒント、機能と手法、開発者が Xamarin.Mac で最新 macOS アプリのビルドに使用できますがについて説明します。



## <a name="related-links"></a>関連リンク

- [macOS サンプル](https://developer.xamarin.com/samples/mac/)
