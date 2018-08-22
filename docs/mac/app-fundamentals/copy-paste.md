---
title: Xamarin.Mac をコピーして貼り付け
description: この記事では、コピーを提供して、Xamarin.Mac アプリケーションで貼り付けるクリップボードの操作について説明します。 作業する方法を示します標準のデータ型の特定のアプリ内のカスタム データをサポートする方法と複数のアプリ間で共有できます。
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 728e0264f7da8f3adfef360dd473772dd7e28a11
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40250991"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Xamarin.Mac をコピーして貼り付け

_この記事では、コピーを提供して、Xamarin.Mac アプリケーションで貼り付けるクリップボードの操作について説明します。作業する方法を示します標準のデータ型の特定のアプリ内のカスタム データをサポートする方法と複数のアプリ間で共有できます。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションで c# と .NET を使用する場合は、同じペースト ボード (コピーと貼り付け) サポートに Objective C で作業する開発者がアクセス権があります。

この記事では、Xamarin.Mac アプリで、クリップボードを使用する 2 つの主な方法を取り上げてするされます。

1. **標準のデータ型**-ペースト ボード操作が、2 つの関連のないアプリ間で通常実行、ために、どちらのアプリは、もう一方をサポートするデータの種類を認識します。 共有する可能性を最大化するには、ペースト (一般的なデータ型の標準セットを使用して) 指定した項目の複数の表現を保持できる、これにより、自社のニーズに最適のバージョンを選択するアプリを使う。
2. **カスタム データ**- コピーと、クリップボードで処理されるカスタム データ型を定義することができます、Xamarin.Mac 内で複雑なデータの貼り付けをサポートします。 たとえば、ベクター描画アプリケーションをコピーして貼り付けるは、複数のデータ型およびポイントで構成される複雑な図形、ユーザーができます。

