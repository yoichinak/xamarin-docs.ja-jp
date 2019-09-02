---
title: Xamarin. iOS のファイル圧縮
description: このドキュメントでは、Xamarin. iOS で libcompression API を使用する方法について説明します。 Deflating、拡大、およびサポートされているさまざまなアルゴリズムについて説明します。
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: bcc63aa4e1926f5502d571bf47c83b0c8ea7e429
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199525"
---
# <a name="file-compression-in-xamarinios"></a>Xamarin. iOS のファイル圧縮

IOS 9.0 または macOS 10.11 (およびそれ以降) を対象とする Xamarin アプリでは、_圧縮フレームワーク_を使用してデータを圧縮 (エンコード) および圧縮解除 (デコード) できます。 Xamarin. iOS は、Stream API に従ってこのフレームワークを提供します。 圧縮フレームワークを使用すると、開発者は、データの圧縮と圧縮解除を、コールバックやデリゲートを使用しなくても、通常のストリームと同様に行うことができます。

圧縮フレームワークでは、次のアルゴリズムがサポートされています。

* LZ4
* LZ4 Raw
* Lzfse
* Lzma
* Zlib

圧縮フレームワークを使用すると、開発者はサードパーティのライブラリや Nuget を使用せずに圧縮操作を実行できます。 これにより、外部の依存関係が減少し、すべてのプラットフォーム (OS の最小要件を満たしている限り) で圧縮操作がサポートされるようになります。

## <a name="general-file-decompression"></a>一般的なファイルの圧縮解除

圧縮フレームワークは、Xamarin. iOS と Xamarin. Mac で stream API を使用します。 この API は、データを圧縮するために、開発者が .NET 内の他の IO Api で使用される通常のパターンを使用できることを意味します。 次のサンプルは、圧縮フレームワークを使用してデータを圧縮解除する方法を示しています`System.IO.Compression.DeflateStream` 。これは、api に含まれる api に似ています。

```csharp
// sample zlib data
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompression stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

は`CompressionStream` `IDisposable`インターフェイスを`System.IO.Streams`実装しているため、開発者は不要になったリソースを解放する必要があります。

## <a name="general-file-compression"></a>一般的なファイルの圧縮

また、圧縮 API を使用すると、開発者は同じ API に従ってデータを圧縮することもできます。 `CompressionAlgorithm`列挙子に示されているアルゴリズムのいずれかを使用してデータを圧縮できます。

```csharp
// sample method that copies the data from the source stream to the destination stream
static void CopyStream (Stream src, Stream dest)
{
    byte[] array = new byte[1024];
    int bytes_read;
    bytes_read = src.Read (array, 0, 1024);
    while (bytes_read != 0) {
        dest.Write (array, 0, bytes_read);
        bytes_read = src.Read (array, 0, 1024);
    }
}

static void CompressExample ()
{
    // create some sample data to compress
    byte [] data = new byte[100000];
    for (int i = 0; i < 100000; i++) {
        data[i] = (byte) i;
    }

    using (var dataStream = new MemoryStream (data)) // stream that contains the data to compress
    using (var backing = new MemoryStream ()) // stream in which the compress data will be written
    using (var compressing = new CompressionStream (backing, CompressionMode.Compress, CompressionAlgorithm.Zlib, true))
    {
        // copy the data to the compressing stream
        CopyStream (dataStream, compressing);
        // at this point, the CompressionStream contains all the data from the dataStream but compressed
        // using the zlib algorithm
    }
```

## <a name="async-support"></a>非同期サポート

は、`System.IO.DeflateStream`でサポートされているすべての非同期操作をサポートしています。つまり、開発者は、asyncキーワードを使用して、UIスレッドをブロックせずに圧縮/圧縮解除操作を`CompressionStream`実行できます。
