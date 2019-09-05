---
title: Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/31/2017
ms.openlocfilehash: f8e3855b10a20bd4312420f8faf6c68dedde0c67
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292101"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。

Mono のランタイム環境変数を設定する必要がある場合は **> [全般] ページで [プロジェクトオプション > 実行**する] を設定できます。

メモ:この方法で設定された SGen\_(\_MONO GC PARAMS) のガベージコレクション環境変数は、Xamarin Studio から起動する場合にのみ使用されます。 デバイスからアプリを起動した場合、Sgen の設定は無視されます。 

アプリの環境変数を永続的に設定するには、これを追加の mtouch 引数として追加する必要があります (すべての関連構成用)。

```csharp
   --setenv=NAME=VALUE
```

設定できる環境変数を確認するには、Mono の man ページを参照してください。[http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1))「」を参照してください。`ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "プロジェクトの環境変数の設定")
