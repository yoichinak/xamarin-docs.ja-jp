---
title: Xamarin. iOS の高度なメッセージアプリ拡張機能
description: この記事では、Xamarin. iOS ソリューションでメッセージアプリ拡張機能を使用するための高度な手法について説明します。このソリューションは、メッセージアプリと統合され、ユーザーに新しい機能を提供します。
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: fb721e36a6b66b90e9660a1c7d5db9e5124e8715
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031731"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Xamarin. iOS の高度なメッセージアプリ拡張機能

_この記事では、Xamarin. iOS ソリューションでメッセージアプリ拡張機能を使用するための高度な手法について説明します。このソリューションは、メッセージアプリと統合され、ユーザーに新しい機能を提供します。_

IOS 10 を初めて使用する場合、メッセージアプリ拡張機能は**Messages**アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。

## <a name="about-message-app-extensions"></a>メッセージアプリの拡張機能について

前述のように、メッセージアプリ拡張機能は**Messages**アプリと統合され、ユーザーに新しい機能を提供します。 拡張機能は、テキスト、ステッカー、メディアファイル、および対話型メッセージを送信できます。 次の2種類のメッセージアプリ拡張機能を使用できます。

- **ステッカーパック**-ユーザーがメッセージに追加できるステッカーのコレクションが含まれています。 ステッカーパックは、コードを記述せずに作成できます。
- **IMessage アプリ**-メッセージアプリ内にカスタムユーザーインターフェイスを提供して、ステッカーの選択、テキストの入力 (オプションの型変換を含む)、および操作メッセージの作成、編集、および送信を行うことができます。

メッセージアプリの拡張機能は、次の3つの主要なコンテンツの種類を提供します。

- **対話型メッセージ**-アプリが生成するカスタムメッセージコンテンツの一種であり、ユーザーがメッセージをタップすると、アプリがフォアグラウンドで起動されます。
- **ステッカー** -ユーザー間で送信されるメッセージに含めることができる、アプリによって生成されるイメージです。 ステッカーパックアプリの実装例については、[アイスクリームビルダー](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)のサンプルアプリを参照してください。
- **サポートされているその他のコンテンツ**-アプリは、メッセージアプリによって常にサポートされている種類の写真、ビデオ、テキスト、リンクなどのコンテンツを提供できます。

IOS 10 の新機能であるメッセージアプリには、独自の専用の組み込みアプリストアが含まれるようになりました。 メッセージアプリの拡張機能を含むすべてのアプリが、このストアに表示され、昇格されます。 新しいメッセージアプリドロワーには、Messages App Store からダウンロードされたすべてのアプリが表示され、ユーザーにすばやくアクセスできるようになります。

また、iOS 10 の新機能である Apple では、ユーザーがアプリを簡単に検出できるようにするインラインアプリの属性が追加されています。 たとえば、1人のユーザーが、2番目のユーザーがインストールしていないアプリから別のユーザーにコンテンツを送信した場合 (ステッカーなど)、送信元アプリの名前はメッセージ履歴のコンテンツの下に一覧表示されます。 ユーザーがアプリ名をタップすると、メッセージアプリストアが開き、ストアでアプリが選択されます。

