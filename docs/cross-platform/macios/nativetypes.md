---
title: IOS と macOS のネイティブ型
description: このドキュメントでは、Xamarin の Unified API が、コンパイルターゲットアーキテクチャに基づき、必要に応じて .NET 型を32ビットおよび64ビットのネイティブ型にマップする方法について説明します。
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: davidortinau
ms.author: daortin
ms.date: 01/25/2016
ms.openlocfilehash: 1168dfe0f2120e87c57b46011caba5e7a8a0c020
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015482"
---
# <a name="native-types-for-ios-and-macos"></a>IOS と macOS のネイティブ型

Mac および iOS API では、アーキテクチャ固有のデータ型を使用します。これは、32ビットプラットフォームでは常に32ビット、64ビットプラットフォームでは64ビットです。

たとえば、`NSInteger` データ型は、32ビットシステムでは `int32_t`、64ビットシステムでは `int64_t` にマップされます。

この動作に対応するために、統合 API では、以前の `int` (.NET では、常に `System.Int32`として定義されている) を新しいデータ型である `System.nint`に置き換えます。 "N" は "ネイティブ" の意味であると考えることができます。したがって、プラットフォームのネイティブ整数型です。

これらの新しいデータ型では、コンパイルフラグに応じて、同じソースコードが32ビットおよび64ビットアーキテクチャ用にコンパイルされます。

## <a name="new-data-types"></a>新しいデータ型

次の表は、この新しい 32/64 ビットの世界に一致するようにデータ型に加えられた変更を示しています。

|ネイティブ型|32ビットのバッキング型|64ビットのバッキング型|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

これらの名前を選択してC# 、現在のコードと同じようにコードを表示できるようにしました。

### <a name="implicit-and-explicit-conversions"></a>暗黙の型変換と明示的な型変換

新しいデータ型の設計は、ホストプラットフォームとコンパイルの設定に応じて、1つの C# ソースファイルで32または64ビットのストレージを自然に使用できるようにすることを目的としています。

これにより、プラットフォーム固有のデータ型と .NET の整数データ型および浮動小数点データ型との間で暗黙的および明示的な変換のセットを設計する必要がありました。

暗黙の変換演算子は、データ損失の可能性がない場合に提供されます (32 ビット値は64ビット空間に格納されています)。

明示的な変換演算子は、データ損失の可能性がある場合に提供されます (64 ビット値は32または32のストレージの場所に格納されています)。

`int`、`uint`、および `float` は、すべて `nint`、`nuint`、および `nfloat` に暗黙的に変換可能であり、32ビットは常に32または64ビットに収まります。

`nint`、`nuint` および `nfloat` はすべて `long`に暗黙的に変換でき、`ulong` は32または64ビット値は常に64ビットストレージに収まります。

ネイティブ型で64ビットのストレージが保持される可能性があるため、`nint`、`nuint`、および `nfloat` からの明示的な変換を `int`、`uint`、および `float` に使用する必要があります。

ネイティブ型は32ビットのストレージしか保持できないため、`long`、`ulong`、および `double` からの明示的な変換を `nint`、`nuint`、および `nfloat` に使用する必要があります。

## <a name="coregraphics-types"></a>CoreGraphics 型

CoreGraphics で使用されるポイント、サイズ、および四角形のデータ型では、実行されているデバイスに応じて32または64ビットが使用されます。  最初に iOS と Mac の API をバインドしたときに、ホストプラットフォームのサイズ (`System.Drawing`のデータ型) に一致するようになった既存のデータ構造を使用しました。

**統合**に移行する場合は、次の表に示すように、`System.Drawing` のインスタンスを対応する `CoreGraphics` に置き換える必要があります。

|System.string の古い型|新しいデータ型 CoreGraphics|説明|
|--- |--- |--- |
|`RectangleF`|`CGRect`|浮動小数点四角形の情報を保持します。|
|`SizeF`|`CGSize`|浮動小数点サイズ情報を保持します (幅、高さ)|
|`PointF`|`CGPoint`|浮動小数点の情報 (X, Y) を保持します。|

新しいデータ型は、データ構造の要素を格納するために float で使用されますが、新しいデータ型は `System.nfloat`を使用します。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
