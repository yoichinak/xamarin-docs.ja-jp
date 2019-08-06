---
title: IOS と macOS のネイティブ型
description: このドキュメントでは、Xamarin の Unified API が、コンパイルターゲットアーキテクチャに基づき、必要に応じて .NET 型を32ビットおよび64ビットのネイティブ型にマップする方法について説明します。
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 9d43bbdb49fe4ab1ff909f709a37f979c360ceb9
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509590"
---
# <a name="native-types-for-ios-and-macos"></a>IOS と macOS のネイティブ型

Mac および iOS API は、アーキテクチャに固有のデータ型を使用します。32 ビット プラットフォームでは常に 32 ビットであり、64 ビット プラットフォームでは常に 64 ビットとなります。

たとえば、Objective-C は`NSInteger`データ型を 32 ビット システムでは`int32_t`に、64 ビット システムでは`int64_t`にマップします。

この動作を適合させるために、統合 API では、以前に使用`int`されて`System.nint`いた (.net では`System.Int32`常にであると定義されている) を新しいデータ型に置き換えます。 "N" は "ネイティブ" の意味であると考えることができます。したがって、プラットフォームのネイティブ整数型です。

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

新しいデータ型の設計は、ホストプラットフォームとコンパイルの設定C#に応じて、1つのソースファイルで32または64ビットのストレージを自然に使用できるようにすることを目的としています。

これにより、プラットフォーム固有のデータ型と .NET の整数データ型および浮動小数点データ型との間で暗黙的および明示的な変換のセットを設計する必要がありました。

暗黙の変換演算子は、データ損失の可能性がない場合に提供されます (32 ビット値は64ビット空間に格納されています)。

明示的な変換演算子は、データ損失の可能性がある場合に提供されます (64 ビット値は32または32のストレージの場所に格納されています)。

 `int`、 `uint` 、 `float`およびは`nint`すべてに`nfloat`暗黙的に変換可能であり、32ビットは常に32または64ビットに収まります。`nuint`

 `nint`、 `nuint` 、 `nfloat`および`long`はすべてに`double`暗黙的に変換可能であり、32または64ビット値は常に64ビットストレージに収まります。`ulong`

ネイティブ型は64ビットの`nint`ストレージを保持し`uint`て`float`いる可能性があるため、 `nuint`と`nfloat` `int`の間の明示的な変換を使用する必要があります。

ネイティブ型で`long`は、 `ulong` 32 ビットのストレージしか保持`nfloat`できない場合があるため、と`double` `nint` `nuint`の間の明示的な変換を使用する必要があります。

## <a name="coregraphics-types"></a>CoreGraphics 型

CoreGraphics で使用されるポイント、サイズ、および四角形のデータ型では、実行されているデバイスに応じて32または64ビットが使用されます。  最初に iOS と Mac の Api をバインドしたときに、ホストプラットフォームのサイズ (で`System.Drawing`はデータ型) に一致するようになった既存のデータ構造を使用しました。

**統合**に移行する場合は、次の表に示す`System.Drawing`ように`CoreGraphics` 、のインスタンスを対応するインスタンスに置き換える必要があります。

|System.string の古い型|新しいデータ型 CoreGraphics|説明|
|--- |--- |--- |
|`RectangleF`|`CGRect`|浮動小数点四角形の情報を保持します。|
|`SizeF`|`CGSize`|浮動小数点サイズ情報を保持します (幅、高さ)|
|`PointF`|`CGPoint`|浮動小数点の情報 (X, Y) を保持します。|

新しいデータ型は、データ構造体の要素を格納するために使用されますが`System.nfloat`、新しいデータ型はを使用します。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
