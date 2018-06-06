---
title: C の概要
description: このドキュメントでは、.NET の埋め込みを使用して C アプリケーションで .NET コードを埋め込む方法について説明します。 .NET の Visual Studio 2017 および Visual Studio の両方で埋め込み for mac を使用する方法についても説明します。
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: 248d44f23495e45d9d35b34622de0f3b85ca3e8d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794099"
---
# <a name="getting-started-with-c"></a>C の概要

## <a name="requirements"></a>必要条件

C では、.NET の埋め込みを使用するには、Mac または Windows 実行するコンピューターを必要があります。

### <a name="macos"></a>macOS

* macOS 10.12 (Sierra) 以降
* Xcode 8.3.2 以降
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7、8、10、またはそれ以降
* Visual Studio 2015 以降

## <a name="installing-net-embedding-from-nuget"></a>.NET は NuGet から埋め込みをインストールします。

手順に従います。[指示](~/tools/dotnet-embedding/get-started/install/install.md)をインストールして、プロジェクトの .NET の埋め込みを構成します。

構成する必要がありますコマンドの呼び出しを (場合によって異なるバージョン番号とパス) のようになります。

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>生成

### <a name="output-files"></a>出力ファイル

場合はすべてうまく行けば、次の出力が表示されます。

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

以降、`--compile`フラグは、ツールに渡された、.NET を埋め込む必要がありますも出力ファイルにコンパイルして見つけることができます、生成されたファイルの横にある、共有ライブラリ、 **libmanaged.dylib** macOS などとファイルを**managed.dll** windows です。

共有ライブラリを使用するには追加、 **managed.h** C ヘッダー ファイルは、それぞれに対応する C 宣言は、マネージ ライブラリ Api と、前述のリンクは、共有ライブラリをコンパイルします。

## <a name="further-reading"></a>関連項目

* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
