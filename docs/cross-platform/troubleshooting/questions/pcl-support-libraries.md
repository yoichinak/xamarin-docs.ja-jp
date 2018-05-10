---
title: どのように、PCL にどのようなライブラリがサポートされているを表示しますか?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.openlocfilehash: 87f65ba2cff2d5990c32aa142f97766a76d6ba05
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>どのように、PCL にどのようなライブラリがサポートされているを表示しますか?

- 下のさまざまな PCL ターゲット プラットフォームでサポートされるさまざまな機能の概要を知ることができます、*サポートされる機能*このページの部分。 [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- 別のオプションは、使用する、 [.NET 移植性アナライザー](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) PCL プロファイルには、既存のライブラリを変換できるかどうかを評価します。

- 3 番目の方法としてでは、使用可能性のある実際のプロファイルの内容を参照です。 プロファイル 78 を例を使用する可能性がありますこちら:`C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\`およびその中のすべてのアセンブリを表示します。

どちらの方法を選択すると、ください NuGet と Microsoft BCL ライブラリを使用してダウンロードするために一部の機能がある注意してください。
