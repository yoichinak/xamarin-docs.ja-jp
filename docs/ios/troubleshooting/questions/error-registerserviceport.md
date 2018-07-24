---
title: iOS デザイナー エラー RegisterServicePort が発生しました
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62d4a06c62bffb23566f4f59f42a0c980f417c45
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776717"
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS デザイナー エラー RegisterServicePort が発生しました

## <a name="sample-error"></a>エラーの例
> System.AggregateException: System.SystemException---> に 1 つまたは複数のエラーが発生しました: (com.xamarin.MTHosting.2a0b1、com.apple.PowerManagement.control) RegisterServicePort: 返されるカーネル:-308 (-308): (ipc/mig) サーバーが失われました

## <a name="explanation"></a>説明
エラーの`RegisterServicePort`上と同様に同様のエラー メッセージは通常、コンピューターにスパイウェア/マルウェアの問題とします。 検討してください、[このバグ レポートにコメント](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4)詳細についてへのリンクと共に、 [Apple フォーラムのディスカッション](https://discussions.apple.com/thread/5596008)で可能なウイルス感染を削除する方法です。 

問題の診断に役立てるため、macOS アプリケーションを開く**コンソール**内のすべてのファイルを削除し、**ユーザー診断レポート**セクション[ http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy)です。 Mac 用 Visual Studio を起動し、デザイナーを使用しようとします。 場合は、新しいログ ファイルは、デザイナーの初期化に失敗した後、このセクションに表示、分析するために、これらを保存してください。  

最も重要なことを確認するには、このファイルに注意してください。 
> /usr/lib/libimckit.dylib

そのファイルが存在する場合は、上記の結果に関係なく、ここに挙げたマルウェア/スパイウェア問題がコンピューター上に存在します。  

次のリンクには、このスパイウェア/マルウェアを削除する手順があります。 [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

