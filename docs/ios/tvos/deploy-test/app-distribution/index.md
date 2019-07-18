---
title: tvOS アプリの配布の概要
description: このドキュメントでは、Xamarin.tvOS アプリで利用できる配布手法の概要を説明しより詳細なドキュメントについてのトピックへのポインターとして機能します。
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: d2f4031eeddbaa206f38b7b1c2bb49d21482c175
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864942"
---
# <a name="tvos-app-distribution-overview"></a>tvOS アプリの配布の概要

_このドキュメントでは、Xamarin.tvOS アプリで利用できる配布手法の概要を説明しより詳細なドキュメントについてのトピックへのポインターとして機能します。_


Xamarin.tvOS アプリの開発が完了するとソフトウェア開発ライフ サイクルの次の手順は、次の図の強調表示されたセクションに示すように、ユーザーにアプリを配布するには。


[![ソフトウェア開発ライフ サイクルの概要](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple では、Xamarin.tvOS でサポートされている次の tvOS アプリを配布する方法を提供します。

1. [**App Store**](#Apple-TV-App-Store-Distribution)
2. [**社内 (エンタープライズ)** ](#In-House-Distribution) 
3. [**アドホック**](#Ad_Hoc_Distribution) 

これらいずれのシナリオでも、適切な*プロビジョニング プロファイル*を使用してアプリケーションをプロビジョニングする必要があります。 プロビジョニング プロファイルは、コード署名情報だけでなく、アプリケーションの ID と使用する配布メカニズムも含むファイルです。 App Store 以外の配布には、アプリを展開できるデバイスに関する情報も含まれています。

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV App Store 配布

これは、Apple TV のデバイスにコンシューマーに tvOS アプリを配布するための主要手段です。 Apple TV App Store に送信されたすべてのアプリでは、Apple によって承認が必要です。

アプリを App Store に提出するには、*iTunes Connect* というポータルを使用します。 参照してください、 [iTunes Connect でアプリを構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)ガイドについては、次のトピック。

- 管理の契約、税金、銀行
- ITunes Connect レコードを作成します。
- アプリのビデオとスクリーン ショットを管理します。
- 管理アプリの名前、説明、新機能については、キーワード、Url。
- 一般的なアプリ情報の保持します。
- Game Center 情報の保持します。
- App Review 情報の保持します。
- 価格情報の保持します。
- アプリ内購入情報の保持します。
- アプリケーションのレビューを表示します。

TvOS 用具体的には書き込まれないため、中にこのドキュメントを設定して、このポータルを使用して、Apple TV App Store、パブリッシング用のアプリを準備する方法の情報を提供します。 同じ手順の多くが iOS または tvOS アプリを設定するかどうか必要です。

すべての上に示した手順を完了すると表示、 [tvOS アプリの iTunes Connect で構成](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md)必要な tvOS アプリ固有情報を追加します。

**Apple Developer Program** に属する開発者のみが iTunes Connect にアクセスすることができる点に注意してください。 **Apple Developer Enterprise Program** のメンバーはアクセスできません。

Xamarin.tvOS、Apple TV App Store にアプリの提出に関する問題が発生した場合を参照してください、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ガイド。 発生する可能性のあるいくつかの既知の問題と、Xamarin.tvOS でそれらを解決する方法が含まれています。

詳細については、次を参照してください、 [、Apple TV App Store に公開](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md)ガイド。

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>社内配布

社内配布は、*エンタープライズ配布*とも呼ばれます。社内配布の場合、**Apple Developer Enterprise Program** のメンバーは、同じ組織内の他のメンバーにアプリを配布できます。 社内配布には、App Store のレビューを必要としないという利点があります。また、アプリケーションをインストールできるデバイス数に制限がありません。 ただし、**Apple Developer Enterprise Program** メンバーは iTunes Connect に**アクセスできない**ため、ライセンシーがアプリを配布する必要があります。

セットアップし、アプリケーションを社内で配布する方法の詳細についてを参照してください、[社内配布ガイド](~/ios/deploy-test/app-distribution/in-house-distribution.md)します。 このドキュメントは、iOS に固有ですが、同じ手法が tvOS アプリの使用されます。

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>アドホック配布

Xamarin.tvOS アプリは、両方で使用されるアドホック配布を使用してユーザー テストを実行できる、 **Apple Developer Program**、および**Apple Developer Enterprise Program**、でき、最大 100 個の Apple TV のデバイステストします。 アドホック配布の最適なユース ケースは、iTunes Connect がオプションではない場合に、社内の分布です。

セットアップし、アプリを社内で配布する方法の詳細についてを参照してください、[アドホック配布ガイド](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)します。 このドキュメントは、iOS に固有ですが、tvOS アプリに同じ手法を使用します。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事で Xamarin.tvOS アプリで利用可能な配布メカニズムの概要を説明しました。 導入された Apple TV App Store では、社内およびアドホック展開しより詳細な情報へのリンクを提供します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
