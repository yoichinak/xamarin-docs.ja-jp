---
title: PathTooLongException エラーを解決する方法
description: この記事では、アプリのビルド中になる可能性がある PathTooLongException を解決する方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 4cb3e13ebbe3d9e8aed153528a35ab16c92e2145
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108705"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException エラーを解決する方法

## <a name="cause"></a>原因

Xamarin.Android プロジェクトで生成されるパス名は非常に長くすることはできます。
たとえば、ビルド中に、次のようなパスを生成できませんでした。

**C:\\一部\\ディレクトリ\\ソリューション\\プロジェクト\\obj\\デバッグ\\__library_projects__ \\Xamarin.Forms.Platform.Android\\library_project_imports\\資産**

Windows 上 (パスの最大長が[260 文字](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx))、 **PathTooLongException**生成されるパスが最大長を超える場合、プロジェクトのビルド時に生成される可能性があります。 

## <a name="fix"></a>修正

Xamarin.Android 8.0 以降、`UseShortFileNames`このエラーを回避するために MSBuild プロパティを設定することができます。 このプロパティに設定しているときに`True`(既定値は`False`)、ビルド プロセスが生成する可能性を減らすために短いパス名を使用、 **PathTooLongException**します。
たとえば、`UseShortFileNames`に設定されている`True`は、次のようなパスに、上記のパスを短縮します。

**C:\\一部\\ディレクトリ\\ソリューション\\プロジェクト\\obj\\デバッグ\\lp\\1\\jl\\資産**

このプロパティを設定するには、プロジェクトに次の MSBuild プロパティを追加 **.csproj**ファイル。

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

このフラグを設定する場合が解決しない、 **PathTooLongException**を指定するエラー、別の方法は、[共通の中間出力ルート](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/)を設定して、ソリューション内のプロジェクトの`IntermediateOutputPath`で、プロジェクト **.csproj**ファイル。 比較的短いパスを使用してください。 例えば:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

ビルドのプロパティの設定の詳細については、[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)を参照してください。
