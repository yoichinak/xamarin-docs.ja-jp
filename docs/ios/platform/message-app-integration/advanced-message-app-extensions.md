---
title: "高度なメッセージ アプリ拡張機能"
description: "この記事では、メッセージ アプリとの統合を新しい機能をユーザーに提示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を使用するための高度なテクニックを示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fcfd1fd2ec9271bb5e8d9e09b43b7dc4cf3b3f12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="advanced-message-app-extensions"></a>高度なメッセージ アプリ拡張機能

_この記事では、メッセージ アプリとの統合を新しい機能をユーザーに提示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を使用するための高度なテクニックを示します。_


新しい 10、iOS にメッセージ アプリ拡張機能 ' æ˜aœg '、**メッセージ**をユーザーにアプリと表示の新機能です。 拡張機能には、テキスト、ステッカー、メディア ファイルおよび対話型のメッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージ アプリ拡張機能の概要

前述のように、メッセージ アプリ拡張機能と統合、**メッセージ**をユーザーにアプリと表示の新機能です。 拡張機能には、テキスト、ステッカー、メディア ファイルおよび対話型のメッセージを送信できます。 2 つの種類のメッセージ アプリ拡張機能を使用できます。

- **ステッカー パック**-ユーザーがメッセージに追加できるステッカーのコレクションを格納します。 すべてのコードを記述することがなくステッカー パックを作成できます。
- **iMessage アプリ**-ステッカーを選択すると、テキストを入力する、(省略可能な型変換) とメディア ファイルを含む、作成、編集、および操作メッセージを送信する用アプリのメッセージのカスタム ユーザー インターフェイスを提供します。

メッセージ アプリ拡張機能は、次の 3 つの主要なコンテンツ タイプを提供します。

- **対話型のメッセージ**-アプリを生成するカスタム メッセージ コンテンツの種類は、ユーザーが、メッセージは、アプリでタップしたときに、フォア グラウンドで起動されます。
- **ステッカー** -ユーザーの間で送信されるメッセージに含めることがアプリによって生成されるイメージです。 参照してください、[アイスクリーム ビルダー](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)ステッカー パック アプリの実装例については、サンプル アプリケーションです。
- **その他のサポートされているコンテンツ**-、アプリケーションは、写真、ビデオ、テキストまたはメッセージ アプリを常にサポートされている種類のリンクなどのコンテンツを表示できます。

新しい 10、iOS にメッセージ アプリが含まれています、独自の組み込みの専用アプリ ストアには。 メッセージ アプリ拡張機能を含むすべてのアプリが表示され、このストアで昇格します。 新しいメッセージ アプリ ドロアーのクイック アクセスをユーザーに提供するメッセージの App Store からダウンロードされているすべてのアプリが表示されます。

また新しい iOS 10、Apple が追加ユーザーがアプリを簡単に検出するインライン アプリ属性。 たとえば、1 人のユーザーは、2 番目のユーザーが持っていないアプリから別のコンテンツを送信する場合 (たとえばステッカー) のようにインストールされている送信元のアプリの名前 下にあるメッセージ履歴内のコンテンツ。 場合、ユーザーはタップ アプリの名前、開くおメッセージ アプリ ストアとストアで選択されているアプリ。

メッセージ アプリ拡張機能は、既存の iOS アプリ開発者が作成に慣れており、すべての標準的なフレームワークと標準的な iOS アプリの機能へのアクセスが必要であることに似ています。 例:

- アプリ内購入へのアクセスがあります。
- Apple Pay へのアクセスがあります。
- カメラなどのデバイスのハードウェアへのアクセスがあります。

メッセージ アプリ拡張機能が iOS 10 でのみサポート、ただし、これらの拡張機能を送信する内容は watchOS および macOS デバイスで表示できます。 新しい_最近ページ_追加 watchOS 3 にはメッセージ アプリ拡張機能のものを含む、携帯電話から送信された最新のステッカーを表示され、ウォッチからそれらのステッカーを送信するユーザーを許可します。

## <a name="about-interactive-messages"></a>対話型のメッセージについて

対話型のメッセージは、カスタム メッセージのバブルを表示され、メッセージ アプリ拡張機能によって提供されます。 メッセージの入力 フィールドに挿入し、送信、ユーザーが対話型メッセージ コンテンツを作成できるようにします。

