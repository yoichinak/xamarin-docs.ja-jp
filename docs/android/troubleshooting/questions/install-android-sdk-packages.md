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
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725061"
---
# <a name="which-android-sdk-packages-should-i-install"></a>インストールする必要がある Android SDK パッケージを教えてください

Android SDK をインストールしても、開発に必要な最小限のパッケージがすべて自動的に含まれるわけではありません。 開発者のニーズは個々に異なりますが、通常以下のパッケージは、Xamarin Android を使用した開発に必要です。

## <a name="tools"></a>ツール

SDK マネージャーの Tools フォルダーから、次の最新のツールをインストールします。

- Android SDK Tools
- Android SDK Platform-Tools
- Android SDK Build-Tools

## <a name="android-platforms"></a>Android Platform(s)

最小限のターゲットとして設定した Android バージョンの "SDK Platform" をインストールします。

次に例を示します。

- ターゲット API 23
- 最小 API 23

API 23 の SDK プラットフォームのみをインストールする必要があります

- ターゲット API 23
- 最小 API 15

API 15 と 23 の SDK プラットフォームをインストールする必要があります 最小とターゲットの間に API レベルをインストールする必要がないことにご注意ください (これらの API レベルに旧バージョンへの移植を行っている場合でも)。

## <a name="system-images"></a>システム イメージ

これらは、Google の既定の Android エミュレーターを使用する場合にのみ必要です。 詳細については、「[Android Emulator のセットアップ](~/android/get-started/installation/android-emulator/index.md)」をご覧ください。

## <a name="extras"></a>Extras
通常、Android SDK Extras は必要ありません。ただし、お客様のユース ケースでは必要な場合もあるため、留意しておく必要はあります。
