---
title: Xamarin.iOS での高度なメッセージ アプリ拡張機能
description: この記事では、メッセージ アプリと連携し、新しい機能をユーザーに表示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を使用するための高度なテクニックを示します。
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: baceb59116dd907918b34eca4f44293051190954
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61155648"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Xamarin.iOS での高度なメッセージ アプリ拡張機能

_この記事では、メッセージ アプリと連携し、新しい機能をユーザーに表示する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を使用するための高度なテクニックを示します。_


新しいメッセージ アプリ拡張機能が統合には、10、iOS、**メッセージ**をユーザーに新しい機能がアプリを提示しています。 拡張機能には、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージ アプリ拡張機能について

前述のように、メッセージ アプリ拡張機能が統合、**メッセージ**をユーザーに新しい機能がアプリを提示しています。 拡張機能には、テキスト、ステッカー、メディア ファイル、および対話型メッセージを送信できます。 メッセージ アプリ拡張機能の 2 つの種類があります。

- **ステッカー パック**-ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカー パックは、すべてのコードを記述することがなく作成できます。
- **iMessage アプリ**-ステッカーを選択すると、テキストを入力する、(省略可能な型変換) によるメディア ファイルを含む、作成、編集、および相互作用のメッセージを送信するためのメッセージ アプリ内のカスタム ユーザー インターフェイスを表示することができます。

メッセージ アプリ拡張機能は、次の 3 つの主要なコンテンツ タイプを提供します。

- **対話型メッセージ**-アプリを生成するカスタム メッセージ コンテンツの種類は、メッセージで、アプリ ユーザーがタップしたときに、フォア グラウンドで起動されます。
- **ステッカー** -ユーザーの間で送信されるメッセージに含めることがアプリによって生成されたイメージします。 参照してください、 [Ice cream ビルダー](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)ステッカー パック アプリの実装例については、サンプル アプリです。
- **その他のサポートされているコンテンツ**-アプリは、写真、ビデオ、テキストまたはメッセージ アプリを常にサポートされている種類のリンクなどのコンテンツを提供できます。

新しい ios 10 では、メッセージのアプリが含まれています、独自の専用の組み込みアプリ ストアには。 メッセージ アプリ拡張機能が含まれているすべてのアプリが表示され、このストアで昇格します。 新しいメッセージ アプリ ドロワーが、ユーザーにすばやくアクセスを提供するメッセージのアプリ ストアからダウンロードされているすべてのアプリが表示されます。

また新しい iOS 10 では、Apple が追加ユーザーを簡単にアプリを検出できるインライン アプリ属性。 たとえば、1 人のユーザーは、2 つ目のユーザーが持っていないアプリから別にコンテンツを送信する場合 (たとえばステッカー) のようにインストールされている送信側のアプリの名前 下にあるメッセージ履歴内のコンテンツ。 ユーザーがアプリのタップした場合の名前、メッセージ アプリ ストアを開くこと、アプリ ストアで選択されています。

メッセージ アプリ拡張機能は、既存の iOS アプリ開発者が使い慣れたを作成して、すべての標準的なフレームワークと標準の iOS アプリの機能へのアクセスことに似ています。 例:

- アプリ内購入へのアクセスがあります。
- これらは、Apple Pay にアクセスします。
- カメラなどのデバイスのハードウェアへのアクセスがあります。

メッセージ アプリ拡張機能は iOS 10 でのみサポート、ただし、これらの拡張機能を送信するコンテンツは watchOS と macOS デバイスで表示できます。 新しい_ページの [最近]_ メッセージ アプリ拡張機能のものを含む、携帯電話から送信された最新のステッカーを表示し、watch から留めませんを送信するユーザーを許可するが、watchOS 3 に追加します。

## <a name="about-interactive-messages"></a>対話型メッセージについて

対話型メッセージは、カスタム メッセージ バブルの提示され、メッセージ アプリ拡張機能によって提供されます。 メッセージ [入力] フィールドに挿入し、送信、対話型メッセージ コンテンツを作成するユーザーを許可します。

