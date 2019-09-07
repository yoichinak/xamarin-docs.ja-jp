---
title: Xamarin. iOS の高度なユーザー通知
description: この記事では、iOS 10 で導入されたユーザー通知フレームワークについて詳しく説明します。 ユーザー通知、ユーザー通知インターフェイス、メディア添付ファイル、カスタムユーザーインターフェイスなどについて説明します。
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 8bca2b47212b9effe637dcd2e116630579609b39
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769413"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Xamarin. iOS の高度なユーザー通知

IOS 10 の新機能であるユーザー通知フレームワークを使用すると、ローカルおよびリモートの通知を配信および処理することができます。 このフレームワークを使用すると、アプリまたはアプリ拡張機能は、場所や時間などの条件のセットを指定することによって、ローカル通知の配信をスケジュールすることができます。

## <a name="about-user-notifications"></a>ユーザーへの通知について

新しいユーザー通知フレームワークを使用すると、ローカルおよびリモートの通知を配信および処理することができます。 このフレームワークを使用すると、アプリまたはアプリ拡張機能は、場所や時間などの条件のセットを指定することによって、ローカル通知の配信をスケジュールすることができます。

また、アプリまたは拡張機能は、ローカルとリモートの両方の通知をユーザーの iOS デバイスに配信するときに、それらを受信 (および変更する可能性があります) することができます。

新しいユーザー通知 UI フレームワークでは、アプリまたはアプリの拡張機能を使用して、ユーザーに表示されるときに、ローカルとリモートの両方の通知の外観をカスタマイズできます。

このフレームワークには、アプリがユーザーに通知を配信するための次の方法が用意されています。

- **視覚的な警告**-通知が画面の上部からバナーとしてロールダウンされます。
- **Sound と Vibrations** -通知に関連付けることができます。
- **アプリアイコン**のバッジ: アプリのアイコンに、新しいコンテンツが使用可能であることを示すバッジが表示されます。 (未読の電子メールメッセージの数など)。

さらに、ユーザーの現在のコンテキストによっては、通知が表示されるさまざまな方法があります。

- デバイスのロックが解除されている場合、通知は画面の上部からバナーとしてロールダウンされます。
- デバイスがロックされている場合は、ユーザーのロック画面に通知が表示されます。
- ユーザーが通知を失った場合、通知センターを開いて、利用可能な待機中の通知を表示できます。

Xamarin iOS アプリには、次の2種類のユーザー通知を送信できます。

- **ローカル通知**-これらは、ユーザーのデバイスにローカルにインストールされたアプリによって送信されます。
- **リモート通知**-リモートサーバーから送信され、ユーザーに表示されるか、アプリのコンテンツのバックグラウンド更新をトリガーします。

詳細については、強化された[ユーザー通知](~/ios/platform/user-notifications/enhanced-user-notifications.md)に関するドキュメントを参照してください。

## <a name="the-new-user-notification-interface"></a>新しいユーザー通知インターフェイス

IOS 10 のユーザー通知には、新しい UI デザインが表示されます。これにより、ロック画面に表示されるタイトル、サブタイトル、オプションのメディア添付ファイルなど、より多くのコンテンツを、デバイスまたは通知センターのバナーとして使用できるようになります。

IOS 10 でユーザー通知が表示される場所に関係なく、同じルックアンドフィールと同じ機能を使用して表示されます。

IOS 8 では、開発者がカスタムアクションを通知に添付し、ユーザーがアプリを起動せずに通知に対してアクションを実行できるようにする、実行可能な通知が導入されました。 IOS 9 では、クイック応答を使用した Apple の強化されたアクション可能な通知で、ユーザーはテキスト入力を使用して通知に応答できます。

ユーザーの通知は iOS 10 のユーザーエクスペリエンスの重要な部分であるため、Apple は、ユーザーが通知を押すとカスタムユーザーインターフェイスが表示され、豊富な対話機能を提供する3D タッチをサポートするために、実行可能な通知をさらに拡張しています。通知を受け取る。

カスタムユーザー通知 UI が表示されている場合、ユーザーが通知に添付されたアクションを操作すると、カスタム UI をすぐに更新して、変更された内容についてのフィードバックを提供できます。

IOS 10 の新機能であるユーザー通知 UI API を使用すると、Xamarin iOS アプリは、これらの新しいユーザー通知 UI 機能を簡単に利用できます。

## <a name="adding-media-attachments"></a>メディア添付ファイルの追加

ユーザー間で共有される一般的な項目の1つは写真です。そのため、iOS 10 では、メディア項目 (写真など) を通知に直接添付する機能が追加されています。これにより、ユーザーは、通知の残りの部分と共に、すぐに使用できるようになります。r.

