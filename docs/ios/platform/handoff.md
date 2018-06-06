---
title: Xamarin.iOS でハンドオフ
description: この記事では、カバーを転送する Xamarin.iOS アプリでハンドオフを操作するユーザーで実行されているアプリ間でのユーザー アクティビティの他のデバイス。
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec324e8fb8327b622424311b89567608311a6a19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787521"
---
# <a name="handoff-in-xamarinios"></a>Xamarin.iOS でハンドオフ

_この記事では、カバーを転送する Xamarin.iOS アプリでハンドオフを操作するユーザーで実行されているアプリ間でのユーザー アクティビティの他のデバイス。_

Apple には、同じアプリか、同じアクティビティをサポートする別のアプリを実行している別のデバイスに iOS 8 および OS X Yosemite (10.10) で自分のデバイスのいずれかの開始アクティビティを転送するユーザーの一般的なメカニズムを提供するハンドオフが導入されました。

[![](handoff-images/handoff02.png "ハンドオフ操作の実行の例")](handoff-images/handoff02.png#lightbox)

この記事は見てクイック アクティビティ Xamarin.iOS アプリでの共有を有効にして、詳しくハンドオフ フレームワークをカバーします。

## <a name="about-handoff"></a>ハンドオフについて

ハンドオフ (継続性とも呼ばれます) は、ユーザーが自分のデバイス (iOS または Mac) のいずれかでアクティビティを起動し、自分のデバイス (ユーザーの iClou で認識される形式の他の場所でその同じアクティビティを続行するための手段として Apple ios 8 および OS X Yosemite (10.10) で導入されました。d アカウント)。

Handoff が強化された検索機能で iOS 9 サポートすることも、新しい展開されました。 詳細についてを参照してください、[検索の機能強化](~/ios/platform/search/index.md)ドキュメント。

たとえば、ユーザーは、iPhone で電子メールを開始し、シームレスに電子メールをすべて同じメッセージの情報を入力し、iOS で残っているのと同じ場所にカーソルを置き、Mac のまま続行することができます。

同じを共有するアプリのいずれかの_チーム ID_は、iTunes アプリ ストアを経由して配信ハンドオフを使用してこれらのアプリのいずれかがいる限り、アプリ間でのユーザー アクティビティを続行するには、対象または登録済みの開発者によって署名されていない (Mac、Enterpriseまたは アドホック アプリ) で変更します。

どの`NSDocument`または`UIDocument`ベースのアプリに自動的にハンドオフ組み込みをサポートし、ハンドオフをサポートするために最小限の変更が必要があります。

### <a name="continuing-user-activities"></a>継続のユーザー アクティビティ

`NSUserActivity`クラス (を少し変更と共に`UIKit`と`AppKit`)、ユーザーのデバイスの他の場所で再開できる可能性のあるユーザーのアクティビティを定義するためのサポートを提供します。

別のユーザーのデバイス経由で渡されるアクティビティをその必要がありますにカプセル化するインスタンス`NSUserActivity`、としてマークされた、_現在のアクティビティ_セットを持つことのペイロード (データの継続を実行するために使用) およびアクティビティは、そのデバイスに送信する必要があります。

ハンドオフは、大きなデータ パケットが iCloud を使用して同期中では、継続するアクティビティを定義する情報の最低限を渡します。

受信デバイスで、ユーザー通知を受信するアクティビティが継続を使用できること。 (実行中でない場合、指定したアプリが起動した場合は、ユーザーは、新しいデバイスをアクティビティを続行するが、およびからペイロード、`NSUserActivity`活動の再開に使用します。

[![](handoff-images/handoffinteractions.png "継続のユーザー アクティビティの概要")](handoff-images/handoffinteractions.png#lightbox)

開発者が同じチーム ID を共有しに対応するアプリにのみ、指定された_アクティビティ タイプ_は継続タスクの対象にします。 アプリでサポートされているアクティビティの種類を定義する、`NSUserActivityTypes`のキーの**Info.plist**ファイル。 継続的デバイスそのため、チームの ID、アクティビティの種類に基づいて継続タスクを実行するアプリを選択し、必要に応じて、_活動のタイトル_です。

受信側のアプリからの情報を使用して、`NSUserActivity`の`UserInfo`ユーザー インターフェイスを構成し、遷移がエンドユーザーにシームレスに表示されるように、指定したアクティビティの状態を復元するためのディクショナリ。

かどうか、継続から効率的に送信できるより多くの情報が必要です、`NSUserActivity`では、呼び出し元のアプリを送信し、必要なデータを送信する 1 つまたは複数のストリームを確立アプリを再開することができます。 たとえば、編集する場合は、アクティビティが複数のイメージを持つ大きなテキスト ドキュメント、ストリーミング必要があります受信デバイスで、アクティビティを続行するために必要な情報を転送します。 詳細については、次を参照してください。、[サポート継続ストリーム](#Supporting-Continuation-Streams)以下のセクションです。

、前に述べたよう`NSDocument`または`UIDocument`ベースのアプリに自動的があるハンドオフ組み込みをサポートします。 詳細については、次を参照してください。、[ドキュメント ベースのアプリでサポートするハンドオフ](#Supporting-Handoff-in-Document-Based-Apps)以下のセクションです。

### <a name="the-nsuseractivity-class"></a>NSUserActivity クラス

`NSUserActivity`クラス ハンドオフ exchange の主要なオブジェクトは、および継続タスクに使用されるユーザー アクティビティの状態をカプセル化するために使用します。 アプリのコピーがインスタンス化され`NSUserActivity`任意のアクティビティをサポートします。 別のデバイスで続行したいのです。 たとえば、ドキュメント エディターは活動を作成、各ドキュメントの現在開いています。 (最前面のウィンドウまたはタブに表示されます)、最前面のドキュメントのみが、_現在のアクティビティ_され、継続タスクを下げて使用できます。

インスタンス`NSUserActivity`両方によって識別されるその`ActivityType`と`Title`プロパティです。 `UserInfo`辞書プロパティは、アクティビティの状態に関する情報を使用します。 設定、`NeedsSave`プロパティを`true`を遅延する場合を使用して状態情報を読み込む、 `NSUserActivity`'s を委任します。 使用して、`AddUserInfoEntries`に他のクライアントから新しいデータをマージする方法、`UserInfo`辞書のように、アクティビティの状態を維持するために必要です。

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate クラス

`NSUserActivityDelegate`に情報を保持するために使用、`NSUserActivity`の`UserInfo`ディクショナリ最新の状態と、アクティビティの現在の状態と同期します。 システムが更新されるアクティビティの情報が必要な場合 (など、別のデバイスでの継続の前に)、呼び出し、`UserActivityWillSave`デリゲートのメソッドです。

実装する必要があります、`UserActivityWillSave`メソッドとする変更が加えられる、 `NSUserActivity` (など`UserInfo`、`Title`など) を現在のアクティビティの状態を反映することを確認します。 システムを呼び出すと、 `UserActivityWillSave` 、メソッド、`NeedsSave`フラグがクリアされます。 アクティビティのデータ プロパティを変更する場合を設定する必要があります`NeedsSave`に`true`もう一度です。

使用する代わりに、`UserActivityWillSave`上に示したメソッドを持つことができます必要に応じて`UIKit`または`AppKit`ユーザーのアクティビティを自動的に管理します。 これを行うには、設定、応答側オブジェクトの`UserActivity`プロパティと実装、`UpdateUserActivityState`メソッドです。 参照してください、[レスポンダーでサポートするハンドオフ](#Supporting-Handoff-in-Responders)詳細については、後述の「します。

### <a name="app-framework-support"></a>アプリのフレームワークのサポート

両方`UIKit`(iOS) および`AppKit`(OS X) に渡すのための組み込みのサポートを提供する、 `NSDocument`、応答側 (`UIResponder`/`NSResponder`)、および`AppDelegate`クラスです。 各 OS では、ハンドオフが少し異なる方法で実装しているときに、基本的なメカニズムと Api は、同じです。

#### <a name="user-activities-in-document-based-apps"></a>ドキュメント ベースのアプリ内のユーザー アクティビティ

ドキュメントに基づく iOS および OS X アプリに自動的に組み込みハンドオフ サポートがあります。 このサポートを有効にする必要がありますを追加する、`NSUbiquitousDocumentUserActivityType`それぞれのキーと値`CFBundleDocumentTypes`エントリで、アプリの**Info.plist**ファイル。

このキーが存在する場合両方`NSDocument`と`UIDocument`を自動的に作成`NSUserActivity`iCloud ベースのドキュメントの指定された型のインスタンス。 アプリをサポートするドキュメントの種類ごとにアクティビティの種類を指定する必要があり、複数のドキュメント タイプは、同じ種類のアクティビティを使用できます。 両方`NSDocument`と`UIDocument`に自動的に設定、`UserInfo`のプロパティ、`NSUserActivity`でその`FileURL`プロパティの値。

OS X 上、`NSUserActivity`によって管理される`AppKit`レスポンダーに自動的に関連付けられていると、ドキュメントのウィンドウ メイン ウィンドウになったときに、現在のアクティビティになります。 Ios の場合の`NSUserActivity`によって管理されるオブジェクト`UIKit`、いずれかの呼び出しを行う必要があります`BecomeCurrent`メソッド明示的にも、ドキュメントの`UserActivity`プロパティの設定、`UIViewController`ときに、アプリが前面に表示します。

`AppKit` いずれかを自動的に復元`UserActivity`OS X 上には、この方法で作成されるプロパティです。これは、場合に発生、`ContinueUserActivity`メソッドを返します。`false`実装がない場合またはします。 このような状況は、ドキュメントが開かれて、`OpenDocument`のメソッド、`NSDocumentController`しを受信して、`RestoreUserActivityState`メソッドの呼び出しです。

参照してください、[ドキュメント ベースのアプリでサポートするハンドオフ](#Supporting-Handoff-in-Document-Based-Apps)詳細については、後述の「します。

#### <a name="user-activities-and-responders"></a>ユーザーのアクティビティとレスポンダー

両方`UIKit`と`AppKit`レスポンダー オブジェクトのとして設定した場合に、ユーザー アクティビティを自動的に管理できる`UserActivity`プロパティです。 設定する必要があります、状態が変更された場合、 `NeedsSave` 、レスポンダーのプロパティ`UserActivity`に`true`です。 システムが自動的に保存、`UserActivity`を呼び出すことによって、状態を更新する応答側の時間を与えた後、必要な場合にその`UpdateUserActivityState`メソッドです。

複数のレスポンダーは、1 つを共有している場合`NSUserActivity`受信インスタンス、`UpdateUserActivityState`システム、ユーザー アクティビティのオブジェクトを更新するときにコールバックします。 応答側を呼び出す必要がある、`AddUserInfoEntries`を更新するメソッド、`NSUserActivity`の`UserInfo`この時点で、現在のアクティビティの状態を反映するためのディクショナリ。 `UserInfo`それぞれ行う前に辞書がオフになって`UpdateUserActivityState`呼び出します。

アクティビティからそれ自体の関連付けを解除、レスポンダーを設定できます、`UserActivity`プロパティを`null`です。 アプリケーション フレームワークが管理されている`NSUserActivity`インスタンスには、関連付けられているレスポンダーよりまたはドキュメントがない、自動的に有効ではありません。

参照してください、[レスポンダーでサポートするハンドオフ](#Supporting-Handoff-in-Responders)詳細については、後述の「します。

#### <a name="user-activities-and-the-appdelegate"></a>ユーザー アクティビティ、および、AppDelegate

アプリの`AppDelegate`ハンドオフ継続タスクを処理するときに、その主なエントリ ポイントがします。 ときに、ユーザーは通知に応答、ハンドオフ、適切なアプリを起動 (既に実行されていない場合) および`WillContinueUserActivityWithType`のメソッド、`AppDelegate`が呼び出されます。 この時点では、アプリは、継続が開始されるユーザーに通知する必要があります。

`NSUserActivity`インスタンスが配信されたときに、`AppDelegate`の`ContinueUserActivity`メソッドが呼び出されます。 この時点では、アプリのユーザー インターフェイスを構成したり、特定のアクティビティを続行してください。

参照してください、[実装ハンドオフ](#Implementing-Handoff)詳細については、後述の「します。

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin アプリでハンドオフを有効にします。

Handoff によって課せられるセキュリティ要件、により Xamarin.iOS アプリ ハンドオフ framework を使用する必要があります適切に構成して Xamarin.iOS プロジェクト ファイル、Apple 開発者ポータルでします。

次の手順で行います。

1. ログイン、 [Apple 開発者ポータル](http://developer.apple.com)です。
2. をクリックして**証明書、識別子、およびプロファイル**です。
3. 完了していない場合は、をクリックして**識別子**、アプリの ID を作成し、(例: `com.company.appname`)、それ以外の場合、既存の ID を編集
4. いることを確認、 **iCloud**サービスは、指定された ID のチェックが完了します。 

    [![](handoff-images/provision01.png "指定した ID の iCloud サービスを有効にします。")](handoff-images/provision01.png#lightbox)
5. 変更内容を保存します。
4. をクリックして**プロビジョニング プロファイル** > **開発**プロファイルをプロビジョニングする新しい開発用アプリを作成するとします。 

    [![](handoff-images/provision02.png "プロビジョニング プロファイル、アプリを新規の開発を作成します。")](handoff-images/provision02.png#lightbox)
5. ダウンロードし、新しいプロビジョニング プロファイルをインストールするか、Xcode を使用してダウンロードし、プロファイルをインストールしてください。
6. Xamarin.iOS プロジェクトのオプションを編集し、先ほど作成したプロビジョニング プロファイルを使用していることを確認します。 

    [![](handoff-images/provision03.png "先ほど作成したプロビジョニング プロファイルを選択します。")](handoff-images/provision03.png#lightbox)
7. 次に、編集、 **Info.plist**ファイルをプロビジョニング プロファイルの作成に使用されたアプリ ID を使用していることを確認してください。 

    [![](handoff-images/provision04.png "アプリ ID を設定します。")](handoff-images/provision04.png#lightbox)
8. スクロール、**バック グラウンド モード**セクションし、次の項目を確認します。 

    [![](handoff-images/provision05.png "必要なバック グラウンド モードを有効にします。")](handoff-images/provision05.png#lightbox)
9. すべてのファイルに変更を保存します。

これらの設定で、アプリケーションはハンドオフ フレームワーク Api にアクセスする準備ができてようになりました。 プロビジョニングの詳細についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[してアプリをプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。

## <a name="implementing-handoff"></a>ハンドオフを実装します。

チーム ID に開発者が同じで署名された、同じアクティビティの種類をサポートしているアプリ間でユーザーの作業を続行できます。 ユーザー アクティビティのオブジェクトを作成する Xamarin.iOS アプリでハンドオフを実装する必要があります (どちらかに`UIKit`または`AppKit`)、アクティビティを追跡するために、オブジェクトの状態を更新し、受信デバイスで、アクティビティを継続します。

### <a name="identifying-user-activities"></a>ユーザー アクティビティを識別します。

ハンドオフの実装の最初の手順は、アプリをサポートするユーザー アクティビティの種類を識別し、継続タスクを別のデバイス上の適切な候補は、それらのアクティビティの表示です。 例: ToDo アプリケーションが 1 つとして項目を編集をサポートする_ユーザー アクティビティの種類_、および別の使用可能な項目の一覧の参照をサポートします。

アプリでは、同数ユーザー アクティビティの種類、必要なアプリが提供するすべての関数のいずれかを作成できます。 ユーザー アクティビティ、各種のアプリは、型のアクティビティが開始されると、終了し、別のデバイスでそのタスクを続行する最新の状態情報を保持する必要がありますを追跡する必要があります。

送信側と受信側のアプリ間で一対一のマッピングがないチーム ID が同じで署名されたすべてのアプリでは、ユーザー アクティビティを続行できます。 たとえば、特定のアプリでは、4 つのさまざまな種類の別のデバイス上の別、個々 のアプリで使用されるアクティビティを作成できます。 これは、ここで、各アプリは小さいと、特定のタスクに焦点を当てて (可能性のある多くの機能および関数) アプリの Mac バージョンと iOS アプリ間で頻繁に発生します。

### <a name="creating-activity-type-identifiers"></a>アクティビティの種類の識別子を作成します。

_アクティビティ型識別子_短い文字列を追加、`NSUserActivityTypes`のアプリの配列**Info.plist**ファイルが指定したユーザー アクティビティの種類を一意に識別するために使用します。 配列内のアプリでサポートされる各アクティビティの 1 つのエントリがあります。 Apple では、競合を避けるためのアクティビティ型識別子の逆引き DNS 形式表記を使用してを提案します。 例:`com.company-name.appname.activity`ベースのアクティビティの特定のアプリまたは`com.company-name.activity`複数のアプリを実行できるアクティビティのためです。

作成するときにアクティビティの型識別子が使用される、`NSUserActivity`アクティビティの種類を識別するインスタンス。 アクティビティを別のデバイスで続行すると、(アプリのチーム ID) と共に、アクティビティの種類は、アクティビティを続行するを起動するには、どのアプリを決定します。

例としてと呼ばれるサンプル アプリを作成するつもりが**MonkeyBrowser** ([ここからダウンロード](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/))。 このアプリは、それぞれ、異なる URL を開いた状態で web ブラウザー ビューで、4 つのタブに表示されます。 ユーザーは、アプリを実行している別の iOS デバイスで任意のタブを続けることになります。

この動作をサポートするために必要なアクティビティの型識別子を作成するには、編集、 **Info.plist**ファイルに切り替えると、**ソース**ビュー。 追加、`NSUserActivityTypes`キーし、次の識別子を作成します。

[![](handoff-images/type01.png "NSUserActivityTypes キーと plist エディターで必要な識別子")](handoff-images/type01.png#lightbox)

4 つ新しいアクティビティの種類の識別子、例ではタブごとに 1 つを作成した**MonkeyBrowser**アプリ。 内容を置き換える独自のアプリを作成するときに、`NSUserActivityTypes`アプリのアクティビティに固有のアクティビティ型の識別子を持つ配列をサポートしています。

### <a name="tracking-user-activity-changes"></a>ユーザー アクティビティの変更の追跡

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

`UserActivityReceivedData`継続ストリームが送信元デバイスからデータを受信したときに、メソッドが呼び出されます。 詳細については、次を参照してください。、[サポート継続ストリーム](#Supporting-Continuation-Streams)以下のセクションです。

`UserActivityWasContinued`メソッドは別のデバイスが、現在のデバイスからアクティビティを取得します。 、ToDo リストに新しい項目の追加などのアクティビティの種類に応じてアプリを送信側のデバイスで、アクティビティ中止必要がある可能性があります。

`UserActivityWillSave`アクティビティへの変更が保存され、ローカルで使用できるデバイス間で同期する前に、メソッドが呼び出されます。 このメソッドを使用するには、最後の 1 分を変更する、`UserInfo`のプロパティ、`NSUserActivity`インスタンスから送信されます。

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity インスタンスを作成します。

アプリが別のデバイスで続行される可能性を提供する各アクティビティをカプセル化する必要があります、`NSUserActivity`インスタンス。 アプリは、必要に応じて多くのアクティビティを作成でき、それらの活動の性質は、機能と対象のアプリの機能に依存します。 たとえば、電子メール アプリは、新しいメッセージとメッセージの読み取り用に別の作成するための 1 つのアクティビティを作成する可能性があります。

この例のアプリの新しい`NSUserActivity`ユーザーがタブ付きの web ブラウザー ビューのいずれかで、新しい URL を入力するたびに作成されます。 次のコードは、指定したタブの状態を格納します。

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

新たに作成、`NSUserActivity`上記で作成したを使用して、ユーザー アクティビティの種類のいずれかと、アクティビティの人間が判読できるタイトルを提供します。 インスタンスにアタッチ、`NSUserActivityDelegate`状態を変更し、このユーザーの利用状況が、現在のアクティビティであることを iOS に通知を監視する上に作成します。

### <a name="populating-the-userinfo-dictionary"></a>ユーザー情報ディクショナリを設定します。

上、これまで見てきたよう、`UserInfo`のプロパティ、`NSUserActivity`クラスは、`NSDictionary`特定のアクティビティの状態を定義するために使用するキー/値ペアのです。 格納されている値`UserInfo`、次の種類のいずれかを指定する必要があります: `NSArray`、 `NSData`、 `NSDate`、 `NSDictionary`、 `NSNull`、 `NSNumber`、 `NSSet`、 `NSString`、または`NSURL`です。 `NSURL` iCloud ドキュメント をポイントするデータ値を受信デバイス上の同じドキュメントを指すように自動的に調整されます。

上記の例で作成しました、`NSMutableDictionary`オブジェクトし、ユーザーが、指定されたタブに現在表示している URL を提供する 1 つのキーが設定されます。`AddUserInfoEntries`アクティビティが受信側のデバイスでの復元に使用されるデータでアクティビティを更新するユーザー アクティビティのメソッドが使用されました。

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple では、受信デバイスに適切なタイミングで、アクティビティが送信されるように、必要最低限に送信される情報を保持するをお勧めします。 大規模な情報が必要な場合、編集、ドキュメントにアタッチされているイメージと同じようにする必要がある、継続のストリームを使用する必要があります。 参照してください、[サポート継続ストリーム](#Supporting-Continuation-Streams)詳細については、後述の「します。

### <a name="continuing-an-activity"></a>アクティビティを続行

ハンドオフはローカル iOS および元のデバイスに物理的に近づくにあり、継続のユーザー アクティビティの可用性、同一の iCloud アカウントに署名する OS X デバイスに自動的に通知します。 (チーム ID とアクティビティの種類に基づく) の適切なアプリと情報に、システムが起動場合は、ユーザーは、新しいデバイスにアクティビティを続行するが、その`AppDelegate`継続は、発生する必要があります。

最初に、`WillContinueUserActivityWithType`メソッドが呼び出されるは、アプリがユーザーに通知する、継続が開始するためです。 次のコードを使用して、 **<code>appdelegate.cs</code>** 継続の開始を処理する例のアプリのファイル。

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

各ビューのコント ローラーに登録前の例では、`AppDelegate`がパブリックと`PreparingToHandoff`をアクティビティのインジケーターとアクティビティの期限が現在のデバイスに渡すことをユーザーに知らせるメッセージを表示するメソッド。 例:

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

`ContinueUserActivity`の`AppDelegate`実際に指定したアクティビティを続行するが呼び出されます。 この例のアプリ: からもう一度、

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

パブリック`PerformHandoff`ビューの各コント ローラーのメソッドは、実際に preforms ハンドオフし、現在のデバイス上のアクティビティを復元します。 例の場合は、ユーザーが別のデバイスで閲覧する指定されたタブで、同じ URL を表示します。 例:

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

`ContinueUserActivity`メソッドが含まれています、`UIApplicationRestorationHandler`活動の再開に基づくドキュメントまたは応答側に対して呼び出すことです。 渡す必要があります、`NSArray`または復元のハンドラーが呼び出されたときに、復元可能なオブジェクトです。 例えば:

```csharp
completionHandler (new NSObject[]{Tab4});
```

渡されると、オブジェクトごとにその`RestoreUserActivityState`メソッドが呼び出されます。 各オブジェクトにデータを使用し、`UserInfo`が自身の状態を復元するためのディクショナリ。 例えば:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

実装しない場合、ドキュメントに基づくアプリ、`ContinueUserActivity`メソッドまたはそれを返します`false`、`UIKit`または`AppKit`アクティビティを自動的に再開できます。 参照してください、[ドキュメント ベースのアプリでサポートするハンドオフ](#Supporting-Handoff-in-Document-Based-Apps)詳細については、後述の「します。

### <a name="failing-handoff-gracefully"></a>ハンドオフを正常に失敗します。

ハンドオフは疎接続コレクション iOS および OS X デバイスの間で情報の送信に依存するので、転送プロセスは失敗することができます。 このようなエラーを適切に処理し、発生する状況がすべてのユーザーに通知するアプリを設計する必要があります。

障害が発生した場合、`DidFailToContinueUserActivitiy`のメソッド、`AppDelegate`が呼び出されます。 例えば:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

指定されたを使用する必要があります`NSError`障害についてユーザーに情報を提供します。

## <a name="native-app-to-web-browser-handoff"></a>Web ブラウザー ハンドオフするネイティブ アプリ

ユーザーは、目的のデバイスにインストールされている、適切なネイティブ アプリをしなくても、アクティビティを続行したい場合があります。 状況によっては、web ベースのインターフェイスが必要な機能を提供し、アクティビティを継続することができますもします。 たとえば、ユーザーの電子メール アカウントは、作成するメッセージを読み取るのための web ベースの UI を提供する可能性があります。

発信元、ネイティブ アプリ URL を知っている場合にこの情報をエンコード、web インターフェイス (および、必要な構文を継続する指定したアイテムを識別するため)、`WebpageURL`のプロパティ、`NSUserActivity`インスタンス。 受信デバイスには、継続の処理にインストールされている、適切なネイティブ アプリが割り当てられていない、指定された web インターフェイスを呼び出すことができます。

## <a name="web-browser-to-native-app-handoff"></a>ネイティブ アプリ ハンドオフする web ブラウザー

かどうか、ユーザーが元のデバイスで web ベースのインターフェイスを使用して、受信デバイスでのネイティブ アプリの要求のドメイン部分、`WebpageURL`プロパティ、そのシステムにそのアプリを継続のハンドルを使用します。 新しいデバイスが受信する、`NSUserActivity`インスタンスとして、アクティビティの種類を示す`BrowsingWeb`と`WebpageURL`ユーザーへのアクセスが、URL が含まれます、`UserInfo`辞書が空になります。

ハンドオフのこの型に参加するアプリでのドメインを要求にする必要があります、`com.apple.developer.associated-domains`形式と権利`<service>:<fully qualified domain name>`(例: `activity continuation:company.com`)。

指定されたドメインと一致する場合、`WebpageURL`ハンドオフ プロパティの値は、そのドメインで web サイトから承認済みのアプリ Id の一覧をダウンロードします。 Web サイトは、という名前の署名された JSON ファイル内の承認済みの Id の一覧を指定する必要があります**apple app サイト関連付け**(たとえば、 `https://company.com/apple-app-site-association`)。

この JSON ファイルには、フォームでのアプリ Id の一覧を示すディクショナリが含まれている`<team identifier>.<bundle identifier>`です。 例えば:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

JSON ファイルに署名する (正しいことがあるできるように`Content-Type`の`application/pkcs7-mime`)、使用、**ターミナル**アプリと`openssl`コマンドと、証明書とキーが iOS で信頼された証明機関によって発行された (を参照してください[http://support.apple.com/kb/ht5012 ](http://support.apple.com/kb/ht5012)一覧)。 例えば:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl`コマンドは、web サイトに配置する署名付き JSON ファイルを出力、 **apple app サイト関連付け**URL。 例えば:

```csharp
https://example.com/apple-app-site-association.
```

アプリは任意のアクティビティを受け取りますが`WebpageURL`ドメインはその`com.apple.developer.associated-domains`権利。 のみ、`http`と`https`プロトコルのサポートは、その他のプロトコルには、例外が発生します。

## <a name="supporting-handoff-in-document-based-apps"></a>ドキュメント ベースのアプリでハンドオフのサポート

IOS および OS X 上前に、述べたようドキュメント ベースのアプリは自動的にサポート ハンドオフ iCloud ベースのドキュメントの場合、アプリの**Info.plist**ファイルが含まれています、`CFBundleDocumentTypes`のキー`NSUbiquitousDocumentUserActivityType`です。 例えば:

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

この例では、文字列は、逆引き DNS アプリの追加アクティビティの名前を持つ指定子は。 この方法を入力した場合、アクティビティの型エントリ必要はありませんで繰り返される、`NSUserActivityTypes`の配列、 **Info.plist**ファイル。

ユーザーの利用状況の自動的に作成されたオブジェクト (を介して、ドキュメントの使用可能な`UserActivity`プロパティ)、アプリで他のオブジェクトによって参照され、継続の状態を復元するために使用できます。 たとえば、追跡するために項目の選択とドキュメントを置きます。 このアクティビティを設定する必要がある`NeedsSave`プロパティを`true`状態を変更および更新するたびに、`UserInfo`ディクショナリで、`UpdateUserActivityState`メソッドです。

`UserActivity`プロパティが任意のスレッドから使用でき、ドキュメントの同期を保つ iCloud の移動とアウトするときに使用できるように、キーと値観測 (KVO) プロトコルに準拠しています。 `UserActivity`ドキュメントが閉じられたときに、プロパティは無効になります。

詳細については、Apple を参照してください[ドキュメント ベースのアプリでのユーザー アクティビティ サポート](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5)ドキュメント。

## <a name="supporting-handoff-in-responders"></a>レスポンダーでハンドオフのサポート

レスポンダーを関連付けることができます (いずれかから継承された`UIResponder`iOS でまたは`NSResponder`OS X 上) の設定により、アクティビティを`UserActivity`プロパティです。 システムが自動的に保存、`UserActivity`時刻、応答側の呼び出し、適切なプロパティ`UpdateUserActivityState`、ユーザー アクティビティを使用してオブジェクトに現在のデータを追加するメソッド、`AddUserInfoEntriesFromDictionary`メソッドです。

## <a name="supporting-continuation-streams"></a>継続のストリームをサポートします。

ここで、アクティビティを続行するために必要な情報の量効率的に転送できません初期ハンドオフ ペイロードで状況があります。 このような場合は、受信側のアプリはそれ自体とデータを転送元アプリ間で 1 つまたは複数のストリームを確立できます。

元のアプリは、設定、`SupportsContinuationStreams`のプロパティ、`NSUserActivity`インスタンスを`true`です。 例えば:

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

受信側のアプリは呼び出すことができますし、`GetContinuationStreams`のメソッド、`NSUserActivity`でその`AppDelegate`ストリームを確立するためにします。 例えば:

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

ユーザー アクティビティのデリゲートが呼び出すことによって、ストリームを受信元のデバイスでその`DidReceiveInputStream`再開のデバイスでユーザーの利用状況を引き続きデータを提供するメソッドが要求されました。

使用して、`NSInputStream`データをストリーミングする読み取り専用のアクセスを提供して、`NSOutputStream`書き込み専用アクセスを提供します。 ストリームは、要求と応答の方法で、受信側のアプリが複数のデータを要求して、発信元のアプリに提供を使用してください。 これにより、元のデバイスに出力ストリームに書き込まれたデータが継続的デバイスで、入力ストリームから読み取られると、その逆です。

継続のストリームが必要な場合であっても必要があります、最小限の背面と 4 番目 2 つのアプリ間の通信です。

詳細については、Apple を参照してください。[を使用して継続ストリーム](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13)ドキュメント。

## <a name="handoff-best-practices"></a>ハンドオフのベスト プラクティス

ハンドオフを介してユーザー アクティビティのシームレスな継続の実装を成功させるには、関連するさまざまなコンポーネントのための慎重に設計が必要です。 Apple では、ハンドオフが有効になっているアプリの次のベスト プラクティスを採用することを示しています。

- 最小のペイロードを継続するアクティビティの状態を関連付けることが必要とするユーザー アクティビティをデザインします。 ペイロードが大きいほど、時間が長いを開始する継続タスクを使用します。
- 大量の継続タスクを正常にデータを転送する必要がある場合、は、構成とネットワークのオーバーヘッドのコストが関係しているを考慮します。
- 大規模な Mac アプリによって処理されるいくつかより小さい場合は、iOS デバイスでアプリのタスクに固有のユーザー アクティビティを作成する一般的なことです。 別のアプリと OS のバージョンは、連携して動作または正常に失敗を設計する必要があります。
- 活動の種類を指定する場合は、競合を避けるために逆引き DNS 表記を使用します。 その名前を種類の定義に含める必要が、アクティビティが特定のアプリに固有の場合は、(たとえば`com.myCompany.myEditor.editing`)。 アクティビティは、複数のアプリで動作できる場合、は、定義からアプリの名前を削除 (たとえば`com.myCompany.editing`)。
- アプリがユーザー アクティビティの状態を更新する必要があるかどうか (`NSUserActivity`) 設定、`NeedsSave`プロパティを`true`です。 ハンドオフが、デリゲートを呼び出して、適切な時期`UserActivityWillSave`メソッドを更新できるように、`UserInfo`必要に応じてディクショナリ。
- 実装する必要があります、ハンドオフ プロセスは、受信デバイスでは瞬時に初期化しない可能性があります、ため、`AppDelegate`の`WillContinueUserActivity`ユーザーに通知する継続タスクを開始しようとします。

## <a name="example-handoff-app"></a>例ハンドオフ アプリ

付属して Xamarin.iOS アプリでハンドオフを使用する例として、 [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)このガイドにサンプル アプリ。 アプリが、指定されたアクティビティ タイプと web を閲覧するユーザーが使用できる 4 つのタブ: 天気、お気に入り、休憩および作業します。

[すべて] タブで、ユーザーは、新しい URL とタップが入ったときに、**移動**ボタンを新しい`NSUserActivity`は、ユーザーが現在を参照する URL を含むタブの作成します。

[![](handoff-images/handoff01.png "例ハンドオフ アプリ")](handoff-images/handoff01.png#lightbox)

別のユーザーのデバイスの場合、 **MonkeyBrowser**アプリをインストールするには、同じユーザー アカウントを使用して iCloud に署名であり、同じネットワーク上のデバイスに近接ホーム ハンドオフ アクティビティが表示されます画面で、左下隅):

[![](handoff-images/handoff02.png "左下隅で、ホーム画面に表示されるハンドオフ アクティビティ")](handoff-images/handoff02.png#lightbox)

ユーザーは、ハンドオフ アイコンの上方向へドラッグ、する場合、アプリを起動してで指定されたユーザーの利用状況、`NSUserActivity`新しいデバイスを継続します。

[![](handoff-images/handoff03.png "新しいデバイスに続き、ユーザー アクティビティ")](handoff-images/handoff03.png#lightbox)

ときに、ユーザー アクティビティが送信されました別 Apple のデバイスに、送信側のデバイスの`NSUserActivity`への呼び出しを受信、`UserActivityWasContinued`メソッドをその`NSUserActivityDelegate`別、ユーザー アクティビティが正常に転送されたことを認識できるようにするにはデバイス。

## <a name="summary"></a>まとめ

この記事は、ユーザーの Apple デバイスの複数の間のユーザー アクティビティを続行するために使用ハンドオフ フレームワークの概要を与えられます。 次に、有効にし、Xamarin.iOS アプリでハンドオフを実装する方法を示しました。 最後に、さまざまな種類の使用可能なハンドオフ継続とハンドオフのベスト プラクティスを説明します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser サンプル](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [新機能 iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit ユーザー インターフェイスのガイドライン](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework リファレンス](https://developer.apple.com/library/ios/home_kit_framework_ref)
