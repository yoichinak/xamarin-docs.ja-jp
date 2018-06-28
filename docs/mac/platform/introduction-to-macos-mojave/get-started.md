---
title: MacOS Mojave の概要
description: このドキュメントでは、ビルド macOS Xamarin.Mac で Mojave アプリのように設定を取得する方法について説明します。 Xcode 10 をダウンロードおよび for mac Visual Studio を更新する方法についても説明します。
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067345"
---
# <a name="getting-started-with-macos-mojave"></a>MacOS Mojave の概要

![[プレビュー]](~/media/shared/preview.png)

> [!WARNING]
> Xamarin の macOS Mojave サポートは、現在プレビューでは、つまりそのバグを含めることは、機能豊富ではありませんし変更することがあります。
> 実験のみを使用します。

> [!NOTE]
> 詳細については、読み取り、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin のプレビュー リリースです。

このドキュメントでは、ビルド macOS Xamarin.Mac で Mojave アプリのように設定を取得する方法について説明します。 Xcode 10 をダウンロードおよび for mac Visual Studio を更新する方法についても説明します。

## <a name="download-and-install"></a>ダウンロードとインストール

1. **最新の Xcode 10 ベータ版をインストール**-登録されている Apple の開発者はダウンロードして、Xcode 10 からの最新バージョンをインストール、 [Apple 開発者ポータル](https://developer.apple.com/download/)です。

   > [!NOTE]
   > MacOS Mojave SDK は Xcode 10 と共に配布されます。

2. **Xcode 10 を実行して**– Xcode 10 を更新して、for Mac; Visual Studio を実行する前に実行 Xamarin を必要とするいくつかのツールをインストールします。

3. **Mac 用の Visual Studio の更新**– の指示に従って、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin preview をインストールします。

4. _(省略可能)_ **、Mac 上の最新の macOS Mojave ベータ版のインストール**– 新たに導入された macOS Mojave Api では、登録されている Apple の開発者を使用して Xamarin.Mac アプリをテストする[ダウンロード](https://developer.apple.com/download/)をインストールし、最新 macOS Mojave 開発者のベータ版です。

   > [!TIP]
   > アプリがすべて新しい macOS Mojave Api を使用しない場合でも必ず macOS (Xcode 10 にインストールされた) Mojave SDK でビルドし、あらゆるものが期待どおりにいるかどうかを確認するテストをします。 アプリでは、新しい Api を呼び出すが場合、macOS Mojave SDK を再コンパイルし、Mac のオペレーティング システムをアップグレードせずにテストできます。

   > [!IMPORTANT]
   > ご使用の Mac を macOS 新しい macOS Mojave Api を呼び出す Xamarin.Mac アプリケーションをビルドおよびテスト Mojave: アップグレードする前に
   > - 読み取り[Apple のリリース ノート](https://developer.apple.com/download/)オペレーティング システムの更新用です。
   > - 読み取り、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin のプレビュー リリースです。 この最初のプレビューに新しい macOS Mojave AppKit Api (濃いモード) などのバインドが含まれていないことに注意してください。

## <a name="related-links"></a>関連リンク

- [Xcode 10 をダウンロードします。](https://developer.apple.com/download/)
- Xamarin プレビュー[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)
