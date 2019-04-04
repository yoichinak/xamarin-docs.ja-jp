---
title: Java の概要
description: このドキュメントでは、Java と .NET の埋め込みの使用を開始する方法について説明します。 これは、システム要件、インストール、およびサポートされているプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: 79a483743946c4f7509833867f2afe4b1e055183
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667179"
---
# <a name="getting-started-with-java"></a>Java の概要

これは、java のサポートされているすべてのプラットフォームの基本について説明しますが、作業の開始ページです。

## <a name="requirements"></a>必要条件

Java での .NET の埋め込みを使用するには、必要があります。

* Java 1.8 以降
* [Mono 5.0](https://www.mono-project.com/download/)

Mac の場合。

* Xcode 8.3.2 またはそれ以降

Windows:

* Visual Studio 2017 と C++ のサポート
* Windows 10 SDK

Android:

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/)またはそれ以降
* [Android Studio 3.x](https://developer.android.com/studio/index.html) Java 1.8 を使用

使用することができます[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)を編集およびコンパイル、C#コード。

> [!NOTE]
> 以前のバージョンの Xcode、Visual Studio、Xamarin.Android、Android Studio、および Mono_可能性があります_機能しますが、テスト、サポートされていません。

## <a name="installation"></a>インストール

現在使用できる .NET の埋め込みは[NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

これは配置**Embeddinator 4000.exe**に、**パッケージ/Embeddinator-4000/ツール**ディレクトリ。

さらに、.NET のソースから埋め込みを構築を参照してください、 [git リポジトリ](https://github.com/mono/Embeddinator-4000/)と[貢献](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)手順についてはドキュメントです。

## <a name="platforms"></a>プラットフォーム

Java は現在 macOS、Windows、および Android 用のプレビュー状態です。

渡すことによって、プラットフォームが選択されている、 `--platform=<platform>` .NET 埋め込みツールのコマンドライン引数。 現在`macOS`、 `Windows`、および`Android`はサポートされています。

### <a name="macos-and-windows"></a>macOS および Windows

開発、Java 1.8 をサポートする任意の Java IDE を使用できる必要があります。 これは Android Studio を使用することもできます。 必要な場合、[こちらをご覧ください](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects)します。 標準の Java jar ファイルと JAR ファイルの出力を使用できます。

### <a name="android"></a>Android

既に設定して 1 つを作成する前に、の Android アプリケーションを開発することを確認してください。 埋め込みの .NET を使用します。 [手順に従って](~/tools/dotnet-embedding/get-started/java/android.md)既に正常にビルドしているコンピューターからの Android アプリケーションのデプロイを前提としています。

Android Studio が開発に推奨されますが、サポートがある限り、他の Ide は動作する必要があります、 [AAR ファイル形式](https://developer.android.com/studio/projects/android-library.html)します。

## <a name="further-reading"></a>関連項目

* [Android の概要](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android コールバック](~/tools/dotnet-embedding/android/callbacks.md)
* [Android の予備調査](~/tools/dotnet-embedding/android/index.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献します。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
