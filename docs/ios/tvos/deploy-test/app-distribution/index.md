---
title: tvOS アプリの配布の概要
description: このドキュメントでは、tvOS アプリで使用できる分布手法の概要を示し、トピックに関する詳細なドキュメントへのポインターとして機能します。
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 0ea96eb3808daeb9f8764695d1b4b3d432727ff2
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292485"
---
# <a name="tvos-app-distribution-overview"></a>tvOS アプリの配布の概要

_このドキュメントでは、tvOS アプリで使用できる分布手法の概要を示し、トピックに関する詳細なドキュメントへのポインターとして機能します。_


TvOS アプリの開発が完了したら、ソフトウェア開発ライフサイクルの次の手順は、次の図の強調表示されたセクションに示すように、ユーザーにアプリを配布することです。


[![ソフトウェア開発ライフサイクルの概要](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple では、tvOS でサポートされている tvOS アプリを配布するための次の方法を提供しています。

1. [**App Store**](#Apple-TV-App-Store-Distribution)
2. [**社内 (エンタープライズ)** ](#In-House-Distribution) 
3. [**アドホック**](#Ad_Hoc_Distribution) 

これらいずれのシナリオでも、適切な*プロビジョニング プロファイル*を使用してアプリケーションをプロビジョニングする必要があります。 プロビジョニング プロファイルは、コード署名情報だけでなく、アプリケーションの ID と使用する配布メカニズムも含むファイルです。 App Store 以外の配布には、アプリを展開できるデバイスに関する情報も含まれています。

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV App Store の配布

これは、tvOS アプリが Apple TV デバイス上のコンシューマーに配布される主な方法です。 Apple TV App Store に送信されたすべてのアプリは、Apple による承認を必要とします。

アプリを App Store に提出するには、*iTunes Connect* というポータルを使用します。 次のトピックについては、 [ITunes Connect でのアプリの構成に](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)関するガイドを参照してください。

- 契約、税金、銀行の管理。
- ITunes Connect レコードを作成しています。
- アプリのビデオとスクリーンショットの管理。
- アプリの名前、説明、新機能、キーワード、Url の管理。
- 一般的なアプリ情報を保持する。
- Game Center 情報を保持しています。
- アプリのレビュー情報を保持します。
- 価格情報を保持しています。
- アプリ内購入情報を保持しています。
- アプリケーションレビューを表示しています。

このドキュメントでは、tvOS 専用に記述されていませんが、Apple TV App Store でアプリを発行する準備を行うために、このポータルを設定して使用する方法についての情報を提供します。 IOS アプリと tvOS アプリのどちらを設定する場合でも、同じ手順の多くが必要です。

上記のすべての手順を完了したら、「 [ITunes Connect で TvOS アプリを構成](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md)する」を参照して、必要となる tvOS アプリ固有の情報を追加します。

**Apple Developer Program** に属する開発者のみが iTunes Connect にアクセスすることができる点に注意してください。 **Apple Developer Enterprise Program** のメンバーはアクセスできません。

TvOS アプリを Apple TV App Store に送信する際に問題が発生した場合は、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ガイドを参照してください。 これには、発生する可能性のある既知の問題と、それらを tvOS で解決する方法が含まれています。

詳細については、「 [APPLE TV App Store への発行](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md)」ガイドを参照してください。

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>社内配布

社内配布は、*エンタープライズ配布*とも呼ばれます。社内配布の場合、**Apple Developer Enterprise Program** のメンバーは、同じ組織内の他のメンバーにアプリを配布できます。 社内配布には、App Store のレビューを必要としないという利点があります。また、アプリケーションをインストールできるデバイス数に制限がありません。 ただし、**Apple Developer Enterprise Program** メンバーは iTunes Connect に**アクセスできない**ため、ライセンシーがアプリを配布する必要があります。

社内でアプリケーションをセットアップして配布する方法の詳細については、[社内配布ガイド](~/ios/deploy-test/app-distribution/in-house-distribution.md)を参照してください。 このドキュメントは iOS に固有のものですが、tvOS アプリでも同じ手法が使用されます。

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>アドホック配布

TvOS アプリは、 **Apple Developer program**と**Apple developer Enterprise program**の両方で使用できるアドホック配布を使用してユーザーテストを行うことができ、最大100の apple TV デバイスをテストできます。 ITunes Connect がオプションでない場合、アドホック配布に最適なユースケースは社内で配布されます。

社内でアプリをセットアップして配布する方法の詳細については、「[アドホック配布ガイド」](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)を参照してください。 ここでも、このドキュメントは iOS に固有のものですが、tvOS アプリでも同じ手法が使用されます。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS アプリで使用できる配布メカニズムの概要について説明しました。 Apple TV App Store、アドホック展開、社内展開が導入され、詳細情報へのリンクが提供されました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
