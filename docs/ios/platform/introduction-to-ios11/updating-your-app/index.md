---
title: IOS 11 に、アプリの更新
description: このドキュメントは、iOS 11 のリリースでの Xamarin.iOS の開発者が利用できる新しい機能を記述するさまざまなガイドにリンクしています。 たとえば、ビジュアル デザインの更新プログラムは、App Store の変更、され、アプリ アイコンが更新されます。
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 3193f27c30080a4335abfe969acb3c8b33516469
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110779"
---
# <a name="updating-your-app-to-ios-11"></a>IOS 11 に、アプリの更新

IOS 11 には、Apple にアーキテクチャの更新プログラムや新しいビジュアルの変更、更新された iTunes Connect のプロセスが導入されています。 このガイドは、Xamarin.iOS アプリを iOS 11 の更新を取得するのに役立つそれぞれこれらの変更を説明します。

## <a name="architecture-changesarchitecture-changesmd"></a>[アーキテクチャの変更点](architecture-changes.md)

IOS 11 を意識する必要がある最大の変更点の 1 つで詳述するように、アプリの 32 ビットのサポートの廃止[Apple の](https://developer.apple.com/news/?id=06282017b)プレス リリースします。

このガイドでは、64 ビットのアプリの更新手順に従ってできます。

## <a name="visual-design-updatesvisual-designmd"></a>[ビジュアル デザインの更新](visual-design.md)

Ios 11 では、Apple は、ナビゲーション バー、検索バー、およびテーブル ビューの更新を含む新しいビジュアルの変更を導入しました。 さらに柔軟性を高めるための余白と全画面表示のコンテンツを許可する改良が加えられてがいます。 このガイドでは、これらの変更がについて説明します。

## <a name="app-store-changesapp-store-changesmd"></a>[App Store の変更点](app-store-changes.md)

IOS App Store は、ストアを効率的に移動することができますが、ことができます、開発者は、ユーザーにアプリの販売促進するだけでなく完全の再設計をしました。 これらの上位変換には、アプリ内購入に更新プログラムと製品のページへの更新が含まれます。 iOS 11 では、ユーザーと通信する方法や、アプリ アイコンを追加する方法、一般に、アプリをリリースする方法に関する更新プログラムも追加します。

## <a name="app-icon-updates"></a>アプリ アイコンの更新

> [!NOTE]
> アプリ アイコンは、今すぐ配信する必要があります、_資産カタログ_します。 

資産カタログの使用に関する情報を参照してください、 [App Store アイコン](~/ios/app-fundamentals/images-icons/app-store-icon.md)ガイド。 ヘルプの移行を Info.plist からアイコンをアセット カタログを参照してください、 [Info.plist から資産カタログへの移行](~/ios/app-fundamentals/images-icons/app-icons.md)ガイド。

資産カタログに必要なアイコンの名前は**App Store** 1024 × 1024 のサイズを指定する必要があります。 Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

![アプリは、アセット カタログのアイコンの場所を格納します。](images/image1.png)

## <a name="related-links"></a>関連リンク

- [IOS 11 (Apple) で新します。](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品のページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (ビデオ) 用のアプリの更新](https://developer.apple.com/videos/play/wwdc2017/204/)
