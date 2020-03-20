---
title: PathTooLongException エラーを解決する方法
description: この記事では、アプリのビルド中に発生する可能性がある PathTooLongException の解決方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/29/2018
ms.openlocfilehash: ffe88546ff58387865d71268bd64ec05c8aec3c5
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026789"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException エラーを解決する方法

## <a name="cause"></a>原因

Xamarin.Android プロジェクトで生成されるパス名は、非常に長くなることがあります。
たとえば、ビルド中に次のようなパスが生成される可能性があります。

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Windows の場合 (パスの最大長は [260 文字](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx))、生成されたパスが最大長を超えた場合、プロジェクトをビルドするときに、**PathTooLongException** が発生する可能性があります。 

## <a name="fix"></a>修正

既定でこのエラーを回避するため、`UseShortFileNames` MSBuild プロパティは `True` に設定されています。 このプロパティが `True` に設定されている場合、ビルド プロセスでは短いパス名が使用されて、**PathTooLongException** が発生する可能性は低くなります。
たとえば、`UseShortFileNames` が `True` に設定されていると、上記のパスは次のようなパスに短縮されます。

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

このプロパティを手動で設定するには、次の MSBuild プロパティをプロジェクトの **.csproj** ファイルに追加します。

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

このフラグを設定しても **PathTooLongException** エラーが解決しない場合は、別の方法として、プロジェクトの **.csproj** ファイルで `IntermediateOutputPath` を設定し、ソリューションのプロジェクトに対して[共通中間出力ルート](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/)を指定します。 比較的短いパスを使用してください。 次に例を示します。

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

ビルド プロパティの設定について詳しくは、「[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)」を参照してください。
