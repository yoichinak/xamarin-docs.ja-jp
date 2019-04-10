---
title: Azure Storage 内のデータ格納とアクセス
description: Azure Storage とは、非構造化、および構造化データの格納に使用できるスケーラブルなクラウド ストレージ ソリューションです。 この記事では、Xamarin.Forms を使用して Azure Storage にテキストおよびバイナリ データを格納する方法と、データにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/28/2018
ms.openlocfilehash: 4ecffa0902d186b659e7df07dbcf17053e29c818
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/28/2018
ms.locfileid: "53814012"
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Azure Storage 内のデータ格納とアクセス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)

_Azure Storage とは、非構造化、および構造化データの格納に使用できるスケーラブルなクラウド ストレージ ソリューションです。この記事では、Xamarin.Forms を使用して Azure Storage にテキストおよびバイナリ データを格納する方法と、データにアクセスする方法を示します。_

Azure Storage では、4 つのストレージ サービスを提供します。

- Blob Storage。 Blob は、バックアップ、仮想マシン、メディア ファイルやドキュメントなどのテキストまたはバイナリのデータを指定できます。
- Table Storage は、NoSQL キー属性ストアです。
- Queue Storage とは、ワークフロー処理およびクラウド サービス間通信のためのメッセージング サービスです。
- File Storage は SMB プロトコルを使用して共有記憶域を提供します。

2 つの種類のストレージ アカウントがあります。

- 汎用ストレージ アカウントは、1 つのアカウントから Azure Storage サービスにアクセスを提供します。
- Blob ストレージ アカウントは、blob を格納するための特殊なストレージ アカウントです。 のみ blob のデータを格納する必要がある場合は、このアカウントの種類を使用することをお勧めします。

この記事と付属のサンプル アプリケーションでは、blob storage、およびそれらをダウンロードするイメージとテキスト ファイルのアップロードを示します。 さらに、これは、blob storage からファイルの一覧を取得して、ファイルの削除にも示します。

