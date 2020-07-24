---
title: Xamarin. iOS でのメッセージアプリ拡張機能の基本
description: この記事では、メッセージアプリと統合し、ユーザーに新しい機能を提供する Xamarin. iOS ソリューションにメッセージアプリ拡張機能を含める方法を示します。
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 2cc27b18bdb58ee633cae2d61e8cc6a8064df581
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937128"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Xamarin. iOS でのメッセージアプリ拡張機能の基本

_この記事では、メッセージアプリと統合し、ユーザーに新しい機能を提供する Xamarin. iOS ソリューションにメッセージアプリ拡張機能を含める方法を示します。_

IOS 10 を初めて使用する場合、メッセージアプリ拡張機能は**Messages**アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージアプリの拡張機能について

前述のように、メッセージアプリ拡張機能は**Messages**アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。 次の2種類のメッセージアプリ拡張機能を使用できます。

- **ステッカーパック**-ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカーパックは、コードを記述せずに作成できます。
- **IMessage アプリ**-メッセージアプリ内にカスタムユーザーインターフェイスを提供して、ステッカーの選択、テキストの入力 (オプションの型変換を含む)、および操作メッセージの作成、編集、および送信を行うことができます。

メッセージアプリの拡張機能は、次の3つの主要なコンテンツの種類を提供します。

- **対話型メッセージ**-アプリが生成するカスタムメッセージコンテンツの一種であり、ユーザーがメッセージをタップすると、アプリがフォアグラウンドで起動されます。
- **ステッカー** -ユーザー間で送信されるメッセージに含めることができる、アプリによって生成されるイメージです。
- **サポートされているその他のコンテンツ**-アプリは、メッセージアプリによって常にサポートされているその他のコンテンツの種類への写真、ビデオ、テキスト、リンクなどのコンテンツを提供できます。

IOS 10 の新機能であるメッセージアプリには、独自の専用の組み込みアプリストアが含まれるようになりました。 メッセージアプリの拡張機能を含むすべてのアプリが、このストアに表示され、昇格されます。 新しいメッセージアプリドロワーには、Messages App Store からダウンロードされたすべてのアプリが表示され、ユーザーにすばやくアクセスできるようになります。

また、iOS 10 の新機能である Apple では、ユーザーがアプリを簡単に検出できるようにするインラインアプリの属性が追加されています。 たとえば、1人のユーザーが、2番目のユーザーがインストールしていないアプリから別のユーザーにコンテンツを送信した場合 (ステッカーなど)、送信元アプリの名前はメッセージ履歴のコンテンツの下に一覧表示されます。 ユーザーがアプリ名をタップすると、メッセージアプリストアが開き、ストアでアプリが選択されます。

メッセージアプリの拡張機能は、開発者が作成に慣れている既存の iOS アプリに似ており、標準の iOS アプリのすべての標準フレームワークと機能にアクセスできます。 次に例を示します。

- アプリ内購入にアクセスできます。
- Apple Pay にアクセスできます。
- これらのユーザーは、カメラなどのデバイスハードウェアにアクセスできます。

