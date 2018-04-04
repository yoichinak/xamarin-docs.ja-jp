---
title: C の概要
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8dff45de6de7c9492b199f323656778ac5c34d57
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-c"></a>C の概要


## <a name="requirements"></a>要件

C で .NET の埋め込みを使用するには、Mac または Windows 実行するコンピューターを必要があります。

* macOS 10.12 (Sierra) 以降
* Xcode 8.3.2 以降

* Windows 7、8、10、またはそれ以降
* Visual Studio 2015 以降

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>インストール

次の手順をダウンロードしてコンピューターに .NET の埋め込みツールをインストールします。

C、および Java ジェネレーターのバイナリのビルドが利用できない場合が、近日公開されます。

代わりにビルドすることは、git リポジトリから参照してください、[の原因となった](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)手順についてはドキュメントです。


## <a name="generation"></a>生成

C のコードを生成するには、ツールを起動、.NET の埋め込み C 言語を対象とする正しいフラグを渡します。

### <a name="windows"></a>Windows の場合:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

呼び出す、Visual Studio のコマンド シェルから特定バージョンの Visual Studio を確認してくださいでは、対象としています。

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>出力ファイル

場合はすべてうまく行けば、次の出力が表示されます。

```csharp
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

以降、`--compile`フラグは、ツールに渡された、.NET を埋め込む必要がありますも出力ファイルにコンパイルして見つけることができます、生成されたファイルの横にある、共有ライブラリ、 `libmanaged.dylib` macOS、上のファイルと`managed.dll`windows です。

含めることができます、共有ライブラリを使用する、 `managed.h` C ヘッダー ファイルは、それぞれに対応する C 宣言は、マネージ ライブラリ Api と、前述のリンクは、共有ライブラリをコンパイルします。

## <a name="further-reading"></a>関連項目

* [Embeddinator の制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
