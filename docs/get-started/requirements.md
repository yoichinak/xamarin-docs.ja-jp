---
title: Xamarin.Forms の要件
description: Xamarin.Forms のプラットフォームと開発システムの要件
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
ms.openlocfilehash: 46a72534fba7a45323a82ad121e5844410472812
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "72584347"
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms の要件

_Xamarin.Forms のプラットフォームと開発システムの要件_

プラットフォーム全体で適用されるインストール方法とセットアップ方法の概要については、[インストール](installation/index.md)記事を参照してください。

## <a name="target-platforms"></a>ターゲット プラットフォーム

Xamarin.Forms アプリケーションは次のオペレーティング システム用として記述できます。

- iOS 9 以降
- Android 4.4 (API 19) 以上 ([詳細](#android))
- Windows 10 ユニバーサル Windows プラットフォーム ([詳細](#windows10))

ただし、最小 API として Android 5.0 (API 21) をお勧めします。 これにより、ほとんどの android デバイスを対象としながら、すべての Android サポートライブラリとの完全な互換性が確保されます。

開発者が[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)について理解していることを前提としています。

### <a name="additional-platform-support"></a>その他のプラットフォームのサポート

これらのプラットフォームの状態は、[Xamarin.Forms GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support) で確認できます。

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="android"></a>Android

最新の Android SDK Tools と Android API プラットフォームをインストールしておく必要があります。 [Android SDK Manager](~/android/get-started/installation/android-sdk.md) を利用して最新版に更新できます。

また、Android プロジェクトのターゲット/コンパイル バージョンを*インストールされている最新のプラットフォームを使用するように*設定する**必要があります**。 ただし、Android 4.4 以降を使用するデバイスを引き続きサポートできるように、最小バージョンを API 19 に設定できます。 値は**プロジェクト オプション**で設定されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**[プロジェクト オプション]、[アプリケーション]、[アプリケーション プロパティ]**

![Visual Studio の Android ビルドオプション](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**[ビルド]、[全般]**

![最新のターゲットフレームワークを選択します](requirements-images/options-general-sml.png)

**[ビルド]、[Android アプリケーション]**

![アプリの最小とターゲットの Android バージョンを選択します](requirements-images/options-android-sml.png)

-----

## <a name="development-system-requirements"></a>開発システムの要件

Xamarin.Forms アプリは macOS と Windows で開発できます。 ただし、アプリの Windows 版を作成するには Windows と Visual Studio が必要になります。

## <a name="mac-system-requirements"></a>Mac のシステム要件

Visual Studio for Mac を使用して、macOS High (10.13) 以降で Xamarin. Forms アプリを開発できます。 IOS アプリを開発するには、最新バージョンの Xcode、iOS、macOS を使用することをお勧めします。 特定のバージョンの要件については、最新の[Xamarin. iOS のリリースノート](/xamarin/ios/release-notes/)を参照してください。

> [!NOTE]
> Windows アプリを macOS で開発することはできません。

## <a name="windows-system-requirements"></a>Windows のシステム要件

iOS と Android 向けの Xamarin.Forms アプリは Xamarin 開発に対応しているあらゆる Windows インストールでビルドできます。 現在のプラットフォーム機能を完全にサポートするには、最新バージョンの Visual Studio を使用します。 

Xcode の最新バージョンと Apple によって指定された最小バージョンの macOS を使用した iOS 開発では、ネットワークに接続された Mac が必要です。

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

UWP 用の Xamarin.Forms アプリの開発に必要なもの:

- Windows 10 (推奨されている最新バージョン、更新プログラムの最小更新プログラム)

- Visual Studio 2019 をお勧めします

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

既存の Xamarin.Forms ソリューションに、いつでも[ユニバーサル Windows プラットフォーム (UWP) アプリを追加](~/xamarin-forms/platform/windows/installation/index.md)できます。

## <a name="deprecated-platforms"></a>非推奨のプラットフォーム

これらのプラットフォームは、Xamarin. Forms 3.0 以降を使用している場合はサポートされません。

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
