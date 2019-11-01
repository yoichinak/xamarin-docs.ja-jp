---
title: RegisterServicePort での iOS Designer エラー
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 07b7e47ac448fdb9a4297fe03e8f4ea16295fddd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031135"
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
