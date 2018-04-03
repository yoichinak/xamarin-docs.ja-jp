---
title: "アプリ ストアの変更"
description: "IOS 11 の新機能の検証"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 78528c750c6350d113b34a07d166a03773119a8b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="app-store-changes"></a>アプリ ストアの変更

_IOS 11 の新機能の検証_

IOS App Store は、ストアを効率的に移動することができますが、することもできます、開発者は、ユーザーにアプリを昇格させるだけでなく全体の再設計をされました。 これらの上位変換は、アプリ内購入を更新プログラムの製品ページを更新します。 iOS 11 では、ユーザーと通信する方法、アプリ アイコンを追加する方法、および公開するアプリをリリースする方法に関する更新も追加します。

![新しいアプリ ストアのレイアウト](app-store-changes-images/image3.jpg)

再設計されたアプリ ストアには、次のセクションがあります。

- **今日**– このタブには、「エディターの選択」をアプリの機能を備えたアプリが含まれています。 昇格をここで、アプリ情報を入力で、[昇格](https://developer.apple.com//contact/app-store/promote/)ページ。
- **ゲーム**– に設定されているすべてのアプリ、**ゲーム**iTunes Connect でカテゴリはこのタブの下にあります。
- **アプリ**– このタブの他のすべてのアプリの機能です。 ユーザーは、おすすめアプリが、カテゴリ、上位有料または無料で参照できます。
- **更新プログラム**– このアプリは、アプリに更新プログラムを表示します。 使用して、アプリをリリースする場合でも[リリースを段階的](#Phased_Release)ユーザーがまだこのタブに移動し、最新バージョンをダウンロードします。
- **検索**– このタブで、アプリの検索をできます。

## <a name="store-icon"></a>ストアのアイコン

ストア アイコン (またはマーケティング アイコン) iTunes Connect で管理されなくなりますが、代わりに、として含める必要があります、[アセット カタログ](~/ios/app-fundamentals/images-icons/app-icons.md)アプリケーション バイナリは、アプリのアイコンに似ています。 PNG 形式で 1024 × 1024 ストア アイコンは、11 の iOS アプリの送信が成功したため、アセット カタログに含める必要があります。

アプリが簡略化は、この追加のアセット カタログがアプリのサイズを拡大しないことを確認します。


## <a name="in-app-purchases-promoted-in-the-app-store"></a>App Store で昇格アプリ内購入

Apple が行われたアプリ内購入発見しやすく、アプリ ストアでします。 20 個まで追加できます_アプリ ストアの昇格_アプリ ページで、アプリの製品 ページで、または検索することで現在使用されているアプリ内購入します。

アプリ ストアに表示される、アプリ内購入を使用して、次のデータを含める必要があります。

- **イメージ**– アイコンは、アプリ内購入を特別に設計されたイメージを提供する必要があります。 このイメージは、アプリ アイコンから異なる必要があります、スクリーン ショットをすることはできません。
- **名前**– 名前は最大 30 文字までにのみできます。
- **説明**– 説明は、45 文字の最大数を指定できますのみです。

どのアプリ内購入されているプロモーションが app store レビューの対象は、公開前にします。

昇格に使用できる、アプリ内購入をするためには、アプリを開きますを見つけて**機能 > アプリ内購入**です。 移動して、**アプリ ストアの昇格 (省略可能)**セクションし、1024 × 1024 イメージを追加し、**保存**次の図に示すように。

![アプリ ストアの昇格」セクション iTune 接続](app-store-changes-images/image4.png)

追加する必要があります、`ShouldAddStorePayment`メソッドを`SKPaymentTransactionObserver`アプリでのプロトコルです。

アプリ内購入の上位変換の詳細については、Apple を参照してください。 [、アプリ内購入を昇格させる](https://developer.apple.com/app-store/promoting-in-app-purchases/)ページ。

## <a name="redesigned-product-page"></a>再設計された製品ページ

製品のページに、次のように変更されました。

- タイトルが 30 文字の最大値に設定されてようになりました
- サブタイトルが追加されました
    - タイトルを補完する簡単な概要になります。
    - 字幕は最大 30 文字がある必要があります。
- アプリのプレビュー
    - 次の 3 つのビデオやスクリーン ショットを保持できます。
    - ビデオ再生ページにアクセスするユーザー。
    - 詳細については、Apple を参照してください。[アプリのプレビュー](https://developer.apple.com/app-store/app-previews/)ページ。
- キャンペーンのテキスト
    - この新しい機能は、アプリケーションに関する情報を頻繁に変更を記述するためのテキストの 170 文字を提供します。
    - 新しいバージョンのアプリを送信することがなくいつでも更新できます。

## <a name="customer-communication"></a>お客様の通信

10.3 には、Apple は、開発者がアプリケーション ユーザーがレビューに応答する機能を利用と直接通信するための新しい方法を起動します。 ITunes でこの新しい機能にアクセスすることができますを参照して接続**アプリ > アクティビティ > 評価とレビュー**次の図に示すように。

![コメントに返信を入力する領域を示すダイアログ](app-store-changes-images/image5.png)

ユーザーに返信する際の注意すべき点がいくつかあります。

- 1 回、返信できますのみが、必要に応じて、両方のパーティは、コメントを編集できます。
- ユーザーは、コメントに返信するときに、通知を受け取ります。
- ITunes でロールが作成された新しいカスタマー サポートに接続します。 このロールまたは管理者の役割を持つユーザーは、コメントに応答できます。

詳細については、Apple を参照してください。[レビューに応答して](https://developer.apple.com/app-store/responding-to-reviews/)ページ。

<a name="Phased_Release"/>

## <a name="phased-release"></a>段階的なリリース

IOS 11、Apple はアプリに更新プログラムの段階的なリリースのオプションを実装しました。 お客様は、要求が、実稼働環境をオーバー ロードしないことを確実にリリースする段階的に、アプリの更新を許可する段階的なリリースを有効にすることができます。

ITunes Connect では、段階的なリリースが有効にします。 サイド バーで、アプリでクリックすると、スクロールして、**の自動更新をリリース段階的**クリックし、下部セクション**リリース 7 日間の期間を使用して更新プログラムがリリースを段階的**:

![段階的リリースの自動更新のオプションを表示](app-store-changes-images/image6.png)

更新内容は、アプリ ストアの更新 タブでのダウンロード後すぐに利用可能なです。 段階的なリリースでは、選択した自動ダウンロードを持っているユーザーに対して使用可能なのみです。


## <a name="related-links"></a>関連リンク

- [新機能 iOS 11 (Apple)](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [更新、iOS 用アプリ 11 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)