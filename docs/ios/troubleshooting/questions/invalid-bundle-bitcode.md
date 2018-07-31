---
title: App Store に送信するときに、エラー:「無効なバンドル - オプションをビットコードに埋め込むことはできませんが、送信で検出された」
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 393c1ed81c68d21b610781dfe09de97969e031d1
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350925"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>App Store に送信するときに、エラー:「無効なバンドル - オプションをビットコードに埋め込むことはできませんが、送信で検出された」

watchOS アプリおよび tvOS アプリ_必要_bitcode が App Store に送信するときにします。 (電子メール通知) を使用して、次のエラーが発生するときに、ビルドして、Xcode 8.3 以前を使用して、watchOS、tvOS のアプリの提出、App Store にアップロードしようとしています。

>無効なバンドル - 送信でビットコードに埋め込まれることができませんオプションが検出されたために、アプリを処理できません。 Xcode で提供されるツール チェーンでアプリをビルドしていないこと可能性があります。

Xcode 9 と最新バージョンの Xamarin.iOS アプリケーションを構築するこの問題を解決することです。
