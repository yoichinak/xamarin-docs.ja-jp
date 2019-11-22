---
title: Xamarin.Forms のローカライズ
description: 組み込みの .NET ローカライズ フレームワークを使って、Xamarin.Forms を使ったクロスプラットフォームの多言語アプリケーションを構築できます。 テキストと画像をローカライズすることができ、アプリケーションのフロー方向を右から左にすることができます。
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: b580c6e41aa689ff8fcea698c40d7aaf5f2ca050
ms.sourcegitcommit: 233aaa1ac3d8f40c09b6daf6d944ea0b4cbee381
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74135260"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms のローカライズ

_組み込みの .NET ローカライズ フレームワークを使って、Xamarin.Forms を使ったクロスプラットフォームの多言語アプリケーションを構築できます。_

## <a name="xamarinforms-string-and-image-localizationtextmd"></a>[Xamarin.Forms の文字列とイメージのローカライズ](text.md)

.NET アプリケーションをローカライズするための組み込みメカニズムでは、`System.Resources` および `System.Globalization` 名前空間にある [RESX ファイル](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files)とクラスが使われます。 翻訳された文字列を含む RESX ファイルは、厳密に型指定された翻訳へのアクセスを提供するコンパイラ生成のクラスと共に、Xamarin.Forms アセンブリに埋め込まれます。 その後、翻訳されたテキストをコードで取得できます。

## <a name="right-to-left-localizationright-to-leftmd"></a>[右から左へのローカライズ](right-to-left.md)

フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 右から左へのローカライズでは、右から左へのフロー方向のサポートが Xamarin.Forms アプリケーションに追加されます。
