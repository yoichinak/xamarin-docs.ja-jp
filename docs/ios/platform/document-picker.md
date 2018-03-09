---
title: "ドキュメントの選択"
description: "ドキュメント ピッカー ビュー コント ローラーは、ユーザーがアプリケーションのサンド ボックス外にあるファイルへのアクセスを許可します。 これは、アプリ間でドキュメントを共有するための簡単なメカニズムです。 複雑なワークフローは、ユーザーが複数のアプリケーションの 1 つのドキュメントを編集できるので、こともできます。 この記事を紹介 Xamarin.iOS アプリケーションでドキュメント ピッカーを使用しており、サポートするように iCloud ドキュメントの変更が必要です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: a10dcbbdcd7792cb7c54c883566911264b6d81e6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="document-picker"></a>ドキュメントの選択

_ドキュメント ピッカー ビュー コント ローラーは、ユーザーがアプリケーションのサンド ボックス外にあるファイルへのアクセスを許可します。これは、アプリ間でドキュメントを共有するための簡単なメカニズムです。複雑なワークフローは、ユーザーが複数のアプリケーションの 1 つのドキュメントを編集できるので、こともできます。この記事を紹介 Xamarin.iOS アプリケーションでドキュメント ピッカーを使用しており、サポートするように iCloud ドキュメントの変更が必要です。_

ドキュメント ピッカーにより、アプリ間で共有するドキュメントです。 ICloud または別のアプリのディレクトリに、これらのドキュメントを格納可能性があります。 ドキュメントのセットを使用して共有[ドキュメント プロバイダーの拡張](~/ios/platform/extensions.md)ユーザーが自分のデバイスにインストールします。 

アプリとクラウド間で同期されているドキュメントを保持するための難易度のために必要な複雑さ量を導入します。

## <a name="requirements"></a>必要条件

この記事で紹介する手順を完了する、次が必要。

-  **Xcode 7 および iOS 8 以降**– Apple の Xcode 7 および iOS 8、または新しい Api をインストールして、開発者のコンピューター上に構成する必要があります。
-  **Visual Studio または Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールする必要があります。
-  **iOS デバイス**– iOS 8 を実行している iOS デバイスまたはそれ以降。

## <a name="changes-to-icloud"></a>ICloud への変更

ドキュメント ピッカーの新機能を実装するのには、次の変更を Apple の iCloud サービスに加えられましたが。

-  ICloud デーモンは完全に書き直さ CloudKit を使用します。
-  機能であった既存 iCloud iCloud ドライブの名前を変更します。
-  ICloud へ、Microsoft Windows OS のサポートが追加されました。
-  Mac OS ファインダー iCloud フォルダーが追加されました。
-  iOS デバイスは、Mac OS iCloud フォルダーの内容にアクセスできます。


## <a name="what-is-a-document"></a>ドキュメントとは何ですか。

ICloud でのドキュメントを参照するとき、には、単一のスタンドアロン エンティティであり、ユーザーによってように認識する必要があります。 ユーザーは、ドキュメントを変更または (を使用して、電子メールなど) の他のユーザーと共有する必要があります。

ファイルをユーザーはすぐにファイルを認識ページなどのドキュメントとして Keynote または数値のいくつかの種類があります。 ただし、iCloud では、この概念に制限はありません。 たとえば、(チェス一致) などのゲームの状態をドキュメントとして扱われます、iCloud に格納されています。 このファイルは、ユーザーのデバイス間で渡すことができ、別のデバイスで中断した場所、ゲームを選択できるようにを許可します。

## <a name="dealing-with-documents"></a>ドキュメントを処理します。

ドキュメント ピッカーを使用して、Xamarin を使用するためのコードに進むには、この記事の内容を移動 iCloud ドキュメントを操作するためのベスト プラクティスをカバーして、ドキュメントの選択をサポートするために必要ないくつかの既存の Api に加えられた変更前にします。

### <a name="using-file-coordination"></a>ファイルの調整を使用します。

複数の場所から、ファイルを変更することができます、ために、データ損失を防ぐための調整を使用する必要があります。

 [ ![](document-picker-images/image1.png "ファイルの調整を使用します。")](document-picker-images/image1.png)

上の図を見てをみましょう。

1.  ファイルの調整を使用して iOS デバイスでは、新しい文書を作成し、iCloud フォルダーに保存します。
2.  iCloud は、すべてのデバイスに配布するためのクラウドを変更したファイルを保存します。
3.  接続された Mac は、iCloud フォルダーに変更したファイルを表示し、ファイルに変更内容をコピーするファイルの調整を使用します。
4.  ファイルの調整を使用していないデバイスは、ファイルに変更が行われ、iCloud フォルダーに保存します。 これらの変更は、他のデバイスにすぐにレプリケートされます。

