---
title: ITunes Connect で tvOS アプリを構成します。
description: この記事では、iOS、tvOS の特定の構成は、iTunes Connect でアプリの構成に補足的なガイドを提供します。
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3f4ef00cfe990de2d5afd461d7a110d32bc4a236
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108820"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>ITunes Connect で tvOS アプリを構成します。

_この記事では、iOS、tvOS の特定の構成は、iTunes Connect でアプリの構成に補足的なガイドを提供します。_


構成と、iOS に従って作成する必要があります設定だけでなく[iTunes Connect でアプリを構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)ガイドでは、このドキュメントでは、具体的な構成を Xamarin.tvOS をリリースする必要がありますApple TV App Store でのアプリ。

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>TvOS リリース バージョンを追加します。

Apple TV の App Store にリリースされる新しいアプリを作成するかに既存の iOS アプリを Apple TV のサポートを追加する必要があります、iTunes Connect レコードを作成およびその構成が次の iOS を使用して特定のガイドします。

- [iTunes Connect レコードの作成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [アプリのビデオとスクリーンショットの管理](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [名前、説明、新機能、キーワード、URL の管理](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [一般的な情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

必要に応じて、する可能性がありますも必要です。

- [Game Center 情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [アプリ内購入情報の保持](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

すべて上記の手順を完了したは、アプリの iTunes Connect レコードと左側にあるサイドバーを使用して、tvOS のサポートを追加する選択を開きます。

[![](itunes-connect-images/connect01.png "左側のサイド バーを使用して、tvOS のサポートを追加します。")](itunes-connect-images/connect01.png#lightbox)

TvOS の特定の情報画面は指定の iTunes Connect レコードの使用可能なになります。

[![](itunes-connect-images/connect02.png "TvOS の特定の情報画面")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS バージョン情報

左側にあるサイド バーから選択**1.0 の送信準備**tvOS アプリ セクションの下。

[![](itunes-connect-images/connect03.png "tvOS バージョン情報")](itunes-connect-images/connect03.png#lightbox)

この画面で、次の情報を指定します。

- 必要なスクリーン ショット、説明、キーワード、Url。
- バージョン番号、著作権、および年齢別規制などの一般的なアプリ情報。
- 省略可能なアプリ内購入します。
- ランキング、実績と省略可能 Game Center のサポート。
- 連絡先、デモ アカウント、およびメモなどの App Review 情報が必要です。

必要な情報を入力すると、クリックして、**保存**変更を保存するには、画面の右上隅のボタン。

[![](itunes-connect-images/connect04.png "送信の tvOS バージョン情報を準備します。")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>確認用に送信する準備をしています

最後に、Apple TV App Store レビューのために、Xamarin.tvOS アプリを送信する準備ができたら、アプリの iTunes Connect レコードに戻り、をクリックして、**確認用に送信**画面の右上隅のボタンをクリックします。

[![](itunes-connect-images/connect05.png "確認用に送信します。")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、iTunes Connect で Apple TV App Store への tvOS アプリをリリースするために必要な tvOS 特定の設定の概要について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
