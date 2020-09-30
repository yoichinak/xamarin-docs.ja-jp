---
title: Azure Storage のデータの格納とアクセス Xamarin.Forms
description: Azure Storage は、構造化されていない構造化データを格納するために使用できるスケーラブルなクラウドストレージソリューションです。 この記事では、を使用して Xamarin.Forms Azure Storage にテキストとバイナリデータを格納する方法と、データにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/28/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cba4c670e9e092eef92f7b37eefc750782c94367
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563836"
---
# <a name="store-and-access-data-in-azure-storage-from-no-locxamarinforms"></a>Azure Storage のデータの格納とアクセス Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurestorage)

_Azure Storage は、構造化されていない構造化データを格納するために使用できるスケーラブルなクラウドストレージソリューションです。この記事では、を使用して Xamarin.Forms Azure Storage にテキストとバイナリデータを格納する方法と、データにアクセスする方法について説明します。_

Azure Storage には、次の4つのストレージサービスがあります。

- Blob Storage。 Blob には、テキストまたはバイナリデータ (バックアップ、仮想マシン、メディアファイル、ドキュメントなど) を指定できます。
- Table Storage は NoSQL のキー属性ストアです。
- Queue Storage は、ワークフローの処理とクラウドサービス間の通信を行うためのメッセージングサービスです。
- File Storage は、SMB プロトコルを使用して共有記憶域を提供します。

ストレージ アカウントには、次の 2 種類があります。

- 汎用ストレージアカウントを使用すると、1つのアカウントから Azure Storage サービスにアクセスできます。
- Blob ストレージアカウントは、blob を格納するための特殊なストレージアカウントです。 このアカウントの種類は、blob データの格納のみが必要な場合に推奨されます。

この記事と付随するサンプルアプリケーションでは、イメージとテキストファイルを blob storage にアップロードしてダウンロードする方法を示します。 また、blob storage からファイルの一覧を取得したり、ファイルを削除したりする方法も示しています。

