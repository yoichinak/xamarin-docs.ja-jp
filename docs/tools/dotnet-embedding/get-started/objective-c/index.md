---
title: "Objective C の概要"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 832ad2e6d462b39df7fa4a45739e97f7a7d6e5b5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-objective-c"></a>Objective C の概要

これは、サポートされているすべてのプラットフォームの基本についての内容を含む Objective C の作業の開始ページです。


## <a name="requirements"></a>必要条件

Objective C で .NET の埋め込みを使用するには、実行している Mac を必要があります。

* macOS 10.12 (Sierra) 以降
* Xcode 8.3.2 以降
* [モノラル 5.0](http://www.mono-project.com/download/)

インストールすることができます[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)を編集し、c# コードをコンパイルします。


メモ:

* 以前のバージョンの macOS、Xcode、およびモノラル_可能性があります_機能しますが、テスト、サポートされていません。
* Windows では、コードの生成を行うことができますが、Xcode がインストールされている Mac コンピューターでコンパイルすることはのみ


## <a name="installation"></a>インストール

次の手順がダウンロードして、mac 上の .NET の埋め込みをインストールするには

* [パッケージ](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [リリース ノート](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

別の方法としてはビルドから行うことが、 [git リポジトリ](https://github.com/mono/Embeddinator-4000/tree/objc)を参照してください、[の原因となった](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)手順についてはドキュメントです。

インストーラーでは、ベース標準のパッケージのインストーラーです。

![インストーラーの紹介](images/install1.png)
![インストーラーの種類をインストールする](images/install2.png)
![インストーラーの概要](images/install3.png)

使用することができます、installer によってインストールされると、新しいターミナル セッションを開始した後、`objcgen`コマンド。
絶対パスを経由してツールを常に実行するそれ以外の場合:`/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`です。

## <a name="platforms"></a>プラットフォーム

Objective C は macOS、iOS、tvOS および watchOS; 用アプリケーションを作成する最もよく使用される言語です。これらのプラットフォームのすべての embeddinator サポートしています。 各プラットフォームで作業がいくつかの重要な違いを意味し、説明しています[ここ](~/tools/dotnet-embedding/objective-c/platforms.md)です。

### <a name="macos"></a>macOS

[MacOS アプリケーションを作成する](~/tools/dotnet-embedding/get-started/objective-c/macos.md)は、id、provisining プロファイル、シミュレーターおよびデバイスの設定と同様に、多くの追加の手順を含まないため最も簡単です。 IOS 用の 1 つ前に、macOS ドキュメントを開始することをお勧めしています。

### <a name="ios--tvos"></a>iOS / tvOS

既に設定されて、embeddinator を使用して作成する前に、iOS アプリケーションを開発することを確認してください。 [の指示に従って](~/tools/dotnet-embedding/get-started/objective-c/ios.md)既に正常にビルドしているコンピューターからの iOS アプリケーションの展開を前提としています。

TvOS のサポートは、iOS プロジェクトではなく Ide (Visual Studio と Xcode の両方) で tvOS プロジェクトのみを使用して、iOS のしくみに似ています。

> [!NOTE]
> 注: watchOS のサポートが将来のリリースで利用可能なされは iOS/tvOS とよく似ています。


## <a name="further-reading"></a>関連項目

* [Objective C に固有の機能の .NET の埋め込み](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C のベスト プラクティス](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)
* [ターゲット プラットフォーム](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>関連リンク

- [気象サンプル (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
