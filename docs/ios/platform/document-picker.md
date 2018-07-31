---
title: Xamarin.iOS でドキュメント ピッカー
description: このドキュメントでは、iOS ドキュメント ピッカーをについて説明し、Xamarin.iOS での使用方法について説明します。 ICloud、ドキュメント、共通のセットアップ コード、ドキュメントのプロバイダーの拡張機能での外観がかかります。
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: ca0c7a6e655fdc44aa673a59be71bc83044d3085
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353335"
---
# <a name="document-picker-in-xamarinios"></a>Xamarin.iOS でドキュメント ピッカー

ドキュメント ピッカーにより、アプリ間で共有するドキュメントです。 ICloud または別のアプリのディレクトリに、これらのドキュメントを格納できます。 ドキュメントのセットを使用して共有[ドキュメント プロバイダーの拡張機能](~/ios/platform/extensions.md)ユーザーが自分のデバイスにインストールします。 

ドキュメントのアプリとクラウド間で同期を維持するの難しいのため、一定量のために必要な複雑性が生じます。

## <a name="requirements"></a>要件

この記事で紹介する手順を完了する、次が必要。

-  **Xcode 7 および iOS 8 以降**– Apple の Xcode 7 と iOS 8、または新しい Api がインストールされ、開発者のコンピューターに構成する必要があります。
-  **Visual Studio または Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールする必要があります。
-  **iOS デバイス**– iOS 8 を実行している iOS デバイスまたはそれ以降。

## <a name="changes-to-icloud"></a>ICloud への変更

ドキュメント ピッカーの新機能を実装するには、Apple の iCloud サービスを次の変更を加えられましたがあります。

-  ICloud デーモンが全面的に書き換えられました CloudKit を使用します。
-  機能であった既存 iCloud iCloud ドライブの名前を変更します。
-  ICloud へ、Microsoft Windows OS のサポートが追加されました。
-  Mac OS Finder で、iCloud フォルダーが追加されました。
-  iOS デバイス、Mac OS iCloud フォルダーの内容にアクセスできます。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="what-is-a-document"></a>ドキュメントとは何ですか。

ICloud のドキュメントを指す場合は単一のスタンドアロン エンティティと、によって、ユーザーとして認識される必要があります。 ドキュメントを変更または (電子メールを使用して、たとえば) その他のユーザーと共有するユーザーが決定できます。

いくつかの種類のファイルをユーザーがすぐにわかるページなどのドキュメントとして基調講演や数字のファイルがあります。 ただし、iCloud では、この概念に限定されません。 たとえば、(チェス一致) などのゲームの状態をドキュメントとして扱うおよび iCloud に格納されています。 このファイルは、ユーザーのデバイス間で渡すことができます、ゲームを別のデバイスで中断した場所を選択することを許可します。

## <a name="dealing-with-documents"></a>ドキュメントを処理します。

この記事では Xamarin を使用したドキュメント ピッカーを使用するために必要なコードについて説明、iCloud のドキュメントを操作するためのベスト プラクティスを網羅することは、ドキュメント ピッカーをサポートするために必要ないくつかの既存の Api に加えられた変更前にです。

### <a name="using-file-coordination"></a>ファイルの調整を使用します。

