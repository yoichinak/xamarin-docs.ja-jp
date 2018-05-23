---
title: iCloud
description: Apple では、アプリケーションを Apple のサーバーにデータを格納して、それを (それらの Apple ID) を使用して、同じユーザーが使用されるすべてのデバイス間で同期を許可するためのサービスとして 5、iOS で iCloud が導入されました。 さらに、バックアップ コンポーネントでは、ここで、デバイス上のデータはバックアップを Apple のサーバー。 このドキュメントでは、Apple によって iCloud 提供の Api の一部を使用して格納および c# のサンプルをキーと値の小さいデータのペアを格納して、ドキュメントを格納するために、それぞれのサーバーからデータを取得する方法について説明します。 ICloud のバックアップが、アプリケーションの設計に影響する方法も説明します。
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: a62d4621a8f3ace64401d64e35c806317a591c03
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
---
# <a name="icloud"></a>iCloud

_Apple では、アプリケーションを Apple のサーバーにデータを格納して、それを (それらの Apple ID) を使用して、同じユーザーが使用されるすべてのデバイス間で同期を許可するためのサービスとして 5、iOS で iCloud が導入されました。さらに、バックアップ コンポーネントでは、ここで、デバイス上のデータはバックアップを Apple のサーバー。このドキュメントでは、Apple によって iCloud 提供の Api の一部を使用して格納および c# のサンプルをキーと値の小さいデータのペアを格納して、ドキュメントを格納するために、それぞれのサーバーからデータを取得する方法について説明します。ICloud のバックアップが、アプリケーションの設計に影響する方法も説明します。_

5、iOS で iCloud ストレージ API には、中央の場所にユーザーのドキュメントおよびアプリケーションに固有のデータを保存し、すべてのユーザーのデバイスからそれらの項目にアクセスするアプリケーションができます。

記憶域の 4 種類あります。

- **キーと値の記憶域**- で少量のアプリケーションを使用してデータを共有する他のデバイスのユーザーのです。

- **UIDocument ストレージ**- UIDocument のサブクラスを使用して、ユーザーの iCloud アカウントに、ドキュメントやその他のデータを格納します。

- **CoreData** -SQLite データベース ストレージです。

- **個々 のファイルとディレクトリ**- ファイル システムに直接多数の異なるファイルを管理するためです。

このドキュメントでは、最初の 2 種類のキーと値のペアと UIDocument サブクラス - Xamarin.iOS でこれらの機能を使用する方法について説明します。

