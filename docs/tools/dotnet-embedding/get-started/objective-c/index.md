---
title: Objective C の概要
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 1769c2cd5e9f68e80f7f4c0ef5e2393971b659f9
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-objective-c"></a>Objective C の概要

これは、サポートされているすべてのプラットフォームの基本についての内容を含む Objective C の作業の開始ページです。

## <a name="requirements"></a>要件

Objective C では、.NET の埋め込みを使用するには、Mac を実行している必要があります。

* macOS 10.12 (Sierra) 以降
* Xcode 8.3.2 以降
* [モノラル 5.0](http://www.mono-project.com/download/)

インストールすることができます[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)を編集し、c# コードをコンパイルします。

> [!NOTE]
> * 以前のバージョンの macOS、Xcode、およびモノラル_可能性があります_機能しますが、テスト、サポートされていません
> * Windows では、コードの生成を行うことができますが、Xcode がインストールされている Mac コンピューターでコンパイルすることはのみ

## <a name="installing-net-embedding-from-nuget"></a>.NET は NuGet から埋め込みをインストールします。

手順に従います。[指示](~/tools/dotnet-embedding/get-started/install/install.md)をインストールして、プロジェクトの .NET の埋め込みを構成します。

コマンド呼び出しの例が記載されて、 [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md)と[iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md)ファースト ステップ ガイドです。

## <a name="platforms"></a>プラットフォーム

Objective C macOS、iOS、tvOS および watchOS 用アプリケーションを作成する最もよく使用される言語です。 これらのプラットフォームのすべてをサポートしている .NET の埋め込み 各プラットフォームの使用を意味一部[主な相違点と以下がここで説明した](~/tools/dotnet-embedding/objective-c/platforms.md)です。

### <a name="macos"></a>macOS

[MacOS アプリケーションを作成する](~/tools/dotnet-embedding/get-started/objective-c/macos.md)は、id、provisining プロファイル、シミュレーターおよびデバイスの設定と同様に、多くの追加の手順を含まないため最も簡単です。 IOS 用の 1 つ前に、macOS ドキュメントを開始することをお勧めしています。

### <a name="ios--tvos"></a>iOS/tvOS

既にセットアップして 1 つを作成する前に、iOS アプリケーションを開発するかどうかを確認してください。 .NET の埋め込みを使用します。 [の指示に従って](~/tools/dotnet-embedding/get-started/objective-c/ios.md)既に正常にビルドしているコンピューターからの iOS アプリケーションの展開を前提としています。

TvOS のサポートは、iOS プロジェクトではなく Ide (Visual Studio と Xcode の両方) で tvOS プロジェクトのみを使用して、iOS のしくみに似ています。

> [!NOTE]
> サポート watchOS は将来のリリースで利用可能なされる iOS/tvOS とよく似ています。

## <a name="further-reading"></a>関連項目

* [Objective C に固有の機能の .NET の埋め込み](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C のベスト プラクティス](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
* [ターゲット プラットフォーム](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>関連リンク

- [気象サンプル (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
