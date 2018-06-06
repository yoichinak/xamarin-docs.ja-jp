---
title: IOS 11 にビルドされたアプリの更新
description: このドキュメントには、iOS 11 のリリースで Xamarin.iOS 開発者が利用できる新しい機能を記述するさまざまなガイドへのリンクがします。 たとえば、ビジュアル デ ザインの更新プログラム、アプリ ストアが変更され、アプリ アイコンを更新します。
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: ea57acbd6165f7b1abd8b9bd69873670c179f411
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787573"
---
# <a name="updating-your-app-to-ios-11"></a>IOS 11 にビルドされたアプリの更新

11、iOS では、Apple がアーキテクチャの更新、新しいビジュアルの変更、および更新された iTunes Connect プロセスに導入されました。 このガイドは、iOS 11 用に更新 Xamarin.iOS アプリを入手することができ、これらの変更の各を説明します。

## <a name="architecture-changesarchitecture-changesmd"></a>[アーキテクチャの変更点](architecture-changes.md)

Ios 11 の注意する必要がある最大の変更点の 1 つは、アプリについては、「32 ビット サポートの廃止[Apple の](https://developer.apple.com/news/?id=06282017b)プレス リリースします。

このガイドに従って、64 ビットのアプリを更新します。

## <a name="visual-design-updatesvisual-designmd"></a>[ビジュアル デザインの更新](visual-design.md)

11、iOS では、Apple には、ナビゲーション バー、検索バー、およびテーブル ビューの更新を含む新しいビジュアルの変更が導入されました。 さらに機能強化に加えられた柔軟性を高めるための余白と全画面表示のコンテンツを許可します。 このガイドでは、これらの変更について説明します。

## <a name="app-store-changesapp-store-changesmd"></a>[App Store の変更点](app-store-changes.md)

IOS App Store は、ストアを効率的に移動することができますが、することもできます、開発者は、ユーザーにアプリを昇格させるだけでなく全体の再設計をされました。 これらの上位変換は、アプリ内購入を更新プログラムの製品ページを更新します。 iOS 11 では、ユーザーと通信する方法、アプリ アイコンを追加する方法、および公開するアプリをリリースする方法に関する更新も追加します。

## <a name="app-icon-updates"></a>アプリのアイコンを更新します。

> [!NOTE]
> アプリのアイコンを配信するようになりました、_アセット カタログ_です。 

アセット カタログの使用の詳細についてを参照してください、[アプリ ストア アイコン](~/ios/app-fundamentals/images-icons/app-store-icon.md)ガイドです。 移行のヘルプについて、資産カタログを Info.plist のアイコンを参照してください、 [Info.plist から資産カタログへの移行](~/ios/app-fundamentals/images-icons/app-icons.md)ガイドです。

アセット カタログに必要なアイコンの名前は**App Store** 1024 × 1024 のサイズを指定する必要があります。 Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

![アプリは、アセット カタログにアイコンの場所を格納します。](images/image1.png)

## <a name="related-links"></a>関連リンク

- [新機能 iOS 11 (Apple)](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [更新、iOS 用アプリ 11 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)
