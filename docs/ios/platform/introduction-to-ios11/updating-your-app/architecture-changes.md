---
title: IOS 11 でのアーキテクチャの変更点
description: このドキュメントでは、iOS 11 での32ビットアプリの非推奨について説明します。 また、64ビットアーキテクチャを対象とするようにアプリケーションを更新する方法についても説明します。
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 421e228a0021b4e4cdaf5da4c5776f9477477c28
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032096"
---
# <a name="architecture-changes-in-ios-11"></a>IOS 11 でのアーキテクチャの変更点

IOS 11 で最も重要な変更点の1つは、 [Apple の](https://developer.apple.com/news/?id=06282017b)プレスリリースで詳しく説明されているように、アプリの32ビットサポートが廃止されたことです。 すべての新しいアプリと既存のアプリの更新は、64ビットをサポートしている必要があります。 32ビットアプリは、iOS 11 で**は起動されません**。

Visual Studio for Mac でアプリを更新するには、次の手順に従います。

1. プロジェクト名を右クリックして、[**プロジェクトオプション]** を開きます。
2. **[IOS ビルド]** タブに移動します。
3. **Debug | リストで iphonesimulator**と**Release | リストで iphonesimulator**の **[サポートされているアーキテクチャ]** ドロップダウンを**x86_64**に設定します。

    ![変更シミュレーターのアーキテクチャドロップダウン](architecture-changes-images/image1.png)

4. IOS デバイスの場合は、構成を**Debug | iPhone**に変更し、**サポートされているアーキテクチャ**のドロップダウンを**ARM64**に設定します。

    ![デバイスアーキテクチャの変更ドロップダウン](architecture-changes-images/image2.png)

5. 構成を**Release | iPhone**に変更し、**サポートされているアーキテクチャ**のドロップダウンを**ARM64**に設定します。

32ビットアーキテクチャと64ビットアーキテクチャの詳細については、 [32/64 ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#ios)に関するガイドを参照してください。

## <a name="related-links"></a>関連リンク

- [IOS 11 (Apple) の新機能](https://developer.apple.com/ios/)
- [更新された App Store 製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) 用のアプリの更新 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)