Azure Storage の詳細については、[Storage の概要](https://azure.microsoft.com/documentation/articles/storage-introduction/)を参照してください。

## <a name="introduction-to-blob-storage"></a>Blob Storage の概要

Blob ストレージは、次の図に示されている 3 つのコンポーネントで構成されます。

![](azure-storage-images/blob-storage.png "Blob Storage の概念")

Azure Storage にアクセスするでは、ストレージ アカウントを使用します。 ストレージ アカウントは、コンテナーの無制限の数を含めることができ、コンテナーがストレージ アカウントの容量の上限に達するまで、blob の無制限の数を格納します。

Blob は、任意の種類とサイズのファイルです。 Azure Storage には、次の 3 つの異なる blob の種類がサポートされています。

- ブロック blob はストリーミングとクラウド オブジェクトの格納用に最適化された、バックアップ、メディア ファイル、ドキュメントなどを格納するための適切な選択です。ブロック blob は最大 195 Gb のサイズを指定できます。
- 追加 blob はブロック blob に似ていますが、用に最適化されたログ記録などの操作を追加します。 追加 blob は最大 195 Gb のサイズを指定できます。
- ページ blob は、頻繁に読み取り/書き込み操作用に最適化され、仮想マシン、およびそれらのディスクを格納するために通常使用されます。 ページ blob は最大 1 Tb のサイズを指定できます。

> [!NOTE]
> Blob ストレージ アカウントがブロックのサポートし、追加 blob、ページ blob ではなくことに注意してください。

Blob は、Azure Storage にアップロードし、バイトのストリームとして、Azure Storage からダウンロードします。 そのため、ファイルをアップロードおよびダウンロード後に、元の形式に変換されたバックする前にバイトのストリームに変換する必要があります。

Azure Storage に格納されているすべてのオブジェクトには、一意の URL アドレスがあります。 ストレージ アカウント名には、そのアドレスでは、およびサブドメインとドメイン名の形式の組み合わせのサブドメインを*エンドポイント*のストレージ アカウント。 たとえば、ストレージ アカウントの名前は*mystorageaccount*、ストレージ アカウントの既定の blob エンドポイントは`https://mystorageaccount.blob.core.windows.net`します。

ストレージ アカウント内のオブジェクトにアクセスするための URL は、ストレージ アカウント内のオブジェクトの場所をエンドポイントに追加することによって構築されます。 たとえば、blob のアドレスは形式である`https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`します。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションに Azure Storage アカウントを統合するためのプロセスは次のとおりです。

1. ストレージ アカウントを作成します。 詳細については、[ストレージ アカウントの作成](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)を参照してください。
1. 追加、 [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage/) Xamarin.Forms アプリケーションにします。
1. ストレージ接続文字列を構成します。 詳細については、[Azure Storage に接続する](#connecting)を参照してください。
1. 追加`using`ディレクティブを`Microsoft.WindowsAzure.Storage`と`Microsoft.WindowsAzure.Storage.Blob`名前空間を Azure Storage にアクセスするクラス。

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Azure Storage への接続

ストレージ アカウントのリソースに対して行われたすべての要求を認証する必要があります。 Blob は、匿名認証をサポートするように構成できるは、アプリケーションがストレージ アカウントに対する認証を使用できます 2 つの主な方法があります。

- 共有キー。 この方法は、記憶域サービスのアクセスを Azure Storage アカウント名とアカウント キーを使用します。 ストレージ アカウントには、共有キー認証を使用できる作成時に 2 つの秘密キーが割り当てられます。
- 共有アクセス署名です。 これは、ストレージ リソースへの委任アクセスできるようにする URL に追加できるトークン、アクセス許可を持つことを指定します、有効期間。

アプリケーションから Azure Storage リソースへのアクセスに必要な認証情報が含まれる接続文字列を指定できます。 さらに、Visual Studio から Azure ストレージ エミュレーターに接続する接続文字列を構成できます。

> [!NOTE]
> Azure Storage は、接続文字列で、HTTP および HTTPS をサポートしています。 ただし、HTTPS を使用することをお勧めします。

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure ストレージ エミュレーターに接続します。

Azure ストレージ エミュレーターでは、Azure blob、キュー、および table service を開発用にエミュレートするローカル環境を提供します。

次の接続文字列は、Azure ストレージ エミュレーターに接続するために使用する必要があります。

```csharp
UseDevelopmentStorage=true
```

Azure ストレージ エミュレーターの詳細については、[開発およびテスト用の Azure ストレージ エミュレーターを使用して](https://azure.microsoft.com/documentation/articles/storage-use-emulator/)を参照してください。

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>共有キーを使用して Azure Storage への接続

共有キーを使用して Azure Storage に接続するには、次の接続文字列の形式を使用してください。

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` ストレージ アカウントの名前で置き換える必要がありますと`myAccountKey`2 つのアカウント アクセス キーのいずれかで置き換える必要があります。

> [!NOTE]
> キー認証、アカウント名とアカウント キーは、共有を使用する場合は、ストレージ アカウントへの完全な読み取り/書き込みアクセスを提供するアプリケーションを使用するユーザーごとに分散されます。 したがって、テスト目的でのみ、共有キー認証を使用して、他のユーザーにキーを配布しません。

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Shared Access Signature を使用して Azure Storage への接続

SAS を使用して Azure Storage に接続するには、次の接続文字列の形式を使用してください。

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` blob エンドポイントの URL で置き換える必要がありますと`mySharedAccessSignature`SAS で置き換える必要があります。 SAS は、プロトコル、サービス エンドポイント、およびリソースへのアクセス資格情報を提供します。

> [!NOTE]
> 実稼働アプリケーションには、SAS 認証を使用することをお勧めします。 ただし、運用アプリケーションでは、SAS をアプリケーションにバンドルされているのではなく、バックエンド サービス、オンデマンドでから取得する必要があります。

Shared Access Signature の詳細については、[Shared Access Signature (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)を参照してください。

## <a name="creating-a-container"></a>コンテナーの作成

`GetContainer`コンテナーから blob を取得するか、コンテナーに blob を追加するに使用できる名前付きコンテナーへの参照を取得するメソッドを使用します。 次のコード例は、`GetContainer`メソッド。

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse`メソッドは、接続文字列を解析しを返します、`CloudStorageAccount`ストレージ アカウントを表すインスタンス。 A`CloudBlobClient`のコンテナーと blob の取得に使用されているインスタンスがによって作成された後、`CreateCloudBlobClient`メソッド。 `GetContainerReference`メソッドとして、指定されたコンテナーの取得、`CloudBlobContainer`インスタンス、呼び出し元のメソッドから返されます。 この例では、コンテナー名は、`ContainerType`列挙値、小文字の文字列に変換します。

> [!NOTE]
> コンテナー名は、小文字にする必要があり、文字または数字で開始する必要があります。 さらに、文字、数字、およびダッシュ文字のみ含めることができますが、3 から 63 文字で指定する必要があります。

`GetContainer`メソッドは次のように呼び出されます。

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer`インスタンスは、存在しない場合は、コンテナーを作成し、使用できます。

```csharp
await container.CreateIfNotExistsAsync();
```

既定では、新しく作成されたコンテナーはプライベートです。 つまり、コンテナーから blob を取得する、ストレージ アクセス キーを指定する必要があります。 パブリック コンテナー内の blob を作成する方法については、[コンテナーを作成する](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container)を参照してください。

## <a name="uploading-data-to-a-container"></a>コンテナーにデータをアップロードします。

`UploadFileAsync`メソッドは、blob storage にデータをバイトのストリームをアップロードするために使用し、次のコード例に示した。

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

コンテナーの参照を取得した後は、メソッドは、存在しない場合に、コンテナーを作成します。 新しい`Guid`として一意の blob 名では、機能を作成して、ブロック blob の参照として取得し、`CloudBlockBlob`インスタンス。 データのストリームを使用して blob をアップロードし、`UploadFromStreamAsync`メソッドで、存在する場合は上書きされますかが既に存在しない場合は、blob を作成します。

ファイルをアップロードすると、blob ストレージにこのメソッドを使用して、前に、バイト ストリームに最初に変換する必要があります。 これは、次のコード例について説明します。

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text`データに渡されるストリームとしてラップし、バイト配列に変換されます、`UploadFileAsync`メソッド。

## <a name="downloading-data-from-a-container"></a>コンテナーからデータをダウンロード

`GetFileAsync`メソッドは、Azure Storage から blob データをダウンロードするために使用し、次のコード例に示した。

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

コンテナーの参照を取得した後は、メソッドは、格納されたデータの blob の参照を取得します。 によって、そのプロパティを取得、blob が存在する場合、`FetchAttributesAsync`メソッド。 適切なサイズのバイト配列が作成され、呼び出し元のメソッドに返されるバイト配列として、blob がダウンロードされます。

Blob のバイトのデータをダウンロードした後、元の形式に変換する必要があります。 これは、次のコード例について説明します。

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

バイトの配列が、Azure Storage から取得した、`GetFileAsync`を UTF8 に変換する前に、メソッドでエンコードされた文字列。

## <a name="listing-data-in-a-container"></a>コンテナー内のデータを一覧表示します。

`GetFilesListAsync`メソッドは、コンテナーに格納された blob の一覧を取得するために使用し、次のコード例に示した。

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

メソッドは、コンテナーの参照を取得した後、コンテナーの使用`ListBlobsSegmentedAsync`コンテナー内の blob への参照を取得します。 によって返される結果、`ListBlobsSegmentedAsync`メソッドを列挙中に、`BlobContinuationToken`インスタンスがない`null`します。 各 blob がキャストから返された`IListBlobItem`を`CloudBlockBlob`アクセスの順序で、`Name`値が前に、blob のプロパティに追加されます、`allBlobsList`コレクション。 1 回、`BlobContinuationToken`インスタンスが`null`、最後の blob 名が返された、および実行がループを終了します。

## <a name="deleting-data-from-a-container"></a>コンテナーからデータを削除します。

`DeleteFileAsync`メソッドは、コンテナーから blob を削除するために使用し、次のコード例に示した。

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

コンテナーの参照を取得した後は、メソッドは、指定された blob の blob の参照を取得します。 Blob は削除し、`DeleteIfExistsAsync`メソッド。

## <a name="related-links"></a>関連リンク

- [Azure Storage (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Storage の概要](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Xamarin から Blob Storage を使用する方法](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Shared Access Signature (SAS) を使用します。](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure ストレージ (NuGet)](https://www.nuget.org/packages/WindowsAzure.Storage/)
