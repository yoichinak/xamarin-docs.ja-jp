---
title: インストールする必要がある Android SDK パッケージを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
ms.openlocfilehash: 24c70c2e869f59091a1519af6d1165dbea9cc467
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725061"
---
# <a name="which-android-sdk-packages-should-i-install"></a>インストールする必要がある Android SDK パッケージを教えてください

Android SDK をインストールすると、開発に必要な最小限のパッケージが自動的に含まれなくなります。 個々の開発者のニーズは異なりますが、Xamarin を使用した開発には通常、次のパッケージが必要になります。

## <a name="tools"></a>ツール

SDK manager の Tools フォルダーから最新のツールをインストールします。

- Android SDK Tools
- Android SDK プラットフォーム-ツール
- Android SDK ビルドツール

## <a name="android-platforms"></a>Android プラットフォーム

最小 & ターゲットとして設定した Android バージョンの "SDK Platform" をインストールします。

例 :

- ターゲット API 23
- 最小 API 23

API 23 の SDK プラットフォームのみをインストールする必要があります

- ターゲット API 23
- 最小 API 15

API 15 および23の SDK プラットフォームをインストールする必要があります。 最小とターゲットの間に API レベルをインストールする必要がないことに注意してください (これらの API レベルにバック移植している場合でも)。

## <a name="system-images"></a>システムイメージ

これらは、既定の Android エミュレーターを Google から使用する場合にのみ必要です。 詳細については、「 [Android Emulator セットアップ](~/android/get-started/installation/android-emulator/index.md)」を参照してください。

## <a name="extras"></a>Extras
通常、Android SDK エクストラは必須ではありません。ただし、ユースケースによっては必要になる場合があるため、注意が必要です。
