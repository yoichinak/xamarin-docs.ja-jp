---
title: どの、PCL にどのようなライブラリがサポートされているを表示しますか?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351471"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>どの、PCL にどのようなライブラリがサポートされているを表示しますか?

- 下のさまざまな PCL ターゲット プラットフォームでサポートされているさまざまな機能の概要を見つけることができます、*でサポートされる機能*このページの部分。 [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- 使用することも、 [.NET Portability Analyzer](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) PCL プロファイルには、既存のライブラリを変換できるかどうかを評価します。

- 3 つ目の可能性がありますを使用する実際のプロファイルの内容を参照することです。 プロファイル 78 を例を使用する場合がありますここに移動して:`C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\`とその中のすべてのアセンブリを表示します。

どちらの方法を選択すると、ください一部の機能は、NuGet と Microsoft BCL ライブラリを使用してダウンロードする必要がありますに注意してください。
