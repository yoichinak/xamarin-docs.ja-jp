---
title: "高度なユーザーへの通知"
description: "この記事では、新しいユーザーへの通知フレームワークについて詳しく説明し、Xamarin.iOS アプリでこれを最大限に活用する方法です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4e1ff652-28f0-4566-b383-9d12664401a4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6408f3b45f93413fa814e410f07e7b71179b7338
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="advanced-user-notifications"></a>高度なユーザーへの通知

_この記事では、新しいユーザーへの通知フレームワークについて詳しく説明し、Xamarin.iOS アプリでこれを最大限に活用する方法です。_

初めて使用する iOS 10 では、ユーザー通知の配信とローカルおよびリモートの通知の処理のフレームワークを利用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカルに関する通知の配信場所などの条件のセットまたは 1 日の時刻を指定することで。

## <a name="about-user-notifications"></a>ユーザー通知について

新しいユーザーの通知フレームワークにより配信とローカルおよびリモートの通知の処理。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカルに関する通知の配信場所などの条件のセットまたは 1 日の時刻を指定することで。

さらに、アプリまたは拡張機能が表示される (および可能性のある変更できます) ローカルとリモートの両方の通知、ユーザーの iOS デバイスに配信されるようにします。

新しいユーザー通知の UI フレームワークが使用するは、アプリまたはアプリの拡張機能をユーザーに表示するときに、ローカルおよびリモートの両方の通知の外観をカスタマイズします。

このフレームワークは、アプリがユーザーに通知を配信できる、次の方法を提供します。

- **Visual アラート**バナーとして画面の上部から下へ通知ロールアップ場所 - です。
- **音と振動**-通知を関連付けることができます。
- **アプリのアイコンのバッジ**- アプリのアイコンには、新しいコンテンツが使用できることを示すバッジが表示されます。 など、未読の電子メール メッセージの数。

さらに、ユーザーの現在のコンテキストによっては、通知が表示されるさまざまな方法です。

- デバイスがロックされている場合は、通知はロール ダウン、画面の上部からバナーとして。
- デバイスがロックされている場合は、ユーザーのロック画面で通知が表示されます。
- ユーザーが通知を受信しなかった場合は、通知センターを開き、任意の使用可能な待機通知がありますを表示、ことができます。

Xamarin.iOS アプリには、2 種類のユーザーへの通知を送信することがあることがあります。

- **ローカル通知**-これらは、ユーザーのデバイスでローカルにインストールされているアプリによって送信されます。
- **リモート通知**- リモート サーバーと、ユーザーに対して表示されるいずれかから送信されるか、アプリのコンテンツのバック グラウンドの更新をトリガーします。

詳細についてを参照してください、[ユーザー通知の拡張](~/ios/platform/user-notifications/enhanced-user-notifications.md)ドキュメント。

## <a name="the-new-user-notification-interface"></a>新しいユーザー通知インターフェイス

デバイスまたは通知センターの上部にバナーとして ロック画面で、タイトル、サブタイトルを提示する省略可能なメディア添付ファイルなどのより多くのコンテンツを提供する新しい UI 設計 iOS 10 のユーザーへの通知が表示されます。

ユーザー通知を表示する場所 iOS 10 に関係なく同じのルック アンド フィールと同じ機能が表示されます。

8、iOS では、Apple で、開発者が、通知にカスタム動作を接続し、ユーザーがアプリを起動しなくても、通知でアクションを実行できるようにでした実践的な通知が導入されました。 IOS 9 では、Apple には、ユーザーが通知され、テキスト入力に応答する簡易応答の実践的な通知が強化されています。

Apple が 3 D Touch、ここでの通知を押すし、カスタム ユーザー インターフェイスは、機能豊富な対話を提供する表示をサポートするために実用的な通知をさらに拡張ユーザーへの通知は iOS 10 のユーザー エクスペリエンスの詳細に不可欠であるため、通知します。

