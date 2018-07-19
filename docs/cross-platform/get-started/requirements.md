---
title: システム要件
description: このドキュメントでは、Mac と Windows の両方のコンピューターで Xamarin を使用してアプリをビルドする場合のシステム要件をリストします。 インストール手順にもリンクしています。
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
ms.openlocfilehash: 04db2fe4e3385c55ecf653b002b909f16e99a101
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780725"
---
# <a name="system-requirements"></a>システム要件

_Xamarin を使用するための前提条件_

Xamarin 製品は、iOS または Android をターゲットとする際に Apple および Google のプラットフォーム SDK に依存するため、システム要件を合わせる必要があります。 このページでは、Xamarin プラットフォームのシステムの互換性と、推奨される開発環境および SDK バージョンについて概説します。

- [開発環境](#devenv)
- [macOS の要件](#mac)
- [Windows の要件](#windows)

ソフトウェアと必要な SDK の取得の詳細については、「[インストール手順](#install)」を参照してください。

<a name="devenv" />

## <a name="development-environments"></a>開発環境

この表は、さまざまな開発ツールとオペレーティング システムを組み合わせてビルドできるプラットフォームを示しています。

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> Windows コンピューターの iOS 用に開発するには、リモート コンパイルおよびデバッグのために、[ネットワークから Mac コンピューターにアクセスできる](~/ios/get-started/installation/windows/connecting-to-mac/index.md)必要があります。 これは、Mac コンピューター上の Windows VM 内で Visual Studio が実行されている場合にも当てはまります。

<a name="mac" />

## <a name="macos-requirements"></a>macOS の要件

Xamarin の開発に Mac コンピューターを使用するには、次のソフトウェア/SDK バージョンが必要です。 オペレーティング システムのバージョンを確認し、[Xamarin インストーラー](#install)の指示に従ってください。

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode は [developer.apple.com](https://developer.apple.com/xcode/download/) または Mac App Store でインストール (および更新) できます。

### <a name="testing--debugging-on-macos"></a>macOS でのテストとデバッグ

Xamarin モバイル アプリケーションを USB 経由で物理デバイスに展開し、テストおよびデバッグを行うことができます (Xamarin.Mac アプリは開発用コンピューターで直接テストできます。Apple Watch アプリは対応する iPhone に最初に展開されます)。

[!include[](~/cross-platform/includes/macos-testing.md)]


<a name="windows" />

## <a name="windows-requirements"></a>Windows の要件

Xamarin の開発に Windows コンピューターを使用するには、次のソフトウェア/SDK バージョンが必要です。
オペレーティング システムのバージョンを確認してください (さらに、*Express* バージョンの Visual Studio を使用していないことを確認します。使用している場合は、*Community* エディションへの更新を検討してください)。
Visual Studio 2015 と 2017 のインストーラーには、Xamarin を自動的にインストールするオプションが含まれます。

[!include[](~/cross-platform/includes/windows-requirements.md)]


> [!NOTE]
>
>* Xamarin for Visual Studio では Visual Studio 2015 または 2017 (Community、Professional、および Enterprise) がサポートされています。
>
>* ユニバーサル Windows プラットフォーム (UWP) 用に Xamarin.Forms アプリを開発するには、Visual Studio 2015 または 2017 がインストールされている Windows 10 が必要です。


### <a name="testing--debugging-on-windows"></a>Windows でのテストとデバッグ

Xamarin モバイル アプリケーションを USB 経由で物理デバイスに展開し、テストおよびデバッグを行うことができます (iOS デバイスは、Visual Studio を実行しているコンピューターではなく、Mac コンピューターに接続する必要があります)。

[!include[](~/cross-platform/includes/windows-testing.md)]


> [!NOTE]
>
>* [Windows Phone 8.1 エミュレーターのダウンロード](https://www.microsoft.com/download/details.aspx?id=43719)。
>* Windows Phone 10 エミュレーターは Visual Studio 2015 UWP SDK に含まれています。

<a name="install" />

## <a name="installation-instructions"></a>インストールの指示:

macOS 用の最新の Xamarin リリースは、[xamarin.com/download](http://xamarin.com/download) からダウンロードできます。 Windows の場合は、[Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) のインストール手順に従ってください。

現在の製品バージョンの完全なリストは、[現在のリリースのページ](http://developer.xamarin.com/releases/current/)で確認できます。 このページには、ベータおよびアルファ チャネルの個々の製品バージョンの概要 (およびリリース ノートへのリンク) も示されています。

各プラットフォームの特定の[インストール](~/cross-platform/get-started/installation/index.md)手順については、以下を参照してください。

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

[Xamarin.Forms の要件とサポートされているプラットフォーム](~/xamarin-forms/get-started/installation.md)に関する追加情報もあります。


## <a name="related-links"></a>関連リンク

- [Xamarin のダウンロード](https://xamarin.com/download/)
- [現在のリリース](https://developer.xamarin.com/releases/current/)