元のファイルの編集された iOS デバイスまたは Mac の変更の紛失や調整デバイスからファイルのバージョンで上書きされるようになりました。 データの損失を回避するのには、ファイルの調整は、クラウド ベースのドキュメントを操作するときに必要がありますです。

### <a name="using-uidocument"></a>UIDocument を使用します。

 `UIDocument` 簡単に処理 (または`NSDocument`macos) 開発者にとって面倒な作業のすべての手順を実行しています。 ファイルの調整のビルドと、アプリケーションの UI のブロックを防止するバック グラウンド キューが用意されています。

 `UIDocument` 複数の公開、開発者の目的での Xamarin アプリケーションの開発作業を容易にする高度な Api が必要です。

次のコードのサブクラスを作成する`UIDocument`格納および iCloud からテキストを取得するために使用する汎用のテキストに基づくドキュメントを実装します。

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

`GenericTextDocument`ドキュメント ピッカーおよび Xamarin.iOS 8 アプリケーションの外部のドキュメントを操作するときにその上に示したクラスがこの記事全体で使用されます。

## <a name="asynchronous-file-coordination"></a>非同期のファイルの調整

iOS 8 では、ファイルの調整の新しい Api を使用していくつかの新しい非同期ファイル調整機能を提供します。 IOS 8 の場合は、前にすべての既存のファイルの調整 Api が完全に同期します。 これは、ため、開発者がアプリケーションの UI がブロックされないファイルの調整を防ぐためにキューが自分のバック グラウンドの実装を担当します。

新しい`NSFileAccessIntent`クラスには、ファイル サービスおよび必要な調整の種類を制御するいくつかのオプションを指す URL が含まれています。 次のコード ファイルを 1 つの場所間で移動するインテントを使用します。

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

## <a name="discovering-and-listing-documents"></a>検出して、ドキュメントの一覧を表示します。

検出して文書を一覧表示する方法が、既存の使用を`NSMetadataQuery`Api です。 このセクションの内容に追加された新機能`NSMetadataQuery`ドキュメントの操作をする前よりも簡単に構成します。

### <a name="existing-behavior"></a>既存の動作

IOS 8 の前に`NSMetadataQuery`などがピックアップするファイルにローカルの変更に遅く: 削除を作成し、名前を変更します。

 [ ![](document-picker-images/image2.png "NSMetadataQuery ローカル ファイルの変更の概要")](document-picker-images/image2.png)

上記の図の説明。

1.  アプリケーションのコンテナーに既に存在するファイルの`NSMetadataQuery`が既存の`NSMetadata`レコードを事前に作成し、アプリケーションにすぐに利用できるように、スプールされました。
1.  アプリケーションでは、アプリケーション コンテナーで新しいファイルを作成します。
1.  前に遅延がある`NSMetadataQuery`アプリケーション コンテナーに変更を認識し、必要な作成`NSMetadata`レコード。


作成時に遅延のため、`NSMetadata`アプリケーションには 2 つのデータ ソースを開くには、レコード: ローカル ファイルの変更のいずれかと 1 つのクラウド ベースの変更。

### <a name="stitching"></a>合成

Ios 8、`NSMetadataQuery`合成と呼ばれる新しい機能を直接使用する方が簡単です。

 [ ![](document-picker-images/image3.png "新機能により、NSMetadataQuery に合成が呼び出されます")](document-picker-images/image3.png)

上の図では、合成を使用します。

1.  アプリケーションのコンテナーに既に存在するファイルを以前と同様、`NSMetadataQuery`が既存の`NSMetadata`レコードを事前に作成し、スプールします。
1.  アプリケーションでは、ファイルの調整を使用してアプリケーション コンテナーで新しいファイルを作成します。
1.  フックをアプリケーション コンテナーで、変更と表示呼び出し`NSMetadataQuery`を必要な作成`NSMetadata`レコード。
1.  `NSMetadata`レコードがファイルのすぐ後に作成され、アプリケーションに使用可能になります。


合成を使用して、アプリケーションがなくなったを開くには、データ ソースをローカルで監視して、クラウド ベースのファイルの変更。 これで、アプリケーションの利用`NSMetadataQuery`直接です。

