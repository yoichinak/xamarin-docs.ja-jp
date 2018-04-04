---
title: Java の概要
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 0bf8a90741df0be014dd48263a165668d0f7f604
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-java"></a>Java の概要


これは、java の場合、サポートされているすべてのプラットフォームの基本についての内容を含む作業の開始ページです。

## <a name="requirements"></a>要件

Java で .NET の埋め込みを使用するには、必要があります。

* Java 1.8 以降
* [モノラル 5.0](http://www.mono-project.com/download/)

For Mac:
* Xcode 8.3.2 以降

Windows の場合。
* C++ のサポートと visual Studio 2017
* Windows 10 SDK

Android:
* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/)以降
* [Android Studio 3.x](https://developer.android.com/studio/index.html) with Java 1.8

使用することができます[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)を編集し、c# コードをコンパイルします。

> [!NOTE]
> 以前のバージョンの Xcode、Visual Studio、Xamarin.Android、Android Studio、およびモノラル_可能性があります_機能しますが、テスト、サポートされていません。

## <a name="installation"></a>インストール

現時点では、.NET の埋め込み[NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```csharp
nuget install Embeddinator-4000
```
これは、配置は`Embeddinator-4000.exe`に、`packages/Embeddinator-4000/tools`ディレクトリ。

さらに、ソースから Embeddinator を構築を参照してください、 [git リポジトリ](https://github.com/mono/Embeddinator-4000/)と[の原因となった](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)手順についてはドキュメントです。

## <a name="platforms"></a>プラットフォーム

Java は現在 macOS、Windows、および Android 用のプレビュー状態です。

渡すことによって、プラットフォームが選択されている、 `--platform=<platform>` embeddinator のコマンドライン引数。 現在`macOS`、 `Windows`、および`Android`はサポートされています。

### <a name="macos-and-windows"></a>macOS および Windows

開発では、できなければならない 1.8 の Java をサポートする任意の Java IDE を使用します。 これを Android Studio を使用することもできます。 必要な場合、[ここに表示](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects)です。 標準の Java jar ファイルと同様、JAR ファイル出力を行うこともできます。

### <a name="android"></a>Android

既にセットアップして 1 つを作成する前に Android アプリケーションを開発するかどうかを確認してください。 Embeddinator を使用します。 [の指示に従って](~/tools/dotnet-embedding/get-started/java/android.md)既に正常にビルドしているコンピューターからの Android アプリケーションのデプロイを前提としています。

Android Studio が、開発用に推奨されますのサポートがある限り、他の Ide を使用する必要があります、 [AAR ファイル形式](https://developer.android.com/studio/projects/android-library.html)です。

## <a name="further-reading"></a>関連項目

* [Android で作業の開始](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android でのコールバック](~/tools/dotnet-embedding/android/callbacks.md)
* [Android の事前調査](~/tools/dotnet-embedding/android/index.md)
* [Embeddinator の制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
