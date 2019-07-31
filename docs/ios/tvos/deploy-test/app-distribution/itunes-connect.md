---
title: iTunes Connect での tvOS アプリの構成
description: この記事では、tvOS 固有の構成のために、iTunes Connect でアプリを構成するための補足ガイドを提供します。
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 528eee6ade7236d83d5a5bdec6b27ba76f540ad0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649154"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>iTunes Connect での tvOS アプリの構成

_この記事では、tvOS 固有の構成のために、iTunes Connect でアプリを構成するための補足ガイドを提供します。_


「 [ITunes Connect でアプリを構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)する」ガイドで説明されている構成と設定に加えて、このドキュメントでは、Apple TV アプリで tvOS アプリをリリースするために必要な特定の構成について説明します。ソース.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>TvOS リリースバージョンの追加

Apple TV App Store でリリースする新しいアプリを作成する場合も、既存の iOS アプリに Apple TV サポートを追加する場合も、iTunes Connect レコードを作成し、次の iOS 固有のガイドを使用して構成する必要があります。

- [iTunes Connect レコードの作成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [アプリのビデオとスクリーンショットの管理](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [名前、説明、新機能、キーワード、URL の管理](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [一般情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

必要に応じて、次のものが必要になる場合もあります。

- [Game Center 情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [アプリ内購入情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

上記の手順をすべて完了したら、アプリの iTunes Connect レコードを開き、左側のサイドバーを使用して tvOS サポートを追加します。

[![](itunes-connect-images/connect01.png "左側のサイドバーを使用して tvOS サポートを追加する")](itunes-connect-images/connect01.png#lightbox)

次に、特定の iTunes Connect レコードで、tvOS 固有の情報画面を使用できるようになります。

[![](itunes-connect-images/connect02.png "TvOS 固有の情報画面")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS のバージョン情報

左側のサイドバーで、[tvOS APP] セクションの **[1.0 For 送信]** を選択します。

[![](itunes-connect-images/connect03.png "tvOS のバージョン情報")](itunes-connect-images/connect03.png#lightbox)

この画面では、次の情報を提供します。

- 必要なスクリーンショット、説明、キーワード、および Url。
- バージョン番号、著作権、年齢区分などの一般的なアプリ情報。
- 省略可能なアプリ内購入。
- オプション Game Center、スコアボードとアチーブメントでサポートされます。
- 連絡先、デモ用アカウント、メモなどのアプリレビュー情報が必要です。

必要な情報を入力したら、画面の右上隅にある **[保存]** ボタンをクリックして、変更を保存します。

[![](itunes-connect-images/connect04.png "tvOS のバージョン情報を送信する準備ができました")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>レビューのために送信する準備をしています

TvOS アプリを確認のために Apple TV App Store に送信する準備ができたら、アプリの iTunes Connect レコードに戻り、画面の右上隅にある **[レビュー用に送信]** ボタンをクリックします。

[![](itunes-connect-images/connect05.png "レビュー用に送信する")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、iTunes Connect で tvOS アプリを Apple TV App Store にリリースするために必要な tvOS 固有の設定の概要を説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
