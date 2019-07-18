---
title: Xamarin.iOS でメッセージ アプリ拡張機能の基本
description: この記事で示しますメッセージ アプリと連携し、新しい機能をユーザーに表示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含める方法です。
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: cdeaae6cb83062f0d84a3605582b9779c9f36145
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66178050"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Xamarin.iOS でメッセージ アプリ拡張機能の基本

_この記事で示しますメッセージ アプリと連携し、新しい機能をユーザーに表示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を含める方法です。_

新しいメッセージ アプリ拡張機能が統合には、10、iOS、**メッセージ**をユーザーに新しい機能がアプリを提示しています。 拡張機能には、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージ アプリ拡張機能について

前述のように、メッセージ アプリ拡張機能が統合、**メッセージ**をユーザーに新しい機能がアプリを提示しています。 拡張機能には、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信できます。 メッセージ アプリ拡張機能の 2 つの種類があります。

- **ステッカー パック**-ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカー パックは、すべてのコードを記述することがなく作成できます。
- **iMessage アプリ**-ステッカーを選択すると、テキストを入力する、(省略可能な型変換) によるメディア ファイルを含む、作成、編集、および相互作用のメッセージを送信するためのメッセージ アプリ内のカスタム ユーザー インターフェイスを表示することができます。

メッセージ アプリ拡張機能は、次の 3 つの主要なコンテンツ タイプを提供します。

- **対話型メッセージ**-アプリを生成するカスタム メッセージ コンテンツの種類は、メッセージで、アプリ ユーザーがタップしたときに、フォア グラウンドで起動されます。
- **ステッカー** -ユーザーの間で送信されるメッセージに含めることがアプリによって生成されたイメージします。
- **その他のサポートされているコンテンツ**- アプリは、写真、ビデオ、テキストなどのコンテンツを提供できます。 またはその他のコンテンツへのリンクの種類を常にメッセージ アプリでサポートされています。

新しい ios 10 では、メッセージのアプリが含まれています、独自の専用の組み込みアプリ ストアには。 メッセージ アプリ拡張機能が含まれているすべてのアプリが表示され、このストアで昇格します。 新しいメッセージ アプリ ドロワーが、ユーザーにすばやくアクセスを提供するメッセージのアプリ ストアからダウンロードされているすべてのアプリが表示されます。

また新しい iOS 10 では、Apple が追加ユーザーを簡単にアプリを検出できるインライン アプリ属性。 たとえば、1 人のユーザーは、2 つ目のユーザーが持っていないアプリから別にコンテンツを送信する場合 (たとえばステッカー) のようにインストールされている送信側のアプリの名前 下にあるメッセージ履歴内のコンテンツ。 ユーザーがアプリのタップした場合の名前、メッセージ アプリ ストアを開くこと、アプリ ストアで選択されています。

メッセージ アプリ拡張機能は、既存の iOS アプリ開発者が簡単に作成し、すべての標準的なフレームワークと標準の iOS アプリの機能へのアクセスことに似ています。 例:

- アプリ内購入へのアクセスがあります。
- これらは、Apple Pay にアクセスします。
- カメラなどのデバイスのハードウェアへのアクセスがあります。

メッセージ アプリ拡張機能は iOS 10 でのみサポート、ただし、これらの拡張機能を送信するコンテンツは watchOS と macOS デバイスで表示できます。 新しい_ページの [最近]_ メッセージ アプリ拡張機能のものを含む、携帯電話から送信された最新のステッカーを表示し、watch から留めませんを送信するユーザーを許可するが、watchOS 3 に追加します。

## <a name="about-the-messages-framework"></a>メッセージのフレームワークについて

新しいメッセージ フレームワークを iOS 10 では、ユーザーの iOS デバイスでメッセージ アプリ拡張機能とメッセージ アプリ間のインターフェイスを提供します。 ユーザーがメッセージ アプリ内からアプリを起動、このフレームワークは、アプリケーションの検出し、データとその UI をレイアウトするために必要なコンテキストを提供します。

