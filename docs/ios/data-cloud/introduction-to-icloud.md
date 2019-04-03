---
title: Xamarin.iOS で iCloud の使用
description: このドキュメントでは、iCloud および Xamarin.iOS アプリケーションでの使用について説明します。 これは、key-value ストレージ、ドキュメント ストレージ、および iCloud のバックアップについて説明します。
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/09/2016
ms.openlocfilehash: 009e061726f655999c08192b5839a5c962d35e24
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855095"
---
# <a name="using-icloud-with-xamarinios"></a>Xamarin.iOS で iCloud の使用

IOS 5 で iCloud ストレージ API には、中央の場所にユーザーのドキュメントおよびアプリケーションに固有のデータを保存し、すべてのユーザーのデバイスからそれらの項目にアクセスするアプリケーションができます。

使用可能な 4 種類のストレージは。

- **Key-value ストレージ**- で少量のアプリケーションでデータを共有するユーザーの他のデバイス。

- **UIDocument ストレージ**- UIDocument のサブクラスを使用して、ユーザーの iCloud アカウントに、ドキュメントやその他のデータを格納します。

- **CoreData** -SQLite データベースのストレージ。

- **個々 のファイルとディレクトリ**- 多くの異なるファイルをファイル システムで直接管理するためです。

このドキュメントでは、最初の 2 種類のキーと値のペアと UIDocument サブクラス - Xamarin.iOS でこれらの機能を使用する方法について説明します。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="requirements"></a>必要条件

- Xamarin.iOS の最新の安定バージョン
- Xcode 10
- Visual Studio for Mac または Visual Studio 2019。

## <a name="preparing-for-icloud-development"></a>ICloud の開発の準備