[![実行中のアプリの使用例](copy-paste-images/intro01.png "実行中のアプリの例")](copy-paste-images/intro01-large.png#lightbox)

この記事では、サポートのコピーと貼り付けの操作に、Xamarin.Mac アプリケーションでのクリップボードの操作の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での c# クラスを Objective-C オブジェクトと UI への要素に使用されます。

## <a name="getting-started-with-the-pasteboard"></a>ペースト ボードの概要

ペースト ボードは、特定のアプリケーション内またはアプリケーション間でデータを交換するための標準化されたメカニズムを表示します。 多くの他の操作は、(ドラッグ & ドロップ アプリケーション サービスなど) もサポートされていますがコピーを処理し、貼り付けの操作、Xamarin.Mac アプリケーションでのクリップボードの一般的な使用います。

すばやくアクセスできる地上から、ここペーストを使用して Xamarin.Mac アプリで単純で実際的な概要を開始します。 後で、クリップボードの動作方法と使用方法の詳細については提供します。

この例では、私たちがするベースの単純なドキュメント アプリケーションを作成するイメージのビューを含むウィンドウを管理します。 ユーザーはコピーして、アプリとにまたは他のアプリ、または同じアプリ内で複数の windows からのドキュメント間でのイメージを貼り付けることになります。

### <a name="creating-the-xamarin-project"></a>Xamarin プロジェクトを作成します。

最初に、私たちはするコピーを追加し、貼り付けることのサポート、新しいドキュメント ベースの Xamarin.Mac アプリを作成するでしょうが。

次の手順で行います。

1. Visual Studio をクリックし、Mac を起動、**新しいプロジェクト.** リンク。
2. 選択**Mac** > **アプリ** > **Cocoa アプリ**、 をクリックし、 **次へ**ボタン。 

    [![Cocoa アプリ プロジェクトを新規作成](copy-paste-images/sample01.png "Cocoa アプリ プロジェクトを新規作成")](copy-paste-images/sample01-large.png#lightbox)
3. 入力`MacCopyPaste`の**プロジェクト名**し、既定値として他のすべてを保持します。 [次へ] をクリックします。 

    [![プロジェクトの名前を設定](copy-paste-images/sample01a.png "プロジェクトの名前を設定")](copy-paste-images/sample01a-large.png#lightbox)

4. をクリックして、**作成**ボタンをクリックします。 

    [![新しいプロジェクトの設定を確認する](copy-paste-images/sample02.png "新しいプロジェクトの設定を確認します。")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>NSDocument を追加します。

次にユーザー設定を追加して、`NSDocument`アプリケーションのユーザー インターフェイスのバック グラウンドのストレージとして機能するクラス。 1 つのイメージ ビューが含まれてし、ビューから既定のクリップボードにイメージをコピーする方法と、既定のクリップボードからイメージを取得し、イメージ ビューに表示する方法。

Xamarin.Mac プロジェクトを右クリックし、 **Solution Pad**選択**追加** > **新しいファイル.**:

![プロジェクトへの追加、NSDocument](copy-paste-images/sample03.png "NSDocument をプロジェクトに追加します。")

**[名前]** に「`ImageDocument`」と入力し、**[新規]** ボタンをクリックします。 編集、 **ImageDocument.cs**クラスし、次のようになります。

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

以下で詳しく、コードをいくつか見てをみましょう。

次のコードは、イメージが使用可能な場合の既定ペースト ボード上の画像データの存在をテストするプロパティを提供します`true`それ以外の場合に返される`false`:。

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

次のコードでは、既定のクリップボードに添付された画像ビューからイメージをコピーします。

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

次のコードは既定のクリップボードからイメージを貼り付けるし、(、クリップボードには、有効なイメージが含まれている) 場合は、添付された画像ビューで表示。

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

このドキュメントでは、Xamarin.Mac アプリのユーザー インターフェイスを作成します。

### <a name="building-the-user-interface"></a>ユーザー インターフェイスの構築

ダブルクリックして、 **Main.storyboard**ファイルを Xcode で開きます。 次に、ツールバーとイメージに追加し、それらを次のように構成します。

[![ツールバーの編集](copy-paste-images/sample04.png "ツールバーの編集")](copy-paste-images/sample04-large.png#lightbox)

コピーを追加し、貼り付けます**イメージ ツール バー アイテム**ツールバーの左側にします。 使用するショートカットとしてそれらを編集 メニューからコピーして貼り付けます。 次に、4 つ追加**イメージ ツールバー項目**ツールバーの右側にします。 これらもいくつかの既定のイメージのイメージの設定に使用します。

ツールバーの使用の詳細についてを参照してください、[ツールバー](~/mac/user-interface/toolbar.md)ドキュメント。

次に、みましょう次 outlet と、ツールバーの項目とイメージの操作にも公開します。

[![Outlet と action を作成する](copy-paste-images/sample05.png "outlet と action を作成します。")](copy-paste-images/sample05-large.png#lightbox)

Outlet と action の操作方法の詳細についてを参照してください、 [Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)のセクション、[こんにちは, Mac](~/mac/get-started/hello-mac.md)ドキュメント。

### <a name="enabling-the-user-interface"></a>ユーザー インターフェイスを有効にします。

Xcode と outlet と action を使用して公開される弊社の UI 要素で作成された、ユーザー インターフェイスでは、UI を有効にするコードを追加する必要があります。 ダブルクリック、 **ImageWindow.cs**ファイル、 **Solution Pad**し、次のようになります。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

以下で詳しくこのコードを見てをみましょう。

インスタンスを最初に、公開しています、`ImageDocument`上で作成したクラス。

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

使用して`Export`、`WillChangeValue`と`DidChangeValue`、セットアップがある、`Document`キー値コーディングし、Xcode でのデータ バインディングを有効にするプロパティ。

私たちも追加しましたを Xcode で UI に次のプロパティを使用してイメージからイメージを公開します。

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

インスタンスと、メイン ウィンドウが読み込まれ、表示、作成、`ImageDocument`クラスし、次のコードにも、UI のイメージをアタッチします。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

最後に、コピーと貼り付けのツールバー項目をクリックすると、ユーザーに対して、呼び出しのインスタンス、`ImageDocument`実際の作業を実行するために。

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>ファイルと編集メニューを有効にします。

最後に実行する必要がありますが、有効にする、**新規**からメニュー項目、**ファイル**メニュー (メイン ウィンドウの新しいインスタンスを作成する) を有効にして、**切り取り**、**コピー**と**貼り付け**メニューから項目を**編集**メニュー。

有効にする、**新規** メニューの項目を編集、 **AppDelegate.cs**ファイルを開き、次のコードを追加します。

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

詳細についてを参照してください、 [Windows の複数の操作](~/mac/user-interface/window.md)のセクション、 [Windows](~/mac/user-interface/window.md)ドキュメント。

有効にする、**切り取り**、**コピー**と**貼り付け**メニュー項目の編集、 **AppDelegate.cs**ファイルを開き、次のコードを追加します。

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

各メニュー項目の現在の最上位キー ウィンドウを取得し、キャスト、`ImageWindow`クラス。

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

呼び出して、`ImageDocument`処理、コピーと貼り付けの操作ウィンドウのクラスのインスタンス。 例えば: 

```csharp
window.Document.CopyImage (sender);
```

のみが必要で**切り取り**、**コピー**と**貼り付け**メニュー項目がある場合にアクセスできるようにイメージの既定のクリップボードやも現在アクティブなウィンドウのイメージ データ。

追加、 **EditMenuDelegate.cs** Xamarin.Mac プロジェクトにファイルを開き、次のようになります。

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

現在、最上位ウィンドウを取得するここでも、しを使用して、その`ImageDocument`クラスのインスタンスに必要な画像データが存在するかどうか。 使用して、`MenuWillHighlightItem`方法を有効にするか、各項目を無効にするこの状態に基づいています。

編集、 **AppDelegate.cs**ファイル、`DidFinishLaunching`メソッドの次のようになります。
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

最初に、自動的に有効にし、メニュー項目の編集 メニューの無効化を無効にします。 次に、インスタンスのアタッチ、`EditMenuDelegate`上で作成したクラス。

詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)ドキュメント。

### <a name="testing-the-app"></a>アプリのテスト

すべての準備で、アプリケーションをテストする準備ができました。 ビルド アプリを実行して、メイン インターフェイスが表示されます。

![アプリケーションを実行している](copy-paste-images/run01.png "アプリケーションを実行します。")

場合は、[編集] メニューを開くことに注意**切り取り**、**コピー**と**貼り付け**がないためイメージ、イメージにも、または既定のクリップボードで無効になっています。

![[編集] メニューを開いて](copy-paste-images/run02.png "編集 メニューを開く")

イメージをイメージにも追加して編集 メニューを再度開く場合、項目は有効になりました。

![アイテムの編集 メニューの表示が有効になって](copy-paste-images/run03.png "アイテムの編集 メニューの表示が有効になっています。")

選択すると、イメージのコピー**新規**ファイル メニューからは、新しいウィンドウにそのイメージを貼り付けることができます。

![イメージを新しいウィンドウに貼り付け](copy-paste-images/run04.png "イメージを新しいウィンドウに貼り付け")

次のセクションでは、Xamarin.Mac アプリケーションでのクリップボードの使用方法について詳しく説明をみましょう。

## <a name="about-the-pasteboard"></a>ペーストについて

MacOS (旧称 OS X) で、クリップボード (`NSPasteboard`) いくつかのサーバーは、コピーし貼り付け、ドラッグ アンド ドロップなどとアプリケーション サービスを処理のサポートを提供します。 次のセクションでは、いくつかのペースト ボード主要な概念について詳しく見てをみましょう。

### <a name="what-is-a-pasteboard"></a>クリップボードとは何ですか。

`NSPasteboard`クラスには、アプリケーション間、または特定のアプリ内の情報を交換するための標準化されたメカニズムが用意されています。 クリップボードの主な機能は、コピーと貼り付けの操作を処理するためです。

1. ユーザーがアプリ内の項目を選択し、使用、**切り取り**または**コピー**メニュー項目を選択した項目の 1 つまたは複数の表現は、クリップボードに配置されます。
2. ユーザーが使用する場合、**貼り付け**メニュー項目を処理できるデータのバージョンのクリップボードからコピーし、アプリに追加 (同じアプリまたは別の 1 つ) の中で。

不明確ペースト ボードの使用は、検索、ドラッグ、ドラッグ アンド ドロップ、およびアプリケーション サービスの操作。

- ユーザーは、ドラッグ操作を開始したとき、ドラッグのデータは、クリップボードにコピーされます。 ドラッグ操作は、別のアプリ上にドロップを伴って終了した場合、そのアプリは、クリップボードからデータをコピーします。
- 翻訳サービスを変換するデータは、要求元のアプリで、クリップボードにコピーされます。 アプリケーション サービスでは、クリップボードからデータを取得、貼り付け、クリップボード上のデータをバックアップし、翻訳は。

最も単純な形式は、特定のアプリ内やアプリ間のデータを移動し、そのため、アプリのプロセスの外部での特殊なグローバル メモリ領域内に存在するペーストを使用します。 ペーストの概念が簡単には、grasps 検討すべきいくつかのより複雑な詳細があります。 これらについては、以下で詳しく説明します。

### <a name="named-pasteboards"></a>名前付きペースト

クリップボードでは、パブリックまたはプライベートにすることができ、さまざまな目的で、アプリケーション内、または複数のアプリ間での使用可能性があります。 macOS には、それぞれ明確に定義された、特定の使用状況がいくつかの標準的なペーストが用意されています。

- `NSGeneralPboard` -既定のクリップボードの**切り取り**、**コピー**と**貼り付け**操作。
- `NSRulerPboard` -サポート**切り取り**、**コピー**と**貼り付け**に対する操作**ルーラー**します。
- `NSFontPboard` -サポート**切り取り**、**コピー**と**貼り付け**に対する操作`NSFont`オブジェクト。
- `NSFindPboard` -サポートするアプリケーションに固有では、検索テキストを共有できるパネルを検索します。
- `NSDragPboard` -サポート**ドラッグ アンド ドロップ**操作。

ほとんどの場合、システム定義ペーストのいずれかを使用します。 独自のペーストを作成することが必要な状況である可能性があります。 このような状況で使用することができます、`FromName (string name)`のメソッド、`NSPasteboard`指定の名前を持つカスタム クリップボードを作成するクラス。

必要に応じて、呼び出すことができます、`CreateWithUniqueName`のメソッド、`NSPasteboard`クリップボードを一意の名前を作成するクラス。

### <a name="pasteboard-items"></a>ペースト ボード項目

アプリケーションが、クリップボードに書き込むデータの各部分と見なされます、_クリップボード アイテム_クリップボードが同時に複数の項目を保持できるとします。 これにより、アプリが、(プレーン テキストの場合のみ) などを処理できるデータのみを読み取ることができます (たとえば、プレーン テキストと書式設定されたテキスト) のクリップボードと取得中のアプリにコピーするデータの複数のバージョンを記述できます。

### <a name="data-representations-and-uniform-type-identifiers"></a>データ表現と同じ型である識別子

ペースト ボードの操作は、通常、またはデータの種類の知識がない 2 つ (または複数) の間でアプリケーションをそれぞれ処理できます。 前のセクションで述べたように、情報を共有する可能性を最大化する、クリップボードを保持できますコピーし、貼り付けされているデータの複数の表現。

各表現を使用して、Uniform 型識別子 (UTI)、これは何も表示されている日付の種類を一意に識別する単純な文字列よりも詳細で識別されます (詳細については、Apple を参照してください[Uniform 型識別子の概要](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319)ドキュメント)。 

カスタム データ型 (たとえば、ベクター描画アプリでの描画のオブジェクト) を作成する場合は、独自の UTI を一意にコピーで識別し、貼り付けの操作を作成できます。

アプリをクリップボードからコピーしたデータを貼り付ける準備は、(いずれかが存在する) 場合に、その機能を最適な表現を見つける必要があります。 豊富な種類使用可能な (ワード プロセッシング アプリなどの書式設定されたテキスト)、一般には必要な (プレーン テキストの単純なテキスト エディターの) として使用可能な最も簡単なフォームにフォールバックします。

<a name="Promised_Data" />

### <a name="promised-data"></a>保証されたデータ

一般に、アプリ間で共有を最大化するには、可能な限りとしてコピーするデータの多くの表現を提供する必要があります。 ただし、時間やメモリの制約によりない可能性がある実際には、クリップボードに各データ型を記述するは実用的です。

このような状況で、クリップボードに、最初のデータ表現を配置することができ、受信側のアプリが生成された、その場で貼り付け操作の直前にできる、さまざまな表現を要求できます。

1 つ以上の他の表現に準拠しているオブジェクトによって提供されることを指定します、クリップボードに最初のアイテムを配置すると、`NSPasteboardItemDataProvider`インターフェイス。 これらのオブジェクトが受信側のアプリからの要求では、オンデマンドで余分な表現を提供します。

### <a name="change-count"></a>変更数

各クリップボードを維持、_変更数_単位ごとに新しい所有者がときに宣言されています。 アプリでは、クリップボードの内容が前回変更数の値をチェックして調べること変更されたかどうかを判断できます。

使用して、`ChangeCount`と`ClearContents`のメソッド、`NSPasteboard`特定のクリップボードの変更の数を変更するクラス。

## <a name="copying-data-to-a-pasteboard"></a>データをクリップボードにコピーします。

コピー操作を行うには、最初、クリップボードへのアクセス、既存の内容をクリアして、クリップボードに必要なデータの多くの表現を記述します。

例えば:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

通常、記述したことをだけ全般のクリップボードに上記の例で行ったよう。 送信する任意のオブジェクト、`WriteObjects`メソッド*する必要があります*に準拠している、`INSPasteboardWriting`インターフェイス。 いくつかの組み込みクラス (など`NSString`、 `NSImage`、 `NSURL`、 `NSColor`、`NSAttributedString`と`NSPasteboardItem`) に自動的にこのインターフェイスに準拠します。

準拠している必要があります、クリップボードにカスタム データ クラスを作成する場合、`INSPasteboardWriting`インターフェイスまたはのインスタンスでラップされる、`NSPasteboardItem`クラス (を参照してください、[データ型のカスタム](#Custom_Data_Types)以下のセクション)。

## <a name="reading-data-from-a-pasteboard"></a>クリップボードからデータを読み取る

前述のように、アプリ間でデータを共有する可能性を最大化する、コピーされたデータの複数の表現書き込むことができます、クリップボードにします。 (いずれかが存在する) 場合は、その機能の豊富なバージョンを選択する受信側のアプリの責任です。

### <a name="simple-paste-operation"></a>単純な貼り付けの操作

ペースト ボードを使用してデータを読み取り、`ReadObjectsForClasses`メソッド。 2 つのパラメーターが必要になります。

1. 配列の`NSObject`ベースのクラスの種類をクリップボードからの読み取り。 必要がありますを注文するこの最も必要なデータ型で最初に、優先順位の高い残りの型。
2. (特定の URL のコンテンツ タイプに制限する) などの追加の制約を含むディクショナリまたは他の制約が必要ない場合は空のディクショナリ。

メソッドは、渡された条件を満たす項目の配列を返します、したがって要求されたデータ型の同じ番号を最大で含まれています。 要求された型がない、空の配列が返されますこともできます。

たとえば、次のコードかどうかをチェックする`NSImage`全般のクリップボードに存在し、場合は、イメージの表示。

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>複数のデータ型を要求します。

Xamarin.Mac アプリケーションの作成中の種類に基づいて、ある可能性があります貼り付けられているデータの複数の表現を処理できません。 このような状況では、クリップボードからデータを取得するための 2 つのシナリオがあります。

1. 1 回の呼び出し、`ReadObjectsForClasses`メソッドを (優先順) にする必要のある表現のすべての配列。
2. 複数の呼び出しを行う、`ReadObjectsForClasses`メソッドの別の配列を求める毎回の型します。

参照してください、**貼り付け操作の単純な**クリップボードからデータを取得する方法の詳細については、前述の「します。

### <a name="checking-for-existing-data-types"></a>既存のデータ型のチェック

実際に、クリップボードからデータを読み取らないまま特定のデータ表現が、クリップボードに含まれているかどうかは確認する時間がある (有効にするなど、**貼り付け**メニュー項目の有効なデータが存在する場合にのみ)。

呼び出す、`CanReadObjectForClasses`に指定された型が含まれているかどうか、クリップボードのメソッド。

たとえば、次のコードを決定全般クリップボードが含まれるかどうか、`NSImage`インスタンス。

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Url をクリップボードから読み取る

特定の Xamarin.Mac アプリの機能に基づいて、ある可能性があります、クリップボードから Url を読み取る必要ですが、唯一指定された一連の (ファイルまたは特定のデータ型の Url を指す) などの条件を満たしている場合。 このような状況では、2 番目のパラメーターを使用して追加の検索条件を指定できます、`CanReadObjectForClasses`または`ReadObjectsForClasses`メソッド。

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>カスタム データ型

ときに、Xamarin.Mac アプリから独自のカスタム型をクリップボードに保存する必要があります。 たとえば、コピーと貼り付けのオブジェクトを描画するユーザーを許可するアプリを描画するベクター。

このような状況から継承するようにカスタム データ クラスを設計する必要があります`NSObject`いくつかのインターフェイスに準拠していると (`INSCoding`、`INSPasteboardWriting`と`INSPasteboardReading`)。 必要に応じて、使用、`NSPasteboardItem`コピーまたは貼り付けするデータをカプセル化します。

これらのオプションについては、以下で詳しくで説明します。

### <a name="using-a-custom-class"></a>カスタム クラスを使用します。

このセクションではこのドキュメントの開始時に作成した単純な例のアプリを拡張してする windows 間でコピーと貼り付けはイメージに関する情報を追跡するカスタム クラスを追加します。

プロジェクトに新しいクラスを追加し、付けます**ImageInfo.cs**します。 ファイルを編集し、次のようになります。

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

以下のセクションで、このクラスを詳しく見てをみましょう。

#### <a name="inheritance-and-interfaces"></a>継承とインターフェイス

準拠している必要がありますカスタム データ クラスに書き込まれたまたは、クリップボードから読み取ることができます、前に、`INSPastebaordWriting`と`INSPasteboardReading`インターフェイス。 継承する必要がありますしてさらに、`NSObject`にも準拠していると、`INSCoding`インターフェイス。

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

クラスは、OBJECTIVE-C を公開することも必要がありますを使用して、`Register`ディレクティブとは、任意の必要なプロパティまたはメソッドを使用して公開する必要がある`Export`します。 例えば:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

イメージの名前と型 (jpg、png など) で、このクラスを格納するデータの 2 つのフィールドを公開しました。 

詳細については、次を参照してください、 [c# を公開するクラス/メソッドを Objective-c](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)ドキュメントについては、説明、`Register`と`Export`属性。Objective C のオブジェクトを UI 要素、c# クラスを接続するために使用します。

#### <a name="constructors"></a>コンストラクター

2 つのコンス トラクターが (Objective C を正しく公開される) をクリップボードから読み取ることができるように、カスタム データ クラスの必要になります。

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

最初に、公開、_空_コンス トラクターの既定の OBJECTIVE-C メソッド `init`します。

次に、公開、`NSCoding`エクスポート名の下に貼り付けるときに、クリップボードからオブジェクトの新しいインスタンスを作成するために使用する準拠のコンス トラクター`initWithCoder`します。

このコンス トラクターは、 `NSCoder` (によって作成された、`NSKeyedArchiver`クリップボードに書き込まれたとき)、キー/値ペアになっているデータを抽出し、データ クラスのプロパティ フィールドに保存します。

#### <a name="writing-to-the-pasteboard"></a>クリップボードへの書き込み

準拠することで、`INSPasteboardWriting`インターフェイス、必要があります、2 つの方法と必要に応じて、3 つ目のメソッドを公開するクラスは、クリップボードに書き込むことができるようにします。

まず、ペーストどのようなデータ型の表現を記述できるカスタム クラスを通知する必要があります。

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

各表現を使用して、統一型識別子 (UTI) は提示されたデータの種類を一意に識別する単純な文字列にすぎませんが識別されます (詳細については、Apple を参照してください[Uniform 型識別子の概要](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319)ドキュメント)。

独自の UTI を作成、カスタム形式の場合:"com.xamarin.image info"(アプリ Id と同じように逆引きの表記では、メモ)。 クラスは、標準の文字列をクリップボードに書き込むことも (`public.text`)。 

次に、実際には、クリップボードに書き込まれる要求の形式でオブジェクトを作成する必要があります。

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

`public.text`型、単純な返す書式設定された`NSString`オブジェクト。 カスタム`com.xamarin.image-info`使用している型を`NSKeyedArchiver`と`NSCoder`キー/値ペアになっているアーカイブにカスタム データ クラスをエンコードするインターフェイス。 実際には、エンコードを処理するために次のメソッドを実装する必要があります。

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

個々 のキー/値のペアは、エンコーダーには書き込まれ、上で追加した 2 つ目のコンス トラクターを使用してデコードされます。

必要に応じて、クリップボードにデータを書き込むときに、すべてのオプションを定義する次のメソッドを含めることができます。

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

現在は、`WritingPromised`オプションは利用でき、指定された型が約束ののみと、クリップボードに実際に書き込まれますために使用する必要があります。 詳細についてを参照してください、[約束データ](#Promised_Data)前のセクション。

これらメソッドと共に、カスタム クラスをクリップボードに書き込む、次のコードを使用できます。

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>クリップボードからの読み取り

準拠することで、`INSPasteboardReading`インターフェイス、カスタム データ クラスをクリップボードから読み取れるように、3 つのメソッドを公開する必要があります。

まず、ペーストどのようなデータ型のカスタム クラスは、クリップボードから読み取り可能な表現を指示する必要があります。

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

これらの単純な Uti として定義され、同じ種類で定義されていることをもう一度、 **、クリップボードへの書き込み**前のセクション。

次に、ペーストを指示する必要があります_方法_次のメソッドを使用して各 UTI の種類の読み取りは。

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

`com.xamarin.image-info`型で作成したキー/値ペアをデコードするクリップボードを示して、`NSKeyedArchiver`すると、クリップボードに呼び出すことによって、クラスを記述、`initWithCoder:`クラスに追加したコンス トラクター。

最後に、他の UTI データ表現をクリップボードから読み取りに次のメソッドを追加する必要があります。

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

これらすべてのメソッドにカスタム データ クラスは、次のコードを使用してクリップボードから読み取ることが可能します。

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>NSPasteboardItem を使用します。

カスタム項目をカスタム クラスの作成が保証されないをクリップボードに書き込む必要がありますまたは必要なだけの一般的な形式でデータを提供する場合の時間である可能性があります。 このような場合は、使用することができます、`NSPasteboardItem`します。

A`NSPasteboardItem`きめ細かな制御を提供するデータをクリップボードに書き込まれ、- の一時的なアクセスに適していますが、クリップボードに書き込まれた後の破棄する必要があります。

#### <a name="writing-data"></a>データの書き込み

カスタム データを書き込む、`NSPasteboardItem`カスタムを提供する必要があります`NSPasteboardItemDataProvider`します。 プロジェクトに新しいクラスを追加し、付けます**ImageInfoDataProvider.cs**します。 ファイルを編集し、次のようになります。

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

カスタム データ クラスと同様、使用する必要があります、`Register`と`Export`OBJECTIVE-C に公開するためのディレクティブ クラスを継承する必要があります`NSPasteboardItemDataProvider`および実装する必要があります、`FinishedWithDataProvider`と`ProvideDataForType`メソッド。

使用して、`ProvideDataForType`でラップされるデータを提供するメソッドを`NSPasteboardItem`次のようにします。

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

ここでは、(名前とイメージの種類)、イメージに関する 2 つの情報を保存して、単純な文字列に書き込みは (`public.text`)。

型は、次のコードを使用してのクリップボードにデータを書き込みます。

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>データの読み取り

クリップボードからデータを読み取るには、次のコードを使用します。

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>まとめ

この記事では、サポートのコピーと貼り付けの操作に、Xamarin.Mac アプリケーションでのクリップボードの使用方法について詳しく説明をしました。 まず、標準のペースト操作を使い慣れてを取得する簡単な例を紹介しました。 次に、ペーストし、そこからデータを読み書きする方法について詳しく説明がかかりました。 最後に、カスタム データ型を使用してコピーと、アプリ内で複合データ型の貼り付けをサポートすることについて説明しました。



## <a name="related-links"></a>関連リンク

- [MacCopyPaste (サンプル)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ペースト ボードのプログラミング ガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
