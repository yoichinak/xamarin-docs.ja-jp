---
title: 'App Store への送信中にエラーが発生しました: "無効なバンドル-ビットコードへの埋め込みは許可されていないオプションが送信で検出されました"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: c6a03fa3327e81f577f85b05a033d9c97495eb2e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031073"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>App Store への送信中にエラーが発生しました: "無効なバンドル-ビットコードへの埋め込みは許可されていないオプションが送信で検出されました"

watchOS apps と tvOS apps は、アプリストアに送信されるときに bitcode を_必要_とします。 Xcode 8.3 以前を使用して watchOS および tvOS アプリをビルドして送信する場合、App Store にアップロードしようとすると、(電子メール通知を使用して) 次のエラーが発生する可能性があります。

>バンドルが無効です。ビットコードに埋め込むことが許可されていないオプションが送信時に検出されたため、アプリを処理できません。 Xcode で提供されているツールチェーンを使用してアプリをビルドしていない可能性があります。

この問題を解決するには、Xcode 9 と最新バージョンの Xamarin. iOS を使用してアプリケーションをビルドします。