> [!IMPORTANT]
> **注**: 合成前のセクションで説明したよう、アプリケーションはファイルの調整を使用する場合にのみ機能します。 ファイルの調整が使用されていない場合、Api 既定より前の既存の iOS 8 の動作になります。




### <a name="new-ios-8-metadata-features"></a>IOS 8 のメタデータの新機能

次の新機能が追加された`NSMetadataQuery`ios 8。

-   `NSMetatadataQuery` クラウドに格納された非ローカル ドキュメントの一覧を表示できますようになりました。
-  クラウド ベースのドキュメントについてのメタデータ情報にアクセスする、新しい Api が追加されました。 
-  新しい`NSUrl_PromisedItems`が利用可能なコンテンツをローカルでない可能性がありますまたは可能性のあるファイルのファイル属性にアクセスする API。
-  使用して、`GetPromisedItemResourceValue`ため、指定されたファイルに関する情報を取得または使用するメソッド、`GetPromisedItemResourceValues`メソッドに一度に 1 つ以上のファイルの情報を取得します。


メタデータを処理するため、2 つの新しいファイル調整フラグが追加されました。

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


上記のフラグを使ってドキュメント ファイルの内容は利用できるようにローカルに使用することは必要ありません。

次のコード セグメントが使用する方法を示します`NSMetadataQuery`を特定のファイルの存在をクエリおよびが存在しない場合は、ファイルをビルドします。

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

Apple では、アプリケーションのドキュメントを一覧表示するときは、最適なユーザー エクスペリエンスをプレビューの使用があると考えています。 最後のユーザー コンテキストのためそれらを使用するドキュメントをすばやく識別できます。

IOS 8 の場合は、前にドキュメント プレビューの表示と、カスタムの実装が必要です。 IOS に新しい 8 は、ドキュメントのサムネイルを簡単に使用する開発者は、使用されるファイル システム属性。

#### <a name="retrieving-document-thumbnails"></a>ドキュメントのサムネイルを取得します。 

呼び出して、`GetPromisedItemResourceValue`または`GetPromisedItemResourceValues`メソッド、 `NSUrl_PromisedItems` API、`NSUrlThumbnailDictionary`が返されます。 現在このディクショナリ内の唯一のキーは、`NSThumbnial1024X1024SizeKey`およびその照合`UIImage`です。

#### <a name="saving-document-thumbnails"></a>ドキュメントのサムネイルを保存しています

使用してサムネイルを保存する最も簡単な方法は、`UIDocument`です。 呼び出して、`GetFileAttributesToWrite`のメソッド、`UIDocument`サムネールを設定するには、自動的に保存されるので、ドキュメント ファイルがときとします。 ICloud デーモンでは、この変更を確認しを iCloud に伝達します。 Mac OS X のサムネイルが自動的に生成、開発者のクイック検索プラグインによってです。

代わりに、既存の API への変更と共にドキュメントを iCloud ベース作業の基礎を準備が整いました Xamarin iOS 8 のドキュメント ピッカー ビュー コント ローラーを実装するモバイル アプリケーションです。


## <a name="enabling-icloud-in-xamarin"></a>Xamarin で iCloud を有効にします。

Xamarin.iOS アプリケーションでは、ドキュメント ピッカーを使用することができます、前に、iCloud サポートは、アプリケーションとの両方で Apple によって有効にする必要があります。 

次の手順チュートリアル iCloud のプロビジョニングのプロセスです。

1. ICloud、コンテナーを作成します。
2. App Service の iCloud を含むアプリ ID を作成します。
3. このアプリ ID を含むプロビジョニング プロファイルを作成します。

[機能を備えた作業](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)ガイドは、最初の 2 つの手順を示します。 プロビジョニング プロファイルを作成する手順を[プロビジョニング プロファイル](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile)ガイドです。



次のように手順チュートリアル iCloud 用にアプリケーションを構成するプロセス:

次の手順で行います。

1.  Mac または Visual Studio の Visual Studio でプロジェクトを開きます。
2.  **ソリューション エクスプ ローラー**プロジェクトを右クリックし、オプションを選択します。
3.  オプション ダイアログ ボックスの選択で**iOS アプリケーション**、いることを確認、**バンドル Id**で定義されているものと一致する**アプリ ID**アプリケーション用の上に作成します。 
4.  選択 **iOS バンドル署名** を選択、**開発者 Identity** と **プロビジョニング プロファイル** 上記で作成しました。
5.  をクリックして、 **OK**変更を保存し、ダイアログ ボックスを閉じるボタンをクリックします。
6.  右クリックして`Entitlements.plist`で、**ソリューション エクスプ ローラー**をエディターで開きます。

    > [!IMPORTANT]
