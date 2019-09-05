---
title: Java の概要
description: このドキュメントでは、Java での .NET 埋め込みの使用を開始する方法について説明します。 システム要件、インストール、およびサポートされているプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: conceptdev
ms.author: crdun
ms.date: 03/28/2018
ms.openlocfilehash: 8d6bc284d07ce1be11ad273f875b75a70ae14a0f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278404"
---
# <a name="getting-started-with-java"></a>Java の概要

これは、サポートされているすべてのプラットフォームの基本を説明する Java の概要ページです。

## <a name="requirements"></a>必要条件

Java で .NET 埋め込みを使用するには、次のものが必要です。

* Java 1.8 以降
* [Mono 5.0](https://www.mono-project.com/download/)

Mac の場合:

* Xcode 8.3.2 以降

Windows の場合:

* Visual Studio 2017 ( C++サポートあり)
* Windows 10 SDK

Android の場合:

* [Xamarin Android 7.5](https://visualstudio.microsoft.com/xamarin/)以降
* Java 1.8 での[Android Studio](https://developer.android.com/studio/index.html) 3.x

[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)を使用すると、コードを編集C#してコンパイルできます。

> [!NOTE]
> 以前のバージョンの Xcode、Visual Studio、Xamarin. Android、Android Studio、Mono は動作する_場合があり_ますが、テストおよびサポートされていません。

## <a name="installation"></a>インストール

現在、.NET 埋め込みは[NuGet](https://www.nuget.org/packages/Embeddinator-4000/)で利用できます。

```shell
nuget install Embeddinator-4000
```

これにより、 **Embeddinator-4000**が**packages/Embeddinator-4000/tools**ディレクトリに配置されます。

また、ソースから .NET 埋め込みを作成することもできます。詳細については、 [git リポジトリ](https://github.com/mono/Embeddinator-4000/)と、関連[するドキュメントを](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)参照してください。

## <a name="platforms"></a>プラットフォーム

現在、Java は macOS、Windows、および Android のプレビュー状態です。

プラットフォームを選択するには、 `--platform=<platform>`コマンドライン引数を .net 埋め込みツールに渡します。 現在`macOS` `Android` 、 `Windows`、、およびがサポートされています。

### <a name="macos-and-windows"></a>macOS と Windows

開発用には、Java 1.8 をサポートする任意の Java IDE を使用できます。 必要に応じて Android Studio を使用することもできます。[こちらを参照してください](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects)。 JAR ファイルの出力は、標準の Java jar ファイルと同じように使用できます。

### <a name="android"></a>Android

.NET 埋め込みを使用して作成する前に、Android アプリケーションを開発するように既に設定されていることを確認してください。 [次の手順](~/tools/dotnet-embedding/get-started/java/android.md)は、既にコンピューターから Android アプリケーションをビルドして展開していることを前提としています。

開発には Android Studio をお勧めしますが、 [AAR ファイル形式](https://developer.android.com/studio/projects/android-library.html)がサポートされている限り、他の ide が動作します。

## <a name="further-reading"></a>関連項目

* [Android でのはじめに](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android でのコールバック](~/tools/dotnet-embedding/android/callbacks.md)
* [Android の暫定版の研究](~/tools/dotnet-embedding/android/index.md)
* [.NET 埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープンソースプロジェクトへの貢献](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラーコードと説明](~/tools/dotnet-embedding/errors.md)
