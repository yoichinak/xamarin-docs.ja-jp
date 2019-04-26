---
title: Visual Studio 2019 で Xamarin をインストールします。
description: このドキュメントでは、Visual Studio 2019 で Xamarin をインストールする方法について説明します。 要件、インストール プロセス、インストールの確認について説明します。
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: 5d9f91300194eb45c5f5f3c52403660cf4898a19
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61271590"
---
# <a name="installing-xamarin-in-visual-studio-2019"></a>Visual Studio 2019 で Xamarin をインストールします。

<a name="requirements" />

開始する前に[システム要件](~/cross-platform/get-started/requirements.md)を確認します。

## <a name="installation"></a>インストール

[!include[](~/cross-platform/includes/install-xamarin-windows.md)]

Visual Studio 2019 でをクリックして、Xamarin がインストールされていることを確認します、**ヘルプ**メニュー。 Xamarin がインストールされている場合は、以下のスクリーン ショットのように、**Xamarin** のメニュー項目が表示されます。

![Xamarin のヘルプ メニューのメニュー項目](windows-images/12-xamarin-menu-item.png "Xamarin のヘルプ メニューのメニュー項目")

**[ヘルプ]、[Microsoft Visual Studio のバージョン情報]** の順にクリックし、インストールされている製品のリストをスクロールして、Xamarin がインストールされているかどうかを確認することもできます。

![Visual Studio 2019 に製品の画面がインストールされている](windows-images/13-xamarin-is-installed.png "Visual Studio 2019 に製品の画面がインストールされています。")

バージョン情報を見つける方法の詳細については、「[Where can I find my version information and logs?](~/cross-platform/troubleshooting/questions/version-logs.md)」 (バージョン情報とログはどこにありますか?) を参照してください

## <a name="next-steps"></a>次の手順

Visual Studio 2019 で Xamarin をインストール、アプリのコードの記述を開始することができますが、構築して、シミュレーター、エミュレーター、およびデバイスにアプリを展開するための追加のセットアップは必要です。 インストールを完了し、クロス プラットフォームのアプリの構築を開始するには、以下のガイドを参照してください。

### <a name="ios"></a>iOS

詳細については、「[Installing Xamarin.iOS on Windows](~/ios/get-started/installation/windows/index.md)」(Windows への Xamarin.iOS のインストール) ガイドを参照してください。 

1. [Visual Studio for Mac をインストールする](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Mac ビルド ホストへの Visual Studio の接続](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [iOS 開発者のセットアップ](~/ios/get-started/installation/device-provisioning/index.md) - デバイスでアプリケーションを実行するために必要
5. [リモートの iOS シミュレーター](~/tools/ios-simulator/index.md)
6. [Xamarin.iOS for Visual Studio の概要](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

詳細については、「[Installing Xamarin.Android on Windows](~/android/get-started/installation/windows.md)」(Windows への Xamarin.Android のインストール) ガイドを参照してください。

1. [Xamarin.Android の構成](~/android/get-started/installation/windows.md#configuration)
2. [Xamarin Android SDK Manager の使用](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK エミュレーター](~/android/get-started/installation/android-emulator/index.md)
4. [開発用のデバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)
