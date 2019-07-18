---
title: IOS 11 のアーキテクチャの変更
description: このドキュメントでは、iOS 11 で 32 ビット アプリの非推奨について説明します。 これには、64 ビット アーキテクチャをターゲットにアプリケーションを更新する方法について説明します。
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: b7d1df6ed2a8bd480025681ebcbe48acd7592564
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169351"
---
# <a name="architecture-changes-in-ios-11"></a>IOS 11 のアーキテクチャの変更

IOS 11 を意識する必要がある最大の変更点の 1 つで詳述するように、アプリの 32 ビットのサポートの廃止[Apple の](https://developer.apple.com/news/?id=06282017b)プレス リリースします。 すべての新しいアプリや既存のアプリに更新プログラムは、64 ビットをサポートする必要があります。 32 ビット アプリ**は起動せず**iOS 11 でします。

For Mac を Visual Studio でアプリを更新するには、次の手順を使用します。

1. 開くプロジェクト名を右クリックして**プロジェクト オプション**します。
2. 参照、 **iOS ビルド**タブ。
3. 設定、**サポートされているアーキテクチャ**ドロップダウンを**x86_64**の**デバッグ | iPhoneSimulator**と**リリース | iPhoneSimulator**:

    ![ドロップダウン リストのシミュレーター アーキテクチャを変更します。](architecture-changes-images/image1.png)

4. IOS デバイスの構成を変更する**デバッグ | iPhone**設定と、**サポートされているアーキテクチャ**ドロップダウンを**ARM64**:

    ![デバイスのアーキテクチャ ドロップダウン リストを変更します。](architecture-changes-images/image2.png)

5. 構成を変更する**リリース | iPhone**設定と、**サポートされているアーキテクチャ**ドロップダウンを**ARM64**します。

32 ビットおよび 64 ビット アーキテクチャの詳細については、次を参照してください。、 [32/64 ビットのプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#ios)ガイド。

## <a name="related-links"></a>関連リンク

- [IOS 11 (Apple) で新します。](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品のページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (ビデオ) 用のアプリの更新](https://developer.apple.com/videos/play/wwdc2017/204/)
