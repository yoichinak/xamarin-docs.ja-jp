---
title: Xamarin.Forms の要件
description: Xamarin.Forms のプラットフォームと開発システムの要件
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/19/2018
ms.openlocfilehash: ce3f2bcf6acc36239fc431bb7f5edece15d2e139
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms の要件

_Xamarin.Forms のプラットフォームと開発システムの要件_

プラットフォーム全体で適用されるインストール方法とセットアップ方法の概要については、[インストール](~/cross-platform/get-started/installation/index.md)記事を参照してください。

## <a name="target-platforms"></a>ターゲット プラットフォーム

Xamarin.Forms アプリケーションは次のオペレーティング システム用として記述できます。

-  iOS 8 以上
-  Android 4.0.3 (API 15) 以上 ([詳細](#android))
-  Windows 10 ユニバーサル Windows プラットフォーム ([詳細](#windows10))
-  *Windows 8.1 / Windows Phone 8.1 WinRT (非推奨)*
-  
  *Windows Phone 8 Silverlight (非推奨)*

[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)と[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)に関する知識が開発者にあることを前提としています。

<a name="android" />

### <a name="android"></a>Android

最新の Android SDK Tools と Android API プラットフォームをインストールしておく必要があります。 [Android SDK Manager](~/android/get-started/installation/android-sdk.md) を利用して最新版に更新できます。

また、Android プロジェクトのターゲット/コンパイル バージョンを*インストールされている最新のプラットフォームを使用するように*設定する**必要があります**。 ただし、Android 4.0.3 以降を使用するデバイスを引き続きサポートできるように、最小バージョンを API 15 に設定できます。 値は**プロジェクト オプション**で設定されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**[プロジェクト オプション]、[アプリケーション]、[アプリケーション プロパティ]**

![](installation-images/options-android-vs-sml.png "Visual Studio の Android 編集オプション")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**[ビルド]、[全般]**

![](installation-images/options-general-sml.png "[ビルド]、[全般]")

**[ビルド]、[Android アプリケーション]**

![](installation-images/options-android-sml.png "[ビルド]、[Android アプリケーション]")

-----

<a name="windows10" />

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ソリューションが macOS で作成されるとき、Windows 10 UWP プロジェクトは追加されません。 これらのプロジェクトを既存のソリューションに追加する方法については、「[Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md)」(Windows プロジェクトの設定) をご覧ください。

## <a name="development-system-requirements"></a>開発システムの要件

Xamarin.Forms アプリは macOS と Windows で開発できます。 ただし、アプリの Windows 版を作成するには Windows と Visual Studio が必要になります。

## <a name="mac-system-requirements"></a>Mac のシステム要件

Visual Studio for Mac to を使用し、OS X El Capitan (10.11) 以降で Xamarin.Forms アプリを開発できます。 iOS アプリを開発する場合、少なくとも iOS 10 SDK と Xcode 8 をインストールすることをお勧めします。

> [!NOTE]
>  Windows アプリを macOS で開発することはできません。

## <a name="windows-system-requirements"></a>Windows のシステム要件

iOS と Android 向けの Xamarin.Forms アプリは Xamarin 開発に対応しているあらゆる Windows インストールでビルドできます。 Visual Studio 2017 以降を Windows 7 以上で実行する必要があります。 iOS 開発には、ネットワークに接続した Mac が必要です。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

UWP 用の Xamarin.Forms アプリの開発に必要なもの:

* Windows 10 (Fall Creators Update を推奨)

* Visual Studio 2017

* [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

UWP プロジェクトは、Visual Studio 2015 と Visual Studio 2017 で作成された Xamarin.Forms ソリューションに含まれています。
[ユニバーサル Windows プラットフォーム (UWP) アプリ](~/xamarin-forms/platform/windows/installation/index.md)を既存の Xamarin.Forms ソリューションに追加することもできます。
