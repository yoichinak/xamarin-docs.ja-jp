---
title: IOS 12、tvOS 12、watchOS 5 を使ってみる
description: このドキュメントでは、Xamarin を使用して iOS 12、tvOS 12、watchOS 5 のアプリをビルドするように設定する方法について説明します。 Xcode 10 をダウンロードし、Visual Studio for Mac と Visual Studio 2017 を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2018
ms.openlocfilehash: 1db249a9e07f178461bcb052508d08f54ecea121
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032021"
---
# <a name="get-started-with-ios-12-tvos-12-and-watchos-5"></a>IOS 12、tvOS 12、watchOS 5 を使ってみる

このドキュメントでは、iOS 12、tvOS 12、および watchOS 5 用の Xcode 10 でリリースされた Api を呼び出す Xamarin アプリの構築を開始する方法について説明します。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **Xcode 10 をインストール**する–登録済みの apple 開発者は、 [apple Developer Portal](https://developer.apple.com/download/)または**App Store**から最新バージョンの Xcode 10 をダウンロードしてインストールできます。

2. **Xcode 10 を実行**する– Xamarin に必要なツールがインストールされるため、Visual Studio for Mac または Visual Studio 2017 を更新して実行する前に Xcode 10 を実行します。

3. **Update Visual Studio for Mac と Visual Studio 2017** – Xamarin の最新の安定バージョンがあることを確認します。

4. _(省略可能)_ ios**デバイスに ios 12 をインストールする**–

   Xcode 10 で導入された Api を使用するアプリのデバイステストでは、登録済みの Apple 開発者は、自分のデバイスにオペレーティングシステムを[ダウンロード](https://developer.apple.com/download)してインストールできます。

   > [!TIP]
   > アプリで新しい Api を使用しない場合でも、最新の Xcode 10 Sdk を使用してビルドし、テストして、すべてが期待どおりに動作することを確認してください。 アプリが新しい Api を呼び出さない場合は、これらの新しい Sdk を使用して再コンパイルし、新しいオペレーティングシステムにまだアップグレードされていないデバイスでテストすることができます。
   >
   > Apple から最新のオペレーティングシステムリリースにデバイスをアップグレードして Xamarin アプリをテストする前に、次のことを確認してください。
   >
   > - オペレーティングシステムの更新プログラムについては、 [Apple のリリースノート](https://developer.apple.com/download/)を参照してください。
   > - Xamarin preview リリースの[ブログ記事](https://releases.xamarin.com/preview-release-xcode-10-beta-6/)をご覧ください。

## <a name="related-links"></a>関連リンク

- [Xcode のダウンロード](https://developer.apple.com/download/)
