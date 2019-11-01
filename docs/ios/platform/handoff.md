---
title: Xamarin のハンドオフ (iOS)
description: この記事では、ユーザーの他のデバイスで実行されているアプリ間でユーザーアクティビティを転送するために、Xamarin iOS アプリでハンドオフを操作する方法について説明します。
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 97a4a90b66e2c205ef8e9081e6989bf0f3650b16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032403"
---
# <a name="handoff-in-xamarinios"></a>Xamarin のハンドオフ (iOS)

_この記事では、ユーザーの他のデバイスで実行されているアプリ間でユーザーアクティビティを転送するために、Xamarin iOS アプリでハンドオフを操作する方法について説明します。_

Apple では、iOS 8 および OS X ヨーク Semite (10.10) にハンドオフが導入されました。ユーザーは、デバイスの1つで開始されたアクティビティを、同じアプリを実行している別のデバイス、または同じアクティビティをサポートする別のデバイスに転送するための一般的なメカニズムを提供します。

[![](handoff-images/handoff02.png "An example of performing a Handoff operation")](handoff-images/handoff02.png#lightbox)

この記事では、Xamarin. iOS アプリでアクティビティの共有を有効にする方法を簡単に説明し、ハンドオフフレームワークについて詳しく説明します。

## <a name="about-handoff"></a>ハンドオフについて

ハンドオフ (継続性とも呼ばれます) は、ユーザーがいずれかのデバイス (iOS または Mac) でアクティビティを開始し、デバイスの別のアクティビティ (ユーザーの iClou によって識別) を続行する方法として、Apple によって iOS 8 および OS X ヨーク Semite (10.10) に導入されました。d アカウント)。

ハンドオフは、新しい高度な検索機能もサポートするために、iOS 9 で拡張されました。 詳細については、[検索の拡張機能](~/ios/platform/search/index.md)に関するドキュメントを参照してください。

たとえば、ユーザーは自分の iPhone で電子メールを開始し、その Mac で同じメッセージ情報をすべて入力したまま、同じ場所に保存した電子メールをシームレスに継続することができます。

同じ_チーム ID_を共有するすべてのアプリは、アプリ間でユーザーのアクティビティを継続するために、ハンドオフを使用することができます。ただし、これらのアプリが ITunes app Store 経由で配信されるか、登録されている開発者 (Mac、エンタープライズ、またはアドホックアプリ) によって署名される場合に限ります。

`NSDocument` または `UIDocument` ベースのアプリには自動的にハンドオフサポートが組み込まれており、ハンドオフをサポートするために最小限の変更を加える必要があります。

### <a name="continuing-user-activities"></a>ユーザーアクティビティの継続

`NSUserActivity` クラス (`UIKit` と `AppKit`に対するいくつかの小さな変更と共に) では、ユーザーのアクティビティを定義して、他のユーザーのデバイスで続行できる可能性があります。

アクティビティをユーザーの別のデバイスに渡すには、そのアクティビティを `NSUserActivity`インスタンスにカプセル化し、_現在のアクティビティ_としてマークし、ペイロードセット (継続を実行するために使用されるデータ) を設定してから、アクティビティをに転送する必要があります。そのデバイス。

ハンドオフは、必要最小限の情報を渡して、継続するアクティビティを定義します。これにより、より大きなデータパケットが iCloud 経由で同期されます。

受信デバイスでは、アクティビティを継続できるという通知がユーザーに送信されます。 ユーザーが新しいデバイスでアクティビティを続行することを選択した場合は、指定されたアプリが起動され (まだ実行されていない場合)、`NSUserActivity` からのペイロードを使用してアクティビティが再開されます。

