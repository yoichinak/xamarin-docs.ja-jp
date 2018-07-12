---
title: MacOS Mojave の概要
description: このドキュメントでは、macOS Xamarin.Mac を使って Mojave アプリのビルドするように設定を取得する方法について説明します。 Xcode の 10 をダウンロードして for mac。 Visual Studio を更新する方法について説明します
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831346"
---
# <a name="getting-started-with-macos-mojave"></a>MacOS Mojave の概要

![[プレビュー]](~/media/shared/preview.png)

> [!WARNING]
> Xamarin の macOS Mojave サポートは現在プレビュー段階で、これにより、バグを含めることができます、機能は完全なが変わる可能性があります。
> 実験目的でのみ使用します。

> [!NOTE]
> 詳細については、読み取り、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin のプレビュー リリース。

このドキュメントでは、macOS Xamarin.Mac を使って Mojave アプリのビルドするように設定を取得する方法について説明します。 Xcode の 10 をダウンロードして for mac。 Visual Studio を更新する方法について説明します

## <a name="download-and-install"></a>ダウンロードとインストール

1. **Xcode の 10 の最新のベータ版をインストール**– Apple の登録されている開発者はダウンロードして、Xcode 10 からの最新バージョンをインストール、 [Apple Developer Portal](https://developer.apple.com/download/)します。

   > [!NOTE]
   > MacOS Mojave SDK は Xcode 10 と共に配布されます。

2. **Xcode の 10 を実行して**– Xcode 10 を更新して、for Mac は Visual Studio を実行する前に実行 Xamarin を必要とするいくつかのツールがインストールされます。

3. **Mac 用 Visual Studio の更新プログラム**– の指示に従って、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin プレビューをインストールします。

4. _(省略可能)_ **、Mac 上で最新の macOS Mojave ベータ版をインストール**– Mojave Api、登録済みの Apple の開発者が新たに導入された macOS を使用して Xamarin.Mac アプリをテストする[ダウンロード](https://developer.apple.com/download/)をインストールし、最新の macOS Mojave 開発者 beta します。

   > [!TIP]
   > アプリでは、新しい macOS Mojave Api を使用しない場合でも、macOS (Xcode 10 と共にインストールされた) Mojave SDK でビルドし、期待どおりに動作するすべてのものかどうかを確認することをテストすることを確認します。 アプリは、新しい Api を呼び出す場合、macOS Mojave SDK で再コンパイルし、Mac のオペレーティング システムのアップグレードなしでテストできます。

   > [!IMPORTANT]
   > Mac を macOS ビルドし、新しい macOS Mojave Api を呼び出す Xamarin.Mac アプリケーションをテストする Mojave: アップグレードする前に
   > - 読み取り[Apple のリリース ノート](https://developer.apple.com/download/)のオペレーティング システムの更新。
   > - 読み取り、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin のプレビュー リリース。 この最初のプレビューに新しい macOS Mojave AppKit Api (ダーク モード) などのバインドが含まれていないことに注意してください。

## <a name="related-links"></a>関連リンク

- [Xcode 10 をダウンロードします。](https://developer.apple.com/download/)
- Xamarin プレビュー[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)
