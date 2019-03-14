---
title: システム要件
description: このドキュメントでは、Mac と Windows の両方のコンピューターで Xamarin を使用してアプリをビルドする場合のシステム要件をリストします。 インストール手順にもリンクしています。
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 486c5c57961e897eae59df66b216a9078d5df517
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667991"
---
# <a name="system-requirements"></a>システム要件

Xamarin 製品は、iOS または Android をターゲットとする際に Apple および Google のプラットフォーム SDK に依存するため、システム要件を合わせる必要があります。 このページでは、Xamarin プラットフォームのシステムの互換性と、推奨される開発環境および SDK バージョンについて概説します。

ソフトウェアと必要な SDK の取得の詳細については、「[インストール手順](#installation-instructions)」を参照してください。

## <a name="development-environments"></a>開発環境

この表は、さまざまな開発ツールとオペレーティング システムを組み合わせてビルドできるプラットフォームを示しています。

[!include[](~/cross-platform/includes/development-environment.md)]

> [!NOTE]
> Windows コンピューターの iOS 用に開発するには、リモート コンパイルおよびデバッグのために、[ネットワークから Mac コンピューターにアクセスできる](~/ios/get-started/installation/windows/connecting-to-mac/index.md)必要があります。 これは、Mac コンピューター上の Windows VM 内で Visual Studio が実行されている場合にも当てはまります。

## <a name="macos-requirements"></a>macOS の要件

Xamarin の開発に Mac コンピューターを使用するには、次のソフトウェア/SDK バージョンが必要です。 オペレーティング システムのバージョンを確認し、[Xamarin インストーラー](#installation-instructions)の指示に従ってください。

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode は [developer.apple.com](https://developer.apple.com/xcode/download/) または Mac App Store でインストール (および更新) できます。

### <a name="testing--debugging-on-macos"></a>macOS でのテストとデバッグ

- Xamarin モバイル アプリケーションを USB 経由で物理デバイスに展開し、テストおよびデバッグを行うことができます (Apple Watch アプリは対応する iPhone に最初に展開されます)。
- Xamarin.Mac アプリは、開発用コンピューターで直接テストすることができます。

[!include[](~/cross-platform/includes/macos-testing.md)]

> [!WARNING]
> 今度の Xamarin.Mac 4.8 リリースでは、macOS 10.9 以降のみをサポートします。
> 以前のバージョンの Xamarin.Mac では macOS 10.7 以降をサポートしていましたが、これらの古い macOS バージョンは TLS 1.2 をサポートするための十分な TLS インフラストラクチャがありませんでした。 macOS 10.7 または macOS 10.8 をターゲットにするには、Xamarin.Mac 4.6 以前を使用してください。

## <a name="windows-requirements"></a>Windows の要件

Xamarin の開発に Windows コンピューターを使用するには、次のソフトウェア/SDK バージョンが必要です。
オペレーティング システムのバージョンを確認してください (さらに、*Express* バージョンの Visual Studio を使用していないことを確認します。使用している場合は、*Community* エディションへの更新を検討してください)。
Visual Studio 2017 インストーラーには、Xamarin を自動的にインストールするオプションが含まれています (**.NET によるモバイル開発**)。

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
> - Xamarin for Visual Studio では Visual Studio 2017 (Community、Professional、および Enterprise) がサポートされています。
> - ユニバーサル Windows プラットフォーム (UWP) 用に Xamarin.Forms アプリを開発するには、Visual Studio 2017 がインストールされている Windows 10 が必要です。

### <a name="testing--debugging-on-windows"></a>Windows でのテストとデバッグ

Xamarin モバイル アプリケーションを USB 経由またはワイヤレスで物理デバイスに展開し、テストおよびデバッグを行うことができます (iOS デバイスは、Visual Studio を実行しているコンピューターではなく、Mac コンピューターに接続する必要があります)。

[!include[](~/cross-platform/includes/windows-testing.md)]

## <a name="installation-instructions"></a>インストール手順

macOS 用の最新の Xamarin リリースは、[xamarin.com/download](http://xamarin.com/download) からダウンロードできます。 Windows の場合は、[Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) のインストール手順に従ってください。

現在の製品バージョンの完全なリストは、[現在のリリースのページ](https://developer.xamarin.com/releases/current/)で確認できます。 このページには、ベータおよびアルファ チャネルの個々の製品バージョンの概要 (およびリリース ノートへのリンク) も示されています。

各プラットフォームの特定の[インストール](~/get-started/installation/index.md)手順については、以下を参照してください。

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

[Xamarin.Forms の要件とサポートされているプラットフォーム](~/get-started/requirements.md)に関する追加情報もあります。

## <a name="related-links"></a>関連リンク

- [Xamarin のダウンロード](https://visualstudio.microsoft.com/xamarin/)
- [現在のリリース](https://developer.xamarin.com/releases/current/)
