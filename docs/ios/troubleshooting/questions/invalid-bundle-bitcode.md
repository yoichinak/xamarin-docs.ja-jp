---
title: App Store に送信するときに、エラー:「無効なバンドル - オプションをビットコードに埋め込むことはできませんが、送信で検出された」
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 867ad29abfa6a38971b60ac9ebf181905949dafd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421730"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>App Store に送信するときに、エラー:「無効なバンドル - オプションをビットコードに埋め込むことはできませんが、送信で検出された」

watchOS アプリおよび tvOS アプリ_必要_bitcode が App Store に送信するときにします。 (電子メール通知) を使用して、次のエラーが発生するときに、ビルドして、Xcode 8.3 以前を使用して、watchOS、tvOS のアプリの提出、App Store にアップロードしようとしています。

>無効なバンドル - 送信でビットコードに埋め込まれることができませんオプションが検出されたために、アプリを処理できません。 Xcode で提供されるツール チェーンでアプリをビルドしていないこと可能性があります。

Xcode 9 と最新バージョンの Xamarin.iOS アプリケーションを構築するこの問題を解決することです。
