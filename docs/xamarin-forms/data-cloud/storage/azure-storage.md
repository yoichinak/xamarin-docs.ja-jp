---
title: 格納して、Azure ストレージ内のデータにアクセスします。
description: Azure ストレージは、非構造化、および構造化データを格納するために使用するスケーラブルなクラウド記憶域ソリューションです。 この記事では、Xamarin.Forms を使用して、Azure ストレージ内のテキストおよびバイナリ データを格納する方法と、データにアクセスする方法を示します。
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 63afeec81eff350b034e8dd3a13da52801937826
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>格納して、Azure ストレージ内のデータにアクセスします。

_Azure ストレージは、非構造化、および構造化データを格納するために使用するスケーラブルなクラウド記憶域ソリューションです。この記事では、Xamarin.Forms を使用して、Azure ストレージ内のテキストおよびバイナリ データを格納する方法と、データにアクセスする方法を示します。_

## <a name="overview"></a>概要

Azure ストレージでは、次の 4 つの記憶域サービスを提供します。

- Blob ストレージです。 Blob には、バックアップ、仮想マシン、メディア ファイルやドキュメントなどのテキストまたはバイナリ データを指定できます。
- テーブル ストレージは、NoSQL キー属性ストアです。
- キュー ストレージは、ワークフローの処理およびクラウド サービス間の通信のメッセージング サービスです。
- ファイル記憶域は、SMB プロトコルを使用して共有記憶域を提供します。

ストレージ アカウントの 2 つの種類があります。

- 汎用的なストレージ アカウントは、1 つのアカウントから Azure ストレージ サービスへのアクセスを提供します。
- Blob ストレージ アカウントとは、blob を格納するための特殊なストレージ アカウントです。 のみ blob のデータを格納する必要がある場合は、このアカウントの種類を使用することをお勧めします。

この記事と付属のサンプル アプリケーションは、blob ストレージ、およびそれらをダウンロードする画像およびテキスト ファイルのアップロードを示します。 さらに、これは、blob ストレージからファイルの一覧を取得し、ファイルを削除するにも示します。

