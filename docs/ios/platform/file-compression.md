---
title: Xamarin.iOS におけるファイル圧縮
description: このドキュメントでは、Xamarin.iOS で libcompression API を操作する方法について説明します。 縮小された、膨張させること、について説明し、さまざまなアルゴリズムをサポートします。
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: f7a1df65047fd8040dd40e9f7f057d6bfe6dea61
ms.sourcegitcommit: 4c97f5d73be7eb2da153a85183be4258b6b11ca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2019
ms.locfileid: "58290927"
---
# <a name="file-compression-in-xamarinios"></a>Xamarin.iOS におけるファイル圧縮

IOS 9.0 または macOS 10.11 (以降) を対象とする Xamarin アプリを使用できます、_圧縮 Framework_を圧縮する (エンコード) を圧縮解除 (デコード) データ。 Xamarin.iOS では、次の Stream API このフレームワークを提供します。 圧縮フレームワークでは、により、開発者は、圧縮と対話して、標準ストリームのコールバックとデリゲートを使用する必要はありませんが、データの圧縮を解除します。

圧縮フレームワークでは、次のアルゴリズムのサポートを提供します。

* LZ4
* 生の LZ4
* Lzfse
* Lzma
* Zlib

圧縮のフレームワークを使用すると、サードパーティ製のライブラリまたは Nuget なしの圧縮操作を実行する開発者ができます。 これにより、外部の依存関係が削減され、によりことと、(ただし、OS の最小要件を満たす場合)、すべてのプラットフォームで、圧縮操作をサポートします。

## <a name="general-file-decompression"></a>一般的なファイルの圧縮解除

圧縮フレームワークでは、Xamarin.iOS および Xamarin.Mac でストリーム API を使用します。 この API は、あるデータを圧縮する開発者は、使用 .NET 内の他の IO Api で使用される通常のパターンを意味します。 次の例については、API に似ていますが、圧縮フレームワークでのデータを圧縮解除する方法を示しています、 `System.IO.Compression.DeflateStream` API:

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

`CompressionStream`実装、`IDisposable`などその他のインターフェイス`System.IO.Streams`ので、開発者が必要でなくなったとリソースが解放されることを確認してください。

## <a name="general-file-compression"></a>一般的なファイルの圧縮

圧縮 API では、次の API は、同じデータを圧縮することもできます。 提供されているアルゴリズムのいずれかを使用してデータを圧縮することができます、`CompressionAlgorithm`列挙子。

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

`CompressionStream`でサポートされているすべての非同期操作をサポートしている、 `System.IO.DeflateStream`、つまり、開発者が UI スレッドをブロックすることがなく、圧縮/圧縮解除の操作を実行する async キーワードを使用できます。