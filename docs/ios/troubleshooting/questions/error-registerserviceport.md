---
title: iOS Designer エラー RegisterServicePort で
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: fc4c143d6b5f7c211d24e6e3ed2ed3bb8d264410
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104728"
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS Designer エラー RegisterServicePort で

## <a name="sample-error"></a>エラーの例
> System.AggregateException: System.SystemException---> に 1 つまたは複数のエラーが発生しました: (com.xamarin.MTHosting.2a0b1、com.apple.PowerManagement.control) RegisterServicePort: 返されるカーネル:-308 (-308): 亡くなった (ipc/mig) サーバー

## <a name="explanation"></a>説明
エラーの`RegisterServicePort`上などのようなエラー メッセージは通常、コンピューターにスパイウェア/マルウェアの問題があるとします。 検討してください、[このバグ レポートにコメント](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4)詳細についてへのリンクと共に、 [Apple フォーラム ディスカッション](https://discussions.apple.com/thread/5596008)考えられる感染を削除する方法の。 

MacOS アプリケーションを開きに、問題の診断に役立てるため、**コンソール**内のすべてのファイルを削除し、**ユーザー診断レポート**セクション[ http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy)します。 Mac の Visual Studio を起動し、次に、デザイナーを使用しようとします。 場合は、新しいログ ファイルがこのセクションに表示するは、デザイナーが初期化に失敗した後、これらを分析するために保存してください。  

確認する最も重要な点は、このファイルに注意してください。 
> /usr/lib/libimckit.dylib

そのファイルが存在する場合は上記の結果に関係なく、前述のスパイウェア/マルウェア問題がコンピューターに存在します。  

次のリンクには、このスパイウェア/マルウェアを削除する手順があります。 [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