メッセージアプリの拡張機能は、開発者が作成に慣れている既存の iOS アプリに似ており、標準の iOS アプリのすべての標準フレームワークと機能にアクセスできます。 (例:

- アプリ内購入にアクセスできます。
- Apple Pay にアクセスできます。
- これらのユーザーは、カメラなどのデバイスハードウェアにアクセスできます。

メッセージアプリの拡張機能は、iOS 10 でのみサポートされていますが、これらの拡張機能が送信するコンテンツは、watchOS および macOS デバイスで表示できます。 WatchOS 3 に追加された新しい  _Recents ページ_には、電話から送信された最近のステッカー (メッセージアプリの拡張機能を含む) が表示され、ユーザーはこれらのステッカーをウォッチから送信できます。

## <a name="about-interactive-messages"></a>対話型メッセージについて

対話型メッセージはカスタムメッセージバブルを提示し、メッセージアプリの拡張機能によって提供されます。 ユーザーは、対話型のメッセージコンテンツを作成し、メッセージ入力フィールドに挿入して送信することができます。

[![](advanced-message-app-extensions-images/interactive01.png "Creating Interactive Message Content")](advanced-message-app-extensions-images/interactive01.png#lightbox)

受信側のユーザーは、メッセージ履歴内のメッセージバブルをタップして、それを作成したメッセージアプリ拡張機能を読み込むことで、対話型メッセージに応答できます。 拡張機能が全画面表示され、ユーザーは応答を作成して、送信元のユーザーに返信することができます。

[![](advanced-message-app-extensions-images/interactive02.png "The Extension launched full-screen")](advanced-message-app-extensions-images/interactive02.png#lightbox)

次のトピックについては、以下で詳しく説明します。

- Messages API の概要
- 拡張機能のライフサイクル
- メッセージの作成
- メッセージの送信

## <a name="messages-api-overview"></a>Messages API の概要

ユーザーによって呼び出されると、メッセージの履歴の下部に、簡易表示モードでメッセージアプリの拡張機能が表示されます。

[![](advanced-message-app-extensions-images/interactive03.png "Messages API Overview")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. メッセージアプリ拡張機能の `MSMessageAppViewController` オブジェクトは、拡張機能のビューがユーザーに表示されるときに呼び出されるメインクラスです。
2. メッセージ交換は、`MSConversation` オブジェクトインスタンスとしてユーザーに提示されます。
3. `MSMessage` クラスは、メッセージ交換の特定のメッセージバブルを表します。
4. `MSSession` は、メッセージの送信方法を制御します。
5. `MSMessageTemplateLayout` メッセージの表示方法を制御します。

## <a name="the-extension-lifecycle"></a>拡張機能のライフサイクル

メッセージアプリ拡張機能がアクティブになったプロセスを見てみましょう。

[![](advanced-message-app-extensions-images/interactive04.png "The process of a Message App Extension becoming active")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. 拡張機能が起動されると (たとえば、アプリドロアーから)、メッセージアプリによってプロセスが起動されます。
2. `DidBecomeActive` メソッドが呼び出され、メッセージアプリ拡張機能が実行されているメッセージ交換を表す `MSConversation` が渡されます。
3. 拡張機能は `UIViewController` に基づいているため、`ViewWillAppear` と `ViewDidAppear` の両方が呼び出されます。

次に、メッセージアプリの拡張機能が非アクティブ化されるプロセスについて見ていきます。

[![](advanced-message-app-extensions-images/interactive05.png "The process of a Message App Extension becoming deactivated")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. メッセージアプリ拡張機能が非アクティブ化されると、最初に `ViewWillDisappear` メソッドが呼び出されます。
2. その後、`ViewDidDisappear` メソッドが呼び出されます。
3. `WillResignActive` メソッドが呼び出され、メッセージアプリ拡張機能が実行されているメッセージ交換を表す `MSConversation` が渡されます。 この時点で、メッセージアプリと拡張機能の間の接続が解放されます。
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

このコードは、新しい `MSMessage` を作成し、いくつかのプロパティ (`Url`など) を設定します。 メッセージは iOS 上でのみ作成できますが、iOS と macOS の両方に送信して表示することができます。

ユーザーが macOS でメッセージ交換のメッセージバブルをクリックすると、Mac は web ブラウザーの URL で指定されたアドレスを開こうとします。 その結果、開発者の web サイトでは、macOS ベースのコンピューターの web ブラウザーにメッセージの一部を表示できるようになります。

`AccessibilityLabel` プロパティは、ユーザーに対するメッセージ交換のトランスクリプトを読み取るためにスクリーンリーダーによって使用されます。 `Layout` プロパティは、メッセージの表示方法を指定します。現在サポートされているのは `MSMessageTemplateLayout` のみで、次のようになります。

[![](advanced-message-app-extensions-images/interactive06.png "The MSMessageTemplateLayout template")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`MSMessageTemplateLayout` の [`Image`] プロパティでは、画面上の MessageBubble のメインボディのコンテンツが提供されます。 また、`MediaFileUrl` プロパティは、メッセージのバブルの本文にコンテンツを提供しますが、`UIImage` でサポートされていないコンテンツ (バックグラウンドでループするビデオファイルなど) を許可します。 `Image` と `MediaFileUrl` の両方のプロパティが指定されている場合は、`Image` プロパティが優先されます。 `MediaFileUrl` は、PNG、JPEG、GIF、およびビデオ (Media Player framework で再生できる任意の形式) をサポートしています。

推奨されるメディアサイズは、3倍の解像度で 300 x 300 ピクセルです。 小さいアセットと小さいアセットも使用できます。 Apple は、最適な結果を得るために、いくつかの異なるサイズのテストを提案します。 メッセージアプリは、必要に応じてこのメディアをダウンサンプリングし、スケーリングします。

アセットが受信者に送信されると、接続されているメディアは、ネットワーク上での転送を最適化するために、メッセージアプリによって自動的にトランスコードされます。 このため、Apple では、メッセージに添付されているメディア内のテキストを含めないようにしています。これは、メディアがスケールダウンされ、転送用に圧縮されるため、テキストが判読不能になる可能性があるためです。

`ImageTitle` と `ImageSubtitle` のプロパティは、メッセージのバブルに表示されるメディアの説明を提供します。 これらのプロパティは、受信デバイスにテキストとして送信され、イメージの左下隅に crisply にレンダリングされます。

`Caption`、`SubCaption`、`TrailingCaption`、および `TrailingSubcaption` の各プロパティでは、イメージについてさらに説明し、イメージの下のセクションにレンダリングします。 これらのプロパティをすべて `null` に設定すると、キャプション領域なしでメッセージバブルが作成されます。

[![](advanced-message-app-extensions-images/interactive07.png "A Message Bubble without the Caption Area")](advanced-message-app-extensions-images/interactive07.png#lightbox)

最後に注意すべき点は、メッセージアプリはメッセージバブルの左上隅にメッセージアプリ拡張機能のアイコンを描画することです。

## <a name="sending-a-message"></a>メッセージの送信

`MSMessage` が構成されたら、次のコードを使用して送信できます。

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

`MSMessagesAppViewController` の `ActiveConversation` プロパティは、メッセージアプリ拡張機能が起動された現在のメッセージ交換を保持します。

`MSConversation` の `InsertMessage` を呼び出してメッセージ交換にメッセージを含め、発生する可能性のあるエラーを処理します。 メッセージが正常に含まれている場合は、入力フィールドにメッセージのバブルが表示されます。

さらに、この拡張機能は、次のようなさまざまな種類のデータをメッセージ交換に送信できます。

- **テキスト** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **添付ファイル** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **ステッカー** - `ActiveConversation.InsertSticker (sticker, (obj) => {...});` `sticker` は `MSSticker`です。

入力フィールドに新しいコンテンツが追加されると、ユーザーは、(通常のメッセージと同様に) 青い **[送信]** ボタンをタップしてメッセージを送信できます。 メッセージアプリ拡張機能でコンテンツを自動的に送信する方法はありません。このプロセスは、ユーザーの制御下にあります。

## <a name="handling-the-compact-and-expanded-modes"></a>コンパクトモードと拡張モードの処理

メッセージアプリの拡張機能は、次の2つの異なる表示モードのいずれかで表示できます。

[![](advanced-message-app-extensions-images/interactive08.png "A Message App Extension displayed in two different view modes: Compact & Expanded")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -これは、メッセージアプリの拡張機能がメッセージビューの下位25% を占有する既定のモードです。 コンパクトモードでは、アプリはキーボード、水平スクロール、またはスワイプジェスチャのレコグナイザーにアクセスできません。 アプリには入力フィールドへのアクセス権があり、そのユーザーには `InsertMessage` の呼び出しが瞬時に表示されます。
- **展開**済み-メッセージアプリの拡張機能によってメッセージビュー全体が表示されます。 入力フィールドへのアクセス権はありませんが、キーボード、水平スクロール、およびスワイプジェスチャのレコグナイザーにアクセスできます。

メッセージアプリの拡張機能は、プログラムによって、またはユーザーが手動でいつでも手動で切り替えることができ、表示モードの変更にすぐに応答する必要があります。

2つの異なる表示モードの切り替えを処理する次の例を見てみましょう。 状態ごとに2つの異なるビューコントローラーが必要になります。 `StickerBrowserViewController` は**コンパクト**ビューを処理し、`AddStickerViewController` は**展開**されたビューを処理します。

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

`DidTransition` メソッドは、次の2つのモード間の切り替えを処理するためにオーバーライドされます。

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

必要に応じて、アプリは `WillTransition` メソッドを使用して、ユーザーに表示される前にビューモードの変更を処理することもできます (上記の Icecream Builder の例のように)。 詳細については、[ステッカーのカスタマイズ](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md)に関するドキュメントを参照してください。

## <a name="replying-to-a-message"></a>メッセージへの返信

メッセージに応答するときに、メッセージアプリの拡張機能が処理する必要があるケースが2つあります。

[![](advanced-message-app-extensions-images/interactive09.png "The Message App Extension in the Inactive and Active modes")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **拡張機能が非アクティブ**になっています。ユーザーがタップして拡張機能をアクティブ化し、対話形式で会話を続けることができるメッセージのトランスクリプトに、メッセージアプリ拡張機能のメッセージバブルの1つがあります。
- **拡張機能がアクティブになってい**ます。ユーザーは、メッセージトランスクリプトで Message App Extension のメッセージバブルをタップして展開されたビューモードに入り、中断した場所から対話型プロセスを続行できます。

### <a name="the-extension-is-inactive"></a>拡張機能がアクティブではありません

メッセージトランスクリプトでユーザーによってメッセージのバブルがタップされ、メッセージアプリの拡張機能が非アクティブになると、次の処理が行われます。

[![](advanced-message-app-extensions-images/interactive10.png "Handling an inactive Message Bubble")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. ユーザーが拡張機能のメッセージバブルをタップします。
2. 拡張機能が起動されると、メッセージアプリによってプロセスが起動されます。
3. `DidBecomeActive` メソッドが呼び出され、メッセージアプリ拡張機能が実行されているメッセージ交換を表す `MSConversation` が渡されます。
4. 拡張機能は `UIViewController` に基づいているため、`ViewWillAppear` と `ViewDidAppear` の両方が呼び出されます。

プロセスが完了すると、メッセージアプリの拡張機能が展開ビューモードで表示されます。

### <a name="the-extension-is-active"></a>拡張機能はアクティブです

メッセージトランスクリプトでユーザーによってメッセージバブルがタップされ、メッセージアプリ拡張機能がアクティブになっている場合、次の処理が行われます。

[![](advanced-message-app-extensions-images/interactive11.png "Handling an active Message Bubble")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. ユーザーが拡張機能のメッセージバブルをタップします。
2. メッセージアプリ拡張機能は既にアクティブになっているため、`MSMessagesAppViewController` の `WillTransition` メソッドを呼び出して、Compact から展開されたビューモードへの切り替えを処理します。
3. `MSMessagesAppViewController` の `DidSelectMessage` メソッドが呼び出され、メッセージバブルが属している `MSMessage` と `MSConversation` が渡されます。
4. `MSMessagesAppViewController` の `DidTransition` メソッドは、Compact から展開されたビューモードへの切り替えを処理するために呼び出されます。

この場合も、プロセスが完了すると、メッセージアプリの拡張機能が展開ビューモードで表示されます。

### <a name="accessing-the-selected-message"></a>選択したメッセージへのアクセス

どちらの場合も、ユーザーがメッセージアプリ拡張機能に属するメッセージバブルをタップすると、`MSConversation`の `SelectedMessage` プロパティを使用してタップされた `MSMessage` にアクセスできる必要があります。

(例:

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

[![](advanced-message-app-extensions-images/interactive12.png "The partially completed Message Bubbles can cluttering the Message Transcript")](advanced-message-app-extensions-images/interactive12.png#lightbox)

代わりに、メッセージのトランスクリプトで前のメッセージのバブルを簡潔なコメントに折りたたむ必要があります。

[![](advanced-message-app-extensions-images/interactive13.png "Collapsing the previous Message Bubbles in the Message Transcript")](advanced-message-app-extensions-images/interactive13.png#lightbox)

これは、既存のすべてのステップを折りたたむために `MSSession` を使用して処理されます。 したがって、`MSMessagesAppViewController` クラスの `DidSelectMessage` メソッドは、次のように変更できます。

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

選択したメッセージに既に既存の `MSSession`がある場合は、新しい `MSSession` が作成されます。 `MSMessage` の `SummaryText` プロパティを使用して、折りたたまれた前の手順にキャプションを追加します。 `SummaryText` プロパティが `null`に設定されている場合、メッセージ交換の前の手順は、メッセージ交換のトランスクリプトから完全に削除されます。

## <a name="advanced-message-api-features"></a>高度なメッセージ API 機能

上記の新しいメッセージ API の基本的な機能については、次に、Apple がフレームワークに組み込まれている高度な機能のいくつかを確認します。

まず、`MSMessagesAppViewController` クラスには、メッセージ交換により深いアクセスを提供するオーバーライドメソッドがいくつかあります。

- `DidStartSendingMessage`-ユーザーが [送信] ボタンをタップしたときに呼び出されます。 これは、送信プロセスが開始されただけで、メッセージが実際に受信者に配信されたことを意味するわけではありません。
- `DidCancelSendingMessage`-ユーザーがメッセージ交換のトランスクリプトのメッセージバブルの右上隅にある [ *X* ] ボタンをタップすると発生します。
- `DidReceiveMessage`-このメソッドは、メッセージアプリの拡張機能がアクティブになっているときに呼び出され、メッセージ交換のいずれかの参加要素から新しいメッセージを受信しました。

### <a name="group-conversations"></a>メッセージ交換のグループ化

メッセージアプリ拡張機能は、ユーザーがグループの会話 (3 人以上) に関与している間に使用できます。これは、メッセージアプリ拡張機能を設計および実装するときに考慮する必要があります。

3人のユーザーとのグループメッセージ交換で、次の対話を見てみましょう。

[![](advanced-message-app-extensions-images/interactive14.png "Interaction in a group conversation with three users")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. ユーザー1は、ユーザー2とユーザー3にハンバーガートッピングを選択するように求めるグループ対話型のメッセージを送信します。
2. ユーザー2はトマトを選択します。
3. ユーザー3が選択を選択します。
4. ユーザー2とユーザー3の両方の選択肢は、ほぼ同時にユーザー1に返されます。 その結果、ユーザー2の選択は概要行に折りたたまれ、使用できなくなります。 このケースは、ユーザー2が表示され、ユーザー3が折りたたまれている場合にも、反転されている可能性があります。

どちらの場合も、ユーザー1はユーザー2とユーザー3の両方の選択肢にアクセスできる必要があるため、この動作は望ましくありません。 この状況に対処するため、Apple では、メッセージアプリ拡張機能がメッセージの状態をクラウドに格納し、この状態にアクセスするために (ユーザー間で送信される) `MSMessage` の URL プロパティを使用することを提案しています。

ユーザーがメッセージを送信すると、セッショントークンが生成され、現在のメッセージ状態でクラウドにプッシュされます。 ユーザーがメッセージ交換のトランスクリプトでメッセージのバブルをタップすると、セッショントークンが使用され、現在のセッション状態がクラウドから取得されます。

### <a name="sender-identifiers"></a>送信者の識別子

メッセージの送信者の識別子にアクセスする方法については、上記のグループメッセージ交換の例を次に示します。

[![](advanced-message-app-extensions-images/interactive15.png "Group conversation sending Identifiers")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. ここでも、ユーザー1は、ユーザー2とユーザー3にハンバーガートッピングを選択するように求めるグループ対話型メッセージを送信します。
2. ユーザー3が選択します。
3. ユーザー3の選択がユーザー1に返され、ユーザー2はまだ返信していません。
4. Apple はユーザーのプライバシーに関係しているため、メッセージアプリの拡張機能は、メッセージ交換の各参加者に割り当てられた一意の識別子 (`NSUUID`) のみを認識します。 ローカルデバイスでは、現在のユーザーの識別子のみが認識されます。
5. `MSMessage` には、拡張機能に認識されている参加者の一覧にあるユーザーの1つに一致する `SenderIdentifier` プロパティがあります。
6. 各ユーザーデバイスには、参加者リストの独自のコピーがあります。ここでも、ローカルユーザーの識別子のみが認識されます。
7. メッセージが送信されると、その `SenderIdentifier` のプロパティは、ローカルユーザーのものでもあります。

送信者の識別子は、次の方法で使用できます。

- 参加者の一覧を見ると、拡張機能によって、メッセージ交換のユーザー数を取得できます。
- 拡張機能は、ユーザーからメッセージを受信すると、送信者の識別子を追跡できます。 同じ送信者識別子を持つ別のメッセージを受信した場合、拡張機能は同じユーザーのものであることを認識します。
- これらは、メッセージ交換の特定のユーザーを識別するために使用できます。

送信者の識別子は、ドル記号 (`$`) を付けることによって、`MSMessageTemplateLayout` のテキストフィールドのいずれかで使用できます。 (例:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

メッセージアプリにこの種類の書式設定のメッセージバブルが表示されると、メッセージを送信したメッセージ交換の参加者の連絡先名に `$uuid...` が置き換えられます。

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

3つのプラットフォームのうち、ユーザーが対話型メッセージを生成することを許可するのは、iOS 10 だけです。 MacOS Sierra では、ユーザーが対話型メッセージのバブルをクリックすると、`MSMessage` に添付されている URL が Safari で開かれ、メッセージの表現がそこに表示されます。

WatchOS では、メッセージアプリは、ユーザーが応答を作成できる接続された iOS デバイスに対話型メッセージをハンドオフできます。

古い Apple プラットフォームで対話型メッセージを受信した場合、新しい Messages API はフォールバックをサポートしています。

- watchOS 2 +
- OS X 10.11 +
- iOS 9 以降

これらは、2つの異なるメッセージとしてフォールバック形式で配信されます。

- 1つは、テンプレートレイアウトによって提供されるイメージです。
- もう1つは、`MSMessage`に指定された URL です。

## <a name="summary"></a>まとめ

この記事では、Xamarin. iOS ソリューションでメッセージアプリ拡張機能を使用するための高度な手法について説明しました。このソリューションは、**メッセージ**アプリと統合され、ユーザーに新しい機能を提供します。

## <a name="related-links"></a>関連リンク

- [アイスクリームビルダー (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [メッセージリファレンス](https://developer.apple.com/reference/messages)
- [アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
