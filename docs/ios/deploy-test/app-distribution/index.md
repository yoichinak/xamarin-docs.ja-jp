---
title: Xamarin.iOS アプリの配布の概要
description: ここでは、Xamarin.iOS アプリケーションで使用できる配布手法の概要について説明します。また、このトピックよりも詳細なドキュメントについても紹介します。
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 6141b84af56d7680e2b294317ba97fb6943e17c5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106053"
---
# <a name="xamarinios-app-distribution-overview"></a>Xamarin.iOS アプリの配布の概要

_ここでは、Xamarin.iOS アプリケーションで使用できる配布手法の概要について説明します。また、このトピックよりも詳細なドキュメントについても紹介します。_

Xamarin.iOS アプリを開発したら、ソフトウェア開発ライフサイクルの次の手順は、下図で強調表示されているセクションのように、ユーザーにアプリを配布することです。


[![](images/publishingdiagram.png "iOS アプリを開発したら、次の手順は下図で強調表示されているセクションのように、ユーザーにアプリを配布することです")](images/publishingdiagram.png#lightbox)


Apple は、次のように Xamarin.iOS でサポートされる iOS アプリケーションを配布する方法を提供しています。

1. [**App Store**](#App_Store_Distribution)
2. [**社内 (エンタープライズ)**](#In-House_Distribution)
2. [**アドホック**](#Ad_Hoc_Distribution)

これらいずれのシナリオでも、適切な*プロビジョニング プロファイル*を使用してアプリケーションをプロビジョニングする必要があります。 プロビジョニング プロファイルは、コード署名情報だけでなく、アプリケーションの ID と使用する配布メカニズムも含むファイルです。 App Store 以外の配布には、アプリを展開できるデバイスに関する情報も含まれています。

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>App Store 配布

> [!IMPORTANT]
> Apple は、2018 年 7 月以降に App Store に提出されるすべてのアプリおよび更新プログラムが iOS 11 SDK でビルドされ、[iPhone X ディスプレイをサポートする](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md)必要があることを[通知しました](https://developer.apple.com/news/?id=05072018a)。

iOS デバイスのユーザーに iOS アプリケーションを配布するには、これが主な方法です。 App Store に提出されたすべてのアプリは、Apple の承認を受ける必要があります。

アプリを App Store に提出するには、*iTunes Connect* というポータルを使用します。 このポータルをセットアップし、使用して、App Store で Xamarin.iOS アプリを発行する準備をするには、「[Configure your App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)」(iTunes Connect でのアプリの構成) ガイドを参照してください。

**Apple Developer Program** に属する開発者のみが iTunes Connect にアクセスすることができる点に注意してください。 **Apple Developer Enterprise Program** のメンバーはアクセスできません。

詳細については、「[App Store Distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)」(App Store の配布) ガイドを参照してください。

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>社内配布

社内配布は、*エンタープライズ配布*とも呼ばれます。社内配布の場合、**Apple Developer Enterprise Program** のメンバーは、同じ組織内の他のメンバーにアプリを配布できます。 社内配布には、App Store のレビューを必要としないという利点があります。また、アプリケーションをインストールできるデバイス数に制限がありません。 ただし、**Apple Developer Enterprise Program** メンバーは iTunes Connect に**アクセスできない**ため、ライセンシーがアプリを配布する必要があります。

社内でアプリケーションをセットアップし、配布する方法については、「[In-House Distribution guide](~/ios/deploy-test/app-distribution/in-house-distribution.md)」(社内配布ガイド) を参照してください。

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>アドホック配布

Xamarin.iOS アプリケーションは、アドホック配布を使用してユーザーテストを実行できます。アドホック配布は、**Apple Developer Program** と **Apple Developer Enterprise Program** の両方で使用できます。また、最大 100 台の iOS デバイスをテストできます。 アドホック配布の最適なユース ケースは、iTunes Connect が選択肢にない社内での配布です。

社内でアプリケーションをセットアップし、配布する方法については、「[Ad Hoc Distribution guide](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)」(アドホック配布ガイド) を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリケーションで使用できる配布メカニズムについて概要を説明しました。 また、iTunes App Store、アドホック、および社内の配布方法と、詳細な情報へのリンクを紹介しました。

## <a name="related-links"></a>関連リンク

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
