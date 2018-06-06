---
title: Xamarin.iOS の国際化エンコーディング
description: このドキュメントでは、Xamarin.iOS、利用可能なエンコーディングとアプリに追加する方法を説明の国際化エンコーディングについて説明します。
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4963b0f95ae48ee56462a82d2f82a8dcaa231a23
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784127"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Xamarin.iOS の国際化エンコーディング

すべてのエンコーディングは、既定で Xamarin.iOS クラス ライブラリに含まれます。

アプリケーションのサイズを減らすためには、Xamarin.iOS が含まれていない特定のエンコーディングを使用してにする必要がありますのエンコーディングのサポートを含むアセンブリを含める mtouch を指示します。

これは、Mac または Visual Studio の Visual Studio での iOS のビルド/詳細 ウィンドウから余分なエンコーディングを選択して行います。

 [![](encodings-images/00.png "余分なエンコーディングを選択します。")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "余分なエンコーディングを選択します。")](encodings-images/00a.png#lightbox)

これらのいずれかを選択できます。

-  cjk: Chineese、日本語および韓国語
-  mideast: アラビア語、ヘブライ語、トルコ語とラテン 5 です。
-  その他の: キリル文字、バルト語、ベトナム語、ウクライナ語、タイ語
-  まれな: EBCDIC エンコーディング、およびその他のまれなコード ページ
-  西: ラテン語の言語、内側および西ヨーロッパ
-  すべて


 <a name="cjk" />


## <a name="cjk"></a>cjk

-  CP51932
-  CP932
-  CP936
-  CP949
-  CP950
-  CP54936


 <a name="mideast" />


## <a name="mideast"></a>mideast

-  CP1254
-  CP1255
-  CP1256
-  CP28596
-  CP28598
-  CP28599
-  CP38598


 <a name="other" />


## <a name="other"></a>その他

-  CP1251
-  CP1257
-  CP1258
-  CP20866
-  CP21866
-  CP28594
-  CP28595
-  CP57002
-  CP874


 <a name="rare" />


## <a name="rare"></a>まれな

-  CP1026
-  CP1047
-  CP1140
-  CP1141
-  CP1142
-  CP1143
-  CP1144
-  CP1145
-  CP1146
-  CP1147
-  CP1148
-  CP1149
-  CP20273
-  CP20277
-  CP20278
-  CP20280
-  CP20284
-  CP20285
-  CP20290
-  CP20297
-  CP20420
-  CP20424
-  CP20871
-  CP21025
-  CP37
-  CP500
-  CP708
-  CP852
-  CP855
-  CP857
-  CP858
-  CP862
-  CP864
-  CP866
-  CP869
-  CP870
-  CP875


 <a name="west" />


## <a name="west"></a>西部

-  CP10000
-  CP10079
-  CP1250
-  CP1252
-  CP1253
-  CP28592
-  CP28593
-  CP28597
-  CP28605
-  CP437
-  CP850
-  CP860
-  CP861
-  CP863
-  CP865