Azure Storage の詳細については、次を参照してください。[ストレージの概要](https://azure.microsoft.com/documentation/articles/storage-introduction/)です。

## <a name="introduction-to-blob-storage"></a>Blob ストレージの概要

Blob ストレージは、次の図に示されている 3 つのコンポーネントで構成されます。

![](azure-storage-images/blob-storage.png "Blob ストレージの概念")

Azure ストレージに対するすべてのアクセスでは、ストレージ アカウントを使用します。 ストレージ アカウントは、コンテナーの無制限の数を含めることができ、コンテナーがストレージ アカウントの容量の上限に達するまで、blob の数は無制限に格納できます。

Blob は、任意の型とサイズのファイルです。 Azure ストレージには、次の 3 つの別の blob の種類がサポートされます。

- ブロック blob はストリーミングとクラウド オブジェクトを格納するために最適化された、バックアップ、メディア ファイル、ドキュメントなどを格納するための適切な選択です。ブロック blob には 195 Gb までのサイズを指定できます。
- 追加 blob はブロック blob に似ていますが、用に最適化されたログ記録などの操作を追加します。 追加 blob は 195 Gb までのサイズを指定できます。
- ページ blob では、頻繁に行われる読み取り/書き込み操作用に最適化され、は、通常、仮想マシンとそれにディスクを格納するために使用します。 ページ blob には、最大 1 Tb のサイズを指定できます。

> [!NOTE]
> Blob ストレージ アカウントはブロックをサポートして、追加 blob はページ blob ではなくことに注意してください。

Blob は Azure Storage にアップロードされ、バイトのストリームとしての Azure ストレージからダウンロードします。 そのため、ファイルをアップロードおよびダウンロード後に、元の形式に変換後のバックアップより前のバイトのストリームに変換する必要があります。

Azure ストレージに格納されているすべてのオブジェクトは、一意の URL アドレスを含んでいます。 ストレージ アカウント名は、そのアドレスとサブドメインとドメイン名の形式の組み合わせのサブドメインを形成、*エンドポイント*ストレージ アカウント。 たとえば、ストレージ アカウントの名前は*mystorageaccount*、ストレージ アカウントの既定の blob エンドポイント`https://mystorageaccount.blob.core.windows.net`です。

ストレージ アカウント内のオブジェクトにアクセスするための URL は、エンドポイントへのストレージ アカウント内のオブジェクトの場所を追加することによって作成されています。 たとえば、blob アドレスの形式がある`https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`です。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションに、Azure ストレージ アカウントを統合するためのプロセスは次のとおりです。

1. ストレージ アカウントを作成します。 詳細については、次を参照してください。[ストレージ アカウントの作成](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)です。
1. 追加、 [Azure ストレージ クライアント ライブラリ](https://www.nuget.org/packages/WindowsAzure.Storage/)Xamarin.Forms アプリケーションにします。
1. ストレージ接続文字列を構成します。 詳細については、次を参照してください。 [Azure ストレージに接続する](#connecting)です。
1. 追加`using`のディレクティブを`Microsoft.WindowsAzure.Storage`と`Microsoft.WindowsAzure.Storage.Blob`名前空間の Azure ストレージにアクセスするクラス。

> [!NOTE]
> このサンプルでは、共有アクセス プロジェクトでは、Azure ストレージ クライアント ライブラリ今すぐもサポート ポータブル クラス ライブラリ (PCL) プロジェクトから使用します。

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Azure ストレージへの接続

ストレージ アカウント リソースに対して行われたすべての要求を認証する必要があります。 Blob は、匿名認証をサポートするために構成できますは、アプリケーションは、ストレージ アカウントを認証に使用できる 2 つの主な方法があります。

- 共有キー。 このアプローチは、記憶域サービスへのアクセスの Azure ストレージ アカウント名とアカウント キーを使用します。 ストレージ アカウントには、共有キー認証を使用できる作成時に 2 つの秘密キーが割り当てられます。
- 共有アクセス署名です。 これはトークンを記憶域リソースへの委任されたアクセスを有効にする URL に追加することができますが、アクセス許可を持つことを指定します、有効である期間。

接続文字列を指定するには、アプリケーションから Azure のストレージ リソースにアクセスするために必要な認証情報が含まれています。 さらに、Visual Studio から Azure ストレージ エミュレーターに接続する接続文字列を構成することができます。

> [!NOTE]
> Azure ストレージは、接続文字列で、HTTP および HTTPS をサポートします。 ただし、HTTPS を使用することをお勧めします。

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure ストレージ エミュレーターへの接続

Azure ストレージ エミュレーターは、Azure blob、キュー、および開発の目的のテーブル サービスをエミュレートするローカル環境を提供します。

次の接続文字列は、Azure ストレージ エミュレーターへの接続に使用する必要があります。

```csharp
UseDevelopmentStorage=true
```

Azure のストレージ エミュレーターの詳細については、次を参照してください。 [Azure のストレージ エミュレーターを使用して開発とテスト](https://azure.microsoft.com/documentation/articles/storage-use-emulator/)です。

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>共有キーを使用して Azure ストレージへの接続

共有キーを持つ Azure ストレージに接続するには、次の接続文字列の形式を使用してください。

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` ストレージ アカウントの名前に置き換える必要がありますと`myAccountKey`2 つのアカウント アクセス キーのいずれかで置き換える必要があります。

> [!NOTE]
> キー認証、アカウント名とアカウント キーは、共有を使用する場合は、ストレージ アカウントへの完全な読み取り/書き込みアクセスを提供するアプリケーションを使用する各ユーザーに配布されます。 そのため、テスト目的でのみ、共有キー認証を使用し、決してを他のユーザー キーを配布します。

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>共有アクセス署名を使用して Azure ストレージへの接続

Azure ストレージで、SAS に接続するには、次の接続文字列の形式を使用してください。

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` blob エンドポイントの URL を置き換える必要がありますと`mySharedAccessSignature`SAS に置き換える必要があります。 SAS は、プロトコル、サービス エンドポイント、およびリソースへのアクセス資格情報を提供します。

> [!NOTE]
> 実稼働アプリケーションには、SAS 認証を使用することをお勧めします。 ただし、実稼働アプリケーションでは、SAS をアプリケーションにバンドルされているのではなく、バックエンド サービス、オンデマンドでから取得する必要があります。

共有アクセス署名の詳細については、次を参照してください。[を使用して共有アクセス署名 (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)です。

## <a name="creating-a-container"></a>コンテナーを作成します。

`GetContainer`コンテナーから blob を取得するかをコンテナーに blob を追加し、使用できる名前付きコンテナーへの参照を取得するメソッドを使用します。 次のコード例は、`GetContainer`メソッド。

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse`メソッドは、接続文字列を解析しを返します、`CloudStorageAccount`をストレージ アカウントを表すインスタンス。 A`CloudBlobClient`で使用する場合は、コンテナーと blob を取得する、インスタンスが作成されます、`CreateCloudBlobClient`メソッドです。 `GetContainerReference`メソッドとして、指定されたコンテナーの取得、`CloudBlobContainer`インスタンス、呼び出し元のメソッドに返される前にします。 この例ではコンテナー名は、`ContainerType`列挙値、小文字の文字列に変換します。

> [!NOTE]
> コンテナー名は小文字である必要があり、アルファベットまたは数字で始める必要があります。 さらに、文字、数字、およびダッシュ文字のみを含めることができます、3 ~ 63 文字の間である必要があります。

`GetContainer`メソッドが次のように呼び出されます。

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer`インスタンスが存在しない場合、コンテナーの作成に使用できます。

```csharp
await container.CreateIfNotExistsAsync();
```

既定では、新しく作成されたコンテナーはプライベートです。 つまり、コンテナーから blob を取得するストレージ アクセス キーを指定する必要があります。 パブリック コンテナー内の blob を行う方法については、次を参照してください。[コンテナーを作成する](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container)です。

## <a name="uploading-data-to-a-container"></a>コンテナーへのデータのアップロード

`UploadFileAsync`メソッドは、バイトへのデータ blob ストレージ、ストリームをアップロードするために使用し、次のコード例に示します。

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

コンテナーの参照を取得後に、メソッドは、存在しない場合にコンテナーを作成します。 新しい`Guid`として一意の blob の名前では、機能を作成して、として blob のブロック参照を取得し、`CloudBlockBlob`インスタンス。 データのストリームを使用して blob にアップロードし、`UploadFromStreamAsync`メソッドで、存在する場合は、それを上書きまたはが既に存在しない場合、blob を作成します。

ファイルは、このメソッドを使用して、ストレージの blob にアップロードすることができます、前に、バイト ストリームに最初に変換する必要があります。 これを次のコード例に示します。

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text`データに渡されるストリームとしてラップしをバイト配列に変換されます、`UploadFileAsync`メソッドです。

## <a name="downloading-data-from-a-container"></a>コンテナーからデータをダウンロードします。

`GetFileAsync`メソッドから Azure Storage の blob データのダウンロードに使用され、次のコード例に示します。

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

コンテナーの参照を取得した後は、メソッドは、格納されたデータの blob 参照を取得します。 そのプロパティが取得された blob が存在する場合、`FetchAttributesAsync`メソッドです。 適切なサイズのバイト配列が作成され、呼び出し元のメソッドに返されるを取得するバイト配列として blob をダウンロードします。

Blob のバイトのデータをダウンロードした後、元の形式に変換する必要があります。 これを次のコード例に示します。

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Azure ストレージからバイトの配列を取得、 `GetFileAsync` UTF8 に変換する前に、メソッドにエンコードされた文字列。

## <a name="listing-data-in-a-container"></a>コンテナー内のデータを一覧表示します。

`GetFilesListAsync`メソッドは、コンテナーに格納された blob の一覧を取得するために使用し、次のコード例に示します。

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

コンテナーの参照を取得後に、メソッドは、コンテナーを使用`ListBlobsSegmentedAsync`コンテナー内の blob への参照を取得します。 によって返される結果、`ListBlobsSegmentedAsync`メソッドを列挙中に、`BlobContinuationToken`インスタンスではありません`null`です。 各 blob はキャストから返された`IListBlobItem`を`CloudBlockBlob`順序アクセスで、`Name`に値が前に、blob のプロパティが追加、`allBlobsList`コレクション。 1 回、`BlobContinuationToken`インスタンスが`null`、最後の blob 名が返された、および実行は、ループを終了します。

## <a name="deleting-data-from-a-container"></a>コンテナーからデータを削除します。

`DeleteFileAsync`メソッドをコンテナーから blob を削除するために使用し、次のコード例に示します。

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

コンテナーの参照を取得した後は、メソッドは、指定された blob の blob 参照を取得します。 Blob が一緒に削除し、`DeleteIfExistsAsync`メソッドです。

## <a name="summary"></a>まとめ

この記事には、Xamarin.Forms を使用して、Azure ストレージ内のテキストおよびバイナリ データを格納する方法と、データにアクセスする方法が示されています。 Azure ストレージは、非構造化、および構造化データの格納に使用できるスケーラブルなクラウド記憶域ソリューションです。


## <a name="related-links"></a>関連リンク

- [Azure ストレージ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [記憶域の概要](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Xamarin から Blob ストレージを使用する方法](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [共有アクセス署名 (SAS) を使用します。](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure ストレージ](https://www.nuget.org/packages/WindowsAzure.Storage/)
