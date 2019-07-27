---
title: MacOS Mojave を使ってみる
description: このドキュメントでは、Xamarin. Mac で macOS Mojave アプリをビルドするように設定する方法について説明します。 Xcode 10 をダウンロードし Visual Studio for Mac を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 369133f49566625dd677dc077ed8ff6a0bb0b4db
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511668"
---
# <a name="get-started-with-macos-mojave"></a>MacOS Mojave を使ってみる

このドキュメントでは、Xamarin. Mac で macOS Mojave アプリをビルドするように設定する方法について説明します。 Xcode 10 をダウンロードし Visual Studio for Mac を更新する方法について説明します。

## <a name="download-and-install"></a>ダウンロードしてインストールする

1. **最新の Xcode 10 beta をインストール**する–登録されている apple 開発者は、 [apple Developer ポータル](https://developer.apple.com/download/)から最新バージョンの Xcode 10 をダウンロードしてインストールできます。

2. **Xcode 10 を実行**する– Visual Studio for Mac を更新して実行する前に Xcode 10 を実行します。Xamarin に必要ないくつかのツールがインストールされます。

3. **Visual Studio for Mac の更新**– [Xamarin 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/mac/xamarin.mac_5/xamarin.mac_5.0.md)以降で Visual Studio for Mac の最新の安定バージョンを使用します。

4. _(省略可能)_ **Mac に MacOS Mojave をインストールする**–

   > [!TIP]
   > アプリで新しい macOS Mojave Api を使用していない場合でも、macOS Mojave SDK を使用してビルドし、テストして、すべてが期待どおりに動作することを確認してください。 アプリが新しい Api を呼び出さない場合は、macOS Mojave SDK を使用して再コンパイルし、Mac のオペレーティングシステムをアップグレードせずにテストすることができます。
   >
   > Mac を macOS Mojave にアップグレードしてから、新しい macOS Mojave Api を呼び出す Xamarin. Mac アプリケーションをビルドしてテストする前に、次のようにします。
   >
   > - オペレーティングシステムの更新プログラムについては、 [Apple のリリースノート](https://developer.apple.com/download/)を参照してください。

## <a name="related-links"></a>関連リンク

- [Xcode 10 のダウンロード](https://developer.apple.com/download/)
- [Xamarin. Mac 5.0 リリースノート](https://docs.microsoft.com/xamarin/mac/release-notes/5/5.0/)