> **注**: Visual Studio での選択を右クリックして、権利エディターを開くには必要があります**ファイルを開く.** プロパティ リスト エディターを選択して

7.  確認**iCloud を有効にする**、 **iCloud ドキュメント**、**キーと値の記憶域**と**CloudKit**です。
8.  確認、**コンテナー** (上記で作成した) と、アプリケーションに存在します。 例 : `iCloud.com.your-company.AppName`
9.  変更内容をファイルに保存します。

権利の詳細についてを参照してください、[権利に関する作業](~/ios/deploy-test/provisioning/entitlements.md)ガイドです。

場所で上記のセットアップで、アプリケーションはクラウド ベースのドキュメントおよびドキュメント ピッカー ビューのコント ローラーここで使用できます。

## <a name="common-setup-code"></a>一般的なセットアップ コード

始める前にドキュメント ピッカー ビュー コント ローラーで、必要ないくつかの標準セットアップ コードがあります。 アプリケーションの変更することによって開始`AppDelegate.cs`ファイルし、次のようになります。

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
> **注**: 上記のコードには、探索およびドキュメントの一覧表示する前のセクションのコードが含まれています。 ここで示す全体、実際のアプリケーションで表示されます。 わかりやすくするため、この例は、ハード コーディングされた 1 つのファイルで動作 (`test.txt`) のみです。

上記のコードでは、アプリケーションの残りの部分で作業しやすくためのいくつかの iCloud ドライブ ショートカットを公開します。

次に、任意のビューまたはビューのコンテナー ドキュメント ピッカーを使用しているかクラウド ベースのドキュメントの操作は次のコードを追加します。

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

取得するショートカットが追加されます、`AppDelegate`し、上記で作成した iCloud ショートカットにアクセスします。

配置でこのコードでは、Xamarin iOS 8 アプリケーションでドキュメント ピッカー ビュー コント ローラーの実装を見てをみましょう。

## <a name="using-the-document-picker-view-controller"></a>ドキュメント ピッカー ビュー コント ローラーを使用します。

IOS 8 の場合は、前に、ドキュメントにアクセスする別のアプリケーションから、アプリ内からアプリケーションの外部でドキュメントを検出する方法がなかったので非常に困難でした。

### <a name="existing-behavior"></a>既存の動作

 [ ![](document-picker-images/image31.png "既存の動作の概要")](document-picker-images/image31.png)

IOS 8 の前に外部のドキュメントへのアクセスを見てをみましょう。

1.  まず、ユーザーは、元ドキュメントを作成したアプリケーションを開く必要があります。
1.  ドキュメントが選択されていると、`UIDocumentInteractionController`新しいアプリケーションにドキュメントを送信するために使用します。
1.  最後に、元のドキュメントのコピーは、新しいアプリケーションのコンテナーに格納されます。


そこから、ドキュメントが開いて編集するのには、2 番目のアプリケーションの使用可能です。

### <a name="discovering-documents-outside-of-an-apps-container"></a>アプリのコンテナーの外部でドキュメントを検出します。

8、iOS では、アプリケーションがアプリケーション コンテナー外のドキュメントを簡単にアクセスできません。

 [ ![](document-picker-images/image32.png "アプリのコンテナーの外部でドキュメントを検出します。")](document-picker-images/image32.png)

新しい iCloud ドキュメント ピッカーを使用して ( `UIDocumentPickerViewController`)、iOS アプリケーションを直接検出し、アプリケーションのコンテナーの外部でアクセスします。 `UIDocumentPickerViewController`提供アクセス許可を使用して、ユーザーへのアクセスを付与し、それらを編集するためのメカニズムがドキュメントを検出します。

