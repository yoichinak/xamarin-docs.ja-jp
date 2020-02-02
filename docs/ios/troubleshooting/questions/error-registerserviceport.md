---
title: RegisterServicePort での iOS Designer エラー
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: b76de0f0dc8f1fd2124eac9cfe03b872c71095b1
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940949"
---
# <a name="ios-designer-error-with-registerserviceport"></a>RegisterServicePort での iOS Designer エラー

## <a name="sample-error"></a>サンプルエラー
> AggregateException: 1 つまたは複数のエラーが発生しました。 SystemException: RegisterServicePort (2a0b1,): カーネルが返されました:-308 (-308): (ipc/mig) サーバーが停止し >---ました。

## <a name="explanation"></a>説明
`RegisterServicePort` と同様のエラーメッセージを含むエラーは、通常、コンピューターのスパイウェア/マルウェアに関する問題です。 詳細については、[このバグレポートのコメント](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4)と、感染の可能性を削除する方法に関する[Apple フォーラム](https://discussions.apple.com/thread/5596008)のリンクをご確認ください。 

この問題の診断を支援するには、macOS アプリケーション**コンソール**を開き、 **[ユーザー診断レポート]** セクション内のすべてのファイルを削除します。 [https://screencast.com/t/y9i3NKcuMy](https://screencast.com/t/y9i3NKcuMy)を参照してください。 次に Visual Studio for Mac を起動し、デザイナーを使用します。 デザイナーの初期化に失敗した後に、このセクションに新しいログファイルが表示された場合は、分析のためにこれらを保存してください。  

次のファイルがあるかどうかを確認することをお勧めします。 
> /usr/lib/libimckit.dylib

上記の結果にかかわらず、そのファイルが存在する場合は、前述のスパイウェア/マルウェアの問題がコンピューターに存在します。  

次のリンクには、このスパイウェア/マルウェアを削除する手順が含まれています: [https://www.thesafemac.com/arg-genieo/](https://www.thesafemac.com/arg-genieo/)  
