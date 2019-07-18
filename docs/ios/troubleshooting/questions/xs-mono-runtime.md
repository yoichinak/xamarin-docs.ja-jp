---
title: Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417665"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。

Mono の任意のランタイム環境変数を設定する必要がある場合で設定できますが、**プロジェクト オプション > 実行 > 全般**ページ。

メモ:SGen のガベージ コレクションの環境変数 (MONO\_GC\_PARAMS) セットをこの方法は、Xamarin Studio から起動するときにのみ使用されます。 デバイスからアプリを起動した場合、Sgen の設定は無視されます。 

アプリ用の環境変数を完全に設定するには (すべての関連する構成) の他の mtouch 引数として追加する必要があります。

```csharp
   --setenv=NAME=VALUE
```

設定可能な環境変数は、Mono の man ページを参照してください。[http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) 」のセクションを参照してください。 `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "プロジェクトの環境変数の設定")
