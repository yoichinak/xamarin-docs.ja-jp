---
title: 環境
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bc06ce3f3a26842340ce6e19741a8a7dfe8f086d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="environment"></a>環境

*実行環境*は、プログラムの実行に影響を与える一連の環境変数です。 環境変数はプロジェクトのプロパティに一時的に設定するか、追加の引数を mtouch パッケージ ツールに指定することで永続的に設定できます。

## <a name="temporary-environment-variables"></a>一時的な環境変数

一時的な環境変数はプロジェクトの **[プロパティ]** から **[オプション]** ウィンドウを開き、**[実行] の [全般]** セクションで設定します。 これらの環境変数は、アプリケーションを Visual Studio for Mac で起動したときにのみ有効になります。アプリをタップして手動で起動した場合、これらの環境変数は設定されません。

## <a name="permanent-environment-variables"></a>永続的な環境変数

永続的な環境変数は、追加の引数を mtouch パッケージ ツールに指定することで設定されます。 これらの環境変数は実行可能ファイルにコンパイルされ、アプリが Visual Studio for Mac から起動されなくても設定されます。

## <a name="example"></a>例

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

