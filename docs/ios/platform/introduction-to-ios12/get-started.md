---
title: IOS 12、12、tvOS と watchOS 5 の概要します。
description: このドキュメントでは、最大 12 のビルド iOS、tvOS 12、および Xamarin を使った watchOS 5 アプリ設定を取得する方法について説明します。 これには、Xcode の 10 をダウンロードし、Mac と Visual Studio 2017 の Visual Studio を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2018
ms.openlocfilehash: 77589d0d644c366fc0feacd874929c7456b4ae30
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61398799"
---
# <a name="get-started-with-ios-12-tvos-12-and-watchos-5"></a>IOS 12、12、tvOS と watchOS 5 の概要します。

このドキュメントでは、12 を iOS、tvOS 12、および watchOS 5 Xcode 10 でリリースされている Api を呼び出す Xamarin アプリの構築を開始する方法について説明します。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **Xcode の 10 をインストール**– Apple の登録されている開発者はダウンロードして、Xcode 10 からの最新バージョンをインストール、 [Apple Developer Portal](https://developer.apple.com/download/)または**App Store**します。

2. **Xcode の 10 を実行して**– ツール、Xamarin の更新といくつかのインストール過程で、for Mac または Visual Studio 2017、Visual Studio を実行する前に、Xcode 10 実行が必要です。

3. **Mac と Visual Studio 2017 用 Visual Studio の更新プログラム**– Xamarin の最新の安定バージョンがあることを確認します。

4. _(省略可能)_ **、IOS デバイスで iOS 12 をインストール**–

   登録済みの Apple の開発者ができるデバイスは、Xcode 10 で導入された Api を使用するアプリのテスト、[ダウンロード](https://developer.apple.com/download)して自分のデバイスで、オペレーティング システムをインストールします。

   > [!TIP]
   > アプリでは、新しい Api を使用しない場合でも、最新の Xcode 10 Sdk を構築し、期待どおりに動作するすべてのものかどうかを確認することをテストすることを確認します。 アプリは、新しい Api を呼び出す場合は、これらの新しい Sdk と再コンパイルし、新しいオペレーティング システムにまだアップグレードされていないデバイスでテストできます。
   >
   > Xamarin アプリをテストする Apple からのリリースに最新のオペレーティング システム、デバイスをアップグレードする前に必ずします。
   >
   > - 読み取り[Apple のリリース ノート](https://developer.apple.com/download/)オペレーティング システムを更新します。
   > - Xamarin のプレビューを読み取る[ブログの投稿をリリース](https://releases.xamarin.com/preview-release-xcode-10-beta-6/)します。

## <a name="related-links"></a>関連リンク

- [Xcode 10 をダウンロードします。](https://developer.apple.com/download/)
