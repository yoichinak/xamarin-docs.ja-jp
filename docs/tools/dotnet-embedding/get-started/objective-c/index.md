---
title: Objective C の概要
description: このドキュメントの説明方法 OBJECTIVE-C と .NET の埋め込みの使用を開始 これには、NuGet、およびサポートされているプラットフォームからの .NET の埋め込みのインストール要件について説明します。
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
---

# <a name="getting-started-with-objective-c"></a>Objective C の概要

これは、サポートされているすべてのプラットフォームの基礎を対象とする OBJECTIVE-C の作業の開始ページです。

## <a name="requirements"></a>必要条件

Objective C では、.NET の埋め込みを使用するには、Mac を実行している必要があります。

* macOS 10.12 (Sierra) またはそれ以降
* Xcode 8.3.2 またはそれ以降
* [Mono 5.0](https://www.mono-project.com/download/)

インストールすることができます[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)を編集およびコンパイル、C#コード。

> [!NOTE]
> * 以前のバージョンの macOS、Xcode、および Mono_可能性があります_機能しますが、テスト、サポートされていません
> * コード生成は、Windows で実行できますが、Xcode がインストールされている Mac コンピューターでコンパイルすることはのみ

## <a name="installing-net-embedding-from-nuget"></a>.NET の埋め込み NuGet からインストールします。

手順に従います。[指示](~/tools/dotnet-embedding/get-started/install/install.md)をインストールして、プロジェクトの .NET の埋め込みを構成します。

コマンド呼び出しの例が記載されて、 [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md)と[iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md)ファースト ステップ ガイドです。

## <a name="platforms"></a>プラットフォーム

OBJECTIVE-C では、macOS、iOS、tvOS、watchOS 向けのアプリケーションに最もよく使用される言語は、すべてのプラットフォームをサポートする .NET の埋め込み。 各プラットフォームの使用を意味一部[主な相違点と、これらはここで説明した](~/tools/dotnet-embedding/objective-c/platforms.md)します。

### <a name="macos"></a>macOS

[MacOS アプリケーションを作成する](~/tools/dotnet-embedding/get-started/objective-c/macos.md)が id、プロビジョニング プロファイル、シミュレーターおよびデバイスの設定など、追加の手順が多く必要としないために、最も簡単です。 IOS 用の 1 つの前に、macOS のドキュメントを開始することが推奨されます。

### <a name="ios--tvos"></a>iOS / tvOS

既に設定して 1 つ作成される前に、iOS アプリケーションを開発することを確認してください。 埋め込みの .NET を使用します。 [手順に従って](~/tools/dotnet-embedding/get-started/objective-c/ios.md)既に正常にビルドしているコンピューターから iOS アプリケーションのデプロイを前提としています。

TvOS のサポートは、iOS プロジェクトではなく Ide (Visual Studio と Xcode の両方) で tvOS のプロジェクトのみを使用して、iOS のしくみに似ています。

> [!NOTE]
> サポート watchOS は将来のリリースで利用でき、iOS と tvOS で非常にようになります。

## <a name="further-reading"></a>関連項目

* [Objective C に固有の .NET の埋め込み機能](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C のベスト プラクティス](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献します。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
* [ターゲット プラットフォーム](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>関連リンク

- [天気のサンプル (iOS および macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