アプリケーションで iCloud 両方を使用するように構成する必要があります、 [Apple Provisioning ポータル](https://developer.apple.com/account/ios/overview.action)と、プロジェクト自体。 前に、iCloud の開発 (または、サンプルをダウンロード) は、次の手順に従います。

ICloud へのアクセスにアプリケーションを正しく構成するには。

-   **検索、別の子です**-へのログイン[developer.apple.com](https://developer.apple.com)しを参照してください、 **Member Center > アカウント > 開発者アカウントの概要**チーム ID (または開発者の 1 つの個別の ID を取得するには). 10 文字の文字列になりますが ( **A93A5CM278**など)-これが「コンテナーの識別子」の一部を形成します。

-   **新しいアプリ ID を作成する**アプリ ID を作成に記載されている手順を[デバイス プロビジョニング ガイドのセクションをストア テクノロジのプロビジョニング](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)を必ず確認して**iCloud**として、サービスを使用できます。

 [![](introduction-to-icloud-images/icloud-sml.png "許可されているサービスとして iCloud を確認してください。")](introduction-to-icloud-images/icloud.png#lightbox)

- **新しいプロビジョニング プロファイルを作成**プロビジョニング プロファイルを作成に記載されている手順を[デバイス プロビジョニング ガイド](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device)します。

- **コンテナーの識別子を Entitlements.plist に追加**-コンテナーの識別子の形式は`TeamID.BundleID`します。 詳細についてを参照してください、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)ガイド。

- **プロジェクトのプロパティを構成する**- Info.plist のファイルを確認してください、**バンドル Id**と一致する、**バンドル ID**場合は、設定[アプリ ID を作成する](~/ios/deploy-test/provisioning/capabilities/index.md)です。IOS バンドル署名 使用して、**プロビジョニング プロファイル**iCloud App Service でアプリ ID が含まれていると**カスタム権利**ファイルを選択します。 これはすべて実行できます Visual Studio でプロジェクトのプロパティ ウィンドウの下。

- **デバイスで iCloud を有効にする**に移動 -**設定 > iCloud**にデバイスを記録することを確認してください。
選択し、有効にする、**ドキュメントおよびデータ**オプション。

- **ICloud をテストするデバイスを使用する必要があります**-シミュレーターでは機能しません。
実際には、2 つまたは複数のデバイスすべて iCloud の動作を確認するは同じ Apple ID でサインイン本当に必要があります。


## <a name="key-value-storage"></a>キー値ストレージ

キー値のストレージは、少量の書籍や雑誌で表示する最後のページなどの間で永続化されたデバイス - くれるかもしれませんが、ユーザー データを対象としています。 バックアップ データのキーと値のストレージを使用する必要があります。

キー値のストレージを使用する場合の注意すべきいくつかの制限があります。

- **キーの最大サイズ**-キー名は 64 バイトより長くすることはできません。

- **最大値のサイズ**-単一の値以上 64 キロバイトを格納することはできません。

- **アプリのキー値ストアの最大サイズ**-アプリケーション: 合計で最大 64 キロバイトのキー値のデータのみを格納できます。 その制限を超えるキーを設定する試みは失敗し、前の値が保持されます。

- **データ型**- 基本型のみなどの文字列、数値とブール値を格納することができます。

**ICloudKeyValue**の例では、そのしくみを示します。 サンプル コードは、各デバイスのという名前のキーを作成します。 1 つのデバイスでこのキーを設定し、他のユーザーに反映される値を見ることができます。 また、一度に多くのデバイスで編集する場合に任意のデバイスの編集可能な"Shared"と呼ばれるキーを作成、iCloud を決定します"wins"(変更のタイムスタンプを使用) の値を反映を取得します。

このスクリーン ショットは、使用中のサンプルを示します。 ICloud から変更通知を受信するときに、画面の下部にスクロール、テキスト ビューで印刷されていて入力フィールドで更新します。



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "デバイス間でメッセージのフロー")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>設定とデータの取得

このコードは、文字列値を設定する方法を示します。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

同期を呼び出すことが確実に値がローカル ディスク ストレージのみを保持します。 ICloud に同期では、バック グラウンドで行われ、「を適用できない」アプリケーション コードによって。 良好なネットワーク接続では更新プログラムをかなり長くかかる場合があります、ネットワークが不適切な (または切断) の場合は、同期を 5 (秒単位) で実行されます多くの場合。

このコードを持つ値を取得できます。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

値がローカル データ ストアから取得 - このメソッドは、「最新」の値を取得するサーバーを iCloud に接続を試行しません。 iCloud は、独自のスケジュールに従って、ローカル データ ストアに更新されます。

### <a name="deleting-data"></a>データの削除

キー/値ペアを完全に削除するには、次のように、Remove メソッドを使用します。

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>変更の確認

オブザーバーを追加することで、iCloud での値が変更されたとき、アプリケーションは通知を受け取ることもできます、`NSNotificationCenter.DefaultCenter`します。
次のコードから**KeyValueViewController.cs** `ViewWillAppear`メソッドは、これらの通知をリッスンし、キーが変更されたうちリストを作成する方法を示します。

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

コードは、これらのローカル コピーを更新または新しい値で、UI の更新など、変更されたキーの一覧を何らかのアクションを実行できます。

可能な変更の理由は次のとおりです。ServerChange (0)、InitialSyncChange (1) または QuotaViolationChange (2)。 理由をアクセスし、必要に応じて、さまざまな処理を実行できます (一部の結果としてのキーを削除するなど、 *QuotaViolationChange*)。

## <a name="document-storage"></a>ドキュメント ストレージ

iCloud ドキュメント ストレージは、アプリ (およびユーザー) に重要なデータを管理する設計されています。 ファイルと、アプリが同時に同じ iCloud ベースのバックアップを提供して、すべてのユーザーのデバイス間で共有機能を実行する必要があるその他のデータを管理するために使用します。

この図では、すべて適合化を示します。 各デバイスには、ローカル ストレージ (UbiquityContainer) とデーモンに任せ、クラウドでのデータの送受信、オペレーティング システムの iCloud に保存されているデータがあります。 FilePresenter/FileCoordinator 同時アクセスを防ぐために使用して、UbiquityContainer へのすべてのファイル アクセスを行う必要があります。 `UIDocument`クラスを実装するは、この例は、UIDocument を使用する方法を示します。

 [![](introduction-to-icloud-images/icloud-overview.png "ドキュメント ストレージの概要")](introduction-to-icloud-images/icloud-overview.png#lightbox)

ICloudUIDoc 例では、実装、単純な`UIDocument`1 つのテキスト フィールドが含まれるサブクラスです。 テキストが表示される、`UITextView`されで赤色で表示通知メッセージを他のデバイスを iCloud での編集が反映されます。 サンプル コードは、競合の解決などのより高度な iCloud 機能に関する処理を行いません。

このスクリーン ショットは、サンプル アプリケーションでキーを押すと、テキストの変更後**UpdateChangeCount**ドキュメントがその他のデバイスを iCloud を使用して同期します。

 [![](introduction-to-icloud-images/iclouduidoc.png "このスクリーン ショットは、テキストを変更すると、UpdateChangeCount キーを押してサンプル アプリケーション")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

ICloudUIDoc サンプルに 5 つの部分があります。

1. **UbiquityContainer へのアクセス**-決定 iCloud が有効になっている場合であれば、アプリケーションの iCloud 記憶域へのパス。

1. **UIDocument サブクラスを作成する**-iCloud ストレージと、モデル オブジェクト間を仲介するクラスを作成します。

1. **ICloud ドキュメントを開くときの検索と**-を使用して、`NSFileManager`と`NSPredicate`iCloud のドキュメントを検索し、それらを開きます。

1. **ICloud ドキュメントを表示する**-からプロパティを公開、 `UIDocument` UI コントロールと対話できるようにします。

1. **ICloud ドキュメントを保存**-UI に加えられた変更がディスクおよび iCloud に保存されることを確認します。

ICloud のすべての操作を実行 (または実行する必要があります) の処理を待機中にブロックしないように非同期的にします。 このサンプルでこれを実現する 3 つの方法が表示されます。

 **スレッド**-`AppDelegate.FinishedLaunching`最初の呼び出し`GetUrlForUbiquityContainer`はメイン スレッドをブロックしないように別のスレッドで行われます。

 **NotificationCenter** - ときに非同期の通知の登録などの操作`NSMetadataQuery.StartQuery`完了します。

 **完了ハンドラー**などの非同期操作の完了時に実行する方法を渡して -`UIDocument.Open`します。

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer へのアクセス

その場合、iCloud が有効になっているかどうかを判断する iCloud ドキュメント ストレージを使用して最初の手順は、「ユビキタス コンテナー」の場所 (デバイスで iCloud が有効なファイルの格納されたディレクトリ)。

このコードは、`AppDelegate.FinishedLaunching`サンプルのメソッド。

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

このサンプルではこれはできません、Apple では、アプリが前面に表示されるたびに、GetUrlForUbiquityContainer を呼び出すことお勧めします。

### <a name="creating-a-uidocument-subclass"></a>UIDocument サブクラスを作成します。

すべて iCloud ファイルとディレクトリ (つまり。 UbiquityContainer ディレクトリに格納されているもの) NSFileManager メソッド、NSFilePresenter プロトコルを実装すると、NSFileCoordinator を使用して書き込みを使用して管理する必要があります。
すべてを行う最も簡単な方法は、これを行う UIDocument サブクラスですが、自分でこれを作成することが自動的にすべて。

ICloud を使用する、UIDocument サブクラスで実装する必要がありますのみの 2 つの方法はあります。

- **LoadFromContents** -モデルのクラス/es に展開するためのファイルの内容の場合に渡します。

- **ContentsForType** -ディスク (とクラウド) に保存するモデルのクラス/es の場合の表現を提供するための要求。

このサンプル コードを**iCloudUIDoc\MonkeyDocument.cs** UIDocument を実装する方法を示しています。

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

データ モデルがここでは非常に単純に 1 つのテキスト フィールド。 データ モデルは複雑な Xml ドキュメントまたはバイナリ データなど、必要なものにすることはできます。 UIDocument 実装の主な役割、モデル クラスとディスクに保存/読み込みできるばかり表現の間で変換することです。

### <a name="finding-and-opening-icloud-documents"></a>検索と iCloud のドキュメントを開く

サンプル アプリのみが扱う 1 つのファイル - test.txt のため、内のコード **appdelegate.cs** を作成、`NSPredicate`と`NSMetadataQuery`を具体的にはそのファイル名を探します。 `NSMetadataQuery`非同期で実行され、完了すると、通知を送信します。 `DidFinishGathering` 通知のオブザーバーによって呼び出される、クエリを停止および呼び出しを使用して、LoadDocument、`UIDocument.Open`ファイルを読み込み、表示しようとする完了ハンドラーを持つメソッドを`MonkeyDocumentViewController`します。

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

### <a name="displaying-icloud-documents"></a>ICloud ドキュメントを表示します。

その他のモデル クラスを他と同じにすることはできません、UIDocument を表示します。
- プロパティは、可能性があるユーザーを編集し、モデルに書き戻さの UI コントロールに表示されます。

例では**iCloudUIDoc\MonkeyDocumentViewController.cs** MonkeyDocument のテキストが表示されます、`UITextView`します。 `ViewDidLoad` 送信された通知をリッスン、`MonkeyDocument.LoadFromContents`メソッド。 `LoadFromContents` 通知は、ドキュメントが更新されたことを示すように、iCloud がある新しいデータ ファイルはときに、呼び出されます。

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

サンプル コードの通知のハンドラーは、競合の検出または解決なし - UI をここで更新するメソッドを呼び出します。

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

Icloud を呼び出すことができます、UIDocument を追加する`UIDocument.Save`直接 (新しいドキュメント用のみ) 使用して既存ファイルを移動または`NSFileManager.DefaultManager.SetUbiquitious`します。 コード例は、次のコードでユビキタスのコンテナーで直接新しい文書を作成 (2 つの完了ハンドラーがここでは、1 つずつ、`Save`操作と開く用)。

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

ドキュメントへの後続の変更はありません「保存された」直接、代わりに言って、`UIDocument`が変更されるとする`UpdateChangeCount`、およびディスク操作への保存を自動的にスケジュール設定は。

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>ICloud ドキュメントを管理します。

ユーザーが iCloud ドキュメントを管理できる、**ドキュメント**; の設定を使用して、アプリケーションの外部で「ユビキタス コンテナー」のディレクトリを削除するのにはスワイプ、ファイルの一覧を表示できます。 アプリケーション コードでは、ユーザーがドキュメントを削除する場所の状況を処理できる必要があります。 内の内部アプリケーション データを格納しないでください、**ドキュメント**ディレクトリ。

 [![](introduction-to-icloud-images/icloudstorage.png "ICloud ドキュメント ワークフローを管理します。")](introduction-to-icloud-images/icloudstorage.png#lightbox)



ICloud を有効にしたアプリケーションを通知することと、そのアプリケーションに関連する iCloud ドキュメントの状態のデバイスから削除を試みるときに、ユーザーは各種の警告も表示されます。

 [![](introduction-to-icloud-images/icloud-delete1.png "ユーザーが iCloud を有効にしたアプリケーションを自分のデバイスから削除しようとしたときサンプル ダイアログ")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "ユーザーが iCloud を有効にしたアプリケーションを自分のデバイスから削除しようとしたときサンプル ダイアログ")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud のバックアップ

ICloud へのバックアップには、開発者が直接アクセスする機能はありませんが、アプリケーションを設計する方法は、ユーザー エクスペリエンスに影響します。
Apple から提供されて[iOS データ ストレージのガイドライン](https://developer.apple.com/icloud/documentation/data-storage/)に従って、iOS アプリケーションの開発者向け。

最も重要な考慮事項が、アプリがユーザーが生成した (たとえば、問題ごとのコンテンツの hundred-plus メガバイト数を格納するマガジン リーダー アプリケーション) ではない大きなファイルを格納するかどうか。 Apple では、この種ののバックアップを iCloud にして、ユーザーの iCloud クォータを不必要に入力データを格納しないようにすることが優先されます。

大量のこのようなデータを格納するアプリケーションで必要がありますはバックアップ (例: ユーザーのディレクトリのいずれかで格納か キャッシュまたは tmp) を使用して、または`NSFileManager.SetSkipBackupAttribute`iCloud のバックアップ操作中にそれらが無視されるように、それらのファイルにフラグを適用します。

## <a name="summary"></a>まとめ

この記事には、iOS 5 に含まれる新しい iCloud 機能が導入されました。 ICloud を使用するプロジェクトを構成するために必要な iCloud 機能を実装する方法の例を提供し、手順が調べられます。

記憶域のキーと値の例では、iCloud を使用して、少量の NSUserPreferences が格納されているのと同様のデータを格納する方法が示されています。 UIDocument 例より多くの複雑なデータを格納し、iCloud を使用して複数のデバイス間で同期を示しました。

最後に iCloud のバックアップを追加する必要があります、アプリケーションの設計に影響するしくみについて簡単に説明が含まれています。



## <a name="related-links"></a>関連リンク

- [概要を iCloud (サンプル)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud セミナー サンプル コード](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud セミナー スライド](https://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud ストレージ](https://support.apple.com/kb/HT4847)