ファイルを変更できるは、いくつかの異なる場所から、ため調整がデータの損失を防ぐために使用する必要があります。

 [![](document-picker-images/image1.png "ファイルの調整を使用します。")](document-picker-images/image1.png#lightbox)

上の図を見てみましょう。

1.  ファイルの調整を使用して iOS デバイスでは、新しい文書を作成し、iCloud フォルダーに保存します。
2.  iCloud は、すべてのデバイスに配布するためのクラウドを変更したファイルを保存します。
3.  添付の Mac では、iCloud フォルダーに変更したファイルを表示し、ファイルに変更内容をコピーするファイルの調整を使用します。
4.  ファイルの調整を使用していないデバイスは、ファイルに変更を加えるし、iCloud フォルダーに保存します。 これらの変更は、他のデバイスにすぐにレプリケートされます。

元を想定しています。 ファイルの編集が iOS デバイスまたは Mac の変更の紛失や一貫性のないデバイスからファイルのバージョンで上書きされるようになりました。 データの損失を防ぐためには、ファイルの調整では、クラウド ベースのドキュメントを使用する場合に、必要がありますです。

### <a name="using-uidocument"></a>UIDocument を使用します。

 `UIDocument` ものごとは簡単な (または`NSDocument`macOS で)、開発者のすべての面倒な作業手順を実行しています。 ビルド、アプリケーションの UI のブロックを防ぐためのバック グラウンド キューとファイルの調整を提供します。

 `UIDocument` 複数を公開する高レベルの Api を開発者の目的では、Xamarin アプリケーションの開発作業を容易にする必要があります。

次のコードのサブクラスを作成する`UIDocument`実装テキスト ベースの汎用的なドキュメントを格納および iCloud からテキストを取得するために使用できます。

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

`GenericTextDocument`ドキュメント ピッカーおよび Xamarin.iOS 8 アプリケーションでの外部のドキュメントを操作にこの記事全体で使用されるクラスの上に表示されます。

## <a name="asynchronous-file-coordination"></a>非同期のファイルの調整

iOS 8 では、新しいファイルの調整の Api を使用していくつかの新しい非同期ファイル調整機能を提供します。 IOS 8 の場合は、前にすべての既存のファイルの調整 Api が完全に同期します。 つまり、開発者が独自のバック グラウンド ファイルの調整がアプリケーションの UI をブロックするを防ぐためにキューを実装する責任を負います。

新しい`NSFileAccessIntent`クラスには、ファイルと必要な調整の種類を制御するいくつかのオプションを参照する URL が含まれています。 次のコード間の 1 つの場所からファイルを移動するインテントを使用します。

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

## <a name="discovering-and-listing-documents"></a>検出およびドキュメントの一覧を表示します。

既存を使用して、検出してドキュメントの一覧を表示する方法は、 `NSMetadataQuery` Api。 このセクションに追加された新しい機能を取り上げます。`NSMetadataQuery`ドキュメントの操作をする前に、よりも簡単に構成します。

### <a name="existing-behavior"></a>既存の動作

IOS 8 では、前に`NSMetadataQuery`などがピックアップするファイルにローカルの変更に遅い: 削除を作成し、名前を変更します。

 [![](document-picker-images/image2.png "NSMetadataQuery ローカル ファイルの変更の概要")](document-picker-images/image2.png#lightbox)

上の図では。

1.  アプリケーションのコンテナーに既に存在するファイルの`NSMetadataQuery`が既存の`NSMetadata`レコードは、事前に作成し、スプール、アプリケーションにすぐに利用できるようにします。
1.  アプリケーションでは、アプリケーション コンテナーで新しいファイルを作成します。
1.  前に遅延がある`NSMetadataQuery`アプリケーション コンテナーに変更を認識し、作成に必要な`NSMetadata`レコード。


作成時に遅延のため、`NSMetadata`アプリケーションに 2 つのデータ ソースを開くには、レコード: ローカル ファイルの変更のいずれかと 1 つのクラウド ベースの変更。

### <a name="stitching"></a>合成

Ios 8、`NSMetadataQuery`合成と呼ばれる新しい機能を直接使用する方が簡単です。

 [![](document-picker-images/image3.png "新しい機能に NSMetadataQuery という合成")](document-picker-images/image3.png#lightbox)

上の図では、合成を使用します。

1.  アプリケーションのコンテナーに既に存在するファイルのと同様に、`NSMetadataQuery`が既存の`NSMetadata`レコードを事前に作成し、スプールします。
1.  アプリケーションでは、ファイルの調整を使用して、アプリケーション コンテナーで新しいファイルを作成します。
1.  アプリケーション コンテナーでのフックは、変更および呼び出しが表示されます。`NSMetadataQuery`作成に必要な`NSMetadata`レコード。
1.  `NSMetadata`レコードは、直後に、ファイルが作成され、アプリケーションに利用可能になっています。


合成を使用して、アプリケーションがなくなったをローカルで監視するデータ ソースを開くし、クラウド ベースのファイルの変更。 アプリケーションが依存できるようになりました`NSMetadataQuery`直接します。

> [!IMPORTANT]
> 合成は、アプリケーションは、前のセクションで紹介されているファイルの調整に使用されている場合にのみ機能します。 ファイルの調整が使用されていない場合、Api は既定のより前の既存の iOS 8 の動作で実行されます。




### <a name="new-ios-8-metadata-features"></a>新しい iOS 8 メタデータ機能

次の新機能が追加されている`NSMetadataQuery`iOS 8 で。

-   `NSMetatadataQuery` クラウドに格納された非ローカル ドキュメントを表示することができますようになりました。
-  クラウド ベースのドキュメントについてのメタデータ情報にアクセスする、新しい Api が追加されました。 
-  新しい`NSUrl_PromisedItems`が使用可能なコンテンツをローカルでない可能性がありますまたは可能性のあるファイルのファイル属性にアクセスする API。
-  使用して、`GetPromisedItemResourceValue`メソッドを指定されたファイルに関する情報を取得または使用して、`GetPromisedItemResourceValues`一度に 1 つ以上のファイルに情報を取得するメソッド。


メタデータを処理するため、2 つの新しいファイルの調整フラグが追加されました。

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


ドキュメント ファイルの内容は、上記のフラグと、ローカルに使用することができるようにする必要はありません。

次のコード セグメントは、使用する方法を示しています。`NSMetadataQuery`を特定のファイルの存在を照会し、存在しない場合は、ファイルをビルドします。

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
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
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

Apple では、アプリケーションのドキュメントの一覧を表示するときは、最適なユーザー エクスペリエンスをプレビューの使用があると考えています。 これにより、エンド ユーザーのコンテキストに使用するドキュメントをすばやく識別できるようにします。

IOS 8 の場合は、前にドキュメント プレビューを表示すると、カスタム実装が必要です。 新しい ios 8 は開発者が簡単にドキュメントのサムネイルで使用できるようにするファイル システム属性。

#### <a name="retrieving-document-thumbnails"></a>ドキュメントのサムネイルを取得します。 

呼び出すことによって、`GetPromisedItemResourceValue`または`GetPromisedItemResourceValues`メソッド、 `NSUrl_PromisedItems` API、`NSUrlThumbnailDictionary`が返されます。 このディクショナリの現在の唯一のキーは、`NSThumbnial1024X1024SizeKey`およびその照合`UIImage`します。

#### <a name="saving-document-thumbnails"></a>ドキュメントのサムネイルを保存しています

使用してサムネイルを保存する最も簡単な方法は、`UIDocument`します。 呼び出すことによって、`GetFileAttributesToWrite`のメソッド、`UIDocument`サムネールを設定するには、自動的に保存されるので、ドキュメント ファイルの場合とします。 ICloud デーモンはこの変更を確認し、iCloud に反映させます。 Mac OS X、サムネイルが開発者向けクイック検索プラグインによって自動的に生成します。

Xamarin iOS 8 でドキュメント ピッカーのビュー コント ローラーを実装する準備がと共に、既存の API への変更の場所でドキュメントを iCloud に基づく作業の基礎をモバイル アプリケーション。


## <a name="enabling-icloud-in-xamarin"></a>Xamarin で iCloud を有効にします。

ドキュメント ピッカーは、Xamarin.iOS アプリケーションで使用できるように、iCloud のサポートを両方と Apple を使用して、アプリケーションで有効にする必要があります。 

次のように手順チュートリアル iCloud のプロビジョニングのプロセスです。

1. ICloud コンテナーを作成します。
2. ICloud App Service を含むアプリ ID を作成します。
3. このアプリ ID を含むプロビジョニング プロファイルを作成します。

[機能の使用](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)ガイドは、最初の 2 つの手順を示します。 次の手順では、プロビジョニング プロファイルを作成する、[プロビジョニング プロファイル](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device)ガイド。



次のように手順チュートリアル iCloud のアプリケーションの構成のプロセス:

次の手順で行います。

1.  Visual studio for Mac または Visual Studio プロジェクトを開きます。
2.  **ソリューション エクスプ ローラー**でプロジェクトを右クリックし、オプションを選択します。
3.  オプション ダイアログ ボックスの選択で**iOS アプリケーション**、いることを確認、**バンドル識別子**で定義されているものと一致する**アプリ ID**上でアプリケーションの作成します。 
4.  選択 **iOS バンドル署名** を選択、**開発者 Identity** と **プロビジョニング プロファイル** 上記で作成しました。
5.  をクリックして、 **OK**  をクリックして変更を保存 ダイアログ ボックスを閉じます。
6.  右クリックして`Entitlements.plist`で、**ソリューション エクスプ ローラー**エディターで開きます。

    > [!IMPORTANT]
    > Visual Studio でする必要がありますを右クリックし、権利エディターを開いて選択**ファイルを開く.** プロパティ リスト エディターの選択

7.  確認**iCloud を有効にする**、 **iCloud ドキュメント**、 **key-value ストレージ**と**CloudKit**します。
8.  確認、**コンテナー** (上記で作成した) と、アプリケーションに存在します。 例 : `iCloud.com.your-company.AppName`
9.  変更内容をファイルに保存します。

権利の詳細についてを参照してください、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)ガイド。

適切に上記のセットアップで、アプリケーションはクラウド ベースのドキュメントと、新しいドキュメント ピッカー ビュー コント ローラーここで使用できます。

## <a name="common-setup-code"></a>共通のセットアップ コード

ドキュメント ピッカーのビュー コント ローラーを始める前に、必要ないくつかの標準セットアップ コードがあります。 まず、アプリケーションの変更を`AppDelegate.cs`ファイルを開き、次のようになります。

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
                    // Yes, inform caller and save location the the Application Container
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
> 上記のコードには、上記の探索およびドキュメントの一覧表示するセクションのコードが含まれています。 ここでは、全体の実際のアプリケーションで表示されます。 わかりやすくするため、この例は、1 つのハード コーディングされたファイルで動作 (`test.txt`) のみです。

上記のコードでは、アプリケーションの残りの部分で操作しやすくためのいくつかの iCloud ドライブ ショートカットを公開します。

次に、任意のビューまたはドキュメント ピッカーを使用してまたはクラウド ベースのドキュメントを操作するビューのコンテナーに次のコードを追加します。

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

取得するショートカットが追加されます、`AppDelegate`上記で作成した iCloud ショートカットにアクセスします。

配置でこのコードでは、Xamarin iOS 8 のアプリケーションでドキュメント ピッカーのビュー コント ローラーの実装を見てをみましょう。

## <a name="using-the-document-picker-view-controller"></a>ドキュメント ピッカーのビュー コント ローラーを使用します。

IOS 8 の場合は、前に、アプリ内からアプリケーションの外部でドキュメントを検出する方法がなかったため、別のアプリケーションからドキュメントにアクセスする非常に困難でした。

### <a name="existing-behavior"></a>既存の動作

 [![](document-picker-images/image31.png "既存の動作の概要")](document-picker-images/image31.png#lightbox)

IOS 8 の前に、外部ドキュメントへのアクセスを見てをみましょう。

1.  まず、ユーザーは、ドキュメントを最初に作成されたアプリケーションを開く必要があります。
1.  ドキュメントが選択されていると、`UIDocumentInteractionController`新しいアプリケーションにドキュメントを送信するために使用します。
1.  最後に、元のドキュメントのコピーは、新しいアプリケーションのコンテナーに配置されます。


そこからドキュメントが開いて編集する 2 つ目のアプリケーションで使用できます。

### <a name="discovering-documents-outside-of-an-apps-container"></a>アプリのコンテナーの外でドキュメントを検出します。

IOS 8 の場合は、アプリケーションを簡単に独自のアプリケーション コンテナー外でドキュメントにアクセスすることは。

 [![](document-picker-images/image32.png "アプリのコンテナーの外でドキュメントを検出します。")](document-picker-images/image32.png#lightbox)

新しい iCloud ドキュメント ピッカーを使用して ( `UIDocumentPickerViewController`)、iOS アプリケーションが直接検出して、アプリケーション コンテナーの外部でアクセスします。 `UIDocumentPickerViewController`検出されたドキュメントをアクセス許可を使用して、ユーザーへのアクセスを許可し、それらを編集するためのメカニズムを提供します。

アプリケーションする必要がありますにオプトイン iCloud ドキュメント ピッカーに表示され、他のアプリケーションを検出し、それらを使用できるように、そのドキュメントを用意します。 アプリケーション コンテナーの共有、編集することは、Xamarin iOS 8 のアプリケーションが`Info.plist`標準テキスト エディターでファイルを開き、ディクショナリの下に次の 2 つの行を追加 (間、`<dict>...</dict>`タグ)。

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController`ユーザーがドキュメントを選択できる優れた新しい UI を提供します。 Xamarin iOS 8 のアプリケーションでは、ドキュメント ピッカーのビュー コント ローラーを表示するには、次の操作を行います。

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
> 開発者が呼び出す必要があります、`StartAccessingSecurityScopedResource`のメソッド、`NSUrl`前に、外部のドキュメントにアクセスできます。 `StopAccessingSecurityScopedResource`ドキュメントが読み込まれているとすぐにセキュリティ ロックを解放するメソッドを呼び出す必要があります。

### <a name="sample-output"></a>出力例

上記のコードが iPhone デバイスで実行時に、ドキュメント ピッカーを表示する方法の例を次に示します。

1.  ユーザーがアプリケーションを起動し、メイン インターフェイスが表示されます。   
 
    [![](document-picker-images/image33.png "メイン インターフェイスが表示されます。")](document-picker-images/image33.png#lightbox)
1.  ユーザー タップ、**アクション**画面の上部にあるボタンをクリックし、選択を求められますが、**ドキュメント プロバイダー**利用可能なプロバイダーの一覧から。   
 
    [![](document-picker-images/image34.png "使用可能なプロバイダーの一覧からドキュメント プロバイダーを選択します。")](document-picker-images/image34.png#lightbox)
1.  **ドキュメント ピッカー ビュー コント ローラー**表示される、選択した**ドキュメント プロバイダー**:   
 
    [![](document-picker-images/image35.png "ドキュメント ピッカーのビュー コント ローラーが表示されます。")](document-picker-images/image35.png#lightbox)
1.  タップする**ドキュメント フォルダー**内容を表示します。   
 
    [![](document-picker-images/image36.png "ドキュメント フォルダーの内容")](document-picker-images/image36.png#lightbox)
1.  ユーザーが選択、**ドキュメント**と**ドキュメント ピッカー**が閉じられました。
1.  メイン インターフェイスが再表示されます、**ドキュメント**外部のコンテナーとその内容の表示から読み込まれます。


ドキュメント ピッカーのビュー コント ローラーの実際の表示は、ユーザーがデバイスにインストールして、ドキュメント ピッカー モードが実装されていますが、ドキュメント プロバイダーに依存します。 上記の例では Open モードを使用して、他の種類のモードは、以下で詳しく説明されます。

## <a name="managing-external-documents"></a>外部のドキュメントを管理します。

IOS 8 では、前に、上記のアプリケーションにアプリケーション コンテナーの一部であったドキュメント アクセスのみでした。 IOS 8 でアプリケーションの外部ソースからドキュメントにアクセスできます。

 [![](document-picker-images/image37.png "外部ドキュメントの管理の概要")](document-picker-images/image37.png#lightbox)

ユーザーは、外部ソースからドキュメントを選択するときに、参照ドキュメントは、元のドキュメントを指すアプリケーション コンテナーに書き込まれます。

既存のアプリケーションにこの新しい機能の追加に役立つ、いくつかの新しい機能が追加されました、 `NSMetadataQuery` API。 通常、アプリケーションは、アプリケーション コンテナー内に居住するドキュメントの一覧に、ユビキタス ドキュメント スコープを使用します。 このスコープを使用して、アプリケーションのコンテナー内のドキュメントのみが表示され続けます。

新しいユビキタス外部ドキュメント スコープを使用すると、アプリケーション コンテナー外のライブおよびそれらのメタデータを返すドキュメントを返します。 `NSMetadataItemUrlKey`ドキュメントが実際に配置されている場所の URL にポイントされます。

アプリケーションは番目の参照によってポイントされているドキュメントを操作する望ましくありません。 代わりに、アプリのリファレンス ドキュメントを直接扱う必要です。 たとえば、UI では、アプリケーションのフォルダーにドキュメントを表示するか、フォルダー内の参照を移動するユーザーを許可するアプリもかまいません。

IOS 8 で新しい`NSMetadataItemUrlInLocalContainerKey`リファレンス ドキュメントに直接アクセスが用意されています。 このキーは、アプリケーション コンテナーでは、外部のドキュメントへの実際の参照を指します。

`NSMetadataUbiquitousItemIsExternalDocumentKey`ドキュメントが外部のアプリケーションのコンテナーかどうかをテストするために使用します。 `NSMetadataUbiquitousItemContainerDisplayNameKey`外部のドキュメントの元のコピーを格納しているコンテナーの名前にアクセスするために使用します。

### <a name="why-document-references-are-required"></a>ドキュメントの参照が必要な理由

その iOS 8 参照を使用して外部のドキュメントにアクセスする主な理由は、セキュリティです。 アプリケーションが指定されていないアクセスを他のアプリケーションのコンテナー。 アウト プロセスで実行中は、システム全体のアクセスを持つためは、ドキュメント ピッカーのみを実行できます。

ドキュメント ピッカーを使用して、アプリケーション コンテナー外のドキュメントを取得する唯一の方法し、ピッカーは、セキュリティ スコープによって、URL が返される場合。 セキュリティ スコープの URL には、スコープを持つドキュメントへのアプリケーション アクセスを許可するために必要な権限と共にドキュメントを選択するのに十分な情報が含まれています。

セキュリティ スコープの URL は、文字列にシリアル化し逆シリアル化が、セキュリティ情報が失われ、ファイルが、URL からアクセスできなくなることに注意してください。 ドキュメントの参照機能は、これらの Url を指すファイルを取得するためのメカニズムを提供します。

アプリケーションが取得した場合は、`NSUrl`リファレンス ドキュメントのいずれかで既にアタッチされている、セキュリティ スコープを持つし、ファイルにアクセスするために使用できます。 このため、強くお勧め、開発者が使用して`UIDocument`にこの情報やプロセスのすべてに対処するためです。

### <a name="using-bookmarks"></a>ブックマークの使用

状態の復元を行うときなど、特定のドキュメントに戻るには、アプリケーションのドキュメントの列挙可能なこととは限りません。 iOS 8 では、特定のドキュメントを直接の対象のブックマークを作成するためのメカニズムを提供します。

次のコードはからブックマークを作成、`UIDocument`の`FileUrl`プロパティ。

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

に対して、既存のブックマークを作成する既存のブックマークの API が使用される`NSUrl`を保存および読み込んで、外部ファイルへの直接アクセスを提供することができます。 次のコードでは、上記で作成されたブックマークがリストアされます。

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Vs を開きます。インポート モードとドキュメント ピッカー

ドキュメント ピッカーのビュー コント ローラーの 2 つの異なるモードを機能します。

1.  **モードを開く**– このモードで、ユーザーが選択し、外部のドキュメントでは、ドキュメント ピッカーはセキュリティ スコープのブックマーク アプリケーションのコンテナーに作成します。   
 
    [![](document-picker-images/image37.png "セキュリティ スコープのアプリケーションのコンテナー内のブックマーク")](document-picker-images/image37.png#lightbox)
1.  **インポート モード**– このモードで、ユーザーが選択したと外部のドキュメント、ドキュメント ピッカーはないブックマークを作成が、代わりに、一時的な場所にファイルをコピーおよびアプリケーションのこの場所にあるドキュメントへのアクセスを提供します。   
 
    [![](document-picker-images/image38.png "ドキュメント ピッカーは一時的な場所にファイルをコピーし、ドキュメント内のこの位置に、アプリケーションのアクセスを提供")](document-picker-images/image38.png#lightbox)   
 何らかの理由でアプリケーションを終了すると、一時的な場所が空になり、ファイルを削除します。 がファイルへのアクセスを維持するために、アプリケーションが必要な場合、コピーを作成し、アプリケーション コンテナーに配置する必要があります。


Open モードは、アプリケーションが別のアプリケーションと連携し、ドキュメントと、そのアプリケーションに加えられた変更を共有する場合に便利です。 インポート モードは、アプリケーションが他のアプリケーションとドキュメントには、その変更を共有しないときに使用されます。

## <a name="making-a-document-external"></a>外部ドキュメントを作成

前述のように、iOS 8 アプリケーションには、独自のアプリケーション コンテナー外のコンテナーへのアクセスはありません。 アプリケーションは、ローカルまたは一時的な場所には、独自のコンテナーに書き込むし、場所を選択したユーザーに、アプリケーション コンテナー外の結果のドキュメントを移動する特殊なドキュメント モードを使用します。

ドキュメントを外部の場所に移動するには、次の操作を行います。

1.  まず、ローカルまたは一時的な場所に新しいドキュメントを作成します。
1.  作成、`NSUrl`新しいドキュメントを指します。
1.  新しいドキュメント ピッカー ビュー コント ローラーを開き、それを渡す、`NSUrl`のモードと`MoveToService`します。 
1.  ユーザーが新しい場所を選択すると、ドキュメントは新しい場所に現在の場所から移動されます。
1.  作成アプリケーションで、ファイルにアクセスすることができるように、リファレンス ドキュメントは、アプリのアプリケーションのコンテナーに書き込まれます。


ドキュメントを外部の場所に移動する、次のコードを使用できます。 `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

上記の手順で返される参照ドキュメントは、正確にドキュメント ピッカーのオープン モードによって作成された 1 つとして同じです。 ただし、アプリケーションへの参照を保持することがなく、ドキュメントを移動する時間があります。

参照を生成せず、ドキュメントを移動するには、使用、`ExportToService`モード。 例 : `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

使用する場合、`ExportToService`モードでは、ドキュメントが外部のコンテナーにコピーされ、既存のコピーが元の場所のままにします。

## <a name="document-provider-extensions"></a>ドキュメント プロバイダーの拡張機能

Ios 8、Apple は、エンドユーザーが実際に存在する場合に関係なく、クラウド ベース ドキュメントのいずれかにアクセスできる必要があります。 この目標を達成するためには、iOS 8 は、新しいドキュメント プロバイダーの拡張機能のメカニズムを提供します。

### <a name="what-is-a-document-provider-extension"></a>ドキュメント プロバイダー拡張機能とは何ですか。

ドキュメント プロバイダー拡張機能は、簡単に言えば、既存 iCloud 記憶域の場所とまったく同じ方法でアクセスできるアプリケーション別のドキュメント ストレージを提供するには、開発者は、またはサード パーティの方法です。

ユーザーはドキュメント ピッカーからこれらの代替の格納場所のいずれかを選択でき、正確な同じのアクセス モード (オープン、インポート、移動またはエクスポート) をその場所でファイルを操作するを使用することができます。

これは、2 つの異なる拡張機能を使用してに実装されます。

-  **ドキュメント ピッカー拡張機能**– 提供、`UIViewController`ユーザーが代替の格納場所からドキュメントを選択するためのグラフィカル インターフェイスを提供するサブクラスです。 このサブクラスは、ドキュメント ピッカーのビュー コント ローラーの一部として表示されます。
-  **ファイル拡張子の提供**– これはファイルの内容を実際に提供することを処理する非 UI 拡張機能。 ファイルの連携を通じて提供されます。 これらの拡張機能 ( `NSFileCoordinator` )。 これは、ファイルの調整が必要になるもう 1 つの重要なケースです。


ドキュメント プロバイダーの拡張機能を使用する場合、次の図は、一般的なデータ フローを示します。

 [![](document-picker-images/image39.png "ドキュメント プロバイダーの拡張機能を使用する場合、この図は、一般的なデータ フロー")](document-picker-images/image39.png#lightbox)

次のプロセスが発生します。

1.  アプリケーションは、ドキュメント ピッカー コント ローラーを使用するファイルを選択して、アクセス許可を表示します。
1.  ユーザーが選択を別のファイルの場所と、カスタム`UIViewController`拡張機能が呼び出され、ユーザー インターフェイスを表示します。
1.  ユーザーがこの場所からファイルを選択して、URL はドキュメント ピッカーに戻されます。
1.  ドキュメント ピッカーでは、ファイルの URL を選択しで作業するユーザーのアプリケーションに返します。
1.  URL は、ファイルの内容をアプリケーションに返されるファイルのコーディネーターに渡されます。
1.  ファイルのコーディネーターは、ファイルを取得するカスタムのファイル プロバイダー拡張機能を呼び出します。
1.  ファイルのコーディネーターには、ファイルの内容が返されます。
1.  ファイルの内容は、アプリケーションに返されます。


### <a name="security-and-bookmarks"></a>セキュリティとブックマーク

このセクションでは、プロバイダーの拡張機能のドキュメントでブックマーク機能を通じてセキュリティと永続的なファイルにアクセスする方法の概要になります。 ICloud ドキュメント プロバイダーは、アプリケーションのコンテナーに、セキュリティとブックマークを自動的に保存とは異なり、ドキュメントの参照をシステムの一部ではないためドキュメント プロバイダーの拡張機能がありません。

例: 独自会社全体のセキュリティで保護されたデータ ストアを提供する、エンタープライズ設定では、管理者がアクセスまたはパブリック iCloud サーバーによって処理された機密の企業の情報をしたくないです。 そのため、組み込みのドキュメント参照システムを使用できません。

引き続きブックマーク システムを使用でき、ブックマークされた URL を正しく処理し、それによって示されるドキュメントの内容を返すファイル プロバイダー拡張機能の役割です。

セキュリティのため、iOS 8 Isolation Layer アプリケーションがどのファイル プロバイダーの内部識別子へのアクセスをに関する情報を保持する必要があります。 この分離のレイヤーによって、すべてのファイル アクセスを管理に注意する必要があります。

次の図は、ブックマークおよびドキュメント プロバイダー拡張機能を使用する場合に、データ フローを示します。

 [![](document-picker-images/image40.png "この図は、ブックマークおよびドキュメント プロバイダー拡張機能を使用する場合に、データ フローを示しています")](document-picker-images/image40.png#lightbox)

次のプロセスが発生します。

1.  アプリケーションはバック グラウンドの入力し、その状態を保持する必要があります。 呼び出す`NSUrl`別のストレージでファイルへのブックマークを作成します。
1.  `NSUrl` ドキュメントに永続的な URL を取得するファイル プロバイダー拡張機能を呼び出します。 
1.  ファイル プロバイダー拡張機能は、文字列として URL を返します、`NSUrl`します。
1.  `NSUrl` URL をブックマークにバンドルし、アプリケーションに返します。
1.  ブックマークを渡すアプリケーションがバック グラウンドで解除されたとき、状態を復元する必要がある場合は、`NSUrl`します。
1.  `NSUrl` ファイルの URL を使用してファイル プロバイダー拡張機能を呼び出します。
1.  ファイル拡張機能プロバイダーは、ファイルにアクセスして、ファイルの場所を返します`NSUrl`します。
1.  ファイルの場所がセキュリティ情報がバンドルされているし、アプリケーションに返されます。


ここでは、アプリケーションは、ファイルにアクセスし、通常どおり使用できます。

### <a name="writing-files"></a>ファイルの書き込み

このセクションでは、別の場所にドキュメント プロバイダーの拡張機能の動作と書き込みのファイルの概要になります。 IOS アプリケーションはアプリケーション コンテナー内のディスクに情報を保存するのにファイルの調整を使用します。 ファイルが正常に書き込まれるとすぐに、変更のファイル プロバイダー拡張機能が通知されます。

この時点では、ファイル プロバイダー拡張機能できる別の場所にファイルのアップロードを開始 (またはダーティで必要とするアップロードとしてファイルをマークします)。

### <a name="creating-new-document-provider-extensions"></a>新しいドキュメント プロバイダーの拡張機能を作成します。

新しいドキュメント プロバイダーの拡張機能を作成するが、この入門記事の範囲外です。 この情報は、ここで表示するには、ユーザーが自分の iOS デバイスで読み込まれた拡張機能に基づいてアプリケーション アクセスがあります iCloud 場所が提供されている Apple の外部ストレージの場所のドキュメントを用意にされています。

ドキュメント ピッカーを使用する場合、開発者はこの事実は外部のドキュメントを操作するとします。 ICloud でこれらのドキュメントがホストされている想定しないでください。

記憶域プロバイダーまたはドキュメント ピッカー拡張機能を作成する方法の詳細についてを参照してください、[アプリ拡張機能の概要](~/ios/platform/extensions.md)ドキュメント。

## <a name="migrating-to-icloud-drive"></a>ICloud ドライブへの移行

Ios 8 では、ユーザーは既存 iCloud を使用して、iOS 7 (および以前のシステム) で使用されるドキュメントのシステムまたは既存のドキュメントを新しい iCloud ドライブ メカニズムに移行を選択して続行できます。

Mac OS X yosemite、Apple が提供されませんが、下位互換性 iCloud ドライブにすべてのドキュメントを移行する必要がありますが不要になったために更新するデバイス間で。

ICloud ドライブをユーザーのアカウントが移行された後、iCloud ドライブを使用してのみのデバイスはこれらのデバイス間でドキュメントに変更を反映することになります。

> [!IMPORTANT]
> 開発者は、この記事で説明する新機能は使用できるは、ユーザーのアカウントが iCloud ドライブへ移行されている場合のみ必要があります。 

## <a name="summary"></a>まとめ

この記事では、既存の iCloud iCloud ドライブと、新しいドキュメント ピッカー ビュー コント ローラーをサポートするために必要な Api への変更について説明しました。 ファイルの調整とは重要なクラウド ベースのドキュメントを扱う場合に理由が説明しました。 Xamarin.iOS アプリケーションでのクラウド ベースのドキュメントを有効にするために必要なセットアップについては、ドキュメント ピッカーのビュー コント ローラーを使用して、アプリのアプリケーション コンテナー外のドキュメントの使用方法の概要の外観を指定します。

さらに、この記事では、プロバイダーの拡張機能のドキュメントと理由、開発者はクラウド ベースのドキュメントを処理できるアプリケーションの作成時にそれらを認識できますを簡単に説明します。

## <a name="related-links"></a>関連リンク

- [DocPicker (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [アプリ拡張機能の概要](~/ios/platform/extensions.md)
