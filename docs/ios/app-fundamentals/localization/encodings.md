---
title: Xamarin の国際化エンコーディングエンコード
description: このドキュメントでは、使用可能なエンコードについて説明し、アプリに追加する方法について説明します。
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: 78c048d793fd792576e2482491ebf11460d5b511
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571806"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Xamarin の国際化エンコーディングエンコード

既定では、すべてのエンコードが Xamarin. iOS クラスライブラリに含まれているわけではありません。

アプリケーションのサイズを小さくするために、Xamarin. iOS には特定のエンコーディングが含まれていないため、必要なエンコーディングのサポートを含むアセンブリを含めるように mtouch に指示する必要があります。

これを行うには、Visual Studio for Mac または Visual Studio の iOS のビルド/詳細ウィンドウで追加のエンコーディングを選択します。

 [![](encodings-images/00.png "Selecting the extra encodings")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "Selecting the extra encodings")](encodings-images/00a.png#lightbox)

次のいずれかを選択できます。

- cjk: Chineese、日本語、および韓国語
- mideast: アラビア語、ヘブライ語、トルコ語、および Latin5。
- その他: キリル、バルト言語、ベトナム語、ウクライナ語、タイ語
- まれ: EBCDIC エンコーディングおよびその他のまれなコードページ
- 西ヨーロッパ: ラテン言語、イースター、西ヨーロッパ
- all

 <a name="cjk"></a>

## <a name="cjk"></a>cjk

- CP51932
- CP932
- CP936
- CP949
- CP950
- CP54936

 <a name="mideast"></a>

## <a name="mideast"></a>mideast

- CP1254
- CP1255
- CP1256
- CP28596
- CP28598
- CP28599
- CP38598

 <a name="other"></a>

## <a name="other"></a>その他

- CP1251
- CP1257
- CP1258
- CP20866
- CP21866
- CP28594
- CP28595
- CP57002
- CP874

 <a name="rare"></a>

## <a name="rare"></a>珍しい

- CP1026
- CP1047
- CP1140
- CP1141
- CP1142
- CP1143
- CP1144
- CP1145
- CP1146
- CP1147
- CP1148
- CP1149
- CP20273
- CP20277
- CP20278
- CP20280
- CP20284
- CP20285
- CP20290
- CP20297
- CP20420
- CP20424
- CP20871
- CP21025
- CP37
- CP500
- CP708
- CP852
- CP855
- CP857
- CP858
- CP862
- CP864
- CP866
- CP869
- CP870
- CP875

 <a name="west"></a>

## <a name="west"></a>西

- CP10000
- CP10079
- CP1250
- CP1252
- CP1253
- CP28592
- CP28593
- CP28597
- CP28605
- CP437
- CP850
- CP860
- CP861
- CP863
- CP865
