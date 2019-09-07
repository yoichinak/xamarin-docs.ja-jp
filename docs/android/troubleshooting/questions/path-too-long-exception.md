---
title: PathTooLongException エラーを解決操作方法には
description: この記事では、アプリのビルド中に発生する可能性がある PathTooLongException の解決方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 915f557db7955dc7b8b9f1bc5e014a683740052b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760812"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException エラーを解決操作方法には

## <a name="cause"></a>原因

Xamarin. Android プロジェクトで生成されたパス名は非常に長くなることがあります。
たとえば、ビルド中に次のようなパスが生成される可能性があります。

**C:\\いくつ\\か\\の\\ディレクトリソリューションプロジェクト\\objDebug\\library_projects\\\\\\library_project_importsアセット\\**

Windows の場合 (パスの最大長は[260 文字](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx))、生成されたパスが最大長を超えた場合にプロジェクトをビルドするときに**PathTooLongException**が生成されることがあります。 

## <a name="fix"></a>Fix

MSBuild `UseShortFileNames`プロパティは、既定で`True`このエラーを回避するようにに設定されています。 このプロパティがに`True`設定されている場合、ビルドプロセスでは短いパス名を使用して、 **PathTooLongException**を生成する可能性を減らします。
たとえば、がに`UseShortFileNames` `True`設定されている場合、上記のパスは次のようなパスに短縮されます。

**C:\\いくつ\\か\\の\\ディレクトリソリューションプロジェクト\\objデバッグ\\lp\\1jlアセット\\\\\\**

このプロパティを手動で設定するには、次の MSBuild プロパティをプロジェクトの **.csproj**ファイルに追加します。

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

このフラグを設定しても**PathTooLongException**エラーが解決しない場合は、別の方法として、プロジェクトの **.csproj**ファイルで`IntermediateOutputPath`を設定して、ソリューション内のプロジェクトに[共通の中間出力ルート](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/)を指定します。 比較的短いパスを使用してください。 例えば:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

ビルドプロパティの設定の詳細については、「[ビルドプロセス](~/android/deploy-test/building-apps/build-process.md)」を参照してください。
