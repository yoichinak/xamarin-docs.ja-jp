---
title: Xamarin.Forms サポートされているプラットフォーム
description: Xamarin.Forms のプラットフォームと開発システムの要件。
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cfb5c7ce11ca2eab5aedfd1b6f862a750fcff6e3
ms.sourcegitcommit: bda07768b51e6d24f897ccae16e152dc7a83effa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94595334"
---
# <a name="no-locxamarinforms-supported-platforms"></a>Xamarin.Forms サポートされているプラットフォーム

Xamarin.Forms アプリケーションは次のオペレーティング システム用として記述できます。

- iOS 9 以上。
- Android 4.4 (API 19) 以上 ([詳細](#android-platform-support))。 ただし、最小 API として Android 5.0 (API 21) をお勧めします。 これにより、ほとんどの Android デバイスを引き続きターゲットとしながら、すべての Android サポート ライブラリとの完全な互換性が確保されます。
- Windows 10 ユニバーサル Windows プラットフォーム: .NET Standard 2.0 サポート用のビルド 10.0.16299.0 以上。 ただし、ビルド 10.0.18362.0 以上をお勧めします。

iOS、Android、ユニバーサル Windows プラットフォーム (UWP) 用の Xamarin.Forms アプリは、Visual Studio で構築できます。 ただし、Xcode の最新バージョンと Apple によって指定された最小バージョンの macOS を使用した iOS 開発では、ネットワークに接続された Mac が必要です。 詳細については、「[Windows の要件](~/cross-platform/get-started/requirements.md#windows-requirements)」を参照してください。

iOS および Android 用の Xamarin.Forms アプリは Visual Studio for Mac で構築できます。 詳細については、「[macOS の要件](~/cross-platform/get-started/requirements.md#macos-requirements)」を参照してください。

> [!NOTE]
> Xamarin.Forms を使用してアプリを開発するには、[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) についての知識が必要です。

## <a name="additional-platform-support"></a>その他のプラットフォームのサポート

Xamarin.Forms では、iOS、Android、および Windows 以外に次のプラットフォームもサポートされています。

- Samsung Tizen
- macOS 10.13 以降
- GTK#
- WPF

これらのプラットフォームの状態は、[Xamarin.Forms GitHub プラットフォームのサポート Wiki で確認できます](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)。

## <a name="android-platform-support"></a>Android プラットフォームのサポート

最新の Android SDK Tools と Android API プラットフォームをインストールしておく必要があります。 [Android SDK Manager](~/android/get-started/installation/android-sdk.md) を利用して最新版に更新できます。

また、Android プロジェクトのターゲット/コンパイル バージョンを *インストールされている最新のプラットフォームを使用するように* 設定する **必要があります**。 ただし、Android 4.4 以降を使用するデバイスを引き続きサポートできるように、最小バージョンを API 19 に設定できます。 値は **プロジェクト オプション** で設定されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

**[プロジェクト オプション]、[アプリケーション]、[アプリケーション プロパティ]**

![Visual Studio の Android ビルド オプション](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

**[ビルド]、[全般]**

![最新のターゲット フレームワークを選択](requirements-images/options-general-sml.png)

**[ビルド]、[Android アプリケーション]**

![ご使用のアプリの最小およびターゲットの Android バージョンを選択](requirements-images/options-android-sml.png)

-----

## <a name="deprecated-platforms"></a>非推奨のプラットフォーム

これらのプラットフォームは、Xamarin.Forms 3.0 以降を使用する場合はサポートされません。

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
