---
title: Xamarin.Forms のローカリゼーション
description: 組み込みの .NET ローカライズ フレームワークを使用して、Xamarin.Forms を利用したクロスプラットフォームの多言語アプリケーションを構築できます。 テキストと画像をローカライズすることができ、アプリケーションのフロー方向を右から左にすることができます。
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5dbcc63eb162454845b78fc24a3beb0ca8296453
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558155"
---
# <a name="no-locxamarinforms-localization"></a>Xamarin.Forms のローカリゼーション

_組み込みの .NET ローカライズ フレームワークを使用して、Xamarin.Forms を利用したクロスプラットフォームの多言語アプリケーションを構築できます。_

## <a name="no-locxamarinforms-string-and-image-localization"></a>[Xamarin.Forms の文字列とイメージのローカライズ](text.md)

.NET アプリケーションをローカライズするための組み込みメカニズムでは、`System.Resources` および `System.Globalization` 名前空間にある [RESX ファイル](/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files)とクラスが使われます。 翻訳された文字列を含む RESX ファイルは、翻訳に対して、厳密に型指定されたアクセスを提供するコンパイラ生成のクラスと共に、Xamarin.Forms アセンブリに埋め込まれます。 その後、翻訳されたテキストをコードで取得できます。

## <a name="right-to-left-localization"></a>[右から左へのローカライズ](right-to-left.md)

フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 右から左へのローカライズでは、Xamarin.Forms アプリケーションに、右から左へのフロー方向のサポートが追加されます。