---
title: Xamarin. Mac にコピーして貼り付ける
description: この記事では、ペーストボードを使用して、Xamarin. Mac アプリケーションにコピーと貼り付けを行う方法について説明します。 ここでは、複数のアプリ間で共有できる標準データ型の操作方法と、特定のアプリ内でカスタムデータをサポートする方法を示します。
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 42ac6c9c729498ad4b70e1e209d63c1ec2e11f8d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291225"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Xamarin. Mac にコピーして貼り付ける

_この記事では、ペーストボードを使用して、Xamarin. Mac アプリケーションにコピーと貼り付けを行う方法について説明します。ここでは、複数のアプリ間で共有できる標準データ型の操作方法と、特定のアプリ内でカスタムデータをサポートする方法を示します。_

## <a name="overview"></a>概要

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合は、目的の C で作業している開発者が使用するのと同じ貼り付け (コピーアンドペースト) のサポートにアクセスできます。

この記事では、Xamarin. Mac アプリでクリップボードを使用する主な2つの方法について説明します。

1. **標準データ型**-通常、クリップボードの操作は、関連付けられていない2つのアプリ間で実行されるため、どちらのアプリも、他のアプリケーションがサポートするデータの種類を認識しません。 共有の可能性を最大にするために、ペーストボードは特定の項目の複数の表現を (共通データ型の標準セットを使用して) 保持できます。これにより、使用中のアプリは、ニーズに最も適したバージョンを選択できます。
2. **カスタムデータ**-Xamarin 内での複雑なデータのコピーと貼り付けをサポートするために、ペーストボードによって処理されるカスタムデータ型を定義できます。 たとえば、複数のデータ型とポイントで構成される複雑な図形のコピーと貼り付けをユーザーに許可するベクター描画アプリなどです。

