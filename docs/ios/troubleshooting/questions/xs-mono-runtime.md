---
title: "Xamarin Studio で iOS のプロジェクト用の Mono ランタイム環境変数を設定する方法"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 6032ea89aa54719cc4b0fdde67e67f1ec8fb183b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio で iOS のプロジェクト用の Mono ランタイム環境変数を設定する方法

モノラルの任意のランタイム環境変数を設定する必要がある場合設定できます、**プロジェクトのオプション > 実行 > 全般**ページ。

注: SGen 用の環境変数のガベージ コレクション (モノラル\_GC\_PARAMS) セットがこの方法は、Xamarin Studio から起動したときにのみ使用されます。 デバイスからアプリを起動すると、Sgen の設定は無視されます。 

アプリ用の環境変数を完全に設定するには、(すべての関連する構成) 追加 mtouch 引数として追加する必要があります。

```csharp
   --setenv=NAME=VALUE
```

設定可能な環境変数を表示するには、Mono マニュアルを参照してください: [http://docs.go-mono.com/?link=man%3amono (1)](http://docs.go-mono.com/?link=man%3amono(1))というタイトルのセクションを参照してください。 `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "プロジェクトの環境変数の設定")