アプリが起動されると、ユーザーは、メッセージを使用して共有する新しいコンテンツを作成することと対話します。 アプリは、メッセージのフレームワークを使用して処理用メッセージ アプリを新しく作成されたコンテンツを転送します。

フレームワークのメッセージとメッセージ アプリ拡張機能は、既存の iOS アプリの拡張機能のテクノロジの上に構築されます。 アプリ拡張機能の詳細については、Apple を参照してください[アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)します。

Apple によって、システム全体で提供されるその他の拡張ポイントとは異なり、開発者は、メッセージ アプリ自体が、コンテナーとして機能するためのメッセージ アプリ拡張機能のホスト アプリを提供するには必要ありません。 ただし、開発者には、新規または既存の iOS アプリ内でメッセージ アプリ拡張機能を含めると、バンドルと共に出荷のオプションがあります。

メッセージ アプリ拡張機能が iOS アプリのバンドルに含まれる場合は、デバイスのホーム画面とメッセージ アプリ内からメッセージ アプリ ドロワーの両方にアプリのアイコンが表示されます。 アプリ バンドルには含まれません場合、は、メッセージのアプリ ドロワーでメッセージ アプリ拡張機能のみが表示されます。

これは、システム メッセージ アプリ ドロワーや設定などの他の部分に表示されるアイコンに、開発者がメッセージ アプリ拡張機能のバンドルのアプリ アイコンを提供する必要は場合でも、ホスト アプリケーションのバンドルでメッセージ アプリ拡張機能が含まれていない、、拡張機能。

## <a name="about-stickers"></a>ステッカーについて

Apple では、他のメッセージの内容としてインラインを送信する iMessage ユーザー ステッカーを許可することで通信するための新しい方法としてステッカーが設計されていますか、メッセージ交換内で前のメッセージの吹き出しに接続されていることができます。

ステッカーは何ですか。

- メッセージ アプリ拡張機能を提供するイメージです。
- アニメーションまたは静的のいずれかのイメージがあります。
- アプリ内からイメージのコンテンツを共有する新しい方法を提供します。

ステッカーを作成する 2 つの方法はあります。

1. ステッカー パック メッセージ アプリ拡張機能はすべてのコードを含めずに Xcode 内から作成できます。 ステッカー、およびアプリ アイコンのアセットは、必要なのです。
2. 標準のメッセージ アプリ拡張機能を作成してメッセージのフレームワークを使用してコードからのステッカーを提供します。

### <a name="creating-sticker-packs"></a>ステッカー パックの作成

ステッカー パックは、Xcode 内の特殊なテンプレートから作成され、単にステッカーとして使用できるイメージ資産の静的なセットを提供します。 前述のように、すべてのコードは必要ありません、開発者は、ステッカーのアセット カタログの内部でステッカー パック フォルダーにイメージ ファイルをドラッグするだけです。

ステッカー Pack に含まれるイメージには、次の要件を満たすする必要があります。

- イメージは、PNG、APNG、GIF、または JPEG 形式である必要があります。 Apple は、ステッカーの資産を提供するときに PNG と APNG 形式のみを使用することを示します。
- アニメーションのステッカーは、APNG と GIF 形式のみをサポートします。
- ステッカーの画像は、ユーザーが、メッセージ交換のメッセージの吹き出し経由で配置できるため、透明な背景を提供します。
- 個別のイメージ ファイルは、500 kb 未満である必要があります。
- イメージは、その 206 x 206 ポイント、拡大または縮小、100 x 100 ポイントよりもをすることはできません。

> [!IMPORTANT]
> ステッカーの画像で常に提供する必要があります、 `@3x` 300 x 300 に 618 x 618 ピクセルの範囲内で解決します。 システムが自動的に生成、`@2x`と`@1x`必要に応じて実行時にバージョン。

