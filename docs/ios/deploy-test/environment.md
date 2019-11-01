---
title: Xamarin.iOS アプリの実行環境
description: このドキュメントでは、Xamarin.iOS アプリ用に一時的な環境変数と永続的な環境変数を設定する方法について説明します。 変数は、プロジェクトのプロパティで、または mtouch パッケージ ツールへの追加引数として指定することができます。
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: b2452c50fa978f3cabb718afe7330bf0fb701ce3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030238"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Xamarin.iOS アプリの実行環境

*実行環境*は、プログラムの実行に影響を与える一連の環境変数です。 環境変数はプロジェクトのプロパティに一時的に設定するか、追加の引数を mtouch パッケージ ツールに指定することで永続的に設定できます。

## <a name="temporary-environment-variables"></a>一時的な環境変数

一時的な環境変数はプロジェクトの **[プロパティ]** から **[オプション]** ウィンドウを開き、 **[実行] の [全般]** セクションで設定します。 これらの環境変数は、アプリケーションを Visual Studio for Mac で起動したときにのみ有効になります。アプリをタップして手動で起動した場合、これらの環境変数は設定されません。

## <a name="permanent-environment-variables"></a>永続的な環境変数

永続的な環境変数は、追加の引数を mtouch パッケージ ツールに指定することで設定されます。 これらの環境変数は実行可能ファイルにコンパイルされ、アプリが Visual Studio for Mac から起動されなくても設定されます。

## <a name="example"></a>例

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```
