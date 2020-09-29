---
title: Xamarin. iOS の高度なメッセージアプリ拡張機能
description: この記事では、Xamarin. iOS ソリューションでメッセージアプリ拡張機能を使用するための高度な手法について説明します。このソリューションは、メッセージアプリと統合され、ユーザーに新しい機能を提供します。
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 8a1f386209ccc1f2cb33348930f29bf5ac65ce4f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435620"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Xamarin. iOS の高度なメッセージアプリ拡張機能

_この記事では、Xamarin. iOS ソリューションでメッセージアプリ拡張機能を使用するための高度な手法について説明します。このソリューションは、メッセージアプリと統合され、ユーザーに新しい機能を提供します。_

IOS 10 を初めて使用する場合、メッセージアプリ拡張機能は **Messages** アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージアプリの拡張機能について

前述のように、メッセージアプリ拡張機能は **Messages** アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。 次の2種類のメッセージアプリ拡張機能を使用できます。

- **ステッカーパック** -ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカーパックは、コードを記述せずに作成できます。
- **IMessage アプリ** -メッセージアプリ内にカスタムユーザーインターフェイスを提供して、ステッカーの選択、テキストの入力 (オプションの型変換を含む)、および操作メッセージの作成、編集、および送信を行うことができます。

メッセージアプリの拡張機能は、次の3つの主要なコンテンツの種類を提供します。

- **対話型メッセージ** -アプリが生成するカスタムメッセージコンテンツの一種であり、ユーザーがメッセージをタップすると、アプリがフォアグラウンドで起動されます。
- **ステッカー** -ユーザー間で送信されるメッセージに含めることができる、アプリによって生成されるイメージです。 ステッカーパックアプリの実装例については、 [アイスクリームビルダー](/samples/xamarin/ios-samples/ios10-icecreambuilder) のサンプルアプリを参照してください。
- **サポートされているその他のコンテンツ** -アプリは、メッセージアプリによって常にサポートされている種類の写真、ビデオ、テキスト、リンクなどのコンテンツを提供できます。

IOS 10 の新機能であるメッセージアプリには、独自の専用の組み込みアプリストアが含まれるようになりました。 メッセージアプリの拡張機能を含むすべてのアプリが、このストアに表示され、昇格されます。 新しいメッセージアプリドロワーには、Messages App Store からダウンロードされたすべてのアプリが表示され、ユーザーにすばやくアクセスできるようになります。

また、iOS 10 の新機能である Apple では、ユーザーがアプリを簡単に検出できるようにするインラインアプリの属性が追加されています。 たとえば、1人のユーザーが、2番目のユーザーがインストールしていないアプリから別のユーザーにコンテンツを送信した場合 (ステッカーなど)、送信元アプリの名前はメッセージ履歴のコンテンツの下に一覧表示されます。 ユーザーがアプリ名をタップすると、メッセージアプリストアが開き、ストアでアプリが選択されます。

メッセージアプリの拡張機能は、開発者が作成に慣れている既存の iOS アプリに似ており、標準の iOS アプリのすべての標準フレームワークと機能にアクセスできます。 次に例を示します。

- アプリ内購入にアクセスできます。
- Apple Pay にアクセスできます。
- これらのユーザーは、カメラなどのデバイスハードウェアにアクセスできます。

