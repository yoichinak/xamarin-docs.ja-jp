---
title: IOS 12、12、tvOS と watchOS 5 の概要
description: このドキュメントでは、最大ビルド iOS 12 と Xamarin で tvOS 12 のアプリ設定を取得する方法について説明します。 これには、Xcode の 10 をダウンロードし、Mac と Visual Studio 2017 の Visual Studio を更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 70f67f934c2503e6f6fa0d3bae1f37bcc1f6f0a4
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030653"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>IOS 12、12、tvOS と watchOS 5 の概要

![[プレビュー]](~/media/shared/preview.png)

> [!WARNING]
> Xamarin の iOS 12、tvOS 12、および watchOS 5 のサポートは現在プレビュー段階で、バグが含まれていることを意味でない機能は完全なし、変わる可能性があります。 実験目的でのみ使用します。

> [!NOTE]
> 詳細については、Xamarin のプレビューをお読みください。[ブログの投稿をリリース](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)します。

このドキュメントでは、最大 12 のビルド iOS、tvOS 12、および Xamarin を使った watchOS 5 アプリ設定を取得する方法について説明します。 これには、Xcode の 10 をダウンロードし、Mac と Visual Studio 2017 の Visual Studio を更新する方法について説明します。

## <a name="download-and-install"></a>ダウンロードとインストール

1. **Xcode の 10 の最新のベータ版をインストール**– Apple の登録されている開発者はダウンロードして、Xcode 10 からの最新バージョンをインストール、 [Apple Developer Portal](https://developer.apple.com/download/)します。

   > [!NOTE]
   > 12 の iOS、tvOS 12、および watchOS 5 Sdk は Xcode 10 で分散されます。

2. **Xcode の 10 を実行して**– Xcode 10 を更新し、Mac または Visual Studio 2017 の Visual Studio を実行する前に実行 Xamarin を必要とするいくつかのツールがインストールされます。

3. **Mac と Visual Studio 2017 用 Visual Studio の更新プログラム**– の指示に従って、[リリース ブログ](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)Xamarin プレビューをインストールします。

4. _(省略可能)_ **、IOS デバイスで iOS の最新のベータ版をインストール**– デバイスのアプリのテスト使用して新たに導入された iOS 12、tvOS、12 または watchOS 5、Api、Apple の登録済み開発者できる[ダウンロード](https://developer.apple.com/download)と自分のデバイスでは、12 の最新の iOS、tvOS、12 または watchOS 5 開発者のベータ版をインストールします。

   > [!TIP]
   > すべて 12 の新しい iOS、tvOS 12、または 5 watchOS アプリを使用しない場合でも Api を iOS 12 や 12、tvOS、watchOS を構築してください (Xcode 10 と共にインストールされた) 5 の SDK およびテストとして機能するすべてのものかどうかを確認することは想定します。 アプリは、新しい Api を呼び出す場合は、12 iOS、tvOS、12 または watchOS 5 recompile できます SDK し、新しいオペレーティング システムにまだアップグレードされていないデバイスでテストします。

   > [!IMPORTANT]
   > 12 の新しい iOS、tvOS、12 または watchOS 5 を呼び出す Xamarin アプリケーションをテストするには、12 を iOS、tvOS 12、または watchOS 5 に、デバイスをアップグレードする前に Api:
   > - 読み取り[Apple のリリース ノート](https://developer.apple.com/download/)のオペレーティング システムの更新。
   > - Xamarin のプレビューを読み取る[ブログの投稿をリリース](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)します。

## <a name="related-links"></a>関連リンク

- [Xcode 10 をダウンロードします。](https://developer.apple.com/download/)
- Xamarin プレビュー[ブログの投稿をリリース](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