カスタム ユーザー通知の UI が表示されたら、ユーザーが、通知に関連付けられた任意のアクションとやり取りする場合の変更点に関するフィードバックを提供するカスタム UI をすぐに更新できます。

新しいユーザー通知 UI API により簡単に活用するためにこれらの新しいユーザー通知の UI 機能 Xamarin.iOS アプリ、10、iOS にします。

## <a name="adding-media-attachments"></a>メディアの添付ファイルを追加します。

ユーザーの間で共有を取得している一般的なアイテムの 1 つは写真、追加ために iOS 10 (写真) などのメディア項目をアタッチすることでことが示され、他の通知のコンテと共にユーザーにすぐに使用できる、通知に直接nt です。

ただし、送信に関係するサイズであるためであっても、小さな画像、リモートの通知のペイロードにアタッチすることが不可能な場合。 このような状況を処理するため、開発者を使用して、新しいサービス拡張 10、iOS で (CloudKit データストア) などの別のソースからイメージをダウンロードし、ユーザーに表示されるまで、通知のコンテンツにアタッチします。

サービス拡張機能を使用して変更するリモート通知の場合は、そのペイロードを変更可能としてマークにする必要があります。 例:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

プロセスの次の概要を参照してください。

[ ![](advanced-user-notifications-images/extension02.png "メディアの添付ファイルのプロセスを追加します。")](advanced-user-notifications-images/extension02.png)

サービス拡張機能が必要なすべての手段を使用して必要なイメージをダウンロードできますし、(APNs) を使用してデバイスをリモートの通知を配信すると、一度 (など、 `NSURLSession`) および通知と表示の内容を変更できるイメージを受信した後これをユーザーにします。

コードでこのプロセスを処理する方法の例を次に示します。

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

APNs からの通知を受け取ったときに、イメージのカスタムのアドレスはコンテンツから読み取られ、サーバーからファイルをダウンロードします。 `UNNotificationAttachement`が一意の ID で、ローカルの場所、イメージの作成 (として、 `NSUrl`)。 通知コンテンツの変更可能なコピーが作成され、メディアの添付ファイルが追加されます。 呼び出して、ユーザーに通知が最後に、ように表示されるか、`contentHandler`です。

添付ファイルは、通知に追加されると、システムは、移動し、ファイルの管理。

上に示したリモート通知、だけでなく、メディアの添付ファイルはローカルの通知からサポートも、ここで、`UNNotificationAttachement`が作成され、その内容と共に通知にアタッチします。

10、iOS での通知は、イメージのメディアの添付ファイルをサポート (静的および Gif)、オーディオまたはビデオとシステムが自動的に表示、正しいカスタム UI のそれぞれの添付ファイルの種類をユーザーに通知が表示された場合。

> [!NOTE]
> **注:**慎重メディア サイズを最適化して、システムとして、リモート サーバーからメディアをダウンロードする (またはローカル通知はメディアをアセンブルする) にかかる時間では、アプリのサービスを実行するときに両方に厳密な制限が設けられて拡張機能です。 たとえば、画像の縮小したバージョンまたは、通知に表示するビデオの小さなクリップを送信するとします。

## <a name="creating-custom-user-interfaces"></a>カスタム ユーザー インターフェイスの作成

そのユーザーへの通知に対するカスタム ユーザー インターフェイスを作成するには、通知コンテンツ拡張機能 (新しい iOS 10 に) ソリューションに追加するアプリの開発者は、必要があります。

通知コンテンツの拡張機能により、開発者は、通知の UI にそれぞれのビューを追加し、必要とするすべてのコンテンツを描画します。 ただし、(ボタンやテキスト フィールド) などの対話型の UI ウィジェットは**いない**通知 UI でサポートされているし、UI に応じたカスタム デザインに追加することはありません。

