---
title: "アーキテクチャの変更"
description: "IOS 11 の新機能の検証"
ms.topic: article
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 0c4b0571d9254edba15177d5f98fd6718f1c6bcb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="architecture-changes"></a>アーキテクチャの変更

_IOS 11 の新機能の検証_

Ios 11 の注意する必要がある最大の変更点の 1 つは、アプリについては、「32 ビット サポートの廃止[Apple の](https://developer.apple.com/news/?id=06282017b)プレス リリースします。 すべての新しいアプリと既存のアプリの更新プログラムは、64 ビットをサポートする必要があります。 32 ビット アプリ**が起動しない**iOS 11 にします。

For Mac を Visual Studio でアプリを更新するには、次の手順を使用します。

1. 開くには、プロジェクト名を右クリックして**プロジェクト オプション**です。
2. 参照、 **iOS ビルド**タブです。
3. 設定、**サポートされているアーキテクチャ**ドロップダウンを**x86_64**の**デバッグ | iPhoneSimulator**と**リリース | iPhoneSimulator**:

    ![シミュレーター アーキテクチャ ドロップダウンを変更します。](architecture-changes-images/image1.png)

4. IOS デバイスの構成を変更して**デバッグ | iPhone**設定と、**サポートされているアーキテクチャ**ドロップダウンを**ARM64**:

    ![デバイスのアーキテクチャ ドロップダウンを変更します。](architecture-changes-images/image2.png)

5. 構成を変更して**リリース | iPhone**設定と、**サポートされているアーキテクチャ**ドロップダウンを**ARM64**です。

32 ビットと 64 ビット アーキテクチャの詳細については、次を参照してください。、 [32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64.md#ios)ガイドです。

## <a name="related-links"></a>関連リンク

- [新機能 iOS 11 (Apple)](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [更新、iOS 用アプリ 11 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)