[![](advanced-message-app-extensions-images/interactive01.png "対話型メッセージのコンテンツを作成します。")](advanced-message-app-extensions-images/interactive01.png#lightbox)

受信側のユーザーは、メッセージの履歴を作成したメッセージ アプリ拡張機能を読み込むには、そのメッセージ バブルをタップして、対話型メッセージに返信できます。 拡張機能は起動の全画面表示にして、応答を作成し、元のユーザーに送信するユーザーを許可します。

[![](advanced-message-app-extensions-images/interactive02.png "拡張機能は、全画面表示を起動")](advanced-message-app-extensions-images/interactive02.png#lightbox)


次のトピックについては、以下で詳しくで説明します。

- メッセージ API の概要
- 拡張機能のライフ サイクル
- メッセージを作成します。
- メッセージを送信します。

## <a name="messages-api-overview"></a>メッセージ API の概要

ユーザーによって呼び出されると、メッセージ アプリ拡張機能がコンパクトに表示モードでメッセージの履歴の下部に表示されます。

[![](advanced-message-app-extensions-images/interactive03.png "メッセージ API の概要")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController`でメッセージ アプリ拡張機能オブジェクトは、拡張機能の表示は、ユーザーに表示されるときに呼び出されるメイン クラスです。
2. メッセージ交換としてユーザーに表示される、`MSConversation`オブジェクト インスタンス。
3. `MSMessage`クラスは、メッセージ交換で指定されたメッセージ バブルを表します。
4. `MSSession` メッセージを送信する方法を制御します。
5. `MSMessageTemplateLayout` メッセージを表示する方法を制御します。

## <a name="the-extension-lifecycle"></a>拡張機能のライフ サイクル

アクティブになるメッセージ アプリ拡張機能のプロセスを参照してください。

[![](advanced-message-app-extensions-images/interactive04.png "アクティブになるメッセージ アプリ拡張機能のプロセス")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. 拡張機能は、(たとえばアプリ ドロワー) から起動すると、メッセージ アプリは、プロセスを起動します。
2. `DidBecomeActive`メソッドが呼び出され、渡された、`MSConversation`でメッセージ アプリ拡張機能が実行されているメッセージ交換を表します。
3. 拡張機能はオフの基づいているため、`UIViewController`両方`ViewWillAppear`と`ViewDidAppear`と呼ばれます。

次に、非アクティブになるメッセージ アプリ拡張機能のプロセスを見てみましょう。

[![](advanced-message-app-extensions-images/interactive05.png "非アクティブになるメッセージ アプリ拡張機能のプロセス")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. メッセージ アプリ拡張機能が非アクティブ化されるときに、`ViewWillDisappear`メソッドが最初に呼び出されます。
2. 次に、`ViewDidDisappear`メソッドが呼び出されます。
3. `WillResignActive`メソッドが呼び出され、渡された、`MSConversation`でメッセージ アプリ拡張機能が実行されているメッセージ交換を表します。 この時点でメッセージ アプリと拡張機能間の接続が解放されようです。
4. 後の時点でメッセージ アプリによって、プロセスが終了します。

拡張機能が短期的なプロセスであるため、積極的に処理し、バッテリ電源を節約するために、システムによって中止されます。 開発者にならないようにこれを設計およびメッセージ アプリ拡張機能を実装するときに注意してください。

## <a name="composing-a-message"></a>メッセージを作成します。

メッセージ アプリ拡張機能では、プロセスで実行するいるし、そのユーザー インターフェイスが表示されます、新しいメッセージを作成する次のコードを使用できます。

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

このコードを作成する新しい`MSMessage`いくつかのプロパティを設定し、(など`Url`)。 メッセージは、iOS でのみ作成できますに送信できます iOS と macOS の両方に表示します。

ユーザーは、macOS でのメッセージ交換でメッセージ バブルをクリックすると、Mac は、web ブラウザーで URL で指定されたアドレスを開いてしようとします。 その結果、開発者の web サイトは、macOS 上の web ブラウザーで、メッセージの一部の表現ベースのコンピューターを表示できる必要があります。

`AccessibilityLabel`プロパティは、メッセージ交換のトラン スクリプトをユーザーにスクリーン リーダーによって使用されます。 `Layout`プロパティを指定する方法、メッセージが表示されます、現在のみ、`MSMessageTemplateLayout`はサポートされており、次のようになります。

[![](advanced-message-app-extensions-images/interactive06.png "MSMessageTemplateLayout テンプレート")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image`のプロパティ、`MSMessageTemplateLayout`画面 MessageBubble のメインの本文のコンテンツを提供します。 `MediaFileUrl`プロパティはまた、メッセージ バブルの本文のコンテンツを提供しますでサポートされていないコンテンツでは、`UIImage`バック グラウンドでのループは、ビデオ ファイル) など。 どちらの場合、`Image`と`MediaFileUrl`プロパティが指定されて、`Image`プロパティが優先されます。 `MediaFileUrl` PNG、JPEG、GIF、および (Media Player フレームワークによって、再生可能な任意の形式) でビデオをサポートしているメディア形式。

推奨されるメディアのサイズは、3 倍解像度で 300 x 300 ピクセルです。 少し大きくしたり小さく資産も受け付けられ、Apple が最適な結果を取得するいくつかのさまざまなサイズでテストを提案します。 メッセージ アプリをダウン サンプリングを必要に応じてこのメディアをスケールします。

資産が、受信側に送信されると、トランス コードされたメッセージ アプリ、ネットワーク経由で転送からそれらを最適化するために、接続されている任意のメディアが自動的になります。 このため、Apple 抑制メディアがスケール ダウンし、転送するために圧縮するために、メッセージに関連付けられているメディアでのテキストを含むため可能性のあるテキストのレンダリング、判読できます。

`ImageTitle`と`ImageSubtitle`プロパティがメッセージ バブル チャートに表示されるメディアの説明を入力します。 これらのプロパティはテキストとして送信されます受信側のデバイスに場所が明確に表示されます、イメージの左下隅。

`Caption`、 `SubCaption`、`TrailingCaption`と`TrailingSubcaption`プロパティは、さらに、イメージを説明し、イメージの下のセクションに表示されます。 すべてのこれらのプロパティを設定`null`キャプション領域なしメッセージ バブルが作成されます。

[![](advanced-message-app-extensions-images/interactive07.png "キャプション領域なしメッセージ バブル")](advanced-message-app-extensions-images/interactive07.png#lightbox)

最後に注意してくださいには、メッセージ アプリは、メッセージ バブルの左上隅でメッセージ アプリ拡張機能のアイコンを描画しないことです。

## <a name="sending-a-message"></a>メッセージを送信します。

1 回、`MSMessage`で構成される、次のコードは、送信に使用できるされました。

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

呼び出す、`InsertMessage`の`MSConversation`メッセージ交換にメッセージを含めるし、生じる可能性のあるエラーを処理します。 メッセージが正常に含まれる場合は、入力フィールドにメッセージ バブルが表示されます。

さらに、拡張機能など、メッセージ交換に、さまざまな種類のデータを送信できます。

- **テキスト** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **添付ファイル** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **ステッカー**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});`場所`sticker`は、`MSSticker`します。

新しいコンテンツは、入力フィールドには、ユーザーは、青をタップして、メッセージを送信できません**送信**(と同様、一般的なメッセージ) ボタンをクリックします。 このプロセスは、ユーザーの制御下で完全には、コンテンツを自動的に送信するメッセージ アプリ拡張機能のための方法はありません。

## <a name="handling-the-compact-and-expanded-modes"></a>Compact および拡張モードの処理

メッセージ アプリ拡張機能は、2 つの異なる表示モードのいずれかで表示できます。

[![](advanced-message-app-extensions-images/interactive08.png "2 つの異なる表示モードで表示されるメッセージ アプリ拡張:圧縮と展開")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -既定のモードは、このメッセージ アプリ拡張機能が、メッセージ ビューの下部にある 25% を占める場所。 コンパクト モードでアプリにキーボード、水平方向スクロールまたはスワイプ ジェスチャ レコグナイザーへのアクセスはありません。 アプリは入力フィールドへのアクセス権し、するために呼び出す`InsertMessage`はすぐにあるユーザーに表示されます。
- **展開**-メッセージ アプリ拡張機能をメッセージ ビュー全体を入力します。 入力フィールドへのアクセスはありませんが、キーボード、水平方向スクロールおよびスワイプ ジェスチャ レコグナイザーへのアクセスにが。

メッセージ アプリ拡張機能は、切り替えることができますの間でこれらのモードは、プログラム、または手動でユーザーがいつでも、いずれかへの応答性はモードで変更にすぐにする必要があります。

次の 2 つの異なる表示モードの切り替えを処理の例を見てください。 2 つの異なるビュー コント ローラーは、各状態の必要になります。 `StickerBrowserViewController`ハンドル、 **Compact**ビューと`AddStickerViewController`を処理する、 **Expanded**ビュー。

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

必要に応じて、アプリが使用される可能性がありますが、 `WillTransition` (上記の例 Icecream ビルダーで行われる) ようユーザーに提示される前に、表示モードの変更を処理するメソッド。 詳細についてを参照してください、[ステッカーのさらにカスタマイズ](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md)ドキュメント。

## <a name="replying-to-a-message"></a>メッセージに返信します。

メッセージ アプリ拡張機能は、メッセージに返信するときに処理する必要がある 2 つのケースがあります。

[![](advanced-message-app-extensions-images/interactive09.png "非アクティブとアクティブ モードでメッセージ アプリ拡張機能")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **拡張機能は非アクティブ**-ユーザーがタップする拡張機能をアクティブ化し、対話型のメッセージ交換を続行をメッセージのトラン スクリプトでメッセージ アプリ拡張機能のメッセージの吹き出しのいずれかを使用する必要があります。
- **拡張機能が有効になって**-ユーザーが展開済みのビュー モードを入力し、中断した場所から対話型プロセスを続行するメッセージのトラン スクリプトでメッセージ アプリ拡張機能のメッセージのバブルをタップします。

### <a name="the-extension-is-inactive"></a>拡張機能が非アクティブです。

メッセージ バブルがメッセージのトラン スクリプトでユーザーがタップされたメッセージ アプリ拡張機能がアクティブでない場合は、次のプロセスが発生します。

[![](advanced-message-app-extensions-images/interactive10.png "非アクティブのメッセージ バブルの処理")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. ユーザーが、拡張機能のメッセージのバブルをタップします。
2. 拡張機能を起動すると、メッセージ アプリは、プロセスを起動します。
3. `DidBecomeActive`メソッドが呼び出され、渡された、`MSConversation`でメッセージ アプリ拡張機能が実行されているメッセージ交換を表します。
4. 拡張機能はオフの基づいているため、`UIViewController`両方`ViewWillAppear`と`ViewDidAppear`と呼ばれます。

プロセスが完了したら、展開の表示モードでメッセージ アプリ拡張機能が表示されます。

### <a name="the-extension-is-active"></a>拡張機能がアクティブ

メッセージのトラン スクリプトでユーザーがメッセージ バブルをタップすると、メッセージ アプリ拡張機能がアクティブになって、次のプロセスが発生します。

[![](advanced-message-app-extensions-images/interactive11.png "作業中のメッセージ バブルの処理")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. ユーザーが、拡張機能のメッセージのバブルをタップします。
2. メッセージ アプリ拡張機能が既にアクティブであるため、`WillTransition`のメソッド、`MSMessagesAppViewController`はコンパクトなから展開済みの表示モードへの切り替えを処理するために呼び出されます。
3. `DidSelectMessage`のメソッド、`MSMessagesAppViewController`呼び出され、渡された、`MSMessage`と`MSConversation`メッセージ バブルが属しています。
4. `DidTransition`のメソッド、`MSMessagesAppViewController`はコンパクトなから展開済みの表示モードへの切り替えを処理するために呼び出されます。

ここでも、プロセスが完了したら、展開の表示モードでメッセージ アプリ拡張機能が表示されます。

### <a name="accessing-the-selected-message"></a>選択したメッセージへのアクセス

どちらの場合、ユーザーは、メッセージ アプリ拡張機能に属しているメッセージ バブルをタップしたときにする必要がありますへのアクセス、`MSMessage`を使用しているがタップされた、`SelectedMessage`のプロパティ、`MSConversation`します。

例えば:

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

## <a name="removing-partially-completed-messages"></a>メッセージ部分の削除が完了しました

2 つユーザーのメッセージ交換の間の対話型のメッセージ交換のさまざまな手順を送信するには、プロセスでメッセージ トラン スクリプトを使用する部分的に完了したメッセージの吹き出しを開始できます。

[![](advanced-message-app-extensions-images/interactive12.png "部分的に完了したメッセージの吹き出しは、メッセージのトラン スクリプトと雑然とさせることができます。")](advanced-message-app-extensions-images/interactive12.png#lightbox)

代わりに、メッセージ アプリ拡張機能では、メッセージのトラン スクリプトにコメントを簡潔に前のメッセージの吹き出しを縮小する必要があります。

[![](advanced-message-app-extensions-images/interactive13.png "メッセージのトラン スクリプトの前のメッセージの吹き出しの折りたたみ")](advanced-message-app-extensions-images/interactive13.png#lightbox)

使用してこの処理は、`MSSession`折りたたむにはすべての既存の手順。 そのため、`DidSelectMessage`のメソッド、`MSMessagesAppViewController`クラスは、次のように変更できます。

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

終了して、選択したメッセージが既にある場合`MSSession`、それ以外の場合、新しい使用が`MSSession`が作成されます。 `SummaryText`のプロパティ、`MSMessage`折りたたまれている前の手順にキャプションを追加するために使用します。 場合、`SummaryText`プロパティに設定されて`null`、メッセージ交換では、前の手順はメッセージ交換のトラン スクリプトから完全に削除されます。

## <a name="advanced-message-api-features"></a>メッセージの API 機能の詳細

上記の詳細について、新しいメッセージ API の基本的な機能、Apple のフレームワークが組み込まれた高度な機能のいくつかのことを確認次に。

最初に、その他のいくつかのオーバーライド メソッドでは、`MSMessagesAppViewController`メッセージ交換に深くアクセスを提供するクラス。

- `DidStartSendingMessage` -これは、ユーザーが [送信] ボタンをタップしたときに呼び出されます。 これは、メッセージの送信プロセスが開始されているだけで、受信者に配信が実際にされているという意味ではありません。
- `DidCancelSendingMessage` -このエラーは、ユーザーがタップした場合、 *X*トラン スクリプトのメッセージ交換のメッセージ バブルの右上隅のボタンをクリックします。
- `DidReceiveMessage` 、メッセージ アプリ拡張機能がアクティブなときにこのメソッドが呼び出されますが、メッセージ交換の参加者のいずれかから新しいメッセージを受け取りました。

### <a name="group-conversations"></a>グループのメッセージ交換

ユーザーが (3 つ以上の個人) とグループのメッセージ交換に関係するし、これを設計およびメッセージ アプリ拡張機能を実装するときに考慮する必要があります、メッセージ アプリ拡張機能を使用できます。

3 人のユーザーとグループのメッセージ交換で次の操作を参照してください。

[![](advanced-message-app-extensions-images/interactive14.png "3 人のユーザーとグループのメッセージ交換内での対話")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. ユーザー 1 はグループ対話型メッセージを送信ハンバーガー トッピングを選択するには、ユーザーの 2 および 3 のユーザーを確認します。
2. ユーザー 2 は、トマトを選択します。
3. 3 にぎやかを選択します。
4. ユーザー 2 の両方のユーザー 3 の選択肢は、ほぼ同時に、ユーザー 1 に到着します。 その結果、ユーザー 2 の選択肢は、概要の行に折りたたまれます、ご利用いただけません。 このケースも反転されている可能性があります、表示されていると、ユーザーの 3 を折りたたみ中のユーザー 2 の選択肢です。

いずれの場合も、この動作はありません必要なユーザー 1 は 2 のユーザーとユーザー 3 の選択肢にアクセスできる必要があります。 このような状況を処理するために Apple が提案されていますメッセージ アプリ拡張機能は、クラウド内のメッセージの状態を格納しの URL プロパティを使用して、 `MSMessage` (を取得、ユーザーの間で送信される) この状態にアクセスします。

ユーザーは、メッセージを送信するときにセッション トークンが生成され、現在のメッセージの状態を使用してクラウドにプッシュされます。 トラン スクリプトのメッセージ交換のメッセージ バブルをタップする、ときに、セッション トークンは、クラウドからの現在のセッション状態の取得に使用されます。

### <a name="sender-identifiers"></a>送信者識別子

メッセージの送信者の識別子へのアクセスについては、グループのメッセージ交換が上記の例を実行します。

[![](advanced-message-app-extensions-images/interactive15.png "グループのメッセージ交換の識別子を送信します。")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. ユーザー 1 がグループ対話型メッセージを送信する、もう一度ハンバーガー トッピングを選択するには、ユーザーの 2 および 3 のユーザーを確認します。
2. ユーザー 3 にぎやかを取得します。
3. ユーザー 3 の選択肢がユーザーの 1 に受信され、ユーザー 2 がまだ返信できません。
4. Apple は、ユーザーのプライバシーと非常に心配であるためメッセージ アプリ拡張機能のみを知って、一意識別子 (として、 `NSUUID`)、メッセージ交換の参加者が割り当てられています。 ローカルのデバイスでは、現在のユーザーの識別子だけ呼ばれます。
5. `MSMessage`が、`SenderIdentifier`ユーザーのいずれかに一致するプロパティの参加要素の一覧で既知の拡張機能にします。
6. 各ユーザー デバイスには、ここでも、ローカル ユーザーの識別子のみがわかっている参加要素のリストのコピーがあります。
7. メッセージが送信されると、その`SenderIdentifier`プロパティは、ローカル ユーザーのも呼ばれます。

送信者の識別子は、次の方法で使用できます。

- 参加要素のリストを確認、拡張機能は、メッセージ交換でユーザーの数を取得できます。
- 拡張機能は、ユーザーからのメッセージを受信したときの追跡できる送信者の識別子。 同じ送信者の識別子を持つ別のメッセージを受信した場合、拡張機能は同じユーザーからあるを認識します。
- メッセージ交換内の特定のユーザーを識別できるように、使用できます。

テキスト フィールドのいずれかで送信者の識別子を使用できる、`MSMessageTemplateLayout`によってドル記号を付けます (`$`)。 例:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

メッセージ アプリでは、この種の書式でメッセージ バブルが表示されたら、置き換えられます、`$uuid...`メッセージを送信したメッセージ交換では参加者の連絡先の名前に置き換えます。

送信者識別子は、異なる一意な送信者の識別子をメッセージ交換内の各参加者があるそのユーザー 1 のデバイスとユーザー 3 のデバイスにもう一度、注上の図を見ているので、各デバイス上で一意で。

メッセージ アプリ拡張機能のインストール、送信者の識別子のスコープです。 したがってユーザーをアンインストールし、メッセージ アプリ拡張機能を再インストールする場合、新しいインストールの新しい送信者の識別子が生成されます。

送信者の識別子にアクセスするには、拡張機能は、次のコードを使用できます。

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

メッセージ アプリ拡張機能によって生成された対話型メッセージは、次の Apple プラットフォームに提供されます。

- watchOS 3
- macOS Sierra
- iOS 10

3 つのプラットフォームでは、iOS 10 だけは対話型メッセージを生成するユーザーを許可します。 Macos Sierra で対話型メッセージ バブル チャートの場合、ユーザーがクリックした場合、URL に接続されている、 `MSMessage` Safari では表示され、メッセージの表現が表示されます。

WatchOS では、メッセージ アプリのことをハンドオフ対話型メッセージを接続されている iOS デバイスをユーザーが応答を作成できます。

新しいメッセージ API は古い Apple プラットフォームで対話型メッセージを受信した場合のフォールバックをサポートしています。

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

2 つの個別のメッセージとしてフォールバック形式で転送されます。

- イメージは、テンプレートのレイアウトによって提供されるいずれかになります。
- 指定された、URL、もう一方になります、`MSMessage`します。

## <a name="summary"></a>まとめ

この記事では、統合する Xamarin.iOS ソリューションでメッセージ アプリ拡張機能を使用するための高度なテクニックを示しましたが、**メッセージ**アプリとユーザーに新しい機能が存在します。


## <a name="related-links"></a>関連リンク

- [Ice cream ビルダー (サンプル)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [メッセージの参照](https://developer.apple.com/reference/messages)
- [アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