ユーザー通知とユーザーの対話をサポートするには、カスタム アクションが作成、システムに登録され、システムでスケジュールする前に、通知にアタッチされています。 通知のコンテンツの拡張は、これらのアクションの処理に呼び出されます。 参照してください、[通知アクションがある作業](~/ios/platform/user-notifications/enhanced-user-notifications.md)のセクションで、[ユーザー通知の拡張](~/ios/platform/user-notifications/enhanced-user-notifications.md)カスタム動作の詳細についてはドキュメントです。

カスタム UI を使用したユーザーの通知がユーザーに表示されたら、次の要素になります。

[ ![](advanced-user-notifications-images/customui01.png "カスタム UI 要素を持つユーザー通知")](advanced-user-notifications-images/customui01.png)

場合は、ユーザーが対話 (通知を記載) カスタム動作、ユーザー インターフェイスは、特定のアクションが呼び出されたときに発生した、新機能とユーザー フィードバックを更新できます。

### <a name="adding-a-notification-content-extension"></a>通知のコンテンツの拡張を追加します。

Xamarin.iOS アプリでは、カスタム ユーザー通知 UI を実装するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual studio for mac アプリのソリューションを開く
2. ソリューション名を右クリックし、**ソリューション パッド**選択**追加** > **新しいプロジェクトの追加**です。
3. 選択**iOS** > **拡張機能** > **通知コンテンツの拡張機能** をクリックし、**次**ボタン。 

    [ ![](advanced-user-notifications-images/notify01.png "通知のコンテンツの拡張機能を選択します。")](advanced-user-notifications-images/notify01.png)
4. 入力、**名前**拡張機能とクリック、 **[次へ]**ボタン。 

    [ ![](advanced-user-notifications-images/notify02.png "拡張機能の名前を入力します。")](advanced-user-notifications-images/notify02.png)
