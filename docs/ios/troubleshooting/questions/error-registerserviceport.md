---
title: "iOS デザイナー エラー RegisterServicePort が発生しました"
ms.topic: article
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8042ea895de3a623279c9d5f3a5309b990489773
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS デザイナー エラー RegisterServicePort が発生しました

## <a name="sample-error"></a>エラーの例
> System.AggregateException: System.SystemException---> に 1 つまたは複数のエラーが発生しました: (com.xamarin.MTHosting.2a0b1、com.apple.PowerManagement.control) RegisterServicePort: 返されるカーネル:-308 (-308): (ipc/mig) サーバーが失われました

## <a name="explanation"></a>説明
エラーの`RegisterServicePort`上と同様に同様のエラー メッセージは通常、コンピューターにスパイウェア/マルウェアの問題とします。 検討してください、[このバグ レポートにコメント](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4)詳細についてへのリンクと共に、 [Apple フォーラムのディスカッション](https://discussions.apple.com/thread/5596008)で可能なウイルス感染を削除する方法です。 

問題の診断に役立てるため、macOS アプリケーションを開く**コンソール**内のすべてのファイルを削除し、**ユーザー診断レポート**セクション[http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy). Mac 用 Visual Studio を起動し、デザイナーを使用しようとします。 場合は、新しいログ ファイルは、デザイナーの初期化に失敗した後、このセクションに表示、分析するために、これらを保存してください。  

最も重要なことを確認するには、このファイルに注意してください。 
> /usr/lib/libimckit.dylib

そのファイルが存在する場合は、上記の結果に関係なく、ここに挙げたマルウェア/スパイウェア問題がコンピューター上に存在します。  

次のリンクがこのスパイウェア/マルウェアを削除する手順: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

