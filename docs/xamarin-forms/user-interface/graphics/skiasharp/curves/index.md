---
title: SkiaSharp の曲線とパス
description: この記事では、SkiaSharp を使用して、アプリケーションで曲線を描画し、パス機能を使用する方法について説明し、サンプルコードを使用してその方法を Xamarin.Forms 示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 781937AA-AA1C-469C-AA92-D42D08B58635
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 69a9ddc0096b0a351a7f190f428040b5092c5154
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373303"
---
# <a name="skiasharp-curves-and-paths"></a>SkiaSharp の曲線とパス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp を使用して曲線を描画し、パス機能を使用する方法について説明します。_

[`SKPath`](xref:SkiaSharp.SKPath)メソッドとプロパティの探索は、SkiaSharp の [**行とパス**](../paths/index.md)に関する記事で開始しました。 この記事では、オブジェクトに曲線を追加 `SKPath` し、その他の強力なパス機能を利用するメソッドについて説明します。 簡潔なテキスト文字列でパス全体を指定する方法、パスの効果を使用する方法、およびパスの内部を掘り下げる方法について説明します。

このセクションのすべてのサンプルプログラムは、 [**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの [ **SkiaSharp の曲線とパス** ] ページと、ソリューションの [ [**曲線**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves)] フォルダーにあります。

## <a name="three-ways-to-draw-an-arc"></a>[円弧を描画する 3 つの方法](arcs.md)

SkiaSharp を使用して、3つの異なる方法で円弧を定義する方法について説明します。

## <a name="three-types-of-bzier-curves"></a>[3 つの種類のベジエ曲線](beziers.md)

SkiaSharp を使用して、3次ベジエ曲線を描画する方法について説明します。

## <a name="svg-path-data"></a>[SVG Path Data](path-data.md)

スケーラブルなベクターグラフィックス形式でテキスト文字列を使用してパスを定義する

## <a name="clipping-with-paths-and-regions"></a>[パスおよび領域でクリッピング](clipping.md)

パスを使用して、特定の領域にグラフィックスをクリップしたり、領域を作成したりします。

## <a name="path-effects"></a>[パスの効果](effects.md)

ストロークの描画と塗りつぶしにパスを使用できるようにするさまざまなパス効果を検出します。

## <a name="paths-and-text"></a>[パスとテキスト](text-paths.md)

パスとテキストの共通部分を調べる

## <a name="path-information-and-enumeration"></a>[パス情報と列挙](information.md)

パスに関する情報を取得して内容を列挙する

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)