アプリケーションする必要がありますオプトインそのドキュメントを iCloud ドキュメント ピッカーに表示され、他のアプリケーションを検出し、作業中に使用できます。 Xamarin iOS 8 アプリケーションのアプリケーションのコンテナーの共有を編集して`Info.plist`標準テキスト エディターでファイルし、ディクショナリの下に次の 2 つの行を追加 (間、`<dict>...</dict>`タグ)。

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController`ユーザーがドキュメントを選択できる優れた新しい UI を提供します。 Xamarin iOS 8 アプリケーションでドキュメント ピッカー ビュー コント ローラーを表示するには、次の操作を行います。

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
> **注**: 開発者が呼び出す必要があります、`StartAccessingSecurityScopedResource`のメソッド、`NSUrl`外部ドキュメントにアクセスする前に、。 `StopAccessingSecurityScopedResource`ロックを解除する、セキュリティ、ドキュメントが読み込まれたとすぐにメソッドを呼び出す必要があります。

### <a name="sample-output"></a>出力例

上記のコードが iPhone デバイス上で実行時にドキュメント ピッカーを表示する方法の例を次に示します。

1.  ユーザーがアプリケーションを起動し、メインのインターフェイスが表示されます。   
 
    [ ![](document-picker-images/image33.png "メインのインターフェイスが表示されます。")](document-picker-images/image33.png)
1.  ユーザー 2 回のタップ、**アクション**画面の上部にあるボタンを指定して、**ドキュメント プロバイダー**利用可能なプロバイダーの一覧から。   
 
    [ ![](document-picker-images/image34.png "ドキュメント プロバイダーを使用可能なプロバイダーの一覧から選択します。")](document-picker-images/image34.png)
1.  **ドキュメント ピッカー ビュー コント ローラー**表示は、選択した**ドキュメント プロバイダー**:   
 
    [ ![](document-picker-images/image35.png "ドキュメントの選択ビュー コント ローラーが表示されます。")](document-picker-images/image35.png)
1.  ユーザーはタップ操作で、**ドキュメント フォルダー**内容を表示します。   
 
    [ ![](document-picker-images/image36.png "ドキュメント フォルダーの内容")](document-picker-images/image36.png)
1.  ユーザーが選択、**ドキュメント**と**ドキュメント ピッカー**が閉じられます。
1.  メインのインターフェイスが再表示されます、**ドキュメント**外部のコンテナーとその内容の表示から読み込まれます。


ドキュメント ピッカー ビュー コント ローラーの実際の表示は、ユーザーがデバイスにインストールされていることと、どのドキュメントの選択モードが実装されましたドキュメント プロバイダーによって異なります。 上記の例が開いているモードを使用して、他の種類のモードは、以下で詳細で説明します。

## <a name="managing-external-documents"></a>外部のドキュメントを管理します。

IOS 8 の前に、前述のとおり、アプリケーションにアプリケーション コンテナーの一部であったドキュメント アクセスのみでした。 IOS 8 でアプリケーションの外部ソースからドキュメントにアクセスできます。

 [ ![](document-picker-images/image37.png "外部のドキュメントの管理の概要")](document-picker-images/image37.png)

ユーザーは、外部ソースからドキュメントを選択するときに、参照ドキュメントは、元のドキュメントを参照するアプリケーションのコンテナーに書き込まれます。

役立てるため、既存のアプリケーションにこの新しい機能を追加することで、いくつかの新機能が追加されて、 `NSMetadataQuery` API です。 通常、アプリケーションは、アプリケーション コンテナー内に居住するドキュメントの一覧にユビキタス ドキュメント スコープを使用します。 このスコープを使用して、アプリケーションのコンテナー内のドキュメントのみが引き続き表示されます。

スコープを使用して、新しい場所を問わず外部ドキュメントは、アプリケーション コンテナー外のライブし、それらのメタデータを返しているドキュメントを返します。 `NSMetadataItemUrlKey`ドキュメントが実際に配置されている URL にポイントされます。

場合もあります th 参照によって示されるドキュメントを扱うアプリケーション許可したくないです。 代わりに、アプリは、リファレンス ドキュメントを直接操作しようとします。 たとえば、アプリは、UI では、アプリケーションのフォルダーにドキュメントを表示するか、フォルダー内の参照を移動するユーザーを許可する必要あります。

8、iOS で新しい`NSMetadataItemUrlInLocalContainerKey`リファレンス ドキュメントに直接アクセス用に用意されています。 このキーは、アプリケーション コンテナーで外部のドキュメントへの実際の参照を指します。

`NSMetadataUbiquitousItemIsExternalDocumentKey`ドキュメントが外部のアプリケーションのコンテナーかどうかをテストするために使用します。 `NSMetadataUbiquitousItemContainerDisplayNameKey`外部ドキュメントの元のコピーを格納しているが、コンテナーの名前にアクセスするために使用します。

### <a name="why-document-references-are-required"></a>ドキュメントの参照が必要な理由

その iOS 8 の参照を使用して外部ドキュメントにアクセスする主な理由は、セキュリティです。 アプリケーションを指定しないアクセスを他のアプリケーションのコンテナーです。 アウト プロセスで実行されているは、システム全体のアクセスを持つためには、ドキュメントの選択だけを実行できます。

アプリケーション コンテナー外のドキュメントを取得する唯一の方法は、ドキュメント ピッカーを使用して、およびピッカーは、セキュリティ スコープによって、URL が返される場合。 セキュリティ スコープ URL には、ドキュメントへのアプリケーション アクセスを許可するために必要なスコープを持つ権利と共にドキュメントを選択するのに十分な情報が含まれています。

セキュリティ スコープ URL は、文字列にシリアル化し、逆シリアル化が、セキュリティ情報が失われると、ファイルに URL からアクセスできなくなる重要です。 ドキュメントの参照機能は、これらの Url を指すファイルに戻るするメカニズムを提供します。

アプリケーションが入手した場合は、`NSUrl`参考資料のいずれかで既に接続されているセキュリティ スコープを持つし、ファイルにアクセスするために使用できます。 このため、強くお勧め、開発者が使用`UIDocument`それらのすべての情報とプロセスの処理するためです。

### <a name="using-bookmarks"></a>ブックマークの使用

ない常に可能な状態の復元を行うとき、たとえば、特定のドキュメントに戻るには、アプリケーションのドキュメントを列挙します。 iOS 8 では、直接、特定のドキュメントの対象のブックマークを作成するためのメカニズムを提供します。

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

に対して、既存のブックマークを作成する、既存のブックマーク API が使用される`NSUrl`を保存および読み込んで、外部ファイルへの直接アクセスを提供することができます。 次のコードの上に作成されたブックマークが復元されます。

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Vs を開きます。インポート モードとドキュメントの選択

ドキュメントの選択ビュー コント ローラーの機能の操作の 2 つの異なるモード。

1.  **モードを開く**– このモードでユーザーを選択し、外部ドキュメントは、ドキュメントの選択はブックマークを作成、セキュリティ スコープ アプリケーション コンテナーでとき。   
 
    [ ![](document-picker-images/image37.png "セキュリティのアプリケーションのコンテナー内のブックマークのスコープ")](document-picker-images/image37.png)
1.  **インポート モード**– このモードでと、ユーザーを選択し、外部ドキュメント、ドキュメントの選択はいないブックマークを作成が、代わりに、一時的な場所にファイルをコピーおよびアプリケーションのドキュメント内のこの位置へのアクセスを提供します。   
 
    [ ![](document-picker-images/image38.png "ドキュメントの選択は、一時的な場所にファイルをコピーし、ドキュメント内のこの場所にアプリケーションへのアクセスを提供")](document-picker-images/image38.png)   
 何らかの理由で、アプリケーションを終了後一時的な場所が空になり、ファイルを削除します。 アプリケーション ファイルへのアクセスを維持する場合は、コピーを作成し、アプリケーション コンテナーに配置することが必要があります。


Open モードは、アプリケーションが別のアプリケーションとの共同作業し、そのアプリケーションを使用してドキュメントに加えられた変更を共有する場合に便利です。 インポート モードは、アプリケーションが他のアプリケーションとドキュメントには、その変更を共有しない場合に使用されます。

## <a name="making-a-document-external"></a>外部のドキュメントを行う

前述のように、iOS 8 アプリケーションには独自のアプリケーション コンテナー外のコンテナーへのアクセスはありません。 アプリケーションはローカルまたは一時的な場所に、独自のコンテナーに書き込むことができますし、特殊なドキュメント モードを使用して場所を選択したユーザーにアプリケーション コンテナー外の結果のドキュメントを移動します。

ドキュメントを外部の場所に移動するには、次の操作を行います。

1.  最初のローカルまたは一時的な場所に、新しいドキュメントを作成します。
1.  作成、`NSUrl`新しいドキュメントを指しています。
1.  新しいドキュメント ピッカー ビュー コント ローラーを開き、それを渡す、`NSUrl`のモードと`MoveToService`です。 
1.  ユーザーが新しい場所を選択した後、ドキュメントは、新しい場所を現在の場所から移動されます。
1.  リファレンス ドキュメントは、作成元のアプリケーションで、ファイルにアクセスすることができるように、アプリのアプリケーションのコンテナーに書き込まれます。


次のコードは、外部の場所に、ドキュメントの移動を使用できます。 `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

