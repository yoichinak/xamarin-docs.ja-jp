---
title: 目的の概要-C
description: このドキュメントでは、.NET 埋め込みの使用を開始する方法について説明します。 要件、NuGet からの .NET 埋め込みのインストール、およびサポートされているプラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: b9c97e871791b633c65e9d374edfe446874567e9
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71106098"
---
# <a name="getting-started-with-objective-c"></a>目的の概要-C

これは、サポートされているすべてのプラットフォームの基本について説明する、目的の C の概要ページです。

## <a name="requirements"></a>必要条件

.NET 埋め込みを使用して C を使用するには、次のような Mac が実行されている必要があります。

- macOS 10.12 (シエラレオネ) 以降
- Xcode 8.3.2 以降
- [Mono 5.0](https://www.mono-project.com/download/)

[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)をインストールして、 C#コードを編集およびコンパイルできます。

> [!NOTE]
>
> - 以前のバージョンの macOS、Xcode、Mono は動作する_場合があり_ますが、テストされており、サポートされていません。
> - コード生成は Windows で実行できますが、Xcode がインストールされている Mac コンピューターでのみコンパイルできます。

## <a name="installing-net-embedding-from-nuget"></a>NuGet からの .NET 埋め込みのインストール

次の[手順](~/tools/dotnet-embedding/get-started/install/install.md)に従って、プロジェクトの .Net 埋め込みをインストールして構成します。

コマンドの呼び出しのサンプルは、 [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md)と[iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md)のファーストステップガイドに記載されています。

## <a name="platforms"></a>プラットフォーム

目標-C は、macOS、iOS、tvOS、watchOS 用のアプリケーションを記述するために最もよく使用される言語であり、.NET 埋め込みはこれらすべてのプラットフォームをサポートしています。 各プラットフォームを使用する場合、重要な違いがいくつかあります。[これらについては、ここで説明](~/tools/dotnet-embedding/objective-c/platforms.md)します。

### <a name="macos"></a>macOS

[MacOS アプリケーションの作成](~/tools/dotnet-embedding/get-started/objective-c/macos.md)は、id の設定、プロファイル、シミュレーター、デバイスの設定など、追加の手順が多く含まれていないため、最も簡単に作成できます。 IOS の場合は、まず macOS ドキュメントから始めることをお勧めします。

### <a name="ios--tvos"></a>iOS/tvOS

.NET 埋め込みを使用して作成する前に、iOS アプリケーションを開発するようにセットアップしていることを確認してください。 [次の手順](~/tools/dotnet-embedding/get-started/objective-c/ios.md)は、既にコンピューターから iOS アプリケーションを正常にビルドし、展開していることを前提としています。

TvOS のサポートは、ios プロジェクトではなく Ide (Visual Studio と Xcode の両方) で tvOS プロジェクトを使用するだけで、iOS の動作に似ています。

> [!NOTE]
> WatchOS のサポートは今後のリリースで提供される予定であり、iOS/tvOS とよく似ています。

## <a name="further-reading"></a>関連項目

- [目的に固有の .NET 埋め込み機能-C](~/tools/dotnet-embedding/objective-c/index.md)
- [目標のベストプラクティス-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
- [.NET 埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
- [オープンソースプロジェクトへの貢献](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [エラーコードと説明](~/tools/dotnet-embedding/errors.md)
- [ターゲットプラットフォーム](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>関連リンク

- [Weather サンプル (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