[![](handoff-images/handoffinteractions.png "An overview of Continuing User Activities")](handoff-images/handoffinteractions.png#lightbox)

同じ開発者チーム ID を共有し、特定の_種類のアクティビティ_に応答するアプリのみが、継続の対象となります。 アプリでは、その**情報の plist**ファイルの `NSUserActivityTypes` キーでサポートされるアクティビティの種類を定義します。 これにより、継続しているデバイスは、チーム ID、アクティビティの種類、および必要に応じて_アクティビティのタイトル_に基づいて、継続を実行するアプリを選択します。

受信側のアプリは、`NSUserActivity`の `UserInfo` ディクショナリからの情報を使用してそのユーザーインターフェイスを構成し、エンドユーザーにシームレスに表示されるように、特定のアクティビティの状態を復元します。

継続が `NSUserActivity`によって効率的に送信できるよりも多くの情報を必要とする場合、再開中のアプリは発信元アプリに呼び出しを送信し、必要なデータを送信するための1つ以上のストリームを確立できます。 たとえば、アクティビティが複数のイメージを含む大きなテキストドキュメントを編集していた場合、受信デバイスでアクティビティを続行するために必要な情報を転送するためにストリーミングが必要になります。 詳細については、以下の「[継続ストリームのサポート](#supporting-continuation-streams)」セクションを参照してください。

前述のように、`NSDocument` または `UIDocument` ベースのアプリには、自動的にハンドオフサポートが組み込まれています。 詳細については、以下の「[ドキュメントベースのアプリでのハンドオフのサポート](#supporting-handoff-in-document-based-apps)」セクションを参照してください。

### <a name="the-nsuseractivity-class"></a>NSUserActivity クラス

`NSUserActivity` クラスは、ハンドオフ交換のプライマリオブジェクトであり、継続に使用できるユーザーアクティビティの状態をカプセル化するために使用されます。 アプリは、サポートしているすべてのアクティビティの `NSUserActivity` のコピーをインスタンス化し、別のデバイスで続行することを望んでいます。 たとえば、ドキュメントエディターは、現在開いている各ドキュメントに対してアクティビティを作成します。 ただし、最前面のドキュメント (最前面のウィンドウまたはタブに表示されます) のみが_現在のアクティビティ_であり、れるために使用できます。

`NSUserActivity` のインスタンスは、`ActivityType` と `Title` の両方のプロパティによって識別されます。 `UserInfo` dictionary プロパティは、アクティビティの状態に関する情報を伝達するために使用されます。 `NSUserActivity`のデリゲートを介して状態情報を遅延読み込みする場合は、`NeedsSave` プロパティを `true` に設定します。 `AddUserInfoEntries` メソッドを使用して、アクティビティの状態を維持するために必要に応じて、他のクライアントからの新しいデータを `UserInfo` ディクショナリにマージします。

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate クラス

`NSUserActivityDelegate` は、`NSUserActivity`の `UserInfo` ディクショナリ内の情報を最新の状態に保ち、アクティビティの現在の状態と同期するために使用されます。 更新するアクティビティの情報 (別のデバイスでの継続など) が必要な場合は、デリゲートの `UserActivityWillSave` メソッドを呼び出します。

`UserActivityWillSave` メソッドを実装し、`NSUserActivity` (`UserInfo`、`Title`など) に変更を加えて、現在のアクティビティの状態が反映されていることを確認する必要があります。 システムが `UserActivityWillSave` メソッドを呼び出すと、`NeedsSave` フラグがクリアされます。 アクティビティのデータプロパティを変更する場合は、`NeedsSave` を再度 `true` するように設定する必要があります。

上記の `UserActivityWillSave` 方法を使用する代わりに、必要に応じて、ユーザーアクティビティを自動的に `UIKit` したり `AppKit` 管理したりすることができます。 これを行うには、応答側オブジェクトの `UserActivity` プロパティを設定し、`UpdateUserActivityState` メソッドを実装します。 詳細については、以下の「[応答側でのハンドオフのサポート](#supporting-handoff-in-responders)」セクションを参照してください。

### <a name="app-framework-support"></a>アプリフレームワークのサポート

`UIKit` (iOS) と `AppKit` (OS X) はどちらも、`NSDocument`、レスポンダー (`UIResponder`/`NSResponder`)、および `AppDelegate` クラスでのハンドオフのサポートが組み込まれています。 各 OS はハンドオフを若干異なる方法で実装しますが、基本的なメカニズムと Api は同じです。

#### <a name="user-activities-in-document-based-apps"></a>ドキュメントベースのアプリでのユーザーアクティビティ

ドキュメントベースの iOS および OS X アプリには、自動的にハンドオフサポートが組み込まれています。 このサポートを有効にするには、アプリの**情報 plist**ファイルの各 `CFBundleDocumentTypes` エントリに `NSUbiquitousDocumentUserActivityType` キーと値を追加する必要があります。

このキーが存在する場合は、`NSDocument` と `UIDocument` の両方で、指定された種類の iCloud ベースのドキュメントの `NSUserActivity` インスタンスが自動的に作成されます。 アプリがサポートするドキュメントの種類ごとにアクティビティの種類を指定する必要があります。また、複数のドキュメントの種類で同じアクティビティの種類を使用できます。 `NSDocument` と `UIDocument` はどちらも、`NSUserActivity` の `UserInfo` プロパティに、その `FileURL` プロパティの値を自動的に設定します。

OS X では、`AppKit` によって管理され、レスポンダーに関連付けられている `NSUserActivity` は、ドキュメントのウィンドウがメインウィンドウになったときに、自動的に現在のアクティビティになります。 IOS では、`UIKit`によって管理される `NSUserActivity` オブジェクトの場合、`BecomeCurrent` メソッドを明示的に呼び出すか、アプリがフォアグラウンドになったときにドキュメントの `UserActivity` プロパティを `UIViewController` に設定する必要があります。

`AppKit` は、この方法で作成された `UserActivity` のプロパティを OS X に自動的に復元します。このエラーは、`ContinueUserActivity` メソッドが `false` を返した場合、または実装されていない場合に発生します。 この場合、ドキュメントは、`NSDocumentController` の `OpenDocument` メソッドを使用して開かれ、`RestoreUserActivityState` メソッドの呼び出しを受け取ります。

詳細については、以下の「[ドキュメントベースのアプリでのハンドオフのサポート](#supporting-handoff-in-document-based-apps)」セクションを参照してください。

#### <a name="user-activities-and-responders"></a>ユーザーアクティビティとレスポンダー

`UIKit` と `AppKit` はどちらも、応答側オブジェクトの `UserActivity` プロパティとして設定した場合、ユーザーアクティビティを自動的に管理できます。 状態が変更されている場合は、応答側の `UserActivity` の `NeedsSave` プロパティを `true`に設定する必要があります。 `UpdateUserActivityState` メソッドを呼び出すことによって状態を更新するために、応答側の時間を指定した後に、必要に応じて `UserActivity` が自動的に保存されます。

複数のレスポンダーが1つの `NSUserActivity` インスタンスを共有している場合は、システムがユーザーアクティビティオブジェクトを更新するときに `UpdateUserActivityState` コールバックを受け取ります。 応答側は、`AddUserInfoEntries` メソッドを呼び出して、この時点で現在のアクティビティの状態を反映するように `NSUserActivity`の `UserInfo` ディクショナリを更新する必要があります。 各 `UpdateUserActivityState` が呼び出される前に、`UserInfo` ディクショナリがクリアされます。

応答側は、アクティビティとの関連付けを解除するために、その `UserActivity` プロパティを `null`に設定できます。 アプリフレームワークマネージ `NSUserActivity` インスタンスに関連付けられたレスポンダーまたはドキュメントがそれ以上ない場合は、自動的に無効になります。

詳細については、以下の「[応答側でのハンドオフのサポート](#supporting-handoff-in-responders)」セクションを参照してください。

#### <a name="user-activities-and-the-appdelegate"></a>ユーザーアクティビティと AppDelegate

ハンドオフ継続を処理するときに、アプリの `AppDelegate` が主なエントリポイントとなります。 ユーザーがハンドオフ通知に応答すると、適切なアプリが起動され (まだ実行されていない場合)、`AppDelegate` の `WillContinueUserActivityWithType` メソッドが呼び出されます。 この時点で、アプリは継続が開始されたことをユーザーに通知する必要があります。

`NSUserActivity` インスタンスは、`AppDelegate`の `ContinueUserActivity` メソッドが呼び出されたときに配信されます。 この時点で、アプリのユーザーインターフェイスを構成し、指定されたアクティビティを続行する必要があります。

詳細については、以下の「[ハンドオフの実装](#implementing-handoff)」を参照してください。

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin アプリでのハンドオフの有効化

ハンドオフによって課せられるセキュリティ要件により、ハンドオフフレームワークを使用する Xamarin iOS アプリは、Apple Developer ポータルと Xamarin の iOS プロジェクトファイルの両方で適切に構成されている必要があります。

次の手順で行います。

1. [Apple Developer ポータル](https://developer.apple.com)にログインします。
2. [**証明書]、[識別子 & プロファイル**] の順にクリックします。
3. まだ行っていない場合は、 **[識別子]** をクリックし、アプリの id を作成します (例: `com.company.appname`)。それ以外の場合は、既存の id を編集します。
4. 指定された ID に対して**iCloud**サービスがチェックされていることを確認します。

    [![](handoff-images/provision01.png "Enable the iCloud service for the given ID")](handoff-images/provision01.png#lightbox)
5. 変更内容を保存します。
6. **[プロビジョニングプロファイル]**  >  **[開発]** をクリックし、アプリ用の新しい開発プロビジョニングプロファイルを作成します。

    [![](handoff-images/provision02.png "Create a new development provisioning profile for the app")](handoff-images/provision02.png#lightbox)
7. 新しいプロビジョニングプロファイルをダウンロードしてインストールするか、Xcode を使用してプロファイルをダウンロードしてインストールします。
8. Xamarin. iOS プロジェクトのオプションを編集し、先ほど作成したプロビジョニングプロファイルを使用していることを確認します。

    [![](handoff-images/provision03.png "Select the provisioning profile just created")](handoff-images/provision03.png#lightbox)
9. 次に、**情報の plist**ファイルを編集し、プロビジョニングプロファイルの作成に使用したアプリ ID を使用していることを確認します。

    [![](handoff-images/provision04.png "Set App ID")](handoff-images/provision04.png#lightbox)
10. **[バックグラウンドモード]** セクションまでスクロールし、次の項目を確認します。

    [![](handoff-images/provision05.png "Enable the required background modes")](handoff-images/provision05.png#lightbox)
11. すべてのファイルに変更を保存します。

これらの設定を適用すると、アプリケーションはハンドオフフレームワーク Api にアクセスする準備ができました。 プロビジョニングの詳細については、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)に関するガイドを参照してください。

## <a name="implementing-handoff"></a>ハンドオフの実装

ユーザーアクティビティは、同じ開発者チーム ID で署名され、同じアクティビティの種類をサポートするアプリ間で継続できます。 Xamarin でのハンドオフの実装 iOS アプリでは、(`UIKit` または `AppKit`で) ユーザーアクティビティオブジェクトを作成し、オブジェクトの状態を更新してアクティビティを追跡し、受信デバイスでアクティビティを続行する必要があります。

### <a name="identifying-user-activities"></a>ユーザーアクティビティの識別

ハンドオフを実装するための最初の手順は、アプリでサポートされているユーザーアクティビティの種類を特定し、どのアクティビティが別のデバイスでの継続の候補として適しているかを確認することです。 例: ToDo アプリは、1つの_ユーザーアクティビティの種類_として項目の編集をサポートし、使用可能な項目の一覧を別の項目として参照することをサポートする場合があります。

アプリでは、アプリが提供する任意の関数に1つずつ、必要な数のユーザーアクティビティを作成できます。 アプリでは、ユーザーアクティビティの種類ごとに、種類のアクティビティが開始および終了するタイミングを追跡し、最新の状態情報を維持して別のデバイスでそのタスクを続行する必要があります。

ユーザーアクティビティは、同じチーム ID で署名されたすべてのアプリに対して継続できます。送信アプリと受信アプリの間に1対1のマッピングはありません。 たとえば、特定のアプリでは、別のデバイス上の異なる個別のアプリによって使用される4種類のアクティビティを作成できます。 これは、アプリの Mac バージョン (多くの機能と機能がある場合もあります) と iOS アプリ (各アプリが小さく、特定のタスクに重点が置かれている場合) の間で一般的に発生します。

### <a name="creating-activity-type-identifiers"></a>アクティビティの種類の識別子の作成

_アクティビティの種類の識別子_は、特定のユーザーアクティビティの種類を一意に識別するために使用される、アプリの**情報 plist**ファイルの `NSUserActivityTypes` 配列に追加される短い文字列です。 配列には、アプリがサポートするアクティビティごとに1つのエントリがあります。 Apple では、競合を避けるために、アクティビティの種類の識別子に対して逆引き DNS スタイルの表記を使用することを提案しています。 例: 特定のアプリベースのアクティビティの `com.company-name.appname.activity`、または複数のアプリで実行できるアクティビティの `com.company-name.activity`。

アクティビティの種類の識別子は、アクティビティの種類を識別するために `NSUserActivity` インスタンスを作成するときに使用されます。 アクティビティが別のデバイスで続行されると、アクティビティの種類 (アプリのチーム ID と共に) によって、アクティビティを続行するために起動するアプリが決まります。

例として、 **Monkeybrowser**というサンプルアプリを作成します ([ここでダウンロード](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)します)。 このアプリでは、4つのタブが表示され、それぞれの URL が web ブラウザービューで開かれます。 ユーザーは、アプリを実行している別の iOS デバイス上の任意のタブを続行できます。

この動作をサポートするために必要なアクティビティの種類の識別子を作成するには、**情報の plist**ファイルを編集し、**ソース**ビューに切り替えます。 `NSUserActivityTypes` キーを追加し、次の識別子を作成します。

[![](handoff-images/type01.png "The NSUserActivityTypes key and required identifiers in the plist editor")](handoff-images/type01.png#lightbox)

例の**Monkeybrowser**アプリの各タブに1つずつ、4つの新しいアクティビティの種類の識別子を作成しました。 独自のアプリを作成するときに、`NSUserActivityTypes` 配列の内容を、アプリがサポートするアクティビティに固有のアクティビティの種類の識別子に置き換えます。

### <a name="tracking-user-activity-changes"></a>ユーザーアクティビティの変更の追跡

`NSUserActivity` クラスの新しいインスタンスを作成するときに、アクティビティの状態の変更を追跡する `NSUserActivityDelegate` インスタンスを指定します。 たとえば、次のコードを使用して、状態の変更を追跡できます。

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

`UserActivityReceivedData` メソッドは、継続ストリームが送信元デバイスからデータを受信したときに呼び出されます。 詳細については、以下の「[継続ストリームのサポート](#supporting-continuation-streams)」セクションを参照してください。

`UserActivityWasContinued` メソッドは、別のデバイスが現在のデバイスからアクティビティを取得したときに呼び出されます。 ToDo リストに新しい項目を追加するなど、アクティビティの種類によっては、アプリが送信元デバイスでアクティビティを中止する必要がある場合があります。

`UserActivityWillSave` メソッドは、アクティビティに対する変更がローカルに保存され、ローカルで使用可能なデバイス間で同期される前に呼び出されます。 このメソッドを使用して、`NSUserActivity` インスタンスの `UserInfo` プロパティに対して、最後の1分間の変更を送信する前に行うことができます。

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity インスタンスの作成

アプリが別のデバイスで継続できる可能性を提供するために必要な各アクティビティは、`NSUserActivity` インスタンスにカプセル化する必要があります。 アプリでは、必要な数のアクティビティを作成できます。また、アクティビティの性質は、対象のアプリの機能と機能に依存します。 たとえば、電子メールアプリでは、新しいメッセージを作成するためのアクティビティを1つ作成し、メッセージを読み取るために別のアクティビティを作成できます。

このサンプルアプリでは、ユーザーがタブ付き web ブラウザービューのいずれかで新しい URL を入力するたびに、新しい `NSUserActivity` が作成されます。 次のコードは、指定されたタブの状態を格納します。

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

この例では、上で作成したユーザーアクティビティの種類のいずれかを使用して新しい `NSUserActivity` を作成し、ユーザーが判読できるアクティビティのタイトルを提供します。 これは、上記で作成した `NSUserActivityDelegate` のインスタンスに接続して状態の変化を監視し、このユーザーアクティビティが現在のアクティビティであることを iOS に通知します。

### <a name="populating-the-userinfo-dictionary"></a>UserInfo 辞書を設定しています

前述したように、`NSUserActivity` クラスの `UserInfo` プロパティは、特定のアクティビティの状態を定義するために使用されるキーと値のペアの `NSDictionary` です。 `UserInfo` に格納される値は、次のいずれかの型である必要があります: `NSArray`、`NSData`、`NSDate`、`NSDictionary`、`NSNull`、`NSNumber`、`NSSet`、`NSString`、または `NSURL`。 iCloud ドキュメントを指すデータ値は、受信デバイスで同じドキュメントを参照するように自動的に調整されます。 `NSURL`。

上の例では、`NSMutableDictionary` オブジェクトを作成し、ユーザーが現在表示している URL を指定したタブで1つのキーを設定しています。ユーザーアクティビティの `AddUserInfoEntries` 方法を使用して、受信デバイスでのアクティビティの復元に使用されるデータを使用してアクティビティを更新しました。

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple は、受信デバイスにアクティビティが適時に送信されるように、必要の最小値に送信される情報を保持することを提案します。 より大きな情報が必要な場合は、編集するドキュメントに添付されたイメージを送信する必要があるため、継続ストリームを使用する必要があります。 詳細については、後述の「[継続ストリームのサポート](#supporting-continuation-streams)」セクションを参照してください。

### <a name="continuing-an-activity"></a>アクティビティの続行

ハンドオフは、元のデバイスと物理的に近接し、同じ iCloud アカウントにサインインしているローカルの iOS および OS X デバイスに対して、継続的なユーザーアクティビティの可用性を自動的に通知します。 ユーザーが新しいデバイスでアクティビティを続行することを選択した場合、システムは (チーム ID とアクティビティの種類に基づいて) 適切なアプリを起動し、その継続が必要になる `AppDelegate` に情報を表示します。

最初に、`WillContinueUserActivityWithType` メソッドが呼び出されます。これにより、アプリは、継続が開始されようとしていることをユーザーに通知できます。 このサンプルアプリの**AppDelegate.cs**ファイルにある次のコードを使用して、継続の開始を処理します。

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

上の例では、各ビューコントローラーが `AppDelegate` に登録され、アクティビティインジケーターと、アクティビティが現在のデバイスに渡されることをユーザーに通知するパブリック `PreparingToHandoff` メソッドを持っています。 例:

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

指定されたアクティビティを実際に続行するために、`AppDelegate` の `ContinueUserActivity` が呼び出されます。 ここでも、サンプルアプリから次のようになります。

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

各ビューコントローラーのパブリック `PerformHandoff` メソッドは、実際にハンドオフを事前に行い、現在のデバイスでアクティビティを復元します。 この例の場合、ユーザーが別のデバイスで参照していた特定のタブに同じ URL が表示されます。 例:

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

`ContinueUserActivity` メソッドには、ドキュメントまたはレスポンダーベースのアクティビティを再開するために呼び出すことができる `UIApplicationRestorationHandler` が含まれています。 `NSArray` または復元可能なオブジェクトを、呼び出されたときに復元ハンドラーに渡す必要があります。 (例:

```csharp
completionHandler (new NSObject[]{Tab4});
```

渡されたオブジェクトごとに、その `RestoreUserActivityState` メソッドが呼び出されます。 各オブジェクトは、`UserInfo` ディクショナリ内のデータを使用して、独自の状態を復元できます。 (例:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

ドキュメントベースのアプリの場合、`ContinueUserActivity` メソッドを実装していない場合、または `false`を返した場合、`UIKit` または `AppKit` がアクティビティを自動的に再開できます。 詳細については、以下の「[ドキュメントベースのアプリでのハンドオフのサポート](#supporting-handoff-in-document-based-apps)」セクションを参照してください。

### <a name="failing-handoff-gracefully"></a>ハンドオフの正常エラー

ハンドオフは、大量の接続された iOS デバイスと OS X デバイス間での情報の送信に依存しているため、転送プロセスが失敗することがあります。 これらのエラーを適切に処理し、発生した状況をユーザーに通知するようにアプリを設計する必要があります。

エラーが発生した場合は、`AppDelegate` の `DidFailToContinueUserActivitiy` メソッドが呼び出されます。 (例:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

指定された `NSError` を使用して、エラーに関する情報をユーザーに提供する必要があります。

## <a name="native-app-to-web-browser-handoff"></a>ネイティブアプリから Web ブラウザーへのハンドオフ

ユーザーは、適切なネイティブアプリが目的のデバイスにインストールされていなくても、アクティビティを続行することができます。 場合によっては、web ベースのインターフェイスが必要な機能を提供し、アクティビティを続行することもできます。 たとえば、ユーザーの電子メールアカウントには、メッセージの作成と読み取りを行うための web ベースの UI が用意されている場合があります。

元のネイティブアプリが web インターフェイスの URL (および指定された項目の継続を識別するために必要な構文) を認識している場合、この情報は `NSUserActivity` インスタンスの `WebpageURL` プロパティでエンコードできます。 受信デバイスに、継続を処理する適切なネイティブアプリがインストールされていない場合は、提供された web インターフェイスを呼び出すことができます。

## <a name="web-browser-to-native-app-handoff"></a>Web ブラウザーからネイティブアプリへのハンドオフ

ユーザーが元のデバイスで web ベースのインターフェイスを使用していて、受信側のデバイス上のネイティブアプリが `WebpageURL` プロパティのドメイン部分を要求している場合、システムはそのアプリを使用して継続を処理します。 新しいデバイスは、アクティビティの種類を `BrowsingWeb` としてマークする `NSUserActivity` インスタンスを受け取ります。 `WebpageURL` には、ユーザーがアクセスしていた URL が含まれます。 `UserInfo` 辞書は空になります。

アプリがこの種類のハンドオフに参加するには、`<service>:<fully qualified domain name>` 形式 (例: `activity continuation:company.com`) を使用して `com.apple.developer.associated-domains` の権利でドメインを要求する必要があります。

指定されたドメインが `WebpageURL` のプロパティの値と一致する場合、ハンドオフは、そのドメインの web サイトから承認されたアプリ Id の一覧をダウンロードします。 Web サイトでは、 **apple-app-site-association**という名前の署名付き JSON ファイルに承認された id の一覧を提供する必要があります (たとえば、`https://company.com/apple-app-site-association`)。

この JSON ファイルには、`<team identifier>.<bundle identifier>`形式のアプリ Id の一覧を指定するディクショナリが含まれています。 (例:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

JSON ファイルに署名するには (正しい `Content-Type` の `application/pkcs7-mime`)、iOS によって信頼されている証明機関によって発行された証明書とキーを使用して、**ターミナル**アプリと `openssl` コマンドを使用します (一覧については、「 [https://support.apple.com/kb/ht5012](https://support.apple.com/kb/ht5012) 」を参照してください)。 (例:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl` コマンドは、web サイトに配置した署名付きの JSON ファイルを、 **apple アプリサイトの関連付け**URL に出力します。 (例:

```csharp
https://example.com/apple-app-site-association.
```

アプリは、`WebpageURL` ドメインが `com.apple.developer.associated-domains` の権利にあるすべてのアクティビティを受け取ります。 `http` プロトコルと `https` プロトコルのみがサポートされており、その他のプロトコルでは例外が発生します。

## <a name="supporting-handoff-in-document-based-apps"></a>ドキュメントベースのアプリでのハンドオフのサポート

前述のように、iOS と OS X では、アプリの**情報の plist**ファイルに `NSUbiquitousDocumentUserActivityType`の `CFBundleDocumentTypes` キーが含まれている場合、ドキュメントベースのアプリは iCloud ベースのドキュメントのハンドオフを自動的にサポートします。 (例:

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

この例では、文字列は、アクティビティの名前が付加された逆引き DNS アプリ指定子です。 このように入力した場合、アクティビティの種類のエントリは、**情報 plist**ファイルの `NSUserActivityTypes` 配列で繰り返す必要はありません。

自動的に作成されたユーザーアクティビティオブジェクト (ドキュメントの `UserActivity` プロパティを通じて利用可能) は、アプリ内の他のオブジェクトから参照でき、継続時に状態を復元するために使用されます。 たとえば、項目の選択とドキュメントの位置を追跡します。 状態が変化し、`UpdateUserActivityState` メソッドの `UserInfo` 辞書を更新するたびに、このアクティビティ `NeedsSave` プロパティを `true` に設定する必要があります。

`UserActivity` プロパティは、任意のスレッドから使用でき、キー値観察 (KVO) プロトコルに準拠しているため、iCloud との間で移動するときにドキュメントの同期を維持するために使用できます。 ドキュメントを閉じると、`UserActivity` プロパティは無効になります。

詳細については、[ドキュメントベースのアプリドキュメントの Apple のユーザーアクティビティサポート](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5)を参照してください。

## <a name="supporting-handoff-in-responders"></a>応答側でのハンドオフのサポート

`UserActivity` プロパティを設定することによって、(iOS の `UIResponder` または OS X 上の `NSResponder` のいずれかから継承された) レスポンダーを活動に関連付けることができます。 システムは、`UserActivity` プロパティを適切なタイミングで自動的に保存します。応答側の `UpdateUserActivityState` メソッドを呼び出して、`AddUserInfoEntriesFromDictionary` メソッドを使用して現在のデータをユーザーアクティビティオブジェクトに追加します。

## <a name="supporting-continuation-streams"></a>継続ストリームのサポート

は、アクティビティを続行するために必要な情報量が、初期ハンドオフペイロードによって効率的に転送されない場合があります。 このような状況では、受信側のアプリは、データを転送するために、それ自体と元のアプリの間に1つ以上のストリームを確立できます。

元のアプリは、`NSUserActivity` インスタンスの `SupportsContinuationStreams` プロパティを `true`に設定します。 (例:

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

受信側アプリは `AppDelegate` 内の `NSUserActivity` の `GetContinuationStreams` メソッドを呼び出して、ストリームを確立できます。 (例:

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

元のデバイスでは、ユーザーアクティビティデリゲートは、`DidReceiveInputStream` メソッドを呼び出してストリームを受信します。これにより、再開中のデバイスでユーザーアクティビティを続行するために要求されたデータが提供されます。

ストリームデータへの読み取り専用アクセスを提供するには `NSInputStream` を使用し、`NSOutputStream` は書き込み専用アクセスを提供します。 ストリームは要求と応答の形式で使用する必要があります。受信側のアプリは、さらに多くのデータを要求し、発信元のアプリがそれを提供します。 そのため、元のデバイスの出力ストリームに書き込まれるデータは、継続しているデバイスの入力ストリームから読み取られます。逆の場合も同様です。

継続ストリームが必要な場合でも、2つのアプリ間の通信は、ごくわずかにする必要があります。

詳細については、「Apple の[継続ストリームの使用](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13)」のドキュメントを参照してください。

## <a name="handoff-best-practices"></a>ハンドオフのベストプラクティス

ハンドオフによるユーザーアクティビティのシームレスな継続を正常に実装するには、関連するさまざまなコンポーネントがあるため、慎重に設計する必要があります。 Apple では、ハンドオフが有効になっているアプリに関する次のベストプラクティスを採用することを提案します。

- アクティビティの状態を継続できるようにするために可能な最小のペイロードを要求するように、ユーザーアクティビティを設計します。 ペイロードが大きいほど、継続の開始にかかる時間が長くなります。
- 継続を成功させるために大量のデータを転送する必要がある場合は、構成およびネットワークのオーバーヘッドに関連するコストを考慮してください。
- 大規模な Mac アプリでは、iOS デバイス上の複数の小さなタスク固有のアプリによって処理されるユーザーアクティビティを作成するのが一般的です。 アプリと OS のバージョンは、連携して動作するように設計されているか、適切に失敗します。
- アクティビティの種類を指定する場合は、競合を回避するために、逆引き DNS 表記を使用します。 アクティビティが特定のアプリに固有のものである場合は、その名前を型定義に含める必要があります (`com.myCompany.myEditor.editing`など)。 アクティビティが複数のアプリで動作する場合は、定義からアプリ名を削除します (たとえば `com.myCompany.editing`)。
- アプリでユーザーアクティビティの状態を更新する必要がある場合 (`NSUserActivity`)、`NeedsSave` プロパティを `true`に設定します。 適切なタイミングで、ハンドオフはデリゲートの `UserActivityWillSave` メソッドを呼び出し、必要に応じて `UserInfo` 辞書を更新できるようにします。
- ハンドオフプロセスは受信側のデバイスですぐに初期化されない可能性があるため、`AppDelegate`の `WillContinueUserActivity` を実装し、継続が開始されることをユーザーに通知する必要があります。

## <a name="example-handoff-app"></a>ハンドオフアプリの例

Xamarin iOS アプリでハンドオフを使用する例として、このガイドには[**Monkeybrowser**](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)サンプルアプリが含まれています。 アプリには4つのタブがあり、ユーザーは web を閲覧するために使用できます。各タブには、天気、お気に入り、コーヒーの休憩、仕事など、特定の種類のアクティビティがあります。

任意のタブで、ユーザーが新しい URL を入力して **[移動**] ボタンをタップすると、そのタブに新しい `NSUserActivity` が作成され、ユーザーが現在参照している url が表示されます。

[![](handoff-images/handoff01.png "Example Handoff App")](handoff-images/handoff01.png#lightbox)

別のユーザーのデバイスに**Monkeybrowser**アプリがインストールされている場合は、同じユーザーアカウントを使用して iCloud にサインインし、同じネットワーク上にあり、上のデバイスに近接しているときに、ハンドオフアクティビティがホーム画面に表示されます (下部)。左上隅):

[![](handoff-images/handoff02.png "The Handoff Activity displayed on the home screen in the lower left hand corner")](handoff-images/handoff02.png#lightbox)

ユーザーがハンドオフアイコンを上にドラッグすると、アプリが起動され、`NSUserActivity` で指定されたユーザーアクティビティが新しいデバイスで続行されます。

[![](handoff-images/handoff03.png "The User Activity continued on the new device")](handoff-images/handoff03.png#lightbox)

ユーザーアクティビティが別の Apple デバイスに正常に送信されると、送信元デバイスの `NSUserActivity` は、ユーザーアクティビティが別のデバイスに正常に転送されたことを通知するために、その `NSUserActivityDelegate` で `UserActivityWasContinued` メソッドの呼び出しを受信します。

## <a name="summary"></a>まとめ

この記事では、ユーザーの Apple デバイスの複数のユーザーアクティビティを続行するために使用されるハンドオフフレームワークの概要について説明しました。 次に、Xamarin iOS アプリでハンドオフを有効にして実装する方法を示しました。 最後に、さまざまな種類のハンドオフ継続の使用方法と、ハンドオフのベストプラクティスについて説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [MonkeyBrowser のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [ホームキットのユーザーインターフェイスのガイドライン](https://developer.apple.com/homekit/ui-guidelines/)
- [ホームキットフレームワークリファレンス](https://developer.apple.com/library/ios/home_kit_framework_ref)
