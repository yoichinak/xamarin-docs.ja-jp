---
title: 最新の macOS アプリの構築
description: この記事では、いくつかのヒント、機能と手法開発者 Xamarin.Mac で最新の macOS アプリのビルドに使用することについて説明します。
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 53bfc9f147b6cf369b8f5ce8d1257dbaf6b0f807
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032830"
---
# <a name="building-modern-macos-apps"></a>最新の macOS アプリの構築

_この記事では、いくつかのヒント、機能と手法開発者 Xamarin.Mac で最新の macOS アプリのビルドに使用することについて説明します。_

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>最新のビューを最新の外観を構築

次に示す例のアプリなど最新のウィンドウとツールバーの外観は、最新の状態が含まれます。

[![](modern-cocoa-apps-images/content08.png "最新の Mac アプリ UI の例")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>コンテンツ ビューのサイズ全体を有効にします。

Xamarin.Mac アプリでは、この検索を実現するために、開発者が使用する、_フル サイズのコンテンツ ビュー_、つまり、ツール、およびタイトル バー領域の下で、コンテンツを拡張および macOS によって自動的にあいまいされます。

コードでは、この機能を有効にするためのカスタム クラスを作成、`NSWindowController`し、次のようになります。

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

この機能も、ウィンドウを選択し、チェック Xcode の Interface Builder で有効にするには**規模のコンテンツ ビューの完全な**:

[![](modern-cocoa-apps-images/content01.png "Xcode の Interface Builder でメインのストーリー ボードの編集")](modern-cocoa-apps-images/content01.png#lightbox)

完全なサイズのコンテンツ ビューを使用する場合、開発者は、(ラベルなど) の特定のコンテンツがその下にスライドしないように、タイトルとツール バー領域の下にコンテンツをオフセットする必要があります。

この問題を複雑にタイトルとツール バーの領域には、ユーザーが現在実行しているアクションに基づいて動的な高さを持つことができます、ユーザーがインストールされている macOS や、アプリがで実行されている Mac のハードウェアのバージョン。

その結果、ユーザー インターフェイスをレイアウトするときに、オフセットを単にハード コーディングは機能しません。 開発者は、動的なアプローチをとる必要があります。

Apple が含まれている、[キーと値の観測可能な](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes)`ContentLayoutRect`のプロパティ、`NSWindow`クラスをコード内の現在のコンテンツ領域を取得します。 開発者は、この値を使用して、コンテンツ領域が変更されたときに必要な要素を手動で配置します。

自動レイアウトとサイズ クラスを使用して、コードまたは Interface Builder で UI 要素を配置することをお勧めします。

アプリのビュー コント ローラーで 自動レイアウトとサイズ クラスを使用する UI 要素を配置する次の例のようなコードを使用できます。

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

このコードは、ラベルに適用される制約を最上位の記憶域を作成します (`ItemTitle`) タイトルとツール バーの下に明細しないことを確認します。

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

ビュー コント ローラーをオーバーライドすることで`UpdateViewConstraints`メソッドでは、開発者が必要な制約が既にビルドされているかをテストおよび必要な場合は、それを作成します。

新しい制約をビルドする必要がある場合、`ContentLayoutGuide`制約が適用される必要があるコントロールにアクセスし、キャストには、ウィンドウのプロパティを`NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

TopAnchor プロパティ、`NSLayoutGuide`アクセスは、使用可能な場合は、目的のオフセットの量を使用して、新しい制約を作成するために使用、および新しい制約が適用することをアクティブになります。

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>合理的なツールバーを有効にします。

通常 macOS ウィンドウには、標準タイトル バーをウィンドウの上端実行にはが含まれています。 ウィンドウには、ツール バーも含まれています、このタイトル バー領域の下で表示されます。

[![](modern-cocoa-apps-images/content02.png "Mac の標準ツールバー")](modern-cocoa-apps-images/content02.png#lightbox)

合理的なツールバーを使用して、タイトル領域は表示されなくなります、タイトル バーの位置にツール バーが移動インラインでウィンドウを閉じる、最小および最大化ボタン。

[![](modern-cocoa-apps-images/content03.png "簡素化された Mac ツールバー")](modern-cocoa-apps-images/content03.png#lightbox)

オーバーライドすることで、合理化されたツールバーが有効になっている、`ViewWillAppear`のメソッド、`NSViewController`と、次のようになります。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

この効果は、通常の使用_Shoebox アプリケーション_(アプリの 1 つのウィンドウ) などのマップ、予定表、ノート、システム環境設定します。 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>アクセサリのビュー コント ローラーを使用します。

アプリの設計によって、開発者は状況に応じたコントロールをアクティビティに基づく、ユーザーが提供する/ツールのタイトル バー領域の下のすぐアクセサリ ビュー コント ローラーを使用して領域は、タイトル バーを補完するも可能性があります。現在進行中で。

[![](modern-cocoa-apps-images/content04.png "サンプルのアクセサリ ビュー コント ローラー")](modern-cocoa-apps-images/content04.png#lightbox)

アクセサリ ビュー コント ローラーのあいまいし、開発者が関与せず、システムによってサイズ変更に自動的には。

アクセサリのビュー コント ローラーを追加するには、次の操作を行います。

1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
2. ドラッグ、**カスタム ビュー コント ローラー**ウィンドウの階層に。 

    [![](modern-cocoa-apps-images/content05.png "新しいカスタム ビュー コント ローラーの追加")](modern-cocoa-apps-images/content05.png#lightbox)
3. アクセサリのビューをレイアウト UI: 

    [![](modern-cocoa-apps-images/content06.png "新しいビューの設計")](modern-cocoa-apps-images/content06.png#lightbox)
4. アクセサリのビューとして公開する**アウトレット**およびその他の**アクション**または**Outlet**その UI の。 

    [![](modern-cocoa-apps-images/content07.png "必要なアウトレットを追加します。")](modern-cocoa-apps-images/content07.png#lightbox)
5. 変更を保存します。
6. Visual Studio for Mac の変更を同期するに戻ります。

編集、`NSWindowController`し、次のようになります。

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

このコードの重要な点は、ビューがインターフェイス ビルダーで定義されとして公開されたカスタム ビューに設定されている、**アウトレット**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

および`LayoutAttribute`を定義する_場所_アクセサーが表示されます。

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

MacOS が完全にローカライズされていますので、`Left`と`Right``NSLayoutAttribute`プロパティは非推奨し、置き換える必要があります`Leading`と`Trailing`します。

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>タブ付きの Windows を使用します。

さらに、macOS システムでは、アプリのウィンドウにアクセサリ ビュー コント ローラーを追加する場合があります。 たとえば、タブ付きの Windows 仮想ウィンドウが 1 つにマージされます、アプリの Windows のいくつかの場所を作成します。

[![](modern-cocoa-apps-images/content08.png "Mac のタブ付きウィンドウの例")](modern-cocoa-apps-images/content08.png#lightbox)

通常、開発者が Xamarin.Mac アプリでのタブ付きの Windows 限定の操作を使用する必要があります、システムでそれらは次のように自動的に処理されます。

- Windows が自動的にタブ付きときに、`OrderFront`メソッドが呼び出されます。
- Windows が自動的に Untabbed ときに、`OrderOut`メソッドが呼び出されます。
- ただしすべてのタブ付きウィンドウを「表示」と見なされますが、コードではすべて非最前面のタブが CoreGraphics を使用して、システムによって非表示は。
- 使用して、`TabbingIdentifier`プロパティの`NSWindow`タブにグループの Windows にします。
- ある場合、`NSDocument`ベースのアプリがこれらの機能のいくつかが自動的に (タブ バーに追加されている [+] ボタン) など、開発者が操作しなくても有効になります。
- 非`NSDocument`ベースのアプリには、オーバーライドすることで、新しいドキュメントを追加するタブ グループで 「+」ボタンが有効にすることができます、`GetNewWindowForTab`のメソッド、`NSWindowsController`します。

すべての部分を統合することで、`AppDelegate`次のように、使用するアプリのベース システムのタブ付きの Windows になります。

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

場所、`NewDocumentNumber`プロパティには作成された新しいドキュメントの数の追跡と`NewDocument`メソッドは、新しい文書を作成し、それを表示します。

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

静的な`App`プロパティを取得するショートカットを提供する、`AppDelegate`します。 `SetDefaultDocumentTitle`メソッドが作成された新しいドキュメントの数に基づく新しいドキュメントのタイトルを設定します。

次のコードでは、タブを使用するアプリが推奨する macOS に指示し、アプリの Windows のタブにグループ化することができる文字列を提供します。

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

次のオーバーライド メソッドは、ユーザーがクリックされたときに、新しいドキュメントを作成するタブ バーを + ボタンを追加します。

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>コア アニメーションを使用します。

コア アニメーションとは、macOS に組み込まれている強力なグラフィックスのレンダリング エンジンです。 コア アニメーションは、コンピューターの速度が低下すると、CPU のグラフィックス操作を実行しているのではなく最新の macOS のハードウェアで使用可能な GPU (グラフィックス処理装置) を活用するために最適化されています。

`CALayer`、コア アニメーションによって提供される、高速で滑らかなスクロールやアニメーションなどのタスクに使用できます。 アプリのユーザー インターフェイスは、複数のサブビューとコア アニメーションを有効に活用するレイヤーで構成する必要があります。

A`CALayer`オブジェクトは、提示された内容を制御する開発者をなどで、ユーザーに画面上に表示できるようにするいくつかのプロパティを提供します。

- `Content` -は、`NSImage`または`CGImage`レイヤーの内容を提供します。
- `BackgroundColor` -レイヤーの背景色を設定します。 `CGColor`
- `BorderWidth` 境界線の幅を設定します。
- `BorderColor` 境界線の色を設定します。

アプリの UI のグラフィックはコアを利用するには、使用する必要がありますのする_レイヤー バックアップ_ビューで、Apple は、開発者がする必要があります、ウィンドウのコンテンツ ビューに常に有効にすることを示します。 これにより、すべての子ビューが自動的に継承レイヤーのバックアップもします。

さらに、Apple は提案を新しい追加ではなくレイヤー バックアップ ビューを使用して`CALayer`副層としてシステムは自動的に処理 (Retina ディスプレイで必要なもの) など、必要な設定のいくつかあるためです。

設定して、レイヤーのバックアップを有効にすることができます、`WantsLayer`の`NSView`に`true`または Xcode の Interface Builder の内部で、**表示効果インスペクター**をチェックして**コア アニメーション レイヤー**:

[![](modern-cocoa-apps-images/content09.png "表示効果インスペクター")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>レイヤーにビューを再描画

もう 1 つの重要なステップ設定は、Xamarin.Mac アプリでレイヤー バックアップ ビューを使用するときに、`LayerContentsRedrawPolicy`の`NSView`に`OnSetNeedsDisplay`で、`NSViewController`します。 例:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

フレームの原点が変更されるたびに、開発者は、このプロパティを設定しない場合、ビューが再描画されるパフォーマンス上の理由は望ましくありません。 このプロパティ設定して`OnSetNeedsDisplay`、開発者が手動で設定する必要があります`NeedsDisplay`に`true`を強制的に再描画するただしコンテンツ。

ビューはダーティとマークされると、システム チェック、`WantsUpdateLayer`ビューのプロパティ。 返された場合`true`、`UpdateLayer`メソッドが呼び出されると、それ以外の場合、`DrawRect`ビューの内容を更新するビューのメソッドが呼び出されます。

Apple では、必要な場合に、ビュー コンテンツを更新するための次の推奨事項があります。

- Apple では、使用が推奨`UpdateLater`経由で`DrawRect`と考えられるが、大幅なパフォーマンスを改善を提供するたびにします。
- 使用して、同じ`layer.Contents`のような UI 要素。
- Apple では、標準的なビューを使用してその UI を作成する開発者もが推奨`NSTextField`、もう一度限りです。

使用する`UpdateLayer`、用のカスタム クラスを作成、`NSView`と、コードの次のようになります。

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

## <a name="using-modern-drag-and-drop"></a>モダン ドラッグ アンド ドロップを使用します。

ユーザー用の最新のドラッグ アンド ドロップ エクスペリエンスに表示し、開発者が採用すべき_ドラッグ群雄_アプリのドラッグ アンド ドロップ操作。 ドラッグ Flocking は、それぞれ個別のファイルまたは最初にドラッグされている項目を群れ (項目の数のカウントと共にカーソルの下にグループ)、ユーザーがドラッグ操作を個々 の要素として表示されます。

ユーザーがドラッグ操作を終了、個々 の要素は Unflock し、元の場所に戻ります。

次のコード例では、カスタム ビューでのドラッグ群雄を有効にします。

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

群れの効果にドラッグされている各項目を送信することによって行われた、`BeginDraggingSession`のメソッド、`NSView`として、配列内の個別の要素。

使用する場合、`NSTableView`または`NSOutlineView`を使用して、`PastboardWriterForRow`のメソッド、`NSTableViewDataSource`クラスがドラッグ操作を開始します。

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

これにより、開発者は、個人を提供する`NSDraggingItem`古いメソッドではなくドラッグされているテーブルの各品目の`WriteRowsWith`を書き込むすべての行を 1 つのグループとしてペーストします。

使用する場合`NSCollectionViews`、もう一度使用して、`PasteboardWriterForItemAt`メソッドではなく、`WriteItemsAt`ドラッグしたときにメソッドを開始します。

開発者は、クリップボードに大きなファイルを配置することに常にしないでください。 Macos Sierra では、新しい_ファイル Promise_の参照を配置する開発者を許可するのには、ユーザーが新しいを使用して、ドロップ操作を完了すると、処理後ではクリップボード上のファイルを指定`NSFilePromiseProvider`と`NSFilePromiseReceiver`クラス。

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>最新のイベントの追跡を使用します。

ユーザー インターフェイス要素 (など、 `NSButton`) 追加されているタイトルまたはツール バー領域をユーザーの要素をクリックし、(ポップアップ ウィンドウを表示する) など、通常どおり、イベントを発生させることができるようにします。 ただし、項目がタイトルまたはツール バー領域もため、ユーザーはクリックしても、ウィンドウの移動先の要素をドラッグすることのようになります。

コードでこれを実現する要素のカスタム クラスを作成 (など`NSButton`) をオーバーライドし、`MouseDown`イベントとして次のとおりです。

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

このコードを使用して、`TrackEventsMatching`のメソッド、`NSWindow`をインターセプトする UI 要素がアタッチされている、`LeftMouseUp`と`LeftMouseDragged`イベント。 `LeftMouseUp`イベント、UI 要素が通常どおり応答します。 `LeftMouseDragged`イベント、イベントに渡される、`NSWindow`の`PerformWindowDrag`ウィンドウを画面に移動します。

呼び出す、`PerformWindowDrag`のメソッド、`NSWindow`クラスは、次の利点を提供します。

- アプリが停止している場合でも、ウィンドウを移動するには、その (場合など詳細なループを処理)。
- 領域の切り替え、期待どおりには機能します。
- 通常どおり、スペース バーが表示されます。
- ウィンドウへのスナップと配置は、通常どおり機能します。

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>最新のコンテナーのビュー コントロールを使用します。

macOS Sierra では、OS の以前のバージョンで使用可能な既存のコンテナーのビュー コントロールに多くの最新の機能強化を提供します。

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>テーブル ビューの機能強化

開発者は、新しい常に使用する必要があります`NSView`などのバージョンのコンテナーのビュー コントロールをベース`NSTableView`します。 例えば:

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

これにより、特定の行を削除するには、右方向のスワイプ操作) などのテーブル内の行に接続するカスタムのテーブル行のアクション。 この動作を有効にするにはオーバーライド、`RowActions`のメソッド、 `NSTableViewDelegate`:

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

静的な`NSTableViewRowAction.FromStyle`次のスタイルの新しいテーブル行のアクションを作成するために使用します。

- `Regular` -"編集などの標準的な非破壊的なアクションを行のコンテンツ"実行します。
- `Destructive` -[削除] などの破壊的な操作、行から実行の表に、します。 これらのアクションは、背景が赤で表示されます。

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>スクロール ビューの機能強化 

スクロール可能なビューを使用する場合 (`NSScrollView`) 直接、または別のコントロールの一部として (など`NSTableView`)、スクロール ビューの内容が最新の検索とビューを使用して Xamarin.Mac アプリのタイトルとツール バー領域の下にスライドできます。

結果として、スクロール ビューのコンテンツ領域の最初の項目は、タイトルとツール バーの領域により部分的に表示します。

この問題を修正する Apple が 2 つの新しいプロパティを追加、`NSScrollView`クラス。

- `ContentInsets` -許可に提供する開発者、`NSEdgeInsets`スクロール ビューの先頭に適用されるオフセットを定義するオブジェクト。
- `AutomaticallyAdjustsContentInsets` If`true`スクロール ビューが自動的に処理、`ContentInsets`開発者向け。

使用して、`ContentInsets`開発者など、[アクセサリ] を含めることを許可する、スクロール ビューの開始を調整できます。

- 並べ替えインジケーターなどのメール アプリで示したものです。
- 検索フィールド。
- 更新または更新するボタン。

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>自動レイアウトと最新のアプリでのローカライズ

Apple には、開発者が、国際対応の macOS アプリを簡単に作成できるように Xcode にいくつかのテクノロジが含まれます。 Xcode は今すぐにより、開発者は、ストーリー ボード ファイルに、アプリのユーザー インターフェイスの設計からユーザーに表示されるテキストを分離して、UI が変更された場合、この分離を維持するためのツールを提供します。

詳細については、Apple を参照してください[国際化とローカリゼーション ガイド](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)します。

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>基本の国際化を実装します。 

ベースの国際化を実装すると、開発者は、アプリの UI を表し、すべてのユーザーに表示される文字列を分割する 1 つのストーリー ボード ファイルを提供できます。 

初期のストーリー ボード ファイル (またはファイル) に、開発者が作成するとき、アプリのユーザー インターフェイスを定義する、ベースの国際化 (講演を行い、開発者の言語) でビルドされます。

次に、開発者は、ローカライズ版と複数の言語に翻訳することができます (でストーリー ボード UI デザイン) ベースの国際化文字列をエクスポートできます。

その後、これらのローカライズをインポートすることができ、Xcode は、ストーリー ボードの言語固有の文字列のファイルを生成します。

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>ローカリゼーションをサポートするために自動レイアウトを実装します。

ローカライズ版のための値の文字列の大幅に異なるサイズであることができますや方向を読むと、開発者が使用自動レイアウト位置し、サイズ、ストーリー ボード ファイルで、アプリのユーザー インターフェイスに。

Apple では、次の手順に従ってお勧めします。

- **固定幅の制約を削除**-すべてのテキスト ベースのビューを許可するサイズを変更するコンテンツに基づいて。 固定幅ビューは、特定の言語でコンテンツをトリミング可能性があります。
- **組み込みのコンテンツ サイズを使用して**- 既定テキスト ベースのビューは自動-サイズで、コンテンツに合わせてにします。 正しく調整されないテキスト ベースのビュー、Xcode の Interface Builder でそれを選択し、選択**編集** > **サイズに合わせてコンテンツ**します。
- **先頭と末尾の属性を適用**- テキストの方向を変更できるため、新しいを使用して、ユーザーの言語に基づいて、`Leading`と`Trailing`既存ではなく制約属性`Right`と`Left`属性。 `Leading` `Trailing`言語方向に基づいて自動的に調整されます。
- **暗証番号 (pin) のビューを隣接するビュー** -これにより、位置し、サイズは、周囲にビューを選択した言語に応じて変更するビュー。
- **Windows の最小および最大サイズを設定しないでください**-選択した言語のコンテンツ領域のサイズを変更するようにサイズを変更するを許可する Windows。
- **テストの変更を継続的にレイアウト**- 中にさまざまな言語でアプリでの開発を継続的にテストする必要があります。 Apple を参照してください。 [、国際化のテスト アプリ](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1)詳細についてはドキュメントです。
- **NSStackViews を一緒に暗証番号 (pin) のビューを使用して、**  -  `NSStackViews`シフトし、予測可能な方法で拡張するには、そのコンテンツとコンテンツは、選択した言語に基づくサイズを変更できます。

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode の Interface Builder でのローカライズ

Apple は、こと、開発者は使用して設計またはアプリの UI を編集するときのローカリゼーションをサポートするために Xcode の Interface Builder でいくつかの機能を提供しました。 **テキスト方向**のセクション、**属性インスペクター**方向を使用およびテキスト ベースのビューの選択 で更新する方法についてのヒントを提供する、開発者 (など`NSTextField`)。

[![](modern-cocoa-apps-images/content10.png "テキストの方向オプション")](modern-cocoa-apps-images/content10.png#lightbox)

3 つの値がある、**テキスト方向**:

- **自然な**-、レイアウト、コントロールに割り当てられた文字列に基づいています。
- **左から右に**-レイアウトは左から右へ常に強制します。
- **右から左に**-レイアウトは右から左へ常に強制します。

2 つの値がある、**レイアウト**:

- **左から右に**-レイアウトは常に左右から。
- **右から左に**-レイアウトは右から左には常にします。

通常これらは変更できませんしない限り、特定の対応が必要です。

**ミラー**プロパティ (セルのイメージの位置) などの特定のコントロールのプロパティを反転するシステムに指示します。 3 つの値があります。

- **自動的に**の位置を選択した言語の方向に基づいて自動的に変更されます。
- **右に左のインターフェイスで**の位置は右左のベース言語からでのみ変更されます。
- **決して**の位置は変更されません。

開発者が指定されている場合**Center**、**両端揃え**または**完全**テキスト ベースのビューのコンテンツを配置、これらは反転に基づいて言語を選択であることはありません。

MacOS Sierra では前と、コードで作成されたコントロールは自動的にミラー化されません。 開発者は、ミラーリングを処理するために、次のようなコードを使用する必要があります。

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

場所、`Alignment`と`ImagePosition`に基づいて設定されて、`UserInterfaceLayoutDirection`のコントロール。

macOS Sierra がいくつかの新しい便利なコンス トラクターを追加します (静的なを使用して`CreateButton`メソッド) を (タイトル、イメージ、およびアクション) などのいくつかのパラメーターを受け取るし、自動的に正しく反映されます。 例:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>システムの外観を使用します。

最新の macOS アプリには、新しい濃いインターフェイス外観はイメージの作成、編集、またはプレゼンテーション アプリケーションに適しているを導入できます。

[![](modern-cocoa-apps-images/content11.png "濃い Mac ウィンドウの UI の例")](modern-cocoa-apps-images/content11.png#lightbox)

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

静的な`GetAppearance`のメソッド、`NSAppearance`システムから名前付きの外観を取得するクラスが使用されます (ここで`NSAppearance.NameVibrantDark`)。

Apple では、システムの外観を使用するための次の推奨事項があります。

- 名前付きの色をハードコーディングされた値より優先 (など`LabelColor`と`SelectedControlColor`)。
- 可能な場合は、システム標準のコントロールのスタイルを使用します。

システムの外観を使用する macOS アプリを自動的に正しく動作するシステム環境設定アプリからのユーザー補助機能を有効にしているユーザー。 その結果、Apple は、macOS アプリの場合、開発者で常にシステムの外観を使用することを提案します。

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>ストーリー ボードでの Ui の設計

ストーリー ボードを使用すると、開発者だけでなく、アプリのユーザー インターフェイスを構成が、視覚化し、UI の設計を個々 の要素のフローのデザインと指定した要素の階層。

コント ローラーは、開発者は合成と Segues 抽象の単位に要素を収集し、一般的な「グルー コード」ビューの階層間を移動するために必要な削除を許可します。

[![](modern-cocoa-apps-images/content12.png "Xcode の Interface Builder では、UI の編集")](modern-cocoa-apps-images/content12.png#lightbox)

詳細についてを参照してください、[ストーリー ボードの概要](~/mac/platform/storyboards/index.md)ドキュメント。

多くの場合、ストーリー ボードで定義されている特定のシーンはビュー階層内で前のシーンからのデータが必要な場合があります。 Apple では、シーンの間で情報を渡すための次の推奨事項があります。

- データの依存関係は必要があります、階層を常に下方向に重ねて表示します。
- この制限が UI の柔軟性はハードコーディング UI 構造の依存関係、しないでください。
- 使用C#のインターフェイスを依存関係を汎用的なデータを提供します。

セグエのソースとして動作しているビュー コント ローラーがオーバーライドできる、`PrepareForSegue`メソッドと、セグエの前にデータを渡す) など、初期化が必要な操作が実行されている場合に対象ビュー コント ローラーを表示します。 例:

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

## <a name="propagating-actions"></a>アクションを伝達します。

MacOS アプリの設計に基づきがありますと UI コントロールに対するアクションの最適なハンドラーがありますで他の場所で UI 階層。 これは通常、メニューと、アプリの UI の残りの部分から独立した独自のシーンに置かれているメニュー項目の当てはまります。

このような状況を処理するために開発者がカスタム アクションを作成し、レスポンダー チェーン アクションを渡すことができます。 詳細情報を参照してください、[カスタム ウィンドウの動作を扱う](~/mac/user-interface/menu.md)ドキュメント。

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>最新の Mac の機能

Apple には、macOS Sierra を使用するなど、Mac プラットフォームのほとんどに開発者にいくつかのユーザー向けの機能が含まれます。

- **NSUserActivity** -これにより、アプリをユーザーが現在に関係するアクティビティについて説明します。 `NSUserActivity` ハンドオフ、ユーザーのデバイスのいずれかで開始されたアクティビティの集荷および別のデバイスで続きをでしたをサポートするために最初に作成されました。 `NSUserActivity` iOS と macOS で同じは works くださいを参照してください、[ハンドオフの概要](~/ios/platform/handoff.md)詳細については、iOS ドキュメント。
- **Mac 上の Siri** -Siri が現在のアクティビティを使用して (`NSUserActivity`) コマンドを発行できるユーザー コンテキストを提供します。
- **状態復元**- macOS でし、その後、アプリが自動的には、アプリが終了のユーザーが以前の状態に返されます。 開発者は、エンコードして、ユーザー インターフェイスがユーザーに表示される前に、一時的な UI の状態を復元状態の復元の API を使用できます。 アプリがある場合`NSDocument`状態の復元が自動的に処理します。 状態の復元を有効にする非`NSDocument`ベースのアプリ設定、`Restorable`の`NSWindow`クラスを`true`します。
- **クラウド内のドキュメント**-アプリを macOS Sierra では、前に、ユーザーの iCloud ドライブ内のドキュメントの操作に明示的にオプトインする必要があります。 MacOS Sierra で、ユーザーの**デスクトップ**と**ドキュメント**フォルダーに可能性があります、システムによって自動的にドライブの iCloud と同期します。 その結果、ドキュメントのローカル コピーを削除して、ユーザーのコンピューター上の領域を解放する可能性があります。 `NSDocument` アプリでは、この変更は自動的に処理します。 その他のすべてのアプリの種類が使用する必要があります、`NSFileCoordinator`ドキュメントの読み取りと書き込みを同期します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、いくつかのヒント、機能と手法開発者 Xamarin.Mac で最新の macOS アプリのビルドに使用することについて説明しました。



## <a name="related-links"></a>関連リンク

- [macOS のサンプル](https://developer.xamarin.com/samples/mac/)
