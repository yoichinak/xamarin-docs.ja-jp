---
title: インストールする必要がある Android SDK パッケージを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: 04a07d5b7f37222515136e5592f31a4583b02fe3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61157365"
---
# <a name="which-android-sdk-packages-should-i-install"></a>インストールする必要がある Android SDK パッケージを教えてください

Android SDK をインストールすると、開発の最低限必要なパッケージを自動的に含まれていません。 個々 の開発者が異なる必要があるときに、次のパッケージは、一般に Xamarin.Android での開発に必要。

## <a name="tools"></a>ツール

SDK マネージャーの [ツール] フォルダから、最新のツールをインストールします。

- Android SDK Tools
- Android SDK プラットフォーム ツール
- Android SDK ビルド ツール

## <a name="android-platforms"></a>Android プラットフォーム

最小値およびターゲットとして設定した Android のバージョンには、「SDK プラットフォーム」をインストールします。 

次に例を示します。 

- ターゲット API 23
- 最小 API 23

のみ API 23 の SDK プラットフォームをインストールする必要があります。

- ターゲット API 23
- 最小 API 15

API 15、23 の SDK プラットフォームをインストールする必要があります。 (これらの API レベルにバックポートである) 場合でも、最小値とターゲット間の API レベルをインストールする必要はありませんに注意してください。

## <a name="system-images"></a>システムのイメージ

これらはのみ必要になる Google からの標準の Android エミュレーターを使用するかどうか。 詳細については、次を参照してください[Android Emulator のセットアップ。](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Extras
Android SDK Extras 通常必要はありません。ユース ケースに応じて必要な場合があるために、それらを認識すると便利です。

## <a name="further-reading"></a>関連項目
次のガイドでは、これらのオプションについて説明し、SDK を別のパッケージについて詳細に説明がマネージャーには使用できません。[Android SDK Manager のセットアップ ガイド](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