[![実行中のアプリの例](copy-paste-images/intro01.png "実行中のアプリの例")](copy-paste-images/intro01-large.png#lightbox)

この記事では、コピーと貼り付け操作をサポートするために、Xamarin. Mac アプリケーションでの貼り付け操作の基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。

## <a name="getting-started-with-the-pasteboard"></a>クリップボードの概要

このペーストボードは、特定のアプリケーション内またはアプリケーション間でデータを交換するための標準化されたメカニズムを提供します。 Xamarin. Mac アプリケーションでは、通常、コピーと貼り付け操作を処理しますが、他の多くの操作もサポートされています (ドラッグ & ドロップとアプリケーションサービスなど)。

ここでは、まず、Xamarin. Mac アプリで pasteboards を使用するための簡単で実用的な概要を説明します。 後で、ペーストボードの動作と使用方法について詳しく説明します。

この例では、イメージビューを含むウィンドウを管理する単純なドキュメントベースのアプリケーションを作成します。 ユーザーは、アプリ内のドキュメント間、および同じアプリ内の他のアプリまたは複数のウィンドウとの間でイメージをコピーして貼り付けることができます。

### <a name="creating-the-xamarin-project"></a>Xamarin プロジェクトの作成

まず、コピーと貼り付けのサポートを追加する新しいドキュメントベースの Xamarin. Mac アプリを作成します。

次の手順で行います。

1. Visual Studio for Mac を開始し、 **[新しいプロジェクト...]** リンクをクリックします。
2. [ **Mac** > **アプリ** cocoa アプリ] を選択し、[次へ] ボタンをクリックします。 >  

    [![新しい Cocoa アプリプロジェクトを作成する](copy-paste-images/sample01.png "新しい Cocoa アプリプロジェクトを作成する")](copy-paste-images/sample01-large.png#lightbox)
3. `MacCopyPaste` **プロジェクト名**として「」と入力し、他のすべてを既定値のままにします。 [次へ] をクリックします。 

    [![プロジェクトの名前を設定する](copy-paste-images/sample01a.png "プロジェクトの名前を設定する")](copy-paste-images/sample01a-large.png#lightbox)

4. **[作成]** ボタンをクリックします。 

    [![新しいプロジェクトの設定を確認しています](copy-paste-images/sample02.png "新しいプロジェクトの設定を確認しています")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>NSDocument を追加する

次に、アプリケーションの`NSDocument`ユーザーインターフェイスのバックグラウンドストレージとして機能するカスタムクラスを追加します。 このファイルには、1つのイメージビューが含まれており、ビューから既定のペーストボードにイメージをコピーする方法と、既定のクリップボードからイメージを取得してイメージビューに表示する方法がわかります。

**Solution Pad**で Xamarin プロジェクトを右クリックし、[**新しいファイル**の**追加** > ] を選択します。

![NSDocument をプロジェクトに追加する](copy-paste-images/sample03.png "NSDocument をプロジェクトに追加する")

**[名前]** に「`ImageDocument`」と入力し、 **[新規]** ボタンをクリックします。 **ImageDocument.cs**クラスを編集し、次のようにします。

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

次に、コードの一部について詳しく見ていきましょう。

次のコードは、既定のペーストボードにイメージデータが存在するかどうかをテストするためのプロパティを`true`提供します`false`。イメージが使用可能な場合は、それ以外は返されません。

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

次のコードでは、イメージをアタッチされたイメージビューから既定のペーストボードにコピーします。

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

次のコードは、既定のペーストボードからイメージを貼り付け、アタッチされたイメージビューに表示します (貼り付けに有効なイメージが含まれている場合)。

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

このドキュメントでは、Xamarin. Mac アプリ用のユーザーインターフェイスを作成します。

### <a name="building-the-user-interface"></a>ユーザーインターフェイスの構築

**メインの storyboard**ファイルをダブルクリックして、Xcode で開きます。 次に、ツールバーとイメージウェルを追加し、次のように構成します。

[![ツールバーの編集](copy-paste-images/sample04.png "ツールバーの編集")](copy-paste-images/sample04-large.png#lightbox)

ツールバーの左側に、[イメージのコピーと貼り付け **] ツールバー項目**を追加します。 これらは、[編集] メニューからコピーおよび貼り付けを行うためのショートカットとして使用されます。 次に、ツールバーの右側に4つの**イメージツールバー項目**を追加します。 これらを使用して、イメージに既定のイメージを設定します。

ツールバーの操作の詳細については、[ツールバー](~/mac/user-interface/toolbar.md)のドキュメントを参照してください。

次に、ツールバー項目とイメージウェルの次のアウトレットとアクションを公開してみましょう。

[![アウトレットとアクションの作成](copy-paste-images/sample05.png "アウトレットとアクションの作成")](copy-paste-images/sample05-large.png#lightbox)

アウトレットとアクションの操作の詳細については、「 [Hello, Mac](~/mac/get-started/hello-mac.md) 」ドキュメントの「[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションを参照してください。

### <a name="enabling-the-user-interface"></a>ユーザーインターフェイスを有効にする

Xcode で作成したユーザーインターフェイスと、アウトレットとアクションを介して公開されている UI 要素を使用して、UI を有効にするためのコードを追加する必要があります。 **Solution Pad**で**ImageWindow.cs**ファイルをダブルクリックし、次のように表示します。

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

次に、このコードについて詳しく見ていきましょう。

まず、上記で作成した`ImageDocument`クラスのインスタンスを公開します。

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

、、 `Export` `WillChangeValue`および`Document`を使用して、Xcode でキーと値のコードおよびデータバインディングを許可するようにプロパティを設定しました。 `DidChangeValue`

また、Xcode の UI に追加したイメージからも、次のプロパティを使用してイメージを公開しています。

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

メインウィンドウが読み込まれて表示されたら、次のコード`ImageDocument`を使用して、クラスのインスタンスを作成し、UI のイメージをそのインスタンスにアタッチします。

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

最後に、[コピーして貼り付ける] ツールバー項目をクリックすると、実際の作業を行う`ImageDocument`ためにクラスのインスタンスが呼び出されます。

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>[ファイル] メニューと [編集] メニューの有効化

最後に、 **[ファイル]** メニューの **[新規]** メニュー項目を有効にし (メインウィンドウの新しいインスタンスを作成するため)、 **[編集]** メニューの **[切り取り]** 、[**コピー** /**貼り付け**] メニュー項目を有効にする必要があります。

**新しい**メニュー項目を有効にするには、 **AppDelegate.cs**ファイルを編集し、次のコードを追加します。

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

詳細については、 [windows](~/mac/user-interface/window.md)ドキュメントの「[複数のウィンドウを使用した作業](~/mac/user-interface/window.md)」を参照してください。

**切り取り**、**コピー** 、**貼り付け**の各メニュー項目を有効にするには、 **AppDelegate.cs**ファイルを編集し、次のコードを追加します。

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

各メニュー項目について、現在の最上位のキーウィンドウを取得し、 `ImageWindow`クラスにキャストします。

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

そこから、そのウィンドウ`ImageDocument`のクラスインスタンスを呼び出して、コピーと貼り付けの操作を処理します。 例えば: 

```csharp
window.Document.CopyImage (sender);
```

既定のクリップボードにイメージデータがある場合、または現在アクティブなウィンドウのイメージウェルの場合にのみ、**切り取り**、**コピー** 、**貼り付け**の各メニュー項目にアクセスできるようにします。

**EditMenuDelegate.cs**ファイルを Xamarin プロジェクトに追加し、次のように表示します。

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

ここでも、現在の最上位ウィンドウを取得し`ImageDocument` 、そのクラスのインスタンスを使用して、必要なイメージデータが存在するかどうかを確認します。 次に、メソッド`MenuWillHighlightItem`を使用して、この状態に基づいて各項目を有効または無効にします。

**AppDelegate.cs**ファイルを編集し、メソッド`DidFinishLaunching`を次のようにします。
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

まず、[編集] メニューのメニュー項目の自動有効化と無効化を無効にします。 次に、上記で作成した`EditMenuDelegate`クラスのインスタンスをアタッチします。

詳細については、[メニュー](~/mac/user-interface/menu.md)のドキュメントを参照してください。

### <a name="testing-the-app"></a>アプリのテスト

すべてが整ったら、アプリケーションをテストする準備ができました。 アプリをビルドして実行すると、メインインターフェイスが表示されます。

![アプリケーションの実行](copy-paste-images/run01.png "アプリケーションの実行")

編集 メニューを開いた場合、**切り取り**、**コピー**、および **貼り付け** は無効になっています。これは、イメージにイメージが表示されていない場合、または既定のペーストボードに画像がないためです。

![[編集] メニューを開く](copy-paste-images/run02.png "[編集] メニューを開く")

イメージをイメージに追加して、[編集] メニューを再び開くと、項目が有効になります。

![[編集] メニュー項目が有効になっていることを表示する](copy-paste-images/run03.png "[編集] メニュー項目が有効になっていることを表示する")

イメージをコピーして ファイル メニューの **新規作成** を選択した場合は、そのイメージを新しいウィンドウに貼り付けることができます。

![新しいウィンドウへのイメージの貼り付け](copy-paste-images/run04.png "新しいウィンドウへのイメージの貼り付け")

以下のセクションでは、Xamarin. Mac アプリケーションでのクリップボードの操作について詳しく説明します。

## <a name="about-the-pasteboard"></a>ペーストボードについて

MacOS (旧称 OS X) では、貼り付け (`NSPasteboard`) によって、コピー & 貼り付け、ドラッグ & ドロップ、アプリケーションサービスなどの複数のサーバープロセスがサポートされます。 以下のセクションでは、いくつかの重要なクリップボードの概念について詳しく見ていきます。

### <a name="what-is-a-pasteboard"></a>ペーストボードとは

クラス`NSPasteboard`は、アプリケーション間または特定のアプリ内で情報を交換するための標準化されたメカニズムを提供します。 貼り付けの主な機能は、コピーと貼り付け操作を処理することです。

1. ユーザーがアプリで項目を選択し、切り取り または **コピー** メニュー項目を使用すると、選択した項目の1つ以上の表現が**貼り**付けに配置されます。
2. ユーザーが貼り付けメニュー項目 (同じアプリ内または別のアプリ内) を使用すると、処理できるデータのバージョンが**貼り付け**られ、アプリに追加されます。

[検索]、[ドラッグ]、[ドラッグアンドドロップ]、[アプリケーションサービス] の各操作を含む、わかりやすいペーストボードの使用方法:

- ユーザーがドラッグ操作を開始すると、ドラッグデータがペーストボードにコピーされます。 ドラッグ操作が別のアプリにドロップして終了すると、そのアプリは貼り付けられたデータをコピーします。
- 翻訳サービスの場合は、翻訳対象のデータが、要求元のアプリによって貼りペーストにコピーされます。 アプリケーションサービスは、ペーストボードからデータを取得し、変換を行い、貼り付けられたデータを貼り付けます。

最も単純な形式では、pasteboards を使用して特定のアプリ内のデータを移動したり、アプリとれるための間でアプリのプロセス外の特殊なグローバルメモリ領域に存在したりします。 Pasteboards の概念は簡単に grasps ますが、考慮する必要がある複雑な詳細がいくつかあります。 これらの詳細については、以下で詳しく説明します。

### <a name="named-pasteboards"></a>名前付き pasteboards

クリップボードは、パブリックまたはプライベートにすることができ、アプリケーション内または複数のアプリ間でさまざまな目的で使用できます。 macOS にはいくつかの標準 pasteboards が用意されており、それぞれに特定の明確に定義された使用法があります。

- `NSGeneralPboard`-**切り取り**、**コピー** 、および**貼り付け**操作の既定のペーストボード。
- `NSRulerPboard`-**切り取り**、**コピー** 、および**貼り付け**操作を**ルーラー**に対してサポートします。
- `NSFontPboard`-オブジェクトに対する**切り取り**、**コピー** 、および`NSFont` **貼り付け**操作をサポートします。
- `NSFindPboard`-検索テキストを共有できるアプリケーション固有の検索パネルをサポートします。
- `NSDragPboard`-**ドラッグ & ドロップ**操作をサポートします。

ほとんどの場合、システム定義の pasteboards のいずれかを使用します。 ただし、独自の pasteboards を作成する必要がある場合もあります。 このような状況では、 `FromName (string name)` `NSPasteboard`クラスのメソッドを使用して、指定された名前のカスタムのペーストボードを作成できます。

必要に応じて、 `CreateWithUniqueName` `NSPasteboard`クラスのメソッドを呼び出して、一意の名前の付いたペーストボードを作成できます。

### <a name="pasteboard-items"></a>クリップボード項目

アプリケーションが1つのクリップボードに書き込むデータの各部分は、"_ペーストボード項目_" と見なされ、1つのクリップボードは同時に複数の項目を保持できます。 このようにすることで、アプリでは、コピーされた複数のバージョンのデータを貼り付け (プレーンテキストや書式設定されたテキストなど) することができます。また、取得元のアプリは、処理可能なデータ (プレーンテキストのみなど) を読み取ることができます。

### <a name="data-representations-and-uniform-type-identifiers"></a>データ表現と uniform type 識別子

通常、クリップボード操作は、相互に認識されない、またはそれぞれが処理できるデータの種類について、2つ (またはそれ以上) のアプリケーション間で行われます。 前のセクションで説明したように、情報を共有する可能性を最大にするために、貼り付けでは、コピーおよび貼り付けされるデータの複数の表現を保持できます。

各表現は、一様な型識別子 (UTI) によって識別されます。これは、表示されている日付の型を一意に識別する単純な文字列にすぎません (詳細については、「Apple の[Uniform type](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) Identifier の概要」を参照してください)。ドキュメント)。 

カスタムデータ型 (ベクター描画アプリ内の drawing オブジェクトなど) を作成する場合は、コピーおよび貼り付け操作で一意に識別できるように独自の UTI を作成できます。

クリップボードからコピーしたデータをアプリで貼り付ける準備ができたら、その機能に最適な表現 (存在する場合) を見つける必要があります。 通常、これは使用可能な最も豊富な型 (ワードプロセッシングアプリ用の書式設定されたテキストなど) になり、必要に応じて使用可能な最も単純な形式にフォールバックします (単純なテキストエディターの場合はプレーンテキスト)。

<a name="Promised_Data" />

### <a name="promised-data"></a>約束データ

一般に、アプリ間の共有を最大化するために、できるだけ多くのデータ表現をコピーするように指定する必要があります。 ただし、時間やメモリの制約があるため、各データ型を貼り表示に実際に書き込むのは現実的ではありません。

このような状況では、最初のデータ表現を貼り付けることができ、受信側のアプリは別の表現を要求できます。これは、貼り付け操作の直前に生成できます。

最初の項目をペーストボードに配置すると、その`NSPasteboardItemDataProvider`インターフェイスに準拠するオブジェクトによって提供されるその他の1つ以上の表現が指定されます。 これらのオブジェクトは、受信側のアプリによって要求されたときに、必要に応じて追加の表現を提供します。

### <a name="change-count"></a>変更数

各ペーストボードは、新しい所有者が宣言されるたびに増加する_変更数_を保持します。 アプリでは、変更回数の値をチェックすることによって、最後にそのクリップボードの内容が変更されたかどうかを判断できます。

クラスの`NSPasteboard`メソッド`ClearContents`とメソッドを使用して、特定のペーストボードの変更数を変更します。 `ChangeCount`

## <a name="copying-data-to-a-pasteboard"></a>ペーストボードへのデータのコピー

コピー操作を実行するには、まず、ペーストボードにアクセスし、既存の内容を消去して、ペーストボードに必要な数だけデータの表現を書き込みます。

例えば:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

通常は、上の例で行ったように、一般的なペーストボードに書き込むだけです。 `WriteObjects`メソッドに送信するオブジェクトは、 `INSPasteboardWriting`インターフェイスに準拠している*必要があり*ます。 いくつかの組み込みクラス ( `NSString` `NSURL`、 `NSImage`、、 `NSColor` `NSAttributedString`、、 `NSPasteboardItem`など) は、自動的にこのインターフェイスに準拠します。

カスタムデータクラスをペーストボードに書き込む場合は、 `INSPasteboardWriting`インターフェイスに準拠しているか、 `NSPasteboardItem`クラスのインスタンスにラップされている必要があります (以下の「[カスタムデータ型](#Custom_Data_Types)」を参照してください)。

## <a name="reading-data-from-a-pasteboard"></a>ペーストボードからのデータの読み取り

前述のように、アプリ間でデータを共有する可能性を最大限に高めるために、コピーされたデータの複数の表現が貼り付けに書き込まれる可能性があります。 機能 (存在する場合) で可能な限り豊富なバージョンを選択するには、受信側アプリが必要です。

### <a name="simple-paste-operation"></a>単純な貼り付け操作

`ReadObjectsForClasses`メソッドを使用して、ペーストボードからデータを読み取ります。 次の2つのパラメーターが必要です。

1. ペーストボードから`NSObject`読み取る、ベースのクラス型の配列。 最初に、必要なデータ型を使用して、その他の種類を優先するように指定する必要があります。
2. 追加の制約 (特定の URL のコンテンツ型への制限など) を含むディクショナリ。それ以上の制約が不要な場合は空のディクショナリ。

メソッドは、渡された条件を満たす項目の配列を返します。したがって、要求されたデータ型の数が最大で含まれます。 要求された型が存在しない可能性もあり、空の配列が返されます。

たとえば、次のコードでは、が全般の`NSImage`クリップボードに存在するかどうかを確認し、次のような場合にイメージに表示します。

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

### <a name="requesting-multiple-data-types"></a>複数のデータ型の要求

作成される Xamarin. Mac アプリケーションの種類に基づいて、貼り付けられたデータの複数の表現を処理できる場合があります。 このような状況では、次の2つのシナリオで、ペーストボードからデータを取得します。

1. `ReadObjectsForClasses`メソッドを1回呼び出して、必要なすべての表現の配列を (優先順に) 指定します。
2. メソッドに対して複数`ReadObjectsForClasses`の呼び出しを行い、毎回異なる型の配列を要求します。

ペーストボードからデータを取得する方法の詳細については、上記の「**単純な貼り付け操作**」セクションを参照してください。

### <a name="checking-for-existing-data-types"></a>既存のデータ型の確認

クリップボードに実際にデータを読み込むことなく、特定のデータ表現が含まれているかどうかを確認する必要がある場合があります (有効なデータが存在する場合にのみ**貼り付け**メニュー項目を有効にするなど)。

指定した型が含まれているかどうかを確認するには、ペーストボードのメソッドを呼び出します。`CanReadObjectForClasses`

たとえば、次のコードは、一般的なペーストボードに`NSImage`インスタンスが含まれているかどうかを判断します。

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

### <a name="reading-urls-from-the-pasteboard"></a>ペーストボードからの url の読み取り

指定された一連の条件 (特定のデータ型のファイルまたは Url を指す場合など) を満たしている場合にのみ、特定の Xamarin. Mac アプリの機能に基づいて、ペーストボードからの Url の読み取りが必要になることがあります。 このような場合`CanReadObjectForClasses`は、メソッドまたは`ReadObjectsForClasses`メソッドの2番目のパラメーターを使用して、追加の検索条件を指定できます。

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>カスタムデータ型

独自のカスタム型を Xamarin. Mac アプリからのペーストボードに保存する必要がある場合もあります。 たとえば、ユーザーが描画オブジェクトをコピーして貼り付けることができるベクター描画アプリなどです。

このような場合`NSObject`は、から継承し、いくつかのインターフェイス (`INSCoding`、 `INSPasteboardWriting`および`INSPasteboardReading`) に準拠するように、データカスタムクラスを設計する必要があります。 必要に応じて、を`NSPasteboardItem`使用して、コピーまたは貼り付けするデータをカプセル化できます。

これらの両方のオプションについては、以下で詳しく説明します。

### <a name="using-a-custom-class"></a>カスタムクラスの使用

このセクションでは、このドキュメントの冒頭で作成した単純なサンプルアプリを拡張し、windows 間でコピーと貼り付けを行うイメージに関する情報を追跡するためのカスタムクラスを追加します。

新しいクラスをプロジェクトに追加し、 **ImageInfo.cs**というメソッドを呼び出します。 ファイルを編集し、次のように表示します。

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

以下のセクションでは、このクラスについて詳しく説明します。

#### <a name="inheritance-and-interfaces"></a>継承とインターフェイス

カスタムデータクラスの書き込みや、ペーストボードからの読み取りを行うには、インターフェイス`INSPastebaordWriting`と`INSPasteboardReading`インターフェイスに準拠している必要があります。 さらに、から`NSObject`継承し、 `INSCoding`インターフェイスにも準拠している必要があります。

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

また、クラスは、 `Register`ディレクティブを使用して目的の C に公開する必要があります。また、を使用`Export`して必要なプロパティまたはメソッドを公開する必要があります。 例えば:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

このクラスに含まれるデータの2つのフィールド (イメージの名前とその型 (jpg、png など)) を公開しています。 

詳細については、「 [Xamarin. Mac の内部](~/mac/internals/how-it-works.md) `Register`ドキュメント」の「[クラス/メソッドを目的に公開C# ](~/mac/internals/how-it-works.md)する」セクションを参照してください。 C#クラスをに接続するために使用される属性と`Export`属性については、「」を参照してください。目的 C オブジェクトと UI 要素。

#### <a name="constructors"></a>コンストラクター

カスタムデータクラスでは、次の2つのコンストラクターを使用して、ペーストボードから読み取ることができるようにする必要があります。

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

まず、の`init`既定の目的 C メソッドの下に_空_のコンストラクターを公開します。

次に、の`NSCoding` `initWithCoder`エクスポートされた名前の下に貼り付けるときに、貼り付けによってオブジェクトの新しいインスタンスを作成するために使用される準拠コンストラクターを公開します。

このコンストラクターは、 `NSCoder` (ペーストボードに書き`NSKeyedArchiver`込まれるときにによって作成された) を取得し、キーと値のペアのデータを抽出して、データクラスのプロパティフィールドに保存します。

#### <a name="writing-to-the-pasteboard"></a>ペーストボードへの書き込み

`INSPasteboardWriting`インターフェイスに準拠して、2つのメソッドを公開し、必要に応じて3番目のメソッドを公開して、クラスをペーストボードに書き込むことができるようにする必要があります。

まず、カスタムクラスに書き込むことができるデータ型の表現を、ペーストボードに指示する必要があります。

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

各表現は、一様な型識別子 (UTI) によって識別されます。これは、表示されているデータの型を一意に識別する単純な文字列ではありません (詳細については、「Apple の[Uniform type](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) Identifier の概要」を参照してください)。ドキュメント)。

カスタム形式の場合は、独自の UTI を作成しています。 "" (はアプリ識別子と同じように逆表記であることに注意してください)。 クラスは、標準の文字列をペーストボード (`public.text`) に書き込むこともできます。 

次に、要求された形式でオブジェクトを作成し、実際にはクリップボードに書き込みます。

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

型の場合は、単純な書式設定`NSString`されたオブジェクトを返します。 `public.text` カスタム`com.xamarin.image-info`型の場合、 `NSKeyedArchiver`および`NSCoder`インターフェイスを使用して、カスタムデータクラスをキーと値のペアのアーカイブにエンコードします。 実際にエンコードを処理するには、次のメソッドを実装する必要があります。

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

個々のキーと値のペアはエンコーダーに書き込まれ、上で追加した2番目のコンストラクターを使用してデコードされます。

必要に応じて、次のメソッドを追加して、ペーストボードにデータを書き込むときのオプションを定義できます。

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

現時点では`WritingPromised` 、オプションのみを使用できます。特定の型が約束されていて、実際にはペーストボードに書き込まれていない場合に使用する必要があります。 詳細については、前の「[約束データ](#Promised_Data)」セクションを参照してください。

これらのメソッドを使用すると、次のコードを使用して、カスタムクラスをクリップボードに書き込むことができます。

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>ペーストボードからの読み取り

`INSPasteboardReading`インターフェイスに準拠することで、カスタムデータクラスをペーストボードから読み取ることができるように、3つのメソッドを公開する必要があります。

まず、カスタムクラスがクリップボードから読み取ることができるデータ型の表現を、貼り付けを指定する必要があります。

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

ここでも、これらは simple Uti として定義されており、前の「**ペーストボードセクションへの書き込み**」で定義したものと同じ型です。

次に、次のメソッドを使用して、UTI の各型が_どのよう_に読み取られるかを、ペーストボードに指示する必要があります。

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

この型については、「」で作成したキーと値のペアを、クラス`NSKeyedArchiver`に追加した`initWithCoder:`コンストラクターを呼び出すことによって、このクラスをクリップボードに書き込むときに、によって作成されたキーと値のペアがクリップボードに伝えられます。 `com.xamarin.image-info`

最後に、次のメソッドを追加して、他の UTI データ表現をペーストボードから読み取る必要があります。

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

これらのメソッドをすべて配置したら、次のコードを使用して、カスタムデータクラスをペーストボードから読み取ることができます。

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

### <a name="using-a-nspasteboarditem"></a>NSPasteboardItem を使用する

カスタムの項目を、カスタムクラスを作成することが保証されていない場合や、データを共通の形式で提供する必要がある場合など、必要な場合もあります。 このような状況では、を`NSPasteboardItem`使用できます。

は`NSPasteboardItem` 、ペーストボードに書き込まれるデータを詳細に制御し、一時的なアクセス用に設計されています。これは、ペーストボードに書き込まれた後に破棄する必要があります。

#### <a name="writing-data"></a>データの書き込み

カスタムデータをに`NSPasteboardItem`書き込むには、カスタム`NSPasteboardItemDataProvider`を提供する必要があります。 新しいクラスをプロジェクトに追加し、 **ImageInfoDataProvider.cs**というメソッドを呼び出します。 ファイルを編集し、次のように表示します。

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

カスタムデータクラスの場合と同様に、ディレクティブ`Register`と`Export`ディレクティブを使用して目的の C に公開する必要があります。 クラスは、から`NSPasteboardItemDataProvider`継承する必要があり、 `ProvideDataForType` `FinishedWithDataProvider`およびメソッドを実装する必要があります。

次のように、 `NSPasteboardItem` メソッドを使用して、にラップされるデータを指定します。`ProvideDataForType`

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

この例では、イメージ (Name と ImageType) について2つの情報を格納し、それらを単純な文字列`public.text`() に書き込みます。

「データをペーストボードに書き込む」と入力し、次のコードを使用します。

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

ペーストボードからデータを読み取るには、次のコードを使用します。

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

この記事では、コピーと貼り付け操作をサポートするために、Xamarin. Mac アプリケーションでのクリップボードの操作方法について詳しく説明しました。 まず、標準的な pasteboards 操作について理解を深めるための簡単な例を紹介しました。 次に、ペーストボードについて詳しく説明し、データの読み取りと書き込みを行う方法を説明しました。 最後に、カスタムデータ型を使用して、アプリ内での複合データ型のコピーと貼り付けをサポートする方法を見てきました。



## <a name="related-links"></a>関連リンク

- [MacCopyPaste (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccopypaste)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [クリップボードのプログラミングガイド](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