ただし、小さなイメージの送信にはサイズが関係しているため、リモート通知ペイロードへのアタッチは実用的ではなくなります。 この状況に対処するために、開発者は iOS 10 の新しいサービス拡張機能を使用して、別のソース (CloudKit データストアなど) からイメージをダウンロードし、ユーザーに表示する前に通知のコンテンツに添付できます。

リモート通知をサービス拡張によって変更するには、そのペイロードを変更可能としてマークする必要があります。 例えば:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

プロセスの概要については、次を参照してください。

[![](advanced-user-notifications-images/extension02.png "メディア添付ファイルの追加プロセス")](advanced-user-notifications-images/extension02.png#lightbox)

リモート通知がデバイスに (APNs 経由で) 配信されると、サービス拡張は、必要な方法 ( `NSURLSession`など) を使用して必要なイメージをダウンロードし、イメージを受信した後で、通知の内容を変更して表示することができます。これをユーザーに対して行います。

このプロセスがコードで処理される方法の例を次に示します。

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

APNs から通知を受信すると、イメージのカスタムアドレスがコンテンツから読み取られ、ファイルがサーバーからダウンロードされます。 次に`UNNotificationAttachement` 、一意の ID とイメージのローカルの場所 ( `NSUrl`) を使用して、が作成されます。 通知コンテンツの変更可能なコピーが作成され、メディア添付ファイルが追加されます。 最後に、を呼び出し`contentHandler`て、通知をユーザーに表示します。

通知に添付ファイルが追加されると、システムはファイルの移動と管理を引き継ぎます。

上に示したリモート通知に加えて、メディアの添付ファイルはローカル通知からもサポート`UNNotificationAttachement`されます。ここでは、が作成され、その内容と共に通知に添付されます。

IOS 10 の通知では、イメージ (静的および Gif)、オーディオまたはビデオ、およびシステムのメディア添付ファイルがサポートされており、ユーザーに通知が表示されるときに、これらの種類の添付ファイルごとに正しいカスタム UI が表示されます。

> [!NOTE]
> メディアのサイズと、リモートサーバーからメディアをダウンロードするためにかかる時間 (またはローカル通知用にメディアを組み立てる) の両方を最適化するためには、アプリのサービス拡張機能を実行するときに、システムが厳密な制限を課す必要があります。 たとえば、画像のスケールダウンバージョンやビデオの小さなクリップを送信して、通知に表示することを検討してください。

## <a name="creating-custom-user-interfaces"></a>カスタムユーザーインターフェイスの作成

ユーザー通知用のカスタムユーザーインターフェイスを作成するには、開発者はアプリのソリューションに通知コンテンツ拡張機能 (iOS 10 の新機能) を追加する必要があります。

通知コンテンツ拡張機能を使用すると、開発者は独自のビューを通知 UI に追加し、必要なコンテンツを描画できます。 IOS 12 以降では、通知コンテンツの拡張機能によって、ボタンやスライダーなどの対話型 UI コントロールがサポートされるようになります。 詳細については、 [iOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md)ドキュメントの「対話型通知」を参照してください。

ユーザー通知を使用したユーザーの操作をサポートするには、システムに登録されたカスタムアクションを作成してシステムに登録し、通知に添付してからシステムでのスケジュールを設定する必要があります。 これらのアクションの処理を処理するために、通知コンテンツの拡張機能が呼び出されます。 カスタムアクションの詳細については、[拡張ユーザー通知](~/ios/platform/user-notifications/enhanced-user-notifications.md)に関するドキュメントの「[通知アクション](~/ios/platform/user-notifications/enhanced-user-notifications.md)の使用」セクションを参照してください。

カスタム UI を使用したユーザー通知をユーザーに提示すると、次の要素が表示されます。

[![](advanced-user-notifications-images/customui01.png "カスタム UI 要素を使用したユーザー通知")](advanced-user-notifications-images/customui01.png#lightbox)

ユーザーがカスタムアクション (通知の下に表示される) と対話する場合は、ユーザーインターフェイスを更新して、特定のアクションを呼び出したときの動作をユーザーにフィードバックすることができます。

### <a name="adding-a-notification-content-extension"></a>通知コンテンツ拡張機能の追加

カスタムユーザー通知 UI を Xamarin iOS アプリに実装するには、次の手順を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac でアプリのソリューションを開きます。
2. **Solution Pad**でソリューション名を右クリックし、[**追加** > ] **[新しいプロジェクト]** の順に選択します。
3. **IOS**拡張機能の > 通知コンテンツ拡張機能を選択し、[次へ] ボタンをクリックします。 >  

    [![](advanced-user-notifications-images/notify01.png "通知コンテンツの拡張機能の選択")](advanced-user-notifications-images/notify01.png#lightbox)
4. 拡張機能の**名前**を入力し、 **[次へ]** ボタンをクリックします。 

    [![](advanced-user-notifications-images/notify02.png "拡張機能の名前を入力してください")](advanced-user-notifications-images/notify02.png#lightbox)
5. 必要に応じて**プロジェクト名**または**ソリューション名**を調整し、 **[作成]** ボタンをクリックします。 

    [![](advanced-user-notifications-images/notify03.png "プロジェクト名またはソリューション名の調整")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio for Mac でアプリのソリューションを開きます。
2. **ソリューションエクスプローラー**でソリューション名を右クリックし、 **[> 新しいプロジェクトの追加]** を選択します。
3. **[Visual C# > IOS Extensions > Notification Content Extension]** を選択します。

    [![](advanced-user-notifications-images/notify01.w157-sml.png "通知コンテンツの拡張機能の選択")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. 拡張機能の**名前**を入力し、 **[OK]** をクリックします。

-----

通知コンテンツ拡張機能をソリューションに追加すると、拡張機能のプロジェクトに次の3つのファイルが作成されます。

1. `NotificationViewController.cs`-これは、通知コンテンツ拡張機能のメインビューコントローラーです。
2. `MainInterface.storyboard`-開発者は、iOS デザイナーで通知コンテンツ拡張機能の表示可能な UI をレイアウトします。
3. `Info.plist`-通知コンテンツ拡張機能の構成を制御します。

既定`NotificationViewController.cs`のファイルは次のようになります。

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

通知がユーザーによって拡張されると、 `UNNotification`メソッドが呼び出されます。これにより、通知コンテンツの拡張機能がのコンテンツを使用してカスタムUIにデータを入力できるようになります。`DidReceiveNotification` 上の例では、ラベルがビューに追加され、という名前`label`のコードに公開され、通知の本文を表示するために使用されます。

### <a name="setting-the-notification-content-extensions-categories"></a>通知コンテンツ拡張機能のカテゴリの設定

システムは、応答する特定のカテゴリに基づいて、アプリの通知コンテンツ拡張機能を検索する方法を通知する必要があります。 次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Solution Pad 内の拡張機能の`Info.plist`ファイルをダブルクリックして、編集用に開きます。
2. **ソース**ビューに切り替えます。
3. キーを`NSExtension`展開します。
4. 拡張機能`UNNotificationExtensionCategory`が属しているカテゴリの値 (この例では "イベント招待") を**文字列**型としてキーを追加します。 

    [![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory キーを追加する")](advanced-user-notifications-images/customui02.png#lightbox)
5. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ソリューションエクスプローラー内の拡張機能の`Info.plist`ファイルをダブルクリックして、編集用に開きます。
2. キーを`NSExtension`展開します。
3. 拡張機能`UNNotificationExtensionCategory`が属しているカテゴリの値 (この例では "イベント招待") を**文字列**型としてキーを追加します。 

    [![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory キーを追加する")](advanced-user-notifications-images/customui02w.png#lightbox)
4. 変更内容を保存します。

-----

通知コンテンツ拡張機能の`UNNotificationExtensionCategory`カテゴリ () は、通知アクションの登録に使用されるのと同じカテゴリ値を使用します。 アプリが複数のカテゴリに同じ UI を使用する場合は、を型の`UNNotificationExtensionCategory` **配列**に切り替えて、必要なすべてのカテゴリを指定します。 例えば:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui03.png "通知コンテンツの拡張機能のカテゴリ")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui03w.png "通知コンテンツの拡張機能のカテゴリ")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>既定の通知コンテンツを非表示にする

カスタム通知 UI に既定の通知 (タイトル、サブタイトル、本文が通知 UI の下部に自動的に表示される) と同じ内容が表示される場合、この既定の情報は、 `UNNotificationExtensionDefaultContentHidden`拡張機能の`NSExtensionAttributes` `YES`ファイル内の値を持つブール型`Info.plist`のキーに対するキー。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui04.png "既定の情報の検索")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui04w.png "既定の情報の検索")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>カスタム UI のデザイン

Notification Content 拡張機能のカスタムユーザーインターフェイスをデザインするには、 `MainInterface.storyboard`ファイルをダブルクリックして iOS デザイナーで編集できるようにし、目的のインターフェイスを構築するために必要な要素をドラッグします ( `UILabels`や`UIImageViews`など)。

> [!NOTE]
> IOS 12 の場合、通知コンテンツの拡張機能には、ボタンやテキストフィールドなどの対話型のコントロールを含めることができます。 詳細については、 [iOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md)ドキュメントの「対話型通知」を参照してください。

Ui がレイアウトされ、必要なコントロールがコードにC#公開されたら、 `NotificationViewController.cs`を編集するために`DidReceiveNotification`を開き、ユーザーが通知を展開するときに ui を設定するようにメソッドを変更します。 例えば:

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

### <a name="setting-the-content-area-size"></a>コンテンツ領域のサイズを設定する

ユーザーに表示されるコンテンツ領域のサイズを調整するには、次のコードで`PreferredContentSize`は、 `ViewDidLoad`メソッドのプロパティを目的のサイズに設定しています。 このサイズは、iOS デザイナーのビューに制約を適用することによって調整することもできます。開発者は、最適な方法を選択します。

通知コンテンツの拡張機能が呼び出される前に通知システムが既に実行されているため、コンテンツ領域は完全にサイズが変更され、ユーザーに表示されると要求されたサイズまでアニメーション化されます。

この影響をなくすには、 `Info.plist`拡張機能のファイルを編集し`UNNotificationExtensionInitialContentSizeRatio` 、 `NSExtensionAttributes`キーのキーに、目的の比率を表す値**を入力し**ます。 例えば:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio キー")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio キー")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>カスタム UI でのメディア添付ファイルの使用

メディア添付ファイル (上記の「[メディア添付ファイルの追加](#adding-media-attachments)」セクションを参照) は通知ペイロードの一部であるため、既定の通知 UI の場合と同様に、通知コンテンツの拡張機能にアクセスして表示できます。

たとえば、上のカスタム UI に、 `UIImageView`コードにC#公開されたが含まれている場合、次のコードを使用して、メディアの添付ファイルをに読み込むことができます。

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

メディア添付ファイルはシステムによって管理されているため、アプリのサンドボックスの外部にあります。 拡張機能は、 `StartAccessingSecurityScopedResource`メソッドを呼び出すことによって、ファイルへのアクセスが必要であることをシステムに通知する必要があります。 ファイルを使用して拡張機能を実行する場合は、を`StopAccessingSecurityScopedResource`呼び出して、その接続を解放する必要があります。

### <a name="adding-custom-actions-to-a-custom-ui"></a>カスタム UI へのカスタムアクションの追加

カスタムアクションボタンを使用すると、カスタム通知 UI にインタラクティビティを追加できます。 カスタムアクションの詳細については、[拡張ユーザー通知](~/ios/platform/user-notifications/enhanced-user-notifications.md)に関するドキュメントの「[通知アクション](~/ios/platform/user-notifications/enhanced-user-notifications.md)の使用」セクションを参照してください。

カスタムアクションに加えて、通知コンテンツの拡張機能は、次の組み込みアクションにも応答できます。

- **既定のアクション**-ユーザーが通知をタップしてアプリを開き、指定された通知の詳細を表示します。
- **アクションを無視**する-このアクションは、ユーザーが特定の通知を終了したときにアプリに送信されます。

また、通知コンテンツの拡張機能では、ユーザーがカスタムアクションのいずれかを実行したときに UI を更新することもできます。たとえば、ユーザーが [カスタムアクションを**受け入れる**] ボタンをタップしたときに受理された日付を表示できます。 また、通知コンテンツの拡張機能は、通知を閉じる前にユーザーがアクションの効果を確認できるように、通知 UI の無視を遅らせるようにシステムに指示することができます。

これを行うには、完了ハンドラーを含む`DidReceiveNotification` 2 番目のバージョンのメソッドを実装します。 例えば:

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

通知コンテンツ拡張`Server.PostEventResponse`機能の`DidReceiveNotification`メソッドにハンドラーを追加することで、拡張機能はすべてのカスタムアクションを処理*する必要があり*ます。 拡張機能では、 `UNNotificationContentExtensionResponseOption`を変更することによって、それを含むアプリにカスタムアクションを転送することもできます。 例えば:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>カスタム UI でのテキスト入力アクションの操作

アプリの設計と通知によっては、ユーザーが通知にテキストを入力する必要がある場合があります (メッセージへの返信など)。 通知コンテンツの拡張機能は、標準の通知と同様に、組み込みのテキスト入力アクションにアクセスできます。

例えば:

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

このコードは、新しいテキスト入力アクションを作成し、拡張機能のカテゴリ ( `MakeExtensionCategory`) メソッドに追加します。 `DidReceive`オーバーライドメソッドでは、次のコードを使用してテキストを入力するユーザーを処理します。

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

テキスト入力フィールドにカスタムボタンを追加するためにデザインでを呼び出す場合は、次のコードを追加して追加します。

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

ユーザーによってコメントアクションがトリガーされた場合は、ビューコントローラーとカスタムテキスト入力フィールドの両方をアクティブにする必要があります。

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

## <a name="summary"></a>Summary

この記事では、Xamarin iOS アプリでの新しいユーザー通知フレームワークの使用について詳しく説明しました。 ここでは、ローカルとリモートの両方の通知にメディア添付ファイルを追加し、新しいユーザー通知 UI を使用してカスタム通知 ui を作成する方法について説明しています。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [UserNotifications フレームワークリファレンス](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [ローカルおよびリモートの通知プログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
