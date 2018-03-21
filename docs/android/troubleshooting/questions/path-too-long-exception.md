---
title: "PathTooLongException エラーを解決する方法"
description: "この記事では、アプリケーションのビルド中に発生する可能性がありますを PathTooLongException を解決する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f4554178648d1257049d1c21cdc3e141acdb3de7
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException エラーを解決する方法

## <a name="cause"></a>原因

Xamarin.Android プロジェクトで生成されるパス名は、非常に長くなることができます。
たとえば、ビルド時に、次のようなパスを生成できませんでした。

**C:\\一部\\ディレクトリ\\ソリューション\\プロジェクト\\obj\\デバッグ\\__library_projects__ \\Xamarin.Forms.Platform.Android\\library_project_imports\\資産**

Windows 上 (パスの最大長は[260 文字](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx))、 **PathTooLongException**生成されたパスが最大長を超える場合、プロジェクトのビルド中に生成される可能性があります。 

## <a name="fix"></a>修正

Xamarin.Android 8.0 以降、`UseShortFileNames`このエラーを回避する MSBuild プロパティを設定できます。 このプロパティを設定すると`True`(既定値は`False`)、ビルド プロセスでは、短いパス名を使用して、生成する可能性を減少、 **PathTooLongException**です。
たとえば、`UseShortFileNames`に設定されている`True`、上記のパス部分は切り捨てられますが、次のようなパス。

**C:\\一部\\ディレクトリ\\ソリューション\\プロジェクト\\obj\\デバッグ\\lp\\1\\jl\\資産**

このプロパティを設定するには、プロジェクトに次の MSBuild プロパティを追加**.csproj**ファイル。

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

ビルドのプロパティの設定の詳細については、次を参照してください。[ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)です。