Azure Storage の詳細については、「 [ストレージの概要](https://azure.microsoft.com/documentation/articles/storage-introduction/)」を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

## <a name="introduction-to-blob-storage"></a>Blob Storage の概要

Blob ストレージは、次の図に示す3つのコンポーネントで構成されています。

![Blob Storage の概念](azure-storage-images/blob-storage.png)

Azure Storage へのすべてのアクセスは、ストレージアカウントを介して行われます。 ストレージアカウントには、無制限の数のコンテナーを含めることができ、コンテナーに格納できる blob の数に制限はありません。これは、ストレージアカウントの容量の上限までです。

Blob は、任意の種類とサイズのファイルです。 Azure Storage では、次の3種類の blob をサポートしています。

- ブロック blob は、クラウドオブジェクトをストリーミングおよび格納するために最適化されており、バックアップ、メディアファイル、ドキュメントなどを格納するための最適な選択肢です。ブロック blob のサイズは最大で 195Gb Gb です。
- 追加 blob はブロック blob に似ていますが、ログ記録などの追加操作用に最適化されています。 追加 blob のサイズは最大で 195Gb Gb です。
- ページ blob は、頻繁な読み取り/書き込み操作用に最適化されており、通常は仮想マシンとそのディスクを格納するために使用されます。 ページ blob の最大サイズは 1 Tb です。

> [!NOTE]
> Blob ストレージアカウントでは、ブロック blob と追加 blob がサポートされますが、ページ blob はサポートされないことに注意してください。

Blob は Azure Storage にアップロードされ、Azure Storage からダウンロードされ、バイトのストリームとしてダウンロードされます。 そのため、ファイルをアップロードする前にバイトのストリームに変換し、ダウンロード後に元の表現に変換し直す必要があります。

Azure Storage に格納されているすべてのオブジェクトには、一意の URL アドレスがあります。 ストレージアカウント名はそのアドレスのサブドメインを形成し、サブドメインとドメイン名の組み合わせによってストレージアカウントの *エンドポイント* が形成されます。 たとえば、ストレージアカウントに *mystorageaccount*という名前が付けられている場合、ストレージアカウントの既定の blob エンドポイントはに `https://mystorageaccount.blob.core.windows.net` なります。

ストレージ アカウント内のオブジェクトにアクセスするための URL は、ストレージ アカウント内のオブジェクトの場所をエンドポイントに追加して作成します。 たとえば、blob アドレスの形式は `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob` です。

## <a name="setup"></a>セットアップ

Azure Storage アカウントをアプリケーションに統合するプロセスは次のとおり Xamarin.Forms です。

1. ストレージ アカウントを作成します。 詳しくは、「[ストレージ アカウントの作成](/azure/storage/common/storage-account-create#create-a-storage-account)」をご覧ください。
1. [Azure Storage クライアントライブラリ](https://www.nuget.org/packages/WindowsAzure.Storage/)をアプリケーションに追加し Xamarin.Forms ます。
1. ストレージ接続文字列を構成します。 詳細については、「 [Azure Storage への接続](#connecting-to-azure-storage)」を参照してください。
1. `using` `Microsoft.WindowsAzure.Storage` `Microsoft.WindowsAzure.Storage.Blob` 名前空間と名前空間のディレクティブを Azure Storage にアクセスするクラスに追加します。

## <a name="connecting-to-azure-storage"></a>Azure Storage に接続する

ストレージアカウントリソースに対して行われたすべての要求は、認証される必要があります。 Blob は匿名認証をサポートするように構成できますが、アプリケーションがストレージアカウントでの認証に使用できる主な方法は2つあります。

- 共有キー。 このアプローチでは、Azure Storage アカウント名とアカウントキーを使用してストレージサービスにアクセスします。 ストレージアカウントには、共有キー認証に使用できる2つの秘密キーが作成時に割り当てられます。
- Shared Access Signature。 これは、有効である期間のストレージリソースへの委任アクセスを可能にする URL に追加できるトークンです。これにより、指定したアクセス許可が付与されます。

アプリケーションから Azure Storage リソースにアクセスするために必要な認証情報を含む接続文字列を指定できます。 さらに、Visual Studio から Azure ストレージエミュレーターに接続するように接続文字列を構成することもできます。

> [!NOTE]
> Azure Storage は、接続文字列で HTTP と HTTPS をサポートしています。 ただし、HTTPS を使用することをお勧めします。

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure Storage エミュレーターへの接続

Azure ストレージエミュレーターは、開発目的で Azure blob、queue、および table service をエミュレートするローカル環境を提供します。

Azure ストレージエミュレーターに接続するには、次の接続文字列を使用する必要があります。

```csharp
UseDevelopmentStorage=true
```

Azure ストレージエミュレーターの詳細については、「 [開発とテストのための azure ストレージエミュレーターの使用](/azure/storage/common/storage-use-emulator)」を参照してください。

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>共有キーを使用した Azure Storage への接続

共有キーを使用して Azure Storage に接続するには、次の接続文字列の形式を使用する必要があります。

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` は、ストレージアカウントの名前に置き換える必要があり `myAccountKey` ます。また、2つのアカウントアクセスキーのいずれかに置き換える必要があります。

> [!NOTE]
> 共有キー認証を使用する場合、アカウント名とアカウントキーは、アプリケーションを使用する各ユーザーに配布されます。これにより、ストレージアカウントに対する完全な読み取り/書き込みアクセスが提供されます。 そのため、テスト目的でのみ共有キー認証を使用し、他のユーザーにはキーを配布しないようにします。

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Shared Access Signature を使用した Azure Storage への接続

SAS を使用して Azure Storage に接続するには、次の接続文字列の形式を使用する必要があります。

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` は blob エンドポイントの URL に置き換える必要があり、 `mySharedAccessSignature` SAS に置き換える必要があります。 SAS は、プロトコル、サービスエンドポイント、およびリソースにアクセスするための資格情報を提供します。

> [!NOTE]
> 実稼働アプリケーションでは、SAS 認証をお勧めします。 ただし、実稼働アプリケーションでは、SAS はアプリケーションにバンドルされるのではなく、オンデマンドでバックエンドサービスから取得する必要があります。

Shared Access Signature の詳細については、「 [Shared Access signature (SAS) の使用](/azure/storage/common/storage-sas-overview)」を参照してください。

## <a name="creating-a-container"></a>コンテナーの作成

`GetContainer`メソッドは、名前付きコンテナーへの参照を取得するために使用されます。これは、コンテナーから blob を取得したり、コンテナーに blob を追加したりするために使用できます。 次のコード例は、`GetContainer` メソッドを示しています。

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

メソッドは、 `CloudStorageAccount.Parse` 接続文字列を解析し、 `CloudStorageAccount` ストレージアカウントを表すインスタンスを返します。 `CloudBlobClient`コンテナーと blob を取得するために使用されるインスタンスは、メソッドによって作成され `CreateCloudBlobClient` ます。 メソッドは、 `GetContainerReference` `CloudBlobContainer` 呼び出し元のメソッドに返される前に、指定されたコンテナーをインスタンスとして取得します。 この例では、コンテナー名は、 `ContainerType` 小文字の文字列に変換された列挙値です。

> [!NOTE]
> コンテナー名は小文字にする必要があり、先頭には文字または数字を使用する必要があります。 また、文字、数字、およびダッシュ文字のみを含めることができ、長さは 3 ~ 63 文字にする必要があります。

`GetContainer`メソッドは次のように呼び出されます。

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer`このインスタンスは、コンテナーがまだ存在しない場合に作成するために使用できます。

```csharp
await container.CreateIfNotExistsAsync();
```

既定では、新しく作成されたコンテナーはプライベートです。 これは、コンテナーから blob を取得するために、ストレージアクセスキーを指定する必要があることを意味します。 コンテナー内の blob をパブリックにする方法の詳細については、「 [コンテナーを作成する](/azure/storage/blobs/storage-quickstart-blobs-dotnet#create-a-container)」を参照してください。

## <a name="uploading-data-to-a-container"></a>コンテナーへのデータのアップロード

メソッドは、 `UploadFileAsync` バイトデータのストリームを blob ストレージにアップロードするために使用され、次のコード例に示します。

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

コンテナー参照を取得した後、コンテナーがまだ存在しない場合は、このメソッドによって作成されます。 `Guid`次に、新しいが作成され、一意の blob 名として機能し、blob ブロック参照がインスタンスとして取得され `CloudBlockBlob` ます。 次に、メソッドを使用してデータのストリームを blob にアップロードします。 blob が `UploadFromStreamAsync` まだ存在しない場合は作成し、存在する場合は上書きします。

このメソッドを使用してファイルを blob storage にアップロードする前に、まずバイトストリームに変換する必要があります。 これを次のコード例に示します。

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text`データはバイト配列に変換され、その後、メソッドに渡されるストリームとしてラップされ `UploadFileAsync` ます。

## <a name="downloading-data-from-a-container"></a>コンテナーからのデータのダウンロード

`GetFileAsync`メソッドは Azure Storage から blob データをダウンロードするために使用され、次のコード例に示します。

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

コンテナー参照を取得した後、メソッドは格納されているデータの blob 参照を取得します。 Blob が存在する場合、そのプロパティはメソッドによって取得され `FetchAttributesAsync` ます。 正しいサイズのバイト配列が作成され、その blob は、呼び出し元のメソッドに返されるバイトの配列としてダウンロードされます。

Blob バイトデータをダウンロードした後は、元の表現に変換する必要があります。 これを次のコード例に示します。

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

バイト配列は、UTF8 でエンコードされ `GetFileAsync` た文字列に変換される前に、メソッドによって Azure Storage から取得されます。

## <a name="listing-data-in-a-container"></a>コンテナー内のデータを一覧表示する

メソッドは、 `GetFilesListAsync` コンテナーに格納されている blob の一覧を取得するために使用され、次のコード例に示します。

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

コンテナー参照を取得した後、メソッドはコンテナーのメソッドを使用して `ListBlobsSegmentedAsync` 、コンテナー内の blob への参照を取得します。 インスタンスがではない場合、メソッドによって返される結果は `ListBlobsSegmentedAsync` 列挙され `BlobContinuationToken` `null` ます。 各 blob は、 `IListBlobItem` `CloudBlockBlob` `Name` 値がコレクションに追加される前に、blob のプロパティにアクセスするために、返されたからにキャストされ `allBlobsList` ます。 `BlobContinuationToken`インスタンスがになると `null` 、最後の blob 名が返され、実行がループを終了します。

## <a name="deleting-data-from-a-container"></a>コンテナーからのデータの削除

`DeleteFileAsync`コンテナーから blob を削除するには、次のコード例に示すように、メソッドを使用します。

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

コンテナー参照を取得した後、メソッドは、指定された blob の blob 参照を取得します。 次に、メソッドを使用して blob を削除し `DeleteIfExistsAsync` ます。

## <a name="related-links"></a>関連リンク

- [Azure Storage (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-azurestorage)
- [ストレージの概要](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Xamarin から BLOB ストレージを使用する方法](/azure/storage/blobs/storage-quickstart-blobs-xamarin)
- [Shared Access Signatures (SAS) の使用](/azure/storage/common/storage-sas-overview)
- [Windows Azure Storage (NuGet)](https://www.nuget.org/packages/WindowsAzure.Storage/)