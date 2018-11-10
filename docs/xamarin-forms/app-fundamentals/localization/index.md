---
title: Xamarin.Forms のローカライズ
description: Xamarin.Forms によるクロスプラット フォームで多言語アプリケーションを構築する、組み込みの .NET ローカライズ フレームワークを使用できます。 テキストとイメージをローカライズすることができ、アプリケーションが右から左にフローの方向をサポートします。
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: 71033e935a2d3a4be88dbcc5d975938771484640
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285574"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms のローカライズ

_Xamarin.Forms によるクロスプラット フォームで多言語アプリケーションを構築する、組み込みの .NET ローカライズ フレームワークを使用できます。_

## <a name="string-and-image-localizationtextmd"></a>[文字列とイメージのローカライズ](text.md)

.NET アプリケーションの使用をローカライズするための組み込みメカニズム[RESX ファイル](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files)クラスとクラスの`System.Resources`と`System.Globalization`名前空間。 翻訳された文字列を含む RESX ファイルは、コンパイラによって生成されたクラスの翻訳を厳密に型指定されたアクセスを提供すると共に、Xamarin.Forms アセンブリに埋め込まれます。 翻訳されたテキストは、コードで取得できます。

## <a name="right-to-left-localizationright-to-leftmd"></a>[右から左へのローカライズ](right-to-left.md)

フローの方向とは、目で、ページの UI 要素をスキャンする方向です。 右から左のローカリゼーションでは、Xamarin.Forms アプリケーションを右から左方向のサポートを追加します。
