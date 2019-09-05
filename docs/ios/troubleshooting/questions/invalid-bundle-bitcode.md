---
title: App Store への送信中にエラーが発生した場合:"無効なバンドル-bitcode への埋め込みを許可されていないオプションが送信時に検出されました"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 84244e0c4c24a8ca6ac71a79de963bedf5c1ee68
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292534"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>App Store への送信中にエラーが発生した場合:"無効なバンドル-bitcode への埋め込みを許可されていないオプションが送信時に検出されました"

watchOS apps と tvOS apps は、アプリストアに送信されるときに bitcode を_必要_とします。 Xcode 8.3 以前を使用して watchOS および tvOS アプリをビルドして送信する場合、App Store にアップロードしようとすると、(電子メール通知を使用して) 次のエラーが発生する可能性があります。

>バンドルが無効です。ビットコードに埋め込むことが許可されていないオプションが送信時に検出されたため、アプリを処理できません。 Xcode で提供されているツールチェーンを使用してアプリをビルドしていない可能性があります。

この問題を解決するには、Xcode 9 と最新バージョンの Xamarin. iOS を使用してアプリケーションをビルドします。
