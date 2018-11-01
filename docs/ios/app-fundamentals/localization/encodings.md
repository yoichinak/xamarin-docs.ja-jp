---
title: Xamarin.iOS での国際化エンコーディング
description: このドキュメントでは、使用可能なエンコーディングとそれらをアプリに追加する方法について説明する Xamarin.iOS の国際化エンコーディングについて説明します。
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: db24c8677b0a3099193132575e92bc43a4c31dea
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675563"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Xamarin.iOS での国際化エンコーディング

すべてのエンコーディングは、既定で Xamarin.iOS クラス ライブラリに含まれるとは限りません。

Xamarin.iOS アプリケーションのサイズを減らすためには、特定のエンコーディングを含めるしないし、mtouch エンコードする必要がありますのサポートを含むアセンブリを含めるように指示する必要があります。

これは、Mac または Visual Studio の Visual Studio で iOS のビルド/詳細設定 ウィンドウから余分なエンコーディングを選択して行います。

 [![](encodings-images/00.png "余分なエンコーディングを選択します。")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "余分なエンコーディングを選択します。")](encodings-images/00a.png#lightbox)

これらのいずれかを選択できます。

-  cjk: Chineese、日本語および韓国語
-  mideast: アラビア語、ヘブライ語、トルコ語、ラテン 5。
-  その他: キリル文字、バルト語、ベトナム語、ウクライナ語、タイ語
-  まれな: EBCDIC エンコーディングとその他のまれなコード ページ
-  西部: イースターと西ヨーロッパ言語ラテン語の言語
-  all


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

