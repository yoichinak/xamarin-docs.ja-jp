---
title: Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/31/2017
ms.openlocfilehash: fdd208122934b6fa8194a644592d1e23c4000d57
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030888"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。

Mono のランタイム環境変数を設定する必要がある場合は **> [全般] ページで [プロジェクトオプション > 実行**する] を設定できます。

注: SGen (MONO\_GC\_PARAMS) のガベージコレクション環境変数は、Xamarin Studio から起動する場合にのみ使用されます。 デバイスからアプリを起動した場合、Sgen の設定は無視されます。 

アプリの環境変数を永続的に設定するには、これを追加の mtouch 引数として追加する必要があります (すべての関連構成用)。

```csharp
   --setenv=NAME=VALUE
```

設定できる環境変数を確認するには、Mono man のページを参照してください。 [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1))については、「」を参照してください `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Setting environment variables for a project")
