---
title: Xamarin での tvOS サポートのインストール
description: この記事では、Xcode 9 と Xamarin. iOS 11 の tvOS のサポートについて説明し、Xamarin を使用して tvOS アプリを開発するための設定方法について簡単な手順を示します。
ms.prod: xamarin
ms.assetid: 0819DC93-A46B-49DC-A566-8E27CAE1B829
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: b814fdb05b3eaba6feb47f6969519a5b99fcdad4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283630"
---
# <a name="installing-tvos-support-in-xamarin"></a>Xamarin での tvOS サポートのインストール

> [!TIP]
> IOS 12 と tvOS 12 の Xamarin のプレビューサポートを試すには、「 [ios 12 ファーストステップガイド」](~/ios/platform/introduction-to-ios12/get-started.md)を参照してください。

Apple は Apple TV 4K と tvOS 11 をリリースしました。 Apple TV platform は開発者にオープンであり、充実したイマーシブアプリを作成し、Apple TV の組み込みの App Store でリリースできます。

Xamarin. iOS 11 以降では、Apple の Xcode 9 に付属する tvOS 11 SDK をサポートしています。

- [Xamarin. iOS のリリースノート](https://docs.microsoft.com/xamarin/ios/release-notes/)
- [Xcode のリリースノート](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html#//apple_ref/doc/uid/TP40001051-CH1-SW876)

## <a name="installation"></a>インストール

Xamarin を使用して tvOS アプリをビルドするには:

1. **最新の Xcode をインストールする**–[最新バージョンの Xcode をダウンロード](https://developer.apple.com/xcode/download/)してインストールします。 Xcode がインストールされていない場合、Xamarin アプリをビルドすることはできません。 
2. **Xcode を実行**する– Xcode をインストールした後、Visual Studio for Mac を更新して実行する前に1回開始します。 Xcode は、Xamarin に必要ないくつかのツールをインストールします。
3. 最新の安定した xamarin リリース更新プログラムを最新の安定した[xamarin リリース](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)に**インストール**します。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin を使用した tvOS 用アプリの構築 (ビデオ)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