[![](advanced-message-app-extensions-images/interactive01.png "対話型のメッセージの内容を作成します。")](advanced-message-app-extensions-images/interactive01.png#lightbox)

受信側のユーザーは、メッセージの履歴を作成したメッセージ アプリ拡張機能を読み込むには、そのメッセージ バブルをタップして対話型のメッセージに返信できます。 拡張機能は起動の全画面表示をするされ、ユーザーが、応答を作成し、元のユーザーに送信できるようにします。

[![](advanced-message-app-extensions-images/interactive02.png "拡張機能は、全画面を起動")](advanced-message-app-extensions-images/interactive02.png#lightbox)


次のトピックについては、説明以下で詳細。

- メッセージの API の概要
- 拡張機能のライフ サイクル
- メッセージを作成します。
- メッセージを送信します。

## <a name="messages-api-overview"></a>メッセージの API の概要

ユーザーによって呼び出されると、メッセージ アプリ拡張機能が、縮小表示モードでのメッセージ履歴の下部に表示されます。

[![](advanced-message-app-extensions-images/interactive03.png "メッセージの API の概要")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController`メッセージ アプリ拡張機能内のオブジェクトが、拡張機能の表示が、ユーザーに表示されるときに呼び出されるメイン クラスです。
2. メッセージ交換としてユーザーに表示される、`MSConversation`オブジェクト インスタンス。
3. `MSMessage`クラスは、メッセージ交換で指定されたメッセージ バブルを表します。
4. `MSSession` メッセージを送信する方法を制御します。
5. `MSMessageTemplateLayout` メッセージの表示方法を制御します。

## <a name="the-extension-lifecycle"></a>拡張機能のライフ サイクル

アクティブになるメッセージ アプリ拡張機能のプロセスを参照してください。

[![](advanced-message-app-extensions-images/interactive04.png "アクティブになるメッセージ アプリ拡張機能の処理")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. 拡張機能を開始するには (たとえば、アプリのドロアー) から、メッセージのアプリは、プロセスを起動します。
2. `DidBecomeActive`メソッドが呼び出され、渡された、`MSConversation`を表す、メッセージ交換でメッセージ アプリ拡張機能が実行されています。
3. 拡張機能はオフの基づいているため、`UIViewController`両方`ViewWillAppear`と`ViewDidAppear`と呼ばれます。

次に、非アクティブになるメッセージ アプリ拡張機能のプロセスを見てみましょう。

[![](advanced-message-app-extensions-images/interactive05.png "非アクティブになるメッセージ アプリ拡張機能の処理")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. メッセージ アプリ拡張機能が非アクティブ化されているときに、`ViewWillDisappear`メソッドは最初に呼び出されます。
2. 続いて、`ViewDidDisappear`メソッドが呼び出されます。
3. `WillResignActive`メソッドが呼び出され、渡された、`MSConversation`を表す、メッセージ交換でメッセージ アプリ拡張機能が実行されています。 この時点でメッセージ アプリと拡張機能の間の接続は解放しています。
4. 後で、プロセスはメッセージ アプリを終了します。

拡張機能は、短期的なプロセスであるためには終了積極的に処理およびバッテリの電力を節約するためにシステム。 開発者は、留意すべきこの設計とメッセージ アプリ拡張機能を実装します。

## <a name="composing-a-message"></a>メッセージを作成します。

メッセージ アプリ拡張機能では、プロセスでが実行され、そのユーザー インターフェイスが表示されますが、新しいメッセージを作成する次のコードを使用できます。

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

このコードでは、新しい`MSMessage`いくつかのプロパティを設定し、(など`Url`)。 メッセージは、iOS でのみ作成でき、中に、送信できますを iOS および macOS の両方を表示します。

ユーザーは、macos メッセージ交換でメッセージのバブルをクリックすると、Mac は web ブラウザーで URL で指定されたアドレスを開いてしようとします。 その結果、developer の web サイトは、macOS 上の web ブラウザー内のメッセージの一部の表現ベースのマシンを表示できる必要があります。

`AccessibilityLabel`プロパティは、ユーザーにメッセージ交換のトラン スクリプトの読み取りにスクリーン リーダーによって使用されます。 `Layout`プロパティを指定する方法、メッセージが表示されます、現在のみ、`MSMessageTemplateLayout`はサポートされており、次のようになります。

[![](advanced-message-app-extensions-images/interactive06.png "MSMessageTemplateLayout テンプレート")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image`のプロパティ、 `MSMessageTemplateLayout` MessageBubble 画面上のメイン ボディのコンテンツを提供します。 `MediaFileUrl`プロパティもメッセージ バブルの本文のコンテンツでサポートされていないコンテンツは、`UIImage`バック グラウンドでのループは、ビデオ ファイル) などです。 両方の`Image`と`MediaFileUrl`プロパティが用意されて、`Image`プロパティが優先されます。 `MediaFileUrl` PNG、JPEG、GIF、および任意の形式で、メディア プレーヤー フレームワークで再生できるビデオをサポートしているメディア形式です。

推奨されるメディアのサイズは、3 倍の解像度で 300 x 300 ピクセルです。 少し大きくしたり小さく資産も受け入れられます、Apple が最適な結果を得るためのいくつかのさまざまなサイズでテストを提案します。 メッセージ アプリはダウン サンプルおよび必要に応じてこのメディアを拡張します。

アセットは、受信側に送信される、メディアを接続する際に自動的にトランス コードによって、ネットワーク経由の転送を最適化するメッセージ アプリします。 このため、Apple を行わないようメディアがスケール ダウンされ、転送の圧縮されたので、メッセージに関連付けられているメディアでテキストを含むため可能性のあるテキストをレンダリング判読できます。

`ImageTitle`と`ImageSubtitle`プロパティがメッセージのバブル チャートに表示されるメディアの説明を入力します。 これらのプロパティはテキストとして送信されます、受信デバイスに、鮮明に区別にレンダリングされる状態イメージの左下隅にします。

`Caption`、 `SubCaption`、`TrailingCaption`と`TrailingSubcaption`プロパティがさらに、イメージを説明し、イメージの下のセクション内に表示されます。 これらのプロパティをすべて設定`null`キャプション領域なしメッセージ バブルが作成されます。

[![](advanced-message-app-extensions-images/interactive07.png "キャプション領域なしメッセージ バブル")](advanced-message-app-extensions-images/interactive07.png#lightbox)

最後の注目するはメッセージ アプリがメッセージのバブルの左上隅でメッセージ アプリ拡張機能のアイコンを描画します。

## <a name="sending-a-message"></a>メッセージを送信します。

1 回、`MSMessage`がされて構成される、次のコードは、送信に使用することができます。

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation`のプロパティ、`MSMessagesAppViewController`でメッセージ アプリ拡張機能を起動した現在の会話を保持します。

呼び出す、`InsertMessage`の`MSConversation`会話にメッセージを含めるし、発生する可能性がありますのあるエラーの処理をします。 メッセージが正常に含まれる場合は、メッセージのバブルが入力フィールドに表示されます。

さらに、拡張機能などのメッセージ交換に異なる種類のデータを送信できます。

- **テキスト** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **添付ファイル** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **ステッカー**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});`場所`sticker`は、`MSSticker`です。

ユーザーが青いをタップして、メッセージを送信することで、新しいコンテンツは、[入力] フィールドには後、**送信**(と同様、一般的なメッセージ) のボタンをクリックします。 コンテンツを自動的に送信するメッセージ アプリ拡張機能の方法はありません、このプロセスは、ユーザーの制御下で、完全にします。

## <a name="handling-the-compact-and-expanded-modes"></a>処理 Compact および展開されているモード

メッセージ アプリ拡張機能は、次の 2 つの異なる表示モードのいずれかで表示できます。

[![](advanced-message-app-extensions-images/interactive08.png "2 つの異なる表示モードで表示されるメッセージ アプリ拡張機能: Compact と展開")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -メッセージ アプリ拡張機能がメッセージのビューの下部にある 25% がここで、既定のモードです。 コンパクト モードで、アプリはキーボード、水平方向にスクロールまたはスワイプ ジェスチャ レコグナイザーへのアクセスをありません。 アプリの [入力] フィールドにはアクセスとするために呼び出す`InsertMessage`は即座にあるユーザーに表示されます。
- **展開**-メッセージ アプリ拡張機能が全体のメッセージ ビューを塗りつぶします。 入力フィールドにアクセスできないが、キーボード、水平方向にスクロールおよびスワイプ ジェスチャ レコグナイザーへのアクセスには。

メッセージ アプリ拡張機能では、切り替えられるこのモードのプログラムまたは手動でユーザーがいつでもと、いずれかへの応答モードが変更にすぐにする必要があります。

次の 2 つの異なる表示モードの切り替えの処理の例を見てください。 2 つの異なるビュー コント ローラーは、各状態の必要があります。 `StickerBrowserViewController`ハンドル、 **Compact**ビューおよび`AddStickerViewController`を処理する、**展開**ビュー。

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
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

必要に応じて、アプリが使用することも、 `WillTransition` (上記 Icecream ビルダーの例で行われる) ようにユーザーに表示されます前に、表示モードの変更を処理するメソッド。 詳細についてを参照してください、[ステッカーをさらにカスタマイズ](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md)ドキュメント。

## <a name="replying-to-a-message"></a>メッセージに返信します。

メッセージ アプリ拡張機能は、メッセージに返信するときに処理する必要がある 2 つのケースがあります。

[![](advanced-message-app-extensions-images/interactive09.png "非アクティブとアクティブ モードでメッセージ アプリ拡張機能")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **拡張機能が非アクティブ**-ユーザーがタップ拡張機能をアクティブ化し、対話型のメッセージ交換を続行するメッセージのトラン スクリプトでメッセージ アプリ拡張機能のメッセージのバブルの 1 つを使用する必要があります。
- **拡張機能が有効になって**-ユーザーが拡大表示モードに切り替わるし、中断した場所から対話型プロセスを続行するメッセージのトラン スクリプトでメッセージ アプリ拡張機能のメッセージのバブルをタップします。

### <a name="the-extension-is-inactive"></a>拡張機能が非アクティブです。

メッセージのトラン スクリプトでユーザーがメッセージのバブルをタップすると、メッセージ アプリ拡張機能がアクティブでない、次の処理が行われます。

[![](advanced-message-app-extensions-images/interactive10.png "非アクティブのメッセージ バブルの処理")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. ユーザーは、拡張機能のメッセージのバブルをタップします。
2. 拡張機能が起動されたときに、メッセージ アプリは、プロセスを起動します。
3. `DidBecomeActive`メソッドが呼び出され、渡された、`MSConversation`を表す、メッセージ交換でメッセージ アプリ拡張機能が実行されています。
4. 拡張機能はオフの基づいているため、`UIViewController`両方`ViewWillAppear`と`ViewDidAppear`と呼ばれます。

プロセスが完了したら、展開ビュー モードでメッセージ アプリ拡張機能が表示されます。

### <a name="the-extension-is-active"></a>拡張機能がアクティブ

メッセージ トラン スクリプトでユーザーがメッセージのバブルをタップすると、メッセージ アプリ拡張機能がアクティブで、次の処理が行われます。

[![](advanced-message-app-extensions-images/interactive11.png "作業中のメッセージ バブルの処理")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. ユーザーは、拡張機能のメッセージのバブルをタップします。
2. メッセージ アプリ拡張機能が既にアクティブであるため、`WillTransition`のメソッド、`MSMessagesAppViewController`コンパクトなから展開の表示モードへの切り替えを処理します。
3. `DidSelectMessage`のメソッド、`MSMessagesAppViewController`が呼び出され、渡された、`MSMessage`と`MSConversation`メッセージ バブルが属するです。
4. `DidTransition`のメソッド、`MSMessagesAppViewController`コンパクトなから展開の表示モードへの切り替えを処理します。

ここでも、プロセスが完了したら、展開されたビュー モードでメッセージ アプリ拡張機能が表示されます。

### <a name="accessing-the-selected-message"></a>選択したメッセージへのアクセス

どちらの場合、ユーザーがメッセージのアプリ拡張機能に属しているメッセージ バブルをタップしたときに、必要がありますにアクセスするために、`MSMessage`を使用してがタップされた、`SelectedMessage`のプロパティ、`MSConversation`です。

例:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

メッセージ アプリ拡張機能の UI に表示される、選択したメッセージと応答を作成するユーザーを許可する必要があります。

## <a name="removing-partially-completed-messages"></a>部分的に削除メッセージを完了

2 つのユーザーのメッセージ交換の間の対話型のメッセージ交換のさまざまな手順を送信すると、処理を行ってメッセージ トラン スクリプトが読みにくく、部分的に完了したメッセージのバブルを起動できます。

[![](advanced-message-app-extensions-images/interactive12.png "部分的に完了したメッセージ バブルできるメッセージのトラン スクリプトを雑然とさせること")](advanced-message-app-extensions-images/interactive12.png#lightbox)

代わりに、メッセージ アプリ拡張機能では、メッセージ トラン スクリプト内の簡潔なコメントに、前のメッセージのバブルを縮小する必要があります。

[![](advanced-message-app-extensions-images/interactive13.png "メッセージ、トラン スクリプトの前のメッセージのバブルを折りたたむ")](advanced-message-app-extensions-images/interactive13.png#lightbox)

これは、処理を使用して、`MSSession`の既存の手順をすべてを折りたたむにします。 そのため、`DidSelectMessage`のメソッド、`MSMessagesAppViewController`クラスは、次のように変更できませんでした。

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

場合は、選択したメッセージは既に終了して、 `MSSession`、それ以外の場合、新しい使用が`MSSession`を作成します。 `SummaryText`のプロパティ、`MSMessage`折りたたまれている前の手順にキャプションを追加するために使用します。 場合、`SummaryText`プロパティに設定されている`null`、メッセージ交換の前の手順はメッセージ交換のトラン スクリプトから完全に削除されます。

## <a name="advanced-message-api-features"></a>メッセージの API 機能の詳細

上で説明した新しいメッセージ API の基本的な機能を次を調べます Apple が、framework に組み込まれている高度な機能の一部。

最初に、その他のいくつかのオーバーライド メソッドでは、`MSMessagesAppViewController`メッセージ交換をより深いアクセスを提供するクラス。

- `DidStartSendingMessage` -これは、ユーザーが [送信] ボタンをタップしたときに呼び出されます。 メッセージの送信プロセスが開始されているだけで、受信者に配信が実際にされたことはありません。
- `DidCancelSendingMessage` -このエラーは、ユーザーがタップする場合、 *X*トラン スクリプトのメッセージ交換のメッセージのバブルの右上隅のボタンをクリックします。
- `DidReceiveMessage` -このメソッドは、メッセージ アプリ拡張機能がアクティブなときに呼び出されますが、メッセージ交換の参加者のいずれかから新しいメッセージを受信しました。

### <a name="group-conversations"></a>グループのメッセージ交換

(3 つ以上の個人) を持つグループのメッセージ交換に関係しているユーザーを設計およびメッセージ アプリ拡張機能を実装するときに考慮する必要がこの中に、メッセージ アプリ拡張機能を使用できます。

見て、次の interaction 3 人のユーザーとグループのメッセージ交換で。

[![](advanced-message-app-extensions-images/interactive14.png "3 人のユーザーとグループのメッセージ交換内での対話")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. ユーザー 1 はグループ対話型のメッセージを送信ハンバーガー トッピングを選択するには、ユーザー 2 および 3 のユーザーを確認します。
2. ユーザー 2 は、トマトを選択します。
3. ユーザー 3 は、にぎやかを選択します。
4. ユーザー 2 と 3 のユーザーの両方のオプションは、ほぼ同時に、ユーザー 1 に到着します。 その結果、ユーザー 2 の選択に概要を示す行が折りたたまれているしは使用できません。 このケースも反転されている可能性があります、表示されているとユーザーの 3 を解除されているのと、ユーザー 2 の選択しました。

どちらの場合、この動作とはありません必要なユーザー 1 は、ユーザー 2 と 3 のユーザーの両方のオプションにアクセスできる必要があります。 このような状況を処理するには、Apple が提案されてメッセージ アプリ拡張機能がクラウドでメッセージの状態を格納しの URL プロパティを使用できる、 `MSMessage` (ユーザー間で送信されるを取得) この状態にアクセスします。

ユーザーは、メッセージを送信するときにセッション トークンが生成され、現在のメッセージの状態を持つクラウドにプッシュします。 トラン スクリプトのメッセージ交換のメッセージのバブルをタップする、クラウドから現在のセッション状態を取得するセッション トークンが使用されます。

### <a name="sender-identifiers"></a>送信者 Id

メッセージの送信者の識別子へのアクセスを説明するには、グループのメッセージ交換が上記の例を実行します。

[![](advanced-message-app-extensions-images/interactive15.png "グループのメッセージ交換の識別子を送信します。")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. ユーザー 1 がグループ対話型のメッセージを送信するもう一度、ハンバーガー トッピングを選択するには、ユーザー 2 および 3 のユーザーを確認します。
2. ユーザー 3 は、にぎやかを取得します。
3. ユーザー 1 に戻す 3 のユーザーの選択が到着して、ユーザー 2 がまだ返信がありません。
4. Apple は非常にユーザーのプライバシーに関係のため、メッセージ アプリ拡張機能のみが知って一意識別子 (として、 `NSUUID`)、メッセージ交換の各参加者に割り当てられています。 ローカルのデバイスには、現在のユーザーの識別子だけ呼ばれます。
5. `MSMessage`が、`SenderIdentifier`ユーザーのいずれかに一致するプロパティの参加要素の一覧で既知の拡張機能にします。
6. 各ユーザー デバイスは、もう一度、ローカル ユーザーの識別子のみがわかっている参加要素のリストのコピーを持っています。
7. メッセージが送信されると、その`SenderIdentifier`プロパティは、ローカル ユーザーのことにも呼ばれます。

送信者の識別子は、次の方法で使用できます。

- 参加要素の一覧を確認して、拡張機能は、メッセージ交換のユーザーの数を取得できます。
- 拡張機能は、ユーザーからのメッセージを受信したときを追跡できます送信者の識別子。 同じ送信者の識別子を持つ別のメッセージを受信した場合、拡張機能同じユーザーからであることを認識します。
- メッセージ交換内の特定のユーザーの識別に使用できます。

送信者の識別子のフィールドのテキストのいずれかで使用できます、`MSMessageTemplateLayout`にドル記号をプレフィックスとして付けること (`$`)。 例:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

メッセージ アプリでは、この種の書式でメッセージのバブルを表示するときに置き換え、`$uuid...`メッセージを送信したメッセージ交換の参加者の連絡先の名前に置き換えます。

送信者の Id はので、そのユーザー 1 のデバイスとユーザー 3 のデバイスに注意してください上の図を見て異なる一意の送信元識別子、メッセージ交換の参加者ごとに各デバイスで一意です。

送信者の識別子は、メッセージ アプリ拡張機能のインストールにスコープされます。 ように、ユーザーは、アンインストールし、メッセージ アプリ拡張機能を再インストールする場合、新しいインストールの新しい送信者 Id が生成されます。

送信者 Id にアクセスするには、拡張機能が次のコードを使用できます。

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>サポートされているプラットフォーム

Apple の次のプラットフォームでメッセージ アプリ拡張機能によって生成された対話型のメッセージが配信されます。

- watchOS 3
- macOS Sierra
- iOS 10

次の 3 つのプラットフォームでは、iOS 10 のみは対話型のメッセージを生成するユーザーを許可します。 Macos Sierra、ユーザーが対話型のメッセージ バブルをクリックした場合、URL に接続されている、 `MSMessage` Safari では表示され、メッセージの表現が表示されます。

WatchOS では、メッセージ アプリには、ハンドオフ、ユーザーが、応答を組み込むことができますが接続されている iOS デバイスに対話型のメッセージことができます。

メッセージの新しい API では、Apple な古いプラットフォームで対話型のメッセージが受信した場合のフォールバック サポートがあります。

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

2 つの独立したメッセージとしてフォールバック形式で転送されます。

- テンプレートのレイアウトによって提供されるように、イメージが 1 つがされます。
- 他の URL となるで許可されている、`MSMessage`です。

## <a name="summary"></a>まとめ

この記事とを統合する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を使用するための高度な技法を提示した、**メッセージ**アプリとユーザーに新しい機能の存在します。


## <a name="related-links"></a>関連リンク

- [アイスクリーム ビルダー (サンプル)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [メッセージ参照](https://developer.apple.com/reference/messages)
- [アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
