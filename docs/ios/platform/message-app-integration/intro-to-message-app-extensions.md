---
title: "メッセージ アプリ拡張機能の基礎"
description: "この記事の内容表示は方法メッセージ アプリとの統合を新しい機能をユーザーに提示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含めます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 083920aba3c8dc83b157b591e194c43935dcc566
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="message-app-extension-basics"></a>メッセージ アプリ拡張機能の基礎

_この記事の内容表示は方法メッセージ アプリとの統合を新しい機能をユーザーに提示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含めます。_

新しい 10、iOS にメッセージ アプリ拡張機能 ' æ˜aœg '、**メッセージ**をユーザーにアプリと表示の新機能です。 拡張機能には、テキスト、ステッカー、メディア ファイルおよび対話型のメッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージ アプリ拡張機能の概要

前述のように、メッセージ アプリ拡張機能と統合、**メッセージ**をユーザーにアプリと表示の新機能です。 拡張機能には、テキスト、ステッカー、メディア ファイルおよび対話型のメッセージを送信できます。 2 つの種類のメッセージ アプリ拡張機能を使用できます。

- **ステッカー パック**-ユーザーがメッセージに追加できるステッカーのコレクションを格納します。 すべてのコードを記述することがなくステッカー パックを作成できます。
- **iMessage アプリ**-ステッカーを選択すると、テキストを入力する、(省略可能な型変換) とメディア ファイルを含む、作成、編集、および操作メッセージを送信する用アプリのメッセージのカスタム ユーザー インターフェイスを提供します。

メッセージ アプリ拡張機能は、次の 3 つの主要なコンテンツ タイプを提供します。

- **対話型のメッセージ**-アプリを生成するカスタム メッセージ コンテンツの種類は、ユーザーが、メッセージは、アプリでタップしたときに、フォア グラウンドで起動されます。
- **ステッカー** -ユーザーの間で送信されるメッセージに含めることがアプリによって生成されるイメージです。
- **その他のサポートされているコンテンツ**- アプリは、写真、ビデオ、テキストなどのコンテンツを提供できます。 またはその他のコンテンツへのリンクの種類を常にメッセージ アプリでサポートされています。

新しい 10、iOS にメッセージ アプリが含まれています、独自の組み込みの専用アプリ ストアには。 メッセージ アプリ拡張機能を含むすべてのアプリが表示され、このストアで昇格します。 新しいメッセージ アプリ ドロアーのクイック アクセスをユーザーに提供するメッセージの App Store からダウンロードされているすべてのアプリが表示されます。

また新しい iOS 10、Apple が追加ユーザーがアプリを簡単に検出するインライン アプリ属性。 たとえば、1 人のユーザーは、2 番目のユーザーが持っていないアプリから別のコンテンツを送信する場合 (たとえばステッカー) のようにインストールされている送信元のアプリの名前 下にあるメッセージ履歴内のコンテンツ。 場合、ユーザーはタップ アプリの名前、開くおメッセージ アプリ ストアとストアで選択されているアプリ。

メッセージ アプリ拡張機能は、既存の iOS アプリを作成する使い慣れている開発者し、すべての標準的なフレームワークと標準的な iOS アプリの機能へのアクセスが必要であることに似ています。 例えば:

- アプリ内購入へのアクセスがあります。
- Apple Pay へのアクセスがあります。
- カメラなどのデバイスのハードウェアへのアクセスがあります。

メッセージ アプリ拡張機能が iOS 10 でのみサポート、ただし、これらの拡張機能を送信する内容は watchOS および macOS デバイスで表示できます。 新しい_最近ページ_追加 watchOS 3 にはメッセージ アプリ拡張機能のものを含む、携帯電話から送信された最新のステッカーを表示され、ウォッチからそれらのステッカーを送信するユーザーを許可します。

## <a name="about-the-messages-framework"></a>メッセージのフレームワークについて

新しい 10、iOS にメッセージ フレームワークは、ユーザーの iOS デバイスでメッセージ アプリの拡張機能とメッセージ アプリ間のインターフェイスを提供します。 ユーザーは、メッセージのアプリ内からのアプリを起動、このフレームワークはアプリの検出を許可し、データとその UI をレイアウトするために必要なコンテキストを提供します。