上記のプロセスによって返される参照ドキュメントは、ドキュメント ピッカーのオープン モードによって作成されたいずれかと同じではまったくです。 ただし、アプリケーションへの参照を保持することがなく、ドキュメントを移動する時間があります。

参照を生成せず、ドキュメントを移動するには、使用、`ExportToService`モード。 例 : `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

使用する場合、`ExportToService`モード、外部のコンテナーに、ドキュメントをコピーおよび既存のコピーは、元の場所のままにします。

## <a name="document-provider-extensions"></a>ドキュメント プロバイダー拡張機能

Ios 8、Apple は、エンドユーザーが実際に存在する場合に関係なく、自分のクラウド ベース ドキュメントのすべてにアクセスできる必要があります。 この目標を達成するのには、iOS 8 は、新しいドキュメント プロバイダー拡張メカニズムを提供します。

### <a name="what-is-a-document-provider-extension"></a>ドキュメント プロバイダー拡張機能とは何ですか。

ドキュメント プロバイダー拡張機能は、簡単に言うと、既存の iCloud 記憶域の場所として、まったく同じ方法でアクセスできる、アプリケーションの代替のドキュメントの記憶域を提供する、developer、またはサード パーティの方法です。

ユーザーは、ドキュメント ピッカーからこれらの代替の格納場所のいずれかを選択できますおよびの正確な同じアクセス モード (開く、インポート、移動またはエクスポート) をその場所にファイルを操作するを使用することができます。

これを実装する 2 つの異なる拡張機能。

-  **ピッカーの拡張機能を文書化**– 提供、`UIViewController`ユーザーが代替の格納場所からドキュメントを選択するためのグラフィカル インターフェイスを提供するサブクラスです。 このサブクラスは、ドキュメント ピッカー ビュー コント ローラーの一部として表示されます。
-  **ファイル拡張子の提供**– これは実際には、ファイルの内容を提供するを処理する非 UI 拡張機能です。 これらの拡張機能は、ファイルの調整により提供されます ( `NSFileCoordinator` )。 これは、ファイルの調整が必要な他の重要なケースです。


ドキュメント プロバイダー拡張機能を使用する場合、次の図は、一般的なデータ フローを示しています。

 [ ![](document-picker-images/image39.png "この図は、ドキュメント プロバイダー拡張機能を使用する場合に、一般的なデータ フローを示しています")](document-picker-images/image39.png)

次の処理が行われます。

1.  アプリケーションを使用するファイルを選択して、ユーザーを許可するドキュメントの選択のコント ローラーを表示します。
1.  ユーザーが選択を別のファイルの場所と、カスタム`UIViewController`拡張機能が呼び出され、ユーザー インターフェイスを表示します。
1.  ユーザーがこの場所からファイルを選択して、URL が、ドキュメント ピッカーに戻されます。
1.  ドキュメントの選択は、ファイルの URL を選択しで作業するユーザーのアプリケーションに返します。
1.  URL は、ファイルの内容をアプリケーションに返されるファイルのコーディネーターに渡されます。
1.  ファイル コーディネーターは、ファイルを取得するカスタムのファイル プロバイダー拡張機能を呼び出します。
1.  ファイルの内容は、ファイルのコーディネーターに返されます。
1.  ファイルの内容は、アプリケーションに返されます。


### <a name="security-and-bookmarks"></a>セキュリティとブックマーク

このセクションでは、簡単に見てドキュメント プロバイダー拡張機能でのブックマーク機能を通じて、セキュリティおよび永続的なファイルがアクセスする方法になります。 ドキュメント プロバイダーは、アプリケーションのコンテナーに、セキュリティとブックマークを自動的に保存、iCloud とは異なりドキュメント プロバイダー拡張機能は、ドキュメントのリファレンス システムの一部ではないためにしません。

例: 独自会社全体のセキュリティで保護されたデータ ストアを提供する、エンタープライズ設定では、管理者が機密の企業情報にアクセスまたはパブリック iCloud サーバーによって処理をしたくないです。 そのため、組み込みのドキュメント参照システムを使用できません。

であっても、ブックマーク システムにできるし、は、プロバイダーのファイル拡張子のブックマークが付けられた URL を正しく処理し、これによって示されるドキュメントの内容を返す必要があります。

セキュリティのため、iOS 8 は、どのアプリケーションがどのファイル プロバイダーの内部識別子へのアクセス情報を保持する分離層を持っています。 この分離層によってすべてのファイル アクセスが制御されることに注意してください。

次の図は、ブックマークおよびドキュメント プロバイダー拡張機能を使用する場合に、データ フローを示しています。

 [ ![](document-picker-images/image40.png "この図は、ブックマークおよびドキュメント プロバイダー拡張機能を使用する場合に、データ フローを示しています")](document-picker-images/image40.png)

次の処理が行われます。

1.  アプリケーションにバック グラウンド入ろうとして、その状態を維持する必要があります。 呼び出す`NSUrl`代わりのストレージでファイルにブックマークを作成します。
1.  `NSUrl` ドキュメントへの永続的な URL を取得するファイルのプロバイダー拡張機能を呼び出します。 
1.  ファイル プロバイダー拡張機能は、文字列として URL を返します、`NSUrl`です。
1.  `NSUrl` URL をブックマークにバンドルし、アプリケーションに返します。
1.  ブックマークを渡すと、アプリケーションはバック グラウンドでされているが解除されたときの状態を復元する必要があります、`NSUrl`です。
1.  `NSUrl` ファイルの URL を使用してファイルのプロバイダー拡張機能を呼び出します。
1.  ファイル拡張機能プロバイダーは、ファイルにアクセスして、ファイルの場所を返します`NSUrl`です。
1.  ファイルの場所はセキュリティ情報にバンドルされているし、アプリケーションに返されます。


ここでは、アプリケーションがファイルにアクセスし、通常どおり操作できます。

### <a name="writing-files"></a>ファイルの書き込み

このセクションでは、簡単に見てを別の場所のドキュメント プロバイダー拡張機能の動作と書き込みのファイルになります。 IOS アプリケーションは、アプリケーション コンテナー内のディスクに情報を保存するのにファイルの調整に使用されます。 ファイルが正常に書き込まれるとすぐには、変更のプロバイダーのファイル拡張機能が通知されます。

この時点では、プロバイダーのファイル拡張機能できる別の場所にファイルのアップロードを開始 (またはファイルをダーティと必要とするアップロードとしてマーク)。

### <a name="creating-new-document-provider-extensions"></a>新しいドキュメント プロバイダー拡張機能の作成

新しいドキュメント プロバイダー拡張機能の作成はこの記事は入門編のスコープ外です。 この情報はここで表示するには、iOS デバイスにユーザーが読み込まれた拡張機能に基づくアプリケーション アクセスがあります iCloud 場所の指定、Apple の外部記憶域の場所のドキュメントを提供。

ドキュメント ピッカーを使用する場合、開発者はこの事実を認識する必要がありますと外部のドキュメントを操作します。 ICloud でそれらのドキュメントがホストされていると想定する必要があります。

記憶域プロバイダーまたはドキュメント ピッカーの拡張機能を作成する方法の詳細についてを参照してください、[アプリ拡張機能の概要](~/ios/platform/extensions.md)ドキュメント。

## <a name="migrating-to-icloud-drive"></a>ICloud ドライブへの移行

Ios 8 の場合は、ユーザーは、引き続き既存の iCloud を使用して、iOS 7 (および以前のシステム) で使用されるシステムのドキュメントまたは既存のドキュメントを iCloud の新しいドライブ機構に移行するよう選択できますを選択できます。

Mac OS X Yosemite を Apple の管轄外の後方互換性ので、すべてのドキュメントを iCloud ドライブへ移行する必要がありますまたはこれらが不要になった更新のデバイス間でします。

ユーザーのアカウントは、ドライブを iCloud に移行されているが後、iCloud ドライブを使用してのみのデバイスは同じドキュメントにそれらのデバイス間での変更を反映することができます。

> [!IMPORTANT]
> **注**: 開発者は、この記事で取り上げられている新しい機能が使用できるは、ユーザーのアカウントが iCloud ドライブへ移行されている場合のみことに注意してください。 

## <a name="summary"></a>まとめ

この記事では、既存の iCloud iCloud ドライブと、新しいドキュメント ピッカー ビュー コント ローラーをサポートするために必要な Api への変更について説明しました。 これは、ファイルを調整し、それがなぜ重要なクラウド ベースのドキュメントを操作するときにについて説明しました。 Xamarin.iOS アプリケーションでのクラウド ベースのドキュメントを有効にするために必要なセットアップをカバーし、ドキュメント ピッカー ビュー コント ローラーを使用して、アプリのアプリケーション コンテナー外のドキュメントの操作の概要の外観を指定します。

さらに、この記事は、ドキュメント プロバイダー拡張機能と理由、開発者に注意してくださいそれらのクラウド ベースのドキュメントを処理できるアプリケーションを作成するときを簡単に説明します。

## <a name="related-links"></a>関連リンク

- [DocPicker (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [アプリの拡張機能の概要](~/ios/platform/extensions.md)