メッセージアプリの拡張機能は、iOS 10 でのみサポートされていますが、これらの拡張機能が送信するコンテンツは、watchOS および macOS デバイスで表示できます。 WatchOS 3 に追加された新しい [ _Recents ページ_には、電話から送信された最近のステッカー (メッセージアプリの拡張機能を含む) が表示され、ユーザーはこれらのステッカーをウォッチから送信できます。

## <a name="about-the-messages-framework"></a>Messages Framework について

IOS 10 の新機能である Messages framework では、メッセージアプリの拡張機能と、ユーザーの iOS デバイス上のメッセージアプリとの間のインターフェイスが提供されています。 ユーザーが Messages アプリ内からアプリを起動すると、このフレームワークによってアプリが検出され、UI をレイアウトするために必要なデータとコンテキストが提供されます。

アプリが起動されると、ユーザーはこのアプリと対話して、メッセージを介して共有する新しいコンテンツを作成します。 その後、アプリは Messages フレームワークを使用して、新しく作成されたコンテンツを処理のためにメッセージアプリに転送します。

Messages framework と Message Apps の拡張機能は、既存の iOS アプリ拡張機能の上に構築されています。 アプリ拡張機能の詳細については、「Apple の[アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)」を参照してください。

システム全体で Apple が提供する他の拡張ポイントとは異なり、メッセージアプリ自体はコンテナーとして機能するため、開発者はメッセージアプリの拡張機能用のホストアプリを提供する必要はありません。 ただし、開発者は、新規または既存の iOS アプリ内に Message Apps 拡張機能を含め、バンドルと共に出荷することができます。

メッセージアプリの拡張機能が iOS アプリのバンドルに含まれている場合、アプリのアイコンは、デバイスのホーム画面と、Messages アプリ内からのメッセージアプリドロアーの両方に表示されます。 アプリバンドルに含まれていない場合、メッセージアプリの拡張機能はメッセージアプリドロワーにのみ表示されます。

メッセージアプリの拡張機能がホストアプリバンドルに含まれていない場合でも、開発者は、メッセージアプリの拡張機能のバンドルにアプリアイコンを提供する必要があります。これは、拡張機能のメッセージアプリのドロアーや設定など、システムの他の部分に表示されるアイコンです。

## <a name="about-stickers"></a>ステッカーの概要

Apple は、ステッカーを他のメッセージコンテンツとしてインラインで送信できるようにすることで、iMessage ユーザーが通信できるようにするための新しい方法としてステッカーを設計しました。また、メッセージ交換内の前のメッセージバブルに添付することもできます。

ステッカーとは何ですか。

- これらは、メッセージアプリの拡張機能によって提供されるイメージです。
- アニメーション化されたイメージまたは静的なイメージを使用できます。
- アプリ内からイメージコンテンツを共有するための新しい方法が用意されています。

ステッカーを作成するには、次の2つの方法があります。

1. ステッカーパックメッセージアプリの拡張機能は、コードを含まない Xcode の内部から作成できます。 必要なのは、ステッカーとアプリアイコンのアセットだけです。
2. メッセージフレームワークを使用してコードからステッカーを提供する、標準のメッセージアプリ拡張機能を作成する。

### <a name="creating-sticker-packs"></a>ステッカーパックの作成

ステッカーパックは、Xcode 内の特殊なテンプレートから作成され、ステッカーとして使用できる画像アセットの静的なセットを提供します。 前述のように、コードは必要ありません。開発者は、イメージファイルをステッカー資産カタログ内のステッカー Pack フォルダーにドラッグするだけです。

イメージをステッカーパックに含めるには、次の要件を満たしている必要があります。

- 画像は、PNG、APNG、GIF、または JPEG 形式である必要があります。 Apple では、ステッカーのアセットを提供するときに、PNG と APNG の形式のみを使用することを提案しています。
- アニメーションステッカーは、APNG と GIF 形式のみをサポートしています。
- ステッカー画像は、ユーザーによってメッセージ交換のメッセージバブルに配置できるため、透明な背景を提供する必要があります。
- 個々のイメージファイルは 500 kb 未満である必要があります。
- イメージは 100 x 100 x 206 ポイントより小さくすること206はできません。

> [!IMPORTANT]
> ステッカー画像は `@3x` 、300 x 300 から 618 x 618 ピクセル範囲の解像度で常に指定する必要があります。 システムは、必要に応じて、およびの各バージョンを実行時に自動的に生成し `@2x` `@1x` ます。

Apple では、さまざまな色の背景 (白、黒、赤、黄、および複数色) に対してステッカーイメージ資産をテストし、すべての可能性がある状況に最適であることを確認することを提案しています。

ステッカーパックでは、次の3つのサイズのいずれかでステッカーを提供できます。

- **小**-100 x 100 ポイント。
- **中**~ 136 x 136 ポイント。 これが既定のサイズです。
- **Large** -206 x 206 ポイント。

Xcode の属性インスペクターを使用して、ステッカーパック全体のサイズを設定し、要求されたサイズに一致する画像アセットのみを提供します。これは、Messages アプリ内のステッカーブラウザーで最適な結果を得るために使用します。

詳細については、[アイスクリームビルダー](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)アプリと Apple の[メッセージリファレンス](https://developer.apple.com/reference/messages)を参照してください。

## <a name="creating-a-custom-sticker-experience"></a>カスタムステッカーエクスペリエンスの作成

ステッカーパックによって提供されるよりも多くの制御や柔軟性が必要なアプリの場合は、メッセージアプリの拡張機能を含めることができます。また、カスタムステッカーエクスペリエンスのために、メッセージフレームワークを介してステッカーを提供できます。

カスタムステッカーエクスペリエンスを作成する利点は何ですか。

1. アプリのユーザーにステッカーを表示する方法をカスタマイズできます。 たとえば、標準のグリッドレイアウト以外の形式でステッカーを表示する場合や、別の背景色を使用する場合などです。
2. シールされたイメージアセットとしてではなく、コードからステッカーを動的に作成できるようにします。
3. アプリストアに新しいバージョンをリリースすることなく、開発者の web サーバーからステッカーイメージアセットを動的にダウンロードできます。
4. デバイスのカメラにアクセスして、その場でステッカーを作成できるようにします。
5. アプリ内購入を許可し、ユーザーがアプリの内部からより多くのステッカーを購入できるようにします。

カスタムのステッカーエクスペリエンスを作成するには、次の手順を実行します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac を起動します。
2. ソリューションを開いて、メッセージアプリの拡張機能を追加します。
3. [ **IOS**  >  **Extensions**  >  **iMessage Extension** ] を選択し、[**次へ**] ボタンをクリックします。

    [![IMessage 拡張機能の選択](intro-to-message-app-extensions-images/message01.png)](intro-to-message-app-extensions-images/message01.png#lightbox)
4. **拡張機能の名前**を入力し、[**次へ**] ボタンをクリックします。

    [![拡張機能名を入力してください](intro-to-message-app-extensions-images/message02.png)](intro-to-message-app-extensions-images/message02.png#lightbox)
5. [**作成**] ボタンをクリックして拡張機能をビルドします。

    [![[作成] ボタンをクリックします。](intro-to-message-app-extensions-images/message03.png)](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio を起動します。
2. ソリューションを開いて、メッセージアプリの拡張機能を追加します。
3. [ **Ios Extensions > IMessage Extension (ios)** ] を選択し、[**次へ**] ボタンをクリックします。

    [![IMessage 拡張機能 (iOS) を選択します。](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. **名前**を入力し、[ **OK** ] ボタンをクリックします。

-----

既定では、 `MessagesViewController.cs` ファイルはソリューションに追加されます。 これは拡張機能へのメインエントリポイントであり、クラスから継承され `MSMessageAppViewController` ます。

Messages フレームワークには、ユーザーに使用可能なステッカーを表示するためのクラスが用意されています。

- `MSStickerBrowserViewController`-ステッカーが表示されるビューを制御します。 また、 `IMSStickerBrowserViewDataSource` 指定されたブラウザーインデックスのステッカーカウントとステッカーを返すインターフェイスに準拠しています。
- `MSStickerBrowserView`-使用可能なステッカーが表示されるビューです。
- `MSStickerSize`-ブラウザービューに表示されるステッカーのグリッドの個々のセルサイズを決定します。

### <a name="creating-a-custom-sticker-browser"></a>カスタムステッカーブラウザーを作成する

開発者は、 `MSMessageAppBrowserViewController` メッセージアプリ拡張機能にカスタムステッカーブラウザー () を提供することで、ユーザーのステッカーエクスペリエンスをさらにカスタマイズできます。 カスタムステッカーブラウザーは、メッセージストリームに含めるステッカーを選択するときに、ユーザーにステッカーを表示する方法を変更します。

次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、拡張機能のプロジェクト名を右クリックし、[**追加**] [  >  **新しいファイル...**  >  ] の順に選択します。**iOS |Apple Watch**  >  **インターフェイスコントローラー**。
2. `StickerBrowserViewController`**名前**として「」と入力し、[**新規**] ボタンをクリックします。

    [![名前として「StickerBrowserViewController」と入力します。](intro-to-message-app-extensions-images/browser01.png)](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. 編集のために `StickerBrowserViewController.cs` ファイルを開きます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、拡張機能のプロジェクト名を右クリックし、[**追加**] [  >  **新しいファイル...**  >  ] の順に選択します。**iOS |Apple Watch**  >  **インターフェイスコントローラー**。
2. `StickerBrowserViewController`**名前**として「」と入力し、[**新規**] ボタンをクリックします。

    [![名前として「StickerBrowserViewController」と入力します。](intro-to-message-app-extensions-images/browser01.w157-sml.png)](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
3. 編集のために `StickerBrowserViewController.cs` ファイルを開きます。

-----

次のようにし `StickerBrowserViewController.cs` ます。

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

上記のコードを詳しく見てみましょう。 拡張機能によって提供されるステッカーのストレージが作成されます。

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

とは、 `MSStickerBrowserViewController` このデータストアからブラウザーにデータを提供するために、クラスの2つのメソッドをオーバーライドします。

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

メソッドは、 `CreateSticker` 拡張機能のバンドルからイメージ資産のパスを取得し、それを使用して、この資産からの新しいインスタンスを作成し `MSSticker` 、コレクションに追加します。

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

`LoadSticker`メソッドは、 `ViewDidLoad` 名前付きイメージアセット (アプリのバンドルに含まれています) からステッカーを作成し、ステッカーのコレクションに追加するために、から呼び出されます。

カスタムステッカーブラウザーを実装するには、ファイルを編集し、次のようにし `MessagesViewController.cs` ます。

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

このコードの詳細を確認すると、カスタムブラウザー用のストレージが作成されます。

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

メソッドでは `ViewDidLoad` 、新しいブラウザーをインスタンス化して構成します。

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

次に、ブラウザーをビューに追加して表示します。

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>ステッカーのカスタマイズ

メッセージアプリ拡張機能に2つのクラスを含めることで、さらにステッカーをカスタマイズできます。

- `MSStickerView`
- `MSSticker`

上記のメソッドを使用すると、拡張機能は、標準のステッカーブラウザーメソッドに依存しないステッカー選択をサポートできます。 また、ステッカー表示は、2つの異なる表示モードを切り替えることができます。

- **Compact** -ステッカービューがメッセージビューの下位25% を占める既定のモードです。
- [**展開**済み]-ステッカービューは、メッセージビュー全体を表示します。

このステッカービューは、プログラムによって、またはユーザーが手動で切り替えることができます。

2つの異なる表示モードの切り替えを処理する次の例を見てみましょう。 状態ごとに2つの異なるビューコントローラーが必要になります。 は `StickerBrowserViewController` **コンパクト**ビューを処理し、次のようになります。

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

は、 `AddStickerViewController` **展開**されたステッカービューを処理し、次のようになります。

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

は、 `MessageViewController` 要求された状態を駆動するために、これらのビューコントローラーを実装します。

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

ユーザーが使用可能なコレクションに新しいステッカーを追加するように要求すると、新しい `AddStickerViewController` が表示されるコントローラーになり、ステッカービューが**展開**されたビューになります。

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

ユーザーが追加するステッカーを選択すると、使用可能なコレクションに追加され、**コンパクト**ビューが要求されます。

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition`メソッドは、2つのモード間の切り替えを処理するためにオーバーライドされます。

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

この記事では、Xamarin. iOS ソリューションにおけるメッセージアプリ拡張機能について説明しました。このソリューションは、 **Messages**アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能を使用して、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信します。

## <a name="related-links"></a>関連リンク

- [アイスクリームビルダー (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [メッセージリファレンス](https://developer.apple.com/reference/messages)
- [アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
