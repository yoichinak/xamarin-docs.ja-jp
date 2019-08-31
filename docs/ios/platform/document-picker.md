---
title: Xamarin のドキュメントピッカー。 iOS
description: このドキュメントでは、iOS ドキュメントピッカーについて説明し、Xamarin. iOS での使用方法について説明します。 ICloud、ドキュメント、一般的なセットアップコード、ドキュメントプロバイダーの拡張機能などについて説明します。
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: 6982f02860db2e89f83b4002d6acb5b28bae906b
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200370"
---
# <a name="document-picker-in-xamarinios"></a>Xamarin のドキュメントピッカー。 iOS

ドキュメントピッカーでは、アプリ間でドキュメントを共有できます。 これらのドキュメントは iCloud または別のアプリのディレクトリに格納されている場合があります。 ドキュメントは、ユーザーがデバイスにインストールした[ドキュメントプロバイダーの拡張機能](~/ios/platform/extensions.md)のセットを介して共有されます。 

アプリとクラウド間でドキュメントを同期させることが困難であるため、必要な複雑さが一定の量になります。

## <a name="requirements"></a>必要条件

この記事に記載されている手順を完了するには、次のものが必要です。

- **Xcode 7 と ios 8**以降– Apple の Xcode 7 と ios 8 以降の api は、開発者のコンピューターにインストールして構成する必要があります。
- **Visual Studio または Visual Studio for Mac** –最新バージョンの Visual Studio for Mac をインストールする必要があります。
- ios**デバイス**-ios 8 以降を実行している ios デバイス。

## <a name="changes-to-icloud"></a>ICloud への変更

ドキュメントピッカーの新機能を実装するために、Apple の iCloud サービスに次の変更が加えられました。

- ICloud デーモンは、CloudKit を使用して完全に書き換えられました。
- 既存の iCloud 機能の名前が iCloud ドライブに変更されました。
- ICloud に Microsoft Windows OS のサポートが追加されました。
- Mac OS Finder に iCloud フォルダーが追加されました。
- iOS デバイスは、Mac OS iCloud フォルダーのコンテンツにアクセスできます。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="what-is-a-document"></a>ドキュメントとは

ICloud でドキュメントを参照する場合は、スタンドアロンの単一のエンティティであり、ユーザーによって認識される必要があります。 ユーザーは、ドキュメントを変更したり、他のユーザーと共有したりすることができます (電子メールを使用するなど)。

ページ、基調講演、数字ファイルなど、ユーザーがドキュメントとしてすぐに認識できるファイルには、いくつかの種類があります。 ただし、iCloud は、この概念に限定されていません。 たとえば、ゲームの状態 (チェスの一致など) をドキュメントとして処理し、iCloud に保存することができます。 このファイルは、ユーザーのデバイス間で渡される可能性があり、別のデバイスで退職したゲームを選択できます。

## <a name="dealing-with-documents"></a>ドキュメントの処理

Xamarin でドキュメントピッカーを使用するために必要なコードについて説明する前に、この記事では、iCloud ドキュメントを操作するためのベストプラクティスと、ドキュメントピッカーをサポートするために必要な既存の Api に加えられたいくつかの変更について説明します。

### <a name="using-file-coordination"></a>ファイル調整の使用

