---
title: Xamarin.Mac でコピーして貼り付ける
description: この記事では、コピーを提供し、Xamarin.Mac アプリケーションに貼り付けますペーストの扱いについて説明します。 作業する方法を示しますの複数のアプリと特定のアプリ内のカスタム データをサポートする方法の間で共有できる標準データ型。
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: becdec771949584919595c84b13ae9e05bfd377b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791898"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Xamarin.Mac でコピーして貼り付ける

_この記事では、コピーを提供し、Xamarin.Mac アプリケーションに貼り付けますペーストの扱いについて説明します。作業する方法を示しますの複数のアプリと特定のアプリ内のカスタム データをサポートする方法の間で共有できる標準データ型。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき、同じペースト ボード (コピーと貼り付け) のサポートを Objective C で作業する開発者が持つアクセス権があります。

この記事の内容おで取り上げる Xamarin.Mac アプリでペーストを使用する 2 つの主な方法。

1. **標準データ型**-ペースト ボードの操作が通常実行されます。 関連付けられていない 2 つのアプリ間で、ため、どちらのアプリは、他のをサポートするデータの種類を認識します。 共有する可能性を最大化するには、(一般的なデータ型の標準セットを使用して) 指定した項目の複数の表現を保持できるペースト、かかるアプリがニーズに最適なであるバージョンを選択を許可このします。
2. **カスタム データ**- コピーとペーストによって処理されるカスタム データ型を定義することができます、Xamarin.Mac 内で複雑なデータの貼り付けをサポートするためにします。 たとえば、ベクター描画アプリケーションでは、ユーザーをコピーして、複数のデータ型とポイントで構成される複雑な図形を貼り付けます。

