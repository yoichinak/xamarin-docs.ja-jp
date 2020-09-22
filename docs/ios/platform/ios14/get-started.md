---
title: IOS 14 を使ってみる
description: このドキュメントでは、Xamarin を使用して iOS 14 アプリをビルドするように設定する方法について説明します。 Xcode 12 をダウンロードし Visual Studio for Mac を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 0d721b4b-86bd-495a-8c0f-1f2f9fd6855e
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2020
ms.openlocfilehash: 1bf77ca71d5fda945c81ac3ac6b338eec0164a76
ms.sourcegitcommit: 0c45e3f810947e3d43223aa01bf3e43a0defca65
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90843619"
---
# <a name="get-started-with-ios-14"></a>IOS 14 を使ってみる

このドキュメントでは、iOS 14 用 Xcode 12 でリリースされた Api を呼び出す Xamarin アプリの構築を開始する方法について説明します。 プレビューを使用するには、macOS Catalina.properties 10.15.4 以降が必要です。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **Install Xcode 12** –登録済みの apple 開発者は、 [apple Developer Portal](https://developer.apple.com/download/) または **App Store**から最新バージョンの Xcode 12 をダウンロードしてインストールできます。

2. **Xcode 12 を実行** する– Xamarin に必要なツールがインストールされるため、Visual Studio for Mac または Visual Studio 2019 を更新して実行する前に Xcode 12 を実行します。

3. **Update Visual Studio for Mac と Visual Studio 2019** – Xamarin の最新の安定バージョンがあることを確認します。

4. OptionalIos **デバイスに ios 14 をインストール** する– Xcode 12 で導入された api を使用するアプリのデバイステストでは、登録済みの Apple 開発者は、デバイスにオペレーティングシステムを [ダウンロード](https://developer.apple.com/download) してインストールできます。 

   > [!TIP]
   > アプリで新しい Api を使用しない場合でも、最新の Xcode 12 Sdk を使用してビルドし、テストして、すべてが期待どおりに動作することを確認してください。 アプリが新しい Api を呼び出さない場合は、これらの新しい Sdk を使用して再コンパイルし、新しいオペレーティングシステムにまだアップグレードされていないデバイスでテストすることができます。
   >
   > Apple から最新のオペレーティングシステムリリースにデバイスをアップグレードして Xamarin アプリをテストする前に、次のことを確認してください。
   >
   > - オペレーティングシステムの更新プログラムについては、 [Apple のリリースノート](https://developer.apple.com/download/) を参照してください。

## <a name="related-links"></a>関連リンク

- [Xcode のダウンロード](https://developer.apple.com/download/)
- [Xamarin.iOS のリリース ノート](/xamarin/ios/release-notes/14/14.0)
- [Xcode 12 リリースノート](https://developer.apple.com/documentation/xcode-release-notes/xcode-12-release-notes)
