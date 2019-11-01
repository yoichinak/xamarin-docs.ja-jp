---
title: アプリを iOS 11 に更新する
description: このドキュメントでは、Xamarin で使用できる新機能について説明するさまざまなガイドにリンクしています。 ios 11 のリリースを使用しています。 たとえば、ビジュアルデザイン更新、アプリストアの変更、アプリアイコンの更新などです。
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 81d863b12aa7748acc6876929fb5d6361a20b507
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032117"
---
# <a name="updating-your-app-to-ios-11"></a>アプリを iOS 11 に更新する

IOS 11 では、Apple はアーキテクチャの更新、新しいビジュアルの変更、および更新された iTunes Connect プロセスを導入しました。 このガイドでは、これらの変更について詳しく説明します。 ios 11 用に更新されています。

## <a name="architecture-changesarchitecture-changesmd"></a>[アーキテクチャの変更点](architecture-changes.md)

IOS 11 で最も重要な変更点の1つは、 [Apple の](https://developer.apple.com/news/?id=06282017b)プレスリリースで詳しく説明されているように、アプリの32ビットサポートが廃止されたことです。

このガイドでは、64ビットのアプリを更新する手順について説明します。

## <a name="visual-design-updatesvisual-designmd"></a>[ビジュアル デザインの更新](visual-design.md)

IOS 11 では、ナビゲーションバー、検索バー、テーブルビューの更新など、新しいビジュアルの変更が導入されました。 さらに、余白と全画面コンテンツの柔軟性を高めることができるように強化されました。 これらの変更については、このガイドで説明します。

## <a name="app-store-changesapp-store-changesmd"></a>[App Store の変更点](app-store-changes.md)

IOS アプリストアには完全な再設計がありました。これにより、ユーザーがストアを効率的に移動できるだけでなく、開発者はアプリをユーザーに昇格させることもできます。 これらのプロモーションには、アプリ内購入と製品ページの更新の更新が含まれます。 iOS 11 では、ユーザーとの通信方法、アプリのアイコンを追加する方法、アプリを公開する方法に関する更新プログラムも追加されています。

## <a name="app-icon-updates"></a>アプリアイコンの更新

> [!NOTE]
> _アセットカタログ_によってアプリアイコンが配信されるようになりました。 

アセットカタログの使用の詳細については、 [App Store アイコン](~/ios/app-fundamentals/images-icons/app-store-icon.md)ガイドを参照してください。 情報 plist から資産カタログへのアイコンの移行については、「[情報から資産カタログへの移行](~/ios/app-fundamentals/images-icons/app-icons.md)」ガイドを参照してください。

アセットカタログの必須アイコンの名前は**App Store**で、サイズは 1024 x 1024 である必要があります。 Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

![アセットカタログ内のアプリストアアイコンの場所。](images/image1.png)

## <a name="related-links"></a>関連リンク

- [IOS 11 (Apple) の新機能](https://developer.apple.com/ios/)
- [更新された App Store 製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) 用のアプリの更新 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)
