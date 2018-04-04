---
title: アプリの配布の概要
description: このドキュメントでは、Xamarin.tvOS アプリで利用可能な配布技術の概要し、トピックの詳細なドキュメントへのポインターとして機能します。
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e5be0bef158c87fe06516d9a58e34c741e6e14b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="app-distribution-overview"></a>アプリの配布の概要

_このドキュメントでは、Xamarin.tvOS アプリで利用可能な配布技術の概要し、トピックの詳細なドキュメントへのポインターとして機能します。_


Xamarin.tvOS アプリを開発すると後、ソフトウェア開発ライフ サイクルの次の手順は、強調表示されたセクションの次の図に示すように、アプリをユーザーに配布するのには。


[![ソフトウェア開発ライフ サイクルの概要](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple では、Xamarin.tvOS でサポートされている、tvOS のアプリを配布する次の方法を提供します。

1. [**App Store**](#Apple-TV-App-Store-Distribution)
2. [**社内 (エンタープライズ)**](#In-House-Distribution) 
2. [**アドホック**](#Ad_Hoc_Distribution) 

これらいずれのシナリオでも、適切な*プロビジョニング プロファイル*を使用してアプリケーションをプロビジョニングする必要があります。 プロビジョニング プロファイルは、コード署名情報だけでなく、アプリケーションの ID と使用する配布メカニズムも含むファイルです。 App Store 以外の配布には、アプリを展開できるデバイスに関する情報も含まれています。

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV のアプリ ストアの配布

これは、Apple TV のデバイスにコンシューマーに tvOS アプリを配布するようにするための主要手段です。 Apple TV の App Store に送信されたすべてのアプリでは、Apple によって承認が必要です。

アプリを App Store に提出するには、*iTunes Connect* というポータルを使用します。 参照してください、 [iTunes Connect でアプリを構成する](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)ガイドについては、次のトピック。

- 管理の契約、税およびバンキングです。
- ITunes Connect レコードを作成します。
- アプリのビデオやスクリーン ショットを管理します。
- 管理するアプリの名前、説明、新機能、キーワードと Url。
- 一般的なアプリの情報を維持できます。
- ゲーム センターの情報を維持できます。
- アプリの確認情報を維持できます。
- 価格情報を維持できます。
- アプリ内購入情報を維持できます。
- アプリケーションのレビューを表示します。

TvOS を具体的には書き込まれません、中にこのドキュメントを設定し、このポータルを使用して Apple TV の App Store、パブリッシング用のアプリを準備する方法の情報を提供します。 同じ手順の多くが iOS または tvOS アプリを設定するかどうかに必要です。

すべての上に示した手順が完了したらを参照してください、 [、tvOS アプリを iTunes Connect で構成](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md)を tvOS アプリ固有の情報を追加が必要になります。

**Apple Developer Program** に属する開発者のみが iTunes Connect にアクセスすることができる点に注意してください。 **Apple Developer Enterprise Program** のメンバーはアクセスできません。

Apple TV の App Store に Xamarin.tvOS アプリの提出の問題が発生した場合を参照してください、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ガイドです。 発生する可能性があるいくつかの既知の問題と、Xamarin.tvOS でそれらを解決する方法が含まれています。

詳細については、次を参照してください、 [Apple TV のアプリ ストアに公開](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md)ガイドです。

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>社内配布

社内配布は、*エンタープライズ配布*とも呼ばれます。社内配布の場合、**Apple Developer Enterprise Program** のメンバーは、同じ組織内の他のメンバーにアプリを配布できます。 社内配布には、App Store のレビューを必要としないという利点があります。また、アプリケーションをインストールできるデバイス数に制限がありません。 ただし、**Apple Developer Enterprise Program** メンバーは iTunes Connect に**アクセスできない**ため、ライセンシーがアプリを配布する必要があります。

セットアップと、アプリケーションを社内で配布する方法を取得する方法についてを参照してください、[社内の配布のガイド](~/ios/deploy-test/app-distribution/in-house-distribution.md)です。 このドキュメントは iOS に固有ですが、tvOS アプリ用のと同じ手法を使用します。

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>アドホック配布

Xamarin.tvOS アプリは、両方で使用できるは、アドホックの配布を使用してユーザー テストできる、 **Apple Developer Program**、および**Apple Developer Enterprise Program**、でき、最大 100 Apple TV のデバイステストします。 アドホック配布用の最適なユース ケースは、社内配布 iTunes Connect がない場合、オプションです。

セットアップと社内で、アプリを配布する方法を取得する方法についてを参照してください、[アドホック配布ガイド](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)です。 このドキュメントは iOS に固有でも、tvOS アプリ用のと同じ手法を使用します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.tvOS アプリで利用可能な配布メカニズムの概要を指定します。 導入された Apple TV の App Store、アドホックおよび社内展開、およびより詳細な情報へのリンクを提供します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
