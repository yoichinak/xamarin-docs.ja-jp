---
title: PCL でサポートされているライブラリを確認する方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: davidortinau
ms.author: daortin
ms.date: 07/27/2018
ms.openlocfilehash: 6d6519fc058c91a0f3519be034f8840469f87f0e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013325"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>PCL でサポートされているライブラリを確認する方法を教えてください

- さまざまな PCL ターゲットプラットフォームでサポートされているさまざまな機能の概要については、このページの*サポートされている機能*の部分に関するページを参照してください[https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- もう1つの方法として、 [.Net 移植性アナライザー](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b)を使用して、既存のライブラリを PCL プロファイルに変換できるかどうかを評価します。

- 3番目の可能性は、使用する可能性のある実際のプロファイルの内容を参照することです。 たとえば、プロファイル78を使用すると、`C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` して、そこに含まれるすべてのアセンブリを表示できます。

どちらの方法を選択した場合でも、NuGet と Microsoft BCL ライブラリを使用していくつかの機能をダウンロードする必要があることに注意してください。
