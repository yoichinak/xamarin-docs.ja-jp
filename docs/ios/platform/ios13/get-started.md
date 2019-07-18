---
title: IOS 13、iPadOS 13、13、tvOS、watchOS 6 の概要します。
description: このドキュメントでは、最大ビルド iOS 13、iPadOS 13、13、tvOS、および Xamarin を使った watchOS 6 アプリ設定を取得する方法について説明します。 Xcode 11 をダウンロードして for mac。 Visual Studio を更新する方法について説明します
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: 554cddf5717401f912cab38c78a6af17659a0cf7
ms.sourcegitcommit: 8ecfa339d0f3e7687977bfe4fc96448942690183
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67558690"
---
# <a name="get-started-with-ios-13"></a>IOS 13 を概要します。

![プレビュー機能](~/media/shared/preview.png)

このドキュメントでは、Xcode の 11 でリリースされている Api を呼び出す iOS 13 の Xamarin アプリの構築を開始する方法について説明します。 MacOS 10.14.4 (Mojave)、プレビューを使用する必要がありますまたはそれ以降。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **Xcode 11 beta のインストール**– Apple の登録されている開発者はダウンロードして、最新のバージョンから Xcode 11 beta のインストール、 [Apple Developer Portal](https://developer.apple.com/download/)または**App Store**します。

2. **Xcode 11 beta が実行**– ツール、Xamarin の更新といくつかのインストール過程で、for Mac、Visual Studio を実行する前に、Xcode 11 実行が必要です。

3. Visual studio for Mac では、次のように選択します**Visual Studio > 更新プログラムを確認しています.。** を選択、 **Xcode 11 プレビュー**チャネルを開いて、使用可能な更新プログラムをインストールします。

4. Visual studio for Mac では、次のように選択します。 **Visual Studio > 設定 > プロジェクト > SDK の場所 > Apple**選択**Xcode beta.app**します。

5. (省略可能)このプレビューを使用して評価する場合_Xcode 11 beta 3_、リンクを有効にする必要があります。 プロジェクトを右クリックしに移動します**オプション > iOS ビルド > リンカーの動作**リンカーの動作の値を設定および**リンク フレームワーク Sdk のみ**します。 この回避策は今後のプレビューで不要になります。

6. (省略可能) **、IOS デバイスで iOS 13 をインストール**-デバイスの登録済み Apple の開発者は、Xcode 11 で導入された Api を使用するアプリのテストの[ダウンロード](https://developer.apple.com/download)して自分のデバイスで、オペレーティング システムをインストールします。 IOS では、ベータ版であるためには、プライマリ デバイスにインストールするように注意します。

   > [!TIP]
   > アプリでは、新しい Api を使用しない場合でも、最新の Xcode 11 Sdk を構築し、期待どおりに動作するすべてのものかどうかを確認することをテストすることを確認します。 アプリは、新しい Api を呼び出す場合は、これらの新しい Sdk と再コンパイルし、新しいオペレーティング システムにまだアップグレードされていないデバイスでテストできます。
   >
   > Xamarin アプリをテストする Apple からのリリースに最新のオペレーティング システム、デバイスをアップグレードする前に必ずします。
   >
   > - 読み取り[Apple のリリース ノート](https://developer.apple.com/download/)オペレーティング システムを更新します。

## <a name="related-links"></a>関連リンク

- [Xcode をダウンロードします。](https://developer.apple.com/download/)
- [Xamarin.iOS リリース ノートのプレビュー](/xamarin/ios/release-notes/12/12.99)
