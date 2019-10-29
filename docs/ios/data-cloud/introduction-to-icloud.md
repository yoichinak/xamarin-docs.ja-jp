---
title: Xamarin で iCloud を使用する
description: このドキュメントでは、iCloud と、Xamarin の iOS アプリケーションでの使用について説明します。 ここでは、キー値のストレージ、ドキュメントストレージ、iCloud のバックアップについて説明します。
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/09/2016
ms.openlocfilehash: 843a6212be44778f9637de631398a56a1d633a69
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016981"
---
# <a name="using-icloud-with-xamarinios"></a>Xamarin で iCloud を使用する

IOS 5 の iCloud ストレージ API を使用すると、アプリケーションはユーザードキュメントとアプリケーション固有のデータを中央の場所に保存し、すべてのユーザーのデバイスからそれらの項目にアクセスできます。

使用できるストレージには、次の4種類があります。

- **キー値のストレージ**-少量のデータをユーザーの他のデバイス上のアプリケーションと共有します。

- **Uidocument ストレージ**-uidocument のサブクラスを使用して、ドキュメントやその他のデータをユーザーの iCloud アカウントに格納します。

- **Coredata** -SQLite データベースストレージ。

- **個々のファイルとディレクトリ**-ファイルシステムで、多数のファイルを直接管理します。

このドキュメントでは、最初の2つの型のキーと値のペアと UIDocument サブクラスについて説明し、これらの機能を Xamarin. iOS で使用する方法について説明します。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="requirements"></a>［要件］

- 最新の安定したバージョンの Xamarin. iOS
- Xcode 10
- Visual Studio for Mac または Visual Studio 2019。

## <a name="preparing-for-icloud-development"></a>ICloud 開発の準備

