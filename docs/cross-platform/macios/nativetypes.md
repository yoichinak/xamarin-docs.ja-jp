---
title: IOS および macOS 用のネイティブ型
description: このドキュメントは、コンパイルのターゲット アーキテクチャに基づいて、必要に応じて、32 ビットおよび 64 ビットのネイティブ型に、Xamarin の Unified API が .NET 型をマップする方法を説明します。
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fc2b91a9265fcf09e4f58d5de27a1fdef9350b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199638"
---
# <a name="native-types-for-ios-and-macos"></a>IOS および macOS 用のネイティブ型

Mac および iOS API は、アーキテクチャに固有のデータ型を使用します。32 ビット プラットフォームでは常に 32 ビットであり、64 ビット プラットフォームでは常に 64 ビットとなります。

たとえば、OBJECTIVE-C でマップ、`NSInteger`のデータ型`int32_t`32 ビット システム上と`int64_t`64 ビット システムで。

統一された API では、この動作を一致するように、前に使用を交換する`int`(常として定義されている .NET の`System.Int32`) を新しいデータ型:`System.nint`します。 考えることができます"n"の意味として「ネイティブ」ので、プラットフォームのネイティブの整数を入力します。

これらの新しいデータ型と、コンパイル フラグによって、32 ビットおよび 64 ビットのアーキテクチャの同じソース コードがコンパイルされます。

## <a name="new-data-types"></a>新しいデータ型

次の表では、この新しい 32/64 ビット環境に一致するように、データ型で、変更を示しています。

|ネイティブ型|32 ビットのバッキング型|64 ビットのバッキング型|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

これらの名前を許可する選択した、C#今日の見た目と同じ方法で検索増減するコード。

### <a name="implicit-and-explicit-conversions"></a>暗黙の型変換と明示的な型変換

1 つを許可するものでは、新しいデータ型のデザインC#当然ながら、ホストのプラットフォームとコンパイルの設定によって 32 ビットまたは 64 ビットのストレージを使用するソース ファイル。

これには、.NET の整数と浮動小数点データ型への明示的および暗黙的な変換とプラットフォームに固有のデータ型とのセットを設計することが必要です。

(32 ビットの値が 64 ビットの領域に格納されている) のデータ損失の可能性がない場合に、暗黙的な変換演算子が提供されます。

データ損失の可能性がある場合に、明示的な変換の演算子を指定 (します 64 ビット値は 32 ビットまたは 32 可能性のある記憶域の場所に保存されている)。

 `int`、`uint`と`float`はすべてに暗黙的に変換`nint`、`nuint`と`nfloat`32 ビットに常に 32 ビットまたは 64 ビットに収まるようにします。

 `nint`、`nuint`と`nfloat`はすべてに暗黙的に変換`long`、`ulong`と`double`32 ビットまたは 64 ビットの値は、64 ビットのストレージで常に収まる限りです。

明示的な変換を使用する必要があります`nint`、`nuint`と`nfloat`に`int`、`uint`と`float`のため、ネイティブ型は、64 ビットの記憶域を保持可能性があります。

明示的な変換を使用する必要があります`long`、`ulong`と`double`に`nint`、`nuint`と`nfloat`ネイティブ型は 32 ビットの記憶域を保持することのみされる可能性があります。

## <a name="coregraphics-types"></a>CoreGraphics 型

CoreGraphics で使用されるポイント、サイズ、および四角形のデータ型で実行されているデバイスに応じて 32 ビットまたは 64 ビットを使用します。  ホストのプラットフォームのサイズに一致するように発生した既存のデータ構造を使用して iOS と Mac の Api は、もともとバインドされたときに (データ型は`System.Drawing`)。

移動するときに**統合**のインスタンスを置換する必要があります`System.Drawing`でその`CoreGraphics`次の表に示すように対応します。

|System.Drawing で古い型|新しいデータ型の CoreGraphics|説明|
|--- |--- |--- |
|`RectangleF`|`CGRect`|浮動小数点の保留は、四角形の情報をポイントします。|
|`SizeF`|`CGSize`|浮動小数点の保留のポイント サイズの情報 (幅、高さ)|
|`PointF`|`CGPoint`|浮動小数点を保持するポイント情報 (X, Y)|

データ構造体の要素を格納する古いデータ型が使用される浮動小数点の数に 1 つを使用して新しい`System.nfloat`します。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
