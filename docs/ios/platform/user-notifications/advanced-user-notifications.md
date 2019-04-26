---
title: Xamarin.iOS での高度なユーザー通知
description: この記事では、iOS 10 で導入された、ユーザー通知フレームワークについて詳しく説明します。 これは、ユーザーへの通知、ユーザー通知のインターフェイス、メディア添付ファイル、カスタム ユーザー インターフェイス、および詳細について説明します。
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 4472654064812142e3281374754ace0042b542bf
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61089249"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Xamarin.iOS での高度なユーザー通知

新しい ios 10 では、ユーザー通知の配信とローカルとリモート通知の処理のフレームワークを使用します。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカル通知の配信の場所などの条件のセットまたは 1 日の時刻を指定することで。

## <a name="about-user-notifications"></a>ユーザーへの通知について

新しいユーザー通知フレームワークの配信とローカルとリモート通知の処理できます。 このフレームワークを使用して、アプリまたはアプリ拡張機能をスケジュールできますローカル通知の配信の場所などの条件のセットまたは 1 日の時刻を指定することで。

さらに、アプリまたは拡張機能が表示される (および可能性のある変更できます) ローカルとリモートの両方の通知、ユーザーの iOS デバイスに配信されるようにします。

新しいユーザー通知の UI フレームワークは、アプリまたはアプリの拡張機能をユーザーに表示するときに、ローカルとリモートの両方の通知の外観をカスタマイズできます。

このフレームワークは、アプリはユーザーに通知を配信することができます、次の方法を提供します。

- **Visual アラート**- バナーとして画面の一番上から下、通知ロール場所。
- **サウンドと振動**-通知を関連付けることができます。
- **アプリ アイコンのバッジ取得**- 場合、アプリのアイコンは、新しいコンテンツが利用可能なであることを示すバッジを表示します。 など、未読の電子メール メッセージの数。

さらに、ユーザーの現在のコンテキストによっては、通知が表示されるさまざまな方法があります。

- デバイスがロックされている場合、通知が転がり落ちて、画面の上部からバナーとして。
- デバイスがロックされている場合は、ユーザーのロック画面に通知が表示されます。
- ユーザーは通知に失敗しました、通知センターを開き、でき、使用可能な待機通知が表示できます。

Xamarin.iOS アプリでは、2 種類のユーザー通知を送信することですがあります。

- **ローカル通知**-これらは、ユーザーのデバイスでローカルにインストールされているアプリによって送信されます。
- **リモート通知**- リモート サーバーと、ユーザーに表示されるいずれかから送信するか、アプリのコンテンツのバック グラウンド更新をトリガーします。

詳細についてを参照してください、[ユーザー通知の強化された](~/ios/platform/user-notifications/enhanced-user-notifications.md)ドキュメント。

## <a name="the-new-user-notification-interface"></a>新しいユーザー通知インターフェイス

または、通知センターで、デバイスの上部にバナーとして ロック画面で、タイトル、サブタイトル、およびことができる、省略可能なメディア添付ファイルなどのより多くのコンテンツを提供する新しい UI デザイン iOS 10 でのユーザー通知が表示されます。

ユーザー通知が iOS 10 で表示される場所に関係なく同じルック アンド フィールと同じ機能と機能で表示されます。

Ios 8 では、Apple は開発者でした、通知にカスタム動作をアタッチし、アプリを起動しなくても、通知に対処するユーザーを許可する場所の実践的な通知を導入しました。 Ios 9 では、Apple には、ユーザーが通知され、テキスト入力に応答する応答がクイック実行可能な通知が強化されています。

Apple が、通知を押すし、カスタム ユーザー インターフェイスは、豊富な対話を提供するためにディスプレイの 3D タッチをサポートするために実践的な通知をさらに拡張ユーザー通知は iOS 10 でのユーザー エクスペリエンスの詳細に不可欠であるため、通知します。

カスタム ユーザー通知の UI が表示されたら、ユーザーが通知に関連付けられたすべてのアクションとやり取りする場合の変更点に関するフィードバックを提供するカスタム UI をすぐに更新できます。

新しいユーザー通知の UI API を iOS 10 では、Xamarin.iOS アプリを簡単にこれらのユーザー通知の UI の新機能の利用ができます。

## <a name="adding-media-attachments"></a>メディア添付ファイルの追加

ユーザー間で共有を取得する一般的な項目の 1 つは、写真を示され、通知の conte の残りの部分と共にユーザーにすぐに利用する、通知を直接ため、iOS 10 (写真) などのメディア アイテムをアタッチする機能を追加します。nt します。

