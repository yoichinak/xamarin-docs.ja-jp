---
title: "環境"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7489c2fe38e8433811c5f298296baebacf1c0727
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="environment"></a>環境

*実行環境*は、プログラムの実行に影響を与える一連の環境変数です。 環境変数はプロジェクトのプロパティに一時的に設定するか、追加の引数を mtouch パッケージ ツールに指定することで永続的に設定できます。

## <a name="temporary-environment-variables"></a>一時的な環境変数

一時的な環境変数はプロジェクトの **[プロパティ]** から **[オプション]** ウィンドウを開き、**[実行] の [全般]** セクションで設定します。 これらの環境変数は、アプリケーションを Visual Studio for Mac で起動したときにのみ有効になります。アプリをタップして手動で起動した場合、これらの環境変数は設定されません。

## <a name="permanent-environment-variables"></a>永続的な環境変数

永続的な環境変数は、追加の引数を mtouch パッケージ ツールに指定することで設定されます。 これらの環境変数は実行可能ファイルにコンパイルされ、アプリが Xamarin Studio から起動されなくても設定されます。

## <a name="example"></a>例

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

