---
title: IOS 12、12、tvOS と watchOS 5 の概要
description: このドキュメントでは、最大 12 のビルド iOS、tvOS 12、および Xamarin を使った watchOS 5 アプリ設定を取得する方法について説明します。 これには、Xcode の 10 をダウンロードし、Mac と Visual Studio 2017 の Visual Studio を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615175"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>IOS 12、12、tvOS と watchOS 5 の概要

![[プレビュー]](~/media/shared/preview.png)

> [!WARNING]
> 12 の iOS、tvOS 12、および Xcode 10 と共に配布 watchOS 5 Sdk の Xamarin のサポートは現在プレビューのバグが含まれている機能ではないことを意味を完了して変わる可能性がありますにです。 実験目的でのみ使用します。

このドキュメントでは、Xcode 10 でリリースされている Api を呼び出す Xamarin アプリをビルドするよう設定を取得する方法について説明します。 これには、Xcode の 10 をダウンロードし、Mac と Visual Studio 2017 の Visual Studio を更新する方法について説明します。

## <a name="download-and-install"></a>ダウンロードとインストール

1. **Xcode の 10 の最新のベータ版をインストール**– Apple の登録されている開発者はダウンロードして、Xcode 10 からの最新バージョンをインストール、 [Apple Developer Portal](https://developer.apple.com/download/)します。

2. **Xcode の 10 を実行して**– ツール、Xamarin の更新といくつかのインストール過程で、for Mac または Visual Studio 2017、Visual Studio を実行する前に、Xcode 10 実行が必要です。

3. **Mac と Visual Studio 2017 用 Visual Studio の更新プログラム**– の指示に従って、[リリース ブログ](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)Xamarin プレビューをインストールします。

4. _(省略可能)_ **、IOS デバイスで iOS の最新のベータ版をインストール**-デバイスの登録済みの Apple の開発者が Xcode 10 導入された Api を使用するアプリのテストの[ダウンロード](https://developer.apple.com/download)し、最新のインストール自分のデバイスでの開発者のベータ版です。

   > [!TIP]
   > アプリでは、新しい Api を使用しない場合でも、最新の Xcode 10 Sdk を構築し、期待どおりに動作するすべてのものかどうかを確認することをテストすることを確認します。 アプリは、新しい Api を呼び出す場合は、これらの新しい Sdk と再コンパイルし、新しいオペレーティング システムにまだアップグレードされていないデバイスでテストできます。
   >
   > Xamarin アプリをテストする Apple からのリリースに最新のオペレーティング システム、デバイスをアップグレードする前に必ずします。
   >
   > - 読み取り[Apple のリリース ノート](https://developer.apple.com/download/)オペレーティング システムを更新します。
   > - Xamarin のプレビューを読み取る[ブログの投稿をリリース](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)します。

## <a name="related-links"></a>関連リンク

- [Xcode 10 をダウンロードします。](https://developer.apple.com/download/)
- Xamarin プレビュー[ブログの投稿をリリース](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