5. 調整、**プロジェクト名**や**ソリューション名**要求されて、をクリックして、**作成**ボタン。 

    [ ![](advanced-user-notifications-images/notify03.png "プロジェクトの名前、およびソリューション名を調整します。")](advanced-user-notifications-images/notify03.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual studio for mac アプリのソリューションを開く
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加**です。
3. 選択**iOS** > **拡張** > **通知コンテンツの拡張機能**: 

    [ ![](advanced-user-notifications-images/notify01w.png "通知のコンテンツの拡張機能を選択します。")](advanced-user-notifications-images/notify01w.png)
4. 入力してください、**名**をクリックして、拡張機能の**OK**ボタンをクリックします。

-----

通知のコンテンツの拡張は、ソリューションに追加するときに、3 つのファイルは、拡張機能のプロジェクトに作成されます。

1. `NotificationViewController.cs` -これは、通知のコンテンツの拡張のメイン ビュー コント ローラーです。
2. `MainInterface.storyboard` -ここで、開発者は、iOS デザイナーに通知のコンテンツの拡張の表示される UI をレイアウトします。
3. `Info.plist` 通知のコンテンツの拡張の構成を制御します。

既定値`NotificationViewController.cs`ファイルは、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

`DidReceiveNotification`メソッドは、通知のコンテンツの拡張の内容にカスタム UI を設定できるように、ユーザーが、通知が展開されているときに呼び出されますが、`UNNotification`です。 上記の例については、ビュー、名前のコードに公開するラベルの追加が`label`通知の本文を表示するために使用します。

### <a name="setting-the-notification-content-extensions-categories"></a>通知コンテンツ拡張機能のカテゴリの設定

システムに応答する、特定のカテゴリに基づいて、アプリの通知のコンテンツの拡張を検索する方法を通知する必要があります。 次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、拡張機能の`Info.plist`ファイルで、**ソリューション パッド**編集用に開きます。
2. 切り替えて、**ソース**ビュー。
3. 展開して、`NSExtension`キー。
4. 追加、`UNNotificationExtensionCategory`型としてキー**文字列**拡張機能が属するカテゴリの値は、(この例では ' イベント招待)。 

    [ ![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory キーを追加します。")](advanced-user-notifications-images/customui02.png)
5. 変更内容を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、拡張機能の`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
3. 展開して、`NSExtension`キー。
4. 追加、`UNNotificationExtensionCategory`型としてキー**文字列**拡張機能が属するカテゴリの値は、(この例では ' イベント招待)。 

    [ ![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory キーを追加します。")](advanced-user-notifications-images/customui02w.png)
5. 変更内容を保存します。

-----

通知コンテンツの拡張機能カテゴリ (`UNNotificationExtensionCategory`) 通知アクションの登録に使用される同じカテゴリの値を使用します。 アプリの複数のカテゴリの同じ UI を使用できる場所の場合、スイッチ、`UNNotificationExtensionCategory`型に**配列**し、すべての必要なカテゴリを提供します。 例:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui03.png "通知のコンテンツの拡張カテゴリ")](advanced-user-notifications-images/customui03.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui03w.png "通知のコンテンツの拡張カテゴリ")](advanced-user-notifications-images/customui03w.png)

-----

### <a name="hiding-the-default-notification-content"></a>通知の既定のコンテンツを非表示にします。

ここでカスタム通知の UI を表示内容が同じ通知 (タイトル、サブタイトル、および通知の UI の下部に自動的に表示される本文) の既定値としての場合、を追加することでこの既定の情報を隠すことができます`UNNotificationExtensionDefaultContentHidden`キーを`NSExtensionAttributes`型としてキー**ブール**の値を持つ`YES`で拡張機能の`Info.plist`ファイル。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui04.png "既定の情報を検索します。")](advanced-user-notifications-images/customui04.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui04w.png "既定の情報を検索します。")](advanced-user-notifications-images/customui04w.png)

-----

### <a name="designing-the-custom-ui"></a>カスタムの UI のデザイン

通知コンテンツ拡張機能のカスタム ユーザー インターフェイスを設計するをダブルクリックして、`MainInterface.storyboard`ファイルを開いて、iOS、デザイナーで編集するファイルが必要なインターフェイスを構築する必要のある要素のドラッグ (など`UILabels`と`UIImageViews`)。

> [!NOTE]
> **注:**通知 UI は_いない_通知コンテンツの拡張でテキスト フィールドやボタンなどの対話型のコントロールをサポートします。 これらは、ストーリー ボードに追加することができます、中に、ユーザーはこれらと対話することはできません。 通知のカスタム UI には、ユーザーの操作を追加するには、代わりにカスタム動作を使用します。

UI のレイアウトされているし、必要なコントロールは、c# コードに公開される、開く、`NotificationViewController.cs`を編集するためと変更、`DidReceiveNotification`ユーザー通知を展開する場合に、UI を設定するメソッド。 例:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>コンテンツ領域のサイズの設定

ユーザーに表示されるコンテンツ領域のサイズを調整するには、次のコードを設定、`PreferredContentSize`プロパティに、`ViewDidLoad`メソッド目的のサイズにします。 このサイズは、ios デザイナー ビューに制約を適用することによっても調整される可能性があります、開発者はそれらの最適な方法の選択に任されています。

最初は、通知の通知が送信されるコンテンツの拡張が呼び出されると、コンテンツ領域、システムが既に実行されているためフル インストール オプションのサイズし、ユーザーに対して表示されるときに、要求されたサイズまでアニメーション化します。

この特殊効果をなくすため、編集、`Info.plist`拡張機能とセットのファイルを`UNNotificationExtensionInitialContentSizeRatio`のキー、`NSExtensionAttributes`入力するキー**数**目的の比率を表す値を持つ。 例:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio キー")](advanced-user-notifications-images/customui05.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio キー")](advanced-user-notifications-images/customui05w.png)

-----

### <a name="using-media-attachments-in-custom-ui"></a>カスタム UI でのメディアの添付ファイルの使用

メディアの添付ファイル (に示すように、[メディアの添付ファイルを追加する](#Adding-Media-Attachments)前のセクション) 通知のペイロードの一部である、アクセスし既定値の場合と同様に、通知のコンテンツの拡張に表示されることができますUI を通知します。

たとえば、上記のカスタム UI が含まれている場合、 `UIImageView` c# コードに公開するが、次のコードを使用すると、そこからメディア添付ファイルでした、します。

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

メディアの添付ファイルがシステムによって管理されているためには、アプリのサンド ボックス外です。 拡張機能を呼び出すことによって、ファイルへのアクセスを求めていることをシステムに通知する必要があります、`StartAccessingSecurityScopedResource`メソッドです。 呼び出す必要があります、拡張機能では、ファイル処理が終わったら、`StopAccessingSecurityScopedResource`その接続を解放します。

### <a name="adding-custom-actions-to-a-custom-ui"></a>カスタム UI カスタム動作の追加

前述のように、通知 UI がテキスト フィールドやボタンなどのコントロールが UI のデザインでサポートされていないためユーザーとの対話をサポートしていません。

代わりに、通知のカスタム UI に対話機能を追加する標準カスタム アクションのメカニズムが使用します。 参照してください、[通知アクションがある作業](~/ios/platform/user-notifications/enhanced-user-notifications.md)のセクションで、[ユーザー通知の拡張](~/ios/platform/user-notifications/enhanced-user-notifications.md)カスタム アクションの詳細についてはドキュメントです。

だけでなくカスタムの動作では、通知のコンテンツの拡張は、次の組み込みアクションにもに応答できます。

- **既定のアクションが**-これは、ユーザーがアプリを開き、特定の通知の詳細を表示する通知をタップしたときにします。
- **アクションを破棄**-ユーザーが特定の通知を閉じるときにこの操作は、アプリに送信されます。

通知のコンテンツの拡張機能もユーザーがカスタム アクションのいずれかを呼び出したときに、UI を更新する機能は、ユーザーがタップしたときに承諾されると、日付の表示など、 **Accept**カスタム アクション ボタンをクリックします。 さらに、通知コンテンツの拡張機能は、通知を閉じる前に、ユーザーはその操作の効果を確認できるように、通知 UI の棄却を遅延するシステムに指示できます。

これは、第 2 のバージョンを実装することで、`DidReceiveNotification`完了ハンドラーを含むメソッドです。 例:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

追加することによって、`Server.PostEventResponse`ハンドラー、`DidReceiveNotification`拡張子通知コンテンツの拡張のメソッド*必要があります*すべてのカスタム アクションを処理します。 拡張機能を変更して、カスタム アクションを含むアプリにログオンを転送できますも、`UNNotificationContentExtensionResponseOption`です。 例:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>カスタム UI テキストの入力操作の操作

アプリと、通知のデザイン、に応じてユーザー (メッセージに返信する) などの通知にテキストを入力する必要は時間があります。 通知のコンテンツの拡張、標準的な通知と同じように組み込みのテキストの入力操作にアクセスしています。

例:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

このコードは、新しいテキストの入力操作を作成し、拡張機能のカテゴリに追加します (で、 `MakeExtensionCategory`) メソッドです。 `DidReceive`メソッドをオーバーライドを次のコードのテキストを入力するユーザーを処理します。

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

設計が呼び出す場合、テキスト入力フィールドにカスタム ボタンを追加するため、それらを含めるには、次のコードを追加します。

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

ユーザーがコメントのアクションがトリガーされると、ビュー コント ローラーと、カスタム テキスト入力フィールドの両方をアクティブにする必要があります。

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリでは、新しいユーザーの通知フレームワークを使用して、高度な参照を取得しました。 ローカルとリモートの通知の両方にメディアの添付ファイルを追加することを説明し、新しいユーザー通知の UI を使用してカスタム通知の Ui を作成することについて説明します。


## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Framework リファレンス](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [ローカルおよびリモートの通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
