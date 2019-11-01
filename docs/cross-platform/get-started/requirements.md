---
title: システム要件
description: このドキュメントでは、Mac と Windows の両方のコンピューターで Xamarin を使用してアプリをビルドする場合のシステム要件をリストします。 インストール手順にもリンクしています。
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: davidortinau
ms.author: daortin
ms.date: 10/16/2019
ms.openlocfilehash: 093369010d9327cd2b19fdac09f77697532bfb75
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016440"
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
> Xamarin.Mac 4.8 では、macOS 10.9 (Mavericks) 以降のみがサポートされます。
> 以前のバージョンの Xamarin.Mac では macOS 10.7 以降をサポートしていましたが、これらの古い macOS バージョンは TLS 1.2 をサポートするための十分な TLS インフラストラクチャがありませんでした。 macOS 10.7 または macOS 10.8 をターゲットにするには、Xamarin.Mac 4.6 以前を使用してください。

## <a name="windows-requirements"></a>Windows の要件

Xamarin の開発に Windows コンピューターを使用するには、次のソフトウェア/SDK バージョンが必要です。
オペレーティング システムのバージョンを確認してください (さらに、*Express* バージョンの Visual Studio を使用していないことを確認します。使用している場合は、*Community* エディションへの更新を検討してください)。
Visual Studio 2019 および Visual Studio 2017 のインストーラーには、Xamarin を自動的にインストールするオプションが含まれています ( **.NET によるモバイル開発**ワークロード)。

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
>
> - Xamarin for Visual Studio では、Visual Studio 2019 または Visual Studio 2017 (Community、Professional、および Enterprise) がサポートされています。
> - 最新バージョンの Android および iOS SDK を使用するには、最新バージョンの Visual Studio が必要です。 特定のバージョン要件については、[Xamarin.Android のリリース ノート](/xamarin/android/release-notes/)と [Xamarin.iOS のリリース ノート](/xamarin/ios/release-notes/)を参照してください。
> - ユニバーサル Windows プラットフォーム (UWP) 用に Xamarin.Forms アプリを開発するには、Visual Studio 2017 がインストールされている Windows 10 が必要です。 Visual Studio 2019 をお勧めします。

### <a name="testing--debugging-on-windows"></a>Windows でのテストとデバッグ

Xamarin モバイル アプリケーションを USB 経由またはワイヤレスで物理デバイスに展開し、テストおよびデバッグを行うことができます (iOS デバイスは、Visual Studio を実行しているコンピューターではなく、Mac コンピューターに接続する必要があります)。

[!include[](~/cross-platform/includes/windows-testing.md)]

## <a name="installation-instructions"></a>インストール手順

macOS 用の最新の Xamarin リリースは、[Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation) でダウンロードできます。 Windows の場合は、[Visual Studio のインストール手順](https://docs.microsoft.com/visualstudio/install/install-visual-studio)に従ってください。

現在の製品リリースの完全なリストは、[新着情報のページ](~/whats-new/index.yml)で確認できます。 このページは、リリース ノートにもリンクしています。

各プラットフォームの特定の[インストール](~/get-started/installation/index.md)手順については、以下を参照してください。

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

[Xamarin.Forms の要件とサポートされているプラットフォーム](~/get-started/requirements.md)に関する追加情報もあります。

## <a name="related-links"></a>関連リンク

- [Xamarin のダウンロード](https://visualstudio.microsoft.com/xamarin/)
- [Xamarin.Forms のリリース ノート](/xamarin/xamarin-forms/release-notes/)
- [Xamarin.Android のリリース ノート](/xamarin/android/release-notes/)
- [Xamarin.iOS のリリース ノート](/xamarin/ios/release-notes/)
