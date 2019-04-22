---
title: C の概要
description: このドキュメントでは、.NET の埋め込みを使用して C# アプリケーションで .NET コードを埋め込む方法について説明します。 .NET の Visual Studio 2019 と Visual Studio の両方で埋め込み for mac を使用する方法について説明します
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: lobrien
ms.author: laobri
ms.date: 04/19/2018
ms.openlocfilehash: 342ba2a6b51483983df7bd04034a4cef62fd57ff
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854913"
---
# <a name="getting-started-with-c"></a>C の概要

## <a name="requirements"></a>必要条件

C では、.NET の埋め込みを使用するには、Mac または Windows マシンの実行を必要があります。

### <a name="macos"></a>macOS

* macOS 10.12 (Sierra) またはそれ以降
* Xcode 8.3.2 またはそれ以降
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7、8、10 以降
* Visual Studio 2015 またはそれ以降

## <a name="installing-net-embedding-from-nuget"></a>.NET の埋め込み NuGet からインストールします。

手順に従います。[指示](~/tools/dotnet-embedding/get-started/install/install.md)をインストールして、プロジェクトの .NET の埋め込みを構成します。

コマンドの呼び出しを構成する必要がありますを (場合によって別のバージョン番号とパス) でのようになります。

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

すべてうまくいけば場合、次の出力が表示されます。

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

以降、`--compile`フラグは、ツールに渡された、.NET を埋め込む必要がありますもが、出力ファイルにコンパイル共有ライブラリが含まれ、生成されたファイルの横にあるを検索する、 **libmanaged.dylib**ファイルに macOS、および**managed.dll** Windows にします。

共有ライブラリを使用するには、含めることができます、 **managed.h** C ヘッダー ファイルをそれぞれに対応する C の宣言を提供するマネージ ライブラリ Api と、前述のリンクは、共有ライブラリをコンパイルします。

## <a name="further-reading"></a>関連項目

* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献します。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
