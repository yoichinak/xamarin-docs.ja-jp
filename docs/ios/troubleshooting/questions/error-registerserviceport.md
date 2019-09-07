---
title: RegisterServicePort での iOS Designer エラー
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 815eb638b3e394dcfe4b60d716820c610b01f8a1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769318"
---
# <a name="ios-designer-error-with-registerserviceport"></a>RegisterServicePort での iOS Designer エラー

## <a name="sample-error"></a>サンプルエラー
> AggregateException:> の---に1つ以上のエラーが発生しました。 SystemException:RegisterServicePort (2a0b1、com.......... コントロール):返されたカーネル:-308 (-308): (ipc/mig) サーバーが停止しました

## <a name="explanation"></a>説明
上記の`RegisterServicePort`ようなエラーメッセージが発生したエラーは、通常、コンピューターのスパイウェア/マルウェアに関する問題です。 詳細については、[このバグレポートのコメント](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4)と、感染の可能性を削除する方法に関する[Apple フォーラム](https://discussions.apple.com/thread/5596008)のリンクをご確認ください。 

問題の診断を支援するには、macOS アプリケーション**コンソール**を開き、 **[ユーザー診断レポート]** セクション[http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy)内のすべてのファイルを削除します。 次に Visual Studio for Mac を起動し、デザイナーを使用します。 デザイナーの初期化に失敗した後に、このセクションに新しいログファイルが表示された場合は、分析のためにこれらを保存してください。  

次のファイルがあるかどうかを確認することをお勧めします。 
> /usr/lib/libimckit.dylib

上記の結果にかかわらず、そのファイルが存在する場合は、前述のスパイウェア/マルウェアの問題がコンピューターに存在します。  

次のリンクでは、このスパイウェア/マルウェアを削除する手順を説明しています。[http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  
