---
title: Xamarin.iOS アプリの実行環境
description: このドキュメントでは、Xamarin.iOS アプリ用に一時的な環境変数と永続的な環境変数を設定する方法について説明します。 変数は、プロジェクトのプロパティで、または mtouch パッケージ ツールへの追加引数として指定することができます。
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: e8f08a96d42aa02cb00f37337ecbc2620a8785e4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762959"
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