アプリが起動した後、ユーザーが対話メッセージを介して共有する新しいコンテンツを作成するためにします。 アプリは、このメッセージのフレームワークを使用して処理用メッセージ アプリを新しく作成されたコンテンツを転送します。

フレームワークのメッセージおよびメッセージ アプリ拡張機能は、既存の iOS アプリの拡張機能のテクノロジの上に構築されます。 アプリの拡張機能の詳細については、Apple を参照してください[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)です。

システム全体で Apple から提供された他の拡張機能ポイントとは異なり、開発者はメッセージ アプリ自体が、コンテナーとして機能するために、そのメッセージ アプリ拡張機能のホスト アプリケーションを提供するには必要ありません。 ただし、開発者には、新規または既存の iOS アプリ内でメッセージ アプリの拡張機能を含むとバンドルと共に配布のオプションがあります。

メッセージ アプリ拡張機能が iOS アプリのバンドルに含まれている場合は、デバイスのホーム画面とでメッセージ アプリ内からメッセージのアプリのドロアーの両方に、アプリのアイコンが表示されます。 アプリ バンドルに含まれていない場合は、メッセージ アプリ ドロアーでメッセージ アプリの拡張機能のみが表示されます。

これは、システム メッセージ アプリ ドロアーや設定などの他の部分で表示されるアイコンに、開発者がメッセージ アプリの拡張機能のバンドルでアプリのアイコンを提供する必要があるホスト アプリ バンドルに含まれるメッセージ アプリ拡張機能が含まれていない場合でもを拡張します。

## <a name="about-stickers"></a>ステッカーについて

Apple ステッカー手段として設計、新しい iMessage ユーザー ステッカーを許可することで通信するための他のメッセージの内容としてインラインを送信するか、メッセージ交換内で前のメッセージ バブルにアタッチされていることができます。

ステッカーは?

- これらは、メッセージのアプリの拡張機能を提供するイメージです。
- アニメーションまたは静的のいずれかのイメージを設定できます。
- アプリ内からイメージのコンテンツを共有する新しい機能を提供します。

これにはステッカーを作成する 2 つの方法があります。

1. Xcode の内部では、コードを含むせずステッカー パック メッセージ アプリ拡張機能をから作成できます。 必要なは、シールとアプリのアイコンのアセットです。
2. 標準メッセージ アプリ拡張機能を作成することで、ステッカー メッセージ フレームワークを使用してコードを提供します。

### <a name="creating-sticker-packs"></a>ステッカー パックの作成

ステッカー パックは、Xcode の内部で特別なテンプレートから作成され、単にステッカーとして使用できるイメージ資産の静的セットを提供します。 前述のように、すべてのコードは必要ありません、開発者がステッカー アセット カタログの内部でステッカー パック フォルダーにイメージ ファイルを単純にドラッグします。

ステッカー パックに含まれるイメージを次の要件を満たす必要が必要があります。

- 画像は PNG、APNG、GIF、または JPEG 形式でなければなりません。 Apple では、形式を使用してのみ、PNG および APNG ステッカー資産を提供するときにお勧めします。
- アニメーションのステッカーは、APNG および GIF 形式をサポートするだけです。
- ユーザーが、メッセージ交換のメッセージのバブルを配置することができるため、ステッカーの画像で透明な背景を提供する必要があります。
- 個々 のイメージ ファイルは、500 kb 未満である必要があります。
- イメージは、その 206 x 206 ポイント、100 x 100 ポイントよりも小さいか大きいをすることはできません。

> [!IMPORTANT]
> ステッカーの画像はで常に提供する必要があります、 `@3x` 300 x 300 に 618 x 618 ピクセルの範囲内で解決します。 システムが自動的に生成、`@2x`と`@1x`必要に応じて実行時にバージョン。

Apple では、テスト可能なすべての状況で最適な表示されることを確認するとさまざまな (白、黒、赤、黄色、およびマルチ色) などのさまざまな色付き背景上の写真、ステッカーのイメージ アセットを提案します。

ステッカー パックには、3 つの利用可能なサイズのいずれかで表示するステッカーを指定できます。