Apple では、考え得るあらゆる状況で最適な表示されるようにさまざまな (白、黒、赤、黄色およびマルチ色分け) などさまざまな色付きの背景と上の写真、に対してステッカーのイメージ アセットのテストをお勧めします。

ステッカー パックには、次の 3 つの利用可能なサイズのいずれかでステッカーを指定できます。

- **小さな**- 100 x 100 ポイント。
- **Medium** - 136 x 136 ポイント。 これは、既定のサイズです。
- **大規模な**- 206 x 206 ポイント。

ステッカー パック全体のサイズを設定し、最適な結果のメッセージ アプリ内のステッカー ブラウザーで、要求されたサイズに一致するためのイメージ アセットのみを提供するには、Xcode の Attributes Inspector を使用します。

詳細についてを参照してください、 [Ice cream ビルダー](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)アプリと Apple の[メッセージ参照](https://developer.apple.com/reference/messages)します。

## <a name="creating-a-custom-sticker-experience"></a>カスタムのステッカー エクスペリエンスを作成します。

アプリに複数のコントロールまたはステッカー パックによって提供されるよりも柔軟性が必要な場合メッセージ アプリ拡張機能は、およびカスタム ステッカーのエクスペリエンスのメッセージのフレームワークを使用してステッカーを提供できます。

カスタム ステッカーのエクスペリエンスを作成する利点とは

1. アプリのユーザーにステッカーを表示する方法をカスタマイズするアプリを許可します。 などに、さまざまな色付きの背景または標準のグリッド レイアウト以外の形式で存在するステッカーです。
2. 静的なイメージの資産として含まれているのではなくコードから動的に作成するステッカーを使用できます。
3. App Store に新しいバージョンをリリースすることがなく、開発者の web サーバーから動的にダウンロードするステッカーのイメージ アセットを使用できます。
4. デバイスのカメラへのアクセスをステッカーの場を作成できます。
5. アプリ内購入は、ユーザーからのアプリ内の複数のステッカーを購入できるようにします。

カスタム ステッカーのエクスペリエンスを作成するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for mac の起動
2. メッセージ アプリ拡張機能を追加するソリューションを開きます。 
3. 選択**iOS** > **拡張機能** > **iMessage 拡張機能** をクリックし、**次**ボタン。 

    [![](intro-to-message-app-extensions-images/message01.png "IMessage 拡張機能を選択します。")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. 入力、**拡張機能名** をクリックし、 **次へ**ボタン。 

    [![](intro-to-message-app-extensions-images/message02.png "拡張機能の名前を入力します。")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. をクリックして、**作成**拡張機能を構築するボタンをクリックします。 

    [![](intro-to-message-app-extensions-images/message03.png "[作成] ボタンをクリックします。")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio を起動します。
2. メッセージ アプリ拡張機能を追加するソリューションを開きます。
3. 選択**iOS 拡張機能 > iMessage 拡張機能 (iOS)**  をクリックし、**次**ボタン。

    [![IMessage 拡張機能 (iOS) を選択します。](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. 入力、**名前** をクリックし、 **ok**ボタン

-----

既定で、`MessagesViewController.cs`ファイルがソリューションに追加されます。 これは、拡張機能へのメイン エントリ ポイントと、継承元、`MSMessageAppViewController`クラス。

メッセージ framework には、ユーザーに使用可能なステッカーを表示するためのクラスが用意されています。

- `MSStickerBrowserViewController` ビューで表示される、ステッカーを制御します。 準拠している、`IMSStickerBrowserViewDataSource`ステッカー カウントと、特定のブラウザーのインデックスのステッカーを返すインターフェイス。
- `MSStickerBrowserView` -これは、ビューで表示される使用可能なステッカーです。
- `MSStickerSize` -ブラウザー ビューで表示するステッカーのグリッドの個々 のセルのサイズを決定します。

### <a name="creating-a-custom-sticker-browser"></a>カスタムのステッカー ブラウザーを作成します。

開発者がカスタム ステッカーのブラウザーを提供することで、ユーザーのステッカー操作性をさらにカスタマイズできます (`MSMessageAppBrowserViewController`) でメッセージ アプリ拡張機能。 カスタムのステッカー ブラウザーのステッカーの表示方法をユーザーにメッセージ ストリームに含めるステッカーを選択すると変更点します。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**拡張機能のプロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**  >  **iOS |Apple Watch** > **インターフェイス コント ローラー**します。
2. 入力`StickerBrowserViewController`の**名前** をクリックし、**新規**ボタン。 

    [![](intro-to-message-app-extensions-images/browser01.png "名前の StickerBrowserViewController を入力します。")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. 開く、`StickerBrowserViewController.cs`ファイルを編集します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプ ローラー**拡張機能のプロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**  >  **iOS |Apple Watch** > **インターフェイス コント ローラー**します。
2. 入力`StickerBrowserViewController`の**名前** をクリックし、**新規**ボタン。 

    [![](intro-to-message-app-extensions-images/browser01.w157-sml.png "名前の StickerBrowserViewController を入力します。")](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
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

詳細で、上記のコードを見てに設定します。 拡張機能を提供するステッカーのストレージが作成されます。

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

2 つのメソッドをオーバーライドし、`MSStickerBrowserViewController`のブラウザーこのデータ ストアからのデータを提供するクラス。

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

`CreateSticker`メソッドが拡張機能のバンドルからのイメージ アセットのパスを取得しの新しいインスタンスを作成することを使用して、`MSSticker`からこの資産は、コレクションに追加します。

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

`LoadSticker`メソッドの呼び出し元`ViewDidLoad`(アプリのバンドルに含まれる)、名前付きのイメージ資産からステッカーを作成し、ステッカーのコレクションに追加します。

カスタム ステッカーのブラウザーを実装するには、編集、`MessagesViewController.cs`ファイルを開き、次のようになります。

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

詳細で、次のコードを見てに設定、カスタムのブラウザーの記憶域を作成します。

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

および、`ViewDidLoad`メソッド、インスタンス化して新しいブラウザーを構成します。

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

それを表示するビューに、ブラウザーが追加されます。

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>さらにステッカーのカスタマイズ

メッセージ アプリ拡張機能に 2 つのクラスを含めることでさらにステッカーのカスタマイズが可能です。

- `MSStickerView`
- `MSSticker`

上記のメソッドを使用して、拡張機能は標準のステッカー ブラウザー メソッドに依存しないステッカーの選択をサポートできます。 さらに、2 つの異なる表示モードの間でステッカーの表示を切り替えることができます。

- **Compact** -既定のモードは、このステッカー ビューが、メッセージ ビューの下部にある 25% を占める場所。
- **展開**-ステッカー表示メッセージ ビュー全体を入力します。

このステッカー ビュー切り替えることができますの間でこれらのモードは、プログラム、または手動でユーザーがします。

次の 2 つの異なる表示モードの切り替えを処理の例を見てください。 2 つの異なるビュー コント ローラーは、各状態の必要になります。 `StickerBrowserViewController`ハンドル、 **Compact**ビューとは、次のようにします。

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

`AddStickerViewController`を処理する、 **Expanded**ステッカー ビューと、次のようになります。

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

使用可能なコレクションに新しいステッカーを追加するユーザーが要求したときに新しい`AddStickerViewController`を作成、表示されているコント ローラーとそのステッカー ビューが入力、**展開**ビュー。

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

使用可能なコレクションに追加されます、ユーザーが ステッカーを追加する、および**Compact**ビューが要求されます。

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

この記事ではカバーされてメッセージ アプリ拡張機能を統合する Xamarin.iOS ソリューションに含める、**メッセージ**アプリとユーザーに新しい機能が存在します。 これは、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信する拡張機能の使用について説明します。



## <a name="related-links"></a>関連リンク

- [Ice cream ビルダー (サンプル)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [メッセージの参照](https://developer.apple.com/reference/messages)
- [アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