> [!IMPORTANT]
> Apple[ツールを提供](https://developer.apple.com/support/allowing-users-to-manage-data/)開発者が、欧州連合の一般的なデータ保護規制 (GDPR) を適切に処理します。

## <a name="requirements"></a>要件

- Xamarin.iOS の最新の安定バージョン
- Xcode 8 以降
- Mac または Visual Studio 2015 以降用の visual Studio です。

## <a name="preparing-for-icloud-development"></a>ICloud 開発のための準備

アプリケーションで iCloud 両方を使用するように構成する必要があります、 [Apple プロビジョニング ポータル](https://developer.apple.com/account/ios/overview.action)およびプロジェクト自体です。 ICloud 用の開発 (または、サンプルを試用して) は、次の手順を実行前にします。

ICloud にアクセスするアプリケーションを正しく構成するには。

-   **検索、TeamID** -へのログイン[developer.apple.com](http://developer.apple.com)しを参照してください、 **Member Center > アカウント > 開発者アカウントの概要**チーム ID (または開発者の 1 つの個別の ID を取得するには). これは 10 文字の文字列になります ( **A93A5CM278**など)-これが、「コンテナーの識別子」の一部を形成します。

-   **新しいアプリ ID の作成**アプリ ID を作成、」の手順を次に、[ストア テクノロジ ガイドのセクションで、デバイスのプロビジョニング用にプロビジョニング](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)を必ず確認して**iCloud**として、サービスを使用できます。

 [![](introduction-to-icloud-images/icloud-sml.png "許可されているサービスとしての iCloud を確認します。")](introduction-to-icloud-images/icloud.png#lightbox)

- **新しいプロビジョニング プロファイルを作成**-、プロビジョニング プロファイルを作成する」の手順に従って、[ガイドのデバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile)です。

- **Entitlements.plist に、コンテナーの識別子を追加**-コンテナーの識別子の形式は`TeamID.BundleID`します。 詳細についてを参照してください、[権利に関する作業](~/ios/deploy-test/provisioning/entitlements.md)ガイドです。

- **プロジェクトのプロパティを構成する**- Info.plist のファイルを確認してください、**バンドル Id**と一致する、**バンドル ID**場合は、設定[アプリ ID を作成する](~/ios/deploy-test/provisioning/capabilities/index.md)です。IOS バンドル署名 使用して、**プロビジョニング プロファイル**iCloud App Service でアプリ ID が含まれていると**カスタム権利**ファイルを選択します。 これは、ことができますすべてで実行する Visual Studio プロジェクトのプロパティ] ウィンドウの [します。

- **デバイスを iCloud を有効にする**に移動して -**設定 > iCloud**にデバイスを記録することを確認してください。
選択して有効に、**ドキュメントおよびデータ**オプション。

- **ICloud をテストするデバイスを使用する必要があります**-シミュレーターでは機能しません。
実際には、次の 2 つまたは複数のデバイスすべての動作の iCloud を表示するのと同じ Apple ID でサインイン本当に必要があります。


## <a name="key-value-storage"></a>記憶域のキー値

記憶域のキー値は、少量のデータをユーザーが書籍や雑誌に表示する最後のページなどの間で永続化されたデバイス向けです。 バックアップ データのキーと値のストレージを使用する必要があります。

キーと値の記憶域を使用する場合の注意すべきいくつかの制限があります。

- **キーの最大サイズ**-キー名は 64 バイトより長くすることはできません。

- **最大値のサイズ**-単一の値に 64 キロバイト以上を格納することはできません。

- **アプリの最大のキー値ストアのサイズ**-アプリケーションが合計で最大 64 キロバイトのキー値のデータをのみ格納できます。 その制限を超えるキーを設定する試行は失敗し、前の値が保持されます。

- **データ型**- 基本型のみなどの文字列、数値とブール値を格納することができます。

**ICloudKeyValue**の例では、そのしくみを示しています。 サンプル コードは、各デバイスのという名前のキーを作成します。 1 つのデバイスでこのキーを設定および他のユーザーに反映される値を見ることができます。 また、一度に多くのデバイスで編集する場合は、"Shared"- 任意のデバイスで編集することができますと呼ばれるキーを作成、iCloud が決定"wins"(変更のタイムスタンプを使用) を値と伝達を取得します。

このスクリーン ショットは、使用中で、サンプルを示します。 ICloud からの変更通知が受信されると、画面の下部のスクロール、テキスト ビューで印刷され入力フィールドで更新します。



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "デバイスの間でメッセージ フロー")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>設定とデータの取得

このコードは、文字列値を設定する方法を示します。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

同期を呼び出すこととは、ローカル ディスクの記憶域のみに値が永続化することを確認します。 ICloud に同期では、バック グラウンドでは行われ、「適用できない」アプリケーション コードによって。 良好なネットワーク接続と同期は多くの場合、内に始まります 5 (秒単位) が、ネットワークが低い (または切断されている) 場合更新プログラムがかなり長くかかる場合があります。

このコードを持つ値を取得できます。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

値は、ローカル データ ストアから取得された - このメソッドは、「最新」の値を取得する iCloud サーバーに接続しようとはしません。 iCloud 独自のスケジュールに従ってローカル データ ストアが更新されます。

### <a name="deleting-data"></a>データの削除

キー/値ペアを完全に削除するには、次のように Remove メソッドを使用します。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>変更を確認します。

値が iCloud にオブザーバーを追加することで変更したときに、アプリケーションは通知を受信もできる、`NSNotificationCenter.DefaultCenter`です。
次のコードから**KeyValueViewController.cs** `ViewWillAppear`メソッドは、これらの通知をリッスンし、キーが変更されているリストを作成する方法を示します。

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

コードは、これらのローカル コピーを更新または新しい値で UI を更新など、変更されたキーの一覧で何らかのアクションを実行できます。

可能な変更の理由: ServerChange (0)、InitialSyncChange (1) または QuotaViolationChange (2)。 理由をアクセスし、必要な場合は、さまざまな処理を実行することができます (たとえばの結果として一部のキーを削除する必要があります、 *QuotaViolationChange*)。

## <a name="document-storage"></a>ドキュメントの格納

iCloud ドキュメントの格納はアプリ (およびユーザー) に重要なデータを管理するよう設計されています。 ファイルと、アプリが実行するには、同時 iCloud ベースのバックアップを提供して、すべてのユーザーのデバイス間で共有機能の中に必要なその他のデータを管理するために使用します。

この図では、すべてに収まるかを一緒に示しています。 各デバイスでは、ローカル ストレージ (UbiquityContainer) とデーモンは、クラウド内のデータの送受信の対処、オペレーティング システムの iCloud に保存されたデータがあります。 FilePresenter/FileCoordinator の同時アクセスを防ぐために使用して、UbiquityContainer へのすべてのファイル アクセスを行う必要があります。 `UIDocument`クラスを実装するものです。 UIDocument を使用する方法を示します。

 [![](introduction-to-icloud-images/icloud-overview.png "ドキュメントの記憶域の概要")](introduction-to-icloud-images/icloud-overview.png#lightbox)

ICloudUIDoc 例では、実装、単純な`UIDocument`1 つのテキスト フィールドを含むサブクラスです。 テキストが表示されます、`UITextView`と赤で表示される通知メッセージでその他のデバイスを iCloud での編集が反映されます。 サンプル コードは、競合の解決などのより高度な iCloud 機能では扱いません。

このスクリーン ショットは、テキストを変更し、キーを押しての後に、サンプル アプリケーションを示しています。 **UpdateChangeCount**ドキュメントがその他のデバイスを iCloud 経由で同期されています。

 [![](introduction-to-icloud-images/iclouduidoc.png "このスクリーン ショットは、UpdateChangeCount を押し、テキストの変更後のサンプル アプリケーションを示しています。")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

これには、iCloudUIDoc サンプルに 5 つの部分があります。

1. **アクセス、UbiquityContainer** -iCloud が有効になっている場合を判断、アプリケーションの iCloud 記憶域へのパス。

1. **UIDocument サブクラスを作成する**-iCloud 記憶域と、モデル オブジェクト間を仲介するクラスを作成します。

1. **検索と iCloud のドキュメントを開く**-使用`NSFileManager`と`NSPredicate`を iCloud ドキュメントを検索して開くこと。

1. **ICloud のドキュメントを表示する**-からプロパティを公開、 `UIDocument` UI コントロールと対話できるようにします。

1. **ICloud 文書を保存する**-UI で行われた変更はディスクと iCloud に保存されていることを確認してください。

ICloud のすべての操作を実行 (または実行する必要があります) の処理を待機中をブロックしないように非同期的にします。 これを実現するサンプルの 3 つの方法が表示されます。

 **スレッド**-`AppDelegate.FinishedLaunching`初めて呼び出したときに`GetUrlForUbiquityContainer`はメイン スレッドをブロックしないように別のスレッドで行われます。

 **NotificationCenter** - ときに非同期の通知の登録などの操作`NSMetadataQuery.StartQuery`完了します。

 **完了ハンドラー**と同様に、非同期操作の完了時に実行する方法を渡して -`UIDocument.Open`です。

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer へのアクセス

ICloud が有効になっているかどうかと、その場合を判断する iCloud ドキュメントの格納を使用して最初の手順は、「ユビキタス コンテナー」の場所 (iCloud が有効なファイルをデバイスに格納する場所のディレクトリ)。

このコードは、`AppDelegate.FinishedLaunching`サンプルのメソッドです。

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

このサンプルはエコーしません、Apple がアプリが前面に表示されるたびに、GetUrlForUbiquityContainer を呼び出すことをお勧めします。

### <a name="creating-a-uidocument-subclass"></a>UIDocument サブクラスを作成します。

すべての iCloud ファイルとディレクトリ (ie。 UbiquityContainer ディレクトリに格納されているもの) NSFileManager メソッド、NSFilePresenter プロトコルを実装すると、NSFileCoordinator 経由での書き込みを使用して管理する必要があります。
ユーザー自身がこれを行う UIDocument サブクラスに記述していないのすべてを実行する最も簡単な方法は、all をします。

ICloud を使用する UIDocument サブクラスに実装する必要がある 2 つの方法はあります。

- **LoadFromContents**のモデルのクラス/es に展開するためのファイルの内容の NSData に渡します。

- **ContentsForType** -ディスク (とクラウド) に保存するモデルのクラス/es の NSData 形式の指定を要求します。

このサンプル コードから**iCloudUIDoc\MonkeyDocument.cs** UIDocument を実装する方法を示します。

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

データ モデルがここでは非常に単純に 1 つのテキスト フィールド。 データ モデルは必要ですが、Xml ドキュメントまたはバイナリ データなどのように複雑にできます。 UIDocument 実装の主な役割では、モデルのクラスとディスクに保存/読み込みできる NSData 表現の間で変換します。

### <a name="finding-and-opening-icloud-documents"></a>検索と iCloud のドキュメントを開く

サンプル アプリのみが扱う 1 つのファイル - test.txt のため、内のコード **appdelegate.cs** を作成、`NSPredicate`と`NSMetadataQuery`を具体的にはそのファイル名を探します。 `NSMetadataQuery`非同期で実行され、それが完了したら、通知を送信します。 `DidFinishGathering` 通知のオブザーバーによって呼び出されます、クエリを停止およびを使用して LoadDocument を呼び出し、`UIDocument.Open`ファイルを読み込み、表示しようとする完了ハンドラーを持つメソッド、`MonkeyDocumentViewController`です。

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>ICloud のドキュメントを表示します。

その他のモデル クラスを他と同じにすることはできません、UIDocument を表示します。
- プロパティは、可能性のあるユーザーを編集し、モデルに書き戻さ、UI コントロールに表示されます。

この例で**iCloudUIDoc\MonkeyDocumentViewController.cs** MonkeyDocument のテキストが表示されます、`UITextView`です。 `ViewDidLoad` 送信された通知をリッスン、`MonkeyDocument.LoadFromContents`メソッドです。 `LoadFromContents` 通知は、ドキュメントが更新されたことを示すように、iCloud がある新しいデータ ファイルはときに、呼び出されます。

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

サンプル コードの通知のハンドラーは、競合の検出または解決策を行わなくてもここでは、UI を更新するメソッドを呼び出します。

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>ICloud ドキュメントを保存しています

Icloud を呼び出すことができます、UIDocument を追加する`UIDocument.Save`直接 (新しいドキュメントのみ) の既存ファイルを使用して、移動または`NSFileManager.DefaultManager.SetUbiquitious`です。 コード例は、次のコード偏在性に関して最近コンテナー内で直接新しい文書を作成 (2 つ完了ハンドラーがここでは、1 つずつ、`Save`操作と open)。

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

ドキュメントへの後続の変更「は保存されず」直接代わりにお知らせ、`UIDocument`が変更されるとする`UpdateChangeCount`、およびディスクの操作への保存をスケジュールが自動的に。

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>ICloud のドキュメントを管理します。

ユーザーが iCloud ドキュメントを管理できます、**ドキュメント**; の設定を使用してアプリケーションの外部で「ユビキタス コンテナー」のディレクトリを削除する方向にスワイプとファイルの一覧を表示できます。 アプリケーション コードでは、ユーザーがドキュメントを削除する場所の状況を処理できる必要があります。 内部アプリケーションのデータを格納しないでください、**ドキュメント**ディレクトリ。

 [![](introduction-to-icloud-images/icloudstorage.png "ICloud ドキュメント ワークフローの管理")](introduction-to-icloud-images/icloudstorage.png#lightbox)



ユーザーは、そのアプリケーションに関連するドキュメントを iCloud の状態のことを通知するために、自分のデバイスから iCloud 対応のアプリケーションを削除しようとするときに、各種の警告を表示もされます。

 [![](introduction-to-icloud-images/icloud-delete1.png "ユーザーが iCloud を有効にしたアプリケーションを自分のデバイスから削除しようとした場合のサンプル ダイアログ")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "ユーザーが iCloud を有効にしたアプリケーションを自分のデバイスから削除しようとした場合のサンプル ダイアログ")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud のバックアップ

ICloud へのバックアップには、開発者によって直接アクセスする機能がない場合は、中に、アプリケーションを設計する方法、ユーザー エクスペリエンスに影響します。
Apple 提供[iOS データ記憶域ガイドライン](http://developer.apple.com/icloud/documentation/data-storage/)するために、iOS アプリケーションの開発者向けです。

最も重要な考慮事項は、アプリではありません (たとえば、雑誌リーダー保存するアプリケーションを問題ごとのコンテンツの hundred-plus メガバイト単位) のユーザーによって生成される大きなファイルを格納するかどうかです。 Apple では、この種の場所があるバックアップを iCloud にし、ユーザーの iCloud クォータを不必要に入力データを格納しないようにすることが推奨されます。

大量の次のようにデータを格納するアプリケーションで必要がありますバックアップ (例えばはユーザーのディレクトリのいずれかで保存するか。 キャッシュまたは tmp) を使用または`NSFileManager.SetSkipBackupAttribute`iCloud のバックアップ操作中にそれらが無視されるように、それらのファイルにフラグを適用します。

## <a name="summary"></a>まとめ

この記事では、iOS 5 に含まれている新しい iCloud 機能が導入されました。 ICloud を使用するプロジェクトを構成するために必要し、iCloud の機能を実装する方法の例を提供し、手順が調べられます。

記憶域のキーと値の例では、iCloud を使用して、少量の NSUserPreferences が格納されているのと同様のデータを格納する方法を説明します。 UIDocument 例より多くの複雑なデータを格納し、iCloud を使用して複数のデバイス間で同期を示しました。

最後に iCloud のバックアップを追加する必要があります、アプリケーションの設計に影響するしくみについて簡単に説明が含まれています。



## <a name="related-links"></a>関連リンク

- [概要を iCloud (サンプル)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud セミナー サンプル コード](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud セミナー スライド](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud 記憶域](http://support.apple.com/kb/HT4847)
