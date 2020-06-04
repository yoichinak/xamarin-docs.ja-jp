---
title: Xamarin.Forms のローカリゼーション
description: 組み込みの .NET ローカライズ フレームワークを使用して、Xamarin.Forms を利用したクロスプラットフォームの多言語アプリケーションを構築できます。 テキストと画像をローカライズすることができ、アプリケーションのフロー方向を右から左にすることができます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dddb80e8fb683547d2bf6ba0b74e870fe240695c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137567"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms のローカリゼーション

_組み込みの .NET ローカライズ フレームワークを使用して、Xamarin.Forms を利用したクロスプラットフォームの多言語アプリケーションを構築できます。_

## <a name="xamarinforms-string-and-image-localizationtextmd"></a>[Xamarin.Forms の文字列とイメージのローカライズ](text.md)

.NET アプリケーションをローカライズするための組み込みメカニズムでは、`System.Resources` および `System.Globalization` 名前空間にある [RESX ファイル](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files)とクラスが使われます。 翻訳された文字列を含む RESX ファイルは、翻訳に対して、厳密に型指定されたアクセスを提供するコンパイラ生成のクラスと共に、Xamarin.Forms アセンブリに埋め込まれます。 その後、翻訳されたテキストをコードで取得できます。

## <a name="right-to-left-localization"></a>[右から左へのローカライズ](right-to-left.md)

フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 右から左へのローカライズでは、Xamarin.Forms アプリケーションに、右から左へのフロー方向のサポートが追加されます。
