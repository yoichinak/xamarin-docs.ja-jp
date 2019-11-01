---
title: C の概要
description: このドキュメントでは、.NET 埋め込みを使用して C アプリケーションに .NET コードを埋め込む方法について説明します。 ここでは、Visual Studio 2019 と Visual Studio for Mac の両方で .NET 埋め込みを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: davidortinau
ms.author: daortin
ms.date: 04/19/2018
ms.openlocfilehash: 9660eccd3e14a6c4ecad901064650fe1e25458f8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029782"
---
# <a name="getting-started-with-c"></a>C の概要

## <a name="requirements"></a>［要件］

C で .NET 埋め込みを使用するには、次の動作を実行する Mac または Windows コンピューターが必要です。

### <a name="macos"></a>macOS

* macOS 10.12 (シエラレオネ) 以降
* Xcode 8.3.2 以降
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7、8、10以降
* Visual Studio 2015 以降

## <a name="installing-net-embedding-from-nuget"></a>NuGet からの .NET 埋め込みのインストール

次の[手順](~/tools/dotnet-embedding/get-started/install/install.md)に従って、プロジェクトの .Net 埋め込みをインストールして構成します。

構成する必要があるコマンドの呼び出しは、次のようになります (バージョン番号とパスが異なる可能性があります)。

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

すべて正常に実行されると、次の出力が表示されます。

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

`--compile` フラグはツールに渡されたため、.NET 埋め込みでは、出力ファイルを共有ライブラリにコンパイルする必要があります。これは、生成されたファイル、macOS 上の**libmanaged. dylib**ファイル、および Windows 上の**マネージ dll**の横にあります。

共有ライブラリを使用するには、マネージヘッダーファイルをインクルードし**ます**。このファイルは、それぞれのマネージライブラリ api に対応する C 宣言を提供し、前に説明したコンパイル済み共有ライブラリとリンクします。

## <a name="further-reading"></a>関連項目

* [.NET 埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープンソースプロジェクトへの貢献](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラーコードと説明](~/tools/dotnet-embedding/errors.md)
