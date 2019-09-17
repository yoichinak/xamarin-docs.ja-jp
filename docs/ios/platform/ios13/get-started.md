---
title: IOS 13、iPadOS 13、tvOS 13、および watchOS 6 を使ってみる
description: このドキュメントでは、Xamarin を使用して iOS 13、iPadOS 13、tvOS 13、および watchOS 6 アプリをビルドするように設定する方法について説明します。 Xcode 11 をダウンロードし Visual Studio for Mac を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: 9dc6f234c4bc14c3644d953eef0d2e0f397436e5
ms.sourcegitcommit: 61a35d0643eb3bf5adb8f8831da54771d8dde626
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033075"
---
# <a name="get-started-with-ios-13"></a>IOS 13 を使ってみる

![プレビュー機能](~/media/shared/preview.png)

このドキュメントでは、iOS 13 用に Xcode 11 でリリースされた Api を呼び出す Xamarin アプリの構築を開始する方法について説明します。 プレビューを使用するには、macOS 10.14.4 (Mojave) 以降が必要です。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **Install Xcode 11 beta** –登録済みの apple 開発者は、 [apple Developer Portal](https://developer.apple.com/download/)または**App Store**から最新バージョンの Xcode 11 beta をダウンロードしてインストールできます。

2. **Run Xcode 11 beta** – Xamarin に必要なツールがインストールされるため、Visual Studio for Mac を更新して実行する前に Xcode 11 を実行します。

3. Visual Studio for Mac で、[Visual Studio] を選択し**て [更新プログラムの確認**] を >、 **Xcode 11 のプレビュー**チャネルを選択して、利用可能な更新プログラムをインストールします。 このチャンネルが表示されない場合は、 **Visual Studio > アカウント**からアカウントにログインしていることを確認してください...

4. Visual Studio for Mac で、[ **Visual Studio] > [基本設定] を選択し > プロジェクト > SDK の場所 > Apple**を選択し、 **[Xcode-beta]** を選択します。

5. Optional_Xcode 11 beta 3_を使用してこのプレビューを評価する場合は、リンクを有効にする必要があります。 プロジェクトを右クリックし、**オプション > IOS ビルド > リンカーの動作** の順に移動して、リンカーの動作 の値を **フレームワーク Sdk のみをリンク**する に設定します。 この回避策は、今後のプレビューでは必要ありません。

6. OptionalIos**デバイスに ios 13 をインストール**する– Xcode 11 で導入された api を使用するアプリのデバイステストでは、登録されている Apple 開発者は、デバイスにオペレーティングシステムを[ダウンロード](https://developer.apple.com/download)してインストールできます。 IOS はベータ版なので、プライマリデバイスにインストールすることをお勧めします。

   > [!TIP]
   > アプリで新しい Api を使用しない場合でも、最新の Xcode 11 Sdk を使用してビルドし、テストして、すべてが期待どおりに動作することを確認してください。 アプリが新しい Api を呼び出さない場合は、これらの新しい Sdk を使用して再コンパイルし、新しいオペレーティングシステムにまだアップグレードされていないデバイスでテストすることができます。
   >
   > Apple から最新のオペレーティングシステムリリースにデバイスをアップグレードして Xamarin アプリをテストする前に、次のことを確認してください。
   >
   > - オペレーティングシステムの更新プログラムについては、 [Apple のリリースノート](https://developer.apple.com/download/)を参照してください。

## <a name="related-links"></a>関連リンク

- [Xcode のダウンロード](https://developer.apple.com/download/)
- [Xamarin iOS preview リリースノート](/xamarin/ios/release-notes/12/12.99)