メッセージアプリの拡張機能は、iOS 10 でのみサポートされていますが、これらの拡張機能が送信するコンテンツは、watchOS および macOS デバイスで表示できます。 WatchOS 3 に追加された新しい [ _Recents ページ_ には、電話から送信された最近のステッカー (メッセージアプリの拡張機能を含む) が表示され、ユーザーはこれらのステッカーをウォッチから送信できます。

## <a name="about-interactive-messages"></a>対話型メッセージについて

対話型メッセージはカスタムメッセージバブルを提示し、メッセージアプリの拡張機能によって提供されます。 ユーザーは、対話型のメッセージコンテンツを作成し、メッセージ入力フィールドに挿入して送信することができます。

[![対話型メッセージのコンテンツの作成](advanced-message-app-extensions-images/interactive01.png)](advanced-message-app-extensions-images/interactive01.png#lightbox)

受信側のユーザーは、メッセージ履歴内のメッセージバブルをタップして、それを作成したメッセージアプリ拡張機能を読み込むことで、対話型メッセージに応答できます。 拡張機能が全画面表示され、ユーザーは応答を作成して、送信元のユーザーに返信することができます。

[![拡張機能が全画面表示を開始しました。](advanced-message-app-extensions-images/interactive02.png)](advanced-message-app-extensions-images/interactive02.png#lightbox)

次のトピックについては、以下で詳しく説明します。

- Messages API の概要
- 拡張機能のライフサイクル
- メッセージの作成
- メッセージの送信

## <a name="messages-api-overview"></a>Messages API の概要

ユーザーによって呼び出されると、メッセージの履歴の下部に、簡易表示モードでメッセージアプリの拡張機能が表示されます。

[![Messages API の概要](advanced-message-app-extensions-images/interactive03.png)](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController`メッセージアプリ拡張機能のオブジェクトは、拡張機能のビューがユーザーに表示されるときに呼び出されるメインクラスです。
2. メッセージ交換は、オブジェクトインスタンスとしてユーザーに提示され `MSConversation` ます。
3. クラスは、 `MSMessage` メッセージ交換の特定のメッセージバブルを表します。
4. `MSSession` メッセージの送信方法を制御します。
5. `MSMessageTemplateLayout` メッセージの表示方法を制御します

## <a name="the-extension-lifecycle"></a>拡張機能のライフサイクル

メッセージアプリ拡張機能がアクティブになったプロセスを見てみましょう。

[![メッセージアプリ拡張機能がアクティブになったプロセス](advanced-message-app-extensions-images/interactive04.png)](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. 拡張機能が起動されると (たとえば、アプリドロアーから)、メッセージアプリによってプロセスが起動されます。
2. `DidBecomeActive`メソッドが呼び出され、 `MSConversation` メッセージアプリ拡張機能が実行されているメッセージ交換を表すが渡されます。
3. 拡張機能はとの両方に基づいているので、 `UIViewController` `ViewWillAppear` `ViewDidAppear` が呼び出されます。

次に、メッセージアプリの拡張機能が非アクティブ化されるプロセスについて見ていきます。

[![メッセージアプリ拡張機能が非アクティブ化されるプロセス](advanced-message-app-extensions-images/interactive05.png)](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. メッセージアプリ拡張機能が非アクティブ化されると、 `ViewWillDisappear` 最初にメソッドが呼び出されます。
2. その後、 `ViewDidDisappear` メソッドが呼び出されます。
3. `WillResignActive`メソッドが呼び出され、 `MSConversation` メッセージアプリ拡張機能が実行されているメッセージ交換を表すが渡されます。 この時点で、メッセージアプリと拡張機能の間の接続が解放されます。
4. 後で、プロセスは Messages アプリによって終了されます。

拡張機能は短時間のプロセスであるため、処理とバッテリの電力を節約するためにシステムによって積極的に終了されます。 開発者は、メッセージアプリ拡張機能を設計および実装するときに、この点に留意する必要があります。

## <a name="composing-a-message"></a>メッセージの作成

メッセージアプリ拡張機能がプロセスで実行され、そのユーザーインターフェイスが表示されたら、次のコードを使用して新しいメッセージを作成できます。

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

このコードは、新しいを作成 `MSMessage` し、いくつかのプロパティ (など) を設定し `Url` ます。 メッセージは iOS 上でのみ作成できますが、iOS と macOS の両方に送信して表示することができます。

ユーザーが macOS でメッセージ交換のメッセージバブルをクリックすると、Mac は web ブラウザーの URL で指定されたアドレスを開こうとします。 その結果、開発者の web サイトでは、macOS ベースのコンピューターの web ブラウザーにメッセージの一部を表示できるようになります。

プロパティは、 `AccessibilityLabel` ユーザーに対するメッセージ交換のトランスクリプトを読み取るためにスクリーンリーダーによって使用されます。 プロパティは、 `Layout` メッセージの表示方法を指定します。現在のは、のみがサポートされ、 `MSMessageTemplateLayout` 次のようになります。

[![MSMessageTemplateLayout テンプレート](advanced-message-app-extensions-images/interactive06.png)](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image`のプロパティは、 `MSMessageTemplateLayout` 画面上の messagebubble の本文のコンテンツを提供します。 `MediaFileUrl`また、プロパティはメッセージバブルの本文にコンテンツを提供しますが、ではサポートされていないコンテンツ `UIImage` (バックグラウンドでループするビデオファイルなど) を使用できます。 プロパティとプロパティの両方が指定されている場合は、 `Image` `MediaFileUrl` プロパティが `Image` 優先されます。 では、 `MediaFileUrl` PNG、JPEG、GIF、およびビデオ (Media Player framework で再生できる任意の形式) がサポートされています。

推奨されるメディアサイズは、3倍の解像度で 300 x 300 ピクセルです。 小さいアセットと小さいアセットも使用できます。 Apple は、最適な結果を得るために、いくつかの異なるサイズのテストを提案します。 メッセージアプリは、必要に応じてこのメディアをダウンサンプリングし、スケーリングします。

アセットが受信者に送信されると、接続されているメディアは、ネットワーク上での転送を最適化するために、メッセージアプリによって自動的にトランスコードされます。 このため、Apple では、メッセージに添付されているメディア内のテキストを含めないようにしています。これは、メディアがスケールダウンされ、転送用に圧縮されるため、テキストが判読不能になる可能性があるためです。

`ImageTitle`プロパティと `ImageSubtitle` プロパティは、メッセージバブルに表示されるメディアの説明を提供します。 これらのプロパティは、受信デバイスにテキストとして送信され、イメージの左下隅に crisply にレンダリングされます。

、、およびの各プロパティでは、 `Caption` `SubCaption` `TrailingCaption` `TrailingSubcaption` イメージについてさらに説明します。イメージの下のセクションにレンダリングされます。 これらのプロパティをすべてに設定する `null` と、キャプション領域なしでメッセージバブルが作成されます。

[![キャプション領域のないメッセージバブル](advanced-message-app-extensions-images/interactive07.png)](advanced-message-app-extensions-images/interactive07.png#lightbox)

最後に注意すべき点は、メッセージアプリはメッセージバブルの左上隅にメッセージアプリ拡張機能のアイコンを描画することです。

## <a name="sending-a-message"></a>メッセージの送信

`MSMessage`が構成されたら、次のコードを使用してそれを送信できます。

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

`ActiveConversation`のプロパティは、 `MSMessagesAppViewController` メッセージアプリ拡張機能が起動された現在のメッセージ交換を保持します。

のを呼び出して `InsertMessage` `MSConversation` メッセージ交換にメッセージを含め、発生する可能性のあるエラーを処理します。 メッセージが正常に含まれている場合は、入力フィールドにメッセージのバブルが表示されます。

さらに、この拡張機能は、次のようなさまざまな種類のデータをメッセージ交換に送信できます。

- **本文** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **対する** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Stickers**  -  ステッカー `ActiveConversation.InsertSticker (sticker, (obj) => {...});`ここで `sticker` 、は `MSSticker` です。

入力フィールドに新しいコンテンツが追加されると、ユーザーは、(通常のメッセージと同様に) 青い [ **送信** ] ボタンをタップしてメッセージを送信できます。 メッセージアプリ拡張機能でコンテンツを自動的に送信する方法はありません。このプロセスは、ユーザーの制御下にあります。

## <a name="handling-the-compact-and-expanded-modes"></a>コンパクトモードと拡張モードの処理

メッセージアプリの拡張機能は、次の2つの異なる表示モードのいずれかで表示できます。

[![2つの異なる表示モードで表示されるメッセージアプリの拡張機能: コンパクト & 展開](advanced-message-app-extensions-images/interactive08.png)](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -これは、メッセージアプリの拡張機能がメッセージビューの下位25% を占有する既定のモードです。 コンパクトモードでは、アプリはキーボード、水平スクロール、またはスワイプジェスチャのレコグナイザーにアクセスできません。 アプリには入力フィールドへのアクセス権があり、の呼び出し `InsertMessage` はすぐにユーザーに表示されます。
- **展開** 済み-メッセージアプリの拡張機能によってメッセージビュー全体が表示されます。 入力フィールドへのアクセス権はありませんが、キーボード、水平スクロール、およびスワイプジェスチャのレコグナイザーにアクセスできます。

メッセージアプリの拡張機能は、プログラムによって、またはユーザーが手動でいつでも手動で切り替えることができ、表示モードの変更にすぐに応答する必要があります。

2つの異なる表示モードの切り替えを処理する次の例を見てみましょう。 状態ごとに2つの異なるビューコントローラーが必要になります。 は `StickerBrowserViewController` **コンパクト** ビューを処理し、は `AddStickerViewController` **展開** されたビューを処理します。

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

必要に応じて、アプリはメソッドを使用して、 `WillTransition` ユーザーに表示される前に、ビューモードの変更を処理することもできます (上記の Icecream Builder の例のように)。 詳細については、 [ステッカーのカスタマイズ](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) に関するドキュメントを参照してください。

## <a name="replying-to-a-message"></a>メッセージへの返信

メッセージに応答するときに、メッセージアプリの拡張機能が処理する必要があるケースが2つあります。

[![非アクティブモードとアクティブモードのメッセージアプリ拡張機能](advanced-message-app-extensions-images/interactive09.png)](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **拡張機能が非アクティブ** になっています。ユーザーがタップして拡張機能をアクティブ化し、対話形式で会話を続けることができるメッセージのトランスクリプトに、メッセージアプリ拡張機能のメッセージバブルの1つがあります。
- **拡張機能がアクティブになってい** ます。ユーザーは、メッセージトランスクリプトで Message App Extension のメッセージバブルをタップして展開されたビューモードに入り、中断した場所から対話型プロセスを続行できます。

### <a name="the-extension-is-inactive"></a>拡張機能がアクティブではありません

メッセージトランスクリプトでユーザーによってメッセージのバブルがタップされ、メッセージアプリの拡張機能が非アクティブになると、次の処理が行われます。

[![非アクティブなメッセージバブルの処理](advanced-message-app-extensions-images/interactive10.png)](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. ユーザーが拡張機能のメッセージバブルをタップします。
2. 拡張機能が起動されると、メッセージアプリによってプロセスが起動されます。
3. `DidBecomeActive`メソッドが呼び出され、 `MSConversation` メッセージアプリ拡張機能が実行されているメッセージ交換を表すが渡されます。
4. 拡張機能はとの両方に基づいているので、 `UIViewController` `ViewWillAppear` `ViewDidAppear` が呼び出されます。

プロセスが完了すると、メッセージアプリの拡張機能が展開ビューモードで表示されます。

### <a name="the-extension-is-active"></a>拡張機能はアクティブです

メッセージトランスクリプトでユーザーによってメッセージバブルがタップされ、メッセージアプリ拡張機能がアクティブになっている場合、次の処理が行われます。

[![アクティブメッセージバブルの処理](advanced-message-app-extensions-images/interactive11.png)](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. ユーザーが拡張機能のメッセージバブルをタップします。
2. メッセージアプリ拡張機能は既にアクティブになっているため、 `WillTransition` `MSMessagesAppViewController` Compact から展開されたビューモードへの切り替えを処理するために、のメソッドが呼び出されます。
3. `DidSelectMessage`のメソッド `MSMessagesAppViewController` が呼び出され、 `MSMessage` メッセージバブルが属しているとが渡され `MSConversation` ます。
4. `DidTransition`のメソッドは、 `MSMessagesAppViewController` Compact から展開されたビューモードへの切り替えを処理するために呼び出されます。

この場合も、プロセスが完了すると、メッセージアプリの拡張機能が展開ビューモードで表示されます。

### <a name="accessing-the-selected-message"></a>選択したメッセージへのアクセス

どちらの場合も、ユーザーがメッセージアプリ拡張機能に属するメッセージバブルをタップすると、の `MSMessage` プロパティを使用してタップされたにアクセスできる必要があり `SelectedMessage` `MSConversation` ます。

次に例を示します。

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

選択したメッセージがメッセージアプリ拡張機能の UI に表示され、ユーザーは応答の作成を許可されている必要があります。

## <a name="removing-partially-completed-messages"></a>部分的に完了したメッセージの削除

メッセージ交換の2人のユーザーの間で対話型会話のさまざまなステップを送信するプロセスでは、部分的に完了したメッセージのバブルが、メッセージトランスクリプトの乱雑さを開始する可能性があります。

[![部分的に完了したメッセージのバブルは、メッセージトランスクリプトを混乱させる可能性があります](advanced-message-app-extensions-images/interactive12.png)](advanced-message-app-extensions-images/interactive12.png#lightbox)

代わりに、メッセージのトランスクリプトで前のメッセージのバブルを簡潔なコメントに折りたたむ必要があります。

[![メッセージトランスクリプトで前のメッセージバブルを折りたたむ](advanced-message-app-extensions-images/interactive13.png)](advanced-message-app-extensions-images/interactive13.png#lightbox)

これは、 `MSSession` 既存のすべてのステップを折りたたむためにを使用して処理されます。 `DidSelectMessage`クラスのメソッドは、 `MSMessagesAppViewController` 次のように変更できます。

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

選択したメッセージに既に終了しているがある場合は、それが使用され `MSSession` ます。それ以外の場合は、新しい `MSSession` が作成されます。 `SummaryText`のプロパティは、 `MSMessage` 折りたたまれた前の手順にキャプションを追加するために使用されます。 `SummaryText`プロパティがに設定されている場合 `null` 、メッセージ交換の前の手順は、メッセージ交換のトランスクリプトから完全に削除されます。

## <a name="advanced-message-api-features"></a>高度なメッセージ API 機能

上記の新しいメッセージ API の基本的な機能については、次に、Apple がフレームワークに組み込まれている高度な機能のいくつかを確認します。

まず、クラスには、 `MSMessagesAppViewController` メッセージ交換により深いアクセスを提供するオーバーライドメソッドがいくつかあります。

- `DidStartSendingMessage` -これは、ユーザーが [送信] ボタンをタップしたときに呼び出されます。 これは、送信プロセスが開始されただけで、メッセージが実際に受信者に配信されたことを意味するわけではありません。
- `DidCancelSendingMessage` -これは、ユーザーがメッセージ交換のトランスクリプトのメッセージバブルの右上隅にある [ *X* ] ボタンをタップしたときに発生します。
- `DidReceiveMessage` -このメソッドは、メッセージアプリ拡張機能がアクティブになっているときに呼び出され、メッセージ交換のいずれかの参加要素から新しいメッセージを受信しました。

### <a name="group-conversations"></a>メッセージ交換のグループ化

メッセージアプリ拡張機能は、ユーザーがグループの会話 (3 人以上) に関与している間に使用できます。これは、メッセージアプリ拡張機能を設計および実装するときに考慮する必要があります。

3人のユーザーとのグループメッセージ交換で、次の対話を見てみましょう。

[![グループと3人のユーザーとの対話](advanced-message-app-extensions-images/interactive14.png)](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. ユーザー1は、ユーザー2とユーザー3にハンバーガートッピングを選択するように求めるグループ対話型のメッセージを送信します。
2. ユーザー2はトマトを選択します。
3. ユーザー3が選択を選択します。
4. ユーザー2とユーザー3の両方の選択肢は、ほぼ同時にユーザー1に返されます。 その結果、ユーザー2の選択は概要行に折りたたまれ、使用できなくなります。 このケースは、ユーザー2が表示され、ユーザー3が折りたたまれている場合にも、反転されている可能性があります。

どちらの場合も、ユーザー1はユーザー2とユーザー3の両方の選択肢にアクセスできる必要があるため、この動作は望ましくありません。 この状況に対処するため、Apple では、メッセージアプリ拡張機能がメッセージの状態をクラウドに格納し、 `MSMessage` (ユーザー間で送信される) の URL プロパティを使用して、この状態にアクセスすることを提案しています。

ユーザーがメッセージを送信すると、セッショントークンが生成され、現在のメッセージ状態でクラウドにプッシュされます。 ユーザーがメッセージ交換のトランスクリプトでメッセージのバブルをタップすると、セッショントークンが使用され、現在のセッション状態がクラウドから取得されます。

### <a name="sender-identifiers"></a>送信者の識別子

メッセージの送信者の識別子にアクセスする方法については、上記のグループメッセージ交換の例を次に示します。

[![グループメッセージ交換識別子の送信](advanced-message-app-extensions-images/interactive15.png)](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. ここでも、ユーザー1は、ユーザー2とユーザー3にハンバーガートッピングを選択するように求めるグループ対話型メッセージを送信します。
2. ユーザー3が選択します。
3. ユーザー3の選択がユーザー1に返され、ユーザー2はまだ返信していません。
4. Apple はユーザーのプライバシーに関係しているため、メッセージアプリの拡張機能は、メッセージ交換の各参加者に割り当てられた一意の識別子 () のみを認識し `NSUUID` ます。 ローカルデバイスでは、現在のユーザーの識別子のみが認識されます。
5. に `MSMessage` は、 `SenderIdentifier` 拡張機能で認識される参加者の一覧にあるユーザーの1つに一致するプロパティがあります。
6. 各ユーザーデバイスには、参加者リストの独自のコピーがあります。ここでも、ローカルユーザーの識別子のみが認識されます。
7. メッセージが送信されると、その `SenderIdentifier` プロパティは、ローカルユーザーのプロパティでもあることがわかります。

送信者の識別子は、次の方法で使用できます。

- 参加者の一覧を見ると、拡張機能によって、メッセージ交換のユーザー数を取得できます。
- 拡張機能は、ユーザーからメッセージを受信すると、送信者の識別子を追跡できます。 同じ送信者識別子を持つ別のメッセージを受信した場合、拡張機能は同じユーザーのものであることを認識します。
- これらは、メッセージ交換の特定のユーザーを識別するために使用できます。

送信者の識別子は、 `MSMessageTemplateLayout` ドル記号 () を付けることによって、のテキストフィールドのいずれかで使用でき `$` ます。 次に例を示します。

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

メッセージアプリにこの種類の書式設定のメッセージバブルが表示されると、は、 `$uuid...` メッセージを送信したメッセージ交換の参加者の連絡先名に置き換えられます。

送信者の識別子は各デバイスで一意であるため、上の図を再び見ると、ユーザー1のデバイスとユーザー3のデバイスは、メッセージ交換の参加者ごとに一意の送信者識別子を持っていることに注意してください。

送信者の識別子のスコープは、メッセージアプリ拡張機能のインストールに限定されます。 そのため、ユーザーがメッセージアプリ拡張機能をアンインストールして再インストールすると、新しいインストールに対して新しい送信者識別子が生成されます。

送信者の識別子にアクセスするために、拡張機能では次のコードを使用できます。

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

メッセージアプリ拡張機能によって生成される対話型メッセージは、次の Apple プラットフォームで配信されます。

- watchOS 3
- macOS Sierra
- iOS 10

3つのプラットフォームのうち、ユーザーが対話型メッセージを生成することを許可するのは、iOS 10 だけです。 MacOS Sierra では、ユーザーが対話型メッセージのバブルをクリックすると、に接続されている URL が `MSMessage` Safari で開かれ、メッセージの表現が表示されます。

WatchOS では、メッセージアプリは、ユーザーが応答を作成できる接続された iOS デバイスに対話型メッセージをハンドオフできます。

古い Apple プラットフォームで対話型メッセージを受信した場合、新しい Messages API はフォールバックをサポートしています。

- watchOS 2 +
- OS X 10.11 +
- iOS 9 以降

これらは、2つの異なるメッセージとしてフォールバック形式で配信されます。

- 1つは、テンプレートレイアウトによって提供されるイメージです。
- もう1つは、に用意されている URL `MSMessage` です。

## <a name="summary"></a>まとめ

この記事では、Xamarin. iOS ソリューションでメッセージアプリ拡張機能を使用するための高度な手法について説明しました。このソリューションは、 **メッセージ** アプリと統合され、ユーザーに新しい機能を提供します。

## <a name="related-links"></a>関連リンク

- [アイスクリームビルダー (サンプル)](/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [メッセージリファレンス](https://developer.apple.com/reference/messages)
- [アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)