[![実行中のアプリの使用例](copy-paste-images/intro01.png "実行中のアプリの例")](copy-paste-images/intro01-large.png#lightbox)

この記事でしれませんにはペースト Xamarin.Mac アプリケーション サポート コピーと貼り付けの操作での操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での c# クラスを Objective-C オブジェクトと UI への要素に使用されます。

## <a name="getting-started-with-the-pasteboard"></a>ペースト ボードの概要

ペースト内または特定のアプリケーションでアプリケーション間でデータを交換するための標準化された機構を表示します。 Xamarin.Mac アプリケーションでペーストの典型的な使用が、コピーを処理し、貼り付けの操作、その他の操作の数が (ドラッグ/ドロップとアプリケーション サービスなど) もサポートされています。

すばやく地上から、しようとして、単純で実用的な使用の概要ペースト Xamarin.Mac アプリ内で開始します。 その後、ペーストのしくみと使用方法の詳細については提供します。

この例では、ここでは作成イメージ ビューを含むウィンドウを管理する単純なドキュメントのベース アプリケーションです。 ユーザーをコピーして、アプリでとをまたはその他のアプリまたは同じのアプリ内で複数のウィンドウからドキュメント間でイメージを貼り付けることになります。

### <a name="creating-the-xamarin-project"></a>Xamarin プロジェクトの作成

最初は、ここを新しいドキュメント ベース Xamarin.Mac アプリケーションを作成することおコピーを追加してするサポートを貼り付けます。

次の手順で行います。

1. Mac とクリックの Visual Studio を起動、**新しいプロジェクト.** リンクします。
2. 選択**Mac** > **アプリ** > **Cocoa アプリ**、をクリックして、**次**ボタン。 

    [![新しい Cocoa アプリ プロジェクトを作成する](copy-paste-images/sample01.png "Cocoa アプリ プロジェクトを新規作成")](copy-paste-images/sample01-large.png#lightbox)
3. 入力`MacCopyPaste`の**プロジェクト名**し、既定値として他のすべてを保持します。 [次へ] をクリックします。 

    [![プロジェクトの名前を設定](copy-paste-images/sample01a.png "プロジェクトの名前を設定")](copy-paste-images/sample01a-large.png#lightbox)

4. クリックして、**作成**ボタンをクリックします。 

    [![新しいプロジェクトの設定を確認する](copy-paste-images/sample02.png "新しいプロジェクトの設定を確認します。")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>NSDocument を追加します。

次にユーザー設定を追加します`NSDocument`アプリケーションのユーザー インターフェイスの背景の記憶域として機能するクラス。 1 つのイメージ ビューが含まれて、既定ペースト ボードへのビューからイメージをコピーする方法と既定ペーストからイメージを取得し、イメージのビューに表示する方法が理解されます。

Xamarin.Mac プロジェクトを右クリックし、**ソリューション パッド**選択**追加** > **新しいファイル.**:

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

以下で詳細を見て、コードの一部を見てみましょう。

次のコードは、イメージがある場合、既定ペースト ボード上の画像データの存在をテストするプロパティを提供`true`それ以外の場合に返される`false`:

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

次のコードでは、既定ペースト ボードに接続されているイメージのビューからイメージをコピーします。

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

次のコードは既定ペーストからイメージを貼り付けるし、(ペースト ボードには、有効なイメージが含まれている) 場合、アタッチされた状態で表示します。

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

このドキュメントで Xamarin.Mac アプリ用のユーザー インターフェイスを作成します。

### <a name="building-the-user-interface"></a>ユーザー インターフェイスの構築

ダブルクリックして、 **Main.storyboard**ファイルを Xcode で開きます。 次に、ツールバーとイメージに追加し、ように構成します。

[![ツールバーを編集](copy-paste-images/sample04.png "編集ツールバー")](copy-paste-images/sample04-large.png#lightbox)

コピーを追加し、貼り付けます**イメージ ツールバー項目**ツールバーの左側にします。 使用するショートカットとしてのこれらを編集 メニューからコピーして貼り付けます。 次に、4 つの追加**イメージ ツールバー項目**ツールバーの右側にします。 これらもいくつかの既定のイメージのイメージの設定に使用されます。

ツールバーの使用の詳細についてを参照してください、[ツールバー](~/mac/user-interface/toolbar.md)ドキュメント。

次に、みましょう以下のコンセントと、ツールバーの項目とイメージのアクションにも公開します。

[![コンセントとアクションの作成](copy-paste-images/sample05.png "コンセントとアクションの作成")](copy-paste-images/sample05-large.png#lightbox)

コンセントとアクションの詳細についてを参照してください、[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)のセクションで、[こんにちは, Mac](~/mac/get-started/hello-mac.md)ドキュメント。

### <a name="enabling-the-user-interface"></a>ユーザー インターフェイスを有効にします。

Xcode およびコンセントとアクションを介して公開される弊社の UI 要素で作成して、ユーザー インターフェイスでは、UI を有効にするコードを追加する必要があります。 ダブルクリックして、 **ImageWindow.cs**ファイルを**ソリューション パッド**され次のようになります。

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

以下に詳しくは、このコードを見るを見てみましょう。

インスタンスを最初に、公開、`ImageDocument`上で作成したクラス。

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

使用して`Export`、`WillChangeValue`と`DidChangeValue`、セットアップがある、`Document`キーと値のコーディングおよび Xcode でのデータ バインディングを有効にするプロパティです。

私たちも追加されました Xcode で UI に次のプロパティを使用してイメージからイメージを公開します。

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

インスタンスを作成のメイン ウィンドウが読み込まれ、表示、ときに、`ImageDocument`クラスし、次のコードにも、UI のイメージをアタッチします。

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

最後に、コピーと貼り付けのツールバー項目をクリックすると、ユーザーへの応答、呼び出しのインスタンス、`ImageDocument`クラスを実際の作業を行うには。

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>ファイル サービスおよび編集のメニューを有効にします。

最後に行う必要がありますが有効にする、**新規**からメニュー項目、**ファイル**メニュー (メイン ウィンドウの新しいインスタンスを作成する) を有効にして、**切り取り**、**コピー**と**貼り付け**からのメニュー項目、**編集**メニュー。

有効にする、**新規**メニュー項目、編集、 **<code>appdelegate.cs</code>** ファイルし、次のコードを追加します。

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

詳細についてを参照してください、[付き](~/mac/user-interface/window.md)のセクションで、 [Windows](~/mac/user-interface/window.md)ドキュメント。

有効にする、**切り取り**、**コピー**と**貼り付け**メニュー項目を編集、 **<code>appdelegate.cs</code>** ファイルし、次のコードを追加します。

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

各メニュー項目は、現在の最上位キー ウィンドウを取得し、キャストを弊社`ImageWindow`クラス。

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

呼び出して、`ImageDocument`および貼り付け操作をコピーを処理するには、そのウィンドウのクラスのインスタンス。 例えば: 

```csharp
window.Document.CopyImage (sender);
```

のみが必要**切り取り**、**コピー**と**貼り付け**イメージ既定ペースト ボード上、または現在のアクティブなウィンドウのもイメージ内のデータのメニュー項目がある場合は、アクセスできるようにします。

追加してみましょう。、 **EditMenuDelegate.cs** Xamarin.Mac プロジェクトへのファイルと、次のようになります。

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

ここでも、現在、最上位のウィンドウを取得し、使用して、その`ImageDocument`クラスのインスタンスは、必要なイメージ データが存在するかどうかを参照してください。 私たちを使用して、`MenuWillHighlightItem`この状態に基づくメソッドを有効にするにまたは各項目を無効にします。

編集、 **<code>appdelegate.cs</code>** ファイルし、`DidFinishLaunching`次のようなメソッドの検索。
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

最初に、自動的に有効にし、[編集] メニューのメニュー項目の無効化を無効にしました。 次のインスタンスをアタッチ、`EditMenuDelegate`上で作成したクラスです。

詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)ドキュメント。

### <a name="testing-the-app"></a>アプリのテスト

配置内のすべてのアプリケーションをテストする準備が整いました。 ビルド アプリを実行して、メインのインターフェイスが表示されます。

![アプリケーションを実行している](copy-paste-images/run01.png "アプリケーションを実行します。")

場合は、[編集] メニューを開くと、なお**切り取り**、**コピー**と**貼り付け**がないためイメージ、イメージにも、または既定ペーストで無効になっています。

![[編集] メニューを開いて](copy-paste-images/run02.png "編集 メニューを開く")

イメージをイメージに追加し、[編集] メニューを再び開き、項目が今すぐ有効になります。

![編集メニュー項目の表示が有効になっている](copy-paste-images/run03.png "アイテムの編集 メニューの表示が有効になっています。")

イメージをコピーしを選択する場合**新規**ファイル メニューから新しいウィンドウにそのイメージを貼り付けることができます。

![イメージを新しいウィンドウに貼り付け](copy-paste-images/run04.png "イメージを新しいウィンドウに貼り付け")

次のセクションでは、ペースト Xamarin.Mac アプリケーションでの操作についての詳細をみましょう。

## <a name="about-the-pasteboard"></a>ペーストについて

(旧称 OS X) macOS でペースト (`NSPasteboard`) およびアプリケーション サービスを使用して、コピーし貼り付け、ドラッグ アンド ドロップなどにいくつかのサーバーが処理のサポートを提供します。 次のセクションで、いくつかのペースト ボード主要な概念について詳しく見てをみましょう。

### <a name="what-is-a-pasteboard"></a>ペースト ボードとは何ですか。

`NSPasteboard`クラスには、アプリケーション間、または特定のアプリ内の情報を交換するための標準化された機構が用意されています。 ペースト ボードの主な機能は、コピーと貼り付けの操作を処理するためです。

1. ユーザーがアプリ内で項目を選択し、使用、**切り取り**または**コピー**メニュー項目、選択された項目の 1 つまたは複数の表現はペーストに配置されます。
2. ユーザーを使用する場合、**貼り付け**メニュー項目を処理できるデータのバージョンのペーストからコピーし、アプリに追加 (同じアプリケーションまたは別の名前) の中でします。

わかりにくいペースト ボードの使用は、検索、ドラッグ、ドラッグ アンド ドロップ、およびアプリケーション サービスの操作。

- ユーザーは、ドラッグ操作を開始する場合は、ドラッグ データがペーストにコピーされます。 ドラッグ操作が別のアプリにドロップダウンが終了した場合、そのアプリはペーストからデータをコピーします。
- 翻訳サービスを変換するデータは、要求元のアプリでペーストにコピーされます。 アプリケーション サービス ペーストからデータを取得、翻訳、ペースト入れデータを貼り付けます。

最も単純な形式では、ペーストは特定のアプリ内、またはアプリ間でデータを移動し、アプリのプロセスの外部で特殊なグローバル メモリ領域に混在させるために使用されます。 中、ペーストの概念を簡単に grasps、考慮すべきいくつかのより複雑な詳細があります。 これらについては、以下で詳しく説明します。

### <a name="named-pasteboards"></a>名前付きペースト

ペースト ボードは、パブリックまたはプライベートにすることができ、さまざまな目的で、アプリケーション内で、または複数のアプリ間で使用することがあります。 macOS には、それぞれ、適切に定義された特定の使用状況がいくつかの標準的なペーストが用意されています。

- `NSGeneralPboard` 既定ペースト**切り取り**、**コピー**と**貼り付け**操作します。
- `NSRulerPboard` -サポート**切り取り**、**コピー**と**貼り付け**に対する操作**ルーラー**です。
- `NSFontPboard` -サポート**切り取り**、**コピー**と**貼り付け**に対する操作`NSFont`オブジェクト。
- `NSFindPboard` -サポートするアプリケーションに固有では、検索テキストを共有するパネルを検索します。
- `NSDragPboard` -サポート**ドラッグ アンド ドロップ**操作します。

ほとんどの場合、システム定義ペーストのいずれかを使用します。 独自のペーストを作成する必要がある状況である可能性があります。 これらの状況で使用することができます、`FromName (string name)`のメソッド、`NSPasteboard`クラスを指定した名前のカスタム ペースト ボードを作成します。

必要に応じて、呼び出すことができます、`CreateWithUniqueName`のメソッド、`NSPasteboard`一意の名前付きペースト ボードを作成するクラス。

### <a name="pasteboard-items"></a>ペースト ボード項目

アプリケーションが、ペーストに書き込むデータの各部分と見なされます、_ペースト項目_ペースト ボードは、同時に複数の項目を格納できます。 この方法でアプリを記述できます (プレーン テキストの場合のみ) などを処理できるデータのみを読み取ることができます (たとえば、プレーン テキストと書式設定されたテキスト)、ペーストおよび取得するアプリケーションにコピーするデータの複数のバージョン。

### <a name="data-representations-and-uniform-type-identifiers"></a>データ表現と uniform 型識別子

ペースト ボードの操作は、通常、またはデータの種類のナレッジを持たない 2 つ (以上) の間でアプリケーションを取りますそれぞれに対応できることです。 前のセクションで説明したように、情報を共有する可能性を最大化する、ペースト保持できるコピーし、貼り付けされているデータの複数の表現。

各表現はを介して、Uniform 型識別子 (ユーティリ ティー)、これは何も表示されている日付の種類を一意に識別する単純な文字列よりも詳細で識別されます (詳細については、Apple を参照してください[Uniform 型識別子の概要](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319)ドキュメント)。 

カスタム データ型 (たとえば、アプリの描画のベクトル内の描画のオブジェクト) を作成する場合は、独自のユーティリ ティーを一意に識別、コピーの貼り付けの操作を作成できます。

アプリは、ペースト ボードからコピーされたデータを貼り付ける準備は、(ある場合)、能力に最適な表現を見つける必要があります。 通常これに、豊富な利用可能な (たとえば書式設定された text 型、ワード プロセッシング アプリケーションの)、必要な (プレーン テキストの単純なテキスト エディターの) として使用できる最も単純なフォームにフォールバックします。

<a name="Promised_Data" />

### <a name="promised-data"></a>約束されていたデータ

一般に、アプリ間で共有を最大化するには、可能な限りとしてコピーするデータの多くの表現を指定する必要があります。 ただし、時間やメモリの制約のためにない可能性がありますペーストに各データ型を実際に作成することは実用的です。

このような状況でペースト ボード上、最初のデータ表現を配置することができ、受信側のアプリが生成されたの随時、貼り付け操作の直前に使用可能なさまざまな表現形式を要求できます。

ペーストに最初の項目を配置すると、1 つ以上の使用可能な他の表現に準拠しているオブジェクトによって提供されることを指定します、`NSPasteboardItemDataProvider`インターフェイスです。 これらのオブジェクトが受信側のアプリからの要求では、必要に応じて、余分な表現を提供します。

### <a name="change-count"></a>変更の数

各ペーストを維持、_変更数_各インクリメントときに新しい所有者が宣言されています。 アプリでは、ペーストの内容が前回変更数の値をチェックして調べること以降に変更されたかどうかを判断できます。

使用して、`ChangeCount`と`ClearContents`のメソッド、`NSPasteboard`特定ペーストの変更の数を変更するクラス。

## <a name="copying-data-to-a-pasteboard"></a>ペースト ボードにデータをコピー

コピー操作を行うには、最初、ペースト ボードにアクセスする、既存の内容をクリアするおよびペースト ボードに必要なデータの多くの表現を記述します。

例えば:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

通常、ありますだけを記述した全般ペースト ボードに上記の例で採用したようです。 送信する任意のオブジェクト、`WriteObjects`メソッド*必要があります*に準拠している、`INSPasteboardWriting`インターフェイスです。 いくつかの組み込みクラス (など`NSString`、 `NSImage`、 `NSURL`、 `NSColor`、 `NSAttributedString`、および`NSPasteboardItem`) 自動的にこのインターフェイスに準拠しています。

準拠している必要がありますペーストするカスタム データ クラスを作成している場合、`INSPasteboardWriting`インターフェイスまたはのインスタンスでラップする、`NSPasteboardItem`クラス (を参照してください、[カスタム データ型](#Custom_Data_Types)以下のセクション)。

## <a name="reading-data-from-a-pasteboard"></a>ペースト ボードからデータを読み込む

前述のよう、アプリ間でデータを共有する可能性を最大化するコピー元のデータの複数の表現書き込むことがペーストします。 (ある場合)、そのような機能の豊富なバージョンを選択する受信側アプリケーションの責任です。

### <a name="simple-paste-operation"></a>単純な貼り付けの操作

ペースト ボードを使用して、データを読み取り、`ReadObjectsForClasses`メソッドです。 これには、2 つのパラメーターが必要です。

1. 配列`NSObject`ペーストから読みたいクラスの型に基づいています。 必要がありますを注文するこの、最も必要なデータ型で最初に、優先内の残りの型にします。
2. (特定の URL のコンテンツ タイプに制限する) などの追加の制約を含むディクショナリまたは他の制約が必要ない場合は空のディクショナリ。

メソッドは、渡された条件を満たす項目の配列を返します、したがって要求されているデータ型の同じ番号には最大でが含まれます。 要求された型のいずれもが存在しいない空の配列が返されますこともできます。

たとえば、次のコードかどうかをチェックする`NSImage`全般ペーストに存在し、存在する場合、画像の表示。

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

作成される Xamarin.Mac アプリケーションの種類に基づいて、ある可能性がありますを貼り付け、データの複数の表現を処理できません。 このような状況では、ペーストからデータを取得するための 2 つのシナリオがあります。

1. 1 つの呼び出しを行う、`ReadObjectsForClasses`メソッドと (優先順序) で目的の形式のすべての配列を提供します。
2. 複数の呼び出しを行う、`ReadObjectsForClasses`メソッドの別の配列を求める型ごとにします。

参照してください、**貼り付け操作を単純な**ペースト ボードからのデータ取得の詳細については、上記セクションです。

### <a name="checking-for-existing-data-types"></a>既存のデータ型のチェック

ペーストから実際には、データを読むことがなく、特定のデータ表現が、ペーストに含まれているかどうかは確認することがあります (有効にするなど、**貼り付け**メニュー項目の有効なデータが存在する場合にのみ)。

呼び出す、`CanReadObjectForClasses`メソッドの特定の型が含まれているかどうかをペーストします。

たとえば、次のコードを決定全般ペースト ボードが含まれるかどうか、`NSImage`インスタンス。

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

### <a name="reading-urls-from-the-pasteboard"></a>ペーストから url の読み取り

特定の Xamarin.Mac アプリの機能に基づいて、ある可能性があります、ペーストから Url を読み取る必要が指定された一連の (ファイル、または特定のデータ型の Url を指す) などの条件を満たしている場合。 このような状況では 2 番目のパラメーターを使用して追加の検索条件を指定することができます、`CanReadObjectForClasses`または`ReadObjectsForClasses`メソッドです。

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>カスタム データ型

ときに、Xamarin.Mac アプリからのペーストに独自のカスタム型を保存する必要があります。 たとえば、ベクター描画コピーと貼り付けの描画オブジェクトにユーザーを許可するアプリです。

このような状況から継承するように、データのカスタム クラスをデザインする必要があります`NSObject`いくつかのインターフェイスに準拠していると (`INSCoding`、`INSPasteboardWriting`と`INSPasteboardReading`)。 必要に応じて、使用することができます、`NSPasteboardItem`コピーまたは貼り付けするデータをカプセル化します。

これらのオプションの両方で取り上げる以下で詳細です。

### <a name="using-a-custom-class"></a>カスタム クラスを使用します。

このセクションでおこのドキュメントの開始時に作成した単純な例のアプリを拡張してする windows 間でイメージをコピーして貼り付けることに関する情報を追跡するためにカスタム クラスを追加します。

新しいクラスをプロジェクトに追加し、それを呼び出す**ImageInfo.cs**です。 ファイルを編集し、次のようになります。

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

次のセクションでは、このクラスの詳細についてをみましょう。

#### <a name="inheritance-and-interfaces"></a>継承とインターフェイス

準拠する必要がありますしてカスタム データ クラスに書き込まれるまたはペースト ボードから読み取ることができます、前に、`INSPastebaordWriting`と`INSPasteboardReading`インターフェイスです。 継承する必要がありますしてさらに、`NSObject`にも従っていると、`INSCoding`インターフェイス。

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

クラスは、OBJECTIVE-C を公開することも必要がありますを使用して、`Register`ディレクティブとそれを任意の必要なプロパティまたはメソッドを使用して公開する必要があります`Export`です。 例えば:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

このクラスが含まれるデータを - イメージの名前と型 (jpg、png など) の 2 つのフィールドを公開するおされます。 

詳細については、次を参照してください、 [c# を公開するクラス/Objective-c メソッド](~/mac/internals/how-it-works.md)のセクションで、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)ドキュメントについては、それについて説明します、`Register`と`Export`属性。ネットワーク上での c# クラスを OBJECTIVE-C オブジェクトと UI への要素に使用されます。

#### <a name="constructors"></a>コンストラクター

2 つのコンス トラクター (Objective C に正しく公開) は、ペースト ボードから読み取れるように、カスタム データ クラスの必要になります。

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

最初に、公開、_空_コンス トラクターの既定の OBJECTIVE-C メソッド `init`です。

次に、公開、`NSCoding`エクスポート名の下に貼り付けるときに、ペーストからオブジェクトの新しいインスタンスを作成するために使用する準拠のコンス トラクター`initWithCoder`です。

このコンス トラクター、 `NSCoder` (によって作成された、`NSKeyedArchiver`ペーストに書き込まれたとき)、キー/値ペアのデータを抽出し、データ クラスのプロパティ フィールドに保存します。

#### <a name="writing-to-the-pasteboard"></a>ペースト ボードへの書き込み

準拠することで、`INSPasteboardWriting`インターフェイス、いただくために 2 つのメソッドと必要に応じて、3 番目のメソッドを公開ペーストするクラスを書き込むことができるようにします。

まず、ペーストがどのデータ型にカスタム クラスを書き込むことができる形式を確認する必要があります。

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

各表現はを介して、Uniform 型識別子 (ユーティリ ティー)、これは何も表示されているデータの種類を一意に識別する単純な文字列よりも詳細で識別されます (詳細については、Apple を参照してください[Uniform 型識別子の概要](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319)ドキュメント)。

カスタムの書式は作成する独自のユーティリ ティー:"com.xamarin.image info"(アプリ Id と同じように逆引き表記されているに注意してください)。 クラスはペーストする標準の文字列を作成することも (`public.text`)。 

次に、実際にはペーストに書き込まれる要求の形式でオブジェクトを作成する必要があります。

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

`public.text`型、おを返す、単純なフォーマット`NSString`オブジェクト。 カスタム`com.xamarin.image-info`を使用して、型、`NSKeyedArchiver`と`NSCoder`キー/値ペアのアーカイブにカスタム データ クラスをエンコードするインターフェイスです。 実際には、エンコーディングを処理する次のメソッドを実装する必要があります。

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

個々 のキー/値のペアは、エンコーダーには書き込まれ、上に追加して 2 つ目のコンス トラクターを使用してデコードされます。

必要に応じて、ペースト ボードにデータを書き込むときに、任意のオプションを定義する次のメソッドを含めることができます。

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

現在は、`WritingPromised`オプションがあるし、指定された型が約束ののみとペースト ボードに実際に書き込まれるときに使用します。 詳細についてを参照してください、[約束データ](#Promised_Data)前のセクションです。

場所にこれらのメソッドでは、クラスを記述する、カスタム ペーストする次のコードを使用できます。

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>ペーストからの読み取り

準拠することで、`INSPasteboardReading`インターフェイス、カスタム データ クラスはペーストから読み込むことができるように、3 つのメソッドを公開する必要があります。

まず、ペーストどのようなデータ型の表現をクリップボードから読み取ることができるカスタム クラスを通知する必要があります。

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

ここでも、これらは単純な UTIs として定義されて、同じ種類で定義されていることを**ペースト ボードへの書き込み**前のセクションです。

次に、ペーストを確認する必要があります_方法_ユーティリ ティー型のそれぞれが読み取られます次のメソッドを使用します。

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

`com.xamarin.image-info`型を指示することにで作成したキー/値ペアをデコードするペースト、`NSKeyedArchiver`ときに呼び出すことによってペーストするクラスを記述、`initWithCoder:`クラスに追加したコンス トラクターです。

最後に、ペーストからの他のユーティリ ティー データ形式を読み取るには、次のメソッドを追加する必要があります。

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

これらすべてのメソッド インプレースでは、次のコードを使用してペーストからカスタム データ クラスを読み取ることができます。

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

### <a name="using-a-nspasteboarditem"></a>使用して、NSPasteboardItem

ときに、カスタム クラスの作成が確保されていないペーストするカスタムの項目を記述する必要がありますや必要なだけの一般的な形式でデータを提供する時間である可能性があります。 このような場合は、使用することができます、`NSPasteboardItem`です。

A`NSPasteboardItem`詳細に制御がペーストに書き込まれ、一時的なアクセスには、データに対してペーストに書き込まれた後の破棄する必要があります。

#### <a name="writing-data"></a>データの書き込み

カスタム データを書き込む、`NSPasteboardItem`カスタムを提供する必要があります`NSPasteboardItemDataProvider`です。 新しいクラスをプロジェクトに追加し、それを呼び出す**ImageInfoDataProvider.cs**です。 ファイルを編集し、次のようになります。

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

カスタム データ クラスで行ったように使用する必要があります、`Register`と`Export`目標 C. に公開するディレクティブ クラスを継承する必要があります`NSPasteboardItemDataProvider`および実装する必要があります、`FinishedWithDataProvider`と`ProvideDataForType`メソッドです。

使用して、`ProvideDataForType`でラップされるデータを提供するメソッド、`NSPasteboardItem`次のようにします。

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

ここでは、(名前とイメージの種類) は、このイメージに関する 2 つの情報を保存して、単純な文字列に書き込みおは (`public.text`)。

型のデータを書き込むペースト、次のコードを使用します。

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

ペーストから返されたデータの読み取りには、次のコードを使用します。

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

この記事では、ペースト Xamarin.Mac アプリケーション サポート コピーと貼り付けの操作での操作についての詳細を取得しました。 最初に、標準的なペースト操作を使い慣れてを取得する簡単な例が導入されました。 次に、ペーストとそこからデータを読み書きする方法の詳細に見てがかかりました。 最後に、コピーと貼り付けのアプリ内の複雑なデータ型をサポートするカスタム データ型を使用する調べるとします。



## <a name="related-links"></a>関連リンク

- [MacCopyPaste (サンプル)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ペースト ボード プログラミング ガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
