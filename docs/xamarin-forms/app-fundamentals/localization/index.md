---
title: Xamarin.Forms ローカリゼーション
description: 組み込みの .NET のローカリゼーション フレームワークは、Xamarin.Forms を使用したクロスプラット フォームの多言語アプリケーションの構築に使用できます。
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 29624eeccc8542b3296774f6b77480b664bee478
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="localization"></a>ローカリゼーション

_組み込みの .NET のローカリゼーション フレームワークは、Xamarin.Forms を使用したクロスプラット フォームの多言語アプリケーションの構築に使用できます。_

## <a name="string-and-image-localizationtextmd"></a>[文字列とイメージのローカライズ](text.md)

.NET アプリケーションの使用をローカライズするための組み込みメカニズム[RESX ファイル](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx)と、クラス、`System.Resources`と`System.Globalization`名前空間。 翻訳された文字列を含む RESX ファイルは、コンパイラによって生成されたクラス、翻訳を厳密に型指定されたアクセスを提供すると共に、Xamarin.Forms アセンブリに埋め込まれます。 翻訳されたテキストは、コードで取得できます。

## <a name="right-to-left-localizationright-to-leftmd"></a>[右から左へのローカライズ](right-to-left.md)

フロー方向とは、ページ上の UI 要素が目によってスキャンされる方向です。 右から左へのローカライズでは、Xamarin.Forms アプリケーションを右から左へのフローの方向のサポートを追加します。
