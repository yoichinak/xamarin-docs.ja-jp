---
title: "環境"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ef5cfa2a9eee0d5d01922e5b7b3f89a209396c56
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
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

