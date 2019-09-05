---
title: Xamarin.Forms の要件
description: Xamarin.Forms のプラットフォームと開発システムの要件
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/01/2019
ms.openlocfilehash: a79f22fa96c4c34d2a9704e9aa3be3825895a15b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291594"
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms の要件

_Xamarin.Forms のプラットフォームと開発システムの要件_

プラットフォーム全体で適用されるインストール方法とセットアップ方法の概要については、[インストール](installation/index.md)記事を参照してください。

## <a name="target-platforms"></a>ターゲット プラットフォーム

Xamarin.Forms アプリケーションは次のオペレーティング システム用として記述できます。

- iOS 8 以上
- Android[5.0 (API](#android)21) 以上 (詳細)
- Windows 10 ユニバーサル Windows プラットフォーム ([詳細](#windows10))

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

Visual Studio for Mac を使用して、macOS High (10.13) 以降で Xamarin. Forms アプリを開発できます。 IOS アプリを開発するには、少なくとも iOS 10 SDK と Xcode 9 をインストールすることをお勧めします。

> [!NOTE]
> Windows アプリを macOS で開発することはできません。

## <a name="windows-system-requirements"></a>Windows のシステム要件

iOS と Android 向けの Xamarin.Forms アプリは Xamarin 開発に対応しているあらゆる Windows インストールでビルドできます。 Visual Studio 2017 以降を Windows 7 以上で実行する必要があります。 iOS 開発には、ネットワークに接続した Mac が必要です。

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

UWP 用の Xamarin.Forms アプリの開発に必要なもの:

- Windows 10 (推奨されている最新バージョン、更新プログラムの最小更新プログラム)

- Visual Studio 2019 を推奨します (Visual Studio 2017 バージョン15.8 以上)

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

既存の Xamarin.Forms ソリューションに、いつでも[ユニバーサル Windows プラットフォーム (UWP) アプリを追加](~/xamarin-forms/platform/windows/installation/index.md)できます。

## <a name="deprecated-platforms"></a>非推奨のプラットフォーム

これらのプラットフォームは、Xamarin. Forms 3.0 以降を使用している場合はサポートされません。

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
