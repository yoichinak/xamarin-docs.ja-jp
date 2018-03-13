---
title: "ITunes Connect で、tvOS アプリを構成します。"
description: "この記事では、iOS、tvOS の特定の構成の iTunes Connect でアプリの構成に補足的なガイドを提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b6fad9eadbff272f86f9e426e3f6eb5d48847127
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>ITunes Connect で、tvOS アプリを構成します。

_この記事では、iOS、tvOS の特定の構成の iTunes Connect でアプリの構成に補足的なガイドを提供します。_


構成と、iOS をに従って作成する必要があります設定だけでなく[iTunes Connect でアプリを構成する](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)このドキュメントのガイドでは、Xamarin.tvOS を解放する必要が特定の構成について説明しますApple TV のアプリ ストアでアプリです。

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>TvOS リリース版を追加します。

Apple TV の App Store でリリースされる新しいアプリを作成するか、Apple TV のサポートを既存の iOS アプリに追加すると、次のように、iTunes Connect レコードを作成し、それを構成する必要ありますか次の iOS を使用して特定のガイドします。

- [iTunes Connect レコードの作成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [アプリのビデオとスクリーンショットの管理](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [名前、説明、新機能、キーワード、URL の管理](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [一般的な情報を保持します。](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

必要に応じて、する可能性がありますも必要とします。

- [Game Center 情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [アプリ内購入情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

すべての上記の手順を完了には、アプリの iTunes Connect レコードと、左側にあるサイドバーを使用して、tvOS のサポートを追加するを選択してを開きます。

[![](itunes-connect-images/connect01.png "左側にあるサイド バーを使用して、tvOS のサポートを追加します。")](itunes-connect-images/connect01.png#lightbox)

TvOS 固有の情報画面れます特定 iTunes Connect レコードの使用。

[![](itunes-connect-images/connect02.png "TvOS 固有の情報画面")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS バージョン情報

左側にあるサイドバーから選択**1.0 の送信準備**tvOS アプリ セクションの下。

[![](itunes-connect-images/connect03.png "tvOS バージョン情報")](itunes-connect-images/connect03.png#lightbox)

この画面で、次の情報を提供します。

- 必要なスクリーン ショット、説明、キーワードと Url。
- バージョン番号、著作権および年齢別規制などの一般的なアプリ情報。
- 省略可能なアプリ内購入します。
- スコアボードと実績オプション ゲーム センターのサポート。
- 連絡先、デモ アカウントおよびのメモなどの情報のアプリの確認が必要です。

クリックして、必要な情報を入力すると、**保存**変更を保存する画面の右上隅のボタン。

[![](itunes-connect-images/connect04.png "tvOS バージョン情報の送信準備完了")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>確認するために送信する準備をしています

最後に、Apple TV App Store レビューのために Xamarin.tvOS アプリを送信する準備ができたら、アプリの iTunes Connect レコードに戻るし をクリックして、**レビューするために送信**画面の右上隅のボタンをクリックします。

[![](itunes-connect-images/connect05.png "確認を送信します。")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、iTunes Connect で Apple TV の App Store に tvOS アプリをリリースするために必要な tvOS の特定の設定の概要を指定します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
