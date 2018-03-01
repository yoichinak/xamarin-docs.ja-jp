---
title: "Android SDK パッケージをインストールする必要がありますか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/02/2018
ms.openlocfilehash: e2b0736a9ccc4dde5c1dcdf2d99f527247040560
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="which-android-sdk-packages-should-i-install"></a>Android SDK パッケージをインストールする必要がありますか。

Android SDK をインストールすると、すべての最低限必要なパッケージを開発するために自動的に含まれません。 個々 の開発者は異なる必要がある、次のパッケージは Xamarin.Android での開発用必要通常になります。

## <a name="tools"></a>ツール

SDK manager で [ツール] フォルダから最新のツールをインストールします。

- Android SDK ツール
- Android SDK プラットフォーム ツール
- Android SDK ビルド ツール

## <a name="android-platforms"></a>Android プラットフォーム

最小値および対象として設定した Android バージョン「SDK プラットフォーム」をインストールします。 

次に例を示します。

- ターゲット API 23
- 最小 API 23

のみ API 23 のプラットフォームの SDK をインストールする必要があります。

- ターゲット API 23
- 最小 API 15

API 15、23 のプラットフォームを SDK をインストールする必要があります。 注 (場合でもこれらの API レベルをバックポート)、最小値とターゲット間の API レベルをインストールする必要はありません。

## <a name="system-images"></a>システムのイメージ
これらはのみ必要になる Google からの既定の Android エミュレーターを使用するかどうか。 

- [既定のエミュレーターを構成する方法](~/android/get-started/installation/android-emulator/index.md)

- [既定のエミュレーターを高速化する方法](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Extras
Android SDK Extras は通常必要ありません。ユース ケースによって必要な場合がありますので、気付かないうちにすると便利です。

## <a name="further-reading"></a>関連項目
次のガイドは、これらのオプションをカバーし、SDK は他のパッケージについての詳細にマネージャーが使用可能な: [Android SDK Manager のセットアップ ガイド](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