- **小さな**- 100 x 100 ポイント。
- **Medium** - 136 x 136 ポイント。 これは、既定のサイズです。
- **大規模な**- 206 x 206 ポイント。

Xcode の属性のインスペクターをステッカー パック全体のサイズを設定し、最適な結果をメッセージ アプリ内でステッカー ブラウザーで、要求されたサイズに一致するためのイメージ アセットのみを使用します。

詳細についてを参照してください、[アイスクリーム ビルダー](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)アプリと Apple の[メッセージ参照](https://developer.apple.com/reference/messages)です。

## <a name="creating-a-custom-sticker-experience"></a>カスタムのステッカー エクスペリエンスを作成します。

アプリには、複数のコントロールまたはステッカー パックによって提供されるよりも柔軟性が必要とする場合メッセージ アプリ拡張機能を含めるし、カスタムのステッカー エクスペリエンスのメッセージ フレームワークを介してステッカーを提供できます。

カスタム ステッカー エクスペリエンスを作成する利点とは

1. アプリのユーザーにステッカーを表示する方法をカスタマイズするアプリを許可します。 たとえば、標準的なグリッド レイアウト以外または形式のステッカーを表示する、さまざまな色付きバック グラウンド。
2. 静的イメージの資産として含まれるのではなくコードから動的に作成されるステッカーを使用できます。
3. アプリ ストアへの新しいバージョンをリリースすることがなく、開発者の web サーバーから動的にダウンロードされるステッカーのイメージ アセットを使用できます。
4. で、デバイスのカメラへのアクセスするステッカー上の場を作成します。
5. アプリ内購入のでは、アプリ内から複数のステッカーを購入するためです。

カスタム ステッカー エクスペリエンスを作成するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for mac を起動します。
2. メッセージ アプリ拡張機能を追加するソリューションを開きます。 
3. 選択**iOS** > **拡張機能** > **iMessage 拡張子** をクリックし、 **次へ**ボタン。 

    [![](intro-to-message-app-extensions-images/message01.png "IMessage 拡張機能を選択します。")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. 入力、**拡張機能の名前** をクリックし、**次**ボタン。 

    [![](intro-to-message-app-extensions-images/message02.png "拡張機能の名前を入力してください。")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. クリックして、**作成**拡張機能をビルドするにはボタン。 

    [![](intro-to-message-app-extensions-images/message03.png "作成する をクリックします。")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動します。
2. メッセージ アプリ拡張機能を追加するソリューションを開きます。 
3. 選択**iOS** > **拡張機能** > **iMessage 拡張子** をクリックし、 **次へ**ボタン。 

    [![](intro-to-message-app-extensions-images/message01w.png "IMessage 拡張機能を選択します。")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. 入力、**拡張機能の名前** をクリックし、 **OK**ボタン

-----

既定では、`MessagesViewController.cs`ファイルはソリューションに追加されます。 これは、拡張機能へのメイン エントリ ポイントと、継承元、`MSMessageAppViewController`クラスです。

メッセージ framework には、ユーザーに利用可能なステッカーを表示するためのクラスが用意されています。

- `MSStickerBrowserViewController` ステッカーに表示される表示を制御します。 準拠している、`IMSStickerBrowserViewDataSource`インターフェイス ステッカー数と、特定のブラウザーのインデックスのステッカーを返します。
- `MSStickerBrowserView` -これは、使用可能なステッカーに表示されるビューです。
- `MSStickerSize` -ブラウザー ビューで表示するステッカーのグリッドの個々 のセルのサイズを決定します。

### <a name="creating-a-custom-sticker-browser"></a>カスタムのステッカー ブラウザーを作成します。

開発者が独自のステッカー ブラウザーを提供することによってユーザーにとってのステッカーをさらにカスタマイズできます (`MSMessageAppBrowserViewController`) でメッセージ アプリ拡張機能です。 独自のステッカー ブラウザーでは、メッセージ ストリームに含めるステッカーを選択するときに、ユーザーにステッカーがどのように表示されるかを変更します。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション パッド**拡張機能のプロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**  >  **iOS |Apple Watch** > **コント ローラーのインターフェイス**です。
2. 入力`StickerBrowserViewController`の**名前** をクリックし、**新規**ボタン。 

    [![](intro-to-message-app-extensions-images/browser01.png "StickerBrowserViewController を名を入力します。")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. 開く、`StickerBrowserViewController.cs`ファイルを編集します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプ ローラー**拡張機能のプロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**  >  **iOS |Apple Watch** > **コント ローラーのインターフェイス**です。
2. 入力`StickerBrowserViewController`の**名前** をクリックし、**新規**ボタン。 

    [![](intro-to-message-app-extensions-images/browser01w.png "StickerBrowserViewController を名を入力します。")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. 開く、`StickerBrowserViewController.cs`ファイルを編集します。

-----

ように、`StickerBrowserViewController.cs`次のようになります。

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

見て上記のコードの詳細。 拡張機能を提供するステッカーの記憶域を作成します。

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

2 つのメソッドをオーバーライドして、`MSStickerBrowserViewController`ブラウザーこのデータ ストアからのデータを提供するクラス。

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker`メソッドが拡張機能のバンドルからイメージ資産のパスを取得しの新しいインスタンスを作成することを使用して、`MSSticker`からこの資産は、コレクションに追加されます。

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

`LoadSticker`からメソッドを呼び出した`ViewDidLoad`(アプリのバンドルに含まれる)、名前付きのイメージ アセットからステッカーを作成し、ステッカーのコレクションに追加します。

独自のステッカー ブラウザーを実装するのには、編集、`MessagesViewController.cs`ファイルし、次のようになります。

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();


            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

見てこのコードの詳細で、カスタムのブラウザーの記憶域を作成します。

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

、`ViewDidLoad`メソッドがインスタンス化され、新しいブラウザー ウィンドウを構成します。

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

それを表示するビューをブラウザーが追加されます。

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>ステッカーをさらにカスタマイズ

メッセージ アプリ拡張機能に 2 つのクラスを含めることでステッカーをさらにカスタマイズができます。

- `MSStickerView`
- `MSSticker`

上記のメソッドを使用して、拡張機能は、標準のステッカー ブラウザー メソッドに依存しないステッカーの選択をサポートできます。 さらに、2 つの異なる表示モードのステッカーの表示を切り替えることもできます。

- **Compact** -ステッカー ビューがメッセージ ビューの下部にある 25% がここで、既定のモードです。
- **展開**-ステッカー表示メッセージ ビュー全体を入力します。

このステッカー ビュー切り替えることができるこれらのモード間で、プログラムまたは手動でユーザーがします。

次の 2 つの異なる表示モードの切り替えの処理の例を見てください。 2 つの異なるビュー コント ローラーは、各状態の必要があります。 `StickerBrowserViewController`ハンドル、 **Compact**ビューとは、次のような。

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

`AddStickerViewController`を処理する、**展開**ステッカー ビューと、次のような。

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);
            
            ...
        }
        #endregion
    }
}
```

`MessageViewController`要求された状態をドライブにこれらのビュー コント ローラーを実装します。

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

        public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            base.DidTransition (presentationStyle);

            // Take action based on style
            switch (presentationStyle) {
            case MSMessagesAppPresentationStyle.Compact:
                PresentStickerBrowser ();
                break;
            case MSMessagesAppPresentationStyle.Expanded:
                PresentAddSticker ();
                break;
            }
        }
        #endregion
    }
}
```

使用可能なコレクションに新しいステッカーを追加するユーザーが要求したときに、新しい`AddStickerViewController`表示されているコント ローラーとステッカー ビューが行われますが入力、**展開**ビュー。

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

選択すると、ユーザーを追加するステッカーが、その利用可能なコレクションに追加され、 **Compact**ビューが要求されます。

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition` 2 つのモードの切り替えを処理するメソッドをオーバーライドします。

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
``` 

## <a name="summary"></a>まとめ

この記事の内容がカバーされてと統合する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含む、**メッセージ**アプリとユーザーに新しい機能の存在します。 これは、テキスト、ステッカー、メディア ファイルおよび対話型のメッセージを送信する拡張機能の使用について説明します。



## <a name="related-links"></a>関連リンク

- [アイスクリーム ビルダー (サンプル)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [メッセージ参照](https://developer.apple.com/reference/messages)
- [アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