ただし、送信に関連するサイズでも、小さいイメージがリモート通知ペイロードにアタッチに実用的でないなります。 このような状況を処理するには、開発者、新しいサービス拡張機能を使用で iOS 10 (CloudKit データストア) などの別のソースからイメージをダウンロードして、ユーザーに表示されるまで、通知のコンテンツにアタッチします。

サービスの拡張機能を使用して変更するリモート通知の場合は、そのペイロードを変更可能としてマークにする必要があります。 例:

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

[![](advanced-user-notifications-images/extension02.png "追加のメディア添付ファイルの処理")](advanced-user-notifications-images/extension02.png#lightbox)

サービス拡張機能が必要な手段を使用して必要なイメージをダウンロードできますし (APNs) を使用してデバイスにリモート通知が配信されると (など、 `NSURLSession`) および表示、通知の内容を変更できる、イメージを受信した後そのユーザーにします。

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

APNs から通知が受信されるは、イメージのカスタムのアドレスは、コンテンツから読み取られ、サーバーからファイルをダウンロードします。 `UNNotificationAttachement`は一意の ID と、イメージのローカルの場所で作成されます (として、 `NSUrl`)。 Notification Content の変更可能なコピーが作成され、メディア添付ファイルが追加されます。 最後に通知が呼び出すことによって、ユーザーに表示される、`contentHandler`します。

添付ファイルは、通知に追加されると、システムは、移動やファイルの管理によって引き継がれます。

ローカルの通知のサポートもメディア添付ファイルだけでなく、リモート通知は、上で示した、場所、`UNNotificationAttachement`が作成され、そのコンテンツと共に通知にアタッチされています。

IOS 10 での通知をサポートしてイメージのメディア添付ファイル (静的および Gif)、オーディオまたはビデオおよびシステム自動的に UI が表示されます、適切なカスタムのそれぞれの添付ファイルの種類をユーザーに通知が表示されます。

> [!NOTE]
> 注意する必要があるメディア サイズを最適化して、システムとリモート サーバーからメディアをダウンロードする (またはローカル通知用のメディアを構成する) のにかかる時間でアプリのサービスの拡張機能を実行するときに両方の厳密な制限が課せられます。 たとえば、画像のスケール ダウン バージョンまたは、通知に表示するビデオの小さなクリップを送信します。

## <a name="creating-custom-user-interfaces"></a>カスタム ユーザー インターフェイスの作成

ユーザー通知のカスタム ユーザー インターフェイスを作成するには、開発者は、アプリのソリューションに、Notification Content 拡張機能 (新しい iOS 10 に) を追加する必要があります。

通知のコンテンツの拡張機能には、通知 UI に独自のビューを追加し、必要とする任意のコンテンツを描画する開発者が使用できます。 IOS 12 以降では、通知のコンテンツの拡張機能は、ボタンとスライダーなどの対話型の UI コントロールをサポートします。 詳細については、次を参照してください。、 [iOS 12 で対話型通知](~/ios/platform/introduction-to-ios12/notifications/interactive.md)ドキュメント。

ユーザー通知でユーザーの操作をサポートするためにカスタム アクションが作成、システムに登録され、システムのようにスケジュールされて前に、通知にアタッチされています。 Notification Content 拡張機能は、これらのアクションの処理を処理するために呼び出されます。 参照してください、[通知アクションの操作](~/ios/platform/user-notifications/enhanced-user-notifications.md)のセクション、[ユーザー通知の強化された](~/ios/platform/user-notifications/enhanced-user-notifications.md)カスタム動作の詳細についてはドキュメントです。

カスタム UI を使用したユーザーの通知がユーザーに表示されるときに、次の要素になります。

[![](advanced-user-notifications-images/customui01.png "カスタム UI 要素を持つユーザー通知")](advanced-user-notifications-images/customui01.png#lightbox)

ユーザーは、カスタム アクション (通知の下に表示) とやり取りする場合、として、特定のアクションを呼び出すことしたらどうなるか、ユーザーのフィードバックを提供するユーザー インターフェイスを更新できます。

### <a name="adding-a-notification-content-extension"></a>Notification content の拡張機能を追加します。

Xamarin.iOS アプリでカスタムのユーザー通知 UI を実装するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual studio for mac アプリのソリューションを開きます
2. ソリューション名を右クリックし、 **Solution Pad**選択**追加** > **新しいプロジェクトの追加**します。
3. 選択**iOS** > **拡張機能** > **Notification Content 拡張機能** をクリックし、**次**ボタン。 

    [![](advanced-user-notifications-images/notify01.png "Notification Content 拡張機能を選択します。")](advanced-user-notifications-images/notify01.png#lightbox)
4. 入力、**名前**拡張機能をクリックして、**次**ボタン。 

    [![](advanced-user-notifications-images/notify02.png "拡張機能の名前を入力します")](advanced-user-notifications-images/notify02.png#lightbox)
5. 調整、**プロジェクト名**や**ソリューション名**ために必要なクリックすると、**作成**ボタン。 

    [![](advanced-user-notifications-images/notify03.png "プロジェクト名またはソリューション名を調整します。")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual studio for mac アプリのソリューションを開きます
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加 > 新しいプロジェクト.**.
3. 選択**Visual C# > iOS 拡張機能 > Notification Content 拡張機能**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "Notification Content 拡張機能を選択します。")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. 入力、**名前**拡張機能をクリックして、 **OK**ボタン。

-----

Notification Content 拡張機能は、ソリューションに追加するときに、拡張機能のプロジェクトで 3 つのファイルが作成されます。

1. `NotificationViewController.cs` -これは、Notification Content 拡張機能のメイン ビュー コント ローラーです。
2. `MainInterface.storyboard` -場所、開発者は iOS Designer の Notification Content 拡張機能に表示される UI をレイアウトします。
3. `Info.plist` -Notification Content 拡張機能の構成を制御します。

既定の`NotificationViewController.cs`ファイルは、次のようにします。

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

`DidReceiveNotification`メソッドは、通知が Notification Content 拡張機能の内容にカスタム UI を設定できるように、ユーザーが展開されているときに呼び出されますが、`UNNotification`します。 上記の例では、名前のコードに公開 ビューにラベルが追加されました`label`通知の本文を表示するために使用します。

### <a name="setting-the-notification-content-extensions-categories"></a>Notification Content 拡張機能のカテゴリの設定

システムに応答する特定のカテゴリに基づいて、アプリの Notification Content 拡張機能を検索する方法を通知する必要があります。 次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 拡張機能のダブルクリック`Info.plist`ファイル、 **Solution Pad**編集用に開きます。
2. 切り替えて、**ソース**ビュー。
3. 展開、`NSExtension`キー。
4. 追加、`UNNotificationExtensionCategory`キーの種類として**文字列**拡張機能が属するカテゴリの値 (この例では ' イベント招待)。 

    [![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory キーを追加します。")](advanced-user-notifications-images/customui02.png#lightbox)
5. 変更内容を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 拡張機能のダブルクリック`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
3. 展開、`NSExtension`キー。
4. 追加、`UNNotificationExtensionCategory`キーの種類として**文字列**拡張機能が属するカテゴリの値 (この例では ' イベント招待)。 

    [![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory キーを追加します。")](advanced-user-notifications-images/customui02w.png#lightbox)
5. 変更内容を保存します。

-----

通知のコンテンツの拡張機能カテゴリ (`UNNotificationExtensionCategory`) 通知アクションの登録に使用する同じカテゴリの値を使用します。 アプリの複数のカテゴリと同じ UI を使用できる場所の場合、切り替え、`UNNotificationExtensionCategory`型に**配列**しすべての必要なカテゴリを指定します。 例:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui03.png "Notification Content 拡張機能のカテゴリ")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui03w.png "Notification Content 拡張機能のカテゴリ")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>既定の通知のコンテンツを非表示

カスタム通知の UI を表示、同じコンテンツ (タイトル、サブタイトル、および本文が自動的に表示される通知の UI の下部にある) 通知の既定値としての状況で、を追加することでこの既定の情報を隠すことが`UNNotificationExtensionDefaultContentHidden`キーを`NSExtensionAttributes`キーの種類として**ブール**の値を持つ`YES`で拡張機能の`Info.plist`ファイル。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui04.png "既定の情報を検索します。")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui04w.png "既定の情報を検索します。")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>カスタム UI の設計

Notification Content 拡張機能のカスタム ユーザー インターフェイスを設計するには、ダブルクリック、`MainInterface.storyboard`ファイルを開き、iOS デザイナーで編集が必要なインターフェイスを構築する必要のある要素のドラッグ (など`UILabels`と`UIImageViews`)。

> [!NOTE]
> Notification Content の拡張機能は 12、iOS の時点で、ボタンやテキスト フィールドなどの対話型のコントロールを含めることができます。 詳細については、次を参照してください。、 [iOS 12 で対話型通知](~/ios/platform/introduction-to-ios12/notifications/interactive.md)ドキュメント。

UI のレイアウトされているしに必要な制御が公開されるC#コード、開いている、`NotificationViewController.cs`を編集するため変更と、`DidReceiveNotification`ユーザー通知を展開する場合に、UI を設定するメソッド。 例:

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

ユーザーに表示されるコンテンツ エリアのサイズを調整するには、次のコードを設定、`PreferredContentSize`プロパティ、`ViewDidLoad`メソッドは目的のサイズにします。 このサイズは、iOS Designer のビューに制約を適用することによっても調整される可能性があります、開発者はそれらの最適な方法を選択します。

最初に、通知システムが既にコンテンツの拡張が呼び出されると、通知、コンテンツ領域の前に実行されているため、フル インストール オプションのサイズし、ユーザーに表示されるときに、要求されたサイズまでアニメーション化します。

この効果をなくすため、編集、`Info.plist`拡張機能とセットのファイル、`UNNotificationExtensionInitialContentSizeRatio`のキー、`NSExtensionAttributes`キーを入力する**数**で目的の比率を表す値。 例:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio キー")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio キー")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>カスタムの UI でのメディア添付ファイルの使用

ため、メディア添付ファイル (に示すよう、[メディア添付ファイルの追加](#adding-media-attachments)前のセクション) 通知のペイロードの一部である、アクセスし既定値の場合と同様に、Notification Content 拡張機能に表示されることができますUI の通知。

たとえば、上記のカスタム UI が含まれている場合、`UIImageView`に公開されたでC#、次のコードからメディア添付ファイルを設定するコードを使用します。

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

メディア添付ファイルは、システムによって管理されるため、アプリのサンド ボックスの外部では。 拡張機能を呼び出すことによって、ファイルへのアクセスを求めていることをシステムに通知する必要があります、`StartAccessingSecurityScopedResource`メソッド。 呼び出す必要が、拡張機能では、ファイル処理が終わったら、`StopAccessingSecurityScopedResource`の接続を解放します。

### <a name="adding-custom-actions-to-a-custom-ui"></a>カスタム UI へのカスタム アクションの追加

カスタム通知の UI にインタラクティビティを追加するカスタム アクション ボタンを使用できます。 参照してください、[通知アクションの操作](~/ios/platform/user-notifications/enhanced-user-notifications.md)のセクション、[ユーザー通知の強化された](~/ios/platform/user-notifications/enhanced-user-notifications.md)カスタム アクションの詳細についてはドキュメントです。

カスタムの動作だけでなく、Notification Content 拡張機能は、次の組み込みアクションもに応答できます。

- **既定のアクションが**-これは、ユーザーがアプリを開き、特定の通知の詳細を表示する通知をタップします。
- **アクションを破棄**-ユーザーが特定の通知を閉じるときにこのアクションは、アプリに送信されます。

Notification Content 拡張機能は、ユーザーがカスタム アクションのいずれかを呼び出したときに、UI を更新する機能もある、ユーザーがタップしたときに承諾済みとしての日付を表示するなど、 **Accept**カスタム アクション ボタンをクリックします。 さらに、Notification Content 拡張機能は、システムに通知を閉じる前に、ユーザーはその操作の効果を確認できるように、通知の UI のジョンソンを遅延するを指示することができます。

2 番目のバージョンを実装することによってこれは、`DidReceiveNotification`完了ハンドラーを含むメソッド。 例えば:

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

追加することで、`Server.PostEventResponse`ハンドラーを`DidReceiveNotification`メソッドは、Notification Content 拡張機能、拡張機能の*する必要があります*すべてのカスタム アクションを処理します。 拡張機能は含むアプリにカスタム アクションを変更することで転送できますも、`UNNotificationContentExtensionResponseOption`します。 例えば:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>カスタムの UI でのテキスト入力のアクションの使用

によって、アプリと通知のデザイン、(メッセージに返信する) などの通知にテキストを入力する必要がある回あります。 Notification Content の拡張機能は、標準的な通知は同様に、組み込みのテキスト入力のアクションへのアクセスが。

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

このコードは、新しいテキスト入力アクションを作成し、拡張機能のカテゴリに追加します (で、 `MakeExtensionCategory`) メソッドです。 `DidReceive`メソッドをオーバーライドを次のコードのテキストを入力するユーザーを処理します。

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

テキスト入力 フィールドにカスタム ボタンを追加する場合、設計に含めるには、次のコードを追加します。

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

ユーザーがコメントのアクションがトリガーされると、ビュー コント ローラーとカスタム テキスト入力フィールドの両方をアクティブにする必要があります。

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

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリで新しいユーザー通知フレームワークを使用して、高度な説明にしました。 ローカルとリモート通知の両方に、メディア添付ファイルを追加することを説明し、新しいユーザー通知の UI を使用してカスタム通知の Ui を作成することについて説明します。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications フレームワーク参照](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [ローカルとリモート通知プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
