---
title: IOS 13、iPadOS 13、tvOS 13、および watchOS 6 を使ってみる
description: このドキュメントでは、Xamarin を使用して iOS 13、iPadOS 13、tvOS 13、および watchOS 6 アプリをビルドするように設定する方法について説明します。 Xcode 11 をダウンロードし Visual Studio for Mac を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: 1f190ef2f99e71c8cf21680f9902caccf3454450
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206325"
---
# <a name="get-started-with-ios-13"></a>IOS 13 を使ってみる

このドキュメントでは、iOS 13 用に Xcode 11 でリリースされた Api を呼び出す Xamarin アプリの構築を開始する方法について説明します。 プレビューを使用するには、macOS 10.14.4 (Mojave) 以降が必要です。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **Install Xcode 11** –登録済みの apple 開発者は、 [apple Developer Portal](https://developer.apple.com/download/)または**App Store**から最新バージョンの Xcode 11 をダウンロードしてインストールできます。

2. **実行 Xcode 11** – Xamarin に必要なツールがインストールされるため、Visual Studio for Mac を更新して実行する前に Xcode 11 を実行します。

3. Visual Studio for Mac で、[Visual Studio] を選択し**て [更新プログラムの確認**] を >、**安定**したチャネルを選択して、利用可能な更新プログラムをインストールします。

4. OptionalIos**デバイスに ios 13 をインストール**する– Xcode 11 で導入された api を使用するアプリのデバイステストでは、登録されている Apple 開発者は、デバイスにオペレーティングシステムを[ダウンロード](https://developer.apple.com/download)してインストールできます。 

   > [!TIP]
   > アプリで新しい Api を使用しない場合でも、最新の Xcode 11 Sdk を使用してビルドし、テストして、すべてが期待どおりに動作することを確認してください。 アプリが新しい Api を呼び出さない場合は、これらの新しい Sdk を使用して再コンパイルし、新しいオペレーティングシステムにまだアップグレードされていないデバイスでテストすることができます。
   >
   > Apple から最新のオペレーティングシステムリリースにデバイスをアップグレードして Xamarin アプリをテストする前に、次のことを確認してください。
   >
   > - オペレーティングシステムの更新プログラムについては、 [Apple のリリースノート](https://developer.apple.com/download/)を参照してください。

## <a name="related-links"></a>関連リンク

- [Xcode のダウンロード](https://developer.apple.com/download/)
- [Xamarin.iOS のリリース ノート](/xamarin/ios/release-notes/13/13.0)