ファイルは複数の異なる場所から変更できるため、データの損失を防ぐために調整を使用する必要があります。

 [![](document-picker-images/image1.png "ファイル調整の使用")](document-picker-images/image1.png#lightbox)

上の図を見てみましょう。

1. ファイル調整を使用する iOS デバイスでは、新しいドキュメントを作成し、iCloud フォルダーに保存します。
2. iCloud は、変更されたファイルをクラウドに保存して、すべてのデバイスに配布します。
3. 接続された Mac は iCloud フォルダー内の変更されたファイルを確認し、ファイル調整を使用して変更をファイルにコピーします。
4. ファイル調整を使用していないデバイスは、ファイルに変更を加えて iCloud フォルダーに保存します。 これらの変更は、すぐに他のデバイスにレプリケートされます。

元の iOS デバイスまたは Mac がファイルを編集していたとします。変更は失われ、一貫性デバイスのファイルのバージョンで上書きされます。 データの損失を防ぐため、ファイル調整はクラウドベースのドキュメントを操作するときに必要になります。

### <a name="using-uidocument"></a>UIDocument の使用

 `UIDocument`開発者にとって`NSDocument`非常に複雑な作業をすべて実行して、シンプルな (または macOS 上で) 物を作成します。 バックグラウンドキューを使用した組み込みのファイル調整機能によって、アプリケーションの UI がブロックされるのを防ぐことができます。

 `UIDocument`は、開発者が必要とする目的で Xamarin アプリケーションの開発作業を容易にする、複数の高レベルの Api を公開します。

次のコードは、のサブ`UIDocument`クラスを作成して、iCloud からテキストを格納および取得するために使用できるテキストベースの一般的なドキュメントを実装します。

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

上記`GenericTextDocument`のクラスは、Xamarin. iOS 8 アプリケーションでドキュメントピッカーと外部ドキュメントを操作するときに、この記事全体で使用されます。

## <a name="asynchronous-file-coordination"></a>非同期ファイル調整

iOS 8 では、新しいファイル調整 Api を使用して、新しい非同期ファイル調整機能をいくつか提供しています。 IOS 8 より前では、すべての既存のファイル調整 Api は完全に同期されていました。 このため、開発者は、ファイルの調整によってアプリケーションの UI がブロックされないように、独自のバックグラウンドキューを実装する責任がありました。

新しい`NSFileAccessIntent`クラスには、ファイルを指す URL と、必要な調整の種類を制御するためのいくつかのオプションが含まれています。 次のコードは、インテントを使用して、ある場所から別の場所にファイルを移動する方法を示しています。

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>ドキュメントを検出して一覧表示する

ドキュメントを検出して一覧表示する方法は、既存`NSMetadataQuery`の api を使用することです。 このセクションでは、以前よりも`NSMetadataQuery`簡単にドキュメントを操作できるようにするために追加された新機能について説明します。

### <a name="existing-behavior"></a>既存の動作

IOS 8 より前の`NSMetadataQuery`では、ローカルファイルの変更 (削除、作成、名前の変更など) のピックアップに時間がかかりました。

 [![](document-picker-images/image2.png "NSMetadataQuery ローカルファイルの変更の概要")](document-picker-images/image2.png#lightbox)

上の図では、次のようになります。

1. アプリケーションコンテナーに既に存在するファイルについ`NSMetadataQuery`ては`NSMetadata` 、既存のレコードが事前に作成され、スプールされるので、アプリケーションですぐに使用できるようになります。
1. アプリケーションによって、アプリケーションコンテナーに新しいファイルが作成されます。
1. アプリケーションコンテナーの変更を`NSMetadataQuery`確認し、必要な`NSMetadata`レコードを作成するまでに、遅延が発生します。


`NSMetadata`レコードの作成には遅延があるため、アプリケーションでは、ローカルファイルの変更用とクラウドベースの変更用の2つのデータソースを開く必要がありました。

### <a name="stitching"></a>ステッチ

IOS 8 では`NSMetadataQuery` 、を、合成と呼ばれる新機能で直接使用する方が簡単です。

 [![](document-picker-images/image3.png "合成と呼ばれる新しい機能を持つ NSMetadataQuery")](document-picker-images/image3.png#lightbox)

上の図では、合成を使用します。

1. 以前と同様に、アプリケーションコンテナーに既に存在するファイル`NSMetadataQuery`につい`NSMetadata`ては、既存のレコードが事前に作成およびスプールされています。
1. アプリケーションは、ファイル調整を使用して、アプリケーションコンテナーに新しいファイルを作成します。
1. アプリケーションコンテナーのフックは、変更を確認し、 `NSMetadataQuery`を呼び出して必要`NSMetadata`なレコードを作成します。
1. `NSMetadata`レコードは、ファイルの直後に作成され、アプリケーションで使用できるようになります。


アプリケーションでは、結合を使用することによって、ローカルおよびクラウドベースのファイル変更を監視するためにデータソースを開く必要がなくなりました。 これで、アプリケーションは直接`NSMetadataQuery`に依存できます。

> [!IMPORTANT]
> 合成は、前のセクションで説明したように、アプリケーションがファイル調整を使用している場合にのみ機能します。 ファイルの調整が使用されていない場合、Api は既定で iOS 8 の既存の動作に設定されます。




### <a name="new-ios-8-metadata-features"></a>新しい iOS 8 メタデータ機能

IOS 8 では、次の新`NSMetadataQuery`機能がに追加されています。

- `NSMetatadataQuery`クラウドに格納されているローカルではないドキュメントを一覧表示できるようになりました。
- クラウドベースのドキュメントのメタデータ情報にアクセスするための新しい Api が追加されました。 
- ファイルのファイル属性`NSUrl_PromisedItems`にアクセスするための新しい API が用意されています。これらのファイルには、ローカルで使用可能なコンテンツが含まれていなくてもかまいません。
- メソッドを使用して、指定されたファイルに関する`GetPromisedItemResourceValues`情報を取得します。または、メソッドを使用して、一度に複数のファイルに関する情報を取得します。 `GetPromisedItemResourceValue`


メタデータを処理するために、2つの新しいファイル調整フラグが追加されました。

- `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
- `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


上記のフラグでは、ドキュメントファイルを使用するために、ドキュメントファイルの内容をローカルで使用する必要はありません。

次のコードセグメントは、を使用`NSMetadataQuery`して特定のファイルが存在するかどうかを照会し、存在しない場合はファイルをビルドする方法を示しています。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),             NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>ドキュメントのサムネイル

Apple は、アプリケーションのドキュメントを一覧表示するときに、プレビューを使用するのが最適なユーザーエクスペリエンスであると感じています。 これにより、エンドユーザーコンテキストが提供されるため、使用するドキュメントをすばやく識別できます。

IOS 8 より前では、ドキュメントのプレビューを表示するには、カスタム実装が必要でした。 IOS 8 の新機能は、開発者がドキュメントのサムネイルをすばやく操作できるようにするファイルシステム属性です。

#### <a name="retrieving-document-thumbnails"></a>ドキュメントのサムネイルの取得 

メソッド`GetPromisedItemResourceValue`または`GetPromisedItemResourceValues`メソッド`NSUrlThumbnailDictionary`を呼び出すことによって、 API(a)が返されます。`NSUrl_PromisedItems` このディクショナリに現在ある唯一のキーは`NSThumbnial1024X1024SizeKey` 、とその`UIImage`一致です。

#### <a name="saving-document-thumbnails"></a>保存 (ドキュメントのサムネイルを)

サムネイルを保存する最も簡単な方法は`UIDocument`、を使用することです。 `GetFileAttributesToWrite`のメソッドを呼び出し、サムネイルを設定することにより、ドキュメントファイルがの場合に、自動的に保存されます。 `UIDocument` ICloud デーモンはこの変更を認識し、iCloud に伝達します。 Mac OS X では、クイック検索プラグインによって、開発者のサムネイルが自動的に生成されます。

既存の API の変更と共に、iCloud ベースのドキュメントを使用するための基本については、Xamarin iOS 8 モバイルアプリケーションでドキュメントピッカービューコントローラーを実装する準備が整いました。


## <a name="enabling-icloud-in-xamarin"></a>Xamarin での iCloud の有効化

Xamarin iOS アプリケーションでドキュメントピッカーを使用するには、アプリケーションと Apple の両方で iCloud のサポートを有効にする必要があります。 

次の手順では、iCloud のプロビジョニングプロセスについて説明します。

1. ICloud コンテナーを作成します。
2. ICloud App Service を含むアプリ ID を作成します。
3. このアプリ ID を含むプロビジョニングプロファイルを作成します。

「[機能の使用](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)」ガイドでは、最初の2つの手順について説明します。 プロビジョニングプロファイルを作成するには、[プロビジョニングプロファイル](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device)ガイドの手順に従います。



次の手順では、iCloud 用にアプリケーションを構成するプロセスについて説明します。

次の手順で行います。

1. Visual Studio for Mac または Visual Studio でプロジェクトを開きます。
2. **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[オプション] を選択します。
3. オプション ダイアログボックスで  **IOS アプリケーション** を選択し、**バンドル識別子**が、アプリケーション用に作成した**アプリ ID**で定義されているものと一致していることを確認します。 
4. 選択 **iOS バンドル署名** を選択、**開発者 Identity** と **プロビジョニング プロファイル** 上記で作成しました。
5. **[OK]** ボタンをクリックして変更を保存し、ダイアログボックスを閉じます。
6. `Entitlements.plist` **ソリューションエクスプローラー**内でを右クリックして、エディターで開きます。

    > [!IMPORTANT]
    > Visual Studio では、権利エディターを右クリックし、[**ファイルを開くアプリケーション**の選択] をクリックして開く必要があります。 および [プロパティリストエディター] を選択します。

7. [ICloud、 **Icloud ドキュメント**、**キー値ストレージ**および**Cloudkit** **を有効にする**] チェックボックスをオンにします。
8. (上記で作成したように) アプリケーションの**コンテナー**が存在することを確認します。 例: `iCloud.com.your-company.AppName`
9. 変更内容をファイルに保存します。

権利の詳細については、「[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)」ガイドを参照してください。

上記のセットアップを実行すると、アプリケーションでクラウドベースのドキュメントと新しいドキュメントピッカービューコントローラーを使用できるようになります。

## <a name="common-setup-code"></a>一般的なセットアップコード

ドキュメントピッカービューコントローラーの使用を開始する前に、標準セットアップコードが必要です。 まず、アプリケーションの`AppDelegate.cs`ファイルを変更し、次のようにします。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                 new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");    

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {    
                    // Yes, inform caller and save location the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> 上記のコードには、上の「ドキュメントの検出と一覧表示」セクションのコードが含まれています。 実際のアプリケーションで表示されるように、ここには完全に表示されます。 わかりやすくするために、この例では、ハードコーディングされ`test.txt`た単一のファイル () のみを使用します。

上のコードでは、アプリケーションの他の部分で簡単に操作できるように、iCloud ドライブのショートカットをいくつか公開しています。

次に、ドキュメントピッカーを使用するか、クラウドベースのドキュメントを操作するビューまたはビューコンテナーに、次のコードを追加します。

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

これにより、 `AppDelegate`にアクセスするショートカットが追加され、上で作成した iCloud ショートカットにアクセスできるようになります。

このコードを使用して、Xamarin iOS 8 アプリケーションでのドキュメントピッカービューコントローラーの実装について見ていきましょう。

## <a name="using-the-document-picker-view-controller"></a>ドキュメントピッカービューコントローラーの使用

IOS 8 より前では、アプリ内からアプリケーションの外部でドキュメントを探索する方法がなかったため、別のアプリケーションからドキュメントにアクセスするのは非常に困難でした。

### <a name="existing-behavior"></a>既存の動作

 [![](document-picker-images/image31.png "既存の動作の概要")](document-picker-images/image31.png#lightbox)

IOS 8 より前の外部ドキュメントへのアクセスを見てみましょう。

1. まず、ユーザーは、最初にドキュメントを作成したアプリケーションを開く必要があります。
1. ドキュメントが選択`UIDocumentInteractionController`され、新しいアプリケーションにドキュメントを送信するためにが使用されます。
1. 最後に、元のドキュメントのコピーが新しいアプリケーションのコンテナーに配置されます。


このドキュメントから、2つ目のアプリケーションが開いて編集できるようになります。

### <a name="discovering-documents-outside-of-an-apps-container"></a>アプリのコンテナー外のドキュメントの検出

IOS 8 では、アプリケーションは、アプリケーションコンテナーの外部にあるドキュメントに簡単にアクセスできます。

 [![](document-picker-images/image32.png "アプリのコンテナー外のドキュメントの検出")](document-picker-images/image32.png#lightbox)

新しい iCloud ドキュメントピッカー ( `UIDocumentPickerViewController`) を使用すると、iOS アプリケーションは、アプリケーションコンテナーの外部で直接検出してアクセスできます。 は`UIDocumentPickerViewController` 、ユーザーがアクセス許可を使用して検出されたドキュメントへのアクセスを許可し、そのドキュメントを編集するためのメカニズムを提供します。

アプリケーションでは、ドキュメントを iCloud ドキュメントピッカーに表示し、他のアプリケーションがそれらを検出して使用できるようにすることをオプトインする必要があります。 Xamarin iOS 8 アプリケーションでアプリケーションコンテナーを共有するには、標準`Info.plist`のテキストエディターでファイルを編集し、次の2行を辞書の下部 ( `<dict>...</dict>`タグの間) に追加します。

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

に`UIDocumentPickerViewController`は、ユーザーがドキュメントを選択できる優れた新しい UI が用意されています。 Xamarin iOS 8 アプリケーションでドキュメントピッカービューコントローラーを表示するには、次の手順を実行します。

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> 開発者は、 `StartAccessingSecurityScopedResource` `NSUrl`外部ドキュメントにアクセスする前にのメソッドを呼び出す必要があります。 ドキュメントが読み込まれるとすぐにセキュリティロックを解放するには、メソッドを呼び出す必要があります。`StopAccessingSecurityScopedResource`

### <a name="sample-output"></a>出力例

IPhone デバイスで実行したときに、上記のコードでドキュメントピッカーを表示する方法の例を次に示します。

1. ユーザーがアプリケーションを起動し、メインインターフェイスが表示されます。   
 
    [![](document-picker-images/image33.png "メインインターフェイスが表示されます。")](document-picker-images/image33.png#lightbox)
1. ユーザーは画面の上部にある **[アクション]** ボタンをタップすると、利用可能なプロバイダーの一覧から**ドキュメントプロバイダー**を選択するよう求められます。   
 
    [![](document-picker-images/image34.png "利用可能なプロバイダーの一覧からドキュメントプロバイダーを選択します")](document-picker-images/image34.png#lightbox)
1. 選択した**ドキュメントプロバイダー**の**ドキュメントピッカービューコントローラー**が表示されます。   
 
    [![](document-picker-images/image35.png "ドキュメントピッカービューコントローラーが表示されます")](document-picker-images/image35.png#lightbox)
1. ユーザーは、**ドキュメントフォルダー**をタップしてその内容を表示します。   
 
    [![](document-picker-images/image36.png "ドキュメントフォルダーの内容")](document-picker-images/image36.png#lightbox)
1. ユーザーが**ドキュメント**を選択すると、**ドキュメントピッカー**が閉じられます。
1. メインインターフェイスが再表示され、**ドキュメント**が外部コンテナーとそのコンテンツから読み込まれます。


ドキュメントピッカービューコントローラーの実際の表示は、ユーザーがデバイスにインストールしたドキュメントプロバイダーと、どのドキュメントピッカーモードが実装されているかによって異なります。 上の例では、オープンモードを使用しています。他のモードの種類については、以下で詳しく説明します。

## <a name="managing-external-documents"></a>外部ドキュメントの管理

前述のように、iOS 8 より前のアプリケーションでは、アプリケーションコンテナーの一部であったドキュメントにしかアクセスできませんでした。 IOS 8 では、アプリケーションは外部ソースからドキュメントにアクセスできます。

 [![](document-picker-images/image37.png "外部ドキュメントの管理の概要")](document-picker-images/image37.png#lightbox)

ユーザーが外部ソースからドキュメントを選択すると、元のドキュメントを指すアプリケーションコンテナーに参照ドキュメントが書き込まれます。

この新しい機能を既存のアプリケーションに追加するために、いくつかの新機能が`NSMetadataQuery` API に追加されました。 通常、アプリケーションでは、アプリケーションコンテナー内に存在するドキュメントを一覧表示するために、ユビキタスドキュメントスコープを使用します。 このスコープを使用すると、アプリケーションコンテナー内のドキュメントだけが引き続き表示されます。

新しいユビキタス外部ドキュメントスコープを使用すると、アプリケーションコンテナーの外部に存在するドキュメントが返され、そのメタデータが返されます。 `NSMetadataItemUrlKey`は、ドキュメントが実際に配置されている URL を参照します。

アプリケーションで、参照によって参照されているドキュメントを操作したくない場合もあります。 代わりに、アプリが参照ドキュメントを直接操作する必要があります。 たとえば、アプリでは、UI のアプリケーションのフォルダーにドキュメントを表示したり、ユーザーがフォルダー内で参照を移動できるようにしたりすることができます。

IOS 8 では、参照`NSMetadataItemUrlInLocalContainerKey`ドキュメントに直接アクセスするための新しいが提供されています。 このキーは、アプリケーションコンテナー内の外部ドキュメントへの実際の参照を指します。

は`NSMetadataUbiquitousItemIsExternalDocumentKey` 、ドキュメントがアプリケーションのコンテナーの外部にあるかどうかをテストするために使用されます。 は`NSMetadataUbiquitousItemContainerDisplayNameKey` 、外部ドキュメントの元のコピーが格納されているコンテナーの名前にアクセスするために使用されます。

### <a name="why-document-references-are-required"></a>ドキュメント参照が必要な理由

IOS 8 が外部ドキュメントへのアクセスに参照を使用する主な理由は、セキュリティです。 他のアプリケーションのコンテナーへのアクセス権がアプリケーションに与えられていません。 がアウトプロセスで実行されていて、システム全体でアクセスできるため、ドキュメントピッカーだけがこれを行うことができます。

アプリケーションコンテナーの外部にあるドキュメントを取得する唯一の方法は、ドキュメントピッカーを使用することと、ピッカーによって返される URL がセキュリティスコープである場合です。 セキュリティスコープの URL には、ドキュメントを選択するために必要な情報だけが含まれており、アプリケーションにドキュメントへのアクセスを許可するために必要なスコープ付き権限も含まれています。

セキュリティスコープの URL が文字列にシリアル化されてからシリアル化解除された場合、セキュリティ情報は失われ、ファイルは URL からアクセスできないことに注意してください。 ドキュメント参照機能は、これらの Url が指すファイルを取得するためのメカニズムを提供します。

そのため、アプリケーションがいずれ`NSUrl`かの参照ドキュメントからを取得した場合は、既にセキュリティスコープがアタッチされており、このファイルにアクセスするために使用できます。 このため、開発者はこれらの情報とプロセスを`UIDocument`すべて処理するため、この方法を使用することをお勧めします。

### <a name="using-bookmarks"></a>ブックマークの使用

状態の復元など、特定のドキュメントに戻るために、アプリケーションのドキュメントを列挙することは、常に可能であるとは限りません。 iOS 8 には、特定のドキュメントを直接対象とするブックマークを作成するためのメカニズムが用意されています。

次のコードでは、 `UIDocument`の`FileUrl`プロパティからブックマークを作成します。

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

既存の bookmark API を使用して、既存`NSUrl`のに対するブックマークを作成します。これは、外部ファイルへの直接アクセスを提供するために保存して読み込むことができます。 次のコードは、上記で作成したブックマークを復元します。

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Vs を開くインポートモードとドキュメントピッカー

ドキュメントピッカービューコントローラーには、2つの異なる操作モードがあります。

1. **オープンモード**–このモードでは、ユーザーが外部ドキュメントを選択すると、ドキュメントピッカーによって、アプリケーションコンテナーにセキュリティスコープのブックマークが作成されます。   
 
    [![](document-picker-images/image37.png "アプリケーションコンテナー内のセキュリティスコープのブックマーク")](document-picker-images/image37.png#lightbox)
1. **インポートモード**–このモードでは、ユーザーが外部ドキュメントを選択すると、ドキュメントピッカーはブックマークを作成しません。代わりに、ファイルを一時的な場所にコピーし、次の場所にあるドキュメントへのアクセスをアプリケーションに提供します。   
 
    [![](document-picker-images/image38.png "ドキュメントピッカーは、ファイルを一時的な場所にコピーし、この場所にあるドキュメントへのアクセスをアプリケーションに提供します。")](document-picker-images/image38.png#lightbox)   
 アプリケーションが何らかの理由で終了すると、一時的な場所が空になり、ファイルが削除されます。 アプリケーションがファイルへのアクセスを維持する必要がある場合は、コピーを作成して、アプリケーションコンテナーに配置する必要があります。


Open モードは、アプリケーションが別のアプリケーションと共同作業を行い、そのアプリケーションでドキュメントに加えられた変更を共有する場合に便利です。 インポートモードは、アプリケーションがその変更を他のアプリケーションとドキュメントに共有したくない場合に使用されます。

## <a name="making-a-document-external"></a>ドキュメントを外部にする

前述のように、iOS 8 アプリケーションは、独自のアプリケーションコンテナーの外部にあるコンテナーにアクセスできません。 アプリケーションは、独自のコンテナーにローカルまたは一時的な場所に書き込むことができます。次に、特殊なドキュメントモードを使用して、生成されたドキュメントをアプリケーションコンテナーの外に、ユーザーが選択した場所に移動します。

ドキュメントを外部の場所に移動するには、次の手順を実行します。

1. まず、ローカルまたは一時的な場所に新しいドキュメントを作成します。
1. 新しいドキュメント`NSUrl`を指すを作成します。
1. 新しいドキュメントピッカービューコントローラーを開き、 `NSUrl`という`MoveToService`モードでに渡します。 
1. ユーザーが新しい場所を選択すると、ドキュメントは現在の場所から新しい場所に移動されます。
1. 作成中のアプリケーションがファイルにアクセスできるように、参照ドキュメントがアプリのアプリケーションコンテナーに書き込まれます。


ドキュメントを外部の場所に移動するには、次のコードを使用します。`var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

上記のプロセスによって返された参照ドキュメントは、ドキュメントピッカーの Open モードによって作成されたものとまったく同じです。 ただし、アプリケーションがドキュメントを参照しなくても移動することが必要になる場合があります。

参照を生成せずにドキュメントを移動するに`ExportToService`は、モードを使用します。 例: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

`ExportToService`モードを使用すると、ドキュメントは外部コンテナーにコピーされ、既存のコピーは元の場所に残ります。

## <a name="document-provider-extensions"></a>ドキュメントプロバイダーの拡張機能

IOS 8 では、Apple は、エンドユーザーが実際に存在する場所に関係なく、クラウドベースのドキュメントにアクセスできるようにする必要があります。 この目標を達成するために、iOS 8 には新しいドキュメントプロバイダーの拡張メカニズムが用意されています。

### <a name="what-is-a-document-provider-extension"></a>ドキュメントプロバイダーの拡張機能とは

単純に言うと、ドキュメントプロバイダーの拡張機能は、開発者またはサードパーティが、既存の iCloud ストレージの場所とまったく同じ方法でアクセスできるアプリケーション代替ドキュメントストレージに提供する方法です。

ユーザーは、ドキュメントピッカーから、これらの代替の保存場所のいずれかを選択できます。また、同じアクセスモード (開く、インポート、移動、またはエクスポート) を使用して、その場所にあるファイルを操作できます。

これは、次の2つの異なる拡張機能を使用して実装されます。

- **ドキュメントピッカー拡張機能**–ユーザー `UIViewController`が別の保存場所からドキュメントを選択するためのグラフィカルインターフェイスを提供するサブクラスを提供します。 このサブクラスは、ドキュメントピッカービューコントローラーの一部として表示されます。
- **ファイルの拡張子を指定**します。これは、実際にファイルの内容を提供する、UI 以外の拡張機能です。 これらの拡張機能は、ファイル調整`NSFileCoordinator` () によって提供されます。 これは、ファイル調整が必要なもう1つの重要なケースです。


次の図は、ドキュメントプロバイダーの拡張機能を使用する場合の一般的なデータフローを示しています。

 [![](document-picker-images/image39.png "次の図は、ドキュメントプロバイダーの拡張機能を使用する場合の一般的なデータフローを示しています。")](document-picker-images/image39.png#lightbox)

次の処理が行われます。

1. アプリケーションは、操作するファイルをユーザーが選択できるようにするためのドキュメントピッカーコントローラーを提供します。
1. ユーザーは別のファイルの場所を選択する`UIViewController`と、カスタム拡張機能が呼び出され、ユーザーインターフェイスが表示されます。
1. ユーザーがこの場所からファイルを選択すると、URL がドキュメントピッカーに戻されます。
1. ドキュメントピッカーは、ファイルの URL を選択し、ユーザーが作業するアプリケーションにその URL を返します。
1. URL は、ファイルの内容をアプリケーションに返すためにファイルコーディネーターに渡されます。
1. ファイルコーディネーターは、カスタムファイルプロバイダー拡張機能を呼び出してファイルを取得します。
1. ファイルの内容がファイルコーディネーターに返されます。
1. ファイルの内容がアプリケーションに返されます。


### <a name="security-and-bookmarks"></a>セキュリティとブックマーク

このセクションでは、ドキュメントプロバイダーの拡張機能で、ブックマークを使用したセキュリティと永続的なファイルアクセスのしくみについて簡単に説明します。 アプリケーションコンテナーにセキュリティとブックマークを自動的に保存する iCloud ドキュメントプロバイダーとは異なり、ドキュメントプロバイダーの拡張機能は、ドキュメント参照システムの一部ではないため、そのような機能はありません。

たとえば、企業全体に安全なデータストアを提供するエンタープライズ設定では、管理者は、パブリック iCloud サーバーによってアクセスまたは処理される機密企業情報を必要としません。 したがって、組み込みのドキュメント参照システムは使用できません。

ブックマークシステムは引き続き使用でき、ブックマークされた URL を正しく処理し、それが指すドキュメントの内容を返すために、ファイルプロバイダーの拡張機能の役割を担います。

セキュリティ上の理由から、iOS 8 には、どのアプリケーションがどのファイルプロバイダー内のどの識別子にアクセスできるかに関する情報を保持する分離レイヤーが用意されています。 すべてのファイルアクセスは、この分離レイヤーによって制御されることに注意してください。

次の図は、ブックマークとドキュメントプロバイダーの拡張機能を使用する場合のデータフローを示しています。

 [![](document-picker-images/image40.png "この図は、ブックマークとドキュメントプロバイダーの拡張機能を操作するときのデータフローを示しています。")](document-picker-images/image40.png#lightbox)

次の処理が行われます。

1. アプリケーションがバックグラウンドに入り、状態を永続化する必要があります。 を呼び出し`NSUrl`て、別のストレージ内のファイルにブックマークを作成します。
1. `NSUrl`ファイルプロバイダーの拡張機能を呼び出して、ドキュメントへの永続的な URL を取得します。 
1. ファイルプロバイダーの拡張機能は、 `NSUrl` URL を文字列としてに返します。
1. は`NSUrl` URL をブックマークにバンドルして、アプリケーションに返します。
1. アプリケーションがバックグラウンドから復帰し、状態を復元する必要があるときに、そのブックマークをに`NSUrl`渡します。
1. `NSUrl`ファイルの URL を使用して、ファイルプロバイダーの拡張子を呼び出します。
1. ファイル拡張子プロバイダーは、ファイルにアクセスして、ファイルの場所を`NSUrl`に返します。
1. ファイルの場所はセキュリティ情報と共にバンドルされ、アプリケーションに返されます。


ここから、アプリケーションはファイルにアクセスして通常どおり操作できます。

### <a name="writing-files"></a>ファイルの書き込み

このセクションでは、ドキュメントプロバイダーの拡張機能を使用して別の場所にファイルを書き込む方法について簡単に説明します。 IOS アプリケーションでは、ファイル調整を使用して、アプリケーションコンテナー内のディスクに情報を保存します。 ファイルが正常に書き込まれるとすぐに、ファイルプロバイダーの拡張機能に変更が通知されます。

この時点で、ファイルプロバイダーの拡張機能は、別の場所へのファイルのアップロードを開始できます (または、ファイルをダーティとしてマークし、アップロードする必要があります)。

### <a name="creating-new-document-provider-extensions"></a>新しいドキュメントプロバイダーの拡張機能の作成

新しいドキュメントプロバイダーの拡張機能の作成は、この入門記事の範囲外です。 この情報は、ユーザーが iOS デバイスに読み込んだ拡張機能に基づいて、アプリケーションが Apple が提供している iCloud の場所の外部にあるドキュメントストレージの場所にアクセスできることを示すために提供されています。

ドキュメントピッカーを使用して外部ドキュメントを操作するときは、開発者がこの事実を認識している必要があります。 これらのドキュメントは iCloud でホストされていると想定しないでください。

ストレージプロバイダーまたはドキュメントピッカー拡張機能の作成の詳細については、[アプリ拡張機能の概要に](~/ios/platform/extensions.md)関するドキュメントを参照してください。

## <a name="migrating-to-icloud-drive"></a>ICloud ドライブへの移行

IOS 8 では、ユーザーは、iOS 7 (およびそれ以前のシステム) で使用されている既存の iCloud ドキュメントシステムを引き続き使用するか、既存のドキュメントを新しい iCloud ドライブメカニズムに移行するかを選択できます。

Mac OS X Yosemite では、Apple は下位互換性を提供しないため、すべてのドキュメントを iCloud ドライブに移行する必要があります。そうしないと、デバイス間で更新されなくなります。

ユーザーのアカウントが iCloud ドライブに移行されると、iCloud ドライブを使用しているデバイスのみが、これらのデバイスにあるドキュメントに変更を伝達できるようになります。

> [!IMPORTANT]
> 開発者は、ユーザーのアカウントが iCloud ドライブに移行されている場合にのみ、この記事で説明されている新機能を利用できることに注意する必要があります。 

## <a name="summary"></a>Summary

この記事では、iCloud ドライブと新しいドキュメントピッカービューコントローラーをサポートするために必要な既存の iCloud Api への変更について説明しました。 ファイルの調整について説明し、クラウドベースのドキュメントを操作するときに重要な理由です。 Xamarin. iOS アプリケーションでクラウドベースのドキュメントを有効にするために必要なセットアップについて説明し、ドキュメントピッカービューコントローラーを使用してアプリのアプリケーションコンテナーの外部にあるドキュメントを操作する方法を紹介しました。

また、この記事では、ドキュメントプロバイダーの拡張機能について簡単に説明し、クラウドベースのドキュメントを処理できるアプリケーションを作成するときに、開発者がそれを認識しておく必要がある理由について説明します。

## <a name="related-links"></a>関連リンク

- [DocPicker (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-docpicker)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [アプリ拡張機能の概要](~/ios/platform/extensions.md)