アプリケーションは、 [Apple Provisioning Portal](https://developer.apple.com/account/ios/overview.action)とプロジェクト自体の両方で iCloud を使用するように構成する必要があります。 ICloud 用に開発する前に (またはサンプルを試す)、次の手順に従います。

ICloud にアクセスするようにアプリケーションを正しく構成するには:

- **TeamID を検索**して[developer.apple.com](https://developer.apple.com)にログインし、**アカウント > メンバーセンターにアクセスして > 開発者アカウントの概要**を参照し、チーム id (または1人の開発者向けの個別の id) を取得します。 これは10文字の文字列 ( **A93A5CM278**など) です。これは "コンテナー識別子" の一部です。

- **新しいアプリ id を作成**する-アプリ id を作成するには、 [「Device Provisioning guide」の「ストアテクノロジのプロビジョニング」セクション](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)に記載されている手順に従って、 **iCloud**を許可されたサービスとして確認します。

 [![](introduction-to-icloud-images/icloud-sml.png "Check iCloud as an allowed service")](introduction-to-icloud-images/icloud.png#lightbox)

- **新しいプロビジョニングプロファイルを作成**する-プロビジョニングプロファイルを作成するには、「 [Device Provisioning Guide (デバイスプロビジョニングガイド](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device))」に記載されている手順に従います。

- **コンテナー識別子を追加します。 plist** -コンテナー識別子形式は `TeamID.BundleID`です。 詳細については、「[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)」ガイドを参照してください。

- **プロジェクトのプロパティを構成**する-情報ファイルで、**バンドル**id が[アプリ id を作成](~/ios/deploy-test/provisioning/capabilities/index.md)するときに設定した**バンドル id**と一致することを確認します。IOS バンドル署名では、iCloud App Service のアプリ ID と、選択した**カスタム権利**ファイルを含む**プロビジョニングプロファイル**を使用します。 これはすべて、Visual Studio のプロジェクトプロパティペインで実行できます。

- **デバイスで Icloud を有効**にする-[**設定] > iCloud**にアクセスし、デバイスがログインしていることを確認します。
**[ドキュメント & データ]** オプションをオンまたはオンにします。

- **ICloud をテストするにはデバイスを使用する必要があり**ます。これはシミュレーターでは機能しません。
実際には、複数のデバイスが同じ Apple ID でサインインしていて、iCloud が動作していることを確認する必要があります。

## <a name="key-value-storage"></a>キー値のストレージ

キー値ストレージは少量のデータを対象としています。ユーザーは、本や雑誌で最後に閲覧したページなど、デバイス間で永続化することができます。 キー値の記憶域は、バックアップデータには使用しないでください。

キー値ストレージを使用する場合に注意する必要がある制限事項がいくつかあります。

- **キーの最大サイズ**-キー名は64バイトより長くすることはできません。

- **最大値サイズ**-1 つの値に 64 kb を超える値を格納することはできません。

- **アプリのキー値ストアの最大サイズ**-アプリケーションは、最大 64 kb のキー値データを合計で格納できます。 この制限を超えてキーを設定しようとすると失敗し、前の値が保持されます。

- **データ型**-文字列、数値、ブール値などの基本型のみを格納できます。

**ICloudKeyValue**の例は、そのしくみを示しています。 このサンプルコードでは、各デバイスに対してという名前のキーを作成します。1つのデバイスにこのキーを設定し、値が他のデバイスに反映されるようにすることができます。 また、任意のデバイスで編集可能な "共有" という名前のキーも作成します。多数のデバイスで一度に編集すると、iCloud によって、"wins" (変更でタイムスタンプを使用) と反映される値が決まります。

このスクリーンショットは、使用されているサンプルを示しています。 ICloud から変更通知を受信すると、画面の下部にあるスクロールテキストビューに印刷され、入力フィールドで更新されます。

 [![](introduction-to-icloud-images/icloud-kv-arrows.png "The flow of messages between devices")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>データの設定と取得

このコードは、文字列値を設定する方法を示しています。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Synchronize を呼び出すと、値はローカルディスクストレージにのみ保存されます。 ICloud への同期はバックグラウンドで行われるため、アプリケーションコードによって "強制" することはできません。 ネットワーク接続が良好な場合、同期は5秒以内に実行されますが、ネットワークが不足している (または切断されている) 場合は、更新にかなりの時間がかかることがあります。

次のコードを使用して値を取得できます。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

値はローカルデータストアから取得されます。このメソッドは、iCloud サーバーに接続して "latest" 値を取得しようとしません。 iCloud は、独自のスケジュールに従ってローカルデータストアを更新します。

### <a name="deleting-data"></a>データの削除

キーと値のペアを完全に削除するには、Remove メソッドを次のように使用します。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>変更の監視

アプリケーションでは、`NSNotificationCenter.DefaultCenter`にオブザーバーを追加することによって、iCloud によって値が変更された場合に通知を受け取ることもできます。
次の**KeyValueViewController.cs** `ViewWillAppear` メソッドのコードは、これらの通知をリッスンし、変更されたキーの一覧を作成する方法を示しています。

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

コードでは、変更されたキーの一覧を使用して何らかのアクションを実行できます。たとえば、ローカルコピーを更新したり、UI を新しい値で更新したりすることができます。

変更の理由としては、ServerChange (0)、InitialSyncChange (1)、または QuotaViolationChange (2) が考えられます。 理由にアクセスし、必要に応じてさまざまな処理を実行できます (たとえば、 *QuotaViolationChange*の結果として一部のキーを削除する必要がある場合など)。

## <a name="document-storage"></a>ドキュメントストレージ

iCloud ドキュメントストレージは、アプリ (およびユーザー) にとって重要なデータを管理するように設計されています。 これを使用すると、アプリで実行する必要のあるファイルやその他のデータを管理できます。同時に、すべてのユーザーのデバイスで iCloud ベースのバックアップと共有機能を提供します。

この図は、すべてがどのように組み合わされているかを示しています。 各デバイスには、ローカルストレージ (UbiquityContainer) に保存されたデータがあり、オペレーティングシステムの iCloud デーモンはクラウドでのデータの送受信を行います。 同時アクセスを防止するには、UbiquityContainer へのすべてのファイルアクセスを FilePresenter/FileCoordinator で実行する必要があります。 これらのクラスは、`UIDocument` クラスによって実装されます。この例は、UIDocument の使用方法を示しています。

 [![](introduction-to-icloud-images/icloud-overview.png "The document storage overview")](introduction-to-icloud-images/icloud-overview.png#lightbox)

ICloudUIDoc の例では、単一のテキストフィールドを含む単純な `UIDocument` サブクラスを実装しています。 テキストは `UITextView` に表示され、iCloud によって、通知メッセージが赤で示された他のデバイスに反映されます。 このサンプルコードでは、競合解決など、より高度な iCloud 機能については扱いません。

このスクリーンショットは、サンプルアプリケーションを示しています。テキストを変更し、 **Updatechangecount**を押すと、ドキュメントは iCloud 経由で他のデバイスに同期されます。

 [![](introduction-to-icloud-images/iclouduidoc.png "This screenshot shows the sample application after changing the text and pressing UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

ICloudUIDoc サンプルには、次の5つの部分があります。

1. **UbiquityContainer へのアクセス**-icloud が有効になっているかどうかを判断し、その場合はアプリケーションの icloud ストレージ領域へのパスを確認します。

1. **UIDocument サブクラスの作成**-iCloud ストレージとモデルオブジェクトの間の中間にクラスを作成します。

1. **Icloud ドキュメントの検索とオープン**-`NSFileManager` と `NSPredicate` を使用して icloud ドキュメントを検索し、開くことができます。

1. **ICloud ドキュメントの表示**-UI コントロールと対話できるように、`UIDocument` のプロパティを公開します。

1. **Icloud ドキュメントの保存**-UI で行った変更がディスクと iCloud に永続化されていることを確認します。

すべての iCloud 操作は非同期に実行されるので、何かの処理を待機している間はブロックされません。 サンプルでこれを実現するには、次の3つの方法があります。

 `AppDelegate.FinishedLaunching`**スレッド**では、メインスレッドがブロックされないように、`GetUrlForUbiquityContainer` への最初の呼び出しが別のスレッドで実行されます。

 **Notificationcenter** -`NSMetadataQuery.StartQuery` などの非同期操作が完了したときの通知の登録。

 **完了ハンドラー** -`UIDocument.Open`のような非同期操作の完了時に実行するメソッドを渡します。

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer へのアクセス

ICloud ドキュメントストレージを使用する最初の手順では、iCloud が有効になっているかどうかを判断し、その場合は "ユビキタス container" (iCloud が有効なファイルがデバイスに格納されているディレクトリ) の場所を確認します。

このコードは、サンプルの `AppDelegate.FinishedLaunching` メソッドに含まれています。

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

このサンプルではありませんが、Apple では、アプリがフォアグラウンドになるたびに GetUrlForUbiquityContainer を呼び出すことをお勧めします。

### <a name="creating-a-uidocument-subclass"></a>UIDocument サブクラスの作成

すべての iCloud ファイルとディレクトリ (UbiquityContainer ディレクトリに格納されているすべてのもの) は、NSFileManager メソッドを使用して管理し、Nsfilemanager プロトコルを実装し、NSFileCoordinator を使用して書き込む必要があります。
すべての操作を実行する最も簡単な方法は、自分で記述するのではなく、すべてを実行する UIDocument をサブクラス化することです。

ICloud を使用するには、UIDocument サブクラスに実装する必要があるメソッドは2つだけです。

- **LoadFromContents** -ファイルの内容の nsdata を渡して、モデルクラス/es にアンパックします。

- [データの**種類**]-モデルクラス/Es の nsdata 表現を指定して、ディスク (およびクラウド) に保存するように要求します。

**ICloudUIDoc\MonkeyDocument.cs**からのこのサンプルコードは、uidocument を実装する方法を示しています。

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

この場合のデータモデルは非常に単純で、1つのテキストフィールドです。 データモデルは、Xml ドキュメントやバイナリデータなど、必要に応じて複雑になることがあります。 UIDocument 実装の主要な役割は、モデルクラスと、ディスクに保存/読み込み可能な NSData 表現との間で変換を行うことです。

### <a name="finding-and-opening-icloud-documents"></a>ICloud ドキュメントの検索と開く

サンプルアプリでは1つのファイル test.txt のみが処理されるため、 **AppDelegate.cs**のコードによって `NSPredicate` が作成され、そのファイル名を具体的に検索する `NSMetadataQuery` ます。 `NSMetadataQuery` は非同期に実行され、完了時に通知を送信します。 通知オブザーバーによって呼び出される `DidFinishGathering` は、クエリを停止し、LoadDocument を呼び出します。この場合、`UIDocument.Open` メソッドと完了ハンドラーを使用して、ファイルを読み込み、`MonkeyDocumentViewController`に表示しようとします。

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

### <a name="displaying-icloud-documents"></a>ICloud ドキュメントの表示

UIDocument を他のモデルクラスと同じように表示することはできません。 UI コントロールにはプロパティが表示され、ユーザーによって編集された後、モデルに書き戻される可能性があります。

この例では、 **iCloudUIDoc\MonkeyDocumentViewController.cs** `UITextView`に MonkeyDocument テキストを表示します。 `ViewDidLoad` は、`MonkeyDocument.LoadFromContents` メソッドで送信された通知をリッスンします。 `LoadFromContents` は、iCloud がファイルの新しいデータを持っている場合に呼び出されます。そのため、ドキュメントが更新されたことが通知に示されます。

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

サンプルコードの通知ハンドラーは、UI を更新するメソッドを呼び出します。この場合、競合の検出や解決は行われません。

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>ICloud ドキュメントの保存

UIDocument を iCloud に追加するには、`UIDocument.Save` を直接呼び出すか (新しいドキュメントの場合のみ)、`NSFileManager.DefaultManager.SetUbiquitious`を使用して既存のファイルを移動します。 このコード例では、このコードを使用してユビキタスコンテナーに新しいドキュメントを直接作成します (2 つの完了ハンドラーがあります。1つは `Save` 操作用、もう1つは開いています)。

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

ドキュメントに対する後続の変更は、直接 "保存" されません。その代わり、`UpdateChangeCount`で変更されたことを `UIDocument` に伝え、ディスクへの保存操作を自動的にスケジュールします。

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>ICloud ドキュメントの管理

ユーザーは、設定を使用して、アプリケーションの外部にある "ユビキタスコンテナー" の**documents**ディレクトリにある iCloud ドキュメントを管理できます。ファイルの一覧を表示し、スワイプして削除することができます。 アプリケーションコードは、ユーザーがドキュメントを削除する状況を処理できる必要があります。 内部アプリケーションデータを**Documents**ディレクトリに格納しないでください。

 [![](introduction-to-icloud-images/icloudstorage.png "Managing iCloud Documents workflow")](introduction-to-icloud-images/icloudstorage.png#lightbox)

また、iCloud が有効になっているアプリケーションをデバイスから削除しようとすると、そのアプリケーションに関連する iCloud ドキュメントの状態を通知するために、異なる警告が表示されます。

 [![](introduction-to-icloud-images/icloud-delete1.png "Sample dialog when the user attempts to remove an iCloud-enabled application from their device")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Sample dialog when the user attempts to remove an iCloud-enabled application from their device")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud バックアップ

ICloud へのバックアップは、開発者によって直接アクセスされる機能ではありませんが、アプリケーションの設計方法によってユーザーエクスペリエンスが影響を受ける可能性があります。
Apple は、ios アプリケーションで開発者が実行できる[Ios データストレージのガイドライン](https://developer.apple.com/icloud/documentation/data-storage/)を提供します。

最も重要な考慮事項は、ユーザーが生成していない大きなファイルをアプリで格納するかどうかです (たとえば、問題ごとに 100 mb のコンテンツを格納する雑誌 reader アプリケーションなど)。 Apple では、この種のデータを保存しないことをお勧めします。このようなデータは iCloud にバックアップされ、ユーザーの iCloud クォータを不必要に埋めることになります。

このような大量のデータを格納するアプリケーションでは、バックアップされていないユーザーディレクトリのいずれかに保存する必要があります (例: キャッシュまたは tmp)、または `NSFileManager.SetSkipBackupAttribute` を使用してこれらのファイルにフラグを適用し、バックアップ操作中に iCloud によって無視されるようにします。

## <a name="summary"></a>まとめ

この記事では、iOS 5 に含まれる iCloud の新機能を紹介しました。 このサンプルでは、iCloud を使用するようにプロジェクトを構成するために必要な手順を確認し、iCloud 機能を実装する方法の例を示しました。

キー値ストレージの例では、データを格納するために iCloud を使用する方法を示しています。 NSUserPreferences の格納方法に似ています。 UIDocument の例では、iCloud を使用して複数のデバイス間でより複雑なデータを格納および同期する方法を示しました。

最後に、iCloud のバックアップの追加がアプリケーションの設計にどのように影響するかについて簡単に説明します。

## <a name="related-links"></a>関連リンク

- [ICloud の概要 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/introductiontoicloud)
- [iCloud セミナーのサンプルコード](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud セミナーのスライド](https://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud ストレージ](https://support.apple.com/kb/HT4847)
