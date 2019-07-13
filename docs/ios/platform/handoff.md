---
title: Xamarin.iOS でハンドオフ
description: この記事では、ハンドオフを転送する Xamarin.iOS アプリで操作を内部では、ユーザーで実行されているアプリ間でのユーザー アクティビティの他のデバイス。
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 084b9924af467459a017413a958ec2e46ff219fc
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865305"
---
# <a name="handoff-in-xamarinios"></a>Xamarin.iOS でハンドオフ

_この記事では、ハンドオフを転送する Xamarin.iOS アプリで操作を内部では、ユーザーで実行されているアプリ間でのユーザー アクティビティの他のデバイス。_

Apple は、同じアプリまたは同じアクティビティをサポートする別のアプリを実行している別のデバイスに iOS 8、およびユーザーが自分のデバイスのいずれかで開始されたアクティビティに転送の一般的なメカニズムを提供する OS X Yosemite (10.10) ハンドオフを導入しました。

[![](handoff-images/handoff02.png "ハンドオフ操作を実行する例")](handoff-images/handoff02.png#lightbox)

この記事では、簡単に見て Xamarin.iOS アプリで共有アクティビティの有効化し、ハンドオフ フレームワークの詳細を説明します。

## <a name="about-handoff"></a>ハンドオフについて

ハンドオフ (継続性とも呼ばれます) は、Apple ios 8 および OS X Yosemite (10.10) でユーザーが自分のデバイス (iOS または Mac) のいずれかのアクティビティを開始して、同じアクティビティに続行 (ユーザーの iClou で識別されるデバイスのもう 1 つの方法として導入されました。d アカウント)。

ハンドオフは、強化された検索機能を iOS 9 でも、新しいサポートで拡張されました。 詳細についてを参照してください、[検索の機能強化](~/ios/platform/search/index.md)ドキュメント。

など、ユーザーは、iPhone でメールを開始し、すべての入力と同じメッセージの情報と iOS で残っているのと同じ場所にカーソルを置き、Mac 上でシームレスに電子メールを引き続き。

同じを共有するアプリのいずれかの_チーム ID_ (for Mac Enterprise は、iTunes App Store を介して配信ハンドオフを使用してこれらのアプリがいずれかのアプリ間でのユーザー アクティビティを続行するには、対象または開発者として登録して署名または アドホック アプリ) で変更します。

すべて`NSDocument`または`UIDocument`ベースのアプリに自動的にハンドオフは、組み込みのサポートし、ハンドオフをサポートするために最小限の変更が必要があります。

### <a name="continuing-user-activities"></a>継続のユーザー アクティビティ

`NSUserActivity`クラス (にいくつかの小さな変更と共に`UIKit`と`AppKit`) ユーザーのデバイスのもう 1 つを継続できる可能性のあるユーザーのアクティビティを定義するためのサポートを提供します。

別のユーザーのデバイス経由で渡されるアクティビティで、カプセル化するインスタンスで`NSUserActivity`、としてマークされた、_現在のアクティビティ_セットを持つのペイロード (データの継続を実行するために使用)、アクティビティは、そのデバイスに送信する必要があります。

ICloud を使用して同期されている、大きなデータ パケットでは、継続するアクティビティを定義する情報の最低限をハンドオフに渡します。

受信側のデバイスでは、アクティビティが継続できることの通知がユーザーに届きます。 (実行中でない場合、指定したアプリが起動された場合は、ユーザーのアクティビティを新しいデバイスで続行を選択およびからペイロード、`NSUserActivity`アクティビティを再起動するために使用します。

[![](handoff-images/handoffinteractions.png "継続のユーザー アクティビティの概要")](handoff-images/handoffinteractions.png#lightbox)

同じ開発者チーム ID を共有し、応答するアプリのみを指定した_アクティビティの種類_は、継続の対象。 アプリでサポートされているアクティビティの種類の定義、`NSUserActivityTypes`のキーの**Info.plist**ファイル。 継続的デバイス、チーム ID、アクティビティの種類に基づく継続を実行するアプリを選択し、必要に応じて、_活動のタイトル_します。

受信側のアプリからの情報を使用して、`NSUserActivity`の`UserInfo`ユーザー インターフェイスを構成し、遷移がエンドユーザーにシームレスに表示されるように指定したアクティビティの状態を復元するためのディクショナリ。

継続をかどうかにより効率的に送信できるより多くの情報が必要です、`NSUserActivity`呼び出し元のアプリを送信し、必要なデータを転送する 1 つまたは複数のストリームを確立するアプリを再開することができます。 たとえば、アクティビティが複数のイメージを持つ大きなテキスト ドキュメントを編集していた場合ストリーミング必要になります、受信側のデバイスで、アクティビティを続行するために必要な情報を転送します。 詳細については、次を参照してください。、[サポート継続ストリーム](#supporting-continuation-streams)以下のセクション。

上記で説明したように`NSDocument`または`UIDocument`ベースのアプリに自動的にハンドオフは組み込みのサポートがあります。 詳細については、次を参照してください。、[ドキュメント ベース アプリでのサポートのハンドオフ](#supporting-handoff-in-document-based-apps)以下のセクション。

### <a name="the-nsuseractivity-class"></a>NSUserActivity クラス

`NSUserActivity`クラスはハンドオフ exchange でプライマリ オブジェクトであり、継続で利用可能なユーザー アクティビティの状態をカプセル化するために使用します。 アプリでのコピーがインスタンス化`NSUserActivity`の任意のアクティビティをサポートし、別のデバイスで続行することを希望します。 たとえば、ドキュメント エディターは活動を作成、各ドキュメントの現在開いています。 ただし、(最前面のウィンドウまたはタブに表示されます)、最前面のドキュメントのみが、_現在のアクティビティ_継続されるためご利用いただけます。

インスタンス`NSUserActivity`両方で識別されるその`ActivityType`と`Title`プロパティ。 `UserInfo`ディクショナリ プロパティは、アクティビティの状態に関する情報を実行するために使用します。 設定、`NeedsSave`プロパティを`true`に遅延する場合を使用して状態情報を読み込む、 `NSUserActivity`'s を委任します。 使用して、`AddUserInfoEntries`に他のクライアントからの新しいデータをマージする方法、`UserInfo`ディクショナリ アクティビティの状態を保持するためにします。

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate クラス

`NSUserActivityDelegate`情報を保持するために使用、`NSUserActivity`の`UserInfo`ディクショナリの最新の状態と、アクティビティの現在の状態と同期します。 システムが更新されるアクティビティの情報が必要な場合 (など別のデバイスで継続する前に) を呼び出し、`UserActivityWillSave`デリゲートのメソッド。

実装する必要があります、`UserActivityWillSave`メソッドとする変更が加えられる、 `NSUserActivity` (など`UserInfo`、`Title`など) を現在のアクティビティの状態を反映することを確認します。 システムを呼び出すと、`UserActivityWillSave`メソッド、`NeedsSave`フラグがクリアされます。 アクティビティのデータのプロパティを変更する場合は、設定する必要あります`NeedsSave`に`true`もう一度です。

使用する代わりに、`UserActivityWillSave`メソッド上で示した、させることもできます`UIKit`または`AppKit`ユーザーのアクティビティを自動的に管理します。 これを行うには、設定、応答側オブジェクトの`UserActivity`プロパティと実装、`UpdateUserActivityState`メソッド。 参照してください、[レスポンダーでサポートしているハンドオフ](#supporting-handoff-in-responders)詳細については後述します。

### <a name="app-framework-support"></a>アプリのフレームワークのサポート

両方`UIKit`(iOS) および`AppKit`(OS X) のハンドオフの組み込みサポートを提供する、 `NSDocument`、レスポンダー (`UIResponder`/`NSResponder`)、および`AppDelegate`クラス。 各 OS では、わずかに異なるハンドオフを実装するときに、基本的なメカニズムと Api は、同じです。

#### <a name="user-activities-in-document-based-apps"></a>ドキュメント ベースのアプリ内のユーザー アクティビティ

ドキュメント ベースの iOS および OS X アプリに自動的に組み込みのハンドオフ サポートがあります。 このサポートを有効にするには、追加する必要があります、`NSUbiquitousDocumentUserActivityType`の各キーと値`CFBundleDocumentTypes`アプリのエントリ**Info.plist**ファイル。

このキーが存在する場合両方`NSDocument`と`UIDocument`を自動的に作成`NSUserActivity`指定された型の iCloud ベースのドキュメントのインスタンス。 アプリをサポートするドキュメントの種類ごとのアクティビティの種類を指定する必要があり、複数のドキュメント タイプは、同じアクティビティの種類を使用できます。 両方`NSDocument`と`UIDocument`を自動的に設定、`UserInfo`のプロパティ、`NSUserActivity`でその`FileURL`プロパティの値。

OS X 上、`NSUserActivity`によって管理される`AppKit`レスポンダーに自動的に関連付けられていると、ドキュメントのウィンドウ、メイン ウィンドウになったときに、現在のアクティビティになります。 Ios では、`NSUserActivity`によって管理されるオブジェクト`UIKit`、いずれかの呼び出しをする必要があります`BecomeCurrent`メソッドに明示的にまたはドキュメントの`UserActivity`プロパティ セットを`UIViewController`ときに、アプリが前面に表示します。

`AppKit` いずれかを自動的に復元`UserActivity`OS X では、この方法で作成されたプロパティ。これは、場合に発生、`ContinueUserActivity`メソッドを返します。`false`実装されていない場合またはします。 このような状況では、ドキュメントが開かれて、`OpenDocument`のメソッド、`NSDocumentController`しを受信して、`RestoreUserActivityState`メソッドの呼び出し。

参照してください、[ドキュメント ベース アプリでのサポートのハンドオフ](#supporting-handoff-in-document-based-apps)詳細については後述します。

#### <a name="user-activities-and-responders"></a>ユーザー アクティビティとレスポンダー

両方`UIKit`と`AppKit`レスポンダー オブジェクトのとして設定した場合に、ユーザー アクティビティを自動的に管理できます`UserActivity`プロパティ。 設定する必要があります、状態が変更された場合、`NeedsSave`のレスポンダーのプロパティ`UserActivity`に`true`します。 システムは自動的に保存、`UserActivity`レスポンダー時間を呼び出すことによって、状態の更新に与えた後、必要な場合にその`UpdateUserActivityState`メソッド。

複数のレスポンダーは、1 つを共有している場合`NSUserActivity`受信する、インスタンス、`UpdateUserActivityState`システムは、ユーザーのアクティビティ オブジェクトを更新する場合にコールバックします。 応答側を呼び出す必要がある、`AddUserInfoEntries`に更新する方法、`NSUserActivity`の`UserInfo`この時点で現在のアクティビティの状態を反映するためのディクショナリ。 `UserInfo`それぞれ行う前にディクショナリがオフになって`UpdateUserActivityState`呼び出します。

アクティビティから自体の関連付けを解除、応答側が設定できるその`UserActivity`プロパティを`null`します。 アプリケーションのフレームワークが管理されている`NSUserActivity`インスタンスには、関連付けられているレスポンダーよりまたはドキュメントがない、自動的に検証済みではありません。

参照してください、[レスポンダーでサポートしているハンドオフ](#supporting-handoff-in-responders)詳細については後述します。

#### <a name="user-activities-and-the-appdelegate"></a>ユーザーのアクティビティと、AppDelegate

アプリの`AppDelegate`ハンドオフ継続を処理するときに、その主なエントリ ポイントが。 ときに、ユーザーは通知に応答をハンドオフ、適切なアプリを起動 (既に実行されていない場合)、`WillContinueUserActivityWithType`のメソッド、`AppDelegate`が呼び出されます。 この時点で、アプリは、継続が開始されるユーザーに通知する必要があります。

`NSUserActivity`インスタンスが配信される、`AppDelegate`の`ContinueUserActivity`メソッドが呼び出されます。 この時点では、アプリのユーザー インターフェイスを構成し、特定のアクティビティを続行する必要があります。

参照してください、[実装ハンドオフ](#implementing-handoff)詳細については後述します。

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin アプリでのハンドオフを有効にします。

Handoff によって課されるセキュリティ要件、によりハンドオフ フレームワークを使用する Xamarin.iOS アプリをする必要があります適切に構成する、Apple Developer ポータルと Xamarin.iOS プロジェクト ファイル。

次の手順で行います。

1. ログイン、 [Apple Developer Portal](https://developer.apple.com)します。
2. をクリックして**証明書, Identifiers & Profiles**します。
3. これをいない場合は、をクリックして**識別子**アプリの ID を作成し、(例: `com.company.appname`)、それ以外の場合、既存の ID を編集
4. いることを確認、 **iCloud**サービスは、指定した ID のチェックが完了します。

    [![](handoff-images/provision01.png "指定した ID の iCloud サービスを有効にします。")](handoff-images/provision01.png#lightbox)
5. 変更内容を保存します。
6. をクリックして**Provisioning Profiles** > **開発**とアプリを作成するには、新しい開発のプロビジョニング プロファイル。

    [![](handoff-images/provision02.png "新しい開発プロビジョニング プロファイル、アプリの作成します。")](handoff-images/provision02.png#lightbox)
7. ダウンロードして、新しいプロビジョニング プロファイルをインストールするまたは、Xcode を使用してダウンロードし、プロファイルをインストールしています。
8. Xamarin.iOS プロジェクトのオプションを編集し、先ほど作成したプロビジョニング プロファイルを使用していることを確認します。

    [![](handoff-images/provision03.png "先ほど作成したプロビジョニング プロファイルを選択します。")](handoff-images/provision03.png#lightbox)
9. 次に、編集、 **Info.plist**ファイルし、プロビジョニング プロファイルの作成に使用されたアプリ ID を使用していることを確認します。

    [![](handoff-images/provision04.png "アプリ ID を設定します。")](handoff-images/provision04.png#lightbox)
10. スクロールして、**バック グラウンド モード**セクションし、次のものを確認してください。

    [![](handoff-images/provision05.png "必要なバック グラウンド モードを有効にします。")](handoff-images/provision05.png#lightbox)
11. すべてのファイルに変更を保存します。

これら設定した状態でのアプリケーションがハンドオフ フレームワークの Api にアクセスする準備ができてようになりました。 プロビジョニングの詳細についてを参照してください、 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)と[アプリのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド。

## <a name="implementing-handoff"></a>Handoff を実装します。

同じ開発者チーム ID で署名され、同じアクティビティの種類をサポートするアプリ間でユーザーの作業を続行できます。 ユーザー アクティビティのオブジェクトを作成する Xamarin.iOS アプリでハンドオフを実装する必要があります (いずれかで`UIKit`または`AppKit`)、アクティビティを追跡するために、オブジェクトの状態を更新し、受信側のデバイスで、アクティビティを継続します。

### <a name="identifying-user-activities"></a>ユーザー アクティビティを識別します。

ハンドオフの実装の最初の手順は、アプリをサポートするユーザー アクティビティの種類を識別するためには、別のデバイスでの継続の候補はそれらのアクティビティの表示. 例: ToDo アプリは、1 つとして項目を編集をサポート可能性があります_ユーザー アクティビティの種類_、および別の利用可能な項目の一覧の参照をサポートします。

アプリでは、多くユーザー アクティビティの種類は、必要なアプリを提供する任意の関数のいずれかを作成できます。 各ユーザー アクティビティの種類、アプリは、型のアクティビティが開始されると、終了し、別のデバイスでそのタスクを続行する最新の状態情報を保持する必要がありますを追跡する必要があります。

送信側と受信側のアプリ間で一対一のマッピングがない同じチーム ID で署名されたすべてのアプリでは、ユーザー アクティビティを続行できます。 たとえば、特定のアプリは、別のデバイスで異なる、個々 のアプリで使用されるアクティビティの 4 つのさまざまな種類を作成できます。 これは、各アプリが小さいと、特定のタスクに焦点は (多くの機能と機能があります) をアプリの Mac のバージョンと iOS のアプリ間で頻繁に発生します。

### <a name="creating-activity-type-identifiers"></a>アクティビティの種類の識別子を作成します。

_アクティビティ型識別子_に短い文字列が追加、`NSUserActivityTypes`のアプリの配列**Info.plist**ファイルが指定したユーザー アクティビティの種類を一意に識別するために使用します。 配列内のアプリをサポートする各アクティビティの 1 つのエントリがあります。 Apple では、アクティビティの型識別子の逆引き DNS スタイルの表記を使用して競合を避けるためお勧めします。 例:`com.company-name.appname.activity`特定のアプリ ベースのアクティビティまたは`com.company-name.activity`の複数のアプリで実行できるアクティビティ。

作成するときに、アクティビティの型識別子が使用される、`NSUserActivity`アクティビティの種類を識別するインスタンス。 アクティビティを別のデバイスで続行すると、(アプリのチーム ID) と共に、アクティビティの種類は、アクティビティの続行を起動するには、どのアプリを決定します。

たとえば、というサンプル アプリを作成するつもりは**MonkeyBrowser** ([ここからダウンロード](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/))。 このアプリは、各 web ブラウザー ビューで別の URL を開き、4 つのタブに表示されます。 ユーザーは、アプリを実行している別の iOS デバイス上の任意のタブを続けることになります。

この動作をサポートするために必要なアクティビティの型識別子を作成するには、編集、 **Info.plist**ファイルに切り替えると、**ソース**ビュー。 追加、`NSUserActivityTypes`キーし、次の識別子を作成します。

[![](handoff-images/type01.png "NSUserActivityTypes キーと plist エディターで識別子が必要です。")](handoff-images/type01.png#lightbox)

4 つ新しいアクティビティの種類の識別子、例では、タブごとに 1 つを作成した**MonkeyBrowser**アプリ。 内容を置き換える独自のアプリを作成するときに、`NSUserActivityTypes`アプリ、アクティビティに固有のアクティビティ型の識別子を持つ配列をサポートしています。

### <a name="tracking-user-activity-changes"></a>ユーザーのアクティビティの変更の追跡

新しいインスタンスを作成するとき、`NSUserActivity`クラスを指定します、`NSUserActivityDelegate`変更アクティビティの状態を追跡するインスタンス。 たとえば、次のコードは、状態の変更を追跡するために使用できます。

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

`UserActivityReceivedData`継続 Stream が送信側のデバイスからデータを受信したときに、メソッドが呼び出されます。 詳細については、次を参照してください。、[サポート継続ストリーム](#supporting-continuation-streams)以下のセクション。

`UserActivityWasContinued`メソッドは、別のデバイスが現在のデバイスからアクティビティを実行したときに呼び出されます。 、ToDo リストに新しい項目の追加などのアクティビティの種類に応じてアプリを送信側のデバイスでのアクティビティ中止必要がある可能性があります。

`UserActivityWillSave`アクティビティへの変更が保存され、ローカルで使用可能なデバイス間で同期する前に、メソッドが呼び出されます。 このメソッドを使用して、最後の 1 分に変更を加えることができます、`UserInfo`のプロパティ、`NSUserActivity`インスタンスから送信されます。

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity インスタンスを作成します。

アプリが別のデバイスで続行の可能性を提供する必要がある各アクティビティにカプセル化する必要があります、`NSUserActivity`インスタンス。 アプリは、必要に応じて多くのアクティビティを作成でき、それらのアクティビティの性質は、機能と対象のアプリの機能に依存します。 たとえば、電子メール アプリは新しいメッセージとメッセージの読み取り用に作成するための 1 つのアクティビティを作成する可能性があります。

この例のアプリでは、新しい`NSUserActivity`タブ付きの web ブラウザー ビューのいずれかで、ユーザーが新しい URL を入力するたびに作成されます。 次のコードでは、指定したタブの状態を格納します。

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

新たに作成、`NSUserActivity`ユーザー アクティビティの種類のいずれかを使用して、上記で作成したし、アクティビティの人間が判読できるタイトルを提供します。 インスタンスにアタッチします、`NSUserActivityDelegate`の状態が変更され、このユーザーのアクティビティが現在のアクティビティであることを iOS に通知を監視する上に作成します。

### <a name="populating-the-userinfo-dictionary"></a>UserInfo ディクショナリの作成

上記で述べたように、`UserInfo`のプロパティ、`NSUserActivity`クラスは、`NSDictionary`特定のアクティビティの状態を定義するために使用するキー/値ペアの。 格納されている値`UserInfo`、次の種類のいずれかを指定する必要があります: `NSArray`、 `NSData`、 `NSDate`、 `NSDictionary`、 `NSNull`、 `NSNumber`、 `NSSet`、 `NSString`、または`NSURL`します。 `NSURL` 受信側のデバイス上の同じドキュメントを指しているように、iCloud のドキュメントを参照するデータ値が自動的に調整されます。

上記の例で作成しました、`NSMutableDictionary`オブジェクトし、ユーザーが特定のタブで現在表示して URL を提供する 1 つのキーが設定されます。`AddUserInfoEntries`ユーザー アクティビティのメソッドが受信側のデバイスでのアクティビティを復元するために使用するデータでアクティビティを更新するために使用します。

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple では、アクティビティが受信側のデバイスに適切なタイミングで送信されるように、必要最低限に送信される情報を維持することをお勧めします。 ドキュメントにアタッチされているイメージ編集より大きな情報が必要な場合に継続ストリームを使用する必要があります、送信する必要があります。 参照してください、[サポート継続ストリーム](#supporting-continuation-streams)詳細については後述します。

### <a name="continuing-an-activity"></a>アクティビティの続行

ローカルの iOS および OS X デバイスで発信元のデバイスに物理的に近接して継続可能ユーザー アクティビティの可用性、同一の iCloud アカウントに署名されたハンドオフ自動的に通知します。 (チーム ID とアクティビティの種類に基づく) 適切なアプリと情報システムが起動場合は、ユーザーは、新しいデバイスのアクティビティを続行するが、その`AppDelegate`継続は、発生する必要があります。

まず、`WillContinueUserActivityWithType`メソッドが呼び出されるは、アプリが継続の開始をユーザーに通知できるようにします。 次のコードを使用して、 **AppDelegate.cs**継続の開始を処理するために、例のアプリのファイル。

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

上記の例では、各ビュー コント ローラーの登録を`AppDelegate`パブリックであり`PreparingToHandoff`メソッドをアクティビティのインジケーターと、アクティビティの期限が現在のデバイスに渡すことをユーザーに知らせるメッセージが表示されます。 例:

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

`ContinueUserActivity`の`AppDelegate`が特定のアクティビティを実際には引き続き呼び出されます。 この例のアプリ: から、もう一度

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

パブリック`PerformHandoff`各ビュー コント ローラーのメソッドは、実際には、ハンドオフが発行され、現在のデバイスでのアクティビティを復元します。 例の場合は、別のデバイスでユーザーが閲覧された特定のタブで、同じ URL を表示します。 例:

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

`ContinueUserActivity`メソッドが含まれています、`UIApplicationRestorationHandler`ベース アクティビティの再開のドキュメントまたは応答側を呼び出すことです。 渡す必要があります、`NSArray`または復元のハンドラーが呼び出されたときに、復元可能なオブジェクト。 例えば:

```csharp
completionHandler (new NSObject[]{Tab4});
```

各オブジェクトが渡されると、その`RestoreUserActivityState`メソッドが呼び出されます。 各オブジェクトにデータを使用し、`UserInfo`が自身の状態を復元するためのディクショナリ。 例えば:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

ドキュメント ベースのアプリを実装していない場合の`ContinueUserActivity`メソッドまたはそれを返します`false`、`UIKit`または`AppKit`アクティビティを自動的に再開することができます。 参照してください、[ドキュメント ベース アプリでのサポートのハンドオフ](#supporting-handoff-in-document-based-apps)詳細については後述します。

### <a name="failing-handoff-gracefully"></a>ハンドオフにおける注意点

ハンドオフは、コレクションが疎に接続されている iOS および OS X デバイスの間で情報の送信に依存するので、転送プロセスされない場合があります。 このようなエラーを適切に処理し、発生する状況がすべてのユーザーに通知、アプリを設計する必要があります。

障害が発生した場合、`DidFailToContinueUserActivitiy`のメソッド、`AppDelegate`が呼び出されます。 例えば:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

指定されたを使用する必要があります`NSError`障害についてユーザーに情報を提供します。

## <a name="native-app-to-web-browser-handoff"></a>Web ブラウザーのハンドオフするネイティブ アプリ

ユーザーは、目的のデバイスにインストールされている適切なネイティブ アプリをしなくても、アクティビティを続行することがあります。 場合によっては、web ベースのインターフェイスが必要な機能を提供し、アクティビティを継続することができますも。 たとえば、ユーザーの電子メール アカウントでは、構成、およびメッセージを読み取るのための web ベース UI を提供できます。

元のネイティブ アプリ URL を知っている場合に、web インターフェイス (および、必要な構文を継続する特定の項目を識別するため)、この情報をエンコードできる、`WebpageURL`のプロパティ、`NSUserActivity`インスタンス。 受信側のデバイスが、継続を処理するためにインストールされている適切なネイティブ アプリを持っていない場合、指定された web インターフェイスを呼び出すことができます。

## <a name="web-browser-to-native-app-handoff"></a>Web ブラウザーでネイティブ アプリ ハンドオフ

ユーザーは、元のデバイスでは、web ベースのインターフェイスを使用していたし、受信側のデバイスでネイティブ アプリのドメイン部分を要求するかどうか、`WebpageURL`プロパティ、その後、システムにそのアプリの継続のハンドルを使用します。 新しいデバイスが受信する、`NSUserActivity`インスタンスとして、アクティビティの種類を示す`BrowsingWeb`と`WebpageURL`は、ユーザーがアクセスして、URL が含まれます、`UserInfo`ディクショナリを空になります。

この種類のハンドオフに参加するアプリでドメインを要求にする必要があります、`com.apple.developer.associated-domains`形式と権利`<service>:<fully qualified domain name>`(例: `activity continuation:company.com`)。

指定したドメインと一致する場合、`WebpageURL`ハンドオフ プロパティの値は、そのドメインに web サイトから承認されたアプリ Id の一覧をダウンロードします。 Web サイトが承認済みの Id という名前の署名された JSON ファイルの一覧を提供する必要があります**apple アプリ サイト関連付け**(たとえば、 `https://company.com/apple-app-site-association`)。

この JSON ファイル形式でアプリ Id のリストを指定するディクショナリを格納して`<team identifier>.<bundle identifier>`します。 例えば:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

JSON ファイルに署名する (正しいことがあるできるように`Content-Type`の`application/pkcs7-mime`)、使用、**ターミナル**アプリと`openssl`証明書とキーは iOS によって信頼された証明書機関によって発行されたコマンド (を参照してください[https://support.apple.com/kb/ht5012 ](https://support.apple.com/kb/ht5012)一覧については)。 例えば:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl`コマンドは、web サイトに設置する署名付き JSON ファイルを出力、 **apple アプリ サイト関連付け**URL。 例えば:

```csharp
https://example.com/apple-app-site-association.
```

アプリは任意のアクティビティを受け取るが`WebpageURL`ドメインがその`com.apple.developer.associated-domains`権利。 のみ、`http`と`https`プロトコルは、サポート、その他のプロトコルには、例外が発生します。

## <a name="supporting-handoff-in-document-based-apps"></a>ドキュメント ベースのアプリでのハンドオフのサポート

IOS および OS X で、前述のようドキュメント ベースのアプリは自動的にサポート ハンドオフ iCloud ベースのドキュメントの場合、アプリの**Info.plist**ファイルが含まれています、`CFBundleDocumentTypes`のキー`NSUbiquitousDocumentUserActivityType`します。 例えば:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

この例では、文字列は、逆引き DNS アプリの追加アクティビティの名前指定子は。 アクティビティの種類のエントリの必要はありませんで繰り返されるこの方法を入力した場合、`NSUserActivityTypes`の配列、 **Info.plist**ファイル。

ユーザー アクティビティの自動的に作成されたオブジェクト (ドキュメントの利用`UserActivity`プロパティ)、アプリの他のオブジェクトによって参照される、継続の状態を復元するために使用できます。 たとえば、追跡するために項目の選択とドキュメントを置きます。 このアクティビティを設定する必要がある`NeedsSave`プロパティを`true`状態を変更および更新されるたびに、`UserInfo`ディクショナリで、`UpdateUserActivityState`メソッド。

`UserActivity`プロパティは、任意のスレッドから使用できるしに iCloud の入出力移動するときに、ドキュメントの同期を保つために使用できるように、監視することのキー値 (KVO) プロトコルに準拠しています。 `UserActivity`ドキュメントが閉じられたときに、プロパティは無効になります。

詳細については、Apple を参照してください[ドキュメント ベース アプリでユーザーのアクティビティ サポート](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5)ドキュメント。

## <a name="supporting-handoff-in-responders"></a>レスポンダーでハンドオフのサポート

レスポンダーを関連付けることができます (いずれかから継承された`UIResponder`iOS でまたは`NSResponder`OS X 上) を設定してアクティビティを`UserActivity`プロパティ。 システムが自動的に保存、`UserActivity`プロパティに適切な応答側の呼び出し時間`UpdateUserActivityState`ユーザー アクティビティのオブジェクトを使用する現在のデータを追加するメソッドを`AddUserInfoEntriesFromDictionary`メソッド。

## <a name="supporting-continuation-streams"></a>継続のストリームをサポートしています。

場合、アクティビティの続行に必要な情報量効率的に転送できません、初期のハンドオフ ペイロードでもあるでしょう。 このような場合は、受信側のアプリは、それ自体と、元のアプリ データを転送する間、1 つまたは複数のストリームを確立できます。

元のアプリの設定、`SupportsContinuationStreams`のプロパティ、`NSUserActivity`インスタンス`true`します。 例えば:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

受信側のアプリを呼び出して、`GetContinuationStreams`のメソッド、`NSUserActivity`でその`AppDelegate`ストリームを確立するためにします。 例えば:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

ユーザー アクティビティのデリゲートが呼び出すことによって、ストリームを受信元のデバイスでは、その`DidReceiveInputStream`再開のデバイスで、ユーザー アクティビティを続行する、データを提供するメソッドが要求されました。

使用する、`NSInputStream`データをストリーミングする読み取り専用アクセスを提供して、`NSOutputStream`書き込み専用アクセスを提供します。 要求と応答の形式で、受信側のアプリより多くのデータを要求し、元のアプリで提供します、ストリームを使用する必要があります。 発信元デバイスの出力ストリームに書き込まれたデータを継続的のデバイスでは、入力ストリームから読み込まれるように、またはその逆です。

継続 Stream が必要な状況であってもが必要最小限の背面と 2 つのアプリ間の通信です。

詳細については、Apple を参照してください。[を使用して継続ストリーム](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13)ドキュメント。

## <a name="handoff-best-practices"></a>ハンドオフのベスト プラクティス

ハンドオフ経由でのユーザー アクティビティのシームレスな継続の実装を成功させるには、関連するさまざまなコンポーネントのための慎重に設計が必要です。 Apple では、ハンドオフが有効になっているアプリの次のベスト プラクティスを採用することを示しています。

- 次回に続くアクティビティの状態に関連する最小のペイロードを必要とするユーザー アクティビティをデザインします。 ペイロードが大きいほど、長くを開始する continuation がかかります。
- 大量のデータを正常に継続を転送する必要がある場合、は、構成とネットワークのオーバーヘッドで関連するコストを考慮します。
- 大きな Mac アプリ ユーザーのアクティビティによって処理される、いくつか小さくなり、iOS デバイスでタスク固有のアプリを作成するが一般的です。 連携して動作が正常に失敗したり、別のアプリと OS のバージョンを設計する必要があります。
- アクティビティの種類を指定する場合は、逆引き DNS 表記を使用して、衝突を避けるためです。 型定義でその名前を含める必要があるアクティビティが特定のアプリに固有の場合は、(たとえば`com.myCompany.myEditor.editing`)。 アクティビティは、複数のアプリで動作できる場合、は、定義から、アプリ名を削除 (たとえば`com.myCompany.editing`)。
- アプリがユーザー アクティビティの状態を更新する必要があるかどうか (`NSUserActivity`) 設定、`NeedsSave`プロパティを`true`します。 Handoff がデリゲートを呼び出し、適切なタイミングに`UserActivityWillSave`メソッドを更新できるように、`UserInfo`ディクショナリとして必要です。
- 実装する必要がありますハンドオフ プロセスは、受信側のデバイスでは瞬時に初期化しない可能性があります、ため、`AppDelegate`の`WillContinueUserActivity`し、ユーザーに通知する、継続が開始されようとしています。

## <a name="example-handoff-app"></a>ハンドオフ アプリの例

付属のハンドオフを使用して Xamarin.iOS アプリで例として、 [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)このガイドを使ってサンプル アプリです。 アプリには、ユーザーがそれぞれ特定のアクティビティ型を持つ web のブラウズに使用できる 4 つのタブがあります。天気、お気に入り、休憩時間および作業します。

任意のタブのユーザーが新しい URL とタップに入ったときに、**移動**ボタンを新しい`NSUserActivity`ユーザーが現在を参照する URL を含むタブが作成されます。

[![](handoff-images/handoff01.png "ハンドオフ アプリの例")](handoff-images/handoff01.png#lightbox)

ユーザーのデバイスのもう 1 つがある場合、 **MonkeyBrowser** 、インストールされているアプリが同じユーザー アカウントを使用して iCloud にサインイン、ネットワーク、およびホーム上のデバイスに近接、ハンドオフ アクティビティが表示されますが、同じ(左下隅) に画面:

[![](handoff-images/handoff02.png "左下隅のホーム画面に表示されるハンドオフ アクティビティ")](handoff-images/handoff02.png#lightbox)

ハンドオフ アイコンをユーザーが上にドラッグした場合、アプリが起動され、ユーザー アクティビティがで指定された、`NSUserActivity`新しいデバイスを継続します。

[![](handoff-images/handoff03.png "新しいデバイスに続き、ユーザー アクティビティ")](handoff-images/handoff03.png#lightbox)

ときに、ユーザー アクティビティが正常に送信された別 Apple のデバイスに送信側のデバイスの`NSUserActivity`への呼び出しが表示されます、`UserActivityWasContinued`メソッドをその`NSUserActivityDelegate`別に、ユーザー アクティビティが正常に転送されたことを認識できるようにするにはデバイスです。

## <a name="summary"></a>まとめ

この記事では、ユーザーの Apple デバイスの複数のユーザー アクティビティを続行するために使用ハンドオフ フレームワークの概要を与えられます。 次に、有効にし、Xamarin.iOS アプリでハンドオフを実装する方法を示しました。 最後に、さまざまな種類の使用可能なハンドオフ継続とハンドオフのベスト プラクティスを説明します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser サンプル](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 を新します。](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit のユーザー インターフェイス ガイドライン](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit フレームワーク参照](https://developer.apple.com/library/ios/home_kit_framework_ref